# Router
<!-- TOC -->

## Components

### Routers

#### `<BrowserRouter>`
Add to index.js
``` javascript
import {BrowserRouter as Router} from "react-router-dom";
```

#### `<HashRouter>`


### Route Matchers

#### `<Switch>`
Sets up a state based area, like a JS switch statement
``` javascript
<Switch>
    // Routes go here
</Switch>
```

#### `<Route>`
``` javascript
<Route exact path="/" component={Home} />
```

### Navigation

#### `<Link>`
``` javascript
import {Link, Switch, Route} from "react-router-dom";

<Link to="/">Home</Link>
<Link to="/about">About</Link>
```

#### `<NavLink>`

#### `<Redirect>`


## Hooks

### `useHistory`

### `useLocation`

### `useParams`

### `useRouteMatch`