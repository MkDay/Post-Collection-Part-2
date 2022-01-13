# not()


In CSS, pseudo-classes are very useful and some of them like mouse related events widely used. For examples,
* `:hover`
* `:target`
* `:active` 

But there are a few other pseudo-classes out there which I found aren't much popular but very useful and very interesting.

One of useful and I feel very interesting pseudo-class is `not()` which is known as *negation pseudo-class*.

Unlike other pseudo-classes you are familiar with, it can accept arguments within its parentheses. That's why it referred to as a *functional pseudo-class*.

As you guessed, it selects everything except that inside its parentheses. Either you can include one selector or selector-list inside the parentheses as its arguments.

**Syntax:**

```css

selector:not(simple-selector) {}

/* OR */

selector:not(<selector-list>) {}

``` 

As I mentioned above you can experiment a lot with it to get understand how interesting it is. So let me try some of them.

Here is the HTML code that we're going to style using `not()` pseudo-class.

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

#### 2. Select everything on the page except the selector(s) within the parentheses. 

`:not(.not-selected) {}`

Unlike the above example, this rule selects everything including `<html>` and `<body>` elements on the page except elements with the class `.not-selected`. 

```css

:not(.not-selected) {
  color: blue;
  border-color: burlywood;
}

```

● #### 3. Difference between div:not() & div :not()

have to work on this.
https://stackoverflow.com/questions/32309416/different-behavior-for-pseudo-class-with-space-and-without-space

body :not() {}
♤ as sof says, pseudo-classes couldn't have spaces between element & :pseudo-class. but in my case it works.

body:not() {}
♤ what doesn't work is body:not() {}
♤ it only adds styles to h2. 
♤ i wanna know why.

but,
div:not() {} works as expected.
div :not() {} doesn't style anything.



```css

/* ======== styles only h2 ========== */
/*body:not(.not-selected) {
  color: blue;
  border-color: burlywood;
}*/

/* ======== styles everything except second one (expected styles) ========== */

/*body :not(.not-selected) {
  color: blue;
  border-color: burlywood;
}*/


/* ============ case : div working ======== */

/*div:not(.not-selected) {
  color: blue;
  border-color: burlywood;
}*/


/* ============ case : div not working ======== */

/*div :not(.not-selected) {
  color: blue;
  border-color: burlywood;
}*/

```

#### 4. Exclude multiple selectors

In this case we're going to select all the element except `.not-selected` and `#heading`.

There are two ways to do the job, shown as below.
1. `body :not(.not-selected, #heading) {}`
2. `body :not(.not-selected):not(#heading) {}`

The first method is not well supported all the browsers yet. Although we can use the second way instead.


```css

/* select anything that is not class="not-selected" or id="heading" */

body :not(.not-selected):not(#heading) {
  color: blue;
  border-color: burlywood;
}

/* below code is not well supported yet */

/*

body :not(.not-selected, #heading) {
  color: blue;
  border-color: burlywood;
}

*/

```

#### 5. Sometimes it seems nonsence

If we use universal selector `*` with `:not()` like below it basically says *do NOT sytle anything in the document* which is meaningless.

```css

:not(*) {
  color: blue;
  border-color: burlywood;
}

```
#### 6. Nested pseudo-class 

It doesn't allow to use `:not()` inside another `:not()` pseudo-class. In short it doesn't allow nested `:not()`.


```css

/* not allowed */

/*
:not(:not()) {}
*/


/* this doesn't work */

body :not(div:not(.selected)) {
  color: blue;
  border-color: burlywood;
}

```

However, we can still use other nested pseudo-classes like below.

The code selects everything on the page except the 3rd child. In this case 3rd child of the `body` is `<div id="second" class="not-selected">`


```css

:not(:nth-child(3)) {
  color: blue;
  border-color: burlywood;
}

```

#### 7. It increase specificity of an element

At the first glance, the below code looks silly. It selects elements with `id="first"` except with `id="second"`. 

As we know,
* one element couldn't have multiple `id`s.
* one `id` couldn't be assigned to multiple elements.

So our code becomes meaningless.

But when it comes to the [specificity](), ` #first:not(#second) {}` has higher specificity than `#first {}`. 

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

#### 8. Elements with multiple classes

The code below selects elements with the class `selected` except elements with the class `special`. 

Also, in our example HTML has a `div` with two class names which is,

`<div id="third" class="selected special">`

So we can exclude this `div` when adding styles like this way.

```css

.selected:not(.special) {
  color: blue;
  border-color: burlywood;
}


```


● #### 9. It cannot select all the anscestors of the element

Here we are trying to select 

```html

<h3><a href="#">Community Members</a></h3>

<table>

  <tr>
    <th>name</th>
    <th>id</th>
  </tr>
  
  <tr>
    <td><a href="#">John</a></td>
    <td>12</td>
  </tr>
  
  <tr>
    <td>Bob</td>
    <td>13</td>
  </tr>
  
</table>

```

```css

body :not(table) a {
 text-decoration: none;
  color: red;
}

```

♤ i guess this says, select links that resides inside the body, but outside the table. 
♤ mdn says, you cannot exclude every ancsestor it can only selects one element which is table but not <tr>.


#### 10. Bring it to the real-life

As I mentioned earlier, the `:not ()` pseudo-class is more interesting not only because of its weird behavior, but also because of its usefulness in some real-world situations.

Let me try some of examples.

 
**(1) Style all the buttons on the page except the ones that are disabled**

We have a couple of buttons to styles like below. 

```html

<button class="btn">Sign in</button>
<button class="btn">Sign out</button>
<button class="btn" disabled>Delete Account</button>

```

In this case we want to style only the buttons that are not disabled. 

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

To improve accessibility, it is a good practice to always add `alt` attribute for images of the page. 

When we have a large array of images it is usual we have forgotten to add `alt` attribute for some images. 

So this is useful in that case too.

There are five images and two of them don't have `alt` attribute that assigned to them.

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

The `:not()` will set dashed borders which are in orange color to images that don't have `alt` attribute.

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

So this is all about how useful `:not()` pseudo-class is. And, there might be a lot more another ways to use it other than discussed above. So if you have more ideas or some ideas to improve these details, please add them here. It will make this article more useful for someone else. 

Also, I hope you enjoyed this article, and you can support me at [ko-fi](https://ko-fi.com/mkdaycode). I always appreciate your support. It really encourages me to keep going. 

 **_Happy Coding!_** 

[![image_description](https://cdn.ko-fi.com/cdn/kofi2.png?v=3)](https://ko-fi.com/mkdaycode) 
