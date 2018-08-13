# 概要

A-Frameの学習用レポジトリ

2018/08/11 : 学習開始

# 学習記録メモ

# 不明点

a-sound

## Asset Management System

アセット機能が使えなかったのは、ローカルで実行してたから。

gh-pagesで試してみると使えるので、前のwebVR実装と同様にgh-pagesで開発していく。

## 重力の実装

Don McCurdy’s aframe-physics-system をアタッチすれば実装可。 [リンク](https://aframe.io/docs/0.8.0/introduction/html-and-primitives.html#attaching-components-to-primitives)

```
<script src="https://unpkg.com/aframe-physics-system@1.4.0/dist/aframe-physics-system.min.js"></script>
<a-scene physics>
</a-scene>
```

## environmentの変更

```
<a-scene>
  <a-assets>
    <img id="boxTexture" src="https://i.imgur.com/mYmmbrp.jpg">
    <img id="skyTexture"
      src="https://cdn.aframe.io/360-image-gallery-boilerplate/img/sechelt.jpg">
  </a-assets>

  <a-box src="#boxTexture" position="0 2 -5" rotation="0 45 45" scale="2 2 2"></a-box>

  <a-sky src="#skyTexture"></a-sky>
</a-scene>
```

360度画像をskyに指定すると出来る

<a-assets>を使うと上手く出来ないので、普通にsrcで指定するべき

## カメラ

```
<a-camera>
  <a-cursor></a-cursor>
</a-camera>
```

カーソルをカメラの子にすることで固定できる。

### VRインタラクティブ

```
<a-box color="red" position="-10 2 -5" rotation="0 0 45" scale="2 2 2">
  <a-animation attribute="position" to="0 2.2 -5" direction="alternate" dur="2000"
    repeat="indefinite"></a-animation>
  <!-- These animations will start when the box is looked at. -->
  <a-animation attribute="scale" begin="mouseenter" dur="300" to="3 3 3"></a-animation>
  <a-animation attribute="scale" begin="mouseleave" dur="300" to="2 2 2"></a-animation>
  <a-animation attribute="rotation" begin="click" dur="2000" to="360 405 45"></a-animation>
</a-box>
```

**mouseenter** : カーソルがオブジェクトに当たった時

**mouseleave** : カーソルがオブジェクトから外れた時

**click** : カーソルがオブジェクト上にある かつ クリックされた時

これでインタラクティブなオブジェクトを作ることが出来る

### Text

```
<a-text value="Hello, A-Frame!" color="#BBB"
        position="-0.9 0.2 -3" scale="1.5 1.5 1.5"></a-text>
```

他のやり方もあるらしい

- [Text Geometry](https://github.com/ngokevin/kframe/tree/master/components/text-geometry/) by Kevin Ngo - 3D text. More expensive to draw.

- [HTML Shader](https://github.com/mayognaise/aframe-html-shader/) by Mayo Tobita - Render HTML as a texture. Easy to style, but can be slow to compute.

### Templateコンポーネント

同じ構文をテンプレート化して、コードを簡潔にすることが出来る

```
<script src="https://unpkg.com/aframe-template-component@3.x.x/dist/aframe-template-component.min.js"></script>
```

```
<a-assets>
  <!-- ... -->
  <script id="plane" type="text/html">
    <a-entity class="link"
      geometry="primitive: plane; height: 1; width: 1"
      material="shader: flat; src: ${thumb}"
      sound="on: click; src: #click-sound"></a-entity>
  </script>
</a-assets>

<!-- ... -->

<!-- Pass image sources to the template. -->
<a-entity template="src: #plane" data-thumb="#city-thumb"></a-entity>
<a-entity template="src: #plane" data-thumb="#cubes-thumb"></a-entity>
<a-entity template="src: #plane" data-thumb="#sechelt-thumb"></a-entity>
```

`${thumb}`でデータを渡すことが出来る。
