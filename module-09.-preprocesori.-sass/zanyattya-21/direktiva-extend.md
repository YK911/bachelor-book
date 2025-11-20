# Директива @extend

Директива `@extend` використовується для наслідування (розширення) вже існуючих стилів. Застосуємо її, щоб створити компонент кнопки з декількома станами.

{% code title="SCSS | main.scss" %}
```scss
.button {
  display: inline-flex;
  border-radius: 3px;
  font-size: 16px;
  padding: 10px 20px;
  color: white;
  background-color: gray;
}

.button-success {
  @extend .button;
  background-color: green;
}

.button-error {
  @extend .button;
  background-color: red;
}
```
{% endcode %}

{% code title="CSS | main.css" %}
```css
.button,
.button-error,
.button-success {
  display: inline-flex;
  border-radius: 3px;
  font-size: 16px;
  padding: 10px 20px;
  color: white;
  background-color: gray;
}

.button-success {
  background-color: green;
}

.button-error {
  background-color: red;
}
```
{% endcode %}

Розширення (наслідування) не зробить копію стилів для кожного селектора, а грамотно додасть потрібні селектори у перелік до правила з наслідуваними стилями.

## Шаблони (плейсхолдери)

Але що, якщо ми хочемо розширити набір стилів, для якого не потрібний базовий селектор? Наприклад, якщо не потрібний селектор `.button` з попереднього прикладу, адже сам по собі він нічого не робить і не буде використаний в HTML.

Для таких випадків існує `placeholder` (плейсхолдер, заповнювач місця, шаблон) - довільне ім'я селектора з обов'язковим символом `%` на початку, наприклад `%button`.

{% code title="SCSS | main.scss" %}
```scss
%button {
  display: inline-flex;
  border-radius: 3px;
  font-size: 16px;
  padding: 10px 20px;
  color: white;
  background-color: gray;
}

.button-success {
  @extend %button;
  background-color: green;
}

.button-error {
  @extend %button;
  background-color: red;
}
```
{% endcode %}

{% code title="CSS | main.css" %}
```css
.button-error,
.button-success {
  display: inline-flex;
  border-radius: 3px;
  font-size: 16px;
  padding: 10px 20px;
  color: white;
  background-color: gray;
}

.button-success {
  background-color: green;
}

.button-error {
  background-color: red;
}
```
{% endcode %}

Після компіляції будуть доступні селектори `.button-success` і `.button-error`, прив'язані до правила шаблону, а самого імені шаблону в CSS не буде.
