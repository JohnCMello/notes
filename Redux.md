Redux
=====

[React Redux (with Hooks) Crash Course](https://www.youtube.com/watch?v=9jULHSe41ls)

Terminologia
============
* Store
* Reducer
* Provider
* useSelector
* Action Creator
* Action
* Action Type
* Action Payload
* Dispatch (useDispatch)

Data Flow
========

UI chama | executa um ACTION CREATOR	

ACTION CREATOR (função) retorna uma ACTION (obj = {type: 'ACTION_TYPE', payload})

ACTION é despachado (DISPATCH) para o REDUCER

REDUCER altera a STORE baseado no ACTION TYPE e ACTION PAYLOAD (opcional)

UI recebe dados da STORE atravez do hook useSelector do react-redux

Store
=====

## Store
	Estado da Aplicação

## Reducer 
	Função que altera o estado baseado no type da ACTION e retorna novo Estado.  
	Recebe 2 parametros (state = initialState, action)

```
|- /src
  |- /store +
    |- /reducers +
       |- accountReducer.js +
```

store/reducers/accountReducer.js

```js 
const accountReducer = ( state = 0, action ) => {
	switch( action.type ) {
		case "DEPOSIT" :
			return state + action.payload ;
		case "WITHDRAW" :
			return state - action.payload ;
		default :
			return state ;
	}
};

export default accountReducer ;
```
```
|- /src
  |- /store
    |- /reducers
      |- accountReducer.js
      |- index.js +
```
store/reducers/index.js

```js
import { combineReducers } from " redux"
import { accountReducer } from "./accountReducer" 

const reducers = combineReducers({
	account: accountReducer
}) 

export default reducers ;
```
```
|- /src
  |- /store
    |- /reducers
      |- accountReducer.js
      |- index.js
    |- store.js +
```
store/store.js

```js
import { createStore } from "redux"
import { reducers } from "./reducers/index"

const defaultState = {}

const store = createStore( reducers , defaultState ); 

export default store ; 
```
Provider
=======
```
|- /src
  |-/store
    |-/action-creators
      |- index.js
    |-/reducers
      |- accountReducer.js
      |- index.js
    |- index.js +	  
    |- store.js
  |- App.js
  |- index.js
```
src/index.js
```js
import { Provider } from "react-redux"
import store from "./store/store" 

ReactDOM.render(
	<React.StrictMode>
		<Provider store={store}>
			<App/>
		</Provider >
	</React.StrictMode>,
	document.getElementById('root')
);
```
useSelector
===========
* useSelector é um hook que recebe um CB.
* esse CB recebe por parametro o estado da aplicação.

* useSelector => state (dentro da store) => reducers (combineReducers) => { key : value } 
* key = nome da propriedade   
* value = retorno do reducer

```
|- /src
  |-/store
    |-/action-creators
      |- index.js
    |-/reducers
      |- accountReducer.js
      |- index.js
    |- index.js +	  
    |- store.js
  |- App.js
  |- index.js
```

src/App.js

```js
import { useSelector } from "react-redux"

function App () {

	const state = useSelector((state) => state) // => { account: 0 };
	const account = useSelector((state) => state.account) // => 0;
		
	return (
		<div className="App">
			<div className="App">
				<h1>{account}</h1>
				<button > Deposit </button>
				<button> > Withdraw </button>
			</div>
		</div>
	) ;
}

export default App
```
Action Creators
===============

* ACTION CREATORS são funções que fazem o DISPATCH de uma ACTION para o REDUCER que interpreta a ACTION e manipula a STORE

```
|- /src
  |-/store
    |-/action-creators +
      |- index.js +
    |- /reducers
      |- accountReducer.js
      |- index.js
	|- store.js
```
store/action-creators/index.js
```js
export const depositMoney = (amount) =>{
	return (dispatch) => {
		dispatch({
			type: "DEPOSIT",
			payload: amount
		})
	}
}

export const withdrawMoney = (amount) =>{
	return (dispatch) => {
		dispatch({
			type: "WITHDRAW",
			payload: amount
		})
	}
}
```
```
|- /src
  |-/store
    |-/action-creators
      |- index.js
    |-/reducers
      |- accountReducer.js
      |- index.js
    |- index.js +	  
    |- store.js
```
store/index.js

```js
export * as actionCreators from "./action-creators/index"
```

useDispatch
===========
```
|- /src
|   |-/store
|   |   |-/action-creators
|   |   |   |- index.js
|   |   |-/reducers
|   |   |   |- accountReducer.js
|   |   |   |- index.js
|   |   |- index.js +	  
|   |   |- store.js
|   |- App.js
```

src/App.js

```js
import { useSelector, useDispatch } from "react-redux"
import { bindActionCreators } from "redux" 
import { actionCreators } from "./Store/index"

function App () {

	const state = useSelector( (state) => state) // => { account: 0 };
	
	const account = useSelector( (state) => state.account) // => 0;

	const dispatch = useDispatch()

	const { depositMoney,  withdrawMoney } = bindActionCreators(actionCreators, dispatch) 
	
	const amountDeposit = 25
	const amountWithdraw = 18
	
	return (
		<div className="App">
			<h1>{account}</h1>
			<button onClick={() => depositMoney(amountDeposit)} > Deposit </button>
			<button onClick={() => withdrawMoney(amountWithdraw)} > Withdraw </button>
		</div>
	) ;
}

export default App
```

Redux Thunk
===========

* Redux Thunk é um middleware que lida com açoes assincronas

* Na criação dos action creators, dispatch é chamado e esse disptach é assincrono  por isso causa um erro:
* Actions must be plain objects. Use custom middleware for async functions.

* O Thunk trata requests assincrons dentro de um ACTION CREATOR.

* Precisamos aplicar o middleware como terceiro parametro para a funçào createStore.

```
|- /src
  |-/store
    |-/action-creators
      |- index.js
    |-/reducers
      |- accountReducer.js
      |- index.js
    |- index.js  
    |- store.js
  |- App.js
```

store/store.js

```js
import { createStore , applyMiddleware } from "redux"
import { reducers } from "./reducers/index"
import { thunk } from "redux-thunk" 

const defaultState = {}

const store = createStore( 
	reducers , 
	defaultState ,
	applyMiddleware(thunk)
); 

export default store ;
```





































