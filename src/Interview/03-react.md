# 03 · React & ecosystem

Matrix: **JavaScript Top Frameworks** (Core, Intermediate) · **ReactJS** (Intermediate).
My stack: React + React Compiler, Redux, React Query, Orval, shadcn/ui, Tailwind, Storybook.
Old notes: [Programming → React / Redux](../../Evernote/Programming/Programming.md#react).

## Checklist

### Fundamentals (+, the base every question builds on)
- [ ] JSX → `React.createElement` / `jsx()`, elements vs components, pure render
- [ ] Render & commit phases, **reconciliation**, why **keys** matter (index as key bug)
- [ ] What triggers a re-render (state, parent re-render, context), batching, state as a snapshot, updater functions `setX(x => x + 1)`
- [ ] `useEffect`: dependencies, cleanup, StrictMode double-invoke, race conditions; "You might not need an effect"
- [ ] `useLayoutEffect` vs `useEffect`, `useId`, `useTransition`/`useDeferredValue`, `useSyncExternalStore` (how Redux subscribes)
- [ ] Custom hooks: rules of hooks and why they exist

### React DOM (A2)
- [ ] `useRef` (DOM access + mutable value without re-render), `createRef` (class components)
- [ ] **Ref forwarding**: `forwardRef` + `useImperativeHandle`; React 19: `ref` is a regular prop
- [ ] **Callback refs** (measure node, ref to dynamic list), React 19 ref cleanup functions
- [ ] **SyntheticEvent**: wrapper over native events, delegation at root, `e.nativeEvent`, no pooling since 17

### Forms (A2)
- [ ] Controlled vs uncontrolled (`value`+`onChange` vs `defaultValue`+ref/FormData), trade-offs
- [ ] React Hook Form (+, uncontrolled under the hood) + zod validation; shadcn `<Form>` is built on it
- [ ] React 19 form actions: `<form action={fn}>`, `useActionState`, `useFormStatus`, `useOptimistic` (+)

### Context (A2)
- [ ] When to use (theme, auth, locale; low-frequency data) vs not (frequently changing state → store)
- [ ] `createContext`, `Provider`, `useContext`, legacy `Context.Consumer`; React 19 `<Context>` as provider and `use(Context)`
- [ ] Re-render problem: every consumer re-renders on value change → memoize value, split contexts

### Performance (A2)
- [ ] `React.memo`, `useMemo`, `useCallback`: what they do, when they're useless (new object props, cheap renders)
- [ ] **React Compiler**: auto-memoization at build time, rules it relies on (pure components, rules of hooks), `"use no memo"`, ESLint plugin; why manual memo is mostly unnecessary with it
- [ ] Keys, list virtualization (`react-window`/TanStack Virtual), code splitting (`React.lazy` + `Suspense`)
- [ ] **Profiler**: React DevTools Profiler (flamegraph, "why did this render"), `<Profiler onRender>` API

### Redux (A2)
- [ ] Core: store, actions, reducers, immutability, one-way data flow, selectors
- [ ] Presentational vs container components; `Provider`, legacy `connect(mapState, mapDispatch)`, `useSelector`, `useDispatch`
- [ ] **Redux Toolkit**: `configureStore`, `createSlice` (Immer), `createSelector` (reselect memoization), `createEntityAdapter`
- [ ] **Async flow**: middleware concept (`store => next => action`), **Redux Thunk**, `createAsyncThunk` (pending/fulfilled/rejected); awareness of saga/listener middleware
- [ ] RTK Query vs React Query: what each one is for

### Server state (+, my project)
- [ ] **TanStack Query**: server vs client state, `queryKey`, `staleTime` vs `gcTime`, refetch triggers, `invalidateQueries`, mutations, optimistic updates, `enabled`, pagination/infinite queries, Suspense mode
- [ ] **Orval**: generate typed hooks/clients + MSW mocks from OpenAPI; how it fits the pipeline
- [ ] Why Redux + React Query together: Redux for client/UI state, Query for server cache

### Routing (A2)
- [ ] React Router: JSX config (`<Routes><Route>`) vs object config (`createBrowserRouter`), nested routes + `<Outlet>`, params, `useNavigate`, `useSearchParams`, loaders/actions (data router), protected routes, lazy routes
- [ ] History: `BrowserRouter` (History API) vs `HashRouter` vs memory history; server must fallback to `index.html`

### Composition & design systems (A2)
- [ ] Composition patterns: `children`, slots via props, **compound components** (shadcn `Dialog`), render props, HOC, custom hooks; lifting state, prop drilling solutions
- [ ] Component communication: props down / callbacks up, context, store, events
- [ ] Design systems: tokens, shadcn/ui (copy-in components on Radix primitives + Tailwind), `cva` variants, `cn()`; Storybook for docs/visual testing
- [ ] Error boundaries; Suspense boundaries
- [ ] Framework comparison (one paragraph each): React vs Vue (reactivity system) vs Angular (DI, full framework)

### Deep dive (Phase 2)
- [ ] Fiber architecture, lanes/priorities, concurrent rendering
- [ ] Server Components vs Client Components, `"use client"`, SSR + hydration (link to 06 rendering), Next.js App Router basics

## Key questions

1. What causes a re-render? How do you prevent unnecessary ones, and how does React Compiler change that?
2. `useMemo` vs `useCallback` vs `memo`: when is memoization harmful?
3. Controlled vs uncontrolled inputs: which does React Hook Form use and why?
4. Why are keys needed? What breaks with `index` as key?
5. When would you put state in Context, Redux, React Query, URL, or local state?
6. Explain the Redux data flow and how a thunk works under the hood.
7. `useEffect` cleanup: give an example of a race condition and fix it.
8. What is `forwardRef` / callback ref used for?
9. React Query: `staleTime` vs `gcTime`, how invalidation works.
10. What's new in React 19 that you used / would use?
11. Compound component pattern: how is shadcn Dialog built?
12. How does `useSelector` decide to re-render? Why can `createSelector` matter?

## Hands-on (in the lab repo)

- [ ] Search with debounced input + React Query + `AbortSignal` (query function gets `signal`)
- [ ] Todo slice with RTK + `createAsyncThunk` + memoized selector
- [ ] Router with nested layout, protected route and lazy page
- [ ] Compound `<Tabs>` component with context; a `forwardRef`/ref-as-prop `Input`
- [ ] Profile a slow list with React DevTools Profiler; fix it

## Links

- [react.dev Learn](https://react.dev/learn), esp. *Managing State*, *Escape Hatches*, [You Might Not Need an Effect](https://react.dev/learn/you-might-not-need-an-effect)
- [React 19 release post](https://react.dev/blog/2024/12/05/react-19) · [React Compiler](https://react.dev/learn/react-compiler)
- [Redux Essentials](https://redux.js.org/tutorials/essentials/part-1-overview-concepts)
- [TkDodo's blog (React Query)](https://tkdodo.eu/blog/practical-react-query)
- [patterns.dev](https://www.patterns.dev/): React patterns + rendering patterns
