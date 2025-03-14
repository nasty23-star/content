---
title: "`aspect-ratio`"
description: "Помогает задать соотношение сторон для элемента."
baseline:
  - group: aspect-ratio
    features:
      - html.elements.video.aspect_ratio_computed_from_attributes
      - html.elements.img.aspect_ratio_computed_from_attributes
      - css.properties.aspect-ratio
      - css.properties.aspect-ratio.auto
authors:
  - denisputnov
contributors:
  - starhamster
keywords:
  - aspect-ratio
  - соотношение сторон
related:
  - css/box-model
  - css/object-fit
  - css/padding
tags:
  - doka
---

## Кратко

Свойство задаёт соотношение сторон для элемента.

<iframe title="Как это работает с блоком" src="demos/variants/" height="600"></iframe>

## Пример

Код ниже стилизует блок так, что его высота будет равна его ширине:

```css
.container {
  width: 200px;
  aspect-ratio: 1 / 1;
}
```

Подобное поведение можно получить и в случае, когда задана только высота:

```css
.container {
  height: 200px;
  aspect-ratio: 1 / 1;
}
```

## Как пишется

- `auto` — соотношение сторон считается автоматически.
- `<width> / <height>` — соотношение сторон всегда считается относительно ширины и высоты блока.
- `auto <width> / <height>` — совмещённая запись.

При совмещённой записи соотношение `<width> / <height>` приоритетнее, но когда у элемента есть собственное соотношение сторон, применится оно:

<iframe title="Собственное соотношение сторон у тега <img>" src="demos/aspect-ratio-auto/" height="565"></iframe>

Если использовать значение `auto 1 / 1` для [`<img>`](/html/img/), то применится соотношение сторон фотографии. Это происходит, потому что она имеет собственное соотношение сторон. При значении `1 / 1` мы принудительно применим соотношение сторон 1 к 1.

Чтобы избежать искажений картинки, можно использовать свойство [`object-fit`](/css/object-fit/).

## Как понять

Свойство вычисляет незаданную сторону, исходя из размера уже известной. Возьмём пример с `aspect-ratio: 2 / 1`:

```css
.container {
  width: 200px;
  aspect-ratio: 2 / 1;
}
```
Соотношение сторон в этом случае — `2 / 1`. Такому соотношению должны равняться и стороны блока: `<width> / <height>`.

Нам известна ширина — `200px`.

<sup>2</sup>⁄<sub>1</sub> = <sup>200</sup>⁄<sub>height</sub>

Отсюда получаем высоту блока — `100px`.

## Подсказки

💡 Свойство нельзя применить к строчным элементам.

💡 Хотя большинство значений поддерживается основными браузерами, с некоторыми могут быть трудности. Поэтому на всякий случай проверяйте поддержку на [Can I use](https://caniuse.com/?search=aspect-ratio).
