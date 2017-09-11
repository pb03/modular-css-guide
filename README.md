# Writing readable and predictable CSS

*This guide has been written for **modular CSS** with the assumption of using SCSS as a pre-processor.*


## Tips for naming classes

#### What should be the name of the class?
Since we are writing modular CSS, you should not confuse with the naming conventions of functional CSS.
This means the class name should tell about the element, not about the function it performs.

_Example:_ If you just want to center align your page title, you should name the class as `.page-title` rather than `.align-center` or something similar.


#### Should I follow [BEM naming methodology](https://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)?
No.
Since your CSS has local scope you don't have to worry prefixing the block names.

Tip: If you feel there are lot elements in your component and you can't manage to name all the classes individually, probably you should break you component into small ones rather than following BEM.
E.g.


#### Make use of modifier classes
The main advantage of using modifier classes is that they make the code more predictable whether it is HTML or CSS.
There's also a plus point in grouping it with it's main class.

_Example:_ `.is-` and `.has-` are commonly used modifier in CSS.

✅
```
.item {
  padding: 14px;
  background-color: #40364D;

  &.is-active {
    background-color: #2b2434;
  }
}
```


#### Don't prefix component name
Doing this is absolutely unnecessary, if your JS framework is already prefixing the component name.

_Example:_

❌
```
.onboarding-heading {
  ...
}
.onboarding-sub-heading {
  ...
}
```
which compiles to `.onboarding-component__onboarding-heading` & `.onboarding-component__onboarding-sub-heading`. It could be even worse if it had a parent component.

✅
```
.heading {
  ...
}
.sub-heading {
  ...
}
```



#### Assign class to every element
Do not nest HTML tags within classes. Tags do not really describe what that element does. Also, nesting makes your CSS code less readable.

_Example:_

❌
```
.time {
  color: black;

  span {
    color: gray;
  }
}
```
✅
```
.time {
  color: black;
}

.time-unit {
  color: gray;
}
```

Apart from readability, nesting also affects the performance of selectors.
<details>
  <summary>Read more about selector performance</summary>
  &nbsp;
  
  There are 2 reasons of avoiding attribute selectors.
  
  1) They are the least performant by themselves.
  
  2) Browsers read CSS selectors from right to left direction. Taking an example of a menu item <code>.menu li a</code>:
  
  Here browser will read the first selector as <code>a</code> then check if it lives inside <code>li</code>, finally check if they both live  inside <code>.menu</code>. This does not affect when there are a few elements on the page but considering too many elements in a complex design, you should definitely avoid this.
  
  
</details>


#### How would I know whether I've named the classes correctly or not
Just a simple trick I use to follow:

If you or your code reviewer can understand what the classes do only by reading the CSS code then 👍.



## Tips for improving Readability

#### Don't nest classes unless really needed
Use proper parent-child relationships. Avoid unnecessary specificity which is also against the CSS selector performance.

_Example:_

❌
```
.box {
  ...

  .title {
    ...
  }
}
```

✅
```
.box {
  ...
}

.box-title {
  ...
}
```


#### Group pseudo and modifier classes
_Example_

❌
```
.dropdown {
  ...
}

.dropdown:hover {
  ...
}

.dropdown.is-collapsed {
  ...
}
```

✅
```
.dropdown {
  ...

  &:hover {
    ...
  }

  &.is-collapsed {
    ...
  }
}
```


#### Use reverse `&` where the style is dependent on a parent element's class
_Example:_ Continuing with the same dropdown example, suppose the trigger element's style is dependent on the main component div.

❌
```
.dropdown-trigger {
  ...
}

.is-collapsed .dropdown-trigger {
  ...
}
```

✅
```
.dropdown-trigger {
  ...
  .is-collapsed & {
    ...
  }
}
```


#### It's good to write the properties in order
If you aren't already doing this, you're gonna feel it a bit difficult to follow in the beginning but believe me you gonna love it once you're used to it.
You might think of sorting them alphabetically but that proven really helpful. The most efficient one is the group order.

_Example:_

❌
```
.box {
  background-color: #fff;
  width: 200px;
  position: relative;
  height: 300px;
}
```
✅
```
.box {
  position: relative;
  width: 200px;
  height: 300px;
  background-color: #fff;
}
```

<details>
  <summary>More details on ordering CSS properties</summary>
  <pre><code>
    .selector {
      
      /* Imported style */
      @extend %another-style;
      
      /* CSS content */
      content: '';
      
      /* Position */
      position: absolute;
      z-index: 10;
      top: 0;
      right: 0;
      
      /* Display & box model */
      display: inline-block;
      overflow: hidden;
      width: 100px;
      height: 100px;
      padding: 10px;
      margin: 10px;
      border: 10px solid #333;
      
      /* Color */
      background: #000;
      color: #fff;
      
      /* Text */
      font-family: sans-serif;
      font-size: 16px;
      line-height: 1.4;
      text-align: right;
      
      /* Other */
      cursor: pointer;
      
      /* Transition & animation */
      transition: color 0.15s;
      animation: slide 0.2s forwards;
    }

 </code></pre>
</details>


#### Use `:not()` instead of overriding property values
This one is quite obvious but I've seen in many codebases, developers forget to use it. So it might be worth mentioning here.

_Example:_

❌
```
.item {
  border: 1px solid #eee;

  &:last-child {
    border: 0;
  }
}
```

✅
```
.item:not(:last-child) {
  border: 1px solid #eee;
}
```


#### Going against best practices?
Several times we write the CSS that simply doesn't feel right like using negative margins, using `!important`, etc. It's good to avoid such code but if you have to then make sure:
a) it's the only solution -- means you know that you have to refactor lot of code otherwise which is not quite worth
b) it won't have any side-effects
**And always add a comment in such conditions.**


#### Abstraction in modular CSS?
Don't do that. Keep your CSS within the same component. If you think that you're repeating the same code in several components, create the mixins instead.



## Make no mistakes - a couple of other tips

#### Avoid using shorthands
Yes, CSS shorthands should be considered as anti-patterns unless you're changing all the values of a property.

How do we misuse the shorthands:

```
.card {
  background: #fff;
}
```

What we are actually telling the browser to do:
```
.card {
  background-image: initial;
  background-position-x: initial;
  background-position-y: initial;
  background-size: initial;
  background-repeat-x: initial;
  background-repeat-y: initial;
  background-attachment: initial;
  background-origin: initial;
  background-clip: initial;
  background-color: #fff;
}
```

What we really meant:
```
.card {
  background-color: #fff;
}
```
<details>
<summary>Brief on problems with shorthands</summary>

Suppose you want to override font properties for an element:
<pre><code>
body {
  font-family: 'Source Sans Pro';
  font-size: 18px;
  line-height: 1.8;
}


.box {
  font: 30px 'Fira Code';
}
</code></pre>

and unknowing your line-height is set to initial.
</details>



#### Avoid `margin-top` and give `margin-bottom`
Unlike horizontal margins, vertical margins do collapse. To avoid this, it's a good idea to always give margin in one direction. Choosing one with the most use cases, good to go with `margin-bottom`.
