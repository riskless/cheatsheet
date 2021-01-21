### Example 1
```html
<!DOCTYPE HTML>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>CSS Tutorial</title>

  <!-- Links to an external Style Sheet -->
  <link rel="stylesheet" type="text/css" href="mainstyle.css">
</head>

<body>
  <h1>CSS Tutorial</h1>

  <p>A CSS3 Tutorial that will cover pretty much everything in one tutorial</p>

  <h2>Let's Get Started</h2>

  <div>
    <h3>My Favorite Sites</h3>
    <ul>
      <li><a class="sitelink" href="google.com">Google</a></li>
      <li><a class="sitelink" href="reddit.com">Reddit</a></li>
      <li><a class="sitelink" href="amazon.com">Amazon</a></li>
    </ul>
  </div>

  <p id="tonyquote">We wake up on a box,
    eat breakfast from a box, go to work in a box,
    sit in a box office all day, have lunch from a box,
    then go home in a box, have dinner from a box,
    in front of the box.<span id="tonyname"> - Tony Robbins</span></p>

    <p class="sitelink">I also like <a href="youtube.com">YouTube</a></p>

    <div id="tutorials">
      <ol>
        <li>Rails</li>
        <li>NodeJS</li>
        <li>Android Games</li>
      </ol>
      <ul>
        <li>SASS</li>
        <li>HAML</li>
      </ul>
    </div>

    <h3>Favorite Video Games</h3>
    <p>List of my current <a href="nintendogames">favorite video games</a></p>
    <ul>
      <li alt="nintendo">Super Mario 3D Land</li>
      <li alt="nintendo">Monster Hunter 4 Ultimate</li>
      <li alt="nintendo">Pokemon Omega Ruby</li>
      <li alt="klei entertainment">Don't Starve</li>
    </ul>

  </body>
</html>
```

```html
/* mainstyle.css */
@import "heading.css";
@import "paragraph.css";

/* @import rules must precede all others */

* { font-family: "Arial Black", Gadget, sans-serif;}

div * { font-family: "Comic Sans MS", cursive, sans-serif; }

.sitelink { font-family: Georgia, serif; }

#tonyquote{
  font-family: "Lucida Sans Unicode", "Lucida Grande", sans-serif;
  color: black;
}

p.sitelink {
  font-family: "Palatino Linotype", "Book Antiqua", Palatino, serif;
  color: black;
}

span#tonyname {
  font-family: "Times New Roman", Times, serif;
  color: blue;
}

#tutorials ol li {
  color: purple;
}

#tutorials ul li {
  color: green;
}

h3 + p{
  font-style: italic;
}

h3 + p > a {
  color: red;
}

p[class] {
  background: gray;
}

p[id] {
  background: yellow;
}

*[alt~="nintendo"] {background: orange;}

/* heading.css */
h1, h2 {color: white; background: black; padding: 15px;}

/* paragraph.css */
p { color: blue;}
```

### Example 2
```html
<!DOCTYPE HTML>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>CSS Tutorial</title>

  <link rel="stylesheet" type="text/css" href="stylesheet2.css">
</head>

<body>

<p id="lorem">
  Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do
  eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut
  enim ad minim veniam, quis nostrud exercitation ullamco laboris
  nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor
  in reprehenderit in voluptate velit esse cillum dolore eu fugiat
  nulla pariatur. Excepteur sint occaecat cupidatat non proident,
  sunt in culpa qui officia deserunt mollit anim id est laborum.
</p>

<!-- Demonstrate Font Properties -->

<p id="fsNormal">Font Style Normal</p>
<p id="fsItalic">Font Style Italic</p>
<p id="fsOblique">Font Style Oblique</p>
<p id="fvNormal">Font Varient Normal</p>
<p id="fvSmallCaps">Font Varient Small-Caps</p>
<p id="fwNormal">Font Weight Normal</p>
<p id="fwBold">Font Weight Bold</p>
<p id="fwBolder">Font Weight Bolder</p>
<p id="fwLighter">Font Weight Lighter</p>
<p id="fszMedium">Font Size Medium</p>
<p id="fszXXSmall">Font Size XX-Small</p>
<p id="fszSmall">Font Size Small</p>
<p id="fszXXLarge">Font Size XX-Large</p>
<p id="fszLarge">Font Size Large</p>
<p id="fszPrct">Font Size 200%</p>
<p id="fsz25pt">Font Size 25pt</p>
<p id="lhNormal">Font Line Height Normal</p>
<p id="lh25pt">Font Line Height 25pt</p>
<p id="lh200Prct">Font Line Height 200%</p>

<!-- Demonstrate Font Properties End -->

</body>
</html>
```

