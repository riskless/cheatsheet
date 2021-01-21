### Example 1
```html
<!DOCTYPE HTML>
<!-- Tells the browser to render the page using the HTML5 specification --> 

<!--
1. HyperText Markup Language : Uses tags to tell the browser how to layout the web page 
2. Uniform Resource Locator : 
	a. http:// is used if data is transfered using the HyperText Transfer Protocol (Common standard of communication between web servers)
	b. The domain is the unique name for the server / servers hosting the data
	c. The URL ends with the path to the data to be served
	d. http://newthinktank.com/html5.html
3. You can validate your HTML here http://validator.w3.org/check
-->

<!-- Defines the documents primary language as English -->
<html lang="en">

	<!-- Contains data that describes the document -->
	<head>
	
	<!-- Defines the documents encoding character set (Multi-lingual Universal Transformation Format) -->
	<meta charset="UTF-8">
	
	<!-- Redriects the browser to another page after 3 seconds
	<meta http-equiv="refresh" content= "3; url='redirect.html'"/> -->
	
	<!-- Description for your page. Shouldn't be longer than 200 characters -->
	<meta name="description"
	content="Learn everything you want to know about HTML5 in this video tutorial. Tags, content sections, embedding media, forms, canvas and more will be covered."/>
	
	<!-- Keywords are used to define your content for search engines. Shouldn't be longer than 1,000 characters -->
	<meta name="keywords"
	content="html5 canvas,html5 tutorial,html5 tutorial, html5 doctype, html5 tags,html5 video,learn html5"/>
	
	<!-- Defines how search engines should index your content.
	index / noindex : indicates if search engines should index the page
	follow / nofollow : indicates if hyperlinks should be followed and indexed
	archive / noarchive : indicates whether the page should be archived
	-->
	<meta name="robots" content="index,follow"/> 
	
	<!-- Defines the location of an outside JavaScript file -->
	<script src="randomcolorchange.js"> </script>
	
	<!-- Must come before any other link tags -->
	<!-- Defines a default location for all links on a page for browsers -->
	<base href="http://localhost/html5/"/>
	
	<!-- Defines the location of a style sheet -->
	<!-- Style sheets have replaced HTML styling features from the past -->
	<link rel="stylesheet" href="styles.css"/>
	
	<!-- References a icon (16 x 16) to associate with your page. Save it in the root directory for IE -->
	<link rel="icon" href="favicon.ico"/>
	
	<!-- You must have a title for your page that should be about 16 characters long -->
	<title>HTML5 Tutorial</title>
	
	</head>
	
	<!-- Contains data to show in the browser -->
	<!-- onload can call a script or open a dialog box -->
	<!-- onunload is not supported in browsers other than IE -->
	<body onload="alert('Hello')" onunload="alert('Bye Bye')">
	
	<div id="wrapper">
	
	<!-- Can jump with in the page by referencing the name -->
	<a name="pageTop">Top of page</a>
	
	<!-- Links to the bottom by referencing the id -->
	<a href="html5.html#pageBottom">Bottom of page</a>
	
	<!-- Message that is displayed if the browser has JavaScript disabled -->
	<noscript>Please Enable JavaScript</noscript>
	
	<!-- Increases the size of the text it contains h1 - h6
	They are used to define the importance of elements in sequence -->
	<h1>Change with a Click</h1>
	
	<!-- The <p> will add 2 line breaks after the </p> and <br> adds one -->
	<p>
	Two hunters are out in the woods when one of them collapses. He doesn’t seem to be breathing and his eyes are glazed. The other guy whips out his phone and calls the emergency services. He gasps, "My friend is dead! What can I do?". The operator says "Calm down. I can help. First, let’s make sure he’s dead." <br>There is a silence, then a shot is heard. Back on the phone, the guy says "OK, now what?"
	</p>
	
	<!-- Draws a line -->
	<hr>
	
	<!-- <blockquote> indents the surrounding text and cite defines where the quote came from -->
	<blockquote cite="http://epicquotes.org/view.php?id=151">
	Never, under any circumstances, take a sleeping pill and a laxative on the same night.
	</blockquote>
	
	<!-- <q> surrounds text with quotes -->
	<q cite="http://www.brainyquote.com/quotes/quotes/h/hennyyoung128883.html">If at first you don’t succeed ... so much for skydiving.</q><br><br>
	
	<!-- <pre> preserves white space, but tabs can get messed up -->
	<pre>
                  _          _
                 (.&lt;        &gt;.)
                \(_)________( \
                 (___________)\\
                    (     )     \
                     |   |
                     |   |
                     |   |
                    _|   |_
                   (_______)
	</pre>
	
	<!-- Use pre and code to surround code -->
	<!-- var is used to emphasive important variables -->
	<pre>
	<code>
	function init(){
		var <var>h1tags</var> = document.getElementsByTagName("h1");
		<var>h1tags[0]</var>.onclick = changeColor;
	}
	</code>
	</pre>
	
	<!-- Adding images : define width, height and alt -->
	<img id="danceImg" src="http://www.gifbin.com/bin/102009/1255606809_acrobatic_dance_fail.gif" width="237" height="178" alt="Dance Fail"><br>
	
	<!-- 4 ways to emphasize text without a line break -->
	<b>Bold : Bold Font</b><br>
	<strong>Strong : Bold Font</strong><br>
	<em>Emphasis : Italic Font</em><br>
	<i>Italic : Italic Font</i><br>
	
	<!-- 3 ways to format text for special purposes -->
	<small>Small : Short Comment</small><br>
	<ins>Insert : Added to Document</ins><br>
	<del>Delete : Strike</del><br>
	<samp>Sample Programming Output</samp><br><br>
	
	<!-- Advisory tags -->
	<!-- dfn adds emphasis to a word without title and a popup with title -->
	<!-- kbd is used to emphasis input expected from a user -->
	This is an abbreviation <abbr title="abbreviation">abbrv.</abbr><br>
	<dfn>Definition: </dfn>the formal statement of the meaning or significance of a word<br>
	<dfn title="the formal statement of the meaning or significance of a word">Definition</dfn><br>
	<kbd>Enter something here</kbd>
	
	<!-- Character Entities : http://dev.w3.org/html5/html-author/charref -->
	&cent;
	&pound;
	&yen;
	&copy;
	&reg;
	&deg;
	&frac14;
	&frac12;
	&frac34;
	&sup2;<br>

	<!-- Superscript / Subscript -->
	4<sup>2</sup><br>
	
	CO<sub>2</sub><br>
	
	<!-- You can define the direction of text with rtl or ltr -->
	<bdo dir="rtl">أين هو مطعم</bdo><br>
	
	<!-- You can provide ruby annotation for pronunciation -->
	<ruby>飯店<rt>hanten</rt></ruby><br>
	
	<!-- HYPERLINKS -->
	<!-- Contains the referenced link with an optional title -->
	<!-- A hyperlink has 3 interactive states hover, active, visited -->
	<a href="http://youtube.com/" title="YouTube">YouTube</a>
	
	<!-- You can link to parts of the same web page -->
	<a href="html5.html#pageTop">Top of Page</a>
	<a id="pageBottom">Bottom of page</a><br>
	
	<!-- You can open the mail software to send a message -->
	<a href="mailto:derekbanas@verizon.net">Send an Email</a>
	
	<!-- You can also dynamically call for a JavaScript function to execute -->
	<a href="javascript:toggleImg()">Toggle Image</a><br>
	
	<!-- You can define hot spot links in images with image maps. 
	Import your image into Gimp and Filters → Web → ImageMap -->
	<img src="http://localhost/html5/imagemap.png" alt="imagemap example" width="600" height="125" usemap="#map" />

	<map name="map">
	<area shape="circle" coords="57,62,39" alt="Yahoo" title="Go to Yahoo" href="http://yahoo.com" />
	<area shape="poly" coords="192,20,166,45,232,105,301,42,272,20,192,19,195,20" alt="Diamonds" title="Go to Diamonds" href="http://diamonds.com" />
	<area shape="rect" coords="332,8,595,115" alt="Google" title="Go to Google" href="http://google.com" />
	</map>
	
	<!-- You can embed numerous external file formats using the object tag -->
	
	<!-- Embed a PDF -->
	<object data="SmallBusinessPresentation.pdf" type="application/pdf" 
		width="750" height="400">
	</object>
	
	<!-- Embed a HTML file -->
	<object data="html52.html" type="text/html" 
		width="750" height="400">
	</object>
	
	<!-- Embed a WAV file -->
	<object data="http://www.wavsource.com/snds_2014-11-23_3967152387108926/animals/bird.wav" type="audio/x-wav">
	
		<!-- Used to pass parameters to an embedded object 
		attributes consist of a name and value -->
		<param name="autoplay" value="true">
		<param name="controller" value="true">
	
	</object>
	
	<!-- Other Formats 
	application/msword : MS Doc Files
	application/x-java-applet : Java Applet
	audio/mpeg : MP3s
	image/png : PNGs
	image/jpeg : JPEGs
	image/gif : GIFS
	image/svg+xml : SVG Vector images
	text/plain : Text
	video/mp4 : MP4s
	video/x-msvideo : AVIs
	video/x-msv-wmv : WMVs
	video/quicktime : MOVs
	-->
	
	<!-- You can embed externale resources in an iframe -->
	<iframe width="560" height="315" src="http://www.youtube.com/embed/Rub-JsjMhWY" allowfullscreen=""></iframe>
	
	<!-- Interactive files like Flash files can be embedded with embed
	You can define the file type, plugin and set other params -->
	<embed src="http://www.itma.vt.edu/tech/flash.swf" pluginspage="http://www.macromedia.com/shockwave/download/" type="application/x-shockwave-flash" width="300" height="120" loop="false" quality="high"> 

	<!-- You can embed MP3 and Wav (Not IE) files using audio 
	mp3 uses audio/mpeg -->
	
	<audio controls autoplay loop preload>
		<source src="comingtotake.wav" type="audio/wav">
		Browser doesn't support the audio tag
	</audio>
		
	<!-- You can embed MP4s with video -->
	<video controls autoplay loop preload>
		<source src="http://techslides.com/demos/sample-videos/small.mp4" type="video/mp4">
	</video>
	
	<br><br><br><br><br><br>
	</div>
	</body>

</html>
```

