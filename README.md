:notebook: Learning SASS syntax.

[toc]

# Sass docs

## https://sass-lang.com/documentation/syntax

## special functions

CSS defines many functions, and most of them work just fine with scss. they're parsed as function calls, resolved to plain CSS functions, and compiled as-is to CSS. there are a few exceptions, though, which have special syntax that can't just be parsed. all special function calls return unquoted strings

- url()

  url() is commonly used in CSS: it can take either a quoted or unquoted URL.

  because an unquoted URL isn't a valid SassScript expression, Sass needs special logic to parse it

  if the url()'s argument is a valid unquoted URL, sass parses it as-is, although interpolation may also be used to inject values.

  it it's not a valid unquoted URL(ex)contains variables or function calls)), it's parsed as a normal plain CSS function call

  ```scss
  $roboto-font-path: "../fonts/roboto";
  
  @font-face {
      // This is parsed as a normal function call that takes a quoted string.
      src: url("#{$roboto-font-path}/Roboto-Thin.woff2") format("woff2");
  }
  
  @font-face {
      // This is parsed as a normal function call that takes an arithmetic expression.
      src: url($roboto-font-path + "/Roboto-Light.woff2") format("woff2");
  }
  
  @font-face {
      // This is parsed as an interpolated special function.
      src: url(#{$roboto-font-path}/Roboto-Regular.woff2) format("woff2");
  }
  ```

  ```css
  @font-face {
    src: url("../fonts/roboto/Roboto-Thin.woff2") format("woff2");
  }
  @font-face {
    src: url("../fonts/roboto/Roboto-Light.woff2") format("woff2");
  }
  @font-face {
    src: url(../fonts/roboto/Roboto-Regular.woff2) format("woff2");
  }
  ```

  

- calc()

# Learn Sass In Under 2 Hours

## Brain Upgrade series

## By Jeremy Mouzin



Sass makes it easy to create and maintain stylesheets.

You can reuse previous work quickly.

There is online sass tool. https://www.sassmeister.com/

## Install on MacOS

```bash
sudo gem install sass
```

- Homebrew

  ```bash
  brew install sass/sass/sass
  ```



## How Sass Works

Sass transforms a source file(Sass syntax) -> valid CSS stylesheet

It uses **.scss** extension

SCSS: Sassy CSS

In HTML file link the generated .css file only.

Linking to a .scss file won't work.



## Generate a stylesheet

1. create style.scss

2. ```bash
   # at the directory where you created style.scss
   sass style.scss style.css
   ```

   It will create style.css & style.css.map

At the end of the CSS stylesheet, there is `/*# sourceMappingURL = style.css.map */`. It's there for speed up the Sass preprocessing computation. It doesn't need to be on your website. So, when you've done developing you can delete them.



## --watch

It's hard to produce the corresponding CSS stylesheet everytime.

To ease the process, you could use `--watch`.

```bash
sass --watch style.scss:style.css
```

Then it will tells you `Sass is watching for changes.`

You should not close that terminal or hit Ctrl+C unless you don't want to update the CSS file.

--watch option will detect automatically the changes in your Sass source file and process them to produce the new corresponding CSS stylesheet

Each time you change your style.scss source file and save it, the style.css file will be rewritten.

When you save your modifications, you'll see the command line prompt display a new message.

```bash
Compiled style.scss to style.css.
```

And the style.css content is updated.



## Variables

You can store any CSS value you can think of in variables to reuse them in your stylesheet wherever you want.

A variable must be first declared before using it.

```scss
// style.scss

// Declare a variable
$deep-green: #014b10;

// Use the variable
body {
  background-color: $deep-green;
}
```

```css
/* style.css */

body {
  background-color: #014b10;
}
```

1. any valid CSS color format

   1. \#014b10
   2. hsl(132, 97%, 15%)
   3. rgb(1, 75, 16)

2. any valid CSS value format

   1. $border: 1px solid green;
   2. $margins: 10px 2em 4rem 0;

3. Strings are valid: no quote, single quote, double quote

   $default-font: Source Sans Pro, "Helvetica", 'sans-serif';

4. numbers in variables names

   $padding-2px: 2px;

5. hypens and underscores are interchangeable

   But you should stick with one. Stay Consistent



## Variables Scopes

a variable can be declared inside a block

```scss
body {
  $my-color: #042;
  color: $my-color;
}
```

$my-color is available within body selector.

for variables declared outside of any block, 

- they're accessible from the declaration until the end of the file. 
- they're also accessible within any block in that space.

