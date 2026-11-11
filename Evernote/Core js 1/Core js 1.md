# Core js 1

Junior-prep notes (cleaned up). Linked from the interview plan: [[src/Interview/README|Interview prep]].
`> [!warning] Fix` boxes = places where the old note was wrong or incomplete.

## ECMAScript Basics

### Advanced Expressions

#### Object.is

Like `===`, but `Object.is(NaN, NaN) === true` and `Object.is(-0, 0) === false`.

#### let, var and const

- `var` всплывает и инициализируется `undefined`. Не имеет блочной области: «вылезает» из `for`, `{}`
- `var` в глобальном коде становится свойством `window`
- повторное объявление `var` без ошибки
- `var` — только функциональная область видимости
- `let` / `const` — блочная область, TDZ, повторное объявление → ошибка; `const` нельзя переприсвоить (но объект внутри можно мутировать)

#### Temporal Dead Zone

Зона, в которой нельзя обратиться к переменной, пока до её объявления не дошёл код → `ReferenceError` (кроме function declaration и `var`).

#### Hoisting

Концепт «поднятия» объявлений переменных (и `let` с `const` тоже) и функций наверх их области: движок сначала регистрирует все объявления. Но к `let` и `const` нельзя обращаться до объявления (TDZ), `var` = `undefined`, function declaration доступна целиком.

#### Polyfills

Замена новой функциональности, не поддерживаемой старыми браузерами, на реализацию, которую эти браузеры поддерживают (например `Array.prototype.flat`). Синтаксис (`?.`, классы) полифилом не закрыть — его транспилирует Babel/TS.

#### Coercion

- `+` — если нет строки → **number**, если есть строка → **string**
- остальные математические операторы → **number**
- операторы сравнения → к примитивам → обе строки — сравнение посимвольно / иначе к числам
- `==` — обе строки → посимвольно / иначе к числам / `null` и `undefined` равны только друг другу и себе
- `===` — по типу и значению, но `NaN !== NaN` и `-0 === 0`
- объекты → `Symbol.toPrimitive`, затем **valueOf** или **toString**. Три **hint**: `number`, `string` и `default`. `default` — при `+` и `==`, тогда сначала пробует `valueOf`

### Functions

#### Arrow functions vs function expressions vs declarations

- function declaration всплывает, можно вызвать до объявления
- function expression не всплывает (переменная есть, функции ещё нет)
- arrow function не имеет своих `this`, `arguments`, `super`, нельзя вызвать через `new`; `this` берётся из внешнего лексического окружения в момент создания

### Functional Scope

По сути scope — это концепция, а в JS её реализует Lexical Environment.

#### Global vs functional scope

- global scope — `window` / `globalThis` (последний в цепочке областей видимости)
- функциональный — внутри функции

#### Variable visibility

`let` / `const` видны в любой блочной или функциональной области, где объявлены.

#### Nested scopes

Снаружи во внутреннюю область доступа нет, изнутри к внешней — есть.

### Function Parameters and Arguments

#### Parameters vs arguments

`function (a, b, c)` — параметры, создаются при вызове функции. Аргументы — значения, которые передаются функции.

#### Passing by value and by reference

Примитивы копируются; у объектов копируется ссылка. Опасность — мутация переданного объекта.

#### Dynamic amount of parameters

`arguments` или `...rest`.

### Closures Advanced

#### Execution context and lexical environment

- контекст выполнения бывает глобальным и функциональным (создаётся для глобального кода и для каждого вызова функции). Контексты кладутся в call stack (видно в дебаггере)
- контекст выполнения проходит стадию создания и стадию выполнения
- на создании создаётся объект, который содержит лексическое окружение, `this` и текущее место выполнения кода
- lexical environment — контейнер с переменными (environment record) и ссылкой на внешний lexical environment (`outer`). Он и реализует концепт области видимости в JS
- каждой функции при создании присваивается текущий `[[Environment]]` → **замыкание** = функция + ссылка на окружение, где она создана

Для заумных:

- As of ES6 it's not true in general that the variable environment contains "the variables and functions". It only contains variables declared with `var`, as `let` and `const` use the lexical environment.
- <https://stackoverflow.com/questions/23948198/variable-environment-vs-lexical-environment>
- <https://tc39.es/ecma262/#sec-block-runtime-semantics-evaluation>
- <https://262.ecma-international.org/5.1/#sec-10.3>

```typescript
ExecutionContext: // for global code and every function call. Lives in the Execution Context Stack (call stack)
(1) LexicalEnvironment: // function scope, for let and const
        d -> nothing // let/const before initialization: "nothing" causes an error (TDZ)
        outer: global // taken from the function's [[Environment]] (where the function was created)
            LexicalEnvironment // block scope (for, while, if, {}). Created when the block runs, outer = (1)
                b -> 2
                outer: (1)
    VariableEnvironment: // var and function declarations (they have no block scope)
        a -> 1, c -> undefined, func -> {}
        outer: global
    This: global // depends on HOW the function is called
    [where the interpreter is now]: 134:14
```

