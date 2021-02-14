### Project Overivew
- Name: Project Management
- Main Functions
	- Adding a new project information
	- Changing a active project to a finished project and vice versa
- Development environment
	- TypeScript, lite-server, VSCode
- User Interface

![User Interface](images/ts-practice-01.png)

### Set up development environment
- npm install -g typescript
- npm install --save-dev lite-server
- tsc --init // creating tsconfig.json
- tsc -w

```js
/* package.json */
"scripts": {
	"start": "lite-server"
}

/* tsconfig.json */
"compilerOptions": {
	"outDir": "./dist"
}
```
### Development
```js
/* app.ts */
// Drag & Drop Interfaces
interface Draggable {
  dragStartHandler(event: DragEvent): void;
  dragEndHandler(event: DragEvent): void;
}

interface DragTarget {
  dragOverHandler(event: DragEvent): void;
  dropHandler(event: DragEvent): void;
  dragLeaveHandler(event: DragEvent): void;
}

// Project Type
enum ProjectStatus {
  Active,
  Finished
}

class Project {
  constructor(
    public id: string,
    public title: string,
    public description: string,
    public people: number,
    public status: ProjectStatus
  ) {}
}

// Project State Management
type Listener<T> = (items: T[]) => void;

class State<T> {
  protected listeners: Listener<T>[] = [];

  addListener(listenerFn: Listener<T>) {
    this.listeners.push(listenerFn);
  }
}

class ProjectState extends State<Project> {
  private projects: Project[] = [];
  private static instance: ProjectState;

  private constructor() {
    super();
  }

  static getInstance() {
    if (this.instance) {
      return this.instance;
    }
    this.instance = new ProjectState();
    return this.instance;
  }

  addProject(title: string, description: string, numOfPeople: number) {
    const newProject = new Project(
      Math.random().toString(),
      title,
      description,
      numOfPeople,
      ProjectStatus.Active
    );
    this.projects.push(newProject);
    this.updateListeners();
  }

  moveProject(projectId: string, newStatus: ProjectStatus) {
    const project = this.projects.find(prj => prj.id === projectId);
    if (project && project.status !== newStatus) {
      project.status = newStatus;
      this.updateListeners();
    }
  }

  private updateListeners() {
    for (const listenerFn of this.listeners) {
      listenerFn(this.projects.slice());
    }
  }
}

const projectState = ProjectState.getInstance();

// Validation
interface Validatable {
  value: string | number;
  required?: boolean;
  minLength?: number;
  maxLength?: number;
  min?: number;
  max?: number;
}

function validate(validatableInput: Validatable) {
  let isValid = true;
  if (validatableInput.required) {
    isValid = isValid && validatableInput.value.toString().trim().length !== 0;
  }
  if (
    validatableInput.minLength != null &&
    typeof validatableInput.value === 'string'
  ) {
    isValid =
      isValid && validatableInput.value.length >= validatableInput.minLength;
  }
  if (
    validatableInput.maxLength != null &&
    typeof validatableInput.value === 'string'
  ) {
    isValid =
      isValid && validatableInput.value.length <= validatableInput.maxLength;
  }
  if (
    validatableInput.min != null &&
    typeof validatableInput.value === 'number'
  ) {
    isValid = isValid && validatableInput.value >= validatableInput.min;
  }
  if (
    validatableInput.max != null &&
    typeof validatableInput.value === 'number'
  ) {
    isValid = isValid && validatableInput.value <= validatableInput.max;
  }
  return isValid;
}

// autobind decorator
function autobind(_: any, _2: string, descriptor: PropertyDescriptor) {
  const originalMethod = descriptor.value;
  const adjDescriptor: PropertyDescriptor = {
    configurable: true,
    get() {
      const boundFn = originalMethod.bind(this);
      return boundFn;
    }
  };
  return adjDescriptor;
}

// Component Base Class
abstract class Component<T extends HTMLElement, U extends HTMLElement> {
  templateElement: HTMLTemplateElement;
  hostElement: T;
  element: U;

  constructor(
    templateId: string,
    hostElementId: string,
    insertAtStart: boolean,
    newElementId?: string
  ) {
    this.templateElement = document.getElementById(
      templateId
    )! as HTMLTemplateElement;
    this.hostElement = document.getElementById(hostElementId)! as T;

    const importedNode = document.importNode(
      this.templateElement.content,
      true
    );
    this.element = importedNode.firstElementChild as U;
    if (newElementId) {
      this.element.id = newElementId;
    }

    this.attach(insertAtStart);
  }

  private attach(insertAtBeginning: boolean) {
    this.hostElement.insertAdjacentElement(
      insertAtBeginning ? 'afterbegin' : 'beforeend',
      this.element
    );
  }

  abstract configure(): void;
  abstract renderContent(): void;
}

// ProjectItem Class
class ProjectItem extends Component<HTMLUListElement, HTMLLIElement>
  implements Draggable {
  private project: Project;

  get persons() {
    if (this.project.people === 1) {
      return '1 person';
    } else {
      return `${this.project.people} persons`;
    }
  }

  constructor(hostId: string, project: Project) {
    super('single-project', hostId, false, project.id);
    this.project = project;

    this.configure();
    this.renderContent();
  }

  @autobind
  dragStartHandler(event: DragEvent) {
    event.dataTransfer!.setData('text/plain', this.project.id);
    event.dataTransfer!.effectAllowed = 'move';
  }

  dragEndHandler(_: DragEvent) {
    console.log('DragEnd');
  }

  configure() {
    this.element.addEventListener('dragstart', this.dragStartHandler);
    this.element.addEventListener('dragend', this.dragEndHandler);
  }

  renderContent() {
    this.element.querySelector('h2')!.textContent = this.project.title;
    this.element.querySelector('h3')!.textContent = this.persons + ' assigned';
    this.element.querySelector('p')!.textContent = this.project.description;
  }
}

// ProjectList Class
class ProjectList extends Component<HTMLDivElement, HTMLElement>
  implements DragTarget {
  assignedProjects: Project[];

  constructor(private type: 'active' | 'finished') {
    super('project-list', 'app', false, `${type}-projects`);
    this.assignedProjects = [];

    this.configure();
    this.renderContent();
  }

  @autobind
  dragOverHandler(event: DragEvent) {
    if (event.dataTransfer && event.dataTransfer.types[0] === 'text/plain') {
      event.preventDefault();
      const listEl = this.element.querySelector('ul')!;
      listEl.classList.add('droppable');
    }
  }

  @autobind
  dropHandler(event: DragEvent) {
    const prjId = event.dataTransfer!.getData('text/plain');
    projectState.moveProject(
      prjId,
      this.type === 'active' ? ProjectStatus.Active : ProjectStatus.Finished
    );
  }

  @autobind
  dragLeaveHandler(_: DragEvent) {
    const listEl = this.element.querySelector('ul')!;
    listEl.classList.remove('droppable');
  }

  configure() {
    this.element.addEventListener('dragover', this.dragOverHandler);
    this.element.addEventListener('dragleave', this.dragLeaveHandler);
    this.element.addEventListener('drop', this.dropHandler);

    projectState.addListener((projects: Project[]) => {
      const relevantProjects = projects.filter(prj => {
        if (this.type === 'active') {
          return prj.status === ProjectStatus.Active;
        }
        return prj.status === ProjectStatus.Finished;
      });
      this.assignedProjects = relevantProjects;
      this.renderProjects();
    });
  }

  renderContent() {
    const listId = `${this.type}-projects-list`;
    this.element.querySelector('ul')!.id = listId;
    this.element.querySelector('h2')!.textContent =
      this.type.toUpperCase() + ' PROJECTS';
  }

  private renderProjects() {
    const listEl = document.getElementById(
      `${this.type}-projects-list`
    )! as HTMLUListElement;
    listEl.innerHTML = '';
    for (const prjItem of this.assignedProjects) {
      new ProjectItem(this.element.querySelector('ul')!.id, prjItem);
    }
  }
}

// ProjectInput Class
class ProjectInput extends Component<HTMLDivElement, HTMLFormElement> {
  titleInputElement: HTMLInputElement;
  descriptionInputElement: HTMLInputElement;
  peopleInputElement: HTMLInputElement;

  constructor() {
    super('project-input', 'app', true, 'user-input');
    this.titleInputElement = this.element.querySelector(
      '#title'
    ) as HTMLInputElement;
    this.descriptionInputElement = this.element.querySelector(
      '#description'
    ) as HTMLInputElement;
    this.peopleInputElement = this.element.querySelector(
      '#people'
    ) as HTMLInputElement;
    this.configure();
  }

  configure() {
    this.element.addEventListener('submit', this.submitHandler);
  }

  renderContent() {}

  private gatherUserInput(): [string, string, number] | void {
    const enteredTitle = this.titleInputElement.value;
    const enteredDescription = this.descriptionInputElement.value;
    const enteredPeople = this.peopleInputElement.value;

    const titleValidatable: Validatable = {
      value: enteredTitle,
      required: true
    };
    const descriptionValidatable: Validatable = {
      value: enteredDescription,
      required: true,
      minLength: 5
    };
    const peopleValidatable: Validatable = {
      value: +enteredPeople,
      required: true,
      min: 1,
      max: 5
    };

    if (
      !validate(titleValidatable) ||
      !validate(descriptionValidatable) ||
      !validate(peopleValidatable)
    ) {
      alert('Invalid input, please try again!');
      return;
    } else {
      return [enteredTitle, enteredDescription, +enteredPeople];
    }
  }

  private clearInputs() {
    this.titleInputElement.value = '';
    this.descriptionInputElement.value = '';
    this.peopleInputElement.value = '';
  }

  @autobind
  private submitHandler(event: Event) {
    event.preventDefault();
    const userInput = this.gatherUserInput();
    if (Array.isArray(userInput)) {
      const [title, desc, people] = userInput;
      projectState.addProject(title, desc, people);
      this.clearInputs();
    }
  }
}

const prjInput = new ProjectInput();
const activePrjList = new ProjectList('active');
const finishedPrjList = new ProjectList('finished');


/* index.html */
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>ProjectManager</title>
    <link rel="stylesheet" href="app.css" />
    <script src="dist/app.js" defer></script>
  </head>
  <body>
    <template id="project-input">
      <form>
        <div class="form-control">
          <label for="title">Title</label>
          <input type="text" id="title" />
        </div>
        <div class="form-control">
          <label for="description">Description</label>
          <textarea id="description" rows="3"></textarea>
        </div>
        <div class="form-control">
          <label for="people">People</label>
          <input type="number" id="people" step="1" min="0" max="10" />
        </div>
        <button type="submit">ADD PROJECT</button>
      </form>
    </template>
    <template id="single-project">
      <li draggable="true">
        <h2></h2>
        <h3></h3>
        <p></p>
      </li>
    </template>
    <template id="project-list">
      <section class="projects">
        <header>
          <h2></h2>
        </header>
        <ul></ul>
      </section>
    </template>
    <div id="app"></div>
  </body>
</html>

```

