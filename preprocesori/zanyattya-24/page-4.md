# Page 4

## Утилітні класи. Принципи побудови дизайн-системи

Для того, щоб розібратися у принципах побудови дизайн-системи варто спочатку визначитись що ж це і коли варто цим користуватись.

Узагальнено дизайн-система – це набір правил, стилів, елементів, відступів на які спираються розробники у повсякденній роботі.

При розробці такої системи все починається з роботи дизайнера, задача якого уніфікувати елементи дизайну продукту на макеті і визначити для них стилі. Під основними стилями дизайн-системи розуміються: **шрифти, кольори, іконки, макети, компоненти, картки**.

Дизайн-система ґрунтується на принципі атомарного дизайну, назва походить від слова «атоми».

Умовно дизайн-систему можна уявити у вигляді наступної послідовності

> _атом - молекула - блок - сторінка_

Атоми сайту об'єднуються в молекули, потім молекули в блоки, блоки вже утворюють сторінку.

На цьому етапі ми займемось розробкою простої дизайн-системи для веб-додатків, в основі якої буде саме атомарний підхід. Окрім повноцінної підтримки `СSS` препроцесор `Sass` надає безліч корисних інструментів, які дозволяють суттєво скоротити написання шаблонного коду. І не зважаючи на стрімкий розвиток CSS в останні роки, використання препроцесорів все ще залишається актуальним, особливо в роботі з великими проектами.

Розробка майбутніх таблиць стилей для веб-додатку завжди починається із організації структури файлів стилей.

Почнемо зі створення в робочій папці `src` папки для структурування та зберінання файлів стилей з назвою `scss`. В цій папці створюємо файл `main.scss`. Це наш основний файл стилів нашого додатку. Так як ми використовуємо бандлер `Vite`, необхідно впевнитись, що саме цей файл імпортується до `main.js`

```javascript
import "../scss/main.scss";
```

#### Скидання стилей

