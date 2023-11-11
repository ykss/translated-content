---
title: Math.clz32()
slug: Web/JavaScript/Reference/Global_Objects/Math/clz32
---

{{JSRef}}

**`Math.clz32()`** 関数は、引数として与えられた数値の 32 ビットバイナリ表現での先頭の 0 の個数を返します。

{{EmbedInteractiveExample("pages/js/math-clz32.html")}}

## 構文

```
Math.clz32(x)
```

### 引数

- `x`
  - : 数値。

### 返値

与えられた数値の 32 ビットバイナリ表現での先頭の 0 の個数。

## 解説

"`clz32`" は **CountLeadingZeroes32** の省略形です。

`x` が数値でない場合、まず数値に変換され、32 ビット符号なし整数値に変換されます。

変換された 32 ビット符号なし整数値が `0` の場合、すべてのビットが `0` であるため、`32` を返します。

この関数は [Emscripten](/ja/docs/Emscripten) のような JS にコンパイルするシステムに対して特に役に立ちます。

### 先頭の 1 を数える

現在のところ、 "Count Leading Ones" を表す `Math.clon` はありません ("clo" ではなく "clon" と名付けられています、なぜなら "clo" と "clz" は特に英語を話さない人にとっては似すぎているからです)。しかし、 `clon` 関数は、数値のビットを逆数にして、その結果を `Math.clz32` に渡すことで簡単に作ることができます。 1 の逆数は 0 であり、その逆も同様です。このように、ビットを逆数にすると、測定された 0 の量が (`Math.clz32` からの) 逆数になり、 `Math.clz32` はゼロの数を数えるのではなく、1 の数を数えるようになります。

以下の 32 ビットワード値を想定してみます。

```js
var a = 32776; // 00000000000000001000000000001000 (16 leading zeros)
Math.clz32(a); // 16

var b = ~32776; // 11111111111111110111111111110111 (32776 inversed, 0 leading zeros)
Math.clz32(b); // 0 (this is equal to how many leading one's there are in a)
```

この論理を使用すると、 `clon` 関数は次のように作成することができます。

```js
var clz = Math.clz32;
function clon(integer) {
  return clz(~integer);
}
```

さらに、この技術を拡張して、以下に示すようなジャンプレスの「Count Trailing Zeros」と「Count Trailing Ones」関数を作成することができます。以下の `ctrz` 関数は、すべての上位ビットを最も低いビットで埋め、そのビットを否定して上位のセットビットをすべて消去するので、clz が使用できます。

```js
var clz = Math.clz32;
function ctrz(integer) {
  // count trailing zeros
  // 1. fill in all the higher bits after the first one
  integer |= integer << 16;
  integer |= integer << 8;
  integer |= integer << 4;
  integer |= integer << 2;
  integer |= integer << 1;
  // 2. Now, inversing the bits reveals the lowest bits
  return (32 - clz(~integer)) | 0; // `|0` ensures integer coercion
}
function ctron(integer) {
  // count trailing ones
  // No shift-filling-in-with-ones operator is available in
  // JavaScript, so the below code is the fastest
  return ctrz(~integer);
  /* Alternate implementation for demonstrational purposes:
       // 1. erase all the higher bits after the first zero
       integer &= (integer << 16) | 0xffff;
       integer &= (integer << 8 ) | 0x00ff;
       integer &= (integer << 4 ) | 0x000f;
       integer &= (integer << 2 ) | 0x0003;
       integer &= (integer << 1 ) | 0x0001;
       // 2. Now, inversing the bits reveals the lowest zeros
       return 32 - clon(~integer) |0;
    */
}
```

これらのヘルパー関数を ASM.JS モジュールに入れます。そして、そうすれば、真のパフォーマンスの傑作ができあがります。このような状況は、まさに ASM.JS のために設計されたものです。

```js
var countTrailsMethods = (function (stdlib, foreign, heap) {
  "use asm";
  var clz = stdlib.Math.clz32;
  function ctrz(integer) {
    // count trailing zeros
    integer = integer | 0; // coerce to an integer
    // 1. fill in all the higher bits after the first one
    // ASMjs for some reason does not allow ^=,&=, or |=
    integer = integer | (integer << 16);
    integer = integer | (integer << 8);
    integer = integer | (integer << 4);
    integer = integer | (integer << 2);
    integer = integer | (integer << 1);
    // 2. Now, inversing the bits reveals the lowest bits
    return (32 - clz(~integer)) | 0;
  }
  function ctron(integer) {
    // count trailing ones
    integer = integer | 0; // coerce to an integer
    return ctrz(~integer) | 0;
  }
  // unfourtunately, ASM.JS demands slow crummy objects:
  return { a: ctrz, b: ctron };
})(window, null, null);
var ctrz = countTrailsMethods.a;
var ctron = countTrailsMethods.b;
```

## 例

### Math.clz32() の使用

```js
Math.clz32(1); // 31
Math.clz32(1000); // 22
Math.clz32(); // 32

var stuff = [
  NaN,
  Infinity,
  -Infinity,
  0,
  -0,
  false,
  null,
  undefined,
  "foo",
  {},
  [],
];
stuff.every((n) => Math.clz32(n) == 32); // true

Math.clz32(true); // 31
Math.clz32(3.5); // 30
```

## ポリフィル

以下のポリフィルが最も効果的です。

```js
if (!Math.clz32)
  Math.clz32 = (function (log, LN2) {
    return function (x) {
      // Let n be ToUint32(x).
      // Let p be the number of leading zero bits in
      // the 32-bit binary representation of n.
      // Return p.
      var asUint = x >>> 0;
      if (asUint === 0) {
        return 32;
      }
      return (31 - ((log(asUint) / LN2) | 0)) | 0; // the "| 0" acts like math.floor
    };
  })(Math.log, Math.LN2);
```

## 仕様書

{{Specifications}}

## ブラウザーの互換性

{{Compat}}

## 関連情報

- {{jsxref("Math")}}
- {{jsxref("Math.imul")}}
