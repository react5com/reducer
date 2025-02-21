# React Reducer SSR Documentation

React Reducer SSR is a library that serves as an alternative to Redux for managing state in React applications. It leverages built-in React hooks and the context API, offering support for Server-Side Rendering (SSR), selectors, and async actions.

## Installation

You can easily install `@react5/reducer` using either Yarn or npm:

```bash
# Using Yarn
yarn add @react5/reducer

# Using npm
npm install @react5/reducer --save
```

## Getting Started

To get started with `@react5/reducer`, you need to import and configure it in your application. Here's a basic setup example:

```jsx
import React from 'react';
import { ReducerProvider } from '@react5/reducer';
import { reducers } from "./context";

function App() {
  return (
    <ReducerProvider reducer={reducers} initialState={{/* Your initial state */}}>
      {/* Your application components */}
    </ReducerProvider>
  );
}

export default App;
```

## Usage

### Creating Reducers

`@react5/reducer` allows you to create reducers to manage your application's state. Here's an example of creating a reducer:

```jsx
import { TypedUseSelectorHook, useStateSelectorT, combineReducers } from '@react5/reducer'
import { preferencesReducer } from './preferences.reducer'
import { usersReducer } from './users.reducer'

export const reducers = combineReducers({
  preferences: preferencesReducer,
  users: usersReducer
})

export type RootState = ReturnType<typeof reducers>
export const useStateSelector: TypedUseSelectorHook<RootState> = useStateSelectorT
```

```jsx
import type { AnyAction } from "@react5/reducer";

export interface IUsersState {
  userList?: string[]
}
export function usersReducer(draft: IUsersState, action: AnyAction): IUsersState {
  switch (action.type) {
    case 'GET_ALL_USERS': {
      draft.userList = action.userList;
    } break;
  }
  return draft
}
```

### Immutability

`@react5/reducer` uses 'immer' internally, thus state mutation is allowed. No need to recreate a new object each time state changes.

### Using Selectors

Selectors help you access specific parts of your state. Here's how you can use selectors:

```jsx
import { useStateSelector } from "./context";

const MyComponent = () => {
  const users = useStateSelector(root => root.users);

  return (
    <div>
      {/* Render your selected data */}
    </div>
  );
};
```

### Async Actions

You can perform asynchronous actions with `@react5/reducer` as well. Here's an example:

```jsx
import { useDispatch } from '@react5/reducer';

const MyComponent = () => {
  const dispatch = useDispatch();

  const fetchData = async () => {
    await dispatch(usersActions.getUsers());
  };

  return (
    <div>
      <button onClick={fetchData}>Fetch Data</button>
    </div>
  );
};
```

```jsx
export const usersActions = {
  getUsers
}

async function getUsers(companyCode: string) {
  try {
    const response = await fetch("some_url");
    const userList = await userList.json();
    return { type: 'GET_ALL_USERS', userList }
  }
  catch(err) {
    return { type: 'GET_ALL_USERS_FAILED', err }
  }
}
```

## Server-Side Rendering (SSR) Support

`@react5/reducer` offers support for Server-Side Rendering.
```ts
import { reducers } from "./context";
import { createServerStore, DispatchFunction } from '@react5/reducer'

const store = createServerStore(reducers, {/*initial state here*/} as any);

...
await loadData(store.dispatch);
...
async function loadData(dispatch: DispatchFunction, cookies: any) {
  await dispatch(userActions.getUsers());
}

```

## Example

For the complete example see [react-ssr-ts-scss-rollup](https://github.com/alfed7/react-ssr-ts-scss-rollup) starter template.

## Contributing

We welcome contributions from the community! If you'd like to contribute to `@react5/reducer`, please follow our [contribution guidelines](CONTRIBUTING.md).

## License

This project is licensed under the MIT License. For more information, see the [LICENSE](LICENSE) file.

## Support

For support and bug reports, please [open an issue](https://github.com/alfed7/@react5/reducer/issues) on GitHub.
