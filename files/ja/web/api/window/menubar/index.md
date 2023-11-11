---
title: Window.menubar
slug: Web/API/Window/menubar
---

{{ APIRef() }}

**`Window.menubar`** プロパティは `menubar` オブジェクトを返します。これによって表示状態を確認することができます。

## 構文

```js
objRef = window.menubar;
```

## 例

以下の完全な HTML の例は、 `menubar` オブジェクトの `visible` プロパティの使用方法を示しています。

```html
<html>
  <head>
    <title>様々な DOM テスト</title>
    <script>
      var visible = window.menubar.visible;
    </script>
  </head>
  <body>
    <p>様々な DOM テスト</p>
  </body>
</html>
```

## 仕様書

{{Specifications}}

## ブラウザーの互換性

{{Compat}}

## 関連情報

- {{domxref("Window.locationbar")}}
- {{domxref("Window.personalbar")}}
- {{domxref("Window.scrollbars")}}
- {{domxref("Window.statusbar")}}
- {{domxref("Window.toolbar")}}
