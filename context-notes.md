## Context object, prototype

- defines the object that you want to manage. This is constant value created by React.createContext and includes both values of the object and functions to modify the object
- This is the prototype for the context object

```js
const exampleContext = React.createContext({
    items: [],
    totalAmount: 0,
    addItem: (item) => {},
    removeItem: (id) => {},
})
```

## Context provider

- A component that wraps children and provides access within that component to query and call context functions.
- Not sure why this is repeated from the context prototype ?
- if you want to use a reducer function, this is the file where you'd define that. Then inside the CartProvider component you useReducer() and then apply the reducer functions within the object functions.

```js
import exampleContext from './example-context'
const CartProvider = (props) => {
  const addItemToCartHandler = (item) => {}
  const removeItemToCartHandler = (id) => {}
  const cartContext = {
    items: [],
    totalAmount: 0,
    addItem: addItemToCartHandler,
    removeItem: removeItemToCartHandler,
  }

  return (
    <CartContext.Provider value={cartContext}>
      {props.children}
    </CartContext.Provider>
  )
}
```

## define the ContextProvider scope

- use the ContextProvider component at a level above anything that should share context. This includes both components that need to consume information, and those that will update the context

```js
import CartProvider from './Store/CartProvider'

function App() {
  return (
    <CartProvider>
    // components that need to consume information
    // components that update the context
    </CartProvider>
  )
} 
```

## context consumers

- import the useContext function from react, and them import the prototype context from the store (not the CartProvider component)
- 
```js
import { useContext } from 'react'
import exampleContext from './Store/example-context'

const Component = () => {
  const exCtx = useContext(exampleContext)

  // get a value
  const currentAmount = exCtx.totalAmount

  // call a function
  const exampleItemRemoveHandler = (id) => {
    exCtx.removeItem(id)
  }

  return
    <h1 key={item.id} 
        // for any function call, make sure we bind the function params
        //  at time of creation
        onClick={exampleItemRemoveHandler.bind(null, item.id)}>
      {item.name}
    </h1>
}
```