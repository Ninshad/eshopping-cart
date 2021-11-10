# eshopping-cart

Redux
------
// import {createStore} from "redux";
   const store = createStore()         //

  (store folder)
  index.js
  `````````````````````````
import { applyMiddleware, createStore } from 'redux';
import rootReducer from '../reducers';
import thunk from 'redux-thunk';

const store = createStore(rootReducer, applyMiddleware(thunk));

export default store;





  (reducers folder)
  auth.reducers.js
  ````````````````````````````````````````
import { authConstants } from "../actions/constants";

const initState = {
    token: null,
    user: {
        email: '',
        password: ''
    },
    authenticate: false,
    authenticating: false
};

export default (state = initState, action) => {

    console.log(action);

    switch(action.type){
        case authConstants.LOGIN_REQUEST:
            state = {
                ...state,
                authenticating: true,
                ...action.payload
            }
            break;
        case authConstants.LOGIN_SUCCESS:
            state = {
                ...state,
                token: action.payload.token,
                authenticate: true,
                authenticating: false
            }
            break;
        case authConstants.LOGOUT_REQUEST:
            state = {
                ...state,
                authenticating: true

            }
            break;  
            case authConstants.LOGOUT_SUCCESS:
                state = {
                    ...initState,
                    authenticating: false
                }
                break;

    }

    return state;
}
   
  
  index.js 
  ```````````````````````````````````````
import { combineReducers } from 'redux';
import authReducer from './auth.reducers';
import signupReducer from './signup.reducer';
import getProductsReducer from './product.reducers';



const rootReducer = combineReducers({
    auth: authReducer,
    signup: signupReducer,
    product: getProductsReducer
});

export default rootReducer;
  





  (actions folder)
  auth.actions.js
  ``````````````````````````````````
import axios from "../helpers/axios";
import { authConstants } from "./constants"

export const login = (user) =>{

    console.log(user);

    return async (dispatch) => {

        dispatch({ type: authConstants.LOGIN_REQUEST});
        const res = await axios.post('/admin/signin', {
            ...user  
        })

        if(res.status ===200){
            const { token, user } = res.data;
            localStorage.setItem('token', token);
            localStorage.setItem('user', JSON.stringify(user));
            dispatch({
                type: authConstants.LOGIN_SUCCESS,
                payload: {
                    token, user
                }
            });
        }else{
            if(res.status === 400){
                dispatch({
                    type: authConstants.LOGIN_FAILURE,
                    payload: { error: res.data.error }
                });
            }
        }


    }
}

   index.js 
   ``````````````````````````````````
export * from './auth.actions';
export * from './signup.actions';
export * from './product.action';

   constants.js
   ````````````````
export const authConstants = {
    LOGIN_REQUEST: 'LOGIN_REQUEST',
    LOGIN_FAILURE: 'LOGIN_FAILURE',
    LOGIN_SUCCESS: 'LOGIN_SUCCESS',
    LOGOUT_REQUEST: 'LOGOUT_REQUEST'
}



getting state
````````````````````````
import { useSelector } from 'react-redux';
const auth = useSelector((state) => state);

dispatch action
```````````````````````
import { useDispatch} from 'react-redux';
const dispatch = useDispatch();


INDEX.js
```````````````````````

import { Provider } from 'react-redux';
import store from './Store'

ReactDOM.render(
  <Provider store={store} >
  <React.StrictMode>
    <App />
  </React.StrictMode>
  </Provider>,
  document.getElementById('root')
);


````````````````````````````````````````````````````````````````````````````````````````````````````````````
id
-----
* import {useParams} from "react-router-dom";
  const {id} = useParams(); 
  <h1> Hello {id} </h1>

* clubList.js =>

  <Link to={`/${clubsData.id}`} className="disableLink">

  App.js =>

  <Router>
        <Switch>
        <Route path="/:id" component={ClubDetails} />
        </Switch>
  </Router>

````````````````````````````````````````````````````````````````````````````````````````````````````````````


react-toastify npm
`````````````````````
index.js =>   import "react-toastify/dist/ReactToastify.css";
App.js => 
          <ToastContainer />      -->popup 
add.js => 
    
  import {toast} from "react-toastify";
	
  const [email, setEmail]= useState("");

  const checkEmail = products.find((product) => product.email === email)
  const checkNumber = products.find((product) => product.number === parseInt(number))
  
   if(!name || !email || !number){
      return toast.warning("please fill all fields")
   }
   
   if(checkEmail){
      return toast.error("Email already exists")
    }
    if(checkNumber){
       return toast.error("Number already exists")
     } 


`````````````````````````````````````````````````````````````````````````````````````````````````````````````

