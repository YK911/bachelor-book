# Директива @mixin

Міксіни або домішки, як і плейсхолдери, дозволяють створювати готові набори властивостей, але з різними значеннями, залежно від отриманих аргументів під час виклику міксіна.

```scss
@mixin name (params) {
  // Властивості
}
```

Міксін оголошується за допомогою директиви `@mixin` та його імені. Далі можуть йти необов'язкові параметри в круглих дужках (самі дужки обов'язкові), а у фігурних - набір властивостей і значень.

Створимо міксін для встановлення тільки верхньої і нижньої рамки елемента, і зробимо колір рамки значенням, що може налаштовуватися. Параметри міксіна - це локальні SASS-змінні.

{% code title="SCSS | mixin.scss" %}
```scss
@mixin bordered($color) {
  border-top: 1px solid $color;
  border-bottom: 1px solid $color;
}
```
{% endcode %}

Додати стилі міксіна до селектора можна за допомогою директиви `@include`, після якої викликаємо міксін і передаємо значення для властивостей, що налаштовуються.

Після компіляції будуть тільки правила для селекторів `.section` і `.header` з доданим кодом з міксіна, коду оголошення самого міксіна не буде.

{% code title="SCSS | main.scss" %}
```scss
/* SCSS | main.scss */

@mixin bordered($color) {
  border-top: 1px solid $color;
  border-bottom: 1px solid $color;
}

.section {
  @include bordered(tomato);
  padding: 20px;
}

.header {
  @include bordered(green);
  min-height: 80px;
}
```
{% endcode %}

{% code title="CSS | main.css (зкомпільований)" %}
```css
/* CSS | main.scss */

.section {
  border-top: 1px solid tomato;
  border-bottom: 1px solid tomato;
  padding: 20px;
}

.header {
  border-top: 1px solid green;
  border-bottom: 1px solid green;
  min-height: 80px;
}
```
{% endcode %}

{% hint style="warning" %}
Міксін відрізняється від плейсхолдера тим, що властивості дублюються в кожен селектор. Все тому, що значення властивостей міксіна можуть бути різними, залежно від переданих аргументів під час виклику `@include міксін(аргументи)`. У той час як властивості та їх значення в плейсхолдері завжди однакові.
{% endhint %}
