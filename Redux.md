# Redux

action由store.dispatch发出，redux-thunk中间件对store.dispatch进行改造，使之接受函数作为参数

```javascript

//actions.js
export default function onLoadPopularData(storeName, url){
	return dispatch => {
		dispatch({type: types.POPULAR_REFRESH, storeName: storeName});
		let dataStore = new DataStore();
		dataStore.fetchData(url)
			.then(data => {
				//console.log(data);
				handleData(data,dispatch,storeName);
			})
			.catch(error => {
				dispatch({type: types.LOAD_POPULAR_FAIL, storeName: storeName});
			})
	}
}

function handleData(data,dispatch,storeName){
	return dispatch({
		type: types.LOAD_POPULAR_SUCCESS,
		data: data && data.data && data.data.items,
		storeName: storeName
	})
}


//reducer.js
export default function popular(state = defaultState, action){
	switch(action.type){
		case types.POPULAR_REFRESH:
			return {
				...state,
				[action.storeName]: {
					...[action.storeName],
					isLoading: true
				}

			};
		case types.LOAD_POPULAR_SUCCESS:
			return {
				...state,
				[action.storeName]: {
					...[action.storeName],
					items: action.data,
					isLoading: false
				}
			};
		case types.POPULAR_REFRESH:
			return {
				...state,
				[action.storeName]: {
					...[action.storeName],
					isLoading: false
				}
			};
		default:
			return state;
	}
}


//store.js
const middleWares = [
	thunk,
];

export default createStore(reducers, applyMiddleware(...middleWares));


//view.js
const mapStateToProps = state => {
  return {
    popular: state.popular
  }
}

const mapDispatchToProps = dispatch => {
  return {
    onLoadPopularData: (storeName, url) => 					dispatch(actions.onLoadPopularData(storeName,url))
  }
}


const PopularTabPage = connect(mapStateToProps, mapDispatchToProps)(PopularTab);
```


