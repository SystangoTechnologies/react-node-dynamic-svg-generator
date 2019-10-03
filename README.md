## Problem(Why this repo is published ?)
There comes this case of customizing SVG according to our requirements and need to reuse the same SVG with different configurations. So to keep at frontend and customize according to out requirements will increase complexity and decrease performance and we can not re-use the same logics in different application or platform(in several client side apps). Also it makes no sense to serve millions of SVG’s via static files or via bucket storage. How about a solution where we can generate our SVG’s in run time & provide it to our client side applications which runs on all platforms
This project was bootstrapped with React, Node with Express framework.

## Solution to the Problem:
We can make any type of complex SVG as well using React and also we can use any npm libraries to make things easier. 

But the concern is We need to customize at backend and get customized SVG on frontend. React has no real DOM dependency, so you can render it on a server easily.

We will be using an API endpoint from Node Express app to get SVG from backend(Rendered via React DOM) using SSR.
Node Js + Express  - Provide an API with proper code structure and routings. 
React Js - Provide a customized SVG with using any NPM library as well!
So, We will be needing an approach that will utilize Node js, Express and React features.

## Demo URL:
https://react-node-svg-generator.herokuapp.com/api/svg?header=React%20Node&title=Dynamic&subHeader=SVG

Try changing these query params:
`header`
`title`
`subHeader`

You'll be able to see dyamic content rendering in SVG.

## Before installing, download and install Node.js. Node.js 0.10 or higher is required.

## Install dependencies
`npm install`

## Serve App on Dev Environment
`npm start`

Runs the app in the development mode.<br>
Open [http://localhost:51132/api/svg?header=React11%20Node&title=Dynamic&subHeader=SVG] to view it in the browser.

## Technology/Library Used
React
Node
Express
Babel

## SVG 
Scalable Vector Graphics (SVG) is an XML-based vector image format for two-dimensional graphics with support for interactivity and animation.
SVG supports JavaScript. Browsers will only run the JavaScript.

## React
React is a JavaScript library for building user interfaces.
React works with the DOM, So we can work with SVG with the same way that work with HTML without any difference.

## Node + Express
To render React app as a server side and Make an utility to serve dynamic generated SVG for any client (Mobile/Web).

## Let's Explore the Codebase Shared in this repo:

## server.js 
Initial file to run the backend code
We use simple Express app that handles incoming requests for SVG graphs (server.js):
```
require("babel-register");
const express = require('express');
const app = express();
const routes = require('./app/routes');
const bodyParser = require('body-parser')

app.use(bodyParser.urlencoded({ extended: false }))
app.use(bodyParser.json())
app.use(function(req, res, next) {
 res.header("Access-Control-Allow-Origin", "*"); //here configure your origin pointing to your app
 res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
 next();
});

app.use('/api', routes);
const server = app.listen(3006, function () {
 const host = server.address().address;
 const port = server.address().port;
 console.log('XML parser and SVG, JSON generator app listening at http://%s:%s', host, port);
});
```


## routes/index.js 
Contain all routes.
```
const express = require('express');
const router = express.Router();
const svgGenerator = require('../controllers/svgGenerator');

router.get('/svg', svgGenerator.getSVG); // this route returns the SVG generated from react app
module.exports = router;`

## controllers/svgGenerator.js
As you can see, only these lines are really from our application, rest server.js had generic boilerplate code for node express app:
`
const svgRenderer = require('../public/scripts/svg_renderer').default;

exports.getSVG = function(req, res) {
   const svg = svgRenderer(req.query);
   res.send(svg);
}
```

## public/scripts/svg_render.js 
To pass parameter to make dynamic SVG and return SVG to the express route.
```
import React from 'react';
import ReactDOMServer from 'react-dom/server';
import App from './app'
export default function(data) {
 return ReactDOMServer.renderToStaticMarkup(<App data={data} />);
}
```

## public/scripts/app.js 
In this script, we will create a configurable SVG.
```
import React from 'react';
export default class App extends React.Component {
 render() {
   const { data } = this.props;
   const { header, title, subHeader } = data;
   return (
     <svg version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg" xmlnsXlink="http://www.w3.org/1999/xlink" x="0px" y="0px"
       width="400px" height="327px" viewBox="0 0 400 327" enableBackground="new 0 0 400 327" xmlSpace="preserve">
       <rect fill="#DEF2FD" width="400" height="327" />
       <polygon opacity="0.2" fill="#7DACDC" enableBackground="new" points="0,327 200,173.797 400,327 " />
       <polygon opacity="0.2" fill="#7DACDC" enableBackground="new" points="91,327 291,280.292 491,327 " />
       <polygon opacity="0.2" fill="#7DACDC" enableBackground="new" points="-101,327 99,201 299,327 " />
       <g>
         <text transform="matrix(1 0 0 1 121.5 57.875)" fill="#0093C0" fontSize="29.7691">{ header ? header : "React Node" }</text>
         <text transform="matrix(1 0 0 1 38.5483 114.1104)" fill="#016082" fontSize="97.8796">{ title ? title : "Dynamic"}</text>
         <text transform="matrix(1 0 0 1 41.5005 141.4561)" fill="#0093C0" fontSize="14.5332" letterSpacing="140">{ subHeader ? subHeader : "SVG"}</text>
       </g>
     </svg>
   )
 }
}
```

## Now Run `node server.js`
To test it, we would run command:

Open : 
http://localhost:3000/api/svg?header=React%20Node&title=Dynamic&subHeader=SVG

## Conclusion
We successfully generated dynamic SVG and now we can easily reuse it with dynamic data at any platform(Server-side rendering!). Here we’re using custom data rendered from query params in the api call ‘/svg?header=React Node&title=Dynamic&subHeader=SVG’ , similarly we can configure and GET OR POST route with custom query params or request body to achieve dynamic rendering by passing necessary template/xml/json data.

Hope this will help you to achieve your requirements, you can easily change all the SVG configuration/generator logic at server end can easily change without breaking any external dependencies. We can also handle complex SVG with animations and custom styles, but this needs to be handled in your client side application where we are consuming this dynamic SVG api.

Also here we can even override the current `SVG` template, in [https://github.com/SystangoTechnologies/react-node-dynamic-svg-generator/tree/master/app/public/scripts/app.js], so there can be multiple cases that can be handled by forking this repo and customising it. 

Feel free to contact on vaibhav@systango.com incase of any help or suggestion.
