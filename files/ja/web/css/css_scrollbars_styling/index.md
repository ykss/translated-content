---
title: CSS スクロールバー
slug: Web/CSS/CSS_scrollbars_styling
---

{{CSSRef}}{{SeeCompatTable}}

**CSS スクロールバー** (CSS Scrollbars) は、 2000 年に Windows の IE 5.5 で導入され、廃止されたスクロールバーの色のプロパティを標準化するためのものです。

## 基本的な例

この例では、緑色のトラックと紫色のつまみを持った細いスクロールバーを使用してみました。

```css
.scroller {
  width: 300px;
  height: 100px;
  overflow-y: scroll;
  scrollbar-color: rebeccapurple green;
  scrollbar-width: thin;
}
```

### HTML

```html
<div class="scroller">
  Veggies es bonus vobis, proinde vos postulo essum magis kohlrabi welsh onion
  daikon amaranth tatsoi tomatillo melon azuki bean garlic. Gumbo beet greens
  corn soko endive gumbo gourd. Parsley shallot courgette tatsoi pea sprouts
  fava bean collard greens dandelion okra wakame tomato. Dandelion cucumber
  earthnut pea peanut soko zucchini.
</div>
```

### 結果

{{EmbedLiveSample("Basic_Example")}}

## リファレンス

### CSS プロパティ

- {{CSSxRef("scrollbar-width")}}
- {{CSSxRef("scrollbar-color")}}

## 仕様書

{{Specifications}}

## アクセシビリティの考慮

スクロールバーをカスタマイズする場合は、十分なコントラストがあることと、ヒット領域がタッチ入力を使っている人が操作できるだけ大きいかどうかを考慮してください。

- [Baseline Rules for Scrollbar Usability | Adrian Roselli](http://adrianroselli.com/2019/01/baseline-rules-for-scrollbar-usability.html)

## ブラウザーの互換性

### scrollbar-width

{{Compat}}

### scrollbar-color

{{Compat}}

## 関連情報

- {{CSSxRef("::-webkit-scrollbar")}}
- {{CSSxRef("-ms-overflow-style")}}