```html
/* stylesheet2.css */
#lorem {
  /*  Inside Border
      padding 10px 10px 10px 10px : Top, Right, Bottom, Left
      padding 10px 25px 15px : Top, Right / Left, Bottom
      padding 10px 15px : Top / Bottom, Left / Right
  */
  padding-top: 20px;
  padding-bottom: 20px;
  padding-left: 20px;
  padding-right: 20px;

  /*  Outside Border
      margin 10px 10px 10px 10px : Top, Right, Bottom, Left
      margin 10px 25px 15px : Top, Right / Left, Bottom
      margin 10px 15px : Top / Bottom, Left / Right
  */
  margin-top: 10px;
  margin-bottom: 10px;
  margin-left: 10px;
  margin-right: 10px;

  /*
      border: 5px solid green;
      border-width, border-style, border-color
      none, dotted, dashed, solid, double, groove, ridge, inset, outset
  */
  border-top: 5px solid #00308F;
  border-bottom: 5px dotted #00308F;
  border-left: 5px dashed #00308F;
  border-right: 5px double #00308F;

  /*
  background: color position/size repeat origin clip attachment
  image|initial|inherit;
  background-position: left top bottom right
  (If you only specify one the other value will be center)
  background-size: auto length;
  background-repeat: repeat repeat-x repeat-y no-repeat
  background: #F2F3F4 url("Repeat.png") repeat;
  */
  background: #F2F3F4;

  /*
  font: font-style font-variant font-weight font-size/line-height font-family;
  font-style: normal, italic, oblique
  font-varient: normal, small-caps
  font-weight: normal, bold, bolder, lighter, 100 - 900
  font-size: medium, xx-small - small, xx-large - large, %, length in pt
  line-height: normal, length in pt, %
  font-family: "Times New Roman", Georgia, Serif
  */

  color: black;
}

#fsNormal {
  font-style: normal;
}

#fsItalic {
  font-style: italic;
}

#fsOblique {
  font-style: oblique;
}

#fvNormal {
  font-variant: normal;
}

#fvSmallCaps {
  font-variant: small-caps;
}

#fwNormal {
  font-weight: normal;
}

#fwBold {
  font-weight: bold;
}

#fwBolder {
  font-weight: bolder;
}

#fwLighter {
  font-weight: lighter;
}

#fszMedium {
  font-size: medium;
}

#fszXXSmall {
  font-size: xx-small;
}

#fszSmall {
  font-size: small;
}

#fszXXLarge {
  font-size: xx-large;
}

#fszLarge {
  font-size: large;
}

#fszPrct {
  font-size: 200%;
}

#fsz25pt {
  font-size: 25pt;
}

#lhNormal {
  line-height: normal;
}

#lh25pt {
  line-height: 25pt;
}

#lh200prct {
  line-height: 200%;
}

#lh200prct {
  line-height: 200%;
}
```

### Example 3
```html
<!DOCTYPE HTML>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>CSS Tutorial</title>

  <link rel="stylesheet" type="text/css" href="stylesheet3.css">
</head>

<body>

<div id="parent-div">
  <div id="default">Default</div>
  <div id="centered">Centered</div>
  <div id="centered-text">Centered Text</div>
</div>

<!-- Demonstrate Absolute Postioning -->

<div id="top-left-pos">Top Left
  <div id="bottom-right-tl-parent">Bottom Right Parent</div>
</div>

<div id="top-right-pos">Top Right
  <div id="bottom-left-tr-parent">Top Right Parent</div>
</div>

<p id="fixed-pos">
  I'm staying right here
</p>

<p>
  Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do
  eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut
  enim ad minim veniam, quis nostrud exercitation ullamco laboris
  nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor
  in reprehenderit in voluptate velit esse cillum dolore eu fugiat
  nulla pariatur. Excepteur sint occaecat cupidatat non proident,
  sunt in culpa qui officia deserunt mollit anim id est laborum.
</p>

<p>
  Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do
  eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut
  enim ad minim veniam, quis nostrud exercitation ullamco laboris
  nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor
  in reprehenderit in voluptate velit esse cillum dolore eu fugiat
  nulla pariatur. Excepteur sint occaecat cupidatat non proident,
  sunt in culpa qui officia deserunt mollit anim id est laborum.
</p>

<p>
  Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do
  eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut
  enim ad minim veniam, quis nostrud exercitation ullamco laboris
  nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor
  in reprehenderit in voluptate velit esse cillum dolore eu fugiat
  nulla pariatur. Excepteur sint occaecat cupidatat non proident,
  sunt in culpa qui officia deserunt mollit anim id est laborum.
</p>

<div id="relative-1">Relative Layout</div>

<div id="relative-2">Relative Layout 2</div>

<p>
  Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do
  eiusmod tempor incididunt ut labore et dolore magna aliqua. <img src="LittleBrain.png"> Ut
  enim ad minim veniam, quis nostrud exercitation ullamco laboris
  nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor
  in reprehenderit in voluptate velit esse cillum dolore eu fugiat
  nulla pariatur. Excepteur sint occaecat cupidatat non proident,
  sunt in culpa qui officia deserunt mollit anim id est laborum.
</p>

<div id="crop"><img src="LittleBrain.png"></div>

</body>

</html>
```

