# Context API

<!-- TOC -->

# Context
In React, context allows data to be passed down to deeply nested components without having to traverse many middle components which don't need that data.

Context is split into two parts - the Provider and the Consumer. The Provider hold the data or methods and passes it directly to whatever Consumer components require it.


## `createContext()`
Creates a context object with Provider and Consumer properties.

When the Provider is called, it must always be passed with a `value` prop.

`themeContext.js`
```JavaScript
import React from "react"
const ThemeContext = React.createContext()
export default ThemeContext
```

`index.js`
```JavaScript
import App from "./App"
import ThemeContext from "./themeContext"

ReactDOM.render(
  <ThemeContext.Provider value={"light"}>
    <App />
  </ThemeContext.Provider>
  , document.getElementById("root"))
```

## contextType
This is essentially assigning which context (assuming you may have created multiple) to use.

```JavaScript
import ThemeContext from "./themeContext"

class Button extends Component {
    render() {
        const theme = this.context
        return (
            <button className={`${theme}-theme`}>Switch Theme</button>
        )    
    }
}

Button.contextType = ThemeContext // pulls in ThemeContext
```
