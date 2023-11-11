---
title: WebGL でのテクスチャの使用
slug: Web/API/WebGL_API/Tutorial/Using_textures_in_WebGL
---

{{DefaultAPISidebar("WebGL")}} {{PreviousNext("Web/API/WebGL_API/Tutorial/Creating_3D_objects_using_WebGL", "Web/API/WebGL_API/Tutorial/Lighting_in_WebGL")}}

現在、サンプルプログラムは回転する 3D キューブを描画します。今回はキューブの表面を単色で塗りつぶすのではなく、テクスチャをマッピングしてみましょう。

## テクスチャを読み込む

始めに、テクスチャを読み込むコードを追加します。今回は 1 個のテクスチャを用いて、そのテクスチャをキューブの 6 面に貼り付けますが、テクスチャがいくつある場合でも同じ方法を適用できます。

> **メモ:** テクスチャの読み込みは[クロスドメインの規則](/ja/docs/Web/HTTP/Access_control_CORS)に従うことへの注意が重要です。従ってコンテンツが CORS で認可されているサイトからのみ、テクスチャを読み込むことができます。クロスドメインのテクスチャについては、後ほど説明します。

テクスチャを読み込むコードは以下のようになります:

```js
function initTextures() {
  cubeTexture = gl.createTexture();
  cubeImage = new Image();
  cubeImage.onload = function () {
    handleTextureLoaded(cubeImage, cubeTexture);
  };
  cubeImage.src = "cubetexture.png";
}

function handleTextureLoaded(image, texture) {
  gl.bindTexture(gl.TEXTURE_2D, texture);
  gl.texImage2D(gl.TEXTURE_2D, 0, gl.RGBA, gl.RGBA, gl.UNSIGNED_BYTE, image);
  gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_MAG_FILTER, gl.LINEAR);
  gl.texParameteri(
    gl.TEXTURE_2D,
    gl.TEXTURE_MIN_FILTER,
    gl.LINEAR_MIPMAP_NEAREST,
  );
  gl.generateMipmap(gl.TEXTURE_2D);
  gl.bindTexture(gl.TEXTURE_2D, null);
}
```

`initTextures()` ルーチンは GL の `createTexture()` 関数を呼び出して、GL のテクスチャオブジェクト `cubeTexture` を作成することから始まります。そして、テクスチャを画像ファイルから読み込むために `Image` オブジェクトを作成して、そのオブジェクトにテクスチャとして使用したい画像ファイルをロードします。`handleTextureLoaded()` コールバックルーチンは、画像の読み込みが完了したときに実行されます。

実際にテクスチャを作成するために、新しいテクスチャが操作したいカレントのテクスチャであることを、`gl.TEXTURE_2D` にバインドすることで指定します。その後読み込んだ画像は、テクスチャとして書き込むために `texImage2D()` へ渡されます。

> **メモ:** テクスチャの幅と高さのピクセル数は**ほとんどの場合**において、それぞれ 2 のべき乗 (1、2、4、8、16……) にしなければなりません。例外については、"[2 のべき乗ではないテクスチャ](/ja/docs/Web/WebGL/Using_textures_in_WebGL#Non_power-of-two_textures)" のセクションをご覧ください。

その次の 2 行はテクスチャのフィルタリングを設定しています。これは画像が拡大縮小される際に適用するフィルタの設定です。今回は、画像を拡大する場合はリニアフィルタ、縮小する場合はミップマップを使用します。ミップマップは `generateMipMap()` を呼び出すことで生成され、最後は null を `gl.TEXTURE_2D` にバインドしてテクスチャの操作を終了することで完了します。

### 2 のべき乗ではないテクスチャ

一般的に、辺の長さが 2 のべき乗であるテクスチャを使うことが理想的です。これはビデオメモリへ効率よく保存され、また使用方法が制限されません。制作されたテクスチャを近い 2 のべき乗のサイズにスケーリングするか、2 のべき乗のサイズで制作を始めます。それぞれの辺の長さを 1、2、4、8、16、32、64、128、256、512、1024、あるいは 2048 ピクセルにすべきです。また、多くのデバイス (すべてではありません) が 4096 ピクセルをサポートします。さらに、8192 ピクセル以上をサポートするものもあります。

ときおり、特殊な事情で 2 のべき乗のテクスチャを使用することが困難な場合があります。サードパーティーの素材を使用する場合はたいてい、WebGL へ渡す前に HTML5 canvas を使用して 2 のべき乗のサイズに変換すると最良の結果を得られます。引き伸ばしが明白である場合は、UV 座標も必要でしょう。

