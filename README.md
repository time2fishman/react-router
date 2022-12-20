# React Router

![React Router Logo](./react-router-logo.png)

## Learning Objectives

- Talk about SPAs
- Review the React component lifecycle and use component methods to integrate
  with API calls
- Use React Router's `BrowserRouter`, `Link`, `Route`, `Routes`, and `Navigate` components
  to add navigation to a React application

## Framing

Up to this point, our React applications have been limited in size, allowing us
to use basic control flow in our components' render methods to determine what
gets rendered to our users. However, as our React applications grow in size and
scope, we need an easier and more robust way of rendering different components.
Additionally, we will want the ability to set information in the url parameters
to make it easier for users to identify where they are in the application.

React Router allows us to build single-page web applications with navigation
without having to rely on page refreshes to take the user to other pages(thereby
avoiding the flash of a white screen). The result is a more seamless user
experience as the user navigates through the app.

React Router is one of the most commonly-used routing libraries for client-side
routing with React. It is relatively straightforward to configure and integrates
with the component architecture nicely (since it's just a collection of
components).

We will configure it as the root component in a React application. Then we'll
tell it to render other components within itself depending on the path in the
url. This way we don't have to reload the entire page to swap out some data.

## We Do: [React Bitcoin Prices](../../../react-bitcoin-prices) Setup

Let's get set up with the react bitcoin price checker! Here is a [live site](https://vigorous-bell-39e27b.netlify.app/) that demonstrates what we're going to build today! 

## You Do: Coindesk API

We will query the Coindesk API in this exercise. Take 5 minutes to test
out (using the browser) the API example endpoint below:

