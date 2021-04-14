:notebook: Learning SASS syntax.

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