```html
/* stylesheet3.css */
#parent-div{
  background: #B2BEB5;
  border: 0.1em solid black;
}

#default{
  background: #DBE9F4;
}

/*
  auto makes sure space on the left = right
  Show how changing between em, % and pixels changes things
*/
#centered{
  background: #89CFF0;
  width: 50%;
  margin: auto;
}

/* text-align: left, right, center, justify */
#centered-text{
  text-align: center;
}

/* Absolute Positioning : Positioning Based on the Document */
#top-left-pos{
  background: #89CFF0;
  border: 0.1em solid black;
  position: absolute;
  width: 200px;
  height: 100px;
  z-index: 4; /* Element with highest z index goes on top */
}

#bottom-right-tl-parent {
  background: #DBE9F4;
  position: absolute;
  bottom: 0px;
  right: 0px;
}

#top-right-pos{
  background: #89CFF0;
  border: 0.1em solid black;
  position: absolute;
  width: 400px;
  height: 100px;
  top: 0px;
  right: 0px;
}

#bottom-left-tr-parent {
  background: #DBE9F4;
  position: absolute;
  bottom: 0px;
  left: 0px;
}

/* Fixed positioning : Positioning Based on the Browser */

#fixed-pos{
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  background: #DBE9F4;
  border: 0.1em solid black;
  margin: 0;
}

/* Relative Positioning : Positioned Relative to the original */

#relative-1{
  background: #DBE9F4;
  border: 0.1em solid black;
  height: 100px;
}

#relative-2{
  position: relative;
  left: 20px; /* 20px to the right of original */
  top: -30px; /* 30px up from original */
  width: 300px;
  background: yellow;
  border: 0.1em solid black;
}

/* Floating Content : Float an element to the left or right of the parent */

img[src="LittleBrain.png"] {
  float: left;
  padding: 10px;
}
```

### Example 5
```html
<!DOCTYPE HTML>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>CSS Tutorial</title>

  <link rel="stylesheet" type="text/css" href="stylesheet4.css">
</head>

<body>

  <div class="little-brain-clip">
    <img src="LittleBrain.png">
  </div>

  <div class="little-brain-small overflow-visible">
    <img src="LittleBrain.png">
  </div>

  <div class="little-brain-small overflow-hidden">
    <img src="LittleBrain.png">
  </div>

  <div class="little-brain-small overflow-scroll">
    <img src="LittleBrain.png">
  </div>

</body>

</html>
```

```html
/* stylesheet4.css */
/*
clip crops an element based on coordinates from the top left
rect(top, right, bottom, left)
*/

div.little-brain-clip{
  position:absolute;
  width:100px;
  clip:rect(0px,50px,50px,0px);
}

/* I shrink the size of the div so the image won't fit */

div.little-brain-small{
  position:absolute;
  width: 100px;
  height: 50px;
}

/* With overflow visible the part of the image that doesn't fit in the div
shows anyway */

div.overflow-visible{
  top: 100px;
  overflow: visible;
}

/* With overflow hidden the part of the image that doesn't fit in the div
is hidden */

div.overflow-hidden{
  top: 200px;
  overflow: hidden;
}

div.overflow-scroll{
  top: 300px;
  overflow: scroll;
}
```