[Coindesk API](https://api.coindesk.com/v1/bpi/currentprice/usd.json)

Try interpolating different currency abbreviates into the URL for `usd`, such as `jpy` for Japanese Yen or `euro` for Euro. 

Also, install the
[React developer tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en)
chrome extension if you haven't already. It'll come in very handy for inspecting
components.

## You Do: Examine Current Codebase (~15 min)

Since we're starting off with a project that already has some scaffolding built
out, we should spend some time getting our bearings.

Take 10 minutes and read through the code to familiarize yourself with the
codebase with a partner or in groups of 3. Prepare to discuss your answers the
following questions:

1. What dependencies is the application currently using? Where can I find
   information on them?
2. What is the purpose of `root.render()`? What file is this method being
   called in?
3. Where are the components of our application located? Why might we want to
   separate them into their own folders?
4. Where is state located in our application? Is state being passed down to
   other components?
5. Look at the Price component. What props is it expecting to be passed?
6. Where is our application getting data from? How is it accomplishing this?

## We Do: React Router Setup (~10 min)

Currently, we are rendering just the App component, which renders the Home
component. Let's bring in React Router and set it up to allow us to display
multiple components.

When working with a new library, it's useful to have
[the documentation](https://reactrouter.com/docs/en/v6/getting-started/overview)
handy!

### Importing Dependencies

First, we need to install `react-router-dom` as a dependency in `package.json`.
Running `npm install` or `npm i` with arguments should automatically do this for
us.

```sh
npm install react-router-dom
```

To configure our current application to use React Router, we need to modify the
root rendering of our app in `index.js`. We need to import the `Router`
component and place it as the root component of our application. `Router` will,
in turn, render `App` through which all the rest of our components will be
rendered:

```jsx
// index.js
import { BrowserRouter as Router } from "react-router-dom";

//...

root.render(
  <Router>
    <App />
  </Router>
  );
```

> Note that we are aliasing `BrowserRouter` as `Router` here for brevity.

By making `Router` the root component of our app, it will pass down several
router-specific objects to its child components. Things like current location
and url can be accessed or changed. Additionally, in order to use the other
routing components provided by React Router, a `Router` parent component is
necessary.

Next, in `App.js`, we need to import all of the other components we want to use
from React Router.

The four main ones we're going to use today are:

```jsx
<Routes />
<Route />
<Link />
<Navigate />
```

Let's go ahead and import just routes, route and link for now, we'll cover redirect
later.

```js
// src/components/App/App.js

import { Routes, Route, Link } from "react-router-dom";
```

Now that we have access to these components, we need to modify the `App`
component's `render()` method to set up navigation. The basic structure we will
use is this:

```jsx
// src/components/App/App.js
  return (
    <div>
      <nav>
      // the link component produces an a element
        <Link to=""></Link>
        <Link to=""></Link>
      </nav>
      <main>
        <Routes>
             // routes render the specified component we pass as the element prop.
            <Route path="" element={}/>
            <Route path="/example" element={}/>
        </Routes>
      </main>
    </div>
  )
```

> **Link** - a component for setting the URL and providing navigation between
> different components in our app without triggering a page refresh. It takes a
> `to` property, which sets the URL to whatever path is defined within it. Link
> can also be used inside of any component that is connected to a `Route`.

> **Route** - a component that renders a specified component (using the 
> `element` prop) based on the current url (`path`) we're at. `path`
> should probably match a `<Link to="">` defined somewhere.

> **Routes** - a component that allows us to add `Route` components to our app. All `Route` components must be nested within a `Routes` component. Automatically picks the `Route` that best matches the current URL.

Now let's modify the render method in `App.js` to include our Link and Route
components.

```jsx
// src/components/App/App.js
return (
  <div>
    <nav>
      <Link to="/">
        <img src="https://en.bitcoin.it/w/images/en/2/29/BC_Logo_.png" alt="" />
        <h1>Bitcoin prices</h1>
      </Link>
    </nav>
    <main>
        <Routes>
            <Route path="/" element={<Home/>} />
        </Routes>
    </main>
  </div>
);
```

Great! But this doesn't do anything because we're already on the homepage.

## You do: Add a Second Route and Link (~10 min)

> 5 minute exercise / 5 minute review

- Using the above instructions as a guide, add a new Link to `/currencies` and a
  route to match it. What component do you think you want to render?

<details>
  <summary>Solution</summary>

```jsx
// src/Components/App/App.js
//...
import Currencies from "../Currencies/Currencies";

// ...
return (
  <div>
    <nav>
      <Link to="/">
        <img src="https://en.bitcoin.it/w/images/en/2/29/BC_Logo_.png" alt="" />
        <h1>Bitcoin prices</h1>
      </Link>
      <Link to="/currencies">Currency List</Link>
    </nav>
    <main>
      <Routes>
        <Route path="/" element={<Home/>} />
        <Route path="/currencies" element={<Currencies/>} />
      </Routes>
    </main>
  </div>
);
```

</details>

Now we've got two components and two routes. Perfect. Let's take a look at our
currencies component and see what we need to do to make it work.

This a good point to talk about React Router's
[URL Parameters](https://reactrouter.com/docs/en/v6/getting-started/overview#reading-url-parameters).

## We do: Currencies component

If we look at this component we see a long list of links. Note that the links
are using regular `<a>` tags.

What happens if we click on a link? It works, but the whole page reloads! Gross.
Let's fix that.

Go ahead and replace the `a` tag with a `<Link>` component. Make the `to` prop
value equal to the `href` value.

```jsx
// src/Components/Currencies/Currencies.js
import { Link } from "react-router-dom";

//...
let list = listOfCurrencies.map((item) => {
  return (
    <div className="currency" key={item.currency}>
      <p>
        <Link to={"/price/" + item.currency}>{item.currency}</Link>:{" "}
        {item.country}
      </p>
    </div>
  );
});
// ...
```

Great! Now go back to the page and click the link again, what happens?

**MAGIC!**

![shia](https://media.giphy.com/media/ujUdrdpX7Ok5W/giphy.gif)

It changes the route for us (notice the URL changing) but we don't have any
routes set up to match that. Let's do that next.

## Break (10 min)

## You do: Prices Component (~10 min)

> 5 min exercise, 5 min review

Back in `App.js`, we need to add another `<Route>` component. This time though,
we want to include a parameter.

Look at the URL that we're on after clicking on a currency. Then look at the
`Price` component. How might you write the `path` prop to make it work?

## We do: Fix prices component (~25 min)

We've added a route but not everything will work yet. HOW COME!?

We need to add a couple things to `<Price />`

We've got a function in this (`App.js`) component called setPrice. let's pass
that in.

```jsx
<Route path="/price/:currency" element={<Price setPrice={setPrice} />} />
```

Finally, we need to pass in the current component's price state.

```jsx
<Route
  path="/price/:currency"
  element={<Price setPrice={setPrice} price={price} />}
/>
```

Now let's check out the `components/Price/Price.js` file and make a couple
additions. In this component, we'll need access to whatever `currency` variable
was passed to the URL parameter when the user clicked on the `Link`.

In order to access it, we'll use the `useParams` hook from React Router and
destructure whatever we called our parameter from the router props.

```jsx
import React, { useEffect } from "react";
// Import useParams hook by destructuring from 'react-router-dom'
import { useParams } from "react-router-dom";
import "./Price.css";

const coindeskURL = "https://api.coindesk.com/v1/bpi/currentprice/";

const Price = ({ price, setPrice }) => {
  // Destructure the currency property from React Router's Route parameters using the useParams Hook
  const { currency } = useParams();
  useEffect(() => {
    const url = `${coindeskURL}${currency}.json`;

    fetch(url)
      .then((res) => res.json())
      .then((res) => {
        let newPrice = res.bpi[currency].rate;
        setPrice(newPrice);
      })
      .catch((err) => {
        console.error(err);
      });
  }, []);

  return (
    <div>
      <h1>Bitcoin price in {currency}</h1>
      <div className="price">{price}</div>
    </div>
  );
};

export default Price;
```

## Which route is chosen? (~10 min)

If we had a list of routes defined with the following path patterns within the same `Routes` component:

- `/currencies/*`
- `/currencies/new`
- `/currencies/:id`

> Note that adding * in a path means any characters can fall in the * position. For example `/currencies/test/something/hi` would match the path pattern `/currencies/*`.

When a user visits the route `/currencies/new` all three of these routes would match. Unless you specify differently, only one route will be rendered. The routed rendered is the most specific route that matches. More information can be found here [here](https://reactrouter.com/docs/en/v6/getting-started/concepts#ranking-routes
).

The order routes are defined doesn't matter.

![shia](https://media.giphy.com/media/ujUdrdpX7Ok5W/giphy.gif)


## Redirects

To redirect a user to a different path using react router, you use the `Navigate` component.

- Import the `Navigate` component from `react-router-dom`
- Add another route called `/currency`
- Instead of rendering one of our components, put the `Navigate` component as the value for the element prop.

```js
<Route path="/currency" element={<Navigate to="/currencies" />} />
```

Navigate only requires a `to` prop which tells it what path to navigate to.

## Wrapping Up (Remainder of Class)

Here's a rough outline of how you should go about building react apps! Follow
these suggestions, or don't, but they will probably help you a lot if you do
them in order. I suggest reading through all of the steps before you start so
you can become familiar with the big picture of the entire process.

1. Start a new app using `create-react-app`. Call it user-router or something
   similar, it doesn't matter. You will be building four components (not
   including App.js). Each one will render something different:

| Component | Renders                                   | Route         |
| --------- | ----------------------------------------- | ------------- |
| Home      | "This is the homepage"                    | /             |
| Greet     | A greeting from a url parameter passed in | /greet/:param |
| Users     | A list of users                           | /users/       |
| NewUser   | A form that lets you add a username       | /users/new    |

1. Set up `react-router` like we did in this lesson, at the top level. What is
   the top level of a React app?

1. Build out each component with placeholders to render something. Do this
   first, before starting to add state, props, or functionality.

1. Set up your routes so that each route only displays the appropriate
   component.

1. Plan out where you think your state should live. If you have to share state
   between multiple components, what's the best place to keep it? Think about
   what your state needs to contain.

1. Initialize your state and pass it down to the appropriate components.

1. Wire up those components to be able to display and update the state as
   necessary. Add the functionality to have the greet component receive and
   display a parameter.

1. Marvel at your creation and your progress after only 5 weeks of programming!