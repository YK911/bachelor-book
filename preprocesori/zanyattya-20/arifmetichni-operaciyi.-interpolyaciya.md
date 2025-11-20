# Арифметичні операції

До того як в CSS з'явилася функція `calc()`, препроцесори були єдиним рішенням за потреби виконати арифметичні обчислення.

## Додавання і віднімання

На відміну від функції `calc()`, в препроцесорі не можна змішувати типи одиниць. При спробі виконати додавання або віднімання несумісних типів, виникне помилка. Пікселі до пікселів, слони до слонів.

{% code title="main.scss" %}
```scss
/* main.scss */

.box {
  width: 960px + 10%;  // Помилка!
  width: 960px + 30px; // 990px
  width: 960px + 30;   // 990px
  width: 100% + 20%;   // 120%

  width: 100% - 50px;  // Помилка!
  width: 960px - 30px; // 930px
  width: 960px - 30;   // 930px
  width: 100% - 20%;   // 80%
}
```
{% endcode %}

Справа в тому, що препроцесор не знає наперед, скільки буде `100%` або `5em` у пікселях. Значення відносних одиниць в пікселях можна дізнатися тільки в момент рендеру HTML-сторінки. Тому для таких обчислень необхідно використовувати нативну функцію `calc()`.

{% code title="main.scss" %}
```scss
/* main.scss */

.box {
  width: calc(100% - 20px);
  width: calc(5em + 20px);
}
```
{% endcode %}

## Множення

Виконується аналогічно функції `calc()` в CSS, за винятком того, що не можна множити несумісні типи. Також не можна множити слонів на слонів, можна тільки слонів на множник.

{% code title="main.scss" %}
```scss
/* main.scss */

.box {
  width: 50px * 50px;      // Помилка!
  width: 50% * 5%;         // Помилка!
  width: 50px * 2;         // 100px
  width: 10px * 2 + 50px;  // 70px
  width: 5 * (5px + 15px); // 100px
}
```
{% endcode %}

## Ділення

Ділення в препроцесорі виконується у трьох випадках.

{% stepper %}
{% step %}
### Значення зберігається у змінній

{% code title="main.scss" %}
```scss
$value: 50px;

.box {
  width: $value / 5; // 10
}
```
{% endcode %}
{% endstep %}

{% step %}
### Значення взяті в круглі дужки

{% code title="main.scss" %}
```scss
.box {
  width: (100px / 5); // 20px
}
```
{% endcode %}
{% endstep %}

{% step %}
### Значення використовується як частина іншого виразу

{% code title="main.scss" %}
```scss
.box {
  width: 100px / 5px + 10px; // 30px
}
```
{% endcode %}
{% endstep %}
{% endstepper %}

## Змінні в операціях

Якщо в арифметичній операції використовується валідне значення змінної, не буде жодних проблем.

{% code title="main.scss" %}
```scss
/* SCSS | main.scss */

$gridItemMargin: 20px;

.box {
  margin: $gridItemMargin * 2;
}
```
{% endcode %}

```css
/* CSS | main.css */

.box {
  margin: 40px;
}
```

Але у разі, якщо SASS-змінна використовується в функції `calc()`, під час компіляції її ім'я не замінюється на значення.

{% code title="main.scss" %}
```scss
/* SCSS | main.scss */

$gridItemMargin: 20px;

.box {
  margin: calc($gridItemMargin * 2);
}
```
{% endcode %}

```css
/* CSS | main.css */

.box {
  margin: calc($gridItemMargin * 2);
}
```

У таких випадках необхідно робити інтерполяцію значення змінної, використовуючи спеціальну конструкцію `#{$ім'я_змінної}`.

{% code title="main.scss" %}
```scss
/* SCSS | main.scss */

$gridItemMargin: 20px;

.box {
  margin: calc(#{$gridItemMargin} * 2);
}
```
{% endcode %}

```css
/* CSS | main.css */

.box {
  margin: calc(20px * 2);
}
```