Axios
`````````
import axios from 'axios';
import { api } from "./urlConfig";

const token = window.localStorage.getItem('token');
console.log(token);

const axiosInstance = axios.create({
    baseURL: api,
    headers: {
        'Authorization': token ? `Bearer ${token}` : ''
    }
});

export default axiosInstance;
	
	
	
	
	
ARRAY FILTER
```````````````````````````````````
	
import React from 'react';

const names = ['James', 'John', 'Paul', 'Ringo', 'George'];

function App() {
  return (
    <div>
      {names.filter(name => name.includes('J')).map(filteredName => (
        <li>
          {filteredName}
        li>
      ))}
    div>
  );
}

export default App;
		
		
*
```````````````````````````````````
		

🆕 Beginners
🔥 Advanced
📝 Tutorials
 
React Filter: Filtering Arrays in React (With Examples)
By James Dietrich
Beginner React Tutorials
How to filter an array in React with examples.
How can we use React to filter an array? What is filter in React? I’ll answer all of these questions plus show you some examples to get you started.

What is Filter in React?
Filtering in React is pretty much what it says it is. It’s the process of looping through an array and including or excluding elements inside that array based on a condition that you provide.

The caveat here is that we’re not actually filtering using React. Filter is a JavaScript function that we can perform on an array type object. Remember, we use JavaScript to write React. None of these methods are special to React. React is just the UI library.

Example 1: Filter an Array of Strings in React

This first example is quite a common scenario. Picture the scene: you’re creating a search filter component to allow your users to search a list of names.

Your users type into a text field to filter that array of names based on what they’re typing into that text field. It’s quite a common input element these days.

Let’s start off with a hard-coded array of strings. Names that you might recognize (plus a small addition):

const names = ['James', 'John', 'Paul', 'Ringo', 'George'];
Let’s assume that you want to only display names that include the letter ‘J’ in them.


To do that, we use the filter function. We can perform the filter function inside of our JSX in a React component like so:

App.js
<div>
  {names.filter(name => name.includes('J')).map(filteredName => (
    <li>
      {filteredName}
    li>
  ))}
div>
Let’s break down the code above.

First, we define our JSX inside by opening a new curly bracket inside of a

element. Then, we specify the array for which we want to perform the filter function on.
Because we are performing a function we open up a set of parentheses. This is where we want to set our condition for the filter: in this case, if the element in the array includes the letter ‘J’.

Did you know that you can chain array function in JavaScript, such as filter, map, and reduce? This can save many lines of code, although it may be difficult to understand for those new to coding.

Finally, we perform another function after the filter function: the map function. This is how we can output the array elements that match the filter condition to the actual user interface inside of li tags.

Let’s see the complete React component code:

App.js
import React from 'react';

const names = ['James', 'John', 'Paul', 'Ringo', 'George'];

function App() {
  return (
    <div>
      {names.filter(name => name.includes('J')).map(filteredName => (
        <li>
          {filteredName}
        li>
      ))}
    div>
  );
}

export default App;
A quick tip that I thought worth mentioning: notice how I write the names array declaration outside of the React component? That’s because I don’t need the array to be declared each time the component re-renders (which will be a lot).

Example 2: Filter an Array of Objects by Value in React
Let’s explore how to filter an array of objects in React, based on a value inside of those objects. It’s similar to what we did previously with the array of strings, just with an extra step.

Our array of names has expanded, and so I renamed the array to be named people. It now looks like this:

const people = [
  {
    name: 'James',
    age: 31,
  },
  {
    name: 'John',
    age: 45,
  },
  {
    name: 'Paul',
    age: 65,
  },
  {
    name: 'Ringo',
    age: 49,
  },
  {
    name: 'George',
    age: 34,
  }
];
		
	
		
<div>
  {people.filter(person => person.age < 60).map(filteredPerson => (
    <li>
      {filteredPerson.name}
    li>
  ))}
div>	
	
	    
	    
	    
	    
	    
	   
CHECKBOX
``````````````````````````````````````
	    
const { useState } = React; // --> for inline use
// import React, { useState } from 'react';  // --> for real project


const App = () => {
  const [checked, setChecked] = useState(false)
  const handleClick = () => setChecked(!checked)
  
  return <input onClick={handleClick} checked={checked} type="checkbox" />
};	   
		
		
		
		
