# Вкладені правила

Подібно до вкладеності тегів в HTML, в SASS можна вкладати CSS-селектори. Це одна з найбільш корисних можливостей, яка також часто неправильно використовується. Вкладеність дозволяє робити одні оголошення правил всередині інших. Нижче наведений CSS і SCSS код оформлення секції із заголовком і абзацом.

{% tabs %}
{% tab title="SCSS" %}
{% code title="main.scss" %}
```scss
.section {
  width: 100%;

  .title {
    color: red;
  }
  .text {
    font-size: 14px;
  }
}
```
{% endcode %}
{% endtab %}

{% tab title="CSS" %}
{% code title="main.css" %}
```css
.section {
  width: 100%;
}

.section .title {
  color: red;
}

.section .text {
  font-size: 14px;
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

Синтаксис `SCSS` виглядає чистішим і менше повторюється. Після компіляції у стандартний CSS, ми отримаємо код як в `main.css`. Але в процесі розробки писати код буде зручніше.

## Конкатенація селекторів

Символ `&` (амперсанд) дозволяє вказати, в яке місце необхідно підставити батьківський селектор. Запишемо імена класів з попереднього прикладу, використовуючи BEM-нотацію в CSS і SCSS.

{% tabs %}
{% tab title="SCSS" %}
{% code title="main.scss" %}
```scss
.section {
  width: 100%;

  &__title {
    color: red;
  }
  &__text {
    font-size: 14px;
  }
}
```
{% endcode %}
{% endtab %}

{% tab title="CSS" %}
{% code title="main.css" %}
```css
.section {
  width: 100%;
}

.section__title {
  color: red;
}

.section__text {
  font-size: 14px;
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

Розглянемо ще один приклад — оформлення посилання зі станами ховеру і фокусу.

{% tabs %}
{% tab title="CSS" %}
{% code title="main.css" %}
```css
.link {
  color: black;
}

.link:hover {
  color: red;
}

.link:focus {
  color: red;
}
```
{% endcode %}
{% endtab %}

{% tab title="SCSS" %}
{% code title="main.scss" %}
```scss
.link {
  color: black;

  &:hover {
    color: red;
  }
  &:focus {
    color: red;
  }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

Що робити, якщо селектори для `:hover` і `:focus` в CSS згруповані? Без проблем групуємо в SASS, не забуваючи поставити амперсанд (`&`) там, де необхідно підставити батьківський селектор.

{% tabs %}
{% tab title="SCSS" %}
{% code title="main.scss" %}
```scss
.link {
  color: black;

  &:hover,
  &:focus {
    color: red;
  }
}
```
{% endcode %}
{% endtab %}

{% tab title="CSS" %}
{% code title="main.css" %}
```css
.link {
  color: black;
}
.link:hover,
.link:focus {
  color: red;
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Правила вкладеності

Вкладеність селекторів — це чудовий спосіб заощадити час і спростити підтримку, але надмірна вкладеність гарантовано викличе проблеми з читабельністю коду.

Уявімо наступну розмітку кнопки з іконкою і текстом.

```html
<button class="button" type="button">
  <svg class="icon"></svg>
  <span class="label">Замовити</span>
</button>
```

Запишемо стилі в CSS і SCSS, і зробимо їх спеціально трохи складнішими, ніж потрібно.

{% tabs %}
{% tab title="SCSS" %}
{% code title="main.scss" %}
```scss
.button {
  color: red;

  &:hover {
    color: blue;

    .button__icon {
      background-color: teal;
    }

    .button__label {
      font-size: 20px;
    }
  }

  &__icon {
    width: 20px;
    height: 20px;

    &:hover {
      width: 50px;
      height: 50px;
    }
  }

  &__label {
    font-size: 16px;
  }
}
```
{% endcode %}
{% endtab %}

{% tab title="CSS" %}
{% code title="main.css" %}
```css
.button {
  color: red;
}
.button:hover {
  color: blue;
}

.button__icon {
  width: 20px;
  height: 20px;
}

.button__icon:hover {
  width: 50px;
  height: 50px;
}

.button__label {
  font-size: 16px;
}

.button:hover .button__icon {
  background-color: teal;
}

.button:hover .button__label {
  font-size: 20px;
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

Прочитати такий SCSS-код швидко досить складно — візуально втрачається зв'язок з батьківським селектором і, замість того щоб розбирати CSS-код, доводиться вчитуватися у синтаксис вкладеностей. Тобто, використовуючи можливості препроцесора, ми зробили гірше — більше не завжди краще.

Створюйте нове правило для кожного блоку або елемента, а вкладеності і конкатенації використовуйте для оформлення станів і BEM-модифікаторів. Тобто користуйтеся здоровим глуздом, і робіть так як зручно, тому що SCSS-код пишеться для зручності розробника.

{% tabs %}
{% tab title="SCSS" %}
{% code title="main.scss" %}
```scss
// Правило для всієї кнопки
.button {
  color: red;

  &:hover {
    color: blue;
  }
}

// Правило для іконки
.button__icon {
  width: 20px;
  height: 20px;

  &:hover {
    width: 50px;
    height: 50px;
  }

  .button:hover & {
    background-color: teal;
  }
}

// Правило для тексту
.button__label {
  font-size: 16px;

  .button:hover & {
    font-size: 20px;
  }
}
```
{% endcode %}
{% endtab %}

{% tab title="CSS" %}
{% code title="main.css" %}
```css
.button {
  color: red;
}

.button:hover {
  color: blue;
}

.button__icon {
  width: 20px;
  height: 20px;
}

.button__icon:hover {
  width: 50px;
  height: 50px;
}

.button__label {
  font-size: 16px;
}

.button:hover .button__icon {
  background-color: teal;
}

.button:hover .button__label {
  font-size: 20px;
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Рекомендація: використовуйте вкладеність помірно — для станів і модифікаторів. Для правил, що відносяться до окремих блоків/елементів, краще створювати окремі верхньорівневі правила, щоб зберегти читабельність та простоту підтримки.
{% endhint %}