Реалізуємо мінімальне скидання стилей за допомогою [The new CSS reset](https://github.com/elad2412/the-new-css-reset). Створимо директорію `scss/utils` і в неї завантажимо файл `_reset.scss` з набором стилів бібліотеки.

Також, за необхідністю, можемо розширити цей файл додатковими стилями для скидання значень із таблиці стилей браузера

```scss
// _reset.scss

body,
h1,
h2,
h3,
h4,
h5,
p,
figure,
picture {
  margin: 0;
}

a {
  text-decoration: none;
  color: currentColor;
}

img,
svg {
  display: block;
  max-width: 100%;
}

input,
button,
textarea,
select {
  font: inherit;
}

@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    scroll-behavior: auto !important;
    transition-duration: 0.01ms !important;
  }
}
```

#### Змінні Variables

Додамо до папки `scss/utils` файл `_variables.scss` із основним набором змінних для проекта

```scss
// _variables.scss

// Мінімальна та максимальна ширина області перегляду з макету
$min-w: 375px;
$max-w: 1440px;

// Відступи
$spacer-base: 30px;

$spacer-xxs: 0.25 * $spacer-base;
$spacer-xs: 0.5 * $spacer-base;
$spacer-s: 0.75 * $spacer-base;
$spacer-m: $spacer-base;
$spacer-l: 1.5 * $spacer-base;
$spacer-xl: 2 * $spacer-base;
$spacer-xxl: 2.25 * $spacer-base;

// Розміри шрифтів
$fs-base: 16px;

$fs-xxs: 11px;
$fs-xs: 12px;
$fs-s: 14px;
$fs-m: $fs-base;
$fs-l: 22px;
$fs-xl: 24px;
$fs-xxl: 26px;

// Палітра кольорів
// Base
$primary: #F07F2E;
$success: #82C43C;
$info: #4E75FF;
$warning: #A461D8;
$danger: #FC5A5A;
$light: #92929D;
$dark: #13131A;
// Additional
$gray: #B5B5BE;
$bg-secondary-1: #1C1C24;
$bg-secondary-2: #292932;
$text-primary-1: #F15C27;
$text-primary-2: #D14A00;
$white: #fff;
$black: #000;

// Брейкпоінти
$sm: 576px;
$md: 768px;
$lg: 992px;
$xl: 1200px;
$xxl: 1400px;
```

Імпортуємо `_variables.scss` на початку файлу стилів. Це зробить змінні доступними глобально для кожного модуля, що імпортується в `main.scss`

#### Система відступів

Додамо до папки `scss/utils` файл `_spacing.scss` наступного змісту

```scss
// Збираємо в мапи
// Набори відступів
$spacers: (
  "0": 0,
  xxs: $spacer-xxs,
  xs: $spacer-xs,
  s: $spacer-s,
  m: $spacer-m,
  l: $spacer-l,
  xl: $spacer-xl,
  xxl: $spacer-xxl
);

// Види відступів
$types: (
  "m": "margin",
  "p": "padding"
);

// Сторони для відступів
$sides: (
  "": "",
  t: "-top",
  r: "-right",
  b: "-bottom",
  l: "-left"
);

// Перебираємо значення для відступів
@each $key-spacer, $factor in $spacers {
  // Перебираємо типи відступів
  @each $key-type, $type in $types {
    // Перебираємо сторони
    @each $key-side, $side in $sides {
      // Генеруємо набір класів для всіх значень, типів, сторін
      .#{$key-type}#{$key-side}-#{$key-spacer} {
        #{$type}#{$side}: $factor;
      }
    }

    // Генеруємо набір класів для горизонтальних відступів
    .#{$key-type}x-#{$key-spacer} {
      #{$type}-left: $factor;
      #{$type}-right: $factor;
    }

    // Генеруємо набір класів для вертикальних відступів
    .#{$key-type}y-#{$key-spacer} {
      #{$type}-bottom: $factor;
      #{$type}-top: $factor;
    }
  }

  // Простір між елементами для флекс або грід контейнерів
  .gap-#{$key-spacer} {
    gap: $factor;
  }
}
```

Цей код дозволяє визначити набір класів для зовнішніх (`margin`) та внутрішніх (`padding`) відступів наступним чином:

#### Кольори

Додамо до папки `scss/utils` файл `_colors.scss` наступного змісту

```scss
// Перелік кольорів
$colors: (
  'gray100': $gray100,
  'gray200': $gray200,
  'gray300': $gray300,
  'gray400': $gray400,
  'gray500': $gray500,
  'gray600': $gray600,
  'gray700': $gray700,
  'gray800': $gray800,
  'gray900': $gray900,
  'primary': $primary,
  'success': $success,
  'info': $info,
  'warning': $warning,
  'danger': $danger,
  'light': $light,
  'dark': $dark,
  'white': $white,
  'black': $black
);

// Перебираємо мапу кольорів
@each $name, $color in $colors {
  // колір тексту
  .color-#{$name} {
    color: $color;
  }

  // колір фона
  .bg-#{$name} {
    background-color: $color;
  }

  // заливка `svg`
  .fill-#{$name} {
    svg {
      color: $color;
    }
  }
}
```

Цей набір класів дозволить визначати кольори для тексту, фону та заливки svg-іконок. Наприклад, `color-primary` компілюється в:

```css
color: #0275d8;
```

`bg-success` компілюється в:

```css
background-color: #5cb85c;
```

#### Розміри шрифтів та товщина накреслення тексту

Додамо до папки `scss/utils` файл `_font.scss` наступного змісту

```scss
// Перелік розмірів шрифта
$font-sizes: (
  "0": 0,
  xxs: $fs-xxs,
  xs: $fs-xs,
  s: $fs-s,
  m: $fs-m,
  l: $fs-l,
  xl: $fs-xl,
  xxl: $fs-xxl
);
// Перебираємо мапу розмірів
@each $size, $factor in $font-sizes {
  .fs-#{$size} {
    font-size: $factor;
  }
}

// Перелік варіантів товщини накреслення
$weight: 100, 200, 300, 400, 500, 600, 700, 800, 900;
// Перебираємо список накреслень текту
@each $w in $weight {
  .fw-#{$w} {
    font-weight: $w;
  }
}
```

`fs-s` компілюється в:

```css
font-size: 14px;
```

`fw-600` компілюється в:

```css
font-weight: 600;
```

Додамо до папки `scss/utils` файл `_sizing.scss` наступного змісту

```scss
$column: 1;
$columns: 12;
@for $i from $column through $columns {
  .w-#{$i} {
    width: calc(100% / $columns * $i);
  }

  .h-#{$i} {
    height: calc(100% / $columns * $i);
  }
}

// Повна ширина/висота відносно вʼюпорту
.w-full {
  width: 100vw;
}

.h-full {
  height: 100vh;
}
```

Така система класів буде зручною для розподілу ширини / висоти блоків, якщо у макеті використовується підхід розрахунку елементів по сіткці із 12 рівних колонок. При використанні класів варто розглядати розмір елемента так: скільки колонок/рядків повинен займати блок?

Наприклад, `w-6` компілюється в:

```css
width: 50%;
```

#### Флекс-бокс <a href="#fleks" id="fleks"></a>

Додамо до папки `scss/utils` файл `_flex.scss` наступного змісту

```scss
// Властивість justify-content
$values: start, end, center, stretch, between, around, evenly;
@each $value in $values {
  .justify-#{$value} {
    @if ($value == start) or ($value == end) {
      justify-content: flex-#{$value};
    } @else if ($value == between) or ($value == around) or ($value == evenly) {
      justify-content: space-#{$value};
    } @else {
      justify-content: $value;
    }
  }
}

// Властивість align-items
$values: start, end, center, stretch;
@each $value in $values {
  .items-#{$value} {
    @if ($value == start) or ($value == end) {
      align-items: flex-#{$value};
    } @else {
      align-items: $value;
    }
  }
}

// Додатково
.flex-center {
  // Підключення міксинів далі
  @include flex-center();
}

.flex-center-column {
  @include flex-center(column);
}

.flex-wrap {
  flex-wrap: wrap;
}
```

За необхідності, аналогічним способом визначається набір класів для `align-content` та `align-self`.

#### Утиліти <a href="#utility" id="utility"></a>

Додамо до папки `scss/utils` файл \_utils`.scss` наступного змісту

<pre class="language-scss"><code class="lang-scss">// Властивість display
$values: none, inline, block, inline-block, flex, inline-flex, grid,
  inline-grid;
@each $value in $values {
  .d-#{$value} {
    display: $value;
  }
}