一方、2 のべき乗ではない (NPOT) テクスチャが**不可欠である**場合でも、WebGL は限定的にネイティブサポートしています。NPOT テクスチャは主に、テクスチャの寸法をモニターなど他の解像度に揃えなければならない場合や、前出の提案に従うだけの価値がない場合に有用です。しかし、このようなテクスチャはミップマッピングで使用することが**できません**。また、"繰り返し" (タイルまたはラップ) を**行ってはいけません**。

テクスチャの繰り返しは、例えば小さなレンガの画像をタイリングしてレンガの壁を作ることです。

ミップマッピングや UV リピートは、`bindTexture()` を使用してテクスチャを作成する際に `texParameteri()` で無効化できます。これによりミップマッピング、UV ラッピング、UV タイリングを犠牲にして NPOT テクスチャを使用できます。また、デバイスがテクスチャをどのように扱うかを制御できます。

```js
// gl.LINEAR の代わりに gl.NEAREST も可能。ミップマップは不可
gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_MIN_FILTER, gl.LINEAR);
// S 座標のラッピング (繰り返し) を禁止
gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_WRAP_S, gl.CLAMP_TO_EDGE);
// T 座標のラッピング (繰り返し) を禁止
gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_WRAP_T, gl.CLAMP_TO_EDGE);
```

繰り返しますがこれらのパラメータを付加すると、WebGL デバイスは自動的に (サポートする最大サイズまでの) 任意の解像度のテクスチャを受け入れます。しかしこれらの設定を行わないと、WebGL は黒色 (`rgba(0,0,0,1)`) を返すことになり、NPOT テクスチャの全サンプルを受け入れてはなりません。

## テクスチャを表面にマッピングする

以上で、テクスチャの読み込みと使用する準備ができました。しかしテクスチャが使用できるようになるには、まずキューブの面の頂点にテクスチャの座標をマッピングする必要があります。これは `initBuffers()` にある、キューブの各面に色を設定する既存のコードの置き換えになります。

```js
cubeVerticesTextureCoordBuffer = gl.createBuffer();
gl.bindBuffer(gl.ARRAY_BUFFER, cubeVerticesTextureCoordBuffer);

var textureCoordinates = [
  // 前面
  0.0, 0.0, 1.0, 0.0, 1.0, 1.0, 0.0, 1.0,
  // 背面
  0.0, 0.0, 1.0, 0.0, 1.0, 1.0, 0.0, 1.0,
  // 上面
  0.0, 0.0, 1.0, 0.0, 1.0, 1.0, 0.0, 1.0,
  // 底面
  0.0, 0.0, 1.0, 0.0, 1.0, 1.0, 0.0, 1.0,
  // 右側面
  0.0, 0.0, 1.0, 0.0, 1.0, 1.0, 0.0, 1.0,
  // 左側面
  0.0, 0.0, 1.0, 0.0, 1.0, 1.0, 0.0, 1.0,
];

gl.bufferData(
  gl.ARRAY_BUFFER,
  new WebGLFloatArray(textureCoordinates),
  gl.STATIC_DRAW,
);
```

このコードは始めに各面のテクスチャの座標を収める GL のバッファを作成して、そのバッファを書き込みを行う配列としてバインドします。

`textureCoordinates` 配列は、各面の各座標に対応するテクスチャの座標を定義します。テクスチャの座標の範囲は 0.0 から 1.0 であることに注意してください。テクスチャマッピングのために、テクスチャの寸法は実際の大きさに関わらず 0.0 から 1.0 の範囲に正規化されます。

テクスチャマッピングの配列を設定したら、配列をバッファに渡すことで GL がそのデータを使用する準備が完了します。

> **メモ:** WebKit ベースのブラウザでは、`WebGLFloatArray` の代わりに `Float32Array` を使用しなければならないでしょう。

## シェーダーを更新する

シェーダープログラム (およびシェーダーを初期化するコード) も、単色に代わりテクスチャを使用するように更新する必要があります。

始めに `initShaders()` で必要になる、シンプルな変更点を見てみましょう:

```js
textureCoordAttribute = gl.getAttribLocation(shaderProgram, "aTextureCoord");
gl.enableVertexAttribArray(textureCoordAttribute);
```

これは頂点の色属性を設定するコードを、各頂点のテクスチャ座標を包含するコードに置き換えます。

