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
### Development
- Intalling parcel for Bundling
```js
/* package.json */
{
	"script": {
		"start": "parcel index.html",
		"build": "parcel build index.html --dist-dir ./dist"
	},
  "devDependencies": {
    "parcel": "^2.0.0-beta.1",
    "sass": "^1.26.10"
  },
  "dependencies": {
    "core-js": "^3.6.5",
    "regenerator-runtime": "^0.13.7"
  }
}
```

- Loading a Recipe from API
```js
/* controller */
import * as model from './model.js';
import recipeView from './views/recipeView.js';

const controlRecipes = async function () {
	try {
		const id = window.location.hash.slice(1); // #id
		console.log(id);
		
		if(!id) return;

		// Loading recipe
		await model.loadRecipe(id);

		// Rendering recipe
		recipeView.render(model.state.recipe);

	} catch (err) {
		console.log(err);
	}
};

// Listening for load and hashchange events
window.addEventListener('hashchange', controlRecipes);
window.addEventListener('load', controlRecipes);
// ['hashchange','load'].forEach(ev => window.addEventListener(ev, controlRecipes));

/* model */
import { API_URL, KEY } from './config.js';
import { getJSON } from './helpers.js';

export const state = {
  recipe: {},

export const loadRecipe = async function (id) {
  try {
    const data = await getJSON(`${API_URL}${id}?key=${KEY}`);
    const {recipe} = data.data;

		recipe = {
			id: recipe.id,
			title: recipe.title,
			publisher:recipe.publisher,
			sourceUrl: recipe.source_url,
			image:recipe.image_url,
			servings: recipe..servings,
			cookingTime: recipe.cooking_time,
			ingredients:recipe.ingredients
		};
    console.log(state.recipe);
  } catch (err) {
    // Temp error handling
    console.error(`${err}`);
  }
};

/* RecipeView */
import Fraction from 'fractional'; 
class RecipeView {
	#parentElement = document.querySelector('.recipe');
	#data;

	render(data) {
		this.#data = data;

		//-- fractional (Number formatting)
		// npm install fractional
		// 0.5 -> 1/2
		const markup = `${this.#data.title} ${quantity ? new Fraction(quantity}.toString() : ''}`;
		this.#parentElement.innerHTML = '';
		this.#parentElement.insertAdjacentHTML('afterbegin', markup);
	}
}
export default new RecipeView()

/* helpers */
import { TIMEOUT_SEC } from './config.js';

const timeout = function (s) {
  return new Promise(function (_, reject) {
    setTimeout(function () {
      reject(new Error(`Request took too long! Timeout after ${s} second`));
    }, s * 1000);
  });
};

export const getJSON = async function (url) {
  try {
		const fetchPro = fetch(url);
    const res = await Promise.race([fetchPro, timeout(TIMEOUT_SEC)]);
    const data = await res.json();

		if (!res.ok) throw new Error(`${data.message} (${res.status})`);
    return data;
  } catch (err) {
    throw err;
  }
};

/* config */
export const API_URL = 'https://forkify-api.herokuapp.com/api/v2/recipes/';
export const TIMEOUT_SEC = 10;
export const KEY = '<YOUR_KEY>';
```
- Spinner
```js
/* controller */
const controlRecipes = async function () {
	try {
		// Spinner
		recipeView.renderSpinner();

		// Loading recipe
		// Rendering recipe
	} catch (err) {
		console.log(err);
	}
};

/* RecipeView */
import icons from 'url:../../img/icons.svg'; // Parcel 2
class RecipeView {
	renderSpinner() {
		const markup = `
			<div class="spinner">
				<svg>
					<use href="${icons}#icon-loader"></use>
				</svg>
			</div>
		`;
		this._clear();
		this._parentElement.insertAdjacentHTML('afterbegin', markup);
	}
}
```

- Event Handlers in MVC (Publisher-Subscriber Pattern)
	- Events should be handled in the controller (otherwise we would have application logic in the view)
	- Events should be listened for in the view (otherwise we would need DOM elements in the controller)
	- Publisher: Code that knows when to react
	- Subscriber: Code that wants to react
	- Subscribe to publisher by passing in the subscriber function

```js
/* controller */
const init = function () {
  // controlRecipes will be passed into addHandlerRender when program starts
  recipeView.addHandlerRender(controlRecipes);
};

