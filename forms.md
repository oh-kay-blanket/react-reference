# Forms

<!-- TOC -->
- [Formik](#formik)
- [Controlled Components](#controlled-components)
  - [`<input>`](#)
    - [Checkbox](#checkbox)
  - [Radio](#radio)
  - [`<textarea>`](#-1)
  - [`<select>`](#-2)
    - [Handling Multiple Inputs](#handling-multiple-inputs)
  - [Submit](#submit)

<!-- TOC END -->

## Formik
https://jaredpalmer.com/formik/  
A library for building React forms more easily.

# Controlled Components

React handles forms differently than standard HTML. A 'controlled component' allows React to override the default HTML behavior and be the holder of state.

Best practice is to use a single `handleChange()` method to handle all form updates.


### `<input>`
```javascript
// constructor() {...}

handleChange(event) {
  const eValue = event.target.value // Always make a copy of the event.target to avoid weird bugs.
  this.setState({ value: eValue })
}

// render() {...
<input type={text} value={this.state.value} onChange={this.handleChange} />
```
Then you can handle the setState() in `handleChange()`.


#### Checkbox
```javascript
handleChange(event) {
  const {name, value, type, checked} = event.target
  type === 'checkbox' ? this.setState({ [name]: checked }) : this.setState({ [name]: value })
}

// render () {...
<input
  type='checkbox'
  name='isChecked'
  checked={this.state.isChecked}
  onChange={this.handleChange}
/>
```


### Radio
Uses the
```javascript
handleChange(event) {
  const {name, value, type, checked} = event.target
  type === 'checkbox' ? this.setState({ [name]: checked }) : this.setState({ [name]: value })
}

// render () {...
<label>
  <input
    type='radio'
    name='gender'
    value='male'
    checked={this.state.gender === 'male'}
    onChange={this.handleChange}
  />Male
</label>
<br />
<label>
  <input
    type='radio'
    name='gender'
    value='female'
    checked={this.state.gender === 'female'}
    onChange={this.handleChange}
  />Female
</label>
```


### `<textarea>`

In HTML, the `<textarea>` text is handled as a child element.
```html
<textarea>
  Info here.
</textarea>
```

In React, it is handled as a value:
```javascript
// Note that in React, <textarea> is a self-closing tag
<textarea value={this.state.value} onChange={this.handleChange} />
```


### `<select>`
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
<select value={this.state.favoriteColor} name='favoriteColor' onChange={this.handleChange}>
  <option value="red">Red</option>
  <option value="green">Green</option>
  <option value="yellow">Yellow</option>
</select>
```

__Note:__ You can pass an array into the value attribute to select multiple options:
```JavaScript
<select multiple={true} value={['mango', 'apple']}>
```


#### Handling Multiple Inputs
You can add a `name` attribute to each element and use that as the key for the object:
```JavaScript
handleChange(event) {
  const { name, value } = event.target // Always make a copy of the event.target to avoid weird bugs.

  this.setState({
    [name]: value // the [] around 'name' is an ES6 convention for computing key names
  })
}

render() {
  return (
    <form>
      <input type='text' name='firstName' placeholder='First Name' value={this.state.firstName} onChange={this.handleChange} />
      <input type='text' name='lastName' placeholder='Last Name' value={this.state.lastName} onChange={this.handleChange} />
    </form>
  )
}
```

### Submit
Create an `handleSubmit()` method to handle submits. Add that method to the `<form>` tag.

```JavaScript
<form onSubmit={this.handleSubmit}>
  <button>Submit</button>
</form>
```
