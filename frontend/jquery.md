### Target & Style Elements
- when the document is ready, execute the JQuery
```html
$("document").ready(function(){
	// your code
});
```

- Target ids (#)
```html
<img id="logo" />

$("#logo").css("float", "left");
```

- Target classes (.)
```html
<p id="p_two">test</p>

$(".p_two").css("color", "purple");
```

- How to pass in multiple arguments
```js
$(".p_two").css({ background: "url('repeatBkgrnd.png') repeat-y" });
```

- Target elements by tag
```html
<a href="#">Google</a>

$("a").css("color", "red");
```

- Target tags only in other tags, classes, or ids
```html
<table id="tableData">
	<tr><td><a href="#">Facebook</a></td></tr>
</table>

$("#tableData a").css("color", "green");
```

- Select and style every <a> element that are placed immediately after <em> elements:
```html
<p>...<em>...</em>...<a href="#">Google</a>...</p>

$("em + a").css("color", "orange");
```

- Target children elements in elements
```html
<p>...<em>...</em>...<a href="#">Google</a>...</p>

$("p > em").css("color", "gray");
```

- Target the 3rd li in a list
```html
<ol id="oListTypes">
	<li>
		Harry Potter
		<ol id="oListIndent">
			<li>and the Sorcerer's Stone</li>
			<li>and the Half-Blood Prince</li>
			<li>and the Chamber of Secrets</li>
			<li>Prisoner of Azkaban</li>
			<li name="best_book">and the Goblet of Fire</li>
		</ol>
	</li>
	<li>The Very Hungry Caterpillar</li>
</ol>

$("#oListIndent li:nth-child(3)").css("color", "red");
```

- Change the text in an html element if the li has a name attribute
```html
$("li[name]").html("'and the Goblet of Fire'");
```

- Change the value in a text input element
```html
<input type="text" id="yourName" size="50" />

$("input[type='text']#yourName").val("Derek");
```

- Target items that contain a value (*=)
```html
<a href="http://google.com">Google</a>

$("a[href*='google']").css("font-weight", "bolder");
```

- Target an image with an alt that starts with (^=) NTT and change the border
```html
<img id="logo" alt="NTT Logo" />
<a href="http://google.com">Google</a>
<a href="mailto:riskless2k@gmail.com">Morbi finibus</a>

$("img[alt^='NTT']").css({
	"border-color": "black",
	"border-width": "1px",
	"border-style": "solid",
});

$("a[href^='http://g']").css("color", "cyan");

$("a[href^='mailto:']").css("color", "yellow");
```

- Change an image height and width on one line
```html
$("img[alt^='NTT']").width(150).height(150);
```

- Target items that end with ($=) a value
```html
<a href="http://www.howtostudyit.com/cheatsheet.pdf">Cheatsheet</a>

$("a[href$='pdf']").css("color", "red");
```

- Select the odd (1,3,5) and even (0,2,4) items
```html
<table id="tableData">
	<tr><td></td></tr>
	<tr><td></td></tr>
	<tr><td></td></tr>
</table>

$("#tableData tr:even").css("background-color", "#FFFDD0");
$("#tableData tr:odd").css("background-color", "#F0F8FF");
```

- Select the first matching element
```js
$("#tableData tr:first").css({
	"background-color": "#001A57",
	color: "white",
});
```

- Select the last matching element
```js
$("#tableData tr:last").css("background-color", "#FFC0DB");
```

- Select the elements that don't contain 'and' in them
```js
$("#oListTypes li:not(:contains(and))").css("color", "red");
```

- Select every <a> element that contains 'gravida'
```js
$("a:contains(Google)").css("color", "blue");
```

- Select every <p> element that contains a <i> element and hide it
```html
<p>...<i>...<i>...</p>

$("p:has(i)").hide();
```

- Display the text between the matching <p> element
```js
alert($("p:has(i)").html());
```

- Change the text in the matching p element and show it 
```js
// .text() works the same but it doesn't recognize html elements
$("p:has(i)").html("<i>Some normal text</i>").show();

// Append adds text at the end of an element
$("p:has(i)").append(" I go at the end");

// Prepend adds text at the beginning of an element
$("p:has(i)").prepend("I go at the beginning ");

// Add a new element before the targeted one
$("p:has(i)").before("<p id='before_p'>A new paragraph before</p>");

// Add a new element after the targeted one
$("p:has(i)").after("<p id='after_p'>A new paragraph after</p>");

// When the element with the id 'after_p' is clicked, remove it from the document
$("#after_p").click(function () {
	$(this).remove();
});

// Replace an element with another on click
$("#before_p").click(function () {
	$(this).replaceWith("<p>I'm the new paragraph</p>");
});
```


- Perform a different action on each matching element
```html
<ol id="oListTypes">
	<li>
		Harry Potter
		<ol id="oListIndent">
			<li>and the Sorcerer's Stone</li>
			<li>and the Half-Blood Prince</li>
			<li>and the Chamber of Secrets</li>
			<li>Prisoner of Azkaban</li>
			<li name="best_book">and the Goblet of Fire</li>
		</ol>
	</li>
	<li>The Very Hungry Caterpillar</li>
</ol>

<input type="text" id="listStuff" size="50" /><br />

$("#oListIndent li").each(function (index) {
	// Get the value in the input element
	var inputListStuff = $("#listStuff").val();

	// Assign a new value to the input element
	// $(this).text() gets the value for each individual <li> element
	$("#listStuff").val(inputListStuff + ", " + $(this).text());
});
```

### Change element attributes
```html
<style type="text/css">
	.highlight {
		background-color: yellow;
	}
</style>

<img src="logo.png" id="logo" />

<input type="text" id="yourName" size="50" />
```

```js
// Add a class to elements
// .removeClass() will take the class away
$("#oListIndent li").addClass("Harry_Potter");

$(".Harry_Potter").css("color", "#36454F");

// You can toggle classes on and off an element
$("#oListIndent li").click(function () {
	$(this).toggleClass("highlight");

	// Get the changing background color and display it
	// in the input element
	var bgColor = $(this).css("background-color");
	$("input[type=text]#yourName").val(bgColor);
});

$("#logo").click(function () {
	// Get the value stored in an attribute
	var imgName = $(this).attr("src");
	$("input[type=text]#yourName").val(imgName);

	// Change the value of an attribute
	$("#logo").attr("src", "logo_change.png");
});
```

### Events in jQuery
```html
<img src="logo.png" id="logo" />

<h2 id="bestSelling">Best Selling Childrens Books</h2>

<h2 id="trackEvents">Tracking Events</h2>

Mouse Click : X : <input type="text" id="mClickXPos" /> Y :
<input type="text" id="mClickYPos" /><br />
Mouse Move : X : <input type="text" id="mMoveXPos" /> Y :
<input type="text" id="mMoveYPos" /><br />
Key Pressed : <input type="text" id="keyPressed" /><br />

<span id="formEvent">Form Event</span> :
<input type="text" id="inputFormEvent" /><br /><br />

<img src="1.jpg" id="accident" alt="accident" />

<div id="jsonData"></div>

<button type="button" id="getData">Get Data</button>
```

```js
// Trigger events on mouseover
$("#logo").mouseover(function () {
	$("#logo").attr("src", "logo_change.png");
});

// Trigger events on mouseout
$("#logo").mouseout(function () {
	$("#logo").attr("src", "logo.png");
});

// Handle mouseover and mouseout with hover
$("h2").hover(
	function () {
		// mouseover
		$("h2").css("color", "green");
	},
	function () {
		// mouseout
		$("h2").css("color", "black");
	}
);

// Perform an action on double click
$("#logo").dblclick(function () {
	$("#logo").attr("src", "logo-double.png");
});

// Get the document x and y position on click
$(document).click(function (e) {
	$("#mClickXPos").val(e.pageX);
	$("#mClickYPos").val(e.pageY);
});

// Get the x and y as the mouse moves
// Use screenX and screenY to get x and y for the screen
$(document).mousemove(function (e) {
	$("#mMoveXPos").val(e.screenX);
	$("#mMoveYPos").val(e.screenY);
});

// Show key pressed on the keyboard
// You can also use keydown() and keyup()
$(document).keypress(function (e) {
	// e.which returns the keycode which we convert into
	// the key with fromCharCode
	var keyPressed = String.fromCharCode(e.which);
	$("#keyPressed").val(keyPressed);
});

// Execute when input loses focus
$("#inputFormEvent").blur(function () {
	$("#formEvent").text("Left Input Element");
});

// Execute when input value changes (Conflicts with blur)
$("#inputFormEvent").change(function () {
	$("#formEvent").text("Input Element Changed");
});

// Execute when input gains focus
$("#inputFormEvent").focus(function () {
	$("#formEvent").text("Input Element Focused");
});

// Execute when input value is selected
$("#inputFormEvent").select(function () {
	$("#formEvent").text("Input Element Selected");
});

// We can assign events with on and pass the event to
// track, an argument to pass and the function to call
// You can attach multiple events by seperating them with
// a space ex. "mouseover keypress"

function showWhatTouched(e) {
	alert(e.data.message);
}

var bsMsg = { message: "Best Selling Touched" };

var teMsg = { message: "Track Events Touched" };

$("#bestSelling").on("mouseover", bsMsg, showWhatTouched);

$("#trackEvents").on("mouseover", teMsg, showWhatTouched);

// Catch when the document loads with ready()
// Catch if the browser resizes with resize()
// Catch when an element is scrolled with scroll()

// Create simple slide show
var accidentImgs = ["1.jpg", "2.jpg", "3.jpg", "4.jpg"];

var focusImg = 1;

$("#accident").click(function () {
	var image = accidentImgs[focusImg];
	focusImg++;
	if (focusImg > 3) {
		focusImg = 0;
	}

	$("#accident").attr("src", image);
});
```
### jQuery Animations
```html
<table id="tableData">
	<tr><td>Bonds</td></tr>
	<tr><td>Hall</td></tr>
	<tr><td></td></tr>
</table>

<p id="p_one">one</p>
<p id="p_two">two</p>
```

```js
// hide an element on click
$("#p_one").click(function () {
	$(this).hide();
});

// slowly fade out an element over 2000 ms
// You can also use fast, normal, and slow
$("#p_two").click(function () {
	$(this).fadeOut(2000);

	// Redisplay the hidden element
	// You can also use .toggle(), .show() and .fadeIn()
	$("#p_one").fadeToggle(2000);
});

// Fade an image to a given opacity
$("#logo").click(function () {
	$(this).fadeTo("slow", 0.5);
});

// Target td that contains 'Bonds'
$("td:contains('Bonds')").click(function () {
	// Hide row that contains a td that contains 'Bonds'
	$("tr:has(td:contains('Bonds'))").slideUp(1000);
});

// Make the Bonds row reappear
// You can also use slideToggle()
$("td:contains('Hall')").click(function () {
	$("tr:has(td:contains('Bonds'))").slideDown(1000);
});

// You can create custom animations with animate
$("#p_one").click(function () {
	// To define left, right, top or bottom 
	// the element must have a position property of relative or absolute
	$(this).css("position", "relative");

	// Pass an object that contains the properties to change,
	// duration in milliseconds, easing function to use for the transition 
	// and the function to call after the animation completes
	// The easing method functions are "swing or "linear"
	// You also have the JQuery UI animations : easeInQuad,
	// easeOutQuad, easeInOutQuad, easeInCubic, easeOutCubic,
	// easeInOutCubic, easeInQuart, easeOutQuart,
	// easeInOutQuart, easeInQuint, easeOutQuint,
	// easeInOutQuint, easeInExpo, easeOutExpo,
	// easeInOutExpo, easeInSine, easeOutSine, easeInOutSine,
	// easeInCirc, easeOutCirc, easeInOutCirc, easeInElastic,
	// easeOutElastic, easeInOutElastic, easeInBack,
	// easeOutBack, easeInOutBack, easeInBounce,
	// easeOutBounce, easeInOutBounce
	$("#p_one").animate(
		{
			left: 300,
			opacity: 0.5,
			"font-size": "22px",
			width: 300,
		},
		2000,
		"easeInQuad",
		function () {
			alert("All Done");
		}
	);
});
```


### JQuery UI
- datepicker
```js
<input type="text" name="birthday" id="birthday">

$("#birthday").datepicker({
	// Show month dropdown
	changeMonth: true,
	// Show year dropdown
	changeYear: true,
	dateFormat: "MM dd, yy",
	// Number of months to display
	numberOfMonths: 1,
	// Define maxDate
	maxDate: 365,
	// Define minDate
	minDate: -3650
});
```

### JQuery & Ajax
```html
<button type="button" id="oneButton">Get Text</button>
<button type="button" id="twoButton">Double Number</button>
<button type="button" id="threeButton">Get XML Data</button>

<!-- from TEXT file -->
<p id="first">Waiting for Message</p>

<!-- from PHP file -->
<form>
	<input type="text" name="data" id="data"></input>
	<h3>Double Number on the Server:</h3>
	<span></span>
</form>

<!-- from XML file -->
<div id="customers">Customer Information</div>

```

```js
$("document").ready(function() {
  $("#oneButton").on("click",getInfoFromServer);
  $("#twoButton").on("click",getDblFromServer);
  $("#threeButton").on("click",getXmlFromServer);
});

/* Pull text from a text file on the server */
// Type defines the type of request to make being GET or POST
// Success defines the function to call if the request succeeds
// Error could be defined to specify the function to call if an error happens
function getInfoFromServer() {
	$.ajax({
		type: "GET",
		url: "textFromServer.txt",
		success: postToPage
	});
}

// Function called to display the message from the server
// Receives the text and the server status
function postToPage(data, status) {
  $('#first').text(data);
}

/* From PHP file */
// Load a value into a span
// I define that the program getDouble.php should be executed
// getDouble is sent the information in the form and then it
// stores the returned info in the span
function getDblFromServer() {
  $("span").load(
		"getDouble.php", 
		$("#theForm").serializeArray()
	);
}

/* From XML file */
function getXmlFromServer() {
	$.ajax({
		type: "GET",
		url: "customers.xml",
		datatype: "xml",
		success: postToPageTwo
	});
}

// I use the find method to search through the xml data
// When I match for any of these attributes I append them to
// the div named customers
function postToPageTwo(data) {

  $(data).find('customer').each(function(){
    var id = $(this).attr('id');
    var firstName = $(this).find('firstName').text();
    var lastName = $(this).find('lastName').text();
    var street = $(this).find('street').text();
    var city = $(this).find('city').text();
    var zip = $(this).find('zip').text();
    
		$('<div class="firstname"></div>').html(firstName).appendTo('#customers');
    $('<div class="lastname"></div>').html(lastName).appendTo('#customers');
    $('<div class="street"></div>').html(street).appendTo('#customers');
    $('<div class="city"></div>').html(city).appendTo('#customers');
    $('<div class="zip"></div><br />').html(zip).appendTo('#customers');

  });
}
```

```php
<?php
// Get the value passed
$numberToDbl = $_POST["data"];

// Create the response
echo $_POST["data"] . " Times 2 Equals ";
$doubleUp = $numberToDbl * 2;
echo $doubleUp;
?>
```

```xml
<!-- customers.xml -->
<?xml version="1.0" encoding="iso-8859-1"?>
<customers>
	<customer id="0">
		<firstName>Paul</firstName>
		<lastName>Jones</lastName>
		<street>123 Main St</street>
		<city>Anywhere</city>
		<zip>15235</zip>
	</customer>

	<customer id="1">
		<firstName>Sally</firstName>
		<lastName>Jones</lastName>
		<street>123 Main St</street>
		<city>Anywhere</city>
		<zip>15235</zip>
	</customer>
</customers>
```

### JQuery Validation
```html
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/jquery-validation@1.17.0/dist/jquery.validate.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>

<form id="registrationForm">
	<div class="form-group">
		<input type="text" name="name" class="form-control" />
	</div>
	<div class="form-group">
		<input type="text" name="email" class="form-control" />
	</div>
	<div class="form-group">
		<input type="password" name="password" class="form-control" />
	</div>
	<div class="form-group">
		<input type="password" name="password_again" class="form-control" />
	</div>
	<div class="form-group">
		<input type="submit" value="Save" class="btn btn-lg btn-primary" />
	</div>
</form>

<script type="text/javascript">
$(document).ready(function() {
	
 	$(".registrationForm").validate({
		rules: {
			name: {
				required : true,
				minlength : 3,
				remote : {
					url: "http://localhost:8080/register/available",
					type: "get",
					data: {
						username: function() {
							return $("#name").val();
						}
					}
				} 
			},
			email: {
				required : true,
				email: true
			},
			password: {
				required : true,
				minlength : 4
			},
			password_again: {
				required : true,
				minlength : 4,
				equalTo: "#password"
			}
		},
		highlight: function(element) {
			$(element).closest('.form-group').removeClass('has-success').addClass('has-error');
		},
		unhighlight: function(element) {
			$(element).closest('.form-group').removeClass('has-error').addClass('has-success');
		},
		messages: {
			name: {
				remote: "Such username already exists!"
			}
		} 
	}
); 
});
</script>
```

### DataTable
```js
// response data
[
	{
		"id": 64,
		"title": "Walden",
		"author": "Henry David Thoreau"
	},
	{
		"id": 63,
		"title": "One night at call centre",
		"author": "Chethan Bhagath"
	}
]
```

```html
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
<link rel="stylesheet" type="text/css" href="https://cdn.datatables.net/1.10.18/css/jquery.dataTables.css"/>
<script type="text/javascript" src="https://cdn.datatables.net/1.10.18/js/jquery.dataTables.js"></script>

<table id = "datatable">
	<thead>
		<tr>
			<th>Id</th>
			<th>Title</th>
			<th>Author</th>
		</tr>
	</thead>
</table>

$(document).ready(function () {
	$.ajax({
		url: "http://localhost:8080/api/book",
		method: "GET",
		dataType: "json",
		success: function (data) {
			$("#datatable").dataTable({
				data: data,
				sort: false,
				searching: false,
				paging: false,
				columns: [
					{'data': 'id'},
					{'data': 'title'},
					{'data': 'author'},
				]
			})
		}
	})
})
```

### References
- [JQuery Tutorial](https://www.youtube.com/watch?v=BWXggB-T1jQ&list=PLGLfVvz_LVvSX7fVd4OUFp_ODd86H0ZIY&index=29)
- [Display the database records in jQuery Datatables](https://www.youtube.com/watch?v=QTgaZAlGPZg&list=PLA7e3zmT6XQX825MndBpE-Uvox5lhrlEn)
