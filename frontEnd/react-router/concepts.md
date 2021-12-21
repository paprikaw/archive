# Basic concepts

## 一些背景
### History
React-router是基于js history库的实现

### Routers
一个基本的router一般像这样
``` js
import React from "react";
import ReactDOM from "react-dom";
import { BrowserRouter } from "react-router-dom";

function App() {
  return <h1>Hello React Router</h1>;
}

ReactDOM.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>,
  document.getElementById("root")
);
```

BrowserRouter和HashRouter之间还有区别
* BrowserRouter uses regular URL paths. These are generally the best-looking URLs, but they require your server to be configured correctly. Specifically, your web server needs to serve the same page at all URLs that are managed client-side by React Router. Create React App supports this out of the box in development, and comes with instructions on how to configure your production server as well.
* HashRouter stores the current location in the hash portion of the URL, so the URL looks something like http://example.com/#/your/page. Since the hash is never sent to the server, this means that no special server configuration is needed.

### Switch(在react-route 6中叫Routes)

Switch从上到下匹配，如果匹配到了，就使用这个route。所以优先级高的匹配应该放在前面（更加specific的route）

一个例子

``` js
function App() {
  return (
    <div>
      <Switch>
        {/* If the current URL is /about, this route is rendered
            while the rest are ignored */}
        <Route path="/about">
          <About />
        </Route>

        {/* Note how these two routes are ordered. The more specific
            path="/contact/:id" comes before path="/contact" so that
            route will render when viewing an individual contact */}
        <Route path="/contact/:id">
          <Contact />
        </Route>
        <Route path="/contact">
          <AllContacts />
        </Route>

        {/* If none of the previous routes render anything,
            this route acts as a fallback.

            Important: A route with path="/" will *always* match
            the URL because all URLs begin with a /. So that's
            why we put this one last of all */}
        <Route path="/">
          <Home />
        </Route>
        {/* 如果我们需要Switch匹配整个url而不是部分的话： */}
        <Route exact path="url">
          <Home />
        </Route>

      </Switch>
    </div>
  );
}
```
### History