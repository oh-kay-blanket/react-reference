# Components

<!-- TOC -->
- [General](#general)
  - [File Structure](#file-structure)
    - [Containers & Component Architecture](#containers--component-architecture)
  - [Function Component](#function-component)
  - [Class Components](#class-components)
- [Props](#props)
  - [Default Props](#default-props)
  - [Prop Types](#prop-types)
- [Class Methods](#class-methods)
- [Children](#children)
- [Mapping Components](#mapping-components)
- [State](#state)
  - [Passing State](#passing-state)
  - [Handling Events](#handling-events)
  - [Changing State](#changing-state)
    - [`setState()`](#setstate)
    - [Passing Arguments](#passing-arguments)
- [Lifecycle Methods](#lifecycle-methods)
  - [`render()`](#render)
  - [`componentDidMount()`](#componentdidmount)
  - [`componentDidUpdate()`](#componentdidupdate)
  - [`getDerivedStateFromProps(props, state)`](#getderivedstatefrompropsprops-state)
  - [`getSnapshotBeforeUpdate()`](#getsnapshotbeforeupdate)
  - [`componentWillReceiveProps(nextProps)` (depreciated method)](#componentwillreceivepropsnextprops-depreciated-method)
  - [`shouldComponentUpdate(nextProps, nextState)`](#shouldcomponentupdatenextprops-nextstate)
  - [`componentWillUnmount()`](#componentwillunmount)
- [Conditional Rendering](#conditional-rendering)
  - [Ternary Conditionals](#ternary-conditionals)

<!-- TOC END -->

# General

## File Structure
* One component per file.
* File name should be an exact match for component name.
* Generally, components should be placed into at least one sub-folder.
* User Upper Camel Case for component names.

### Containers & Component Architecture
A methodology for separating business logic from presentation rendering. Rather than have one large component with long handle...() methods and many <html> tags, split it into two components and pass the state as props. The business component would be the "container" and the presentation area would be the "component".


## Function Component
Useful when state is not kept. Cleaner text. Better performance.

```JavaScript
import React from 'react'

const Info = props => (
  <>
    <h1>About {props.title}</h1>
    <p>And some more info here</p>
  </>
)

export default Info
```


## Class Components
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



# Props
Function components use `prop.property`, Class components use `this.props.property`.

## Default Props
A default props can by set with FunctionName.defaultProps = { key: value }  

As a function component:
```JavaScript
function Card(props) {
    const styles = {
        backgroundColor: props.cardColor,
        height: props.height,
        width: props.width
    }

    return (
        <div style={styles}></div>
    )
}

Card.defaultProps = {
    cardColor: "blue",
    height: 100,
    width: 100
}
```

As a class component:
```JavaScript
class Card extends Component {
  static defaultProps = {
    cardColor: "blue",
    height: 100,
    width: 100
  }

  render() {
    const styles = {
      backgroundColor: this.props.cardColor,
      height: this.props.height,
      width: this.props.width
    }

    return (
      <div style={styles}></div>
    )
  }
}
```


## Prop Types
A good way to validate incoming data. Now a separate library. Useful only for development. Gives a warning when listed type is not met. Can also set required props.

[Docs](https://reactjs.org/docs/typechecking-with-proptypes.html#proptypes)

`npm i prop-types`

```JavaScript
import React from 'react'
import PropTypes from 'prop-types'

class Card extends Component {
  static defaultProps = {
    height: 100,
    width: 100
  }

  static propTypes = {
    cardColor: PropTypes.oneOf(['red', 'blue']).isRequired, // this will be a required prop
    height: PropTypes.number,
    width: PropTypes.number
  }

  render() {
    const styles = {
      backgroundColor: this.props.cardColor,
      height: this.props.height,
      width: this.props.width
    }

    return (
      <div style={styles}></div>
    )
  }
}
```


# Class Methods
If you use an arrow function to declare methods, you don't need to bind that method in the constructor
```javascript
class App extends React.Component {
  constructor() {
    super()
    this.state = {}
  }

  handleChange = event => {

  }
}
```



# Children
Normally, components are self-closing:
```javascript
const App () => (
  <>
    <CTA />
  </>
)

const CTA = () => (
  <>
    <h1>Howdy there</h1>
  </>
)
```

If a sub-component is used with open and closed tags, it can then accept children, which are accessible via `props.children`. This is handy when you might have a component that has a particular styling (such as a Call to Action), but might differ wildly in regards to what content it hosts.
```javascript
const App () => (
  <>
    <CTA>
      <h1>Howdy there</h1>
    </CTA>
  </>
)

const CTA = (props) => (
  <div className="cta">
    {props.children}
  </div>
)
```



# Mapping Components
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

## Passing State
State can be passed down through child components just like any other prop.

```javascript
import React, { Component } from 'react'

class Parent extends Component {
  constructor() { // this method is required when using state
    super()
    this.state = { // state is always an object
      answer: 'yes'
    }
  }

  render() {
    return <Child answer={this.state.answer} />
  }
}

class Child extends Component {
  constructor() {
    super()
  }

  render() {
    return <p>{this.props.answer}</p>
  }
}
```

## Handling Events
All event listeners will use camelCase:
* onClick
* onMouseOver
* onChange  

See all  [supported events](https://reactjs.org/docs/events.html#supported-events)


## Changing State
Can be done via `setState()` or Hooks. You should never change the state (prevState), but rather return a new object, which is a modification of prevState.

### `setState()`
```JavaScript
class App extends Component {
  constructor() {
    super()
    this.state = {
      count: 0
    }
  }

  // Put handle...() above render().
  handleClick = () => {
    // if you don't care what the previous state was, you can just pass a new object in.
    // this.setState({ count: 1 })

    // if the new state relies on the old state, you must pass a function as the parameter in `setState()`
    this.setState(prevState => {
      return { count: prevState.count + 1 }
    })
  }

  render() {
    return (
      <Child handleClick={this.handleClick} count={this.state.count} />
    )
  }
}

function Child(props) {
  <div>
    <h1>{props.count}</h1>
    <button onClick={props.handleClick}>Add</button> // for class methods, you must use `this`  beforehand.
  </div>
}

```


### Passing Arguments
If you call a method with arguments, it will fire upon page render
```javascript
<button onClick={handleClick(id)}>Click me</button> // This will fire upon rendering
```

Instead, create a new function, which returns the handler() method.
```javascript
// These will only fire when clicked
<button onClick={() => this.handleClick(id)}>Click me</button>
```



# Lifecycle Methods
Help to ensure recurring events don't take up too many resources. These work only on class components.

https://engineering.musefind.com/react-lifecycle-methods-how-and-when-to-use-them-2111a1b692b1
https://reactjs.org/blog/2018/03/29/react-v-16-3.html#component-lifecycle-changes

### `render()`
Affects how the component is displayed.


### `componentDidMount()`
Runs once, immediately after the component is mounted (rendered) to the screen. Will not run on subsequent mounts. Often `componentDidMount()` will process getting data from an API.

```javascript
componentDidMount() {
  this.timerID = setInterval(
    () => this.tick(),
    1000
  );
}
```


### `componentDidUpdate()`
Won't run on initial mount, like `componentDidMount()`, but will run on subsequent mounts, after render.

```JavaScript
componentDidUpdate() {

}
```


### `getDerivedStateFromProps(props, state)`
A static method. You mostly should not use this.


### `getSnapshotBeforeUpdate()`
Not commonly used. Takes a snapshot of state before change is made.


### `componentWillReceiveProps(nextProps)` (depreciated method)
This runs whenever the component receives props from a parent component. Runs the every time a parent component sends props to child. Can be useful for checking whether new props passed are different from existing props. If so, you can decide to run a process on those props.

```javascript
componentWillReceiveProps(nextProps) {
  if (nextProps.info !== this.info) {
    // process data
  }
}
```


### `shouldComponentUpdate(nextProps, nextState)`
Looks at update being pushed and decides whether or not the component actually needs to be updated. Generally, you won't use this and will instead use a `PureComponent`.

```javascript
shouldComponentUpdate(nextProps, nextState) {
    if (nextProps.count === this.props.count) {
        return false // won't update
    }
    return true // will update
}
```


### `componentWillUnmount()`
Can choose to remove components.

```javascript
componentWillUnmount() {
  // teardown or cleanup your code before your component disappears
  // remove event listener
}
```


# Conditional Rendering
There are many ways to do conditional rendering. There are some useful methods.

## Ternary Conditionals
```JavaScript
class App extends Component {
  constructor() {
    super()
    this.state = {
      isLoading: true,
      didMount: false
    }
  }

  componentDidMount() {
    setTimeout(() => {
      this.setState({ isLoading: false })}, 1500)
  }

  componentDidUpdate() {
    !this.state.didMount && // Without this, componentDidUpdate() will cause a re-render on infinite loop
    setTimeout(() => {
      this.setState({ didMount: true })}, 2500)
  }

  render() {
    return (
      <>
        {this.state.isLoading ? <h2>Loading...</h2> : <Conditional />}
        {this.state.didMount && <p>`Pssst...we're all done here.`}
      </>
    )
  }
}

function Conditional(props) {
  return <h2>Here is that data you wanted</h2>
}
```
