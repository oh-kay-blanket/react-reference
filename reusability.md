# Reusability

<!-- TOC -->
- [Higher-Order Components](#higher-order-components)
  - [HOC Syntax](#hoc-syntax)
- [Render Props](#render-props)

<!-- TOC END -->


# Higher-Order Components
An HOC is a function, which takes a component as its first argument, returns a new component, which wraps the argument component and gives it new capabilities.

## HOC Syntax
HOC compononents begin with a lower case `with`. For example `withToggle`.

`withToggler.js`
```javascript

class Toggler extends Component {
  state = {
    on: this.props.defaultOnValue
  }

  toggle = () => {
    this.setState(prevState => {
      return { on: !prevState.on }
    })
  }

  render() {
    const {component: C, defaultOnValue, ...props} = this.props;
    return (
      <C on={this.state.on} toggle={this.toggle} {...props} />
    )
  }
}

export const withToggler = (component, optionsObj) => {
  return function(props) {
    return (
      <Toggler {...props} component={component} defaultOnValue={optionObj.defaultOnValue} />
    );
  }
}
```

`App.js`
```javascript
import {withToggler} from './withToggler';

const App = props => (
  <>
    <h2>Follow me!</h2>
    <p><span onClick={props.toggle}>{props.on ? "Follow" : "Unfollow"}</span><p>
  </>
)

export default withToggler(App, {defaultOnValue: true}) // Returns a new component, still called App, which a super-powered version of the original App.
```
``


# Render Props
Sort of like a simpler inverse of HOCs. Rather than having a HOC render a super-charged component. You are passing the additional functionality component inside the function component, and passing the desired content as a `render` prop. These are preferable over HOCs.

Useful for separating the UI from the logic. Allows a component to maintain its state and to let other components conditionally render, depending on the state of the initial component.

Rather than using a `render` prop, you can essentially do the same thing by using `props.children`.


`Toggler.js`
```javascript
class Toggler extends Component {
  state = {
    on: this.props.defaultOnValue
  }

  static defaultProps = {
    defaultOnValue: false
  }

  toggle = () => {
    this.setState(prevState => ({ on: !prevState.on }))
  }

  render() {
    return (
      <>
        {this.props.render(this.state.on, this.toggle)}
      </>
    )
  }
}

export default Toggler
```

`App.js`
```javascript
import Toggler from './Toggler';

const App = props => (
  <Toggler defaultOnValue={false} render={ // Toggle has a prop called render, which passes a function to return specific content
    (on, toggle) => (
      <>
        <h2>Follow me!</h2>
        <p><span onClick={toggle}>{on ? "Follow" : "Unfollow"}</span><p>
      </>
    )
  } />
)

export default App
```
