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
![Architecture](images/mapty-architecture.png)

```js
/* Workout */
class Workout {
	constructor() {}
	_setDescription() {}
	click() {}
}

/* Running */
class Running extends Workout {
	constructor() {}
	calcPace() {}
}

/* Cycling */
class Cycling extends Workout {
	constructor() {}
	calcSpeed() {}
}

/* App */
class App {
	constructor() {}
	_getPosition() {}
	_loadMap() {}
	_showForm() {}
	_hideForm() {}
	_toggleElevationField() {}
	_newWorkout() {}
	_renderWorkoutMarker() {}
	_renderWorkout() {}
	_moveToPopup() {}
	_setLocalStorage() {}
	_getLocalStorage() {}
	reset() {}
}
const app = new App();
```
```html
<!-- index -->
<head>
	<link rel="stylesheet" href="style.css" />
	<!-- leaflet CDN -->
	<script defer src="script.js"></script>
</head>
<body>
	<div class="sidebar">
		<ul class="workouts">
			<form class="form hidden">
				<div class="form__row">
					<select class="form__input form__input--type">
						<option value="running">Running</option>
						<option value="cycling">Cycling</option>
					</select>
				</div>
				<div class="form__row">
					<input class="form__input form__input--distance" placeholder="km" />
				</div>
				<div class="form__row">
					<input class="form__input form__input--duration" placeholder="min" />
				</div>
				<div class="form__row">
					<input class="form__input form__input--cadence" placeholder="step/min" />
				</div>
				<div class="form__row form__row--hidden">
					<input class="form__input form__input--elevation" placeholder="meters" />
				</div>
				<button class="form__btn">OK</button>
			</form>
		</ul>

		<p class="copyright">PRACTICE MAKES PERFECT</p>
	</div>
	<div id="map"></div>
</body>
```

### Development
- Get user's position
```js
class App {
	constructor() {
    // Get user's position
    this._getPosition();
	}

	_getPosition() {
    if (navigator.geolocation)
      navigator.geolocation.getCurrentPosition(
        this._loadMap.bind(this),
        function () {
          alert('Could not get your position');
        }
      );
  }
}
```

- Show map using the leaflet library which is an open-source JavaScript library for mobile-friendly interactive maps
```js
class App {
	#map;
	#mapZoomLevel = 13;

	_loadMap(position) {
    const { latitude } = position.coords;
    const { longitude } = position.coords;
    // console.log(`https://www.google.pt/maps/@${latitude},${longitude}`);

    const coords = [latitude, longitude];

    this.#map = L.map('map').setView(coords, this.#mapZoomLevel);

    L.tileLayer('https://{s}.tile.openstreetmap.fr/hot/{z}/{x}/{y}.png', {
      attribution:
        '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
    }).addTo(this.#map);

    // Handling clicks on map
    this.#map.on('click', this._showForm.bind(this));

    this.#workouts.forEach(work => {
      this._renderWorkoutMarker(work);
    });
  }

  _showForm(mapE) {
    this.#mapEvent = mapE;
    form.classList.remove('hidden');
    inputDistance.focus();
  }

}
```
```html
// CDN (Content Delivery Network)
<link
	rel="stylesheet"
	href="https://unpkg.com/leaflet@1.6.0/dist/leaflet.css"
	integrity="sha512-xwE/Az9zrjBIphAcBb3F6JVqxf46+CDLwfLMHloNu6KEQCAWi6HcDUbeOfBIptF7tcCzusKFjFw2yuvEpDL9wQ=="
	crossorigin=""
/>
<script
	defer
	src="https://unpkg.com/leaflet@1.6.0/dist/leaflet.js"
	integrity="sha512-gZwIG9x3wUXg2hdXF6+rVkLF/0Vi9U8D2Ntg4Ga5I5BZpVkVxlJWbSQtXPSiUTtC0TjtGOmxa1AJPuV0CPthew=="
	crossorigin=""
></script>
```

- Creating a new workout
```js
const form = document.querySelector('.form');
const containerWorkouts = document.querySelector('.workouts');
const inputType = document.querySelector('.form__input--type');
const inputDistance = document.querySelector('.form__input--distance');
const inputDuration = document.querySelector('.form__input--duration');
const inputCadence = document.querySelector('.form__input--cadence');
const inputElevation = document.querySelector('.form__input--elevation');

class App {
  constructor() {
    // Attach event handlers
    form.addEventListener('submit', this._newWorkout.bind(this));
		inputType.addEventListener('change', this._toggleElevationField);
  }

	_newWorkout(e) {
    const validInputs = (...inputs) => inputs.every(inp => Number.isFinite(inp));
    const allPositive = (...inputs) => inputs.every(inp => inp > 0);

    e.preventDefault();

    // Get data from form
    const type = inputType.value;
    const distance = +inputDistance.value;
    const duration = +inputDuration.value;
    const { lat, lng } = this.#mapEvent.latlng;
    let workout;

    // If workout running, create running object
    if (type === 'running') {
      const cadence = +inputCadence.value;

      // Check if data is valid
      if (
        // !Number.isFinite(distance) ||
        // !Number.isFinite(duration) ||
        // !Number.isFinite(cadence)
        !validInputs(distance, duration, cadence) ||
        !allPositive(distance, duration, cadence)
      )
        return alert('Inputs have to be positive numbers!');

      workout = new Running([lat, lng], distance, duration, cadence);
    }

    // If workout cycling, create cycling object
    if (type === 'cycling') {
      const elevation = +inputElevation.value;

      if (
        !validInputs(distance, duration, elevation) ||
        !allPositive(distance, duration)
      )
        return alert('Inputs have to be positive numbers!');

      workout = new Cycling([lat, lng], distance, duration, elevation);
    }

		// Add new object to workout array
		//this.#workouts.push(workout);

		// Render workout on map as marker
		//this._renderWorkoutMarker(workout);

		// Render workout on list
		//this._renderWorkout(workout);

		// Hide form + clear input fields
		this._hideForm();

		// Set local storage to all workouts
		//this._setLocalStorage();
  }

