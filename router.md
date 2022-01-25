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

#### Nested Routes
``` javascript
<div>
    <Header />
    
    <Switch>
        <Route exact path="/">
            <Home />
        </Route>
        <Route path="/profile"> // Don't use exact here
            <div>
                <h1>Profile Page</h1>
                <ul>
                    <li><Link to="/profile/info">Profile Info</Link></li>
                    <li><Link to="/profile/settings">Profile Settings</Link></li>
                </ul>
                <Switch>
                    <Route exact path="/profile/info">
                        <h2>Profile Info</h2>
                    </Route>
                    <Route exact path="/profile/settings">
                        <h2>Profile Settings</h2>
                    </Route>
                </Switch>
            </div>
        </Route>
    </Switch>
    
    <Footer />
</div>
function Profile() {
    return (
        
    )
}
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