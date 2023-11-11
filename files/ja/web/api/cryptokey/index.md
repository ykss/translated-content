---
title: CryptoKey
slug: Web/API/CryptoKey
---

{{APIRef("Web Crypto API")}}

**`CryptoKey`** インターフェイスは、特定の鍵アルゴリズムによりもたらされる{{glossary("key", "暗号鍵")}}を表します。

`CryptoKey` オブジェクトは、{{domxref("SubtleCrypto.generateKey()")}} または {{domxref("SubtleCrypto.deriveKey()")}}、{{domxref("SubtleCrypto.importKey()")}} を使用して取得できます。

## プロパティ

_このインターフェイスはどのプロパティも継承しません。_

- {{domxref("CryptoKey.type")}}
  - : 鍵の種類と、(対称アルゴリズムでは) 秘密鍵、(非対称アルゴリズムでは) 公開鍵またはプライベートキーを表す列挙値を返します。
- {{domxref("CryptoKey.extractable")}}
  - : 生の情報がアプリケーションにエクスポートされるかどうかを示す {{jsxref("Boolean")}} を返します。
- {{domxref("CryptoKey.algorithm")}}
  - : 鍵が使用される特定の暗号法を表す透過オブジェクトを返します。
- {{domxref("CryptoKey.usages")}}
  - : どの用途で使用される鍵かを示す列挙値の配列を返します。

## メソッド

_このインターフェイスはどのメソッドも定義または継承しません。_

## 仕様

{{Specifications}}

## ブラウザーの実装状況

{{Compat}}

## 関連情報

- {{domxref("Crypto")}} および {{domxref("Crypto.subtle")}}。