you could not access a variable declared below.

```scss
html {
  background-color: $my-color; // It is NOT accessible
}

$my-color: #999;

body {
  $local-color: #f1c;
  color: $my-color;
  background-color: $local-color;
}

div {
  border: 1px solid $local-color; // NOT accessible
}
```

you can make a variable scope global by using the `!global` flag.

BUT, from Dart Sass 2.0.0 !global assignments won't be able to declare new variables. adding `$var: null` at the root will make it work.

```scss
$max-width: null;

body {
  $max-width: 800px !global;
}

#main {
  max-width: $max-width; // It can be accessed
}
```



## Comments

Same as in CSS

```scss
// One line comments
/*
It's multi-
line
comments
*/
```

one line comments are **not copied** to the CSS stylesheet.

multiline comments are **copied** to the CSS stylesheet.



## Interpolation

Sass variables can be used in CSS selectors and property names. 

but you can't use the $var syntax.

```scss
$class-name: my-class;
$attribute: border;

p.$class-name { // INCORRECT
  color: #999;
}

h1 {
	$attribute: 1px solid red; // INCORRECT
}
```

if you want to use it, you must use interpolation `#{$var-name}`

by using interpolation, you simply replace it with its value

```scss
$class-name: my-class;
$attribute: border;

p.#{$class-name} { 
  #{$attribute}-color: #999;
}
```

```css
/* style.css */
p.my-class {
  border-color: #999;
}
```

you can even use interpolation within comments



## architecture

if project is large and you have lots of files and several different stylesheets, you don't want to mess up your source files and the CSS stylesheets.

```
medium-project
|-- index.html
|-- *.html
|-- src
|    -- style.scss
|    -- *.scss
|-- stylesheets
     -- style.css
     -- *.css
```



## Nested Rules

to avoid repetitons, Sass introduces nested rules

simply put children declarations within the parent rule.

```css
/* common repetition on css */

h2 {
  font-size: 1.4em;
}

h2 .small {
  font-size: 0.7em;
}

h2 .medium {
  font-size: 1em;
}
```

```scss
h2 {
  font-size: 1.4em;
  
  .small{
    font-size: 0.7em;
  }
  .medium {
    font-size: 1em;
  }
}
```

nested rules can work on several levels of nesting.

```scss
.parent {
  color: #000;
  .child1, .shild1a { // One level deep
    color: #111;
    .child2 { // two levels deep
      color: #222;
    }
  }
}
```

```css
/* css */
.parent {
  color: #000;
}
.parent .child1, .parent .child1a {
  color: #111;
}
.parent .shild1 .child2, .parent .child1a .child2 {
  color: #222;
}
```



## referencing parent selector with the & special character

if you want to specify a rule for the :hover state of a .parent class, 

```scss
.parent {
  :hover {
    border: 1px solid blue;
  }
}
```

code above won't work.

because in the css it will become `.parent :hover {}`. there will be a space between.

the correct way would be like this.

```scss
.parent {
  &:hover {
    border: 1px solid blue;
  }
}
```

below code is advanced examples of &

```scss
.parent {
  &:hover {
    color: #000;
  }
  .child1 & .child1a {
    color: #111;
  }
  .child2 & {
    color: #222;
  }
  .child3 {
    .child3-1 & {
      color: #333;
    }
  }
  &-small {
    font-size: .5em;
  }
}
```

```css
/* css */
.parent:hover {color: #000;}
.child1 .parent .child1a {color: #111;}
.child2 .parent {color:#222;}
.child3-1 .parent .child3 {color:#333;}
.parent-small {font-size:.5em;}
```

usint the & char will let you generate classes names very easily based on the parent value.



## partials and import

using several sass source files in project will make it easier to maintain and more flexible.

CSS built-in import feature is bad for network performances

each file importation requires one HTTP request

use several small source files and combine them into a single output file!



### import

@import

allows you to import any source file within another source file.

at the end you can combine any number of files together into a single one

### partials

partial files are regular sass source files with one particularity: their name starts with an underscore.

underscore prefix will tell sass to not generate the corresponding CSS output for this file.

by using partial files you can modularize your project

```
project
|-- index.html
|-- src
|    |-- partials
|    |    |-- _colors.scss
|    |    |-- _spacing.scss
|    |    |-- _visual.scss
|    |-- style.scss
|
|-- stylesheets
     |-- style.css
```

single command line to manage your whold project simply.

```bash
// from the project directory
sass --watch src:stylesheets
```

