# Hooks

<!-- TOC -->
- [`useState()`](#usestate)
- [`useEffect()`](#useeffect)
- [`useRef()`](#useref)

<!-- TOC END -->

### `useState()`
Returns and sets stateful values.

```javascript
import React,{ useState } from 'react';

// const [state, setState] = useState(initialState);
const [name, setName] = useState('');

const handleInput = event => {
  setName(event.target.value);
}

return (
  <>
    <input type='text' onchange={handleInput}>
  </>
)
```

### `useEffect()`
Accepts a function that contains imperative, possibly effectful code.

```javascript
import React,{ useState, useEffect } from 'react';

const [name, setName] = useState('');

// Similar to componentDidMount and componentDidUpdate:
  useEffect(() => {
    // Update the document title using the browser API
    document.title = `Welcome ${name}.`;
  });

const handleInput = event => {
  setName(event.target.value);
}

return (
  <>
    <input type='text' onchange={handleInput}>
  </>
)
```


### `useRef()`
useRef returns a mutable ref object whose .current property is initialized to the passed argument (initialValue). The returned object will persist for the full lifetime of the component.

```JavaScript
import React,{ useRef } from 'react';

function TextInputWithFocusButton() {
  const inputEl = useRef(null);
  const onButtonClick = () => {
    // `current` points to the mounted text input element
    inputEl.current.focus();
  };
  return (
    <>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>Focus the input</button>
    </>
  );
}
```