```html
<!-- redirect.html -->
<!DOCTYPE HTML>

<html lang="en">
	<head>
	<meta charset="UTF-8">
	<title>HTML5 Tutorial</title>
	</head>
	
	<body>
		<h3>Redirected Here</h3>
	</body>
</html>
```

```js
// randomcolorchange.js
function init(){

	// Get a reference to every h3 tag
	var h1tags = document.getElementsByTagName("h1");
	
	// When the first h1 tag is clicked call the function changeColor
	h1tags[0].onclick = changeColor;

}

function changeColor(){

	// this refers to the item clicked and changes the content in the tag to Click Again
	this.innerHTML = "Click Again";
	
	// Generates a random color hex code
	var randomcolor = '#'+Math.floor(Math.random()*16777215).toString(16);
	
	// Change the color of the element to the random color
	this.style.color = randomcolor;

}

function toggleImg(){

	var img = document.getElementById("danceImg");
	var isImgVisible = img.style.visibility !== "visible";
	img.style.visibility = isImgVisible ? "visible" : "hidden";

}

// When the script runs execute the function init()
onload = init;
```

```html
/* styles.css */
/* Change the color of all text in h1 tags */
h1 {
	color : purple;
}

/* Change the color of links in the document based on events */
a:hover { color: blue; }
a:active { color: red; }
a:visited { color: #989898; }

body {
	background: #5D8AA8;
}

div#wrapper{
	margin:auto;
	background:white;
	width:800px;
	height:2000px;
}

header { 
	display:block;
	/* Padding Top, Right, Bottom, Left */
	padding: 0px 0px 70px 0px;
}

#websiteLogo{
	float:left;
}

/* Treat the nav tag as a block element */
#horzNav { 
	display:block;
	/* Padding Top, Right, Bottom, Left */
	padding: 10px 20px 10px 0px;
	background: black;
}

/* Remove underlines from links in the nav element */
#horzNav a {
	text-decoration: none;
	padding: 0px 0px 0px 20px;
	color: white;
}

#vertNav{
	float:left;
	background: purple;
	/* Padding Top, Right, Bottom, Left */
	padding: 0px 30px 0px 20px;
}

#vertNav a {
	text-decoration: none;
	/* Links don't except padding by default so we must switch them to block */
	display: inline-block;
	/* Padding Top, Right, Bottom, Left */
	padding: 0px 0px 20px 0px;
	color:white;
}

main {
	/* Padding Top, Right, Bottom, Left */
	padding: 10px 20px 20px 20px;
}

aside {
	background:#EFDECD;
}

footer{
	background: black;
	color: white;
	padding: 10px 10px 10px 10px; 
}

/* There are 4 types of list bullets */
#uoListTypes{
	/* list-style-type:circle; */
	/* list-style-type:disc; */
	/* list-style-type:square; */
	list-style-image: url(favicon.ico);
}

/* Ordered list types have many options
decimal, decimal-leading-zero, lower-roman, upper-roman, lower-latin, upper-latin, lower-alpha, upper-alpha, lower-greek, armenian, georgian */
#oListTypes{
	list-style-type:upper-roman;
}

#oListIndent{
	list-style-type:lower-roman;
}

#tableData {
	width:600px;
	border: 3px solid black;
}

#tableData td, th{
	border: 1px solid black;
}

#tableData colgroup.BBPName { background:#C9FFE5; }
```

