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

Example component:
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

Essentially a JS function. It accepts `props` (properties). Props are read only, and should not be modified by the component.
```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}<h1>;
}
```
Normally you will use an ES6 class to define a component:

```javascript
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

# Arrays

## `map()`
Renders an array as JSX elements. Always use a key attribute when creating lists.
```javascript
class NameList extends Component {
  const girls = ['Monica in my life', 'Erica by my side', 'Rita is all I need', 'Tina is what I see', 'Sandra in the sun', 'Mary all night long', 'Jessica, here I am', 'You makes me a man']
  const girlElements = girls.map(girl =>
    <li key={girl.toString()}>
      A little bit of {girl}
    </li>
  )

  return (
    <ul>
      {girlElements}
    </ul>
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



# Events

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