	_toggleElevationField() {
		inputElevation.closest('.form__row').classList.toggle('form__row--hidden');
		inputCadence.closest('.form__row').classList.toggle('form__row--hidden');
  }
}
```

### Rendering Workouts
```js
/* App */
class App {
  _hideForm() {
    // Empty inputs
    inputDistance.value = inputDuration.value = inputCadence.value = inputElevation.value =
      '';

    form.style.display = 'none';
    form.classList.add('hidden');
    setTimeout(() => (form.style.display = 'grid'), 1000);
  }

  _renderWorkoutMarker(workout) {
    L.marker(workout.coords)
      .addTo(this.#map)
      .bindPopup(
        L.popup({
          maxWidth: 250,
          minWidth: 100,
          autoClose: false,
          closeOnClick: false,
          className: `${workout.type}-popup`,
        })
      )
      .setPopupContent(`${workout.type === 'running' ? '🏃‍♂️' : '🚴‍♀️'} ${workout.description}`) // emoji & description
      .openPopup();
  }

  _renderWorkout(workout) {
    let html = `
      <li class="workout workout--${workout.type}" data-id="${workout.id}">
        <h2 class="workout__title">${workout.description}</h2>
        <div class="workout__details">
          <span class="workout__icon">${workout.type === 'running' ? '🏃‍♂️' : '🚴‍♀️'}</span>
          <span class="workout__value">${workout.distance}</span>
          <span class="workout__unit">km</span>
        </div>
        <div class="workout__details">
          <span class="workout__icon">⏱</span>
          <span class="workout__value">${workout.duration}</span>
          <span class="workout__unit">min</span>
        </div>
    `;

    if (workout.type === 'running')
      html += `
        <div class="workout__details">
          <span class="workout__icon">⚡️</span>
          <span class="workout__value">${workout.pace.toFixed(1)}</span>
          <span class="workout__unit">min/km</span>
        </div>
        <div class="workout__details">
          <span class="workout__icon">🦶🏼</span>
          <span class="workout__value">${workout.cadence}</span>
          <span class="workout__unit">spm</span>
        </div>
      </li>
      `;

    if (workout.type === 'cycling')
      html += `
        <div class="workout__details">
          <span class="workout__icon">⚡️</span>
          <span class="workout__value">${workout.speed.toFixed(1)}</span>
          <span class="workout__unit">km/h</span>
        </div>
        <div class="workout__details">
          <span class="workout__icon">⛰</span>
          <span class="workout__value">${workout.elevationGain}</span>
          <span class="workout__unit">m</span>
        </div>
      </li>
      `;

    form.insertAdjacentHTML('afterend', html);
  }
}

/* Workout */
class Workout {
  _setDescription() {
    // prettier-ignore
    const months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];

    this.description = `${this.type[0].toUpperCase()}${this.type.slice(1)} on ${
      months[this.date.getMonth()]
    } ${this.date.getDate()}`;
  }
}

/* Running */
class Running extends Workout {
  type = 'running';

  constructor(coords, distance, duration, cadence) {
    this._setDescription();
  }
}

/* Cycling */
class Cycling extends Workout {
  type = 'cycling';

  constructor(coords, distance, duration, elevationGain) {
    this._setDescription();
  }
}
```

### Move to Marker on click
```js
class App {
	constructor() {
		containerWorkouts.addEventListener('click', this._moveToPopup.bind(this));
	}

  _moveToPopup(e) {
    // BUGFIX: When we click on a workout before the map has loaded, we get an error. But there is an easy fix:
    if (!this.#map) return;

    const workoutEl = e.target.closest('.workout');

    if (!workoutEl) return;

    const workout = this.#workouts.find(
      work => work.id === workoutEl.dataset.id
    );

    this.#map.setView(workout.coords, this.#mapZoomLevel, {
      animate: true,
      pan: {
        duration: 1,
      },
    });

    // using the public interface
    // workout.click();
  }
}
```

### LocalStroage
```js
/* App */
class App {
	constructor() {
    // Get data from local storage
    this._getLocalStorage();
	}

  _loadMap(position) {
    this.#workouts.forEach(work => {
      this._renderWorkoutMarker(work);
    });
  }

	_newWorkout(e) {
	  // Set local storage to all workouts
    this._setLocalStorage();
	}

  _setLocalStorage() {
    localStorage.setItem('workouts', JSON.stringify(this.#workouts));
  }

  _getLocalStorage() {
    const data = JSON.parse(localStorage.getItem('workouts'));

    if (!data) return;

    this.#workouts = data;

    this.#workouts.forEach(work => {
      this._renderWorkout(work);
    });
  }

  reset() {
    localStorage.removeItem('workouts');
    location.reload();
  }
}
```

### References
- [The Complete JavaScript Course](https://www.udemy.com/course/the-complete-javascript-course/)
