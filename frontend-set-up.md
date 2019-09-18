> Need to add:

## From a pre-existing project

- `npm install` to install module dependencies listed in package.json

## Project from scratch

### Github

- create new repo on github
- `git clone repoUrl`
- `cd` into folder
- `git add` & `git commit`
- `git push -u origin branchName`

### Node package manager

- `yarn create react-app client` or `npm create react-app client`
- `yarn start` or `npm start` to start up on localhost:3000
- Add dependencies:
  - `yarn add` or `npm i` the following:
    - axios, react-router-dom
- Delete these files:
  - serviceworker
  - logo
- create `components` folder

## Set up Router

- index.js

  ```js
  import { BrowserRouter as Router } from "react-router-dom";

  ReactDOM.render(
    <Router>
      <App />
    </Router>,
    document.getElementById("root")
  );
  ```