// Властивість position
$values: relative, absolute, fixed, sticky;
@each $value in $values {
  .p-#{$value} {
    position: $value;
  }
}

// Контент для screen-readers
.visually-hidden {
  position: absolute;
  width: 1px;
  height: 1px;
  margin: -1px;
  border: 0;
  padding: 0;
  white-space: nowrap;
  clip-path: inset(100%);
  clip: rect(0 0 0 0);
  overflow: hidden;
}

// Переповнення текстових блоків
.text-overflow {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
// Центрування для тексту
.text-center {
  text-align: center;
}

/**
 |======================================
<strong> | Цей файл можна розширювати
</strong> | іншими додатковими класами-утилітами
 |======================================
*/
</code></pre>

Базові утилітні класи для дизайн-системи готові. Інший функціонал залежатиме від необхідностей конкретного веб-додатку та особистих запитів розробників.

#### Додатковий функціонал дизайн-системи <a href="#bonus" id="bonus"></a>

**Функції**

Додамо до папки `scss/utils` файл `_functions.scss`

{% code overflow="wrap" lineNumbers="true" %}
```scss
@use "sass:math";

/// Функція для видалення одиниць виміру з виразу
/// @param  {String} $value Значення з одиницями виміру в "px"
/// @return {Number} Число без одиниць виміру
/// @example
///     strip-unit(12px) => 12

@function strip-unit($value) {
  @if type-of($value) != "number" {
    @error "#{$value} is not a number.";
  }

  @if type-of($value) == "number" and not unitless($value) {
    @return math.div($value, ($value * 0 + 1));
  }

  @return $value;
}

/// Функція для конвертації "px" в "rem"
/// @param  {String} $px-value Значення, яке потрібно конвертувати в "px"
/// @param  {String} [$base-font-size: 16px] Базове значення відносно якого буде відбуватися обрахунок
/// @return {String} Конвертоване значення в "rem"
/// @example
///     rem(1000px) => 62.5rem

@function rem($px-value, $base-font-size: 16px) {
  @return #{math.div(strip-unit($px-value), strip-unit($base-font-size))}rem;
}
```
{% endcode %}

Функції дозволяють автоматизувати та абстрагувати повторювані дії при роботі з кодом, пришвидшуючи процес розробки.

**Міксини**

Додамо до папки `scss/utils` файл `_mixins.scss`

<pre class="language-scss" data-overflow="wrap" data-line-numbers><code class="lang-scss"><strong>// Міскин для керуванням властивостями флекс контейнера 
</strong><strong>// Використовується у файлі _flex.scss
</strong><strong>@mixin flex-center($is-column: false) {
</strong>  display: flex;
  justify-content: center;
  align-items: center;

  @if $is-column {
    flex-direction: column;
  }
}

// Міксин для стилізації плейсхолдера інпута
@mixin placeholder {
  &#x26;::placeholder {
    @content;
  }
}

// Міксин для респонсивного розмішу шрифтів
@mixin fluid-fs($min-w, $max-w, $min-fs, $max-fs) {
  $u1: unit($min-w);
  $u2: unit($max-w);
  $u3: unit($min-fs);
  $u4: unit($max-fs);

<strong>  @if $u1 == $u2 and $u1 == $u3 and $u1 == $u4 {
</strong>    &#x26; {
      font-size: $min-fs;

      @media only screen and (min-width: $min-w) {
        font-size: calc(
          #{$min-fs} + #{strip-unit($max-fs - $min-fs)} *
            ((100vw - #{$min-w}) / #{strip-unit($max-w - $min-w)})
        );
      }

      @media only screen and (min-width: $max-w) {
        font-size: $max-fs;
      }
    }
  } @else {
      @error "Incompatible units. Check mixin params";
  }
}
</code></pre>

Приклад використання `fluid font-size`:

```scss
html {
  @include fluid-fs($min-w, $max-w, $fs-xs, $fs-xl);
}
```

Під час зміни області вʼюпорту розмір шрифта для кореневого елемента `html (:root)` буде пропорційно змінюватись від `12px` до `24px`.

В комбінації із застосуванням функції `rem()` із файлу `_functions.scss` для конвертації значень в одиниці виміру `rem` можна автоматично отримати гнучку систему шрифтів в усьому веб-додатку.