![[Evernote/Core js 1/assets/image.png|image.png]]

Когда интерпретатор доходит до блока, он создаёт для него новое лексическое окружение и делает его текущим для функции / глобального кода. В конце блока выходит из него и снова ставит внешнее.
Все `var` и function declaration попадают в окружение переменных.

#### Scope vs context

`this` никак не относится к области видимости: он не ищется по цепочке областей, а вычисляется в зависимости от того, **как вызвана** функция.

#### Lexical environment traversal

Поиск идёт по областям видимости — от внутренней к внешней по ссылкам `outer`. До глобальной; не нашли → `ReferenceError`.

Связь функции и окружения: функция → свой контекст выполнения → своё лексическое окружение (`outer` = `[[Environment]]` функции).

### Advanced Functions

#### this in functions

Зависит от того, как вызвана функция:

- через `new` (конструктор) — новый создаваемый объект
- обычный вызов — глобальный объект, либо `undefined` в strict mode
- как метод — объект перед точкой
- `call`, `apply`, `bind` — то, что передали первым параметром
- arrow function — `this` из внешнего окружения

#### Losing this (Reference Type)

- `this` теряется, когда метод объекта записываешь в отдельную переменную / передаёшь колбэком и вызываешь без точки: `const f = obj.method; f()`
- `.` — оператор, который возвращает специальный Reference Type (объект + имя свойства), из которого вызов берёт `this`. Если между точкой и вызовом есть что-то ещё (присваивание, `||`), `this` теряется

#### Function vs method

Метод — функция, записанная в свойство объекта. По сути разницы нет: глобальные функции — методы глобального объекта. Разница только в том, какой будет `this`.

#### call, apply, bind

- `func.call(ctx, arg1, arg2)` — вызывает сразу
- `func.apply(ctx, [arg1, arg2])` — вызывает сразу, аргументы массивом
- `func.bind(ctx, arg1...)` — возвращает новую функцию с привязанным `this` (и частично аргументами)

#### Binding a function twice

Со встроенным `bind` невозможно: повторный `bind` не меняет `this`, остаётся первый. Только своя реализация, например такая:

```typescript
Function.prototype.bind = function (context) {
    const targetFunc = this
    return function (...args) {
        const rightContext = this === global ? context : this;
        return targetFunc.apply(rightContext, args);
    };
};
```

### ECMAScript Intermediate

#### Default parameters

`(b, a = 4)` — значение по умолчанию, если аргумент `undefined`.

#### Spread and rest

- rest: `(a, b, ...rest)` — только последним параметром, иначе ошибка синтаксиса
- spread для массивов: `[...arr, ...arr2]`, для вызова: `fn(...args)`

#### arguments vs rest parameters

- `arguments` — array-like (нет методов массива), нет у стрелочных функций
- rest — настоящий массив; не меняется, когда меняют `arguments`

#### Destructuring

`([arg1, arg2])`, `({ arg1, arg2 })`, со значениями по умолчанию и переименованием `({ a: alias = 1 })`.

#### for of loop

`for (const a of str / Object.keys(obj) / array) {}` — по итерируемым объектам (значения). `for…in` — по ключам объекта, включая унаследованные.

### Objects Built-in Methods

#### Object.keys, values, entries

`Object.keys(obj)` / `Object.values(obj)` / `Object.entries(obj)` — массивы собственных перечисляемых ключей / значений / пар. `Object.fromEntries(entries)` — обратно в объект.

#### Static Object methods

`freeze`, `seal`, `fromEntries`, `entries`, `assign`, `create(proto, descriptors)`, `getPrototypeOf`, `defineProperty`.

#### Property flags and descriptors

```typescript
Object.getOwnPropertyDescriptor(user, 'name');
{
  "value": "John",
  "writable": true,
  "enumerable": true,
  "configurable": true
}
Object.defineProperties(user, {
  name: { value: "John", writable: false },
  surname: { value: "Smith", writable: false },
  // ...
});
let clone = Object.defineProperties({}, Object.getOwnPropertyDescriptors(obj));
// Object.create(Object.getPrototypeOf(obj), Object.getOwnPropertyDescriptors(obj))
```

#### Iterables and Symbol.iterator

Объект итерируемый, если у него есть метод `[Symbol.iterator]()`, возвращающий итератор `{ next() { return { value, done } } }`. Так работают `for…of`, spread, деструктуризация.

```typescript
const range = {
  from: 1, to: 3,
  *[Symbol.iterator]() { for (let i = this.from; i <= this.to; i++) yield i; },
};
[...range]; // [1, 2, 3]
```

### ECMAScript Data Types and Expressions

#### Computed properties

`{ [var + var2]: value }`

