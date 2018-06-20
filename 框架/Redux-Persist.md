## Redux Persist

```
// store.js

import { createStore } from 'redux'
import { persistStore, persistReducer } from 'redux-persist'
import storage from 'redux-persist/lib/storage' // defaults to localStorage for web and AsyncStorage for react-native

import rootReducer from './reducers'

const persistConfig = {
  key: 'root',
  storage,
}

const persistedReducer = persistReducer(persistConfig, rootReducer)

export default () => {
  let store = createStore(persistedReducer)
  let persistor = persistStore(store)
  return { store, persistor }
}
```
### State Reconciler 状态协调器
State reconcilers define how incoming state is merged in with initial state. It is critical to choose the right state reconciler for your state. There are three options that ship out of the box, lets look at how each operates:

1. hardSet (import hardSet from 'redux-persist/lib/stateReconciler/hardSet') This will hard set incoming state. This can be desirable in some cases where persistReducer is nested deeper in your reducer tree, or if you do not rely on initialState in your reducer.
```
incoming state: { foo: incomingFoo }
initial state: { foo: initialFoo, bar: initialBar }
reconciled state: { foo: incomingFoo } // note bar has been dropped
```

2. autoMergeLevel1 (default) This will auto merge one level deep. Auto merge means if the some piece of substate was modified by your reducer during the REHYDRATE action, it will skip this piece of state. Level 1 means it will shallow merge 1 level deep.
```
incoming state: { foo: incomingFoo }
initial state: { foo: initialFoo, bar: initialBar }
reconciled state: { foo: incomingFoo, bar: initialBar } // note incomingFoo overwrites initialFoo
```
3. autoMergeLevel2 (import autoMergeLevel2 from 'redux-persist/lib/stateReconciler/autoMergeLevel2') This acts just like autoMergeLevel1, except it shallow merges two levels
```
incoming state: { foo: incomingFoo }
initial state: { foo: initialFoo, bar: initialBar }
reconciled state: { foo: mergedFoo, bar: initialBar } // note: initialFoo and incomingFoo are shallow merged
```

### Blacklist & Whitelist


```
// BLACKLIST
const persistConfig = {
  key: 'root',
  storage: storage,
  blacklist: ['navigation'] // navigation will not be persisted
};

// WHITELIST
const persistConfig = {
  key: 'root',
  storage: storage,
  whitelist: ['navigation'] // only navigation will be persisted
};
```

