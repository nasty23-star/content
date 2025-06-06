---
title: "`::backdrop`"
description: "Стилизуем подложку под всплывающими элементами."
baseline:
  - group: backdrop
    features:
      - css.selectors.backdrop
authors:
  - solarrust
contributors:
  - skorobaeus
related:
  - html/dialog
  - css/pseudoelements
  - html/popover
keywords:
  - паранжа
  - подложка
  - вуаль
  - затемнение
tags:
  - doka
---

## Кратко

Псевдоэлемент `::backdrop` отвечает за подложку под элементами, всплывающими на верхний слой страницы. Типа [`<dialog>`](/html/dialog/) в режиме модального окна, [поповеров](/html/popover/) или открытых на весь экран [`<video>`](/html/video/).

## Пример

```css
dialog::backdrop {
  background-image: linear-gradient(
    130deg,
    #123E66,
    #593273 41.07%,
    #623D45 76.05%
  );
}
```

<iframe title="Базовый пример" src="demos/base/" height="400"></iframe>

## Как понять

Псевдоэлемент `::backdrop` подсказывает пользователю, что сейчас его внимание должно быть направлено на открывшийся элемент. Вся остальная страница перекрывается, чтобы не отвлекать от основного действия. К тому же, остальной контент, как правило, недоступен для взаимодействия.

Стили по умолчанию:

```css
dialog::backdrop {
  position: fixed;
  inset: 0px;
  background: rgba(0, 0, 0, 0.1);
}
```

Подложка перекрывает собой всю доступную область просмотра (вьюпорт), а также фиксирована, чтобы при прокрутке оставаться на месте. Эти свойства лучше не менять, чтобы не удивлять пользователя и сохранить основную функцию псевдоэлемента.

Как правило, достаточно изменить фоновую заливку или показать картинку, чтобы было красиво.

## Как пишется

Ничего затейливого: указываете элемент и через два двоеточия пишите `backdrop`. Для стилизации доступны все CSS-свойства, но постарайтесь не трогать позиционирование.

Можно реагировать на действия пользователя. В демо ниже попробуйте понажимать <kbd>Tab</kbd> и <kbd>Shift Tab</kbd>. Когда фокус будет попадать на кнопку в открытом диалоге, цвета градиента будут меняться местами.

```css
dialog:focus-within::backdrop {
  background-image: linear-gradient(
    130deg,
    #123E66,
    #593273 41.07%,
    #623D45 76.05%
  );
}
```

<iframe title="Реагируем на фокус" src="demos/focus/" height="400"></iframe>