/* RecipeView */
// addHandlerRender listens for events (addEventListener), and uses controlRecipes as callback
addHandlerRender(handler) {
	['hashchange','load'].forEach(ev => window.addEventListener(ev, handler));
}
```

- Error Handling
```js
/* controller */
const controlRecipes = async function () {
  try {
    // Loading recipe
    // Rendering recipe
  } catch (err) {
    recipeView.renderError();
    console.error(err);
  }
};

/* model */
export const loadRecipe = async function (id) {
  try {
  } catch (err) {
    console.error(`${err}`);
    throw err;
  }
};

/* RecipeView */
import icons from 'url:../../img/icons.svg'; // Parcel 2
class RecipeView {
	_errorMessage = 'We could not find that recipe. Please try another one!';
	_message = 'Success';

	// error message
  renderError(message = this._errorMessage) {
    const markup = `
      <div class="error">
        <div>
          <svg>
            <use href="${icons}#icon-alert-triangle"></use>
          </svg>
        </div>
        <p>${message}</p>
      </div>
    `;
    this._clear();
    this._parentElement.insertAdjacentHTML('afterbegin', markup);
  }
	
	// success message
  renderMessage(message = this._message) {
    const markup = `
      <div class="message">
        <div>
          <svg>
            <use href="${icons}#icon-smile"></use>
          </svg>
        </div>
        <p>${message}</p>
      </div>
    `;
    this._clear();
    this._parentElement.insertAdjacentHTML('afterbegin', markup);
  }
}
```
- search results
```js
/* model */
export const state = {
  recipe: {},
  search: {
    query: '',
    results: []
  }
};

export const loadSearchResults = async function (query) {
  try {
		state.search.query = query;

		const data = await getJSON(`${API_URL}?search=${query}`);
    console.log(data);

		state.search.results = data.data.recipes.map(rec => {
			return {
        id: rec.id,
        title: rec.title,
        publisher: rec.publisher,
        image: rec.image_url
			};
		});

		console.log(state.search.results);
  } catch (err) {
    throw err;
  }
};

/* controller */
import searchView from './views/searchView.js';

const controlSearchResults = async function () {
  try {
	  // Get search query
    const query = searchView.getQuery();
    if (!query) return;

		// Load search results
		await model.loadSearchResults(query);

		// Render results
		resultsView.render(model.state.search.results);
  } catch (err) {
    console.log(err);
  }
};

const init = function () {
  searchView.addHandlerSearch(controlSearchResults);
};

/* SearchView */
class SearchView {
  _parentEl = document.querySelector('.search');

  getQuery() {
    const query = this._parentEl.querySelector('.search__field').value;
    this._clearInput();
    return query;
  }

  _clearInput() {
    this._parentEl.querySelector('.search__field').value = '';
  }

  addHandlerSearch(handler) {
    this._parentEl.addEventListener('submit', function (e) {
      e.preventDefault();
      handler();
    });
  }
}
export default new SearchView();

/* ResultsView */
import View from './View.js';

class ResultsView extends View {
  _parentElement = document.querySelector('.results');
  _errorMessage = 'No recipes found for your query! Please try again ;)';
  _message = '';

	_generateMarkup() {
		console.log(this._data);
		return this._data.map(this._generateMarkupPreview).join('');
	}

  _generateMarkupPreview(result) {
		
    return `
			<li class="preview">
				<a class="preview__link preview__link--active" href="#23456">
					<figure class="preview__fig">
						<img src="${result.image}" alt="${result.title}" />
					</figure>
					<div class="preview__data">
						<h4 class="preview__title">${result.title}</h4>
						<p class="preview__publisher">${result.publisher}</p>
						<div class="preview__user-generated">
							<svg>
								<use href="${icons}#icon-user"></use>
							</svg>
						</div>
					</div>
				</a>
			</li>
		`;
  }
}
export default new ResultsView();

/* View */
import icons from 'url:../../img/icons.svg'; // Parcel 2
export default class View {
  _data;

  render(data) {
		if (!data || (Array.isArray(data) && data.length === 0))
			return this.renderError();
    this._data = data;
    const markup = this._generateMarkup();
    this._clear();
    this._parentElement.insertAdjacentHTML('afterbegin', markup);
  }

