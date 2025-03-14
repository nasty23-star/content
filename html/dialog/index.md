---
title: "`<dialog>`"
description: "Тег для создания всплывающего окна без боли и страданий."
baseline:
  - group: dialog
    features:
      - api.HTMLDialogElement
      - api.HTMLDialogElement.close
      - api.HTMLDialogElement.open
      - api.HTMLDialogElement.returnValue
      - api.HTMLDialogElement.show
      - api.HTMLDialogElement.showModal
      - html.elements.dialog
      - html.elements.dialog.open
authors:
  - baileys-li
contributors:
  - tatianafokina
  - skorobaeus
keywords:
  - диалог
  - модальное окно
  - модалка
related:
  - a11y/role-dialog
  - html/form
  - js/deal-with-forms
tags:
  - doka
---

## Кратко

Тег создаёт всплывающее окно или диалог. По умолчанию не показывается на странице.

Может открываться в двух режимах:

1. Всплывающее окно — не блокирует взаимодействие со страницей.
1. Модальное окно — откроется поверх страницы, имеет фоновое затемнение, остальной контент не доступен для взаимодействия.

## Как пишется

Парный тег `<dialog></dialog>`, внутри которого находится содержимое всплывающего окна. У `<dialog>` нельзя использовать [атрибут `tabindex`](/html/tabindex/).

```html
<dialog>
  Привет, мир!
</dialog>
```

Также у модального окна обязательно должно быть имя — его краткое название. Благодаря этому пользователи вспомогательных технологий знают, что это за элемент и какое у него содержимое.

Имя окну можно добавить двумя способами:

- [`aria-label`](/a11y/aria-label/),
- [`aria-labelledby`](/a11y/aria-labelledby/).

`aria-label` добавляет имя, о котором знают только пользователи [скринридеров](/a11y/screenreaders/).

```html
<dialog aria-label="Приветствие">
  Привет, мир!
</dialog>
```

`aria-labelledby` связывает `<dialog>` с видимым всем именем.

```html
<dialog aria-labelledby="dialog-name">
  <h3 id="dialog-name">
    Привет, мир!
  </h3>
  <p>Вы не ждали, а вот он я.</p>
</dialog>
```

### Как открыть

Как и у элемента [`<details>`](/html/details/), по умолчанию содержимое окна скрыто от пользователя, но его можно отобразить через атрибут `open`.

```html
<dialog open>
  Я виден. Привет! 👋
</dialog>

<dialog>
  Я скрыт от пользователя 🥷
</dialog>
```

Также окно можно открыть с помощью JavaScript-методов:

1. `show()` — добавляет атрибуты `open` и [`aria-modal="false"`](/a11y/aria-modal/).
1. `showModal()` — открывает в режиме «модального окна». Добавляет атрибуты `open` и `aria-modal="true"`. Появляется подложка в виде псевдоэлемента `::backdrop`, который можно стилизовать.

```html
<button type="button" onclick="window.myDialog.show()">
  Просто открыть
</button>
<button type="button" onclick="window.myDialog.showModal()">
  Открыть как модалку
</button>
<dialog id="myDialog">🖖 Живи долго и процветай!</dialog>
```

<iframe title="Пример dialog в модальном и обычном режиме" src="demos/basic/" height="385"></iframe>

### Как закрыть

- Из JavaScript с помощью метода `close()`.
- Из HTML по [событию `submit`](/js/event-submit/) (например по нажатию кнопки `<button type="submit">`), если в `<dialog>` есть [тег `<form>`](/html/form/) с атрибутом `method="dialog"`.

```html
<dialog
  open="open"
  id="closeMe"
  aria-labelledby="heading"
>
  <h2 id="heading">Закрой меня! 🙏</h2>
  <p>Результат этих кнопок одинаковый.</p>

  <button type="button" onclick="window.closeMe.close()">
    Закрыть с помощью JavaScript
  </button>

  <form method="dialog">
    <!-- Если у тега <button> не указан type, то по-умолчанию
    он будет type="submit" ! -->
    <button>
      Закрыть с помощью формы
    </button>
  </form>
</dialog>
```

<iframe title="Варианты закрытия" src="demos/close-variants/" height="300"></iframe>

### Возвращаемое значение

Если кнопкам в форме задать [`value`](/html/value/), то при закрытии диалога это значение будет присваиваться `dialog.returnValue`.

Присвоим двум кнопкам разные значения:

```html
<form class="options" method="dialog">
  <button class="button button--dark" value="debug">
    Дави его!
  </button>
  <button class="button button--light" value="reproduction">
    Каждая жизнь священна
  </button>
</form>
```

Если всплывающее окно закрыто по кнопке <kbd>Дави его!</kbd>, то количество 🐞 уменьшается. А если по кнопке <kbd>Каждая жизнь священна</kbd>, то увеличивается:

```javascript
if (dialog.returnValue === "debug") {
  bugs.innerText = bugs.innerText.substring(0, bugs.innerText.length - 2)
} else {
  bugs.innerText += "🐞"
}
```

<iframe title="Пример использования returnValue" src="demos/return-value/" height="290"></iframe>

## Как понять

Долгое время в HTML не существовало тега для создания всплывающих окон. Если такая задача возникала, то использовались либо самописные решения для красивых попапов, либо JavaScript-методы [`alert()`](/js/alert), [`prompt()`](/js/prompt) и [`confirm()`](/js/confirm), если красота была не важна.

Тег `<dialog>` появился как альтернатива. Хорошее диалоговое окно — это не просто логика «Показать» и «Скрыть». В `<dialog>` реализовано то, о чём часто забывают:

- Для вспомогательных технологий `<dialog>` — аналог [`role="dialog"`](/a11y/role-dialog/). Если окно открыто в режиме модального, то и аналог `aria-modal="true"`. Также у тега есть [`aria-live="assertive"`](/a11y/aria-live/), поэтому скринридеры сразу же зачитывают его содержимое.
- Модальные диалоги закрываются по нажатию на <kbd>Esc</kbd>.
- У модального диалога при открытии появляется «ловушка фокуса»: для клавиатурной навигации доступны только интерактивные элементы только текущего диалога.
- Браузер запоминает какой элемент был в фокусе до открытия окна и после закрытия окна снова переводит его в фокус.

Вся это логика реализована в самом браузере «из коробки». А значит пользователю не отправляется лишний трафик.

## Подсказки

💡 Google Chrome при закрытии модального окна клавишей <kbd>Esc</kbd> ставит предыдущий элемент не просто в [`:focus`](/css/focus/), а в [`:focus-visible`](/css/focus-visible/). Подразумевая, что пользователь перешёл на клавиатурную навигацию.

💡 По нажатию <kbd>Esc</kbd> сначала запускается событие `cancel`, а затем `close`. Это может быть полезно, если мы хотим отгородить пользователя от случайного нажатия клавиши, сначала предупредив, что изменённые данные не сохранятся, и только при повторном нажатии закрывать окно.

💡 Контент `<dialog>` по умолчанию скрыт с помощью [`display: none`](/css/display/). Можно переписать это поведение в стилях и анимировать открытие и закрытие. Намного легче, чем аналогичная задача в [`<details>`](/html/details/) например.

💡 Модальные окна «ускользают» от контекста: даже если в HTML-разметке после модального окна указан [тег `<div>`](/html/div/) с [`z-index: 99999`](/css/z-index/), то модальное окно всё равно отобразится поверх этого `<div>`. Или если родитель наклонён с помощью `skew()`, то дочернее модальное окно всё равно откроется без наклона.
