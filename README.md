# React Reference

## Table of Contents
* [React Basics](#react-basics)
* [Events](#events)
* [Forms](#forms)

## <a name"react-basics"></a>React Basics
### Arrays

#### Lists
Using map(), you can render an array as JSX elements. Always use a key attribute when creating lists.
```javascript
class NameList extends Component {
  const names = ['donna', 'beverly', 'claire', 'beatrice', 'meg', 'bonita'];
  return (
    <ul>
      {names.map((name) =>
        <li key={name.toString()}> // Add key value to every item inside `map`
            value={name}
        </li>
      )}
    </ul>
  );
}
```
Ideally you will build lists that use unique IDs as keys:
```javascript
const todoItems = todos.map((todo) =>
  <li key={todo.id}>
    {todo.text}
  </li>
);
```
However, if that's not possible, you can use the items index. This is not recommended as indices may change:
```JavaScript
const todoItems = todos.map((todo, index) =>
  // Only do this if items have no stable IDs
  <li key={index}>
    {todo.text}
  </li>
);
```

---
### Components
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
#### return()
What is returned when that component (function) is called.

#### State
Similar to props, but it is private and fully controlled by the component.

##### Lifecycle Methods
Help to ensure recurring events don't take up too many resources.
###### componentDidMount()
This runs after the component has been rendered to the DOM. A good place for a timer.
```javascript
componentDidMount() {
  this.timerID = setInterval(
        () => this.tick(),
        1000
      );
}
```
###### componentWillUnmount()
```javascript
componentWillUnmount() {
  clearInterval(this.timerID);
}
```
##### setState()
* Never modify state directly, always use  setState().
* State calls may be asynchronous.

---
### Elements
Essentially a JS variable. The individual component that holds JSX and gets rendered.  
```JavaScript
const header = <h1>Howdy there</h1>;
```
Elements can also represent user-defined components:
```JavaScript
const header = <Welcome name="Michael" />;
```

---
### <a name="events"></a>Events

#### Toggle buttons
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

#### Passing arguments
Two methods for passing extra arguments
```javascript
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

---
### Forms<a name='forms'></a>

#### Controlled Components
A way of letting JS handle form data (rather than the standard HTML submit). It allows React to override the default HTML behavior. To do this set the form's input value and onChange like so:
```javascript
<input value={this.state.value} onChange={this.handleChange} />
```
Then you can handle the setState() in `handleChange()`.

##### textarea
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

##### select
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

---
### JSX
A mix between HTML and JavaScript.  
```javascript
<h1>Hello {name}!</h1>;
```