### バーテックスシェーダー

次にバーテックスシェーダーを、色のデータを取り出すものからテクスチャ座標のデータを取り出すものに置き換える必要があります。

```html
<script id="shader-vs" type="x-shader/x-vertex">
  attribute vec3 aVertexPosition;
  attribute vec2 aTextureCoord;

  uniform mat4 uMVMatrix;
  uniform mat4 uPMatrix;

  varying highp vec2 vTextureCoord;

  void main(void) {
    gl_Position = uPMatrix * uMVMatrix * vec4(aVertexPosition, 1.0);
    vTextureCoord = aTextureCoord;
  }
</script>
```

重要な変更点は、頂点の色を取り出すのに代わりテクスチャ座標を設定していることです。これは頂点に対応する、テクスチャ内の位置を指し示します。

### フラグメントシェーダー

フラグメントシェーダーも同様に更新する必要があります:

```html
<script id="shader-fs" type="x-shader/x-fragment">
  varying highp vec2 vTextureCoord;

  uniform sampler2D uSampler;

  void main(void) {
    gl_FragColor = texture2D(uSampler, vec2(vTextureCoord.s, vTextureCoord.t));
  }
</script>
```

色の値をフラグメントの色に割り当てるのに代わり、フラグメントの色は、サンプラーが最適とするフラグメントの位置のテクセル (テクスチャ内のピクセル) を取り出すことで算出されます。

## テクスチャを貼り付けたキューブを描画する

`drawScene()` 関数の変更点は簡単です (コードを明瞭にするために、キューブを空間中で動かすアニメーションのコードを取り除いて単に回転するようにした点は除きます)。

色を割り当てるコードをテクスチャを割り当てるようにするためには、以下のように置き換えます:

```js
gl.activeTexture(gl.TEXTURE0);
gl.bindTexture(gl.TEXTURE_2D, cubeTexture);
gl.uniform1i(gl.getUniformLocation(shaderProgram, "uSampler"), 0);
```

GL は 32 個のテクスチャレジスタを提供し、その 1 つ目が `gl.TEXTURE0` です。前に読み込んだテクスチャをそのレジスタに結びつけて、そのテクスチャを使用するためにシェーダーサンプラー `uSampler` (シェーダープログラムにより明示されます) を設定します。

以上でテクスチャが貼り付けられた、回転するキューブが完成します。

{{EmbedGHLiveSample('dom-examples/webgl-examples/tutorial/sample6/index.html', 670, 510)}}

[コードを確認する](https://github.com/mdn/dom-examples/tree/main/webgl-examples/tutorial/sample6) | [新しいページでデモを開く](https://mdn.github.io/dom-examples/webgl-examples/tutorial/sample6/)

## クロスドメインのテクスチャ

WebGL のテクスチャの読み込みは、クロスドメインアクセス制御に従います。コンテンツで他のドメインからテクスチャを読み込むためには、CORS で許可を得なければなりません。CORS について詳しくは、[HTTP access control](/ja/docs/HTTP_access_control) をご覧ください。

CORS で許可された画像を WebGL のテクスチャとして使用する方法の説明を [こちらの hacks.mozilla.org の記事](http://hacks.mozilla.org/2011/11/using-cors-to-load-webgl-textures-from-cross-domain-images/) に掲載していますので、[サンプル](http://people.mozilla.org/~bjacob/webgltexture-cors-js.html) と合わせてご覧ください。

> **メモ:** WebGL テクスチャ向けの CORS サポートと、画像要素の `crossOrigin` 属性は Gecko 8.0 で実装されました。

汚染された (書き込みのみ) 2D canvas を WebGL のテクスチャとして使用することはできません。2D {{HTMLElement("canvas")}} が汚染されたとは例えば、クロスドメインの画像が canvas 上に描画された状態を指します。

> **メモ:** Canvas 2D `drawImage` 向けの CORS サポートを Gecko 9.0 で実装しました。これは、CORS で許可されたクロスドメインの画像が 2D canvas を汚染しないので、2D canvas を WebGL のテクスチャ素材として引き続き使用できることを意味します。

> **メモ:** クロスドメインの動画に対する CORS サポートと、{{HTMLElement("video")}} 要素の`crossorigin` 属性は Gecko 12.0 で実装されました。

{{PreviousNext("Web/API/WebGL_API/Tutorial/Creating_3D_objects_using_WebGL", "Web/API/WebGL_API/Tutorial/Lighting_in_WebGL")}}