### Example 5
```html
<!DOCTYPE HTML>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>CSS Tutorial</title>

  <link rel="stylesheet" type="text/css" href="stylesheet5.css">
</head>

<body>
<!-- A div wrapper is often used to define the size and position
of the page divs and spans should be used only for styling and
not structural reasons -->
<div id="wrapper">

  <header>
    <a href="http://localhost/css/csstut4.html">
      <img id="websiteLogo" alt="New Think Tank Logo" src="nttlogo.png"/>
    </a>
  </header>

  <nav id="horzNav">
    <p>
      <a href="google.com">Google</a>
      <a href="youtube.com">YouTube</a>
      <a href="yahoo.com">Yahoo</a>
    </p>
  </nav>

  <!-- main surrounds the main content of the document
  You can only have one main -->
  <main id="content">

    <!-- We surround blocks of content in a document with section tags -->
    <section>
      <!-- Section elements should have a heading specific to it -->
      <h1>Interesting</h1>

      <!-- An article is a self contained block of content that is complete on
      its own with a heading and content -->
      <article>

        <h2>Body Language</h2>
        <p id="para-1">In this article, I’ll cover <span>Arm and Hand Body
          Language Analysis</span>. I know doubt know someone that reads this article
          will say to themselves, <span id="common-quote">"I just feel more comfortable with my arms
          crossed in front of me."</span> Body Language Analysis is used because it
          has been proven to work for many people.
        </p>
        <p id="para-2">
          If you feel comfortable with a specific body position it is because
          you are feeling a certain way. Ask yourself, when would you feel
          comfortable with your arms crossed while enjoying yourself with a
          loved one? Have you ever seen anyone cross their arms in excitement?
          Just a few things to think about, as you read this article.
        </p>
        <p id="para-3">
          This article was created because one of my readers asked for it. If
          you have any <span>questions</span> leave them in the comment section below. I’ll
          gladly answer any questions that I can.
        </p>
        <p id="para-4">
          When shaking someones hand, if your hand is on top you are trying
          to dominate and vice versa.
        </p>
        <p id="para-5">
          An equal handshake, in which both people shake in a vertical manor,
          with equal pressure, is the best way to generate good rapport.
        </p>

        <dl id="buttons">
          <dt class="site-button"><a href="google.com">Google</a></dt>
          <dt class="site-button"><a href="youtube.com">YouTube</a></dt>
          <dt class="site-button"><a href="yahoo.com">Yahoo</a></dt>
        </dl>

        <dl id="kids-pics">
        <dt><a><img src="kids-1.jpg" width="60" height="34">
          <img class="hidden" src="kids-1.jpg" width="600" height="338"></a></dt>
        <dt><a><img src="kids-2.jpg" width="60" height="34">
          <img class="hidden" src="kids-2.jpg" width="600" height="338"></a></dt>
        <dt><a><img src="kids-3.jpg" width="60" height="34">
          <img class="hidden" src="kids-3.jpg" width="600" height="338"></a></dt>
        </dl>

      </article>

    </section>

  </main>

  <div id="sidebar">
    <ol id="sidebar-list">
      <li>Random 1</li>
      <li>Random 2</li>
      <li>Random 3</li>
    </ol>

    <ol id="sidebar-list-2">
      <li>Random 1</li>
      <li>Random 2</li>
      <li>Random 3</li>
    </ol>

    <h3>Favorite Movies</h3>
      <h4>District 9</h4>
      <h4>The Matrix</h4>
      <h4>The Cabin in the Woods</h4>

    <h3>Others</h3>
      <h4>Minority Report</h4>
      <h4>Lord of the Rings</h4>
      <h4>Fargo</h4>
  </div>

  <div id="filler"></div>

  <footer>
    <!-- copyright is normally placed in a small tag -->
    <small>copyright 2014</small>
  </footer>

</div> <!-- END OF WRAPPER -->

</body>

</html>
```

