Залог быстрой и чистой верстки во многом зависит от правильно проведенной подготовки проекта. Зачастую много времени отнимают такие рутиные процессы как: поиск и встраивание шрифтов, нарезка изображений, копирование текста из макета и т.п. Если эти процессы отлажены и оптимизированы, они не будут занимать много времени, стало быть основное время будет потрачено на саму верстку, а не рутиную подготовку.

Второй этап - сама верстка - тоже должен быть отлажен и строиться на базовых неукоснительных приницпах, придерживаясь которых, можно сберечь много времени и написать максимально чистый и моудльный код, который будет легко поддерживать в будущем.

## Содержание
- Подготовка проекта
  - Подготовка шрифтов
  - Подготовка изображений
  - Подготовка текста
- Работа со стилями
  - Работа со шрифтами
- Верстка
  - Общие неукоснительные принципы
  - Правило для Adblock

## § Подготовка проекта в Adobe Photoshop

### Подготовка шрифтов

Если дизайнер не приложил используемые шрифты вместе с макетом (а такое бывает часто), придется искать их самому. Для начала нужно получить список всех шрифтов в макете. Есть несколько способов:

- с помощью [скрипта для Photoshop](https://github.com/frontendbeast/list-fonts)
- загрузив макет в [Adobe Assets](https://assets.adobe.com)

Отличный ресурс для поиска кириллических шрифтов: https://fontstorage.com/. Там можно скачать шрифты сразу для веба в нужных форматах, а также готовый css файлик для быстрого подключения.

Если подходящий комплект шрифтов найти не удается, придется искать любой и генерировать на его основе нужные форматы. Есть несколько способов: 

- [FontSquirrel](https://www.fontsquirrel.com/)
- [Transfonter](https://transfonter.org/)

**Настройки для FontSquirrel:**
*приложить скрин с настройками*

**Настройки для Transfonter:**
- Обязательно отмечаем Family support. 
- Если шрифт популярный (типа Open Sans), отмечаем Add local() rule.
- Ни в коем случае не отмечаем: Autohint font (были случаи, когда от этого шрифт выглядел хуже).
- Base64 encode тоже не отмечаем.

**Общие рекомендации**

В большинстве случаем можно генерировать только WOFF, поскольку у него [хорошая поддержка](http://caniuse.com/#search=woff).

Очень помогает в работе менеджер шрифтов [Nexus Font](http://www.xiles.net/). Он позволяет просматривать шрифты в графическом редакторе без установки их в систему. У этого менеджера есть недостаток: чем больше шрифтов в папке, которую он читает, тем дольше он запускается (хорошо заметно на цифре 1000+). Поэтому имеет смысл скормить ему специальную папку, где будут лежать шрифты только текущего проекта.

### Подготовка изображений

Для получения всех изображений в PS также есть автоматизация. Для ее включения нужно активировать флаг `Файл => Генерировать => Набор изображений`. Когда рядом с надписью `Набор изображений` стоит галочка, это значит, включена динамическая генерация картинок. Автогенерация срабатывает всякий раз, когда происходит переименование слоя с картинкой по принципу "название.расширение". Например:

- icon.svg
- header.jpg
- something.png

Во время переименования желательно также следить, чтобы на картинках не было эффектов, которые можно воспроизвести с помощью CSS. Если имеются, лучше их снять. На сверстанной странице это даст больший простор для анимации.

Картинки сохраняются рядом с макетом в папку `<имя_макета>-assets`.

По завершении проверяем, все ли какртинки сохранились как ожидалось. Если нет, добиваем их вручную.

#### SVG

*Скрин настроек для простых иконок* - decimal places = 1
*Скрин настроек для сложных изображений (логотипов и т.д.)* - decimal places = 3+

### Подготовка текста

Выдирать текст из слоев редактора неудобно, а если его много, то и долго. Самый быстрый способ сразу получить весь текст из макета: установить [скрипт для Photoshop](https://github.com/pivanov/photoshop-scripts). Бонусом конкретно этого скрипта идет экспорт стилей для каждого фрагмента текста. 

Другой способ: загрузить макет в [Adobe Assets](https://assets.adobe.com) и копировать текст оттуда. Этот способ чуть менее долгий, чем копирование из редактора.

## § Работа со стилями

https://assets.adobe.com

### Работа со шрифтами

Шрифтовые файлы необходимо подключать именно как гарнитуры. Это является хорошим тоном, а также позволяет избежать некоторых проблем с рендером шрифтов в браузере. Подробнее можно почитать [здесь](https://fontstorage.com/ru/blog/about-font-face-part-two/).

Если коротко, предположим есть 2 файла: opensans.woff и opensanslight.woff. Несмотря на то, что файлы разные, это один и тот же шрифт - Open Sans, - отличающийся только начертаниями (font-weight): normal и light. Стало быть и подключать их нужно как семейство одного шрифта. 

<strong style="color: red !important">Плохо</strong>
```css
@font-face {
    font-family: 'Open Sans';
    src: url('opensans.woff') format('woff');
    font-weight: normal;
    font-style: normal;
}
@font-face {
    font-family: 'Open Sans Light';
    src: url('opensanslight.woff') format('woff');
    font-weight: normal;
    font-style: normal;
}
```

<strong style="color: green !important">Хорошо</strong>
```css
@font-face {
    font-family: 'Open Sans';
    src: url('opensans.woff') format('woff');
    font-weight: normal;
    font-style: normal;
}
@font-face {
    font-family: 'Open Sans';
    src: url('opensanslight.woff') format('woff');
    font-weight: 300;
    font-style: normal;
}
```

Если в проекте много шрифтов и начертаний, для быстрого их подключения удобно сделать миксины для каждого начертания. Пример для stylus:

```stylus
// Variables
$reserveFonts = Tahoma, Arial, sans-serif;
$fontOpenSans = 'Open Sans', $reserveFonts;

// Mixins

// Open Sans normal normal
font-open-sans() {
    font-family: $fontOpenSans;
    font-weight: normal;
    font-style: normal;
}

// Open Sans light normal
font-open-sans-light() {
    font-family: $fontOpenSans;
    font-weight: 300;
    font-style: normal;
}

// Open Sans light italic
font-open-sans-light-italic() {
    font-family: $fontOpenSans;
    font-weight: 300;
    font-style: italic
}
```

[Сниппеты для остальных препроцессоров](https://gist.github.com/ncer/6efc59f59268e02be5ba2de9fbd8e08f).

Иногда бывает так, что по названию шрифта трудно определить степень жирности начертания. Для облегчения задачи, можно воспользоваться табличкой из [этой статьи](https://www.webtype.com/info/articles/fonts-weights/).

- 100 - Extra Light or Ultra Light
- 200 - Light or Thin
- 300 - Book or Demi
- 400 - Normal or Regular
- 500 - Medium
- 600 - Semibold, Demibold
- 700 - Bold
- 800 - Black, Extra Bold or Heavy
- 900 - Extra Black, Fat, Poster or Ultra Black

## § Верстка

### Общие неукоснительные принципы

- Верстать блоками, а не страницами.
- Использовать БЭМ-подобное именование классов.
- Один блок - один неймспейс и один файл стилей.
- Уровень вложенности элементов только ОДИН: `.block__element` (не будут раздуваться верстка и стили).
- При появлении дополнительных уровней вложенности `.block__element__element2` - их надо выносить в другой блок `.block2__element2`.
- Блок не должен никак позиционироваться по отношению к внешнему миру: никаких внешних отступов, а тем более абсолютного позиционирования у блока быть не должно. Позиционирование блока в потоке - это задача: а) либо лэйаута, который окружает блок, б) либо модификаторов блока (.block--offsets).

**Как определить блок:**
- устоявшийся компонент: хедер, футер, аккордеон, табы, карусель, галерея и т.п.
- контрол: инпут, селект, чекбокс и т.п.
- смысловой блок: отзывы, преимущества и т.п.
- визуальный блок: в макете обычно как-то выделен заголовком, бордером, другим цветом фона, фоновой картинкой и т.п.
- в остальных случаях: если во время верстки у элемента начинают появляться крупные вложенные элементы `.block__element__element2`, то, вероятно, их надо выносить в отдельный блок.

### Правило для Adblock

Классы по возможности не должны состоять из следующих конструкций или содержать эти конструкции в названии: 

- ad(v)
- banner
- promo, 
- third-party
- и т.п.

Все это слова из [стоп-листа Adblock](https://easylist-downloads.adblockplus.org/easylist.txt) и блоки с такими классами просто будут резаться с установленным Adblock у пользователя.

Если сложно придумать более подходящий неймспейс, то взять за правило: тестировать верстку с включенным адблоком. (Хотя это не панацея.)
