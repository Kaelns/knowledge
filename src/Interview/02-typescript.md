# 02 · TypeScript

Matrix: **TypeScript** (Core). A1 = Novice → **A2 = Intermediate**. This is one of the biggest level jumps in the matrix.
Old notes: [Core js 2 → TypeScript](../../Evernote/Core%20js%202/Core%20js%202.md#typescript).

**Pass 1 priority.** P1: `any`/`unknown`/`never`, interface vs type, generics + constraints, utility types, discriminated unions, narrowing + type predicates, `strict` flags. P2: enums pitfalls, overloads, mapped/template types, `satisfies`, TS in React, `.d.ts`. P3: decorators order/metadata, compiler optimization, triple-slash.

## Checklist

### A1 baseline (quick check, should be easy)
- [x] Primitives, `void` vs `undefined`, arrays, tuples, objects, `Record`
- Record- Utility type
- [x] Function typing: optional/default/rest params, return types, generic functions
- [x] `interface` vs `type` (declaration merging, `extends` vs `&`, unions only via `type`)
- [x] Classes: `public/private/protected`, `readonly`, `implements` vs `extends`, get/set, parameter properties
- [x] Union vs intersection, `keyof`, `typeof`, `Partial`, `Pick`, `Omit`, `Exclude`, `Extract`
- [x] Narrowing with `typeof`/`instanceof`/`in`, `as` casting; decorator kinds; `.d.ts`, namespace vs module
	- [x] .d.ts - only type info without js logic - for js libs (ts to js and d.ts)
	- [x] namespace vs module
	- [ ] decorator kinds

### A2 (Intermediate)
- [x] **Primitives**: numeric / string / `const enum`, enum pitfalls (reverse mapping, not erasable, prefer `as const` objects / union literals), **`any` vs `unknown`**, literal types, `symbol`, `bigint`
- literal types - sub type of somt other type like "Type", for primitive types
- enums - not erasable as typescript, they generate real object
- [x] **Arrays/objects**: `Record<K,V>` vs `object` vs `{}` vs `Object`, multi-type arrays `(string | number)[]` vs tuples, `readonly` arrays

| Type         | What it represents                           | Allows primitives (string, number, etc.)? | Allows null / undefined? | Can read properties?      |
| :----------- | :------------------------------------------- | :---------------------------------------- | :----------------------- | :------------------------ |
| Record<K, V> | A dictionary with specific key & value types | ❌                                         | ❌                        | ✅ (Typed as V)            |
| object       | Any non-primitive type (func, arr)           | ❌                                         | ❌                        | ❌ (Unknown properties)    |
| {}           | Any value that is not nullish                | ✅ (Surprise!)                             | ❌                        | ❌ (Except Object methods) |
| Object       | Same as {} (the JS Object interface)         | ✅                                         | ❌                        | ❌ (Except Object methods) |
- [x] **Functions**: overloads, `this` parameter, `void` vs `never` returns, `Function` vs `() => any` (why `Function` is bad)
	- [ ] this
	-  function smth (this: YourType) {} // disappears after compiling
	-  method(): this { ...; return this} - infers type automatically 
- Function have no parameter limit
- [x] **Generics**: index signatures, constraints `<T extends …>`, defaults, `const` type parameters `<const T>`, generic `keyof` access `T[K]`
- `const T` makes obj like `as const` - removes narrowing, makes readonly
- [x] **Classes**: `override`, `abstract`, `static`, arrow-function methods (`this` binding vs prototype), generic classes
- `override` - safety when override methods or var in child class
- [x] **Type manipulation**: **discriminated unions** + exhaustive `switch` with `never`, mapped types (`{ [K in keyof T]: … }`, `-readonly`, `?`, key remapping `as`), **template literal types**, type composition
- mapped types
```Typescript
type Getters<T> = { 
	[P in keyof T & string as `get${Capitalize<P>}`]: () => T[P]; 
};

type OnlyStringProperties<T> = { 
	[P in keyof T as T[P] extends string ? P : never]: T[P]; 
};
```

- **discriminated unions** -  allows to make an union with different shapes depending on what value the discriminator has
```Typescript
type Props = {type: "big", value: "smth"} | {type: "small", isDone: boolean}

/// When you still access the diff props without narrowing the type
interface LoadingState {
  status: 'loading';
  data?: undefined;
  error?: undefined;
}

interface SuccessState {
  status: 'success';
  data: string[];
  error?: undefined;
}

interface ErrorState {
  status: 'error';
  data?: undefined;
  error: Error;
}
```
- never - value that can never exist or impossible to reach. Switch case in this example returns never in default so we have to check every possible union type so typescript won't give us an error
- template literals 
```Typescript
type Position = "top" | "bottom"; type Alignment = "left" | "right"; 
// Generates: "top-left" | "top-right" | "bottom-left" | "bottom-right" 
type ViewportCorner = `${Position}-${Alignment}`
```
- composing two `types` with intersecting prop cause **never**, while extending `interface` - type error
- [x] **Utility types**: `ReturnType`, `Parameters`, `ConstructorParameters`, `Awaited`, `NonNullable`, `Capitalize`/`Uppercase`, extracting array element type `T[number]`
- Awaited - unwraps Promise<>
- [x] **Conditional types + `infer`** (+): `T extends U ? X : Y`, distributive behaviour, write your own `ElementType<T>`
- infer
 In TypeScript, the **`infer` keyword acts like a variable declaration for types**. It is used exclusively within **conditional types**
```Typescript
type MyConditional<T> = T extends SourceStructure<infer InferredVariable> 
  ? InferredVariable 
  : FallbackType;

type GetReturnType<T> = T extends (...args: any[]) => infer R ? R : never;

// Usage:
type Task = () => string;
type Result = GetReturnType<Task>; // Evaluates to: string
```
- distributive 
``` Typescript
type Distributive<T> = T extends U ? X : Y;
// If you pass `A | B` into `Distributive`, works like map()
// (A extends U ? X : Y) | (B extends U ? X : Y)

// Distributive type utility
type ToArray<T> = T extends any ? T[] : never;

// Usage
type Result = ToArray<string | number>;
// Evaluates to: string[] | number[]
// (Instead of: (string | number)[])

type RemoveString<T> = T extends string ? never : T;

type OnlyNumbers = RemoveString<string | number | boolean>;
// 1. string  extends string -> never
// 2. number  extends string -> number
// 3. boolean extends string -> boolean
// Result: never | number | boolean -> simplifies to: number | boolean

```
- [ ] **Decorators**: composition, evaluation vs execution order (evaluated top→down, applied bottom→up), passing metadata (decorator factories); TS 5 standard decorators vs `experimentalDecorators`
- A decorator is a function applied to a class or class member with `@name` syntax. It receives the thing being decorated and a context object, and can return a replacement: a wrapped method, a new class, or a function that transforms a field's initial value. Common uses are logging, validation, caching and registering classes.
- [x] **Narrowing**: custom type predicates `x is T`, assertion functions `asserts x is T` (+), `never` for exhaustiveness, **`satisfies`** vs annotation vs `as`
	- [ ] assertion functions `asserts x is T`
- [x] **tsconfig**: `strict` (and what it turns on), `strictNullChecks`, `noImplicitAny`, `noImplicitOverride`, `noImplicitReturns`, `noImplicitThis`, `strictPropertyInitialization`, `useUnknownInCatchVariables`, `downlevelIteration`, `lib`, `target`, `module`/`moduleResolution` (`bundler`), `paths` aliases, `extends`, `noUncheckedIndexedAccess` (+)
- ! read shallowly, have a general idea what each means, i don't thing it's important
- [x] **Modules**: triple-slash directives, ambient declarations, `declare module`, augmenting third-party types
- declare tells typescript this entity exists in real time

| Entity                                             | Primary Purpose                                                       | Quick Code Example                                                                   | Real-World Scenario                                                                                     |
| :------------------------------------------------- | :-------------------------------------------------------------------- | :----------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------ |
| Ambient Variable (declare const/let)               | Type a runtime global variable that TS doesn't know about.            | declare const __DEV__: boolean;                                                      | Environment variables injected at build-time by Webpack/Vite define.                                    |
| Ambient Function (declare function)                | Type a global function loaded from an external script.                | declare function alert(msg?: any): void;                                             | Legacy scripts, CDN-loaded SDKs (like Google Analytics, Stripe).                                        |
| declare global                                     | Declare something globally from inside a file that has import/export. | declare global {  interface Window {    ethereum: any;  }}                           | Adding properties to the browser window or Node.js global object.                                       |
| declare module "name"                              | Provide types for an untyped npm library or package.                  | declare module 'untyped-pkg' {  export function run(): void;}                        | Silencing errors when installing an old npm package that has no @types/*.                               |
| Wildcard Module (declare module "*.ext")           | Tell TS that asset imports are valid modules and what they return.    | declare module '*.png' {  const path: string;  export default path;}                 | Importing .png, .svg, .css or .gql files when using bundlers.                                           |
| Module Augmentation                                | Add properties to an existing, already-typed library.                 | import 'express';declare module 'express' {  interface Request {    user?: User;  }} | Adding req.user in Express middleware, or custom color keys to Material-UI / Tailwind theme interfaces. |
| Triple-Slash Types (/// <reference types="..." />) | Tell compiler to load ambient types from an installed package.        | /// <reference types="vite/client" />                                                | Placed at the top of an env.d.ts file to get types for import.meta.env.                                 |
- [ ] **Compiler performance**: `isolatedModules` (why Vite/esbuild need it), `incremental` / project references, `skipLibCheck`, avoid huge unions & deep inference, explicit return types on hot paths

| Strategy                | Performance Target       | Why It Works                                                                                    |
| :---------------------- | :----------------------- | :---------------------------------------------------------------------------------------------- |
| "isolatedModules": true | Bundler Dev Speed        | Ensures single-file emitters (Vite/esbuild) don't hit runtime errors (empty files due to types) |
| "incremental": true     | CI & Local tsc           | Caches the dependency graph and AST hashes in .tsbuildinfo.                                     |
| Project References      | Large Repos / Monorepos  | Consumes pre-compiled .d.ts files instead of re-parsing source files.                           |
| "skipLibCheck": true    | Whole Compilation        | Skips validating internal consistency of .d.ts in node_modules.                                 |
| Explicit Return Types   | Type Checker / Hot Paths | Prevents cascading cross-file inference; allows the checker to skip function bodies.            |
| Flattening Unions       | Type Solver Memory       | Eliminates O(N2)O(N^2) Cartesian products in conditional types and template literals.           |

### TS in React (+), since interviewers test this in practice
- [x] Typing props, `children: React.ReactNode`, `React.ComponentProps<'button'>` / `ComponentPropsWithoutRef`
- `PropsWithChildren`, `FC`
- [x] Typing `useState`, `useRef` (DOM ref vs mutable ref), `useReducer` with discriminated-union actions
- [x] Event types (`React.ChangeEvent<HTMLInputElement>`), generic components (`<List<T>>`), `forwardRef` typing / React 19 `ref` as prop
- [x] Redux Toolkit typed hooks (`RootState`, `AppDispatch`, `useAppSelector`), Orval-generated types
- ! Know in general, but won't remember exactly one to one
- useSelector.withTypes<>()

## Key questions

1. `any` vs `unknown` vs `never`: give a use for each.
2. `interface` vs `type`: when do you choose which? What's declaration merging?
3. Why are enums considered problematic? What do you use instead?
4. What's a discriminated union? Show exhaustive checking.
5. Write `MyPartial<T>`, `MyPick<T, K>`, `MyReturnType<T>`, `DeepReadonly<T>`.
	1.  ```Typescript
	   type Partial<T> = { [P in keyof T]?: T[P]; };
	   ```
	2. ```Typescript
	   type Pick<T, P extends keyof T> = { [Key in T]: T[Key]; };
	   ```
	3. ```Typescript
	   type MyReturnType<T extends (...rest: any[]) => any> = T extends (...rest: any[]) => infer P ? P : any;
	   ```
6. `satisfies` vs `: Type` vs `as Type`.
7. What does `strict: true` enable? Which flag catches `arr[10].foo`?
	- wonder if anybody in history asked what flag catches smth in typescript
8. Function overloads vs union params.
9. What does `isolatedModules` do and why does Vite need it?
10. How would you type a generic `Select<T>` component with `getLabel(item: T)`?

## Hands-on

- [ ] [type-challenges](https://github.com/type-challenges/type-challenges): all **easy** + 5–8 **medium** (Pick, Readonly, TupleToObject, First, Awaited, If, Includes, ReturnType, Omit, DeepReadonly)
- [ ] Rewrite an enum to `as const` + union; write an exhaustive `switch` with `assertNever`
- [ ] Write a type guard + assertion function for API data (`unknown` → `User`)
- [ ] Open the lab repo `tsconfig.json`: explain every flag aloud

## Links

- [TS Handbook](https://www.typescriptlang.org/docs/handbook/intro.html): esp. *Narrowing*, *Generics*, *Type Manipulation*
- [TSConfig reference](https://www.typescriptlang.org/tsconfig)
- [Total TypeScript: free tutorials](https://www.totaltypescript.com/tutorials)
- [React TypeScript Cheatsheet](https://react-typescript-cheatsheet.netlify.app/)
