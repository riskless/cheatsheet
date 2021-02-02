### Project Planning
1. User Stories
2. Features
3. Flowchart
4. Architecture
5. Development Step

### User Stories
- User story: Description of the application's functionality from the user's perspective
- Common format: As a [type of user], I want [an action] so that [a benefit]
- Example
	- As a user, I want to search for recipes, so that I can find new ideas for meals
	- As a user, I want to be able to update the number of servings, so that I can cook a meal for different number of people
	- As a user, I want to bookmark recipes, so that I can review them later
	- As a user, I want to be able to create my own recipes, so that I have them all organized in the same app
	- As a user, I want to be able to see my bookmarks and own recipes when I leave the app and come back later, so that I can close the app safely after cooking

### Features
1. Search for recipes
	- Search functionality: input field to send request to API with searched keywords
	- Display results with pagination
	- Display recipe with cooking time, servings and ingredients
2. Update the number of servings
	- Change servings functionality: update all ingredients according to current number of servings
3. Bookmark recipes
	- Bookmarking functionality: display list of all bookmarked recipes
4. Create my own recipes
	- Uer can upload own recipes
	- User recipes will automatically be bookmarked
	- User can only see their own recipes, not recipes from other users
5. See my bookmarks and own recipes when I leave the app and come back later
	- Store bookmark data in the browser using local storage

### Flowchart
![Flowchart](images/flowchart-1.png)
![Flowchart](images/flowchart-2.png)
![Flowchart](images/flowchart-2.png)

### Architecture
![Architecture](images/architecture.png)

### Why worry about architecture?
- structure
	- Like a house, software needs a structure: the way we organize our code
- maintainablility
	- A project is never done! We need to be able to easily change it in the future
- expandability
	- We also need to be able to easily add new features
- what kind of architecture
	- We can create our own architecture
	- We can use a well-established architecture pattern like MVC, MVP, Flux, etc.
	- We can use a framework like React, Angular, Vue, Svelte, etc.
- components of any architecture
	- business logic
		- Code that solves the actual business problem
		- Directly related to what business does and what it needs
		- Example: sending messages, storing transactions, caculating taxes, ...
	- state
		- Essentially stores all the data about the application
		- Should be the "single source of truth"
		- UI should be kept in sync with the state
		- State libraries exist (Redux,etc)
	- http library
		- Responsible for making and reciving AJAX requests
		- Optional but almost always necessary in real-world apps
	- application logic (router)
		- Code that is only concerned about the implementation of application itself
		- Handles navigation and UI events
	- presentation logic (UI layer)
		- Code that is concerned about the visible part of the application
		- Essentially displays application state

- MVC (Model-Controller-View) archiecture
	- Model
		- business logic
		- state
		- http library
	- Controller
		- application logic
		- Bridge between model and views (which don't know about one another)
		- Handles UI events and dispatches tasks to model and view
	- View
		- presentation logic

```js
/* model */
export const state = {
  recipe: {},
  search: {
    query: '',
    results: [],
    page: 1,
    resultsPerPage: RES_PER_PAGE,
  },
  bookmarks: [],
};

const createRecipeObject = function (data) {};
export const loadRecipe = async function (id) {};
export const loadSearchResults = async function (query) {};
export const getSearchResultsPage = function (page = state.search.page) {};
export const updateServings = function (newServings) {};
const persistBookmarks = function () {};
export const addBookmark = function (recipe) {};
export const deleteBookmark = function (id) {};
const init = function () {};
init();
export const uploadRecipe = async function (newRecipe) {};

/* controller */
import * as model from './model.js';
import recipeView from './views/recipeView.js';

const controlRecipes = async function () {};
const controlSearchResults = async function () {};
const controlPagination = function (goToPage) {};
const controlServings = function (newServings) {};
const controlAddBookmark = function () {};
const controlBookmarks = function () {};
const controlAddRecipe = async function (newRecipe) {};
const init = function () {};
init();

/* views */
// View
export default class View {
}

// resultsView
import View from './View.js';
class RecipeView extends View {
}
export default new RecipeView();

// addRecipeView
// bookmarksView
// paginationView
// previewView
// recipeView
// searchView

```
- index.html
```html
<head>
	<link rel="stylesheet" href="src/sass/main.scss" />
	<script defer src="src/js/controller.js"></script>
<head>
<body>
	<div class="container">
		<header class="header">
			<img src="src/img/logo.png" alt="Logo" class="header__logo" />
			<form class="search"></form>
			<nav class="nav">
				<ul class="nav__list">
					<li class="nav__item"><span>Add recipe</span></li>
					<li class="nav__item">
						<button class="nav__btn nav__btn--bookmarks">
							<span>Bookmarks</span>
						</button>
						<div class="bookmarks">
							<ul class="bookmarks__list"></ul>
						</div>
					</li>
				</ul>
			</nav>
		</header>
		<div class="search-results">
			<ul class="results"></ul>
			<div class="pagination"></div>
			<p class="copyright"></p>
		</div>
		<div class="recipe">
			<div class="message"></div>
			<div class="spinner"></div>
			<div class="error"></div>
			<figure class="recipe__fig"></figure>
			<div class="recipe__details"></div>
			<div class="recipe__ingredients"></div>
			<div class="recipe__directions"></div>
		</div>
	</div>

	<div class="overlay hidden"></div>
	<div class="add-recipe-window hidden">
		<button class="btn--close-modal">&times;</button>
		<form class="upload">
			<div class="upload__column"></div>
			<div class="upload__column"></div>
			<button class="btn upload__btn"></button>
		</form>
	</di>
</body>
```

### References
- [The Complete JavaScript Course](https://www.udemy.com/course/the-complete-javascript-course/)
