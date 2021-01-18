### SASS (Syntactically Awesome Style Sheet)
- A css pre-processor
- Sass lets you use features that do not exist in CSS, like variables, nested rules, mixins, imports, inheritance, built-in functions and other stuff.
- @mixin
  - The @mixin directive lets you create CSS code  that is to be reused throughout the website.
  - Define a mixin
  > @mixin name {property:value; property:value;}
  - The @include directive is used to include a mixin.
  > selector {@include mixin-name}
  - A mixin can also include other mixins.
  - Passing variables to a mixin
  ```html
  /* Define mixin with two arguments */
@mixin bordered($color, $width) {
  border: $width solid $color;
}

.myArticle {
  @include bordered(blue, 1px);  // Call mixin with two values
}

.myNotes {
  @include bordered(red, 2px); // Call mixin with two values
}
  ```