```html
/* stylesheet5.css */
/* Change the color of links in the document based on events */
a:hover { color: blue; }
a:active { color: red; }
a:visited { color: #989898; }

body {
  background: #5D8AA8;
}

div#wrapper{
  margin:0 auto;
  background:white;
  width:800px;
  height:1500px;
}

header {
  /* Display like a <p> instead of like a <span> */
  display:block;
  /* Padding Top, Right, Bottom, Left */
  padding: 0px 0px 10px 0px;
}

#websiteLogo{
  padding: 5px 0px 0px 5px;
}

/* Treat the nav tag as a block element */
#horzNav {
  display:block;
  /* Padding Top, Right, Bottom, Left */
  padding: 5px 20px 5px 0px;
  background: black;
}

/* Remove underlines from links in the nav element */
#horzNav a {
  text-decoration: none;
  padding: 0px 0px 0px 20px;
  color: white;
}

/* Add hover color */
#horzNav a:hover {
  color: #89CFF0;
}

main {
  /* Padding Top, Right, Bottom, Left */
  padding: 10px 10px 20px 20px;
  float: left;
  width: 600px;
}

#sidebar{
  padding: 20px 10px 0px 10px;
  float: right;
  width: 150px;
  height: auto;
  background: #DBE9F4;
}

h2{
  font-size: 1em;
}

/* text-align aligns text either to the left, center, or right */
/* word-spacing defines the size of the space between words */
#para-1 {
  text-align: left;
  word-spacing: .5em;
}

#para-2 {
  text-align: right;
}

#para-3 {
  text-align: center;
}

#para-4 {
  text-indent: 2em;
}

/* letter-spacing changes the space between letters */
#para-5 {
  letter-spacing: .5em;
}

/* clear makes sure no elements appear next to footer */
footer{
  clear: both;
  background: black;
  color: white;
  padding: 10px 10px 10px 10px;
}

/* Different list marker options
circle, disc, lower-latin, lower-greek, upper-latin, upper-greek,
square, url("pic.png"), upper-roman, lower-roman, decimal
*/
#sidebar-list{
  list-style-type: lower-latin;
  list-style-position: outside;
}

/* You can define that list markers should be inside list item
content boxes (Default is outside above)*/

#sidebar-list-2{
  list-style-position: inside;
}

/* You can place content before an element */
p#para-1:before{
  content: "*";
}

/* You can place content after as well */
p#para-1:after{
  content: "@";
}

/* Auto generate numbered content
Create a counter to count the number of h3 tags starting with 1
and store the value in num, or sub. You can also define the increment
amount.
You can output the values with content
*/
h3{
  font-size: .9em;
  counter-increment: num;
}

h4{
  font-size: .7em;
  counter-increment: sub 2;
}

h3:before{
  content: counter(num) " ";
}

h4:before{
  content: counter(num) "." counter(sub) " ";
}

/* There are many CSS Pseudo-classes */
/* You can style the first letter in an element */
#para-2:first-letter{
  font-size: 25pt;
}

/* You can style the first line */
#para-3:first-line{
  background: yellow;
}

/* Color the first child span in the document */
span:first-child{
  background-color: orange;
}

/* Select every p that is the last child of its parent */
p:last-child {
  background: #FAE7B5;
}

/* Select the 2nd child span */
span:nth-child(2){
  background: #FFE4C4;
}

h3:hover {
  background: orange;
}

/* We can change the cursor when an element is hovered over
pointer, crosshair, move, text, wait, progress, help, url(samp.cur)
*/
#common-quote {
  cursor: pointer;
}

/* Draw a box around an element */
#para-4 {
  outline: 2px solid blue;
}

#para-5 {
  border: 2px solid black;
}

/* You can create buttons with hover effects as well */
dl#buttons{
  width: 150px;
}

dt.site-button{
  text-align: center;
  margin-bottom: 5px;
}

dt.site-button a{
  display: block;
  color: white;
  text-decoration: none;
}

dt.site-button a:link{
  background: blue;
  border: 5px solid blue;
}

dt.site-button a:hover{
  background: green;
  border: 5px solid blue;
}

dl#kids-pics a img.hidden{
  visibility: hidden;
  position: absolute;
  left: 110px;
  top: 742px;
}

dl#kids-pics a:hover img.hidden{
  visibility: visible;
}
```