sass take all the .scss files contained in src and output the corresponding css stylesheets into stylesheets.

sass will looks in subdirectories, too.

```
project
|-- index.html
|-- src
|    |-- partials
|    |    |-- _colors.scss
|    |    |-- _spacing.scss
|    |    |-- _visual.scss
|    |-- style.scss
|    |-- sub
|         |-- example.scss
|
|-- stylesheets
     |-- style.css
     |-- sub
     			|-- example.css
```

by adding sub/example.scss to src folder, stylesheets also have sub/example.css file.



to include the files in partials/ you need to add the line at the beginning of the main file

```scss
// style.scss

@import "partials/colors";
@import "partials/spacing";
@import "partials/visual";
```



## Mixins

mixins allow you to write sass code once and reuse it wherever you want in your source file.

declare the code with @mixin

use it with @include

```scss
@mixin apply-green-border {
  border: 1px solid green;
  border-radius: 10px;
}

blockquote {
  @include apply-green-border;
}
```

```css
blockquote {
  border: 1px solid green;
  border-radius: 10px;
}
```

variables can be used in mixins

```scss
$color-border: red;
$size-radius: 6px;

@mixin apply-border($color, $radius) {
  border: 1px solid $color;
  border-radius: $radius;
}

.apply-border {
  @include apply-border(green, 20px);
}
.apply-border-global {
  @include apply-border($color-border, $size-radius);
}
```

```css
.apply-border {
  border: 1px solid green;
  border-radius: 20px;
}
.apply-border-global {
  border: 1px solid red;
  border-radius: 6px;
}
```

through mixin you can declare new CSS rule too

```scss
@mixin awesome-p {
  .awesome-p {
    font-size: 2rem;
    color: #ff9800;
    margin: 2rem;
  }
}
@include awesome-p;
```

```css
.awesome-p {
  font-size: 2rem;
  color: #ff9800;
  margin: 2rem;
}
```

include mixin into another mixin is also possible

```scss
@mixin text-blue { color: blue; }
@mixin background-grey { background-color: grey; }
@mixin info-style {
  @include text-blue;
  @include background-grey;
}

div#info {
  @include info-style;
}
```

```css
div#info {
  color: blue;
  background-color: grey;
}
```

you can even include a mixin within itself



you can set a default value to the arguments passed in mixins

```scss
@mixin apply-border2($thickness: 1px, $color: red) {
  border: $thickness solid $color;
}

div#default {
  @include apply-border2;
  // @include apply-border2(); is same
}
```

```css
div#default {
  border: 1px solid red;
}
```



```scss
div#default2 {
  @include apply-border2($color: blue);
}

div#default3 {
  @include apply-border2(5px);
}

div#default4 {
  @include apply-border2(green);
}
```

```css
div#default2 {
  border: 1px solid blue;
}

div#default3 {
  border: 5px solid red;
}

div#default4 {
  border: green solid red;
}
```

use an arguments list and gets flexibility

arguments list parameter: ...

arguments list parameter need to be at the end of the declaration

```scss
@mixin paddings ($padding-list...) {
  padding: $padding-list;
}
.paddings-4-values { @include paddings(4em, 5em, 6em, 7em); }
.shorthand-2-values { @include paddings(1em, 2em); }
.shorthand-1-values { @include paddings(3em); }
```

```css
.paddings-4-values {
  padding: 4em, 5em, 6em, 7em;
}

.shorthand-2-values {
  padding: 1em, 2em;
}

.shorthand-1-values {
  padding: 3em;
}
```



## Inheritance

to keep your source files short and clean by applying DRY principle.

@extend

```scss
.message {
  border: 1px solid #c69;
  padding: $padding-3x;
  color: $deep-green;
}

.success {
  @extend .message;
  border-color: violet;
}
```

```css
.message, .success {
  border: 1px solid #c69;
  padding: 30px;
  color: #014b10;
}

.success {
  border-color: violet;
}
```

.success class will get all the CSS properties from .message and add new ones on top of it.

by using inheritance, you can avoid using several classes in a HTML.

if .success didn't inherit .message,

html code would be like 

`<div class="message success">Success!</div>`



using a mixin instead of inheritance would duplicate the code.

always use inheritance over mixin when possible to reduce your website content  size

using inheritance allows you to add new rules related to .message and make all other rules that extend it automatically inherit these modifications

