# My-note2

## Wk5 -API
App.jsx
```
import React from 'react'
import Urban from './Urban' 
import { getDefinition } from '../api/urban';



class App extends React.Component {

  constructor(props) {
    super(props)

    this.state = {
      name : null
    }

    this.handleChange = this.handleChange.bind(this)
    this.loadDefinition=this.loadDefinition.bind(this)
  }


  handleChange(event){
    this.setState({ name: event.target.value })
  }


  loadDefinition(){
    return getDefinition(this.state.name)
    .then((data) => {this.setState({name:data})})
  }


  render(){
  return (
    <React.Fragment>
    <Urban name={this.state.name} />
    <form onSubmit={event => event.preventDefault()}>
       <label><input type="text" name="text" onChange={this.handleChange}/></label>
       <button onClick={this.loadDefinition}>Submit</button>
    </form>
    </React.Fragment>
  )
  }
}

export default App
```
Urban.jsx
```

import React from 'react'

class Urban extends React.Component {
  constructor(props) {
    super(props)

  }

  render () {
    return(
    <div>
      {this.props.name && (
        <div>
          <h2>{this.props.name.word}</h2>
          <p>Definition: {this.props.name.definition}</p>
          <p>Example: {this.props.name.example}</p>
        </div>
        )}
     
    </div>
    )
  }
}

export default Urban
```
api/urban.js
```
import request from 'superagent'
const apiEndpoint = ("https://mashape-community-urban-dictionary.p.mashape.com/define?term=")

export function getDefinition(name) {
  return request 
  .get(apiEndpoint + name).set('x-mashape-key', 'IUU7GyrOK4mshNOCbJEmBmj5SKSop1kSmPJjsn2nWIxgxzZbEc').then(res => {
    return res.body.list[0]
  })
}
```
## Wk5 -REACT
App.jsx
```
import React from "react";
import Wombats from "./Wombats";
import NewWombatForm from "./NewWombatForm";

class App extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      wombats: ["sam", "tim"]
    };

    this.addWombat = this.addWombat.bind(this);
  }

  addWombat(wombat) {
    let newWombats = [...this.state.wombats];
    newWombats.push(wombat);

    this.setState({
      wombats: newWombats
    });
  }
  render() {
    return (
      <React.Fragment>
        <h1>HELLO</h1>
        <Wombats wombats={this.state.wombats} />
        <NewWombatForm addWombat={this.addWombat} />
      </React.Fragment>
    );
  }
}

export default App;
```
NewWombatForm.jsx
```
import React from "react";
import Wombat from "./Wombat";

class NewWombatForm extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      wombat: ""
    };

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }
  handleChange(event) {
    this.setState({
      [event.target.name]: event.target.value
    });
  }

  handleSubmit(event) {
    event.preventDefault();
    this.props.addWombat(this.state.wombat);
    this.setState({ wombat: "" });
  }

  render() {
    return (
      <div>
        <form onSubmit={this.handleSubmit}>
          <input
            type="text"
            name="wombat"
            value={this.state.wombat}
            onChange={this.handleChange}
          />
          <input type="submit" value="Save" />
        </form>
      </div>
    );
  }
}

export default NewWombatForm;
```
Wombats.jsx
```
import React from "react";
import Wombat from "./Wombat";

class Wombats extends React.Component {
  render() {
    return (
      this.props.wombats.length > 0 && (
        <ul>
          {this.props.wombats.map((wombat, index) => {
            return <Wombat key={"wombat-${index}"} wombat={wombat} />;
          })}
        </ul>
      )
    );
  }
}

export default Wombats;
```
 Wombat.jsx
 ```
 import React from "react";

class Wombat extends React.Component {
  render() {
    return (
      <React.Fragment>
        <li>{this.props.wombat}</li>
      </React.Fragment>
    );
  }
}

export default Wombat;
```
## Wk5 -REACT,REDUX
clients/index.js
```
import React from "react";
import ReactDOM from "react-dom";
import { createStore } from "redux";
import { Provider } from "react-redux";

import App from "./components/App";
import reducers from "./reducers";

const store = createStore(
  reducers,
  window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
);

document.addEventListener("DOMContentLoaded", () => {
  ReactDOM.render(
    <Provider store={store}>
      <App />
    </Provider>,
    document.getElementById("app")
  );
});
```
App.jsx
```
import { connect } from "react-redux";

import Wombats from "./Wombats";
import NewWombatForm from "./NewWombatForm";

import { addWombat } from "../actions";

const App = props => {
  return (
    <React.Fragment>
      <h1>HELLO</h1>
      <Wombats wombats={this.props.wombats} />
      <NewWombatForm
        addWombat={wombat => this.props.dispatch(addWombat(wombat))}
      />
    </React.Fragment>
  );
};

function mapStateToProps(state) {
  return {
    wombats: state.wombats
  };
}
export default connect(mapStateToProps)(App);
```
reducers/index.js
```
import { combineReducers } from "redux";
import wombats from "./wombats";

const reducers = combineReducers({
  wombats
});

export default reducers;
```
reducers/wombats.js
```
const initialState = ["sam", "tim"];

const reducer = (state = initialState, action) => {
  switch (action.type) {
    case "ADD_WOMBAT":
      return [...state, action.wombat];
    default:
      return state;
  }
  return state;
};

export default reducer;
```
actions/index.js
```
export function addWombat(wombat) {
  return {
    type: "ADD_WOMBAT",
    wombat
  };
}
```
