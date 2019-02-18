# react-from-scratch
create-react-app hides the complications of creating a new react project and exposes a working app to the end user. This project is to understand the complexity of the easy react app!


1) Make sure you have installed:

    NodeJS
		node -v 
			v10.14.2
    npm
		npm -v 
			6.8.0

2) Libraries to set up react from scratch:

	We’ll be using webpack and babel to set up react, and I don’t want you to get confused.
    Babel is the compiler for next-generation JavaScript. It compiles newer JavaScript (ES6/7/8) to older ES5 standard to run the same on old and new browsers.
    Webpack is a module bundler. We’ll use multi-directory and multi-file approach for easy management of the project. Webpack will bundle all our files into one, offering better performance and easier dependency management.

3) In terminal provide following commands :

	mkdir react-from-scratch
	cd react-from-scratch
	npm init -y
		This creates a directory react-from-scratch and initializes the node project, the -y flag is used to skip all the questions with default answers.
	npm install react react-dom
		package.json holds information about name, dependencies and more scripts.
		What isreact and react-dom?
			react is the library that defines view components, the React components.
			react-dom is the library that creates the view. react-dom is equivalent to the Web DOM. It creates and displays the web page.
			The separation has allowed React to be used on multiple platforms with only changing the rendering library in place of react-dom. React Native renders for the iOS and the Android. ReactVR is for VR devices.

4) Initializing Webpack development server
	We have a way of creating and rendering React components now. We have yet to send these components to the browser to show them. This is what web server is used for.

	npm install webpack webpack-dev-server webpack-cli --save-dev

	The --save-dev flag saves these as the development dependencies. They won’t be a part of the final build that’s deployed on the server, they will be used for development process. webpack-cli is required to run the project from the terminal.
	webpack must be installed because webpack-dev-server depends on it. This dev-server will live-reload our app.

5) Creating a React app

	a) In the root directory, which I named react-from-scratch, create a new file index.html. This will be the main file served to the browser.

		<!DOCTYPE html>
		<html lang="en">
		<head>
			<meta charset="UTF-8">
			<title>ReactJS Sample Project</title>
		</head>
		<body>
		<div id="root"></div>
		<script type="text/javascript" src="bundle.js"></script>
		</body>
		</html>

		The react components will go in the div with id root.
		The script bundle.js will be created using webpack, which will contain all our react code, including react library and renderer, in proper format.

	b) Create a file index.js, with the following code

		import React from 'react';
		import {render} from 'react-dom';

		render(
		  React.createElement("div", null, "Hello World"),
		  document.getElementById("root")
		);

		React must be imported for a react app. render method is imported from react-dom using destructuring.
		render takes two arguments: first one is the component and second one is the location.
		React.CreateElement is a top-level React API. It creates elements, no JSX invloved.

		document.getElementById("root") is our location on index.html.

6) Setting up a Webpack development server

	webpack-dev-server will compile our code fine and server at localhost:8080 than it’ll fail to find ./src.This is because webpack is expecting index.js at the ./src/ . Either you can move the index.js to src or modify the package.json file or set up the entry file in the webpack config file. Last option is most preferred and we’ll use the same without moving .
	
	a) TYPE IN TERMINAL:
		mkdir webpack
		cd webpack
		touch webpack.dev.config.js

		Create a directory webpack with a file webpack.dev.config.js.
		
			var webpack = require('webpack');
			var path = require('path');

			var parentDir = path.join(__dirname, '../');

			module.exports = {
				entry: [
					path.join(parentDir, 'index.js')
				],
				module: {
					rules: [{
						test: /\.(js|jsx)$/,
							exclude: /node_modules/,
							loader: 'babel-loader'
						},{
							test: /\.less$/,
							loaders: ["style-loader", "css-loder", "less-loader"]
						}
					]
				},
				output: {
					path: parentDir + '/dist',
					filename: 'bundle.js'
				},
				devServer: {
					contentBase: parentDir,
					historyApiFallback: true
				}
			}

		It uses webpack as the dependency and sets entry point to index.js
		It contains a bunch of rules:
		Entry point
		index.js is the starting point for all scripts.
		The packages
			babel-loader for loading jsx files.
			less-loader for loading less files
			less-loader requires less as a peer dependency.

		
	b)Install all dependencies and peer dependencies with:

		npm install --save-dev style-loader css-loader less-loader less
		
7) Setting up babel
	We need babel to convert ES6 code to ES5. Install babel and supporting libraries with: 

	npm install --save-dev babel-cli babel-core babel-loader babel-plugin-transform-object-rest-spread babel-preset-es2015 babel-preset-react babel-preset-stage-0 babel-register
	npm install --save-dev @babel/core @babel/preset-env

8) Configure the react app to make use of babel in package.json.

	"babel": {
	  "presets": ["es2015", "react", "stage-0"],
	  "plugins": ["transform-object-rest-spread"]
	}

	It also uses a plugin to support rest/spread operator.

9) Make the changes to index.js, use App component instead of manually creating elements.

	import React from 'react'
	import { render } from 'react-dom'
	import App from './containers/App'

	render(<App />, document.getElementById('root'))

10)Create ./containers/App.js file to serve the App

	mkdir containers 
	cd containers
	touch App.js

11) App.js code:
	import React, {Component} from 'react';

	class App extends Component {
		render () {
			return <p>Hello from the App!</p>
		}
	}
	export default App

12) Inside the package.json  update the script:

	"scripts": {
	  "test": "echo \"Error: no test specified\" && exit 1",
	  "dev": "./node_modules/.bin/webpack-dev-server --config ./webpack/webpack.dev.config.js"
	}

	npm run dev will run the webpack-dev-server from the node modules with config file from ./webpack/webpack.dev.config.js.

13) npm run dev
