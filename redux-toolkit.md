## Setup

- Check out my demo [circuit-toggle-app](https://github.com/dkrozelle/circuit-panel/tree/with-redux) for a working example with sliced redux-toolkit demo
- Official redux-toolkit [tutorial](https://redux-toolkit.js.org/tutorials/quick-start)

```
npm install @reduxjs/toolkit react-redux
```

- these notes generally assume the most complex configuration is required, so it shows the basic use of slices each with associated reducer functions that are mapped into a single store. 


## define slices for each group of related context setting

- just a separate definition of initial state
- the slice defines
  -  name of the slice
  -  the actual state object
  -  reducer functions that modify the state. 
     -  Redux requires that we write all state updates immutably, by making copies of data and updating the copies. However, Redux Toolkit's createSlice and createReducer APIs use Immer inside to allow us to write "mutating" update logic that becomes correct immutable updates.
     -  the names you use here are function names on the exported slice.actions object. This lets us call the reducer functions directly without the need to pass normal {type:'ADD'} style actions.

```js
import { createSlice } from '@reduxjs/toolkit';

const initialAuthState = {
  isAuthenticated: false,
};

const authSlice = createSlice({
  name: 'authentication',
  initialState: initialAuthState,
  reducers: {
    login(state) {
      state.isAuthenticated = true;
    },
    logout(state) {
      state.isAuthenticated = false;
    },
  },
});

export const authActions = authSlice.actions;
```


## Grouping slices into a single store

- We import individual slices, provide mapping names and export the store
- 
```js
import { configureStore } from '@reduxjs/toolkit';

import counterReducer from './counter';
import authReducer from './auth';


const store = configureStore({
  reducer: { counter: counterReducer, auth: authReducer },
});

export default store;
```

## Use store data via useSelector

```js
import { useSelector } from 'react-redux';

const isAuth = useSelector(state => state.auth.isAuthenticated);

// use in  local functions
const notice_color = isAuth ? "green" : "red"

// use in the return
return(<div>
        <Header />
        {!isAuth && <Auth />}
        {isAuth && <UserProfile />}
    </div>)
```

## Update store variables via useDispatch

- to use the redux reducer functions, you need call them as a Dispatch argument. Note these are actually executed with "()" and any payload.
```js
import { useDispatch } from 'react-redux';
import { authActions } from '../store/auth';

const Component () => {
    const dispatch = useDispatch();
    
    const loginHandler = (event) => {
        event.preventDefault();
        dispatch(authActions.login());
    };

    return(<span onClick={loginHandler}>Click here to login</span>)
}
```


## Wrap App in provider

- Make sure you wrap a provider that is linked to your store to the top index.js
- 
```js
import store from './store/index'
import { Provider } from 'react-redux'

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root'),
)
```