### Example 2
```html
<!-- html52.html -->
<!DOCTYPE HTML>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<link rel="stylesheet" href="styles.css">
	<title>HTML5 Tutorial</title>
</head>

<body>
	<!-- A div wrapper is often used to define the size and position of the page divs and spans should be used only for styling and not structural reasons -->
	<div id="wrapper">

	<!-- The header normally contains the logo, the main header, search form -->
	<header>
		
		<a href="http://localhost/html5/html52.html">
		<img id="websiteLogo" alt="New Think Tank Logo" src="http://localhost/html5/NTTLogo.gif"/>
		</a>
		
		<h1>New Think Tank</h1>
	
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
		<h2>Fun Fact</h2>
		
			<!-- An article is a self contained block of content that is complete on its own with a heading and content -->
			<article>
			
				<h3>Tomato Fact</h3>
				<p>Tomatoes are rich in lycopene, an antioxidant that is good for the heart and effective against certain cancers. Cooked tomatoes are actually better for you than raw ones, as more beneficial chemicals are released. Tomatoes are also packed with vitamins A and C, calcium, potassium.</p>
				
				<!-- Content in an aside should be stand alone content related to the article -->
				<aside>I don't care what anybody says: Nothing is better than a tomato you grow. - Tom Vilsack</aside>
			
			</article>
	
	
	</section>
	
	<section>

		<h2>Fun Fact</h2>

			<article>
			
				<h3>Types of Lists</h3>
				
					<!-- Unordered list -->
					<ul id="uoListTypes">
						<li>Unordered</li>
						<li>Ordered</li>
						<li>Definition</li>
					</ul>
					
					<!-- Ordered list -->
					<ol id="oListTypes">
						<li>Harry Potter
							<ol id="oListIndent">
								<li>and the Sorcerer's Stone</li>
								<li>and the Half-Blood Prince</li>
								<li>and the Chamber of Secrets</li>
								<li>Prisoner of Azkaban</li>
								<li>and the Goblet of Fire</li>
								<li>and the Order of the Phoenix</li>
								<li>and the Deathly Hallows</li>
							</ol>
							
							<!-- To use lists in lists put the new ol between the top li -->
							</li>
						<li>The Very Hungry Caterpillar</li>
					</ol>
					
					<!-- Definition list : Surrounded by <dl>, list item surrounded by <dt>, <dfn> defines it is a word being defined and <dd> surrounds the definition -->
					<dl>
					<dt><dfn>Definition</dfn></dt>
					<dd>the act of making definite, distinct, or clear</dd>
					</dl>
				
				<aside>Mostly I make lists for projects. This can be daunting. Breaking something big into its constituent parts will help you organize your thoughts, but it can also force you to confront the depth of your ignorance and the hugeness of the task. That's OK. The project may be the lion, but the list is your whip. - Adam Savage</aside>
			
			</article>
	
	
	</section>
	
	<section>

		<h2>Fun Fact</h2>

			<article>
			
				<h3>Tables</h3>
				
				<!-- Tables are surrounded with table tags -->
				<table id="tableData">
				
				<!-- caption can be used to define a table title -->
				<caption>Baseball</caption>
				
					<!-- thead shows at the top of the table and must come before tbody -->
					<thead>
						<tr>
							<td colspan="4">Best Baseball Players</td>
						</tr>
					</thead>
					
					<!-- thead shows at the bottom of the table and must come before tbody -->
					
					<tfoot>
						<!-- You can define how many rows are taken up with colspan and you can take up multiple rows with rowspan -->
						<!-- tr defines the row and td each cell -->
						<tr><td colspan="4">* Hall of Fame</td></tr>
					</tfoot>
					
					<!-- The main data for the table goes between tbody if you use thead or tfoot -->
					<tbody>
						<tr>
							<td></td>
							<td>Average</td>
							<td>RBIs</td>
							<td>Homeruns</td>
						</tr>
						
						<tr>
							<td>Hank Aaron*</td>
							<td>.305</td>
							<td>2297</td>
							<td>755</td>
						</tr>
						
						<tr>
							<td>Babe Ruth*</td>
							<td>.342</td>
							<td>2214</td>
							<td>714</td>
						</tr>
						
						<tr>
							<td>Barry Bonds</td>
							<td>.298</td>
							<td>1996</td>
							<td>762</td>
						</tr>
					</tbody>
					
				</table>
			
			</article>
	
	
	</section>
	
	</main>
	
	<footer>
		<!-- address normally contains the authers contact info -->
		<address>derekbanas@verizon.net</address>
		<!-- copyright is normally placed in a small tag -->
		<small>copyright 2014</small>
	</footer>
	
	<!-- Vertical Menu
	<nav id="vertNav">
	
		<p>
			<a href="google.com">Google</a><br>
			<a href="youtube.com">YouTube</a><br>
			<a href="yahoo.com">Yahoo</a><br>
		</p>
	
	</nav>
	-->

	</div>
</body>

</html>
```