```scss
.message {
  border: 1px solid #c69;
  padding: $padding-3x;
  color: $deep-green;
}

// new rules to add
.message:hover {
  background-color: blue;
}

.success {
  @extend .message;
  border-color: violet;
}
```

```css
.message, .success {
  border: 1px solid #c69;
  padding: 30px;
  color: #014b10;
}

.message:hover, .success:hover {
  background-color: blue;
}

.success {
  border-color: violet;
}
```

inheritance works with any type of CSS selector(it doesn't have to be class)

extend a rule using several different selectors,

extend selectors that already inherit from other selectors are possible



### placeholder selectors

a placeholder selectors tells sass to not generate the corresponding css for that selector

it will help you keep your css leaner and avoid names collisions

in the example above, you were be able to get rid of .message class from the HTML code.

now .message class doesn't have to be in your CSS anymore.

to .success class inherit from .message, using a placeholder selector

```scss
%message {
  border: 1px solid #c69;
  padding: $padding-3x;
  color: $deep-green;
}

%message:hover {
  background-color: blue;
}

.success {
  @extend %message;
  border-color: violet;
}
```

```css
.success {
  border: 1px solid #c69;
  padding: 30px;
  color: #014b10;
}

.success:hover {
  background-color: blue;
}

.success {
  border-color: violet;
}
```



```scss
%message {
  border: 1px solid #c69;
  padding: $padding-3x;
  color: $deep-green;
}

%message:hover {
  background-color: blue;
}

.success {
  @extend %message;
  border-color: violet;
}

.info {
  @extend %message;
  border-color: yellowgreen;
}
```

```css
.info, .success {
  border: 1px solid #c69;
  padding: 30px;
  color: #014b10;
}

.info:hover, .success:hover {
  background-color: blue;
}

.success {
  border-color: violet;
}

.info {
  border-color: yellowgreen;
}
```

%message selector disappears from the css but still in the css as a .success

using placeholder selectors can let you create classes quickly based on existing code while keeping your css clean and small

```scss
div %message-action #action {
  font-weight: bold;
  color: red;
}

.success2 {
  @extend %message;
  @extend %message-action;
}
```

```css
div .success2 #action {
  font-weight: bold;
  color: red;
}
```

`div %message-action #action ` won't be in the css because it contains a placeholder selector

.success2 class name is used at the exact place of the placeholder selector



## Operators

you can use mathmatical operators like +, -, *, / and % to compute property values

```scss
div.operators {
  width: 100px + 25px;
  height: 10 * 25px;
  font-size: 3.2rem - .8;
  padding: 5 + 12%;
  line-height: 5 * 5px;
}
```

```css
div.operators {
  width: 125px;
  height: 250px;
  font-size: 2.4rem;
  padding: 17%;
  line-height: 25px;
}
```

incorrect way of using operators

1. mix incompatible units

   ```scss
   div.operators {
     width: 10em + 25px;
     width: 20px + 13%;
   }
   ```

2. css output has no meaning

   ```scss
   div.operators {
     width: 100 + 25; // width: 125;
     font-size: (100 / 4); // font-size: 25;
   }
   ```

3. produces invalid (squared) css units

   ```scss
   div.operators {
     width: 4px * 25px;
     width: 3em * 2em;
   }
   ```

   

### enforcing divisions

the division symbol(/) may be used in css to separate values

```css
/* css 
	below code sets font-size: 20px, line-height: 30px
	/ is not use for a division
*/
h1 {
  font: 20px/30px helvetica;
}
```

sass will detect it and won't compute a division.

but sometimes, you need to enforce a division.

use parenthesis and take care of the unit

```scss
div {
  width: (100px/4); // 25px
  //incorrect use
  width: ((100/4)px); // 25 px
  width: (100/4)px; // 25 px
  width: (100/4px); // it is trying to divide unitless value by pixels
  width: 100px/4; // 100px/4
  width: 100/4px; // 100/4px
  width: 100px/4px; // 100px/4px
  width: (100px/4px); // 25
}
```

special cases like use sass variables or other operators, you don't need to use parenthesis

```scss
div.operators#special {
  $container-width: 400px;
  width: $container-width / 2;
  height: $padding-3x * 2 + 100px / 2;
}
```

```css
div.operators#special {
  width: 200px;
  height: 110px;
}
```



### preventing divisions

if you want to use variables with the css value separation symbol /, use interpolation

```scss
$default-font-size: 1.8em;
div.no-division {
  font: #{$default-font-size}/1.5;
}
```

```css
div.no-division {
  font: 1.8em/1.5;
}
```