### Example 6
```html
<!DOCTYPE HTML>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>CSS Tutorial</title>

  <link rel="stylesheet" type="text/css" href="stylesheet6.css">
</head>

<body>

<p>
  <span id="underline">Underline </span>
  <span id="line-through">line through </span>
  <span id="overline">overline </span>
  <span id="lowercase">LOWERCASE </span>
  <span id="uppercase">uppercase </span>
  <span id="capitalize">capitalize </span>
</p>

<p id="whitespace-pre">
  White space      will    be     preserved
</p>

<p id="never-wrap">
  Lorem ipsum dolor sit amet, consectetur adipiscing elit. Cras accumsan tortor vitae aliquet imperdiet. Sed a convallis sapien. Praesent a tellus vitae enim scelerisque pharetra in et nunc. Praesent vel mauris non leo ultrices faucibus. Nulla sit amet ipsum neque. Ut interdum diam a lorem consequat blandit. Suspendisse faucibus magna mauris, id blandit purus ultricies sit amet. Quisque sed orci mi. Fusce eu vulputate mi. Nam pretium ultricies aliquet.
</p>

<p id="preserve-wrap">
  Lorem ipsum dolor sit amet, consectetur adipiscing elit.         Cras accumsan tortor vitae aliquet imperdiet.       Sed a convallis sapien.
</p>

<p id="left-right">
I prefer left to right
</p>

<p id="right-left">
I prefer right to left
</p>

<table>
  <caption>Auto Table</caption>
  <tr>
    <td>Random</td>
    <td>More Random Words</td>
    <td>Even More Random Words</td>
  </tr>
</table>

<table id="fixed">
  <caption>Auto Table</caption>
  <tr>
    <td>Random</td>
    <td>More Random Words</td>
    <td>Even More Random Words</td>
  </tr>
</table>

<table id="spacing">
  <caption>Auto Table</caption>
  <tr>
    <td>Random</td>
    <td></td>
    <td>Even More Random Words</td>
  </tr>
</table>

</body>

</html>
```

```html
/* stylesheet6.css */
#underline{
  text-decoration: underline;
}

#line-through{
  text-decoration: line-through;
}

#overline{
  text-decoration: overline;
}

#lowercase{
  text-transform: lowercase;
}

#uppercase{
  text-transform: uppercase;
}

#capitalize{
  text-transform: capitalize;
}

/* Whitespace is preserved and wraps on line breaks */
#whitespace-pre{
  white-space: pre;
}

/* White space collapses, but only wraps on line break */
#never-wrap{
  white-space: nowrap;
}

#preserve-wrap{
  white-space: pre-wrap;
}

#left-right{
  direction: ltr;
}

#right-left{
  direction: rtl;
}

table{
  width: 600px;
  caption-side: top;
  text-align: center;
  border: 2px solid black;
}

td {
  border: 2px solid black;
  padding: 10px;
}

table#fixed{
  table-layout: fixed;
}

/*
border spacing applies 10px space on left / right and
15px on top / bottom

empty-cells will hide the border if the column doesn't
contain anything

border-collapse deletes the table border
*/
table#spacing{
  border-spacing: 10px 15px;
  empty-cells: hide;
  border-collapse: collapse;
}
```

### Example 7
```html
<!DOCTYPE HTML>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>CSS Tutorial</title>

  <link rel="stylesheet" type="text/css" href="stylesheet7.css">
</head>

<body>

<div id="div1"></div>
<div id="div2"></div>
<div id="div3"></div>

<div id="div4">CSS3</div>
<div id="div5">Is Awesome</div>

<div id="div6"></div>
<div id="div7"></div>
<div id="div8"></div>
<div id="div9"></div>
<div id="div10"></div>

<div id="div11">Rounded</div>
<div id="div12">Circle</div>
<div id="div13">Tab</div>

<div id="div14"></div>
<div id="div15"></div>


</body>
</html>
```

```html
/* stylesheet7.css */
div#div1, div#div2, div#div3{
  height: 100px;
  width: 100px;
  margin: 10px;
  border: 3px solid black;
}

#div1{
  /* horz-pos, vert-pos, blur amount, size of shadow, color */
  box-shadow: 5px 5px 5px 2px gray;
}

#div2{
  /* inset creates an inner shadow */
  box-shadow: 5px 5px 5px 2px gray inset;
}

#div3{
  /* Create a glow */
  box-shadow: 0px 0px 10px 5px yellow;
}

div#div4, div#div5{
  height: 30px;
  width: 30px;
  font: 20pt Arial;
}

#div4{
  /* horz-pos, vert-pos, blur amount, color */
  text-shadow: 2px 2px 3px gray;
}

#div5{
  /* Create a glow */
  text-shadow: 0 0 3px yellow;
}

div#div6, div#div7, div#div8, div#div9, div#div10{
  height: 100px;
  width: 100px;
  float: left;
  margin-top: 40px;
}

/* Define background by RGB */
#div6{
  background: rgb(0, 0, 205);
}

/* You can change opacity */
#div7{
  background: rgb(0, 0, 205);
  opacity: .5;
}

/* You can also change the alpha */
#div8{
  background: rgba(0, 0, 205, .5);
}

/* Hue, Saturation, Lightness */
#div9{
  background: hsl(205, 100%, 50%);
}

/* You also can change the alpha */
#div10{
  background: hsla(205, 100%, 50%, .5);
}

div#div11, div#div12, div#div13{
  height: 100px;
  width: 100px;
  float: left;
  margin: 20px;
  border: 3px solid black;
  text-align: center;
}

/* Round the corners */
#div11{
  border-radius: 30px;
}

/* Round them until a circle appears */
#div12{
  border-radius: 50px;
}

/* Round 1 Corner */
#div13{
  border-top-right-radius: 30px;
}

/* You can change the background image */
div#div14, div#div15{
  height: 92px;
  width: 80px;
  margin-top: 200px;
  float: left;
  background-image: url(nttlogo.png);
}

#div14{
  background-size: 200% 200%;
}

#div15{
  background-size: 50% 50%;
}
```

