# Weird But Still Useful - CSS not() Selector


In CSS, pseudo-classes are very useful and, some of them are widely used. For examples,
* `:hover`
* `:target`
* `:active` 

But there are a few other pseudo-classes out there that I found, are not much popular but very useful and very interesting.

One of the useful and I feel much interesting pseudo-class is `:not()` known as *negation pseudo-class*.

Unlike other pseudo-classes you are familiar with, it can accept arguments within its parentheses. That is why `not()` is referred to as a *functional pseudo-class*.

As you guessed, it selects everything except that inside its parentheses. Either you can include one selector or selector-list inside the parentheses as its arguments.

**Syntax:**

```css

selector:not(simple-selector) {}

/* OR */

selector:not(<selector-list>) {}

``` 

As I mentioned above, you can experiment a lot with it and get some sense of how interesting is it. So let me try some of them.

Here is the HTML code we are going to style using `:not()` pseudo-class.

In this HTML we have following elements,
* `<h2 id="heading" class="selected">`
* `<div id="first" class="selected">`
* `<div id="second" class="not-selected">`
* `<div id="third" class="selected special">`

```html

<body>

  <h2 id="heading" class="selected">CSS pseudo-class not()</h2>

  <div id="first" class="selected">This is selected</div>
  <div id="second" class="not-selected">This is not selected</div>
  <div id="third" class="selected special">This is selected and Special</div>
  
</body>

```

And it has basic CSS styles like below.

```css

body {
  display: flex;
  flex-direction: column;
  juztify-content: center;
  align-items: center;
}

div {
  width: 200px;
  height: 100px;
  border: solid black;
  margin: 0.625rem;
  padding: 0.625rem;
  color: #333;
}

```

#### 1. with simple selector

Using this CSS rule, `div:not(.not-selected) {}`, we can select all the `div` elements except `div` elements with the class `not-selected`. 

So it selects only the following two elements.
 
* `<div id="first" class="selected">`
* `<div id="third" class="selected special">`


```css

div:not(.not-selected) {
  color: blue;
  border-color: burlywood;
}

```

**_Note:_**

