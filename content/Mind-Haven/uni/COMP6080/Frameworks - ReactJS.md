




## Transpilation

compilation: source code -> machine code (C++ -> Machine code)
transpilation: source code -> source code (C++ -> Javascript)

### Other language -> Javascript
javascript is the only natively supported language in the browser, this means that by default if you want to use a different language to write your frontend web apps, you need to transpile to JS

*interesting* : compiling new -> old javascript https://babeljs.io/repl
typescriptlang.org/play


### JSX transformation
react is just javascript, it doesn't use string or HTML templating
under the hood when you write something like component = `<div><button /></div>` which actually is `component = React.createElement("div", null, React.createElement("button", null));`

### Minification + Obfuscation
javascript minification and/or obfuscation are both technically types of transpilation

minification/obfuscation takes our existing code and condenses it, removing whitespace, removing commends, shortening variable names and in some cases even automatically refactoring control flow to save network transfer when downloading scripts
https://closure-compiler.appspot.com/home

## useEffect

### Methods
1. Outside the component
```javascript

function App() {
	const [seconds, setSeconds] = React.useState(0);

	React.useEffect(() => {
		return window.setInterval(() => {
			setSeconds(s => s + 1);
		}, 1000);
	}, []);  // The dependency array specifies which props/state variables will trigger the effecthook if they get changed
	
	const reset = () => setSeconds(0);

	return (
		<section className="App">
			<div className="Seconds">
				<p>Seconds Passed: < /p>
				<p>{seconds} < /p>
			< /div>
			<button onClick={reset}< /button>
		< /section>
	);
}
```

#### Building a `useEffect` Hook

1st param - function to run







