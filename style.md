# Styling

<!-- TOC -->
- [Inline Style](#inline-style)

<!-- TOC END -->

# Inline Style
* Can be handy to use inline styles, because styles can be managed as JavaScript.
* JSX expects inline styles to be an object, rather than a string.
* No dashes in JS, convert CSS properties to lowerCC.
* If there will be multiple variables, it's a good idea to put them into a variable.

```javascript
function App() {
  const date = new Date()
  const hours = date.getHours()

  const styles = {
    backgroundColor: '#f8f8f8',
    fontSize: '2em'
  }

  let timeOfDay
  if (hours < 12) {
    timeOfDay = 'morning'
    styles.color = 'orange'
  } else if (hours >= 12 && hours < 17) {
    timeOfDay = 'afternoon'
    styles.color = 'green'
  } else {
    timeOfDay = 'night'
    styles.color = 'purple'
  }

  return (
    <h1 style={styles}>Good {timeOfDay}.</h1>
  )
}
```
