# JSX

<!-- TOC -->
- [Elements](#elements)

<!-- TOC END -->

# Elements
Essentially a JS variable. The individual component that holds JSX and gets rendered.  

```JavaScript
const header = <h1>Howdy there</h1>;
```
Elements can also represent user-defined components:
```JavaScript
const header = <Welcome name="Michael" />;
```

A mix between HTML and JavaScript.
```javascript
<h1>Hello {name}!</h1>;
```
