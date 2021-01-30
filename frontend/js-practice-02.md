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

### References
- [The Complete JavaScript Course](https://www.udemy.com/course/the-complete-javascript-course/)
