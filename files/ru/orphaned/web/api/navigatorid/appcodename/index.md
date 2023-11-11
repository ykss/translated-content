---
title: NavigatorID.appCodeName
slug: orphaned/Web/API/NavigatorID/appCodeName
---

{{APIRef("HTML DOM")}}{{deprecated_header}}

**`NavigatorID.appCodeName`** всегда возвращает`'Mozilla'` в любом браузере. Это свойство сохраняется только для совместимости.

> **Примечание:** Не полагаетесь на это свойство, чтобы получить настоящее имя продукта. Все браузеры возвращают "Mozilla" в качестве значения свойства.

## Синтаксис

```
codeName = window.navigator.appCodeName
```

### Значение

`codeName` это внутреннее имя браузера в виде {{domxref("DOMString")}}.

## Пример

```js
console.log(window.navigator.appCodeName);
```

## Спецификации

{{Specifications}}

## Доступность в браузере

{{Compat}}

## Смотрите также

- {{domxref("NavigatorID.product")}}