### Example 3
```html
<!DOCTYPE HTML>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<link rel="stylesheet" href="styles.css">
	<title>HTML5 Tutorial</title>
</head>

<body>
<div id="wrapper">

	<!-- To send data to the server you need to surround your input with form tags. 
	The GET method embeds the data in the URL
	The POST method enclodes the data and transmits it in the HTTP message -->
	<form method="GET" action="phpscript.php">
	
		<!-- You can pass text where the name will be used as the key to get the passed value. You can mark as readonly and disabled -->
		Your Name : <input type="text" name="name" size="50" maxlength="50"><br>
		
		<!-- You can have data hidden with password -->
		Your Password : <input type="password" name="password"><br>
		
		<!-- A textarea is used for multiple lines of input. It can be marked as readonly="true" -->
		Your History :<br>
		<textarea name="history" rows="20" cols="50" >Just some random words
		</textarea><br>
		
		<!-- You can visually surround like data with fieldset -->
		<fieldset>
		
			<!-- Set the group name with legend -->
			<legend>Hobbies</legend>
			
			<!-- Create a checkbox -->
			Programming<input type="checkbox" name="cbprogramming" value="Programming">
			Running<input type="checkbox" name="cbrunning" value="Running">
		</fieldset>
		
		<fieldset>
			<legend>Top Skill</legend>
			
			<!-- Create a radio button to allow only 1 option -->
			Programming<input type="radio" name="skill" value="Programming" checked>
			Running<input type="radio" name="skill" value="Running">
		</fieldset>
		
		<!-- Put all the options between select. You could show all options with  size="3" and selected can define the default value-->
		Favorite Number : 
		<select name="favNum">
		
			<option value="PI">Pi (3.14...)</option>
			<option value="EulersNum">Euler’s Number (2.718...)</option>
			<option value="GoldenRatio" selected>Golden Ratio</option>
		
		</select><br>
		
		<!-- You can group options in a select with optgroup -->
		Favorite Hero : 
		<select name="favHero">
  			<optgroup label="Marvel">
    			<option value="spiderman">Spiderman</option>
    			<option value="wolverine">Wolverine</option>
  			</optgroup>
  			<optgroup label="DC">
    			<option value="flash">Flash</option>
    			<option value="batman">Batman</option>
  			</optgroup>
		</select><br>
		
		<!-- You can submit hidden data such as ip address, browser, etc with hidden -->
		<input type="hidden" name="browser" id="browser"><br>
		
		<!-- You can upload files as well, but you must use the POST method -->
		<input type="file" name="Upload">
		
		<!-- You could use an image for submit and then catch the event with JavaScript -->
		<input type="image" alt="Logo" src="http://localhost/html5/NTTLogo.gif">
		
		<!-- Reset deletes all entered data -->
		<input type="reset" value="Reset">
		
		<!-- When submit is clicked the data is passed to be processed on the server -->
		<input type="submit" name="submit" value="Submit">
	
	</form>

</div>
</body>

</html>
```

