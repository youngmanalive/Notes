
# REACT

- Front end Javascript library for reating UI components
- Virtual DOM tree ---> real DOM

### Components
- JS functions that return HTML:
- Example:
  - app - main
  - index
  - index items

##### Props
  - Attributes from parent components, shouldn't mutate

##### State
  - Mutable data that affects rendered output
  - changes over time (ex: user input)
  - owned by component
  - set initial value in constructor

##### Render
  - Describes at any moment in time what component should look like
  - A function of props and state
  - Never explicitly called
  - Changes to props/state induce a render
  - Uses JSX --> conventional syntax for creating React elements
  - Must return single React element but can have any number of descendants

##### JSX
  - Transpiled into raw JS

###### Babel

##### Events
  - Handler set by passing prop
  - Value is pointer to handle function
  - Event handler function is passed event object
  - Event object contains target, current target, time, mouse location
    - usually will `preventDefault()`



# Flux

- Front-end app architecture
- Provides unidirectional data flow
  - more predictability than multi-threaded, cascading updates like MVC

#### Action

- Begins flow of data in Flux
- Object that at minimum contains a `type` indicating type of change to app's state
- May contain additional data ('payload') to change former state to new one

#### Dispatcher

- Mechanism for distributing actions to app's store
- Registry of callback functions into the store
- Redux consolidates dispatcher into single `dispatch()` function

#### Store

- Represents the entire state of the app
- Responsible for updating app state whenever action is received
  - Gives dispatcher a callback function which receives action
  - The callback uses the action's type to invoke proper function to change state
- Emits a change after store has changed state

#### View

- Unit of code responsible for rendering UI
- Listens to change events emitted by store
- Capable of creating actions itself such as user-triggered events
  - Turns Flux pattern into unidirectional loop:

```
               ← ← ← ← Action ← ← ← ←
              ↓                      ↑
              ↓                      ↑
Action → → Dispatcher → → Store → → View
```

# Redux

- Node package that facilitates a particular implementation of Flux
- 3 principles:
  1. Single Source of Truth
  2. State is Read-Only
  3. Only Pure Functions Change State

### Store

- Central element of Redux architecture
- Holds global state of app
- Responsibilities:
  - updates state via *reducer*
  - broadcasts state to the view layer via *subscription*
  - listens for *actions* that determine how/when to change global state


- Creating the store:
  - `createStore(reducer, [preloadedState], [enhancer])`:
    - `reducer` (required)
      - reducing function that receives current state/incoming actions
      - determines how to update, returns next state
    - `preloadedState` (optional)
      - object representing any state that existed before store was created
    - `enhancer` (optional)
      - function that adds extra functionality to the store  

    ```JavaScript
    // store.js
    import { createStore } from `redux`;
    import reducer from './reducer.js';

    const store = createStore(reducer);
    ```

- Store API
  - `getState()` - Returns the store's current state
  - `dispatch(action)` - Passes an action into the store's reducer telling it what information to update
  - `subscribe(callback)` - Registers callbacks to be triggered whenever the store updates. Returns a function, which when invoked, unsubscribes the callback function from the store.


- Updating the store:
  - store updates can only be triggered by dispatching actions
  - `action` in Redux is plain object with:
    - `type` - key indicating action being performed
    - optional payload keys containing any new info
  - `store.dispatch(action)`:
    - passes current `state` and `action` to the `reducer`
    - `reducer` returns next `state`


- Subscribers - callbacks that can be added to the store via `subscribe()`

### Reducers
- function for updating `state` that:
  - receives current `state` and `action`
  - updates state based on `action.type`
  - returns next state


- Never mutates arguments
  - use `Object.freeze(state)` to ensure current state becomes immutable


- `combineReducers()`
  - combines multiple reducers and return a single reducer
  - using this allows `createStore()` to take the singular reducer it requires

### Actions
- Only way for view components to trigger changes to store
- Simple POJOs with mandatory `type` key and optional payloads
- Sent using `store.dispatch()`
- Primary drivers of Redux loop

- New state data is sent along as the *payload*

```JavaScript
const addDog = {
    type: "ADD_DOG",
    animal: "dog"
};

// dynamic...
const addDog = dog => ({
    type: "ADD_DOG",
    dog
});
```




---------------------


### Higher-Order Functions
- functions that operate on other functions, either by receiving them as arguments or returning them.

## Middleware
Refers to an `enhancer` passed to Store via `createStore()`. When `dispatch` is made, the middleware intercepts the `action` before it reaches the `reducer`, where it can then:
- resolve the action itself (ex: AJAX request)
- pass along the action if middleware isn't concerned with it
- generate a side effect
- send another dispatch if action triggers other actions

Middleware is given to the store via optional enhancer argument
```JavaScript
let configureStore = (state = {}) => (
  createStore(
    rootReducer,
    state,
    applyMiddleware(logger)
  )
);

// Note: applyMiddleware() accepts multiple arguments
```

- Function signature - set of inputs and outputs of a function
- Redux middleware must have the following signature:
```JavaScript
const middleware = store => next => action => {
    //side effects here, if any
    return next(action);
};
```
  - receive `store` as argument and returns a function that takes the `next` link in the middleware chain as argument, which returns another function that receives the `action` and triggers any side effects before returning the result of the `next(action)`.


## Thunks
- action creator that returns a function
