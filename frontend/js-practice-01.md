### Application name: Mapty

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
	- As a user, I want to log my running workouts with location, distance, time, pace and steps/minutes, so I can keep a log of all my running
	- As a user, I want to log my cycling workouts with location, distance, time, speed and elevation gain, so I can keep a log of all my cycling
	- As a user, I want to see all my workouts at a glance, so I can easily track my progress over time
	- As a user, I want to also see my workouts on a map, so I can easily check where I work out the most
	- As a user, I want to see all my workouts when I leave the app and come back later, so that I can keep using there app over time.

### Features
1. Log my running workouts with location, distance, time, pace and steps/minute
	- Search functionality: input field to send request to API with searched keywords
	- Map where user clicks to add new workout (best way to get location coordinates)
	- Geolocation to display map at current location (more user friendly)
	- Form to input distance, time, pace, steps/minute
2. Log my cycling workouts with location, distance, time, speed and elevation gain
	- Form to input distance, time, speed, elevation gain
3. See all my workouts at a glance
	- Display all workouts in a list
4. See my workouts on a map
	- Display all workouts on the map
5. See all my workouts when I leave the app and come back later
	- Store workout data in the browser using local storage API
	- On page load, read the saved data from local storage and display

### Flowchart
- In the real-world, you don't have to come with the final flowchart right in the planning phase. It's normal that it changes throughout implementation.
![Flowchart](images/mapty-flowchart.png)

### Architecture
![Flowchart](images/mapty-archiecture.png)


### References
- [The Complete JavaScript Course](https://www.udemy.com/course/the-complete-javascript-course/)