### Modulization (namespace)
```js
/* app.ts */
/// <reference path="components/project-input.ts" />
/// <reference path="components/project-list.ts" />

namespace App {
  new ProjectInput();
  new ProjectList('active');
  new ProjectList('finished');
}

/* base-component.ts */
// Component Base Class
namespace App {
  export abstract class Component<T extends HTMLElement, U extends HTMLElement> {
    templateElement: HTMLTemplateElement;
    hostElement: T;
    element: U;

    constructor(
      templateId: string,
      hostElementId: string,
      insertAtStart: boolean,
      newElementId?: string
    ) {
      this.templateElement = document.getElementById(
        templateId
      )! as HTMLTemplateElement;
      this.hostElement = document.getElementById(hostElementId)! as T;

      const importedNode = document.importNode(
        this.templateElement.content,
        true
      );
      this.element = importedNode.firstElementChild as U;
      if (newElementId) {
        this.element.id = newElementId;
      }

      this.attach(insertAtStart);
    }

    private attach(insertAtBeginning: boolean) {
      this.hostElement.insertAdjacentElement(
        insertAtBeginning ? 'afterbegin' : 'beforeend',
        this.element
      );
    }

    abstract configure(): void;
    abstract renderContent(): void;
  }
}

/* project-input.ts */
/// <reference path="base-component.ts" />
/// <reference path="../decorators/autobind.ts" />
/// <reference path="../util/validation.ts" />
/// <reference path="../state/project-state.ts" />

namespace App {
  // ProjectInput Class
  export class ProjectInput extends Component<HTMLDivElement, HTMLFormElement> {
    titleInputElement: HTMLInputElement;
    descriptionInputElement: HTMLInputElement;
    peopleInputElement: HTMLInputElement;

    constructor() {
      super('project-input', 'app', true, 'user-input');
      this.titleInputElement = this.element.querySelector(
        '#title'
      ) as HTMLInputElement;
      this.descriptionInputElement = this.element.querySelector(
        '#description'
      ) as HTMLInputElement;
      this.peopleInputElement = this.element.querySelector(
        '#people'
      ) as HTMLInputElement;
      this.configure();
    }

    configure() {
      this.element.addEventListener('submit', this.submitHandler);
    }

    renderContent() {}

    private gatherUserInput(): [string, string, number] | void {
      const enteredTitle = this.titleInputElement.value;
      const enteredDescription = this.descriptionInputElement.value;
      const enteredPeople = this.peopleInputElement.value;

      const titleValidatable: Validatable = {
        value: enteredTitle,
        required: true
      };
      const descriptionValidatable: Validatable = {
        value: enteredDescription,
        required: true,
        minLength: 5
      };
      const peopleValidatable: Validatable = {
        value: +enteredPeople,
        required: true,
        min: 1,
        max: 5
      };

      if (
        !validate(titleValidatable) ||
        !validate(descriptionValidatable) ||
        !validate(peopleValidatable)
      ) {
        alert('Invalid input, please try again!');
        return;
      } else {
        return [enteredTitle, enteredDescription, +enteredPeople];
      }
    }

    private clearInputs() {
      this.titleInputElement.value = '';
      this.descriptionInputElement.value = '';
      this.peopleInputElement.value = '';
    }

    @autobind
    private submitHandler(event: Event) {
      event.preventDefault();
      const userInput = this.gatherUserInput();
      if (Array.isArray(userInput)) {
        const [title, desc, people] = userInput;
        projectState.addProject(title, desc, people);
        this.clearInputs();
      }
    }
  }

}

/* project-item.ts */
/// <reference path="base-component.ts" />
/// <reference path="../decorators/autobind.ts" />
/// <reference path="../models/project.ts" />
/// <reference path="../models/drag-drop.ts" />

namespace App {
  // ProjectItem Class
  export class ProjectItem extends Component<HTMLUListElement, HTMLLIElement>
    implements Draggable {
    private project: Project;

    get persons() {
      if (this.project.people === 1) {
        return '1 person';
      } else {
        return `${this.project.people} persons`;
      }
    }

    constructor(hostId: string, project: Project) {
      super('single-project', hostId, false, project.id);
      this.project = project;

      this.configure();
      this.renderContent();
    }

    @autobind
    dragStartHandler(event: DragEvent) {
      event.dataTransfer!.setData('text/plain', this.project.id);
      event.dataTransfer!.effectAllowed = 'move';
    }

    dragEndHandler(_: DragEvent) {
      console.log('DragEnd');
    }

    configure() {
      this.element.addEventListener('dragstart', this.dragStartHandler);
      this.element.addEventListener('dragend', this.dragEndHandler);
    }

    renderContent() {
      this.element.querySelector('h2')!.textContent = this.project.title;
      this.element.querySelector('h3')!.textContent =
        this.persons + ' assigned';
      this.element.querySelector('p')!.textContent = this.project.description;
    }
  }

}

/* project-list.ts */
/// <reference path="base-component.ts" />
/// <reference path="../decorators/autobind.ts" />
/// <reference path="../state/project-state.ts" />
/// <reference path="../models/project.ts" />
/// <reference path="../models/drag-drop.ts" />

namespace App {
  // ProjectList Class
  export class ProjectList extends Component<HTMLDivElement, HTMLElement>
    implements DragTarget {
    assignedProjects: Project[];

    constructor(private type: 'active' | 'finished') {
      super('project-list', 'app', false, `${type}-projects`);
      this.assignedProjects = [];

      this.configure();
      this.renderContent();
    }

    @autobind
    dragOverHandler(event: DragEvent) {
      if (event.dataTransfer && event.dataTransfer.types[0] === 'text/plain') {
        event.preventDefault();
        const listEl = this.element.querySelector('ul')!;
        listEl.classList.add('droppable');
      }
    }

    @autobind
    dropHandler(event: DragEvent) {
      const prjId = event.dataTransfer!.getData('text/plain');
      projectState.moveProject(
        prjId,
        this.type === 'active' ? ProjectStatus.Active : ProjectStatus.Finished
      );
    }

    @autobind
    dragLeaveHandler(_: DragEvent) {
      const listEl = this.element.querySelector('ul')!;
      listEl.classList.remove('droppable');
    }

    configure() {
      this.element.addEventListener('dragover', this.dragOverHandler);
      this.element.addEventListener('dragleave', this.dragLeaveHandler);
      this.element.addEventListener('drop', this.dropHandler);

      projectState.addListener((projects: Project[]) => {
        const relevantProjects = projects.filter(prj => {
          if (this.type === 'active') {
            return prj.status === ProjectStatus.Active;
          }
          return prj.status === ProjectStatus.Finished;
        });
        this.assignedProjects = relevantProjects;
        this.renderProjects();
      });
    }

    renderContent() {
      const listId = `${this.type}-projects-list`;
      this.element.querySelector('ul')!.id = listId;
      this.element.querySelector('h2')!.textContent =
        this.type.toUpperCase() + ' PROJECTS';
    }

    private renderProjects() {
      const listEl = document.getElementById(
        `${this.type}-projects-list`
      )! as HTMLUListElement;
      listEl.innerHTML = '';
      for (const prjItem of this.assignedProjects) {
        new ProjectItem(this.element.querySelector('ul')!.id, prjItem);
      }
    }
  }
}

/* project.ts */
namespace App {
  export enum ProjectStatus {
    Active,
    Finished
  }

  export class Project {
    constructor(
      public id: string,
      public title: string,
      public description: string,
      public people: number,
      public status: ProjectStatus
    ) {}
  }
}

/* drag-drop.ts */
// Drag & Drop Interfaces
namespace App {
  export interface Draggable {
    dragStartHandler(event: DragEvent): void;
    dragEndHandler(event: DragEvent): void;
  }

  export interface DragTarget {
    dragOverHandler(event: DragEvent): void;
    dropHandler(event: DragEvent): void;
    dragLeaveHandler(event: DragEvent): void;
  }
}

/* project-state.ts */
namespace App {
  // Project State Management
  type Listener<T> = (items: T[]) => void;

  class State<T> {
    protected listeners: Listener<T>[] = [];

    addListener(listenerFn: Listener<T>) {
      this.listeners.push(listenerFn);
    }
  }

  export class ProjectState extends State<Project> {
    private projects: Project[] = [];
    private static instance: ProjectState;

    private constructor() {
      super();
    }

    static getInstance() {
      if (this.instance) {
        return this.instance;
      }
      this.instance = new ProjectState();
      return this.instance;
    }

    addProject(title: string, description: string, numOfPeople: number) {
      const newProject = new Project(
        Math.random().toString(),
        title,
        description,
        numOfPeople,
        ProjectStatus.Active
      );
      this.projects.push(newProject);
      this.updateListeners();
    }

    moveProject(projectId: string, newStatus: ProjectStatus) {
      const project = this.projects.find(prj => prj.id === projectId);
      if (project && project.status !== newStatus) {
        project.status = newStatus;
        this.updateListeners();
      }
    }

    private updateListeners() {
      for (const listenerFn of this.listeners) {
        listenerFn(this.projects.slice());
      }
    }
  }

  export const projectState = ProjectState.getInstance();
}

/* validation.ts */
namespace App {
  // Validation
  export interface Validatable {
    value: string | number;
    required?: boolean;
    minLength?: number;
    maxLength?: number;
    min?: number;
    max?: number;
  }

  export function validate(validatableInput: Validatable) {
    let isValid = true;
    if (validatableInput.required) {
      isValid =
        isValid && validatableInput.value.toString().trim().length !== 0;
    }
    if (
      validatableInput.minLength != null &&
      typeof validatableInput.value === 'string'
    ) {
      isValid =
        isValid && validatableInput.value.length >= validatableInput.minLength;
    }
    if (
      validatableInput.maxLength != null &&
      typeof validatableInput.value === 'string'
    ) {
      isValid =
        isValid && validatableInput.value.length <= validatableInput.maxLength;
    }
    if (
      validatableInput.min != null &&
      typeof validatableInput.value === 'number'
    ) {
      isValid = isValid && validatableInput.value >= validatableInput.min;
    }
    if (
      validatableInput.max != null &&
      typeof validatableInput.value === 'number'
    ) {
      isValid = isValid && validatableInput.value <= validatableInput.max;
    }
    return isValid;
  }
}

/* autobind.ts */
namespace App {
   // autobind decorator
   export function autobind(_: any, _2: string, descriptor: PropertyDescriptor) {
    const originalMethod = descriptor.value;
    const adjDescriptor: PropertyDescriptor = {
      configurable: true,
      get() {
        const boundFn = originalMethod.bind(this);
        return boundFn;
      }
    };
    return adjDescriptor;
  }
}

/* index.html */
<script src="dist/bundle.js" defer></script>

/* tsconfig.json*/
"compilerOptions": {
	"module": "amd",
	"outFile": "./dist/bundle.js", 
	"outDir": "./dist",
	"rootDir": "./src"
}

```

