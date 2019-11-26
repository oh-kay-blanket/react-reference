# Components

<!-- TOC -->
- [General](#general)
  - [File Structure](#file-structure)
- [Arrays](#arrays)
  - [`map()`](#map)
- [State](#state)
- [Events](#events)
  - [Toggle buttons](#toggle-buttons)
  - [Passing arguments](#passing-arguments)
- [Forms](#forms)
  - [Controlled Components](#controlled-components)
    - [textarea](#textarea)
    - [select](#select)
- [Lifecycle Methods](#lifecycle-methods)
  - [`componentDidMount()`](#componentdidmount)
  - [`componentWillUnmount()`](#componentwillunmount)
  - [`setState()`](#setstate)

<!-- TOC END -->

# General

## File Structure
* One component per file.
* File name should be an exact match for component name.
* Generally, components should be placed into at least one sub-folder.
* User Upper Camel Case for component names.

### Function Component
Useful when state is not kept. Cleaner text. Better performance.

```JavaScript
import React from 'react'

function Info(props) {
  return (
    <>
      <h1>About {props.title}</h1>
      <p>And some more info here</p>
    </>
  )
}

export default Info
```

### Class Components
Use this when you will need to manage state.  

```javascript
import React, { Component } from 'react'

class Welcome extends Component {

  // add other methods here

  render() { // render() method is required.

    const date = new Date() // display/style logic goes here

    return <h1>`Hello, ${this.props.name}. It is ${date}.`</h1>
  }
}
```



# Arrays

## `map()`
Renders an array as JSX elements. Always use a key attribute when creating lists.
```javascript

function NameList() {
  const girlsList = [
    {id: 1, name: 'Monica', preposition: 'in my life'},
    {id: 2, name: 'Erica', preposition: 'by my side'},
    {id: 3, name: 'Rita', preposition: 'is all I need'},
    {id: 4, name: 'Tina', preposition: 'is what I see'},
    {id: 5, name: 'Sandra', preposition: 'in the sun'},
    {id: 6, name: 'Mary', preposition: 'all night long'},
    {id: 7, name: 'Jessica', preposition: 'here I am'},
    {id: 8, name: 'You', preposition: 'makes me a man'}
  ]

  const girlsComponents = girlsList.map(girl => <Name key={girl.id} info={girl} />)

  return (
    <ul>
      {girlsComponents}
    </ul>
  )
}

function Name(props) {
  return (
    <li style={{listStyleType: 'none'}}>{`A little bit of ${props.info.name} ${props.info.preposition}`}</li>
  )
}
```

Ideally you will build lists that use unique IDs as keys:
```javascript
const todoItems = todos.map(todo =>
  <li key={todo.id}>
    {todo.text}
  </li>
)
```

However, if that's not possible, you can use the items index. This is not recommended as indices may change:
```JavaScript
const todoItems = todos.map((todo, index) =>
  // Only do this if items have no stable IDs
  <li key={index}>
    {todo.text}
  </li>
)
```


# State
Similar to props, but it is private and fully controlled by the component.

```JavaScript
import React, { Component } from 'react'

class App extends Component {
  constructor() { // this method is required when using state
    super()
    this.state = { // state is always an object
      answer: 'yes'
    }
  }

  render() {
    return (
      <div>
          <h1>`Is state important to know?`</h1>
          <h2>{this.state.answer}</h2>
      </div>
    )
  }
}
```


# Conditional Rendering



# Events
All event listeners will use camelCase:
* onClick
* onMouseOver
* onChange  

See all  [supported events](https://reactjs.org/docs/events.html#supported-events)


## Handling Event
Put handler method above render method.
```javascript
class App extends React.Component {
  constructor() {
    super()
  }

  handleClick() {
    alert("OUCH")
  }

  render() {
    return (<button onClick={this.handleClick}>Touch Me</button>)
  }
}
```


### Toggle buttons
You must use bind to use 'this' in the callback

```JavaScript
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // This binding is necessary to make `this` work in the callback
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(state => ({
      isToggleOn: !state.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
```


### Passing arguments
Two methods for passing extra arguments

```javascript
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```


# Forms

### Controlled Components
A way of letting JS handle form data (rather than the standard HTML submit). It allows React to override the default HTML behavior. To do this set the form's input value and onChange like so:
```javascript
<input value={this.state.value} onChange={this.handleChange} />
```
Then you can handle the setState() in `handleChange()`.

#### textarea
In HTML, the `<textarea>` text is handled as a child element.

```html
<textarea>
  Info here.
</textarea>
```

In React, it is handled as a value.
```javascript
<textarea value={this.state.value} onChange={this.handleChange} />
```

#### select
For a dropdown selection in HTML, the default selection is given a `selected` attribute.

```html
<select>
  <option value="red">Red</option>
  <option value="green">Green</option>
  <option selected value="yellow">Yellow</option>
</select>
```

In React, it is accessed as a value of the `<select>` element.
```javascript
<select value={this.state.value} onChange={this.handleChange}>
  <option value="red">Red</option>
  <option value="green">Green</option>
  <option value="yellow">Yellow</option>
</select>
```



# Lifecycle Methods
Help to ensure recurring events don't take up too many resources.

### `componentDidMount()`
This runs after the component has been rendered to the DOM. A good place for a timer.
```javascript
componentDidMount() {
  this.timerID = setInterval(
        () => this.tick(),
        1000
      );
}
```

### `componentWillUnmount()`

```javascript
componentWillUnmount() {
  clearInterval(this.timerID);
}
```

### `setState()`
* Never modify state directly, always use  setState().
* State calls may be asynchronous.
