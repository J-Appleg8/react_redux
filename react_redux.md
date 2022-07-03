<style>
th, thead {
    border-top:1pt solid;
    border-bottom: 2px solid;
    border-left: none;
    border-right: none;
}
td {
    border-top: 1px solid;
    border-bottom: 1px solid;
    border-left: 1px solid;
    border-right: 1px solid;
}
</style>

# React Redux

## <span style="color:lightgreen">Why Use Redux?:</span>

---

React hooks like useState and useReducer exist to allow us to manage data that typically changes, and where changes to that data then lead to the UI being updated. We use React's state management hooks like useState so that we can tell react that some data has changed so it updates the UI for us. The definition of state can be split into three main kinds:

- Local State
- Cross-Component State
- App-Wide State

### <span style="color:turquoise">Local State:</span>

State that belongs to a single component

- E.g. listening to user input in a input field; toggling a "show more" details field
- Should be managed component-internal with useState() or useReducer()

### <span style="color:turquoise">Cross-Component State:</span>

State that affects multiple components

- E.g. open/closed state of a modal overlay
- Requires "prop-chains"/"prop drilling"

### <span style="color:turquoise">App-Wide State:</span>

State that affects the entire app (most/all components)

- E.g. user authentication status
- Can also use "prop-chains"/"prop drilling"
- Can also use React Context to manage this type of state
- Can also use Redux

---

<br>

## <span style="color:lightgreen">What Is Redux?:</span>

---

Redux is a state management system for cross-component or app-wide state. React Context can offer this type of state management as well, but React Context can have some disadvantages

### <span style="color:turquoise">React Context - Potential Disadvantages:</span>

**Complex Setup/ Management**

- For larger applications, context management can become very complex and hard to manage - can lead to deeply nested JSX code and/or huge "Context Provider" components

**Performance**

- React Context is meant for low-frequency updates (like locale/theme) but it is not ready to be used as a replacement for all Flux-like state propagation (Redux)
- Not optimized for high-frequency state changes

---

<br>

## <span style="color:lightgreen">How Redux Works:</span>

---

### <span style="color:turquoise">Storing Data/State:</span>

All state is kept in one Central Data (State) Store in the application. All state for the application is stored in one central data store

- For Example: You would store authentication state theming, user input state etc. in one store

### <span style="color:turquoise">Retrieving Data/State:</span>

Ultimately, we have data that we keep in that store, so that we can use it inside of our components

- Components set up **subscriptions** - they subscribe to the store, and when data changes the store notifies the components, and then the components can get the data they need (like current authentication status) and use it

### <span style="color:turquoise">Mutating Data/State:</span>

RULE: components NEVER directly manipulate the store data. Components instead use a concept called reducers

- The reducer function that we have to set up is responsible for mutating the store data
  - Reducer functions are functions that take input, transform it and then outputs a new result

### <span style="color:turquoise">Process Flow:</span>

1. Components dispatch actions
   - Actions are a JavaScript object which describes the kind of operation that the reducer should perform
1. Redux forwards actions to the reducer, reads the description of the desired operation
1. Reducer then performs the operation and outputs a new state which will effectively replace the existing state in that central data store
1. Then subscribing components are notified that the data in the central store has been updated so that the component can then update the UI

---

<br>

## <span style="color:lightgreen">Basic Redux With NodeJs:</span>

---

So first there is redux storage the place where we store all our state. It is created by using redux.createStorage a method provided by redux and we are storing the redux storage in a constant and here we pass the pointer to the reducer function.

```javascript
const store = redux.createStore(counterReducer);
```

Then the reducer function is not the same as useReducer. For better understanding, I have used the next lecture's reducer function. It is the function that changes the state which receives the state and the action that triggered it. We set the default value by state = { counter: 0 } It's just javascript. The function should be pure which means the same inputs give the same output every time, this is done by using things that are provided to the function here which are action and state. So the reducer changes state according to the action which I will talk about next, but till then bare with me. We use the type property of the action to decrease counter on decrement and increase on increment else (when run the first time) change nothing.

```javascript
const counterReducer = (state = { counter: 0 }, action) => {
  if (action.type === 'increment') {
    return {
      counter: state.counter + 1,
    };
  }
  if (action.type === 'decrement') {
    return {
      counter: state.counter - 1,
    };
  }
  return state;
};
```

Next is the action which is called by using the dispatch method provided by store const(the const where we stored the redux storage) this method receives an object with a property type. The action triggers the reducer function which changes the state.

```javascript
store.dispatch({ type: 'increment' });
store.dispatch({ type: 'decrement' });
```

Finally the subscriber or the state consumer. It is a function here and can also be a component that gets the latest state by the getState method of store const(the const where we stored the redux storage) and we tell the store that this function is a subscriber/state consumer by store.subscribe(counterSubscriber); and here we pass the pointer to the subscriber, we give the subscriber's

```javascript
const counterSubscriber = () => {
  const latestState = store.getState();
  console.log(latestState);
};
```

I think now you are clear on what we are doing in this lecture and why

---

<br>

## <span style="color:lightgreen">React With Redux:</span>

---
