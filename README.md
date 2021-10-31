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
