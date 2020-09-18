# React && React Native

### Reconciliation

React implements a heuristic O(n) algorithm based on two assumptions:

1. Two elements of different types will produce different trees.
2. The developer can hint at which child elements may be stable across different renders with a `key` prop.

### The Diffing Algorithm

When diffing two trees, React first compares the two root elements. 

#### Elements of Different Types

Whenever the root elements have different types, React will tear down the old tree and build the new tree from scratch. 

When tearing down a tree, old DOM nodes are destroyed. Component instances receive `componentWillUnmount()`. When building up a new tree, new DOM nodes are inserted into the DOM. Component instances receive `componentWillMount()` and then `componentDidMount()`. 

#### DOM Elements Of The Same Type

When comparing two React DOM elements of the same type, React looks at the attributes of both, keeps the same underlying DOM node, and only updates the changed attributes.

After handling the DOM node, React then recurses on the children.

#### Component Elements Of The Same Type

When a component updates, the instance stays the same, so that state is maintained across renders. React updates the props of the underlying component instance to match the new element, and calls `componentWillReceiveProps()` and `componentWillUpdate()` on the underlying instance.

Next, the `render()` method is called and the diff algorithm recurses on the previous result and the new result.

#### Recursing On Children

By default, when recursing on the children of a DOM node, React just iterates over both lists of children at the same time and generates a mutation whenever there’s a difference.

#### Keys

When children have keys, React uses the key to match children in the original tree with children in the subsequent tree. 

Keys should be stable, predictable, and unique. Unstable keys (like those produced by `Math.random()`) will cause many component instances and DOM nodes to be unnecessarily recreated.



### React-Router

- A `<BrowserRouter>` uses regular URL paths. These are generally the best-looking URLs, but they require your server to be configured correctly. Specifically, your web server needs to serve the same page at all URLs that are managed client-side by React Router. (History API) 可以使用history.push
- A `<HashRouter>` stores the current location in [the `hash` portion of the URL](https://developer.mozilla.org/en-US/docs/Web/API/HTMLHyperlinkElementUtils/hash), so the URL looks something like `http://example.com/#/your/page`. Since the hash is never sent to the server, this means that no special server configuration is needed. 不支持location.key或location.state。