  _clear() {
    this._parentElement.innerHTML = '';
  }

	renderSpinner() {
		const markup = `
			<div class="spinner">
				<svg>
					<use href="${icons}#icon-loader"></use>
				</svg>
			</div>
		`;
		this._clear();
		this._parentElement.insertAdjacentHTML('afterbegin', markup);
	}

	// error message
  renderError(message = this._errorMessage) {
    const markup = `
      <div class="error">
        <div>
          <svg>
            <use href="${icons}#icon-alert-triangle"></use>
          </svg>
        </div>
        <p>${message}</p>
      </div>
    `;
    this._clear();
    this._parentElement.insertAdjacentHTML('afterbegin', markup);
  }
	
	// success message
  renderMessage(message = this._message) {
    const markup = `
      <div class="message">
        <div>
          <svg>
            <use href="${icons}#icon-smile"></use>
          </svg>
        </div>
        <p>${message}</p>
      </div>
    `;
    this._clear();
    this._parentElement.insertAdjacentHTML('afterbegin', markup);
  }
}
```

- Pagination
```js
/* model */
export const state = {
  recipe: {},
  search: {
    page: 1,
    resultsPerPage: RES_PER_PAGE
  }
};

export const loadSearchResults = async function (query) {
  try {
		state.search.page = 1;
  } catch (err) {
    throw err;
  }
};

export const getSearchResultsPage = function (page = state.search.page) {
  state.search.page = page;

  const start = (page - 1) * state.search.resultsPerPage; // 0
  const end = page * state.search.resultsPerPage; // 9

  return state.search.results.slice(start, end);
};

/* PaginationView */ 
class PaginationView extends View {
  _parentElement = document.querySelector('.pagination');

  addHandlerClick(handler) {
    this._parentElement.addEventListener('click', function (e) {
      const btn = e.target.closest('.btn--inline');
      if (!btn) return;

      const goToPage = +btn.dataset.goto;
      handler(goToPage);
    });
  }

  _generateMarkup() {
    const curPage = this._data.page;
    const numPages = Math.ceil(this._data.results.length / this._data.resultsPerPage);

    // Page 1, and there are other pages
    if (curPage === 1 && numPages > 1) {
      return `
        <button data-goto="${
          curPage + 1
        }" class="btn--inline pagination__btn--next">
          <span>Page ${curPage + 1}</span>
          <svg class="search__icon">
            <use href="${icons}#icon-arrow-right"></use>
          </svg>
        </button>
      `;
    }

    // Last page
    if (curPage === numPages && numPages > 1) {
      return `
        <button data-goto="${
          curPage - 1
        }" class="btn--inline pagination__btn--prev">
          <svg class="search__icon">
            <use href="${icons}#icon-arrow-left"></use>
          </svg>
          <span>Page ${curPage - 1}</span>
        </button>
      `;
    }

    // Other page
    if (curPage < numPages) {
      return `
        <button data-goto="${
          curPage - 1
        }" class="btn--inline pagination__btn--prev">
          <svg class="search__icon">
            <use href="${icons}#icon-arrow-left"></use>
          </svg>
          <span>Page ${curPage - 1}</span>
        </button>
        <button data-goto="${
          curPage + 1
        }" class="btn--inline pagination__btn--next">
          <span>Page ${curPage + 1}</span>
          <svg class="search__icon">
            <use href="${icons}#icon-arrow-right"></use>
          </svg>
        </button>
      `;
    }

    // Page 1, and there are NO other pages
    return '';
  }
}
export default new PaginationView();

/* controller */
import searchView from './views/PaginationView.js';

const controlSearchResults = async function () {
  try {
	  // Get search query
		// Load search results

		// Render results
		//resultsView.render(model.state.search.results);
		resultsView.render(model.getSearchResultsPage());

    // Render initial pagination buttons
    paginationView.render(model.state.search);
  } catch (err) {
    console.log(err);
  }
};

const controlPagination = function (goToPage) {
  // 1) Render NEW results
  resultsView.render(model.getSearchResultsPage(goToPage));

  // 2) Render NEW pagination buttons
  paginationView.render(model.state.search);
};

const init = function () {
  paginationView.addHandlerClick(controlPagination);
};
```
### References
- [The Complete JavaScript Course](https://www.udemy.com/course/the-complete-javascript-course/)
