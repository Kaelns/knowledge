# 03 · React & ecosystem

Matrix: **JavaScript Top Frameworks** (Core, Intermediate) · **ReactJS** (Intermediate).
My stack: React + React Compiler, Redux, React Query, Orval, shadcn/ui, Tailwind, Storybook.
Old notes: [Programming → React / Redux](../../Evernote/Programming/Programming.md#react).

**Pass 1 priority.** P1: re-render causes, keys, `useEffect` + cleanup, memo/useMemo/useCallback (+ Compiler), controlled vs uncontrolled, Context, Redux/RTK + thunk, React Query basics. P2: refs/forwardRef/callback refs, router, composition patterns, error boundaries, React 19. P3: Fiber, RSC, framework comparison.

## Checklist

### Fundamentals (+, the base every question builds on)
📚 Old notes: [[Programming#React|React (old notes)]] · [[Core js 2#Critical rendering path|Critical rendering path]]
- [x] 🟡 SHALLOW JSX → `React.createElement` / `jsx()`, elements vs components, pure render
```Typescript
// Output: Modern Compiled Code (Automatically injected by compiler)
import { jsx as _jsx } from "react/jsx-runtime";


// Output: Classic Compiled Code
const element = React.createElement(
  'div', 
  { id: 'greeting', className: 'highlight' }, 
  React.createElement('h1', null, 'Hello World')
);

const element = _jsx("div", {
  id: "greeting",
  className: "highlight",
  children: _jsx("h1", { children: "Hello World" })
});

```
- [x] 🔴 CORE Render & commit phases, **reconciliation**, why **keys** matter (index as key bug)
- render (react updates) -> commit (to dom)

| Situation                                      | Render Phase Action                             | Commit Phase Action                           | State (useState)                         |
| :--------------------------------------------- | :---------------------------------------------- | :-------------------------------------------- | :--------------------------------------- |
| Different Node/Component Type                  | Old Fiber destroyed, new Fiber built            | Old DOM node deleted, new DOM node created    | Wiped completely                         |
| Same Type, Same Key, Changed Props             | Fiber updated, function called to diff children | DOM attributes/text patched                   | Preserved                                |
| Same Key, Reordered                            | Fiber matched from Map, tagged with Placement   | DOM node moved via insertBefore               | Preserved                                |
| Key Changed (or Random Key like Math.random()) | Old Fiber unmounted, new Fiber mounted          | Old DOM deleted, new DOM created from scratch | Wiped completely                         |
| Index as Key (Reordered/Prepended)             | Fibers matched to wrong items                   | In-place DOM attribute patching (wasteful)    | Corrupted (stays tied to index position) |
- [x] 🔴 CORE What triggers a re-render (state, parent re-render, context), batching, state as a snapshot, updater functions `setX(x => x + 1)`
- state as a snapshot - means that state is incapsulated during render cycle, it can't be updated right away by state functions. Instead of updating everything immediately batching used
- [x] 🔴 CORE `useEffect`: dependencies, cleanup, StrictMode double-invoke, race conditions; "You might not need an effect"
- [x] 🟡 SHALLOW `useLayoutEffect` vs `useEffect`, `useId`, `useTransition`/`useDeferredValue`, `useSyncExternalStore` (how Redux subscribes)
- [x] 🔴 CORE Custom hooks: rules of hooks and why they exist

### React DOM (A2)
📚 Old notes: [[Core js 1#Event delegation|Delegation (how SyntheticEvent works)]] · [[Core js 1#Event phases|Event phases]]
- [x] 🔴 CORE `useRef` (DOM access + mutable value without re-render), `createRef` (class components)
- [x] 🟡 SHALLOW **Ref forwarding**: `forwardRef` + `useImperativeHandle`; React 19: `ref` is a regular prop
- useImperativeHandle

```Typescript
function MyInput({ ref }) {
  const inputRef = useRef(null);

  useImperativeHandle(ref, () => {
    return {
      focus() {
        inputRef.current.focus();
      },
      scrollIntoView() {
        inputRef.current.scrollIntoView();
      },
    };
  }, []);

  return <input ref={inputRef} />;
}
```
- [x] ⚪ SKIP **Callback refs** (measure node, ref to dynamic list), React 19 ref cleanup functions
	- [ ] ref to dynamic list
- cleanup + measure
```Typescript
  const measureRef = useCallback((node: HTMLElement | null) => {
    if (!node) return;

    const observer = new ResizeObserver((entries) => {
      for (const entry of entries) {
        setDimensions({
          width: entry.contentRect.width,
          height: entry.contentRect.height,
        });
      }
    });

    observer.observe(node);

    // React 19: Return a dedicated cleanup function
    return () => {
      observer.disconnect();
    };
  }, []);
```
- [ ] 🟡 SHALLOW **SyntheticEvent**: wrapper over native events, delegation at root, `e.nativeEvent`, no pooling since 17

### Forms (A2)
- [ ] 🔴 CORE Controlled vs uncontrolled (`value`+`onChange` vs `defaultValue`+ref/FormData), trade-offs
- [x] 🟡 SHALLOW React Hook Form (+, uncontrolled under the hood) + zod validation; shadcn `<Form>` is built on it
- know about them shallowly, never tried
- [ ] 🟡 SHALLOW React 19 form actions: `<form action={fn}>`, `useActionState`, `useFormStatus`, `useOptimistic` (+)

### Context (A2)
- [x] 🔴 CORE When to use (theme, auth, locale; low-frequency data) vs not (frequently changing state → store)
- [x] 🟡 SHALLOW `createContext`, `Provider`, `useContext`, legacy `Context.Consumer`; React 19 `<Context>` as provider and `use(Context)`
	- [ ] `Context.Consumer`
```jsx
import { createContext, useState } from 'react';

// Still created the exact same way
export const ThemeContext = createContext();

export function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');

  const toggleTheme = () => {
    setTheme((prev) => (prev === 'light' ? 'dark' : 'light'));
  };

  // React 19: Use `<ThemeContext>` directly instead of `<ThemeContext.Provider>`
  return (
    <ThemeContext value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext>
  );
}

// Component file
import { use } from 'react';
import { ThemeContext } from './ThemeProvider';

export function ThemeButton({ showButton }) {
  // 1. Standard usage (similar to useContext)
  const { theme, toggleTheme } = use(ThemeContext);

  // 2. React 19 Superpower: Conditional reading!
  // This would crash if you used legacy useContext().
  if (!showButton) {
    return null; 
  }

  const { theme, toggleTheme } = use(ThemeContext);

  return (
    <button 
      onClick={toggleTheme} 
      style={{ background: theme === 'light' ? '#fff' : '#333' }}
    >
      Toggle to {theme === 'light' ? 'Dark' : 'Light'} Mode
    </button>
  );
}


```
- [x] 🔴 CORE Re-render problem: every consumer re-renders on value change → memoize value, split contexts

### Performance (A2)
📚 Old notes: [[Core js 2#Layout, Paint and Composite|Reflow / repaint / composite]] · [[Core js 2#Dynamic imports|Dynamic import (code splitting)]]
- [x] 🔴 CORE `React.memo`, `useMemo`, `useCallback`: what they do, when they're useless (new object props, cheap renders)
- [x] 🔴 CORE **React Compiler**: auto-memoization at build time, rules it relies on (pure components, rules of hooks), `"use no memo"`, ESLint plugin; why manual memo is mostly unnecessary with it

| Task | Before React Compiler | With React Compiler |
| :-- | :-- | :-- |
| Pass callback to child | const cb = useCallback(() => ..., [deps]) | const cb = () => ... |
| Pass object to child | const obj = useMemo(() => ({ a }), [a]) | const obj = { a } |
| Prevent child re-renders | export default React.memo(MyComponent) | export default MyComponent |
| Expensive calculation | useMemo(() => math(a), [a]) | math(a) |
| Logic after early returns | Refactor into multiple components | Write it right after the if |
- [x] 🟡 SHALLOW Keys, list virtualization (`react-window`/TanStack Virtual), code splitting (`React.lazy` + `Suspense`)
	- [ ] `react-window` - never used, only tanstack virtual. REnders only limited amount of lines for list instead of thousands
- [x] 🟡 SHALLOW **Profiler**: React DevTools Profiler (flamegraph, "why did this render"), `<Profiler onRender>` API
	- [ ] `<Profiler onRender>` API

### Redux (A2)
📚 Old notes: [[Programming#Redux|Redux (old notes)]] · [[Core js 2#Pattern categories|Patterns: Observer, Command]]
- [x] 🔴 CORE Core: store, actions, reducers, immutability, one-way data flow, selectors
- one-way data flow
	- action dispatched -> store runs reducer -> store updates tree -> ui rerenders
- [x] 🟡 SHALLOW Presentational vs container components; `Provider`, legacy `connect(mapState, mapDispatch)`, `useSelector`, `useDispatch`
- Presentational vs container components - legacy things with connect, now the boundaries are blurred
- [x] 🔴 CORE **Redux Toolkit**: `configureStore`, `createSlice` (Immer), `createSelector` (reselect memoization), `createEntityAdapter`
- createEntityAdapter - know about it, never used
- [x] 🔴 CORE **Async flow**: middleware concept (`store => next => action`), **Redux Thunk**, `createAsyncThunk` (pending/fulfilled/rejected); awareness of saga/listener middleware
	- [ ] saga - **legacy**, didn't check as not the important
- middleware
```typescript
const myMiddleware = (store) => (next) => (action) => {
  // 1. Custom logic BEFORE the action moves forward (e.g., logging, async calls)
  const result = next(action); // 2. Pass action to the next middleware or root reducer
  // 3. Custom logic AFTER the reducer has processed the action
  return result;
};

```
- listener - cool concept, used it to listen on some state from selector to run effect (==which is faster the react effect==) that has access to `action`, full `prev state`
	- They can react to state changes with `predicate`. You can compare current and prev state
- createAsyncThunk - just a œthunk that automatically generates and dispatches three lifecycle action

```typescript
import { createAsyncThunk } from '@reduxjs/toolkit';
import axios from 'axios';

export const fetchUserById = createAsyncThunk(
  // 1. Action Type Prefix
  'users/fetchById',

  // 2. Payload Creator
  async (userId: string, thunkAPI) => {
    try {
      const response = await axios.get(`/api/users/${userId}`);
      // Whatever you return here becomes the `action.payload` 
      // of the `fulfilled` action!
      return response.data;
    } catch (error: any) {
      // Custom error handling (More on this below)
      return thunkAPI.rejectWithValue(error.response.data);
    }
  }
);

// Then

const userSlice = createSlice({
  name: 'users',
  initialState,
  reducers: {}, // regular synchronous reducers go here
  extraReducers: (builder) => {
    builder
      // 1. Request started
      .addCase(fetchUserById.pending, (state) => {
        state.loading = true;
        state.error = null;
      })
      // 2. Request succeeded
      .addCase(fetchUserById.fulfilled, (state, action) => {
        state.loading = false;
        state.user = action.payload; // Data returned from your thunk
      })
      // 3. Request failed
      .addCase(fetchUserById.rejected, (state, action) => {
        state.loading = false;
        state.error = action.payload as string; // Set by rejectWithValue
      });
  },
});
```
- [x] 🟡 SHALLOW RTK Query vs React Query: what each one is for

| Feature                          | React Query (TanStack)                                                     | RTK Query                                                                                    |
| -------------------------------- | -------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| **Primary Ecosystem**            | Decoupled / Standalone (supports React, Vue, Svelte, Solid)                | Tightly coupled with Redux Toolkit                                                           |
| **Code Architecture**            | **Decentralized:** Defined inline inside components or local custom hooks. | **Centralized:** Defined ahead of time in a global "API slice".                              |
| **Setup & Boilerplate**          | **Minimal:** Wrap the app in a provider and start writing hooks.           | **Structured:** Requires configuring a store, slice, and middleware.                         |
| **Cache Strategy**               | Key-based caching with fine-tuned "staleness" timers.                      | Redux store-based caching with referential equality.                                         |
| **Pagination & Infinite Scroll** | **Excellent:** Robust, native out-of-the-box features.                     | Supported, but often requires more custom implementation logic.                              |
| **Code Generation**              | Not a primary feature.                                                     | Has advanced tools to auto-generate fully-typed hooks from OpenAPI specs or GraphQL schemas. |
| Suspence                         | yes                                                                        | no                                                                                           |
| Devtools                         | advanced                                                                   | basic                                                                                        |
| Support                          | more                                                                       | less                                                                                         |
| Query gen                        | has, needs more setup                                                      | has                                                                                          |

### Server state (+, my project)
- [x] 🔴 CORE **TanStack Query**: server vs client state, `queryKey`, `staleTime` vs `gcTime`, refetch triggers, `invalidateQueries`, mutations, optimistic updates, `enabled`, pagination/infinite queries, Suspense mode
- `staleTime` vs `gcTime`

|   |   |   |
|---|---|---|
|**Feature**|**staleTime**|**gcTime** **(formerly** **cacheTime****)**|
|**What it tracks**|**Validity / Freshness** of the data.|**Lifetime in memory** of inactive data.|
|**Default Value**|0 (Data is immediately stale).|5 * 60 * 1000 (5 minutes).|
|**When timer starts**|As soon as the network request successfully finishes.|As soon as the component unmounts (0 active observers).|
|**When it expires**|Data becomes **stale**. The next time a component mounts or focus triggers, it pulls from cache _and_ refetches in the background.|Data is **permanently deleted** from the cache. The next mount triggers a hard loading state (isLoading).|

- [x] 🟡 SHALLOW **Orval**: generate typed hooks/clients + MSW mocks from OpenAPI; how it fits the pipeline
- [ ] 🔴 CORE Why Redux + React Query together: Redux for client/UI state, Query for server cache

### Routing (A2)
📚 Old notes: [[Core js 2#History API|History API]] · [[Core js 2#Location|Location]]
- [x] 🟡 SHALLOW React Router: JSX config (`<Routes><Route>`) vs object config (`createBrowserRouter`), nested routes + `<Outlet>`, params, `useNavigate`, `useSearchParams`, loaders/actions (data router), protected routes, lazy routes
- **Routes vs object**: Object allows

1. **Fetch data before rendering:** It starts fetching data for `/dashboard` the exact millisecond the user clicks the link, preventing UI loading spinners.
2. **Handle Optimistic UI and Form Actions:** Forms can submit directly to router actions without manually writing `fetch()` calls.
3. **Support Server-Side Rendering (SSR):** It seamlessly transitions to full-stack frameworks (like Remix or modern React Router v7).

- `useSearchParams` - returns special object URLSearchParams and setSeachParams. Works like map
- [x] 🟡 SHALLOW History: `BrowserRouter` (History API) vs `HashRouter` vs memory history; server must fallback to `index.html`
	- memory history - doesn't interact with url, stores everything in array. Used for testing (there is no browser and doesn't pollute the window obj), where there is no url (react native, electron, web workers), modals with its own nav

| Feature / Aspect               | BrowserRouter                                            | HashRouter                                                   | MemoryRouter                                                |
| :----------------------------- | :------------------------------------------------------- | :----------------------------------------------------------- | :---------------------------------------------------------- |
| Underlying API                 | HTML5 History API (pushState, popstate)                  | URL Hash (window.location.hash, hashchange)                  | Plain JavaScript Array / Stack                              |
| URL Example                    | example.com/users/42                                     | example.com/#/users/42                                       | example.com (URL never changes)                             |
| Syncs with Address Bar?        | Yes — fully controls and updates the URL                 | Yes — only updates the part after #                          | No — completely invisible to the browser                    |
| Browser Back/Forward Buttons   | Supported natively                                       | Supported natively                                           | No (only via in-memory controls)                            |
| Requires Server Configuration? | YES (Server must rewrite requests to index.html)         | NO (Browser never sends the # part to the server)            | NO (Never touches the browser/server)                       |
| SEO & Crawlers                 | Optimal (Search engines read it as normal web pages)     | Poor (Crawlers often ignore or penalize hash fragments)      | N/A (Used outside searchable contexts)                      |
| Direct URL Sharing / Bookmarks | Works seamlessly (if server fallback is configured)      | Works seamlessly (out of the box)                            | Impossible (State is lost on reload)                        |
| Page Reload (F5)               | Retains route (if server fallback exists)                | Retains route natively                                       | Resets to default initial state                             |
| Primary Use Cases              | Standard modern web apps, e-commerce, SaaS, public sites | Electron (desktop apps), GitHub Pages, strict legacy servers | Unit tests (Vitest/Jest), React Native, embedded UI widgets |

### Composition & design systems (A2)
📚 Old notes: [[Core js 2#Pattern categories|Patterns: Composite, Decorator (HOC)]] · [[Core js 2#SOLID|SOLID with React examples]]
- [x] 🔴 CORE Composition patterns: `children`, slots via props, **compound components** (shadcn `Dialog`), render props, HOC, custom hooks; lifting state, prop drilling solutions
- [x] 🟡 SHALLOW Component communication: props down / callbacks up, context, store, events
- [x] 🟡 SHALLOW Design systems: tokens, shadcn/ui (copy-in components on Radix primitives + Tailwind), `cva` variants, `cn()`; Storybook for docs/visual testing
	- [ ] tokens
- [x] 🟡 SHALLOW Error boundaries; Suspense boundaries
- [ ] 🟡 SHALLOW Framework comparison (one paragraph each): React vs Vue (reactivity system) vs Angular (DI, full framework)

### Deep dive (Pass 2)
- [ ] ⚪ SKIP Fiber architecture, lanes/priorities, concurrent rendering
- [ ] ⚪ SKIP Server Components vs Client Components, `"use client"`, SSR + hydration (link to 06 rendering), Next.js App Router basics

## Key questions

1. 🔴 CORE What causes a re-render? How do you prevent unnecessary ones, and how does React Compiler change that?
2. 🔴 CORE `useMemo` vs `useCallback` vs `memo`: when is memoization harmful?
3. 🔴 CORE Controlled vs uncontrolled inputs: which does React Hook Form use and why?
4. 🔴 CORE Why are keys needed? What breaks with `index` as key?
5. 🔴 CORE When would you put state in Context, Redux, React Query, URL, or local state?
6. 🔴 CORE Explain the Redux data flow and how a thunk works under the hood.
7. 🔴 CORE `useEffect` cleanup: give an example of a race condition and fix it.
8. 🟡 SHALLOW What is `forwardRef` / callback ref used for?
9. 🔴 CORE React Query: `staleTime` vs `gcTime`, how invalidation works.
10. 🟡 SHALLOW What's new in React 19 that you used / would use?
	 ref as prop
	 use with Context behind if else
11. 🟡 SHALLOW Compound component pattern: how is shadcn Dialog built?
12. 🟡 SHALLOW How does `useSelector` decide to re-render? Why can `createSelector` matter?

## Hands-on (in the lab repo)

- [ ] ⚪ SKIP Search with debounced input + React Query + `AbortSignal` (query function gets `signal`)
- [ ] ⚪ SKIP Todo slice with RTK + `createAsyncThunk` + memoized selector
- [ ] ⚪ SKIP Router with nested layout, protected route and lazy page
- [ ] ⚪ SKIP Compound `<Tabs>` component with context; a `forwardRef`/ref-as-prop `Input`
- [ ] ⚪ SKIP Profile a slow list with React DevTools Profiler; fix it

## Links

- [react.dev Learn](https://react.dev/learn), esp. *Managing State*, *Escape Hatches*, [You Might Not Need an Effect](https://react.dev/learn/you-might-not-need-an-effect)
- [React 19 release post](https://react.dev/blog/2024/12/05/react-19) · [React Compiler](https://react.dev/learn/react-compiler)
- [Redux Essentials](https://redux.js.org/tutorials/essentials/part-1-overview-concepts)
- [TkDodo's blog (React Query)](https://tkdodo.eu/blog/practical-react-query)
- [patterns.dev](https://www.patterns.dev/): React patterns + rendering patterns