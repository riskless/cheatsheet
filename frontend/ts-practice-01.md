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

### References
- [Understanding TypeScript](https://www.eduonix.com/understanding-typescript)

