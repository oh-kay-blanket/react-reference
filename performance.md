# Performance

<!-- TOC -->
- [Render Tree](#render-tree)
- [Shallow comparison](#shallow-comparison)

<!-- TOC END -->


# Render Tree
When state changes in Component A, all sub-components will also update by default, even if state is not being passed to them. Therefore, never store state higher up in the tree than is required.

# Shallow comparison
The operator `===` works correctly on primitive types (string, number, boolean), but cannot compare complex types (arrays, objects), even if they are identical. This is because complex types are stored separately in memory.

However, it can work on individual items of objects — a shallow comparison. It can also work when one object is a copy of another.
```javascript
const one = {
  name: "Michael"
}

const two = {
  name: "Michael"
}

const three = one;

console.log(one === two) // false
console.log(one.name === two.name) // true
console.log(three == one) // true, it is a copy of one
```

If, however, objects or arrays store more complex types inside of them, then even if they are identical, they will not longer be considered shallow equal.
```javascript
const one = {
  name: "Michael",
  siblings: {
    brother: "Justin",
    sisters: ["Angie", "Krystle", "Nikki"]
  }
}

const two = {
  name: "Michael",
  siblings: {
    brother: "Justin",
    sisters: ["Angie", "Krystle", "Nikki"]
  }
}

console.log(one.siblings === two.siblings) // false, these are not shallow equal
```

# PureComponent
An alternative to `Component`. It automatically implements a `shouldComponentUpdate()` and performs a shallow comparison. Requires a class component to be used.


# React.memo()
A HOC. Essentially the same as PureComponent, but can be used with functional components. Only compares `prevProps` and `nextProps`, no state checking.

To implement, use `export default React.memo(App)`. It will handle all of the checking and will manage all sub-components of App.

## `areEqual()`
Use this if you want to run your own custom check. Re-renders only when it returns `false`.
```javascript
const App = props => (
  <>
    <h1>Hi {props.name}</h1>
    <Form />
  </>
)

const areEqual = (prevProps, nextProps) => {
  // return true if passing nextProps to render would return
  // the same result as passing prevProps to render,
  // otherwise return false
}

export default React.memo(App, areEqual)
```