But, if you use elements with CSS combinators as an argument for the `:not()` pseudo-class, you may get nothing since CSS combinators are *NOT* recognized as [simple selectors](https://www.w3.org/TR/selectors-3/#simple-selectors-dfn).

In the example below, we target an element using [descendant combinator](https://developer.mozilla.org/en-US/docs/Web/CSS/Descendant_combinator) as the argument.

```html

<ul>
  <li>First</li>
  <li>Second</li>
  <li>Third</li>
</ul>

```

```css

/* below code are not well supported yet */

body :not(ul li) {
  color: red;
}

```

#### 2. Select everything on the page except the selector within the parentheses. 

`:not(.not-selected) {}`

The above rule selects everything, including `<html>` and `<body>` elements on the page except elements with the class `.not-selected`.

```css

:not(.not-selected) {
  color: blue;
  border-color: burlywood;
}

```


#### 3. Exclude multiple selectors

In this case, we want to select all the elements except `.not-selected` and `#heading`.

There are two ways to do the job, shown below.
1. `body :not(.not-selected, #heading) {}`
2. `body :not(.not-selected):not(#heading) {}`

The first method is not well supported in all the browsers yet. However, we can use the second way instead.


```css

/* select anything that is not class="not-selected" or id="heading" */

body :not(.not-selected):not(#heading) {
  color: blue;
  border-color: burlywood;
}

/* below code are not well supported yet */

/*

body :not(.not-selected, #heading) {
  color: blue;
  border-color: burlywood;
}

*/

```

#### 4. Sometimes it seems nonsense

If we use universal selector `*` with `:not()` like below, it says that *do NOT style anything in the document* which is meaningless.

```css

:not(*) {
  color: blue;
  border-color: burlywood;
}

```
#### 5. A pseudo-class as an argument

It doesn't allow to use `:not()` inside another `:not()` pseudo-class. In short it doesn't allow nested `:not()`.


```css

/* not allowed */

/*
:not(:not()) {}
*/


/* This code will not work */

body :not(div:not(.selected)) {
  color: blue;
  border-color: burlywood;
}

```

However, we can still use other pseudo-classes inside parentheses like below.

The code selects everything on the page except the 3rd child. In this case 3rd child of the `body` is `<div id="second" class="not-selected">`


```css

body :not(:nth-child(3)) {
  color: blue;
  border-color: burlywood;
}

```

#### 6. It increases the specificity of an element

At the first glance, the below code looks silly. It selects elements with `id="first"` except with `id="second"`. 

As we know,
* one single element cannot have multiple `id` values.
* multiple elements cannot have the same `id` value.

So our code becomes meaningless.

But when it comes to the **_specificity_**, ` #first:not(#second) {}` has higher specificity than `#first {}`. 

That's why `#first` gets the following styles.

* `color: blue`
* `border-color: burlywood`  


```css

/* first box get color: blue and border-color: burlywood */

#first {
  color: red;
  border-color: green;
}

#first:not(#second) {
  color: blue;
  border-color: burlywood;
}

```

#### 7. Elements with multiple classes

The code below,

* selects the elements with the class `selected`.
* doesn't select elements with the class `special`. 

Also, in our example HTML has a `div` with two class names which is,

`<div id="third" class="selected special">`

So we can exclude this `div` when adding styles like this way.

```css

.selected:not(.special) {
  color: blue;
  border-color: burlywood;
}


```


#### 8. Bring it to the real-life

As I mentioned earlier, the `:not()` pseudo-class is more interesting, not only because of its weird behaviors but because of its usefulness in some real-world situations.

Let me try some examples.

 
**(1) Style all the buttons on the page except the ones that are disabled**

We have a couple of buttons to styles like below. 

```html

<button class="btn">Sign in</button>
<button class="btn">Sign out</button>
<button class="btn" disabled>Delete Account</button>

```

In this case, we want to style only the buttons that are not disabled. 

```css

.btn {
  margin: 0.625rem;
  padding: 1rem 2rem;
  cursor: pointer;
}

.btn:not([disabled]) {
 background: #333;
 color: Snow;
 font-weight: bold;
 border: none;
 border-radius: 20px;
}

```


**(2) For improving accessibility**

To improve accessibility, it is a good practice to always add the `alt` attribute to the images of the page. 

When we have a large array of images, also, we might forget to add the `alt` attribute for some of them. 

So this is useful in that case too.

There are five images and, two of them don't have the `alt` attribute assigned to them.

```html

<div>
  <img src="https://images.unsplash.com/photo-1641057273235-50725552d22a?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxNDU4OXwwfDF8cmFuZG9tfHx8fHx8fHx8MTY0MjAzOTc2Mw&ixlib=rb-1.2.1&q=85" alt="A red car on a snowy road">
</div>

<div>
  <img src="https://images.unsplash.com/photo-1641057273235-50725552d22a?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxNDU4OXwwfDF8cmFuZG9tfHx8fHx8fHx8MTY0MjAzOTc2Mw&ixlib=rb-1.2.1&q=85" alt="A red car on a snowy road">
</div>

<div>
  <img src="https://images.unsplash.com/photo-1641057273235-50725552d22a?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxNDU4OXwwfDF8cmFuZG9tfHx8fHx8fHx8MTY0MjAzOTc2Mw&ixlib=rb-1.2.1&q=85">
</div>

<div>
  <img src="https://images.unsplash.com/photo-1641057273235-50725552d22a?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxNDU4OXwwfDF8cmFuZG9tfHx8fHx8fHx8MTY0MjAzOTc2Mw&ixlib=rb-1.2.1&q=85">
</div>

<div>
  <img src="https://images.unsplash.com/photo-1641057273235-50725552d22a?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxNDU4OXwwfDF8cmFuZG9tfHx8fHx8fHx8MTY0MjAzOTc2Mw&ixlib=rb-1.2.1&q=85" alt="A red car on a snowy road">
</div>

```

The `:not()` will set orange-colored dashed borders to the images that don't have the `alt` attribute.

```css

div {
  width: 300px;
  height: 300px;
  margin: 1rem;
}

img {
  width: 300px;
  height: 300px;
  border: 5px solid burlywood;
}

img:not([alt]) {
  border: dashed 5px orange;
}

```

### Conclusion

So this is all about how useful this `:not()` pseudo-class is. And, there might be more other ways to use it, other than discussed above. So if you have more ideas or some ideas to improve these details, please add them here. It will make this article more useful for someone else. 

Also, I hope you enjoyed this article, and you can support me at [ko-fi](https://ko-fi.com/mkdaycode). I always appreciate your support. It really encourages me to keep going. 

 **_Happy Coding!_** 

[![image_description](https://cdn.ko-fi.com/cdn/kofi2.png?v=3)](https://ko-fi.com/mkdaycode) 
