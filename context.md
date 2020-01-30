# Context API

<!-- TOC -->

# Context
In React, context allows data to be passed down to deeply nested components without having to traverse many middle components which don't need that data.

Context is split into two parts - the Provider and the Consumer. The Provider hold the data or methods and passes it directly to whatever Consumer components require it.


## `createContext()`
Creates a context object with Provider and Consumer properties.


`index.js` - Where the context is held
```JavaScript
import App from "./App"
import {ThemeContextProvider} from "./themeContext"

ReactDOM.render(
  <ThemeContextProvider>
    <App />
  </ThemeContextProvider>
  , document.getElementById("root"))
```

When the Provider is called, it must always be passed with a `value` prop.

`themeContext.js` - The Provider
```JavaScript
import React from "react"
const {Provider, Consumer} = React.createContext()

class ThemeContextProvider extends React.Component {
  state = {
    theme: "dark"
  }

  toggleTheme = () => {
    this.setState(prevState => ({ theme: prevState.theme === "light" ? "dark" : "light" }))
  }

  render() {
    return (
      <Provider value={{ theme: this.state.theme, toggleTheme: this.toggleTheme }}>
        {this.props.children}
      </Provider>
    )
  }
}

export {ThemeContextProvider, Consumer as ThemeContextConsumer}
```

`button.js` - A Consumer
```JavaScript
import {ThemeContextConsumer} from "./themeContext"

const Button = props => (
  <ThemeContextConsumer>
    {context => (
      <button onClick={context.toggleTheme} className={`${context.theme}-theme`}>Switch Theme</button>      
    )}
  </ThemeContextConsumer>
)    
```
