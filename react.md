# React Getting Started

## Setting up Dev Env for React

### Auto config

- create-react-app
  - no need to worry, it's ready with all available
    tools mentioned above already
  - beginner friendly
  - provided by facebook (npm)
  - npm i -g create-react-app && create-react-app my-app
  - npx create-react-app my-app
  - to eject the application from the internal abstraction:
    `npm run eject` :: this action is permanent
  - this action copy all configs and scripts locally to the project

### Manual config (barebones / from scratch)

- Libs
  - Multiple tools
  - APIs
  - Configurations
- Envs
  - Releases
  - Development
  - Production
  - Test
- Rendering
  - DOM
  - SSR (Server Side Rendering)
    [For more recent Env setup](https://jscomplete.com/reactful)

#### Steps for React Env from Scratch (Full Stack)

1. `npm init -y` inside empty directory
2. Production Dependencies

- `npm i express`
- `npm i react react-dom`
- `npm i webpack webpack-cli`
- `npm i babel-loader @babel/core @babel/node @babel/preset-env @babel/preset-react`

3. Development Dependencies

- `npm i -D nodemon`
- `npm i -D eslint babel-eslint eslint-plugin-react eslint-plugin-react-hooks`
- `npm i -D jest babel-jest react-test-renderer`

4. Configuring .eslintrs.js
```
module.exports = {
  parser: 'babel-eslint',
  env: {
    browser: true,
    commonjs: true,
    es6: true,
    node: true,
    jest: true,
  },
  plugins: ['react-hooks', 'react'],
  extends: ['eslint:recommended', 'plugin:react/recommended'],
  parserOptions: {
    ecmaVersion: 2018,
    ecmaFeatures: {
      impliedStrict: true,
      jsx: true,
    },
    sourceType: 'module',
  },
  rules: {
    // You can do your customizations here...
    // For example, if you don't want to use the prop-types package,
    // you can turn off that recommended rule with: 'react/prop-types': ['off']
  },
};
```

5. Creating directory structure

```
newDir/
  dist/
    main.js
  src/
    index.js
    components/
      App.js
    server/
      server.js
```

6. Babel Configuration

```
module.exports = {
  presets: [ '@babel/preset-env', '@babel/preset-react'],
};
```

7. Webpack Configuration

```
module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
        },
      },
    ],
  },
};
```
8. Creating npm scripts for development

```
  // In package.json
  scripts: {
    "test": "jest",
    "dev-server": "nodemon --exec babel-node src/server/server.js --ignore dist/",
    "dev-bundle": "webpack -wd"
  }
```

9. Simple App

  - src/components/App.js
```
import React, { useState } from 'react';

export default function App() {
  const [count, setCount] = useState(0);
  return (
    <div>
      This is a sample stateful and server-side
      rendered React application.
      <br />
      <br />
      Here is a button that will track
      how many times you click it:
      <br />
      <br />
      <button onClick={() => setCount(count + 1)}>{count}</button>
    </div>
  );
}
```

  - src/index.js
```
import React from 'react';
import ReactDOM from 'react-dom';

import App from './components/App';

ReactDOM.hydrate(
  <App />,
  document.getElementById('mountNode'),
);
```
  - src/server/server.js
```
import express from 'express';
import React from 'react';
import ReactDOMServer from 'react-dom/server';
import App from '../components/App';

const server = express();
server.use(express.static('dist'));

server.get('/', (req, res) => {
  const initialMarkup = ReactDOMServer.renderToString(<App />);

  res.send(`
    <html>
      <head>
        <title>Sample React App</title>
      </head>
      <body>
        <div id="mountNode">${initialMarkup}</div>
        <script src="/main.js"></script>
      </body>
    </html>
  `)
});

server.listen(4242, () => console.log('Server is running...'));
```

10. Running the Application
  - `npm run dev-server`
  - `npm run dev-bundle`