### Example 8
```html
<!DOCTYPE HTML>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>CSS Tutorial</title>

  <link rel="stylesheet" type="text/css" href="stylesheet8.css">
</head>

<body>

<div id="div1"></div>
<div id="div2"></div>
<div id="div3"></div>
<div id="div4"></div>

<div id="div5"></div>
<div id="div6"></div>
<div id="div7"></div>
<div id="div8"></div>

<div id="div9"></div>

<div id="div10"></div>

</body>
</html>
```

```html
/* stylesheet8.css */
/*
Browsers add prefixes to property names before they are stable
moz : Firefox, ms : IE, o : Opera and webkit : Chrome / Safari
*/

div#div1, div#div2, div#div3, div#div4{
  height: 100px;
  width: 100px;
  margin: 10px;
  border: 3px solid black;
}

/* Direction or left / right / top / bottom, color stops*/
#div1{
  background: -moz-linear-gradient(45deg, red, yellow, blue);
  background: -webkit-linear-gradient(45deg, red, yellow, blue);
  background: -ms-linear-gradient(45deg, red, yellow, blue);
  background: -o-linear-gradient(45deg, red, yellow, blue);
  background: linear-gradient(45deg, red, yellow, blue);
}

/* You can also define how much of the gradient each color takes up */
#div2{
  background: -moz-linear-gradient(left top, red 20%, yellow 30%, blue 60%);
  background: -webkit-linear-gradient(left top, red 20%, yellow 30%, blue 60%);
  background: -ms-linear-gradient(left top, red, yellow, blue);
  background: -o-linear-gradient(left top, red, yellow, blue);
  background: linear-gradient(left top, red, yellow, blue);
}

/* You can have the gradient repeat */
#div3{
  background: -moz-repeating-linear-gradient(left top, red 20%, yellow 30%, blue 60%);
  background: -webkit-repeating-linear-gradient(left top, red 20%, yellow 30%, blue 60%);
  background: -ms-repeating-linear-gradient(left top, red 20%, yellow 30%, blue 60%);
  background: -o-repeating-linear-gradient(left top, red 20%, yellow 30%, blue 60%);
  background: repeating-linear-gradient(left top, red 20%, yellow 30%, blue 60%);
}

/* Radial gradients are another option */
#div4{
  background: -moz-radial-gradient(red 20%, yellow 30%, blue 60%);
  background: -webkit-radial-gradient(red 20%, yellow 30%, blue 60%);
  background: -ms-radial-gradient(red 20%, yellow 30%, blue 60%);
  background: -o-radial-gradient(red 20%, yellow 30%, blue 60%);
  background: radial-gradient(red 20%, yellow 30%, blue 60%);
}

div#div5, div#div6, div#div7, div#div8{
  height: 100px;
  width: 100px;
  margin: 20px;
  border: 3px solid black;
}

/* Make an element scale when interacted with */
#div5:hover{
  -webkit-transform: scale(2);
  -moz-transform: scale(2);
  -o-transform: scale(2);
  -ms-transform: scale(2);
}

/* Make an element rotate when interacted with
Also have rotateX(deg) & rotateY(deg)*/
#div6:hover{
  -webkit-transform: rotate(45deg);
  -moz-transform: rotate(45deg);
  -o-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
}

/* Make an element skew Also have skewX(deg) & skewY(deg) */
#div7:hover{
  -webkit-transform: skew(20deg, 10deg);
  -moz-transform: skew(20deg, 10deg);
  -o-transform: skew(20deg, 10deg);
  -ms-transform: skew(20deg, 10deg);
}

/* Make an element move Also have translateX(deg) & translateY(deg) */
#div8:hover{
  -webkit-transform: translate(20px, 20px);
  -moz-transform: translate(20px, 20px);
  -o-transform: translate(20px, 20px);
  -ms-transform: translate(20px, 20px);
}

div#div9{
  height: 100px;
  width: 100px;
  margin: 20px;
  border: 3px solid black;
  background: orange;
}

/* You can define a change to an element and have it slowly take effect
Transition
ease : Increasing change
ease-in : Increase at start
ease-out : Decrease at end
ease-in-out : Increase at start, decrease at end
linear : Constant change
*/

#div9:hover{
  background: red;
  border-radius: 20px;
  -webkit-transition: 2s ease-in-out;
  -ms-transition: 2s ease-in-out;
  -o-transition: 2s ease-in-out;
  -moz-transition: 2s ease-in-out;
  transition: 2s ease-in-out;
}

div#div10, div#div11, div#div12{
  height: 100px;
  width: 100px;
  margin: 20px;
  border: 3px solid black;
  background: red;
}

/* Animate by changing attributes over time
Define name, duration, delay and the changes to make */

div#div10{
    width: 100px;
    height: 100px;
    background-color: red;
    position: relative;
    -webkit-animation-name: example;
    -webkit-animation-duration: 4s;
    -webkit-animation-delay: 2s;
    animation-name: example;
    animation-duration: 4s;
    animation-delay: 2s;
}

@-webkit-keyframes example {
    0%   {background-color:red; left:0px; top:0px;}
    25%  {background-color:yellow; left:200px; top:0px;}
    50%  {background-color:blue; left:200px; top:-200px;}
    75%  {background-color:green; left:0px; top:-200px;}
    100% {background-color:red; left:0px; top:0px;}
}

/* Standard syntax */
@keyframes example {
    0%   {background-color:red; left:0px; top:0px;}
    25%  {background-color:yellow; left:200px; top:0px;}
    50%  {background-color:blue; left:200px; top:-200px;}
    75%  {background-color:green; left:0px; top:-200px;}
    100% {background-color:red; left:0px; top:0px;}
```

