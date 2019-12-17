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