### Modulization (es6 module)
```js
/* app.ts */
import { ProjectInput } from './components/project-input.js';
import { ProjectList } from './components/project-list.js';

new ProjectInput();
new ProjectList('active');
new ProjectList('finished');

/* base-component.ts */
// Component Base Class
export abstract class Component<T extends HTMLElement, U extends HTMLElement> {}

/* project-input.ts */
import { Component } from './base-component.js';
import { Validatable, validate } from '../util/validation.js';
import { autobind } from '../decorators/autobind.js';
import { projectState } from '../state/project-state.js';

// ProjectInput Class
export class ProjectInput extends Component<HTMLDivElement, HTMLFormElement> {}

/* project-item.ts */
import { Draggable } from '../models/drag-drop.js';
import { Project } from '../models/project.js';
import { Component } from './base-component.js';
import { autobind } from '../decorators/autobind.js';

// ProjectItem Class
export class ProjectItem extends Component<HTMLUListElement, HTMLLIElement> implements Draggable {}

/* project-list.ts */
import { DragTarget } from '../models/drag-drop.js';
import { Project, ProjectStatus } from '../models/project.js';
import { Component } from './base-component.js';
import { autobind } from '../decorators/autobind.js';
import { projectState } from '../state/project-state.js';
import { ProjectItem } from './project-item.js';

/* project.ts */
export enum ProjectStatus {}

export class Project {}

/* drag-drop.ts */
// Drag & Drop Interfaces

export interface Draggable {}

export interface DragTarget {}

/* project-state.ts */
import { Project, ProjectStatus } from '../models/project.js';

// Project State Management
type Listener<T> = (items: T[]) => void;

class State<T> {}

export class ProjectState extends State<Project> {}

export const projectState = ProjectState.getInstance();

/* validation.ts */
// Validation
export interface Validatable {}

export function validate(validatableInput: Validatable) {}

/* autobind.ts */
// autobind decorator
export function autobind(_: any, _2: string, descriptor: PropertyDescriptor) {}

/* index.html */
<script type="module" src="dist/app.js"></script>

/* tsconfig.json*/
"compilerOptions": {
	"module": "es2015",
	"outDir": "./dist"
}

```
### References
- [Understanding TypeScript](https://www.eduonix.com/understanding-typescript)