### Example 9
```html
<!DOCTYPE HTML>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>CSS Tutorial</title>

  <!-- You can provide many alternative style sheets -->

  <link rel="stylesheet" type="text/css" media="screen"
  href="stylesheet9.css" title="Default Style">

  <link rel="stylesheet" type="text/css" media="screen"
  href="large-text.css" title="Large Text Style">

  <link rel="stylesheet" type="text/css" media="handheld"
  href="handheld.css" title="Mobile Device Style">

  <link rel="stylesheet" type="text/css" media="print"
  href="print.css" title="Printer Style">

</head>

<body>

<div id="lorem">
  <h2>Lorem Ipsum</h2>
  <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do
  eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut
  enim ad minim veniam, quis nostrud exercitation ullamco laboris
  nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in
  reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla
  Duis aute irure dolor in reprehenderit in voluptate velit esse cillum
  dolore eu fugiat nulla</p>
</div>

<div id="container">
  <div id="div1">
    <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit</p>
  </div>
  <div id="div2">
    <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit</p>
  </div>
  <div id="div3">
    <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit</p>
  </div>
</div>

</body>
</html>
```

```html
/* stylesheet9.css */
/* You can create column boxes that fits content into boxes */

div#lorem{
  height: 150px;
  width: 450px;
  border: 2px solid black;
  overflow: hidden;
}

div#lorem{
  -webkit-columns: 4;
  -ms-columns: 4;
  -o-columns: 4;
  -moz-columns: 4;
  columns: 4;
}

div#lorem{
  -webkit-column-rule: 2px dashed black;
  -ms-columns: 2px dashed black;
  -o-columns: 2px dashed black;
  -moz-columns: 2px dashed black;
  columns: 2px dashed black;
}

/* You can define the amount of space taken up by elements in a containing
element */
#container{
  height: 150px;
  width: 450px;
  border: 2px solid black;
}

#container{
  display: -webkit-box;
}

#div1{
  -webkit-box-flex : 2;
}

#div2{
  -webkit-box-flex : 2;
}

#div3{
  -webkit-box-flex : 3;
}
```

### References
- [CSS3 Tutorial](https://www.youtube.com/watch?v=CUxH_rWSI1k&list=PLGLfVvz_LVvSX7fVd4OUFp_ODd86H0ZIY&index=14)