```php
<?php
// phpscript.php
$usersName = $_GET['name'];
$usersPassword = $_GET['password'];

echo 'Hello ' . $usersName . ' </br>';
echo 'That is a great password ' . $usersPassword . ' </br>';

?>
```

### Example 4
```html
<!DOCTYPE HTML>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<link rel="stylesheet" href="styles.css">
	<title>HTML5 Tutorial</title>
	<script src="excanvas.js"></script>
</head>

<body>
<div id="wrapper">

	<!-- You can paint shapes and text on a canvas using JavaScript -->
	<canvas id="canvas" width="800" height="400">
	</canvas>

</div>
</body>
</html>
```

```js
// excanvas.js
function init(){

	var canvas = document.getElementById("canvas");
	if(canvas.getContext){
	
		// The ctx object provides functions to paint on the canvas 
		var ctx = canvas.getContext("2d");
		
		// Pick the color to use
		ctx.fillStyle = "#FAEBD7";
		
		// Draw a rectangle starting in the upper left corner of the canvas
		ctx.fillRect(0, 0, canvas.width, canvas.height);
		
		// Change the draw color
		ctx.fillStyle = "#AF002A";
		
		// Draw a rectangle starting 100 down and 100 in with width
		// and height of 50px
		ctx.fillRect(100, 100, 50, 50);
		
		// You can define a stroke width and color and draw it without a fill
		ctx.lineWidth = 5;
		ctx.strokeStyle = "#A4C639";
		ctx.strokeRect(100, 100, 50, 50);
		
		ctx.fillStyle = "#00308F";
		
		// Says that we want to start drawing a path
		ctx.beginPath();
		
		// Draw an arc / circle (x, y, radius, startAngle, endAngle, direction)
		// Start and end angles are set as radians, but a start of 0 and an
		// end of Math.PI*2 gives you a circle
		// true is counterclockwise and false is clockwise
		ctx.arc(200, 200, 50, 0, Math.PI*2, true);
		
		// Paints the defined shape on the canvas
		ctx.fill();
		
		ctx.fillStyle = "#3B444B";
		ctx.beginPath();
		
		// To draw polygons you move to a starting x y and then use lineTo
		// To draw the other lines followed by closePath
		ctx.moveTo(350, 200);
		ctx.lineTo(400, 50);
		ctx.lineTo(450, 200);
		ctx.closePath();
		ctx.fill();
		
		// You can apply strokes to polygons and circles as well
		ctx.strokeStyle = "#A4C639";
		ctx.beginPath();
		ctx.moveTo(350, 200);
		ctx.lineTo(400, 50);
		ctx.lineTo(450, 200);
		ctx.closePath();
		ctx.stroke();
		
		// You can create gradient fills (top / left to bottom / right)
		var linGrad = ctx.createLinearGradient(400, 100, 500, 500);
		
		// Add the colors and their position in the gradient
		linGrad.addColorStop(0, "#8DB600");
		linGrad.addColorStop(0.5, "#9966CC");
		linGrad.addColorStop(1, "#7C0A02");
		
		ctx.fillStyle = linGrad;
		
		// You could also define the color RGB and opacity for a fill
		// ctx.fillStyle = "rgba(10, 150, 255, 0.5)";
		
		ctx.fillRect(400, 100, 100, 100);
		
		// Create a radial gradient (starting circle x, y, radius - 
		// ending circle x, y, radius )
		var radGrad = ctx.createRadialGradient(275, 250, 5, 290, 260, 100);
		radGrad.addColorStop(0,"red");
		radGrad.addColorStop(1,"white");
		ctx.fillStyle=radGrad;
		ctx.arc(250, 250, 50, 0, Math.PI*2, true);
		ctx.fill();
		
		// Clear screen by filling with background color
		ctx.fillStyle = "#FAEBD7";
		ctx.fillRect(0, 0, canvas.width, canvas.height);
		
		// You can also write text on canvas
		ctx.font = "bold 40px Arial";
		ctx.fillStyle = "#8DB600";
		ctx.fillText("Hello", 100, 100);
		
		// We can stroke the text as well
		ctx.strokeStyle = "black";
		ctx.lineWidth = 1;		
		ctx.strokeText("Hello", 100, 100);
		
		// We can also paint drop shadows
		ctx.shadowOffsetX = 2;
		ctx.shadowOffsetY = 2;
		ctx.shadowBlur = 3;
		ctx.shadowColor = "black";
		ctx.fillText("Hello", 100, 100);
		
		// Turn off shadows
		ctx.shadowColor = "rgba(0,0,0,0)";
		ctx.shadowBlur = 0;
		ctx.shadowOffsetX = 0;
		ctx.shadowOffsetY = 0;
		
		// We will now draw lines with different end points
		ctx.lineWidth = 20;
		ctx.strokeStyle = "purple";
		ctx.beginPath();
		ctx.moveTo(200, 150);
		ctx.lineCap = "butt";
		ctx.lineTo(200, 250);
		ctx.stroke();
		ctx.lineCap = "round";
		ctx.stroke();
		ctx.lineCap = "square";
		ctx.stroke();
		
		// Drawing curves
		ctx.beginPath();
		
		// Draw a curve from 0 to 180 degrees (3.14 Radians)
		ctx.arc(270,270, 50, 0, 3.14, true);
		ctx.stroke();
		
		ctx.beginPath();
		ctx.arc(320,270, 100, 0, 3.14, false);
		ctx.stroke();
		
		// Create a simple curve by defining XY for the start and end
		ctx.strokeStyle="red";
		ctx.beginPath();
		ctx.moveTo(300, 150);
		ctx.quadraticCurveTo(350, 250, 450, 250);
		ctx.stroke();
		
		// X of the 1st Bézier, Y of the 1st Bézier, X of the 2nd Bézier, 
		// Y of the 2nd, X of the ending Bézier, Y of the ending Bézier 
		ctx.strokeStyle = "blue";
		ctx.beginPath();
		ctx.moveTo(450, 250);
		ctx.bezierCurveTo(550, 250, 450, 100, 550, 100);
		ctx.stroke();
		
		// Clear screen by filling with background color
		ctx.fillStyle = "#FAEBD7";
		ctx.fillRect(0, 0, canvas.width, canvas.height);
		
		ctx.fillStyle = "purple";
		ctx.fillRect(200, 200, 100, 100);
		
		// You can transform elements as well
		ctx.fillStyle = "rgba(10, 150, 255, 0.5)";
		
		// There are many ways to transform including translate, scale, rotate
		ctx.rotate(Math.PI / 4);
		ctx.scale(0.5, 1);
		ctx.fillRect(400, 50, 100, 100);
		
	}

}

onload = init;
```

