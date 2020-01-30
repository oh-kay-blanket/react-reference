# Hooks

<!-- TOC -->
- [`useState()`](#usestate)
- [`useEffect()`](#useeffect)
- [`useRef()`](#useref)

<!-- TOC END -->

Hooks allow for using only functional components, even when dealing with lifecycle methods.

Hooks essentially replace the following:
* this.state
* Higher-Order Components
* Render Props


### `useState()`
Returns and sets stateful values.

```javascript
import React,{ useState } from 'react'

function App() {
  // const [state, setState] = useState(initialState)
  const [count, setCount] = useState(0)

  function increment() {
    setCount(prevCount => prevCount + 1)
  }

  function decrement() {
    setCount(prevCount => prevCount - 1)
  }

  return (
    <>
      <h2>{count}</h2>
      <button onClick={decrement}>Decrement</button>
      <button onClick={increment}>Increment</button>
    </>
  )
}
```

Using objects and arrays as data:
```JavaScript
import React,{ useState } from 'react'

function App() {
  const [inputData, setInputData] = useState({firstName: "", lastName: ""})
  const [contactsData, setContactsData] = useState([])

  function handleChange(event) {
    const {name, value} = event.target
    setInputData(prevInputData => ( // need to take in prev data
      ...prevInputData, // and copy it over using Object Spread Notation
      [name]: value // then assign changes
    ))
  }

  function handleSubmit(event) {
    event.preventDefault()
    setContactsData(prevContacts => [...prevContacts, inputData])
  }

  const contacts = contactsData.map(contact => <p>{contact.firstName} {contact.lastName}</p>)

  return (
    <>
      <form onSubmit={handleSubmit}>
        <input
          placeholder="First Name"
          name="firstName"
          value={inputData.firstName}
          onChange={handleChange}
        />
        <input
          placeholder="Last Name"
          name="lastName"
          value={inputData.lastName}
          onChange={handleChange}
        />
        <br />
        <button>Add contact</button>
      </form>
      {contacts}
    </>
  )
}
```


### `useEffect()`
Useful for creating side-effects based on a component prop/state. Use for network requests, manipulating parts of DOM outside of component. Anything you want to change that is outside of the component itself. Replaces `componentDidMount()`, `componentDidUpdate()`, and `componentWillUnmount()`.

`useEffect(() => { callbackFunction() }, [listenerArray])`

First parameter is a callback function.

Second parameter is an array, which listens for what variable(s) change should fire `useEffect()`.
* Placing an empty array will run `useEffect()` only on the first render, like `componentDidMount()`.
```javascript
useEffect(() => { setColor(randomcolor()) }, []) // this will run once
```

* Putting in the name of a variable will listen to changes in that variable and fire each time the variable change, similar to `componentDidUpdate()`.
```JavaScript
useEffect(() => { setColor(randomcolor()) }, [count]) // This will run each time `count` changes
```

* Placing a return statement in `useEffect()` will make it run the code inside the return when the component is unmounted, similar to `componentWillUnmount()`.
```javascript
useEffect(() => {
    const intervalId = setInterval(() => {
      setCount(prevCount => prevCount + 1)
    }, 1000)

    return () => clearInterval(intervalId) // this will run when the component unmounts
}, [])
```


```javascript
import React,{ useState, useEffect } from 'react'
import randomcolor from "randomcolor"

function App() {
  const [count, setCount] = useState(0)
  const [color, setColor] = useState("")

  function increment() {
    setCount(prevCount => prevCount + 1)
  }

  function decrement() {
    setCount(prevCount => prevCount - 1)
  }

  useEffect(() => {
    setColor(randomcolor())
  }, [count])

  return (
    <>
      <h2 style={{color: color}}>{count}</h2>
      <button onClick={decrement}>Decrement</button>
      <button onClick={increment}>Increment</button>
    </>
  )
}
```


### `useRef()`
Returns a mutable ref object whose `.current` property is initialized to the passed argument (initialValue). The returned object will persist for the full lifetime of the component.

It is similar to accessing an element by `getElementById()`. However, since react could create many elements with the same ID, this is used.

```JavaScript
import React,{ useRef } from 'react'

function TextInputWithFocusButton() {
  const inputEl = useRef(null)
  const onButtonClick = () => {
    // `current` points to the mounted text input element
    inputEl.current.focus()
  }

  return (
    <>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>Focus the input</button>
    </>
  )
}
```

### `useContext()`
Theme example:

`themeContext.js`
```JavaScript
import React, {useState} from "react"
const ThemeContext = React.createContext()

const ThemeContextProvider = props => {
    const [theme, setTheme] = useState('dark');

    const toggleTheme = () => setTheme(prevTheme => prevTheme === 'dark' ? 'light' : 'dark');

    return (
        <ThemeContext.Provider value={{theme, toggleTheme}}>
            {props.children}
        </ThemeContext.Provider>
    )
}

export {ThemeContextProvider, ThemeContext}
```

`index.js`
```JavaScript
import React from "react"
import ReactDOM from "react-dom"

import App from "./App"
import {ThemeContextProvider} from "./themeContext"

ReactDOM.render(
    <ThemeContextProvider>
        <App />
    </ThemeContextProvider>,
    document.getElementById("root")
)
```

`button.js`
```JavaScript
import React, {useContext} from "react"
import {ThemeContext} from "./themeContext"

function Button(props) {
    const {theme, toggleTheme} = useContext(ThemeContext)
    return (
        <button
            onClick={toggleTheme}
            className={`${theme}-theme`}
        >
            Switch Theme
        </button>
    )    
}

export default Button
```

### Custom Hooks
A pattern of developing reusable logic across an app. Custom hooks always start with `useXxx.js` and are placed in their own file.

`useToggler.js`
```javascript
import {useState} from "react"

const useToggler = (defaultOnValue = false) => {
  const [isToggledOn, setIsToggledOn] = useState(defaultOnValue);

  const toggle = () => setIsToggledOn(prev => !prev);

  return [isToggledOn, setIsToggledOn];
}

export default useToggler;
```

`Like.js`
```javascript
import React from "react";
import useToggler from "./useToggler";

const Like = props => {
  const [isLiked, toggle] = useToggler(true);

  return (
    <div>
      <h3>Click heart to like</h3>
      <h1><span onClick={toggle}>
        {isLiked ? "Unlike" : "Like"}
      </span></h1>
    </div>
  )
}

export default Like
```