### Arrays Built-in Methods

`map forEach reduce push pop shift unshift flat sort join split slice splice`
Мутируют: `push pop shift unshift splice sort reverse fill`. Не мутируют: `map filter slice concat flat` + новые `toSorted toReversed toSpliced with`.

#### Copying and modifying arrays

`slice()`, `concat`, `[...arr]`, `map`, `Array.from` — поверхностная копия. Глубокая — `structuredClone`.

#### Flattening arrays

`arr.flat(depth)`, `arr.flat(Infinity)`.

### Arrays Iterating, Sorting, Filtering

#### Custom sorting

```typescript
function compare(a, b) { // для чисел просто return a - b (или b - a)
  if (a < b) return -1; // a слева, b справа
  if (a > b) return 1;  // b слева, a справа
  return 0;             // оставляет как есть
}
```

> [!warning] Fix
> `sort()` без компаратора сортирует как **строки**: `[10, 9, 1].sort()` → `[1, 10, 9]`. И `sort` мутирует массив; без мутации — `toSorted`.

## JavaScript in Browser

### Global Object Window

`window` — глобальный объект браузера: содержит DOM (`document`), BOM (`location`, `history`, `navigator`), таймеры, storage. DOM — дерево объектов-узлов, представляющее HTML-документ, через которое JS читает и меняет страницу.

### Events Basics

#### Types of DOM events

- мыши (`click`, `mousedown`, `mouseover`…), клавиатуры (`keydown`, `keyup`)
- на элементах управления (`submit`, `input`, `focus`, `change`, `blur`)
- документа (`DOMContentLoaded`, `load`, `beforeunload`, `unload`)
- CSS (`transitionend`, `animationend`)

#### Event listeners

`on<event>` (одно свойство — один обработчик) или `addEventListener(type, handler, { once, passive, capture })` — много обработчиков, снимается через `removeEventListener` с той же ссылкой на функцию.

#### Event phases

1. **погружение (capturing)** — от `window` вниз к цели
2. **фаза цели (target)**
3. **всплытие (bubbling)** — от цели вверх

Остановка всплытия: `event.stopPropagation()`.

> [!warning] Fix
> В старой заметке «фаза цели или захвата» — это разные фазы: **захват = погружение** (первая фаза), target — вторая. Обработчики по умолчанию срабатывают на всплытии; на погружении — с `{ capture: true }`.

#### Custom events

`element.dispatchEvent(new CustomEvent('my-event', { detail: data, bubbles: true }))`

### Events Propagation and Preventing

Цикл: погружение → цель → всплытие.

#### Stopping propagation

`event.stopPropagation()` — дальше не всплывает; `event.stopImmediatePropagation()` — ещё и другие обработчики на этом же элементе не сработают.

#### Preventing default behaviour

`event.preventDefault()` — отменяет действие браузера (переход по ссылке, отправку формы). Не останавливает всплытие.

#### Event delegation

Один обработчик на родителе и проверка `event.target` (`target.closest('.item')`) вместо кучи слушателей на дочерних элементах. Плюсы: меньше памяти, работает для динамически добавленных элементов, снять один обработчик вместо 100. Минусы: событие должно всплывать (`focus` не всплывает → `focusin`), `stopPropagation` внутри ломает делегирование.

### Timers

- `setTimeout(fn, ms)` / `setInterval(fn, ms)` возвращают идентификатор → передать в `clearTimeout` / `clearInterval`
- задержка минимальная, не точная: колбэк — макрозадача, ждёт пустой call stack

### Web Storage API and Cookies

| | localStorage | sessionStorage | Cookies |
|---|---|---|---|
| Жизнь | пока не удалят | пока открыта вкладка | до `Expires` / `Max-Age` (или сессия) |
| Размер | ~5–10 MB | ~5 MB | ~4 KB |
| Уходит на сервер | нет | нет | **да**, с каждым запросом |
| Доступ из JS | да | да | да, если нет `HttpOnly` |

> [!warning] Fix
> Старая заметка: «LocalStorage, SessionStorage безопаснее, т.к. не передаются на сервер». Наоборот для токенов: localStorage читает **любой** JS на странице → при XSS токен украдут. Cookie с `HttpOnly; Secure; SameSite` из JS не прочитать — это безопаснее для auth-токена (но нужна защита от CSRF).

### Date and Time

- `new Date()`, `Date.now()`, многие методы имеют UTC-версии (`getUTCHours`)
- `Intl.DateTimeFormat`, `Intl.NumberFormat`, `toLocaleString` — форматирование по локали и часовому поясу

## Principles

### KISS, DRY and YAGNI

Keep It Simple; Don't Repeat Yourself; You Aren't Gonna Need It. Подробно с примерами: [[13-soft-skills-process#🔴 CORE DRY / KISS / YAGNI / SOLID|13 · DRY / KISS / YAGNI / SOLID]].
