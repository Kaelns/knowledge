# 02 · TypeScript

Matrix: **TypeScript** (Core). A1 = Novice → **A2 = Intermediate**. This is one of the biggest level jumps in the matrix.
Old notes: [Core js 2 → TypeScript](../../Evernote/Core%20js%202/Core%20js%202.md#typescript).

## Checklist

### A1 baseline (quick check, should be easy)
- [ ] Primitives, `void` vs `undefined`, arrays, tuples, objects, `Record`
- [ ] Function typing: optional/default/rest params, return types, generic functions
- [ ] `interface` vs `type` (declaration merging, `extends` vs `&`, unions only via `type`)
- [ ] Classes: `public/private/protected`, `readonly`, `implements` vs `extends`, get/set, parameter properties
- [ ] Union vs intersection, `keyof`, `typeof`, `Partial`, `Pick`, `Omit`, `Exclude`, `Extract`
- [ ] Narrowing with `typeof`/`instanceof`/`in`, `as` casting; decorator kinds; `.d.ts`, namespace vs module

### A2 (Intermediate)
- [ ] **Primitives**: numeric / string / `const enum`, enum pitfalls (reverse mapping, not erasable, prefer `as const` objects / union literals), **`any` vs `unknown`**, literal types, `symbol`, `bigint`
- [ ] **Arrays/objects**: `Record<K,V>` vs `object` vs `{}` vs `Object`, multi-type arrays `(string | number)[]` vs tuples, `readonly` arrays
- [ ] **Functions**: overloads, `this` parameter, `void` vs `never` returns, `Function` vs `() => any` (why `Function` is bad)
- [ ] **Generics**: index signatures, constraints `<T extends …>`, defaults, `const` type parameters `<const T>`, generic `keyof` access `T[K]`
- [ ] **Classes**: `override`, `abstract`, `static`, arrow-function methods (`this` binding vs prototype), generic classes
- [ ] **Type manipulation**: **discriminated unions** + exhaustive `switch` with `never`, mapped types (`{ [K in keyof T]: … }`, `-readonly`, `?`, key remapping `as`), **template literal types**, type composition
- [ ] **Utility types**: `ReturnType`, `Parameters`, `ConstructorParameters`, `Awaited`, `NonNullable`, `Capitalize`/`Uppercase`, extracting array element type `T[number]`
- [ ] **Conditional types + `infer`** (+): `T extends U ? X : Y`, distributive behaviour, write your own `ElementType<T>`
- [ ] **Decorators**: composition, evaluation vs execution order (evaluated top→down, applied bottom→up), passing metadata (decorator factories); TS 5 standard decorators vs `experimentalDecorators`
- [ ] **Narrowing**: custom type predicates `x is T`, assertion functions `asserts x is T` (+), `never` for exhaustiveness, **`satisfies`** vs annotation vs `as`
- [ ] **tsconfig**: `strict` (and what it turns on), `strictNullChecks`, `noImplicitAny`, `noImplicitOverride`, `noImplicitReturns`, `noImplicitThis`, `strictPropertyInitialization`, `useUnknownInCatchVariables`, `downlevelIteration`, `lib`, `target`, `module`/`moduleResolution` (`bundler`), `paths` aliases, `extends`, `noUncheckedIndexedAccess` (+)
- [ ] **Modules**: triple-slash directives, ambient declarations, `declare module`, augmenting third-party types
- [ ] **Compiler performance**: `isolatedModules` (why Vite/esbuild need it), `incremental` / project references, `skipLibCheck`, avoid huge unions & deep inference, explicit return types on hot paths

### TS in React (+), since interviewers test this in practice
- [ ] Typing props, `children: React.ReactNode`, `React.ComponentProps<'button'>` / `ComponentPropsWithoutRef`
- [ ] Typing `useState`, `useRef` (DOM ref vs mutable ref), `useReducer` with discriminated-union actions
- [ ] Event types (`React.ChangeEvent<HTMLInputElement>`), generic components (`<List<T>>`), `forwardRef` typing / React 19 `ref` as prop
- [ ] Redux Toolkit typed hooks (`RootState`, `AppDispatch`, `useAppSelector`), Orval-generated types

## Key questions

1. `any` vs `unknown` vs `never`: give a use for each.
2. `interface` vs `type`: when do you choose which? What's declaration merging?
3. Why are enums considered problematic? What do you use instead?
4. What's a discriminated union? Show exhaustive checking.
5. Write `MyPartial<T>`, `MyPick<T, K>`, `MyReturnType<T>`, `DeepReadonly<T>`.
6. `satisfies` vs `: Type` vs `as Type`.
7. What does `strict: true` enable? Which flag catches `arr[10].foo`?
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