### Example 5
```html
<!DOCTYPE HTML>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<link rel="stylesheet" href="styles.css">
	<title>HTML5 Tutorial</title>
	<script src="webstore.js"></script>
</head>

<body>
<div id="wrapper">

	<!-- You can store data using the web storage API rather then with cookies -->
	<p id="notify"></p>
	
	<input type="text" id="yourName" size="50">
	<button onClick="setName()">Store</button>
	<button onClick="getName()">Get</button>
	<button onClick="removeName()">Remove</button>

</div>
</body>
</html>
```

```js
// webstore.js
function init(){

	// Check if we can get a location
	if(navigator.geolocation){
	
		// Notify the user we are trying to find their location
		document.getElementById("notify").innerHTML = "We are trying to find you";
		
		// Declare what function to call on success and error
		navigator.geolocation.getCurrentPosition(successFunc, errorFunc);
	
	} else {
	
		// Notify the user that we can't get their location
		document.getElementById("notify").innerHTML = "Your browser doesn't support geolocation";
	
	}
}

function successFunc(pos){

	// Get the lat and long and output it
	var lat = pos.coords.latitude;
	var long = pos.coords.longitude;
	document.getElementById("notify").innerHTML = "You are at Lat: " +
		lat + " Long: " + long;

}

// Notify the user that an error occurred
function errorFunc(pos){

	document.getElementById("notify").innerHTML = "Error Occurred Locating You";

}

function setName(){

	// Get the value stored in the input element
	var userName = document.getElementById("yourName").value;
	
	// Check to make sure it isn't empty
	if(userName === "") return false;
	
	// Store the value with the key name
	localStorage.setItem("name", userName);
	
	// Change the text in the input to show the name was saved
	document.getElementById("yourName").value = "Name Saved";

}

function getName(){

	// Check that a value was saved
	if(localStorage.getItem("name") === null) return false;
	
	// Get the value stored and display it
	document.getElementById("yourName").value = "Name Stored : " +
		localStorage.getItem("name");

}

function removeName(){

	// Check that a value was saved
	if(localStorage.getItem("name") === null) return false;
	
	// Remove the value associated with the key name
	localStorage.removeItem("name");
	
	// Notify the user that the value was deleted
	document.getElementById("yourName").value = "Name Removed";

}

onload = init;
```

### References
- [HTML 5 Tutorial](https://www.youtube.com/watch?v=kDyJN7qQETA&list=PLGLfVvz_LVvSX7fVd4OUFp_ODd86H0ZIY&index=11)
