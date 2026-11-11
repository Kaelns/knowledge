# Core js 2

Junior-prep notes (cleaned up). Linked from the interview plan: [[src/Interview/README|Interview prep]].
`> [!warning] Fix` boxes = places where the old note was wrong or incomplete.

## JavaScript Advanced

### Modules in JavaScript

#### Module pattern purpose

Улучшает читаемость, поддержку и переиспользование кода: большой код делится на маленькие файлы, которые делятся функциональностью друг с другом. Модуль инкапсулирует данные — снаружи видно только то, что экспортировали. У каждого модуля своя область видимости; ES-модули всегда в strict mode и выполняются один раз (кэшируются).

#### Module types AMD CommonJS ES6 UMD

- **AMD** (Asynchronous Module Definition) — асинхронная загрузка модулей в браузере. Самая известная реализация — [RequireJS](https://requirejs.org/). Сейчас почти не используется

```js
// math.js
define(function() {
    return {
        add: function(a, b) { return a + b; },
        multiply: function(a, b) { return a * b; }
    };
});
// calculator.js
define(['math'], function(math) {
    console.log(math.add(1, 2)); // 3
    console.log(math.multiply(3, 4)); // 12
});
```

Подключить RequireJS из HTML — атрибут `data-main` с точкой входа:

```html
<script data-main="scripts/main" src="scripts/require.js"></script>
```

- **CommonJS** — стандарт Node.js (серверная сторона). Каждый файл — модуль. Синхронный `require`, экспорт через `module.exports`. Так экспортируются и встроенные модули (`fs`, `path`)

```js
module.exports = { myFunction, myVariable }
// ---
const { myFunction, myVariable } = require('./myModule');
```

- **ES6 modules (ESM)** — `import` / `export`, динамический `import()`. Стандарт языка: работает и в браузере (`<script type="module">`), и в Node. Статическая структура → возможен **tree shaking**
- **UMD** — файл, который во время выполнения угадывает, в какой модульной системе он запущен (AMD, CommonJS или просто `<script>` с глобальной переменной), и ведёт себя соответственно. Используется для библиотек

> [!warning] Fix
> Важное отличие ESM от CommonJS: ESM-импорты — **live bindings** (видят изменения экспортированной переменной), CommonJS отдаёт **копию** `module.exports` на момент `require`. ESM статичен и асинхронен, CJS — динамичен и синхронен.

#### Export and import syntax

```js
export default class User {}
import User from "./user.js"

export class User {}
import { User } from "./user.js"

export default class User {}
export class NotUser {}
import { NotUser as FakeUser, default as User } from "./user.js"

import * as say from './say.js';

// re-export
export { login, logout } from './helpers.js';
export * from "module"              // doesn't re-export default
export { default as y } from "module" // re-export default

import "module" // only run the module (side effects)
```

#### Dynamic imports

`import()` — специальный синтаксис (не функция, не наследует `Function.prototype`). Возвращает промис с объектом модуля. Используется для code splitting / ленивой загрузки (`React.lazy`).

```js
let { hi, bye, default: say } = await import('./say.js');

import(modulePath)
  .then(obj => <module object>)
  .catch(err => <loading error, e.g. if no such module>)
```

### Functional Patterns

#### Callbacks and IIFE

- **callback** — функция, которая передаётся аргументом в другую функцию и вызывается там позже
- **IIFE** (Immediately Invoked Function Expression) — функция, которая вызывается сразу при объявлении: `(function () { ... })()`. Создаёт свою область видимости (так раньше делали модули и приватные переменные); на неё больше нет ссылок, и сборщик мусора её удалит

#### Callback hell

- читаемость — сложно следить за порядком выполнения и логикой
- поддерживаемость — сложно поддерживать запутанный, сильно связанный код
- callback hell («pyramid of doom») — последовательные асинхронные шаги, каждый внутри колбэка предыдущего → код растёт вправо, ошибки обрабатываются в каждом уровне. Решение — промисы и `async/await`

![[Evernote/Core js 2/assets/image 5.png|image 5.png]]

#### Currying

Каррирование — сбор аргументов по одному с помощью замыкания, пока их не наберётся столько, сколько нужно исходной функции: `f(a, b, c)` → `f(a)(b)(c)`.

```js
const curry3 = (func) => (a) => (b) => (c) => func(a, b, c)

function curry(func) {
  return function curried(...args) {
    if (args.length >= func.length) return func.apply(this, args);
    return (...next) => curried.apply(this, [...args, ...next]);
  };
}
```

> [!warning] Fix
> В старой версии аргументы копились в общем массиве `argsRes` снаружи → после первого полного вызова каррированная функция ломается (`add(1)(2)(3)` работает, второй `add(4)…` — уже нет). Исправленная версия выше хранит аргументы в замыкании каждого шага.

#### Partial functions

Partial — функция, которая создаётся на основе другой с частично заполненными аргументами.

```js
function multiply(a, b) {
  return a * b;
}
function partial(func, ...fixedArgs) {
  return (...restArgs) => func(...fixedArgs, ...restArgs);
}
const double = partial(multiply, 2); // то же самое: multiply.bind(null, 2)
double(5);  // 10
double(20); // 40
```

> [!warning] Fix
> Старый `partial` сразу вызывал `func(...args)` и возвращал число, а не функцию — `double(5)` падал. Partial должен **возвращать функцию**.

### Object-Oriented Programming

#### new keyword

- специальный синтаксис, который создаёт объект с помощью класса или функции-конструктора
- шаги: создаётся пустой объект → его `[[Prototype]]` = `Constructor.prototype` → конструктор выполняется с `this` = этот объект → возвращается `this` (если конструктор не вернул другой **объект**)
- у стрелочной функции нет `new`

#### Constructor functions

- методы нужно записывать в `prototype`, чтобы они не создавались заново для каждого экземпляра
- `new.target` — проверка, вызвана ли функция через `new`

```js
function User(name) {
  // this = {};  (неявно)
  this.name = name;
  this.isAdmin = false;
  // return this;  (неявно)
}

let user = new User("Jack");
```

#### Public, private, protected, static

- **public** — доступ снаружи и внутри объекта
- **private** — доступ только внутри класса (в JS: `#field`, с ES2022)
- **protected** — в классе и наследниках (в JS только соглашение `_field`; настоящий `protected` есть в TypeScript)
- **static** — поля и методы класса, а не экземпляра: `ClassName.method()`
- **readonly** — только в TypeScript (в JS аналог — `Object.defineProperty` с `writable: false` или геттер без сеттера)

### ECMAScript Classes

#### Class syntax

Класс задаёт шаблон объекта, методы записываются в прототип.

```js
class CoffeeMachine {
  static #PRIVATE_STATIC_FIELD;
  #privateField; // since ES2022
  _waterAmount = 0; // "protected" by convention

  constructor(power) {
    this.#privateField = 42;
    this.power = power;
  }

  set waterAmount(value) {
    if (value < 0) value = 0;
    this._waterAmount = value;
  }
  get waterAmount() {
    return this._waterAmount;
  }

  #privateMethod() {
    return 'hello world'
  }

  static basePublicStaticMethod() {
    this.#PRIVATE_STATIC_FIELD = 42;
    return this.#PRIVATE_STATIC_FIELD;
  }
}
```

#### Classes vs constructor functions

Классы — в основном синтаксический сахар над функциями-конструкторами и прототипами. Отличия:

- класс нельзя вызвать без `new` (ошибка)
- код внутри класса всегда в strict mode
- методы класса неперечисляемые (`enumerable: false`)
- классы не всплывают как функции (TDZ, как у `let`)
- есть то, чего нет у функций: приватные поля `#`, `static`-блоки, `extends` + `super` в удобном виде

> [!warning] Fix
> В старой заметке: «главное различие в том, что классы могут быть наследуемы». Функции-конструкторы тоже наследуются (через `Object.create` / `setPrototypeOf`), просто это неудобно. Отличия — список выше.

#### super

- `super(...)` в конструкторе наследника — обязательный вызов родительского конструктора **до** использования `this`: дочерний класс ждёт, пока родитель создаст `this`

```js
// added automatically if the child has no constructor
constructor(...args) {
  super(...args);
}
```

- `super.method()` — вызов родительского метода из дочернего
- работает через `[[HomeObject]]`: когда функция объявлена как метод класса или объекта, её `[[HomeObject]]` = этот объект; `super` берёт его прототип. `[[HomeObject]]` привязан навсегда, поэтому скопированный в другой объект метод продолжит вызывать «старый» `super`. В обычных `function`-свойствах (`method: function () {}`) `super` не работает

### Prototypal Inheritance

#### proto vs prototype

- `[[Prototype]]` (доступ через `__proto__` или `Object.getPrototypeOf`) — скрытая ссылка **любого объекта** на его прототип. При чтении свойства, которого нет в объекте, JS ищет его по цепочке прототипов до `null`
- `prototype` — обычное свойство **функции-конструктора / класса**: объект, который станет `[[Prototype]]` для экземпляров, созданных через `new`
- `user.__proto__ === User.prototype`, `User.prototype.constructor === User`
- запись свойства всегда идёт в сам объект (не в прототип), кроме сеттеров

#### Object.create and prototype methods

- `Object.getPrototypeOf(rabbit)`
- `Object.setPrototypeOf(rabbit, {})` (медленно, лучше задать прототип при создании)
- `Object.create(prototype, descriptors?)` — второй аргумент — **дескрипторы** свойств; по умолчанию они `writable`, `enumerable`, `configurable` = `false`
- `Object.create(null)` — объект без прототипа (чистый словарь)

### ECMAScript Advanced Data Types and Expressions

#### Map

[Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map) — коллекция ключ/значение, как `Object`, но ключи **любого типа** (объекты тоже), сохраняет порядок вставки, есть `size`, итерируется напрямую.

- `new Map()` — создаёт коллекцию
- `map.set(key, value)`, `map.get(key)` (→ `undefined`, если нет), `map.has(key)`, `map.delete(key)`, `map.clear()`, `map.size`
- `map.keys()`, `map.values()`, `map.entries()` — итерируемые объекты (`entries` — по умолчанию в `for…of`)
- `let map = new Map(Object.entries(obj));`
- `let obj = Object.fromEntries(map);`
- `Array.from(map.keys());`

#### Set

`Set` — коллекция **уникальных** значений (без ключей). `add`, `has`, `delete`, `size`. Быстрое удаление дублей: `[...new Set(arr)]`.

```js
set.forEach((value, valueAgain, set) => {
  console.log(value);
});
```

#### WeakMap and WeakSet

- ключи `WeakMap` — только объекты. Если на объект-ключ больше нет ссылок, запись удалится сборщиком мусора
  - `weakMap.get(key)`, `weakMap.set(key, value)`, `weakMap.delete(key)`, `weakMap.has(key)`
- `WeakSet` — как `Set`, но только объекты; объект в нём, пока доступен где-то ещё
- оба не итерируются, нет `size`, `keys()`, `clear()`
- применение: кэш / метаданные для объектов без утечек памяти, «приватные» данные для объектов

### JavaScript Errors

#### try catch finally

```js
try {
  // код… синтаксические ошибки не ловит (код не запустится вообще),
  // работает синхронно: ошибку в setTimeout внутри try не поймает
} catch (err) {
  // обработка ошибки
} finally {
  // выполняется всегда при любом выходе из try/catch, в том числе при return.
  // если в finally есть свой return, он перекроет return из try
}

// глобально
window.onerror
window.addEventListener('error', ...)
window.addEventListener('unhandledrejection', ...)

// промисы
promise.catch(err => console.error(err))
// async/await → обычный try/catch вокруг await
```

#### Custom errors

```js
class Error {
  constructor(message) {
    this.message = message;
    this.name = "Error"; // different names for different built-in error classes
    this.stack = <call stack>; // non-standard, but widely supported
  }
}
// ---
class ValidationError extends Error {
  constructor(message, options) {
    super(message, options); // options = { cause: originalError }
    this.name = "ValidationError";
  }
}
// проверка: err instanceof ValidationError
```

### ECMAScript Advanced

#### Promises, async await and the event loop

- **Event loop**: выполнить синхронный код (call stack) → выполнить **все** микрозадачи (`promise.then`, `queueMicrotask`, `MutationObserver`) → отрисовка (если нужно; перед ней `requestAnimationFrame`) → взять **одну** макрозадачу (`setTimeout`, события, I/O) → снова микрозадачи…
- `await x` = «остаток функции — в `.then`» → продолжение функции всегда микрозадача
- `queueMicrotask(fn)` — поставить функцию в очередь микрозадач
- `MutationObserver` — уведомления об изменении DOM-элементов (колбэк — микрозадача)

```js
console.log(1);
setTimeout(() => console.log(2));
Promise.resolve().then(() => console.log(3));
console.log(4);
// 1 4 3 2
```

#### Garbage collection

Нет ссылок на объект → он недостижим → удаляется (алгоритм mark-and-sweep от корней: глобальные переменные, текущий стек). Важны только **входящие** ссылки, исходящие значения не имеют.
Как если обрубить ветку дерева: у дерева больше нет доступа к листочкам на этой ветке, даже если они связаны между собой.

## JavaScript in Browser

### Global Object Window (location, history, navigator)

#### History API

`history` — объект `window`, дающий доступ к истории сессии браузера. На нём построены клиентские роутеры (React Router).

```js
history.length
history.back();
history.forward();
history.go(-1);
history.go(1);
history.replaceState({ page: 3 }, "", "?page=3"); // history.state = { page: 3 }; меняет URL и state без перезагрузки
history.pushState(state, "", "?page=3");          // добавляет запись в историю, меняет URL без перезагрузки

window.addEventListener("popstate", (event) => { // кнопки назад/вперёд (на pushState НЕ срабатывает)
  console.log(`location: ${document.location}, state: ${JSON.stringify(event.state)}`);
});
```

#### Location

`location` — текущий URL и навигация.

```js
location.protocol // https:
location.hostname // developer.mozilla.org (без порта)
location.host     // developer.mozilla.org:4097
location.pathname // /en-US/docs/Web/API/Location/pathname
location.search   // всё после ? (удобнее: new URLSearchParams(location.search))
location.hash     // после #
location.assign("https://www.mozilla.org"); // переход (или location.href = url)
location.replace(url) // как assign, но без записи текущей страницы в историю
location.reload();
```

#### Navigator

`navigator` — состояние и возможности user agent.

```js
navigator.userAgent // строка с информацией о браузере (ненадёжна: лучше проверять возможности — feature detection)
navigator.platform  // платформа (устаревает)
navigator.language  // 'ru-RU'
navigator.onLine    // true / false
```

### Page Lifecycle

#### Critical rendering path

- HTML парсится и разбивается на токены
- токены преобразуются в узлы (nodes)
- узлы собираются в **DOM**-дерево
- параллельно парсится CSS и строится **CSSOM** (CSS Object Model)
- CSSOM + DOM → **Render Tree**: в нём нет невидимых элементов (`meta`, `script`, `link`, `display: none`), но есть то, чего нет в DOM (псевдоэлементы `::before`, `::after`)
- дальше: **Layout (Reflow)**, **Paint (Repaint)** — в основном потоке, и **Composite** — в отдельном потоке композитора

![[Evernote/Core js 2/assets/image 1.png|image 1.png]]

CSS блокирует рендеринг; обычный `<script>` блокирует парсинг HTML (поэтому `defer` / `async`).

#### Layout, Paint and Composite

- **Layout** (первый раз) / **Reflow** (повторно) — расчёт размеров и позиций элементов. Идёт по Render Tree от корня. Для `display: none` вычислений нет, для псевдоэлементов и `visibility: hidden` — есть. Может проходить по дереву несколько раз: одни элементы влияют на другие
- **Paint / Repaint** — заполнение пикселями (цвет, тени, текст). Изменение размеров → reflow **и** repaint; только цвет → только repaint
- **Composite** — ничего не пересчитывает и не перерисовывает: двигает / масштабирует уже готовые слои на GPU. `will-change: transform` выносит блок в отдельный слой

Так как **Composite** — в отдельном потоке, тяжёлый JS на него не влияет: даже если страница зависнет в бесконечном цикле, анимация на `transform` / `opacity` продолжит работать.
Reflow и Repaint для элементов одного слоя не затрагивают другие слои.
Но слой на каждый элемент делать нельзя — каждый слой занимает память.
Layout thrashing — чередование чтения (`offsetHeight`) и записи стилей в цикле → принудительный reflow на каждой итерации.

![[Evernote/Core js 2/assets/image.png]]

### Network requests

#### Fetch

```js
let controller = new AbortController();
const options = {
  method: 'POST',
  headers: { 'Content-Type': 'application/json;charset=utf-8' },
  body: JSON.stringify(user),
  signal: controller.signal,
};
let response = await fetch(url, options);
if (!response.ok) throw new Error(`HTTP ${response.status}`);
let data = await response.json();

controller.abort(); // отмена → fetch реджектится с AbortError
```

- `response.ok` — статус 200–299
- `response.status`, `response.headers.get('Content-Type')`

> [!warning] Fix
> `fetch` **не** реджектится на 404 / 500 — только на сетевую ошибку или abort. Проверять `response.ok` нужно вручную (как выше).

#### XMLHttpRequest

```js
let xhr = new XMLHttpRequest();
xhr.open('GET', '/article/xmlhttprequest/example/load');
xhr.timeout = 10000; // прерывается, если не успел
xhr.responseType = 'json';
xhr.send([formData]); // body для POST

xhr.onload = function() {
  if (xhr.status != 200) {
    console.log(`Ошибка ${xhr.status}: ${xhr.statusText}`); // 404: Not Found
  } else {
    console.log(`Готово, получили ${xhr.response.length} байт`);
  }
};
xhr.onprogress = function(event) {
  if (event.lengthComputable) console.log(`Получено ${event.loaded} из ${event.total} байт`);
  else console.log(`Получено ${event.loaded} байт`); // нет Content-Length
};
xhr.onerror = function() { console.log("Запрос не удался"); };

xhr.abort();
xhr.setRequestHeader('Content-Type', 'application/json');
```

```js
UNSENT = 0;           // исходное состояние
OPENED = 1;           // вызван open
HEADERS_RECEIVED = 2; // получены заголовки ответа
LOADING = 3;          // ответ в процессе передачи
DONE = 4;             // запрос завершён

xhr.onreadystatechange = function() {
  if (xhr.readyState == 3) { /* загрузка */ }
  if (xhr.readyState == 4) { /* запрос завершён */ }
};
```

Плюс XHR перед fetch — события прогресса **загрузки на сервер** (`xhr.upload.onprogress`).

### Timers and requestAnimationFrame

`requestAnimationFrame(cb)` — просит браузер вызвать колбэк перед следующей отрисовкой (~60 раз в секунду, синхронно с частотой экрана). Используется для анимаций; в фоновой вкладке ставится на паузу. `setTimeout` не синхронизирован с отрисовкой → рывки и лишняя работа.

## TypeScript

### Basic types

| Примитивные | Остальные |
| --- | --- |
| `boolean` | `array` (`T[]`, `Array<T>`) |
| `number` / `bigint` | `tuple` (`[string, number]`) |
| `string` | `enum` — перечисление |
| `symbol` | `object`, `interface`, `class` |
| `null`, `undefined` | `void` — функция ничего не возвращает |
| | `never` — функция никогда не завершится или выбросит ошибку |
| | `unknown` — безопасный «любой»: перед использованием нужно сузить тип |
| | `any` — отключает проверку типов |

### Enums

Удобный объект TypeScript с именованными константами, например для описания состояний. Можно использовать как тип: переменная принимает только значения из enum.
Числовые enum получают обратное отображение `value → key`:

```ts
{
  "0": "ERROR",
  "1": "WARN",
  "2": "INFO",
  "ERROR": 0,
  "WARN": 1,
  "INFO": 2,
  "DEBUG": "sdfc" // строковые члены обратного отображения не получают
}
```

### Type vs interface

- интерфейсы можно расширять, повторно объявив их (**declaration merging**), и через `extends`
- `type` — для всего остального: union, примитивы, кортежи, mapped / conditional types
- обычно `interface` — для формы объектов и публичных контрактов (API, props), `type` — для остального

```ts
interface ICar extends IMovable, IHasWheels {}
```

> [!warning] Fix
> В старой заметке: «интерфейсы нельзя многоразово наследовать». Можно: `interface A extends B, C {}`. Нельзя другое — **класс** не может `extends` несколько классов (но может `implements` несколько интерфейсов).

### Union and intersection

- **union** `A | B` — значение либо этого типа, либо того (сужается проверками)
- **intersection** `A & B` — значение одновременно обоих типов (объединение свойств)

### Optional and readonly properties

```ts
interface IUser {
    readonly id: number;
    name?: string;
    authenticate(): void;
    // (name: string, surname: string): string; // call signature: интерфейс описывает функцию
}
```

### Function types

`(num: number) => void`, `type Fn = (a: string) => number`, обобщённая `<T>(x: T) => T`. [Utility types](https://www.typescriptlang.org/docs/handbook/utility-types.html): `Partial`, `Required`, `Pick`, `Omit`, `Record`, `ReturnType`, `Parameters`.

### Type guards

- `typeof x === 'string'`
- `x instanceof Date` — есть ли прототип конструктора в цепочке прототипов объекта
- `'meow' in pet` — есть ли свойство (в т.ч. в цепочке прототипов)
- `is` — пользовательский type predicate: если функция вернула `true`, тип сужается

```ts
function isCat(pet: Dog | Cat): pet is Cat {
    return (pet as Cat).meow !== undefined;
}

let pet: Dog | Cat;
if (isCat(pet)) { // в этом блоке TS знает: pet is Cat
    pet.meow();
} else {
    pet.bark();
}
```

## Design Patterns

### Pattern categories

Паттерн проектирования — типовое решение частой архитектурной проблемы.

| Creational — создание объектов | Structural — сочетание сущностей | Behavioral — распределение ответственности и общение |
| --- | --- | --- |
| [Factory Method](https://refactoring.guru/design-patterns/factory-method) — создание похожих объектов с разными отличиями (транспорт: лодка, самолёт, грузовик) | [Adapter](https://refactoring.guru/design-patterns/adapter) — прослойка между двумя несовместимыми интерфейсами (например, DTO с API → модель UI) | [Chain of Responsibility](https://refactoring.guru/design-patterns/chain-of-responsibility) — запрос передаётся по цепочке, пока кто-то его не обработает (middleware) |
| [Abstract Factory](https://refactoring.guru/design-patterns/abstract-factory) — интерфейс для создания **семейств** связанных объектов, не указывая их классы. Фабрика фабрик | [Bridge](https://refactoring.guru/design-patterns/bridge) — разделяет абстракцию и реализацию на две иерархии, связанные композицией (например, «фигура» и «способ отрисовки») | [Command](https://refactoring.guru/design-patterns/command) — действие как объект (можно хранить, ставить в очередь, отменять). Пример: Redux action |
| [Builder](https://refactoring.guru/design-patterns/builder) — пошагово выбираем, что добавить, затем методом строим объект | [Composite](https://refactoring.guru/design-patterns/composite) — дерево объектов, с которым работают как с одним (категории товаров из плоского массива parent–child; дерево компонентов React) | [Iterator](https://refactoring.guru/design-patterns/iterator) — способ обхода коллекции вынесен в отдельный объект |
| [Prototype](https://refactoring.guru/design-patterns/prototype) — создание объектов копированием (клонированием) готового образца | [Decorator](https://refactoring.guru/design-patterns/decorator) — оборачивает объект/функцию, добавляя поведение (debounce-обёртка, HOC) | [Mediator](https://refactoring.guru/design-patterns/mediator) — объекты общаются только через посредника (светофор на перекрёстке, event bus) |
| [Singleton](https://refactoring.guru/design-patterns/singleton) — один экземпляр класса на приложение; повторное создание возвращает первый (store, API client) | [Facade](https://refactoring.guru/design-patterns/facade) — простой интерфейс поверх сложной логики модулей (API service layer) | [Memento](https://refactoring.guru/design-patterns/memento) — сохраняет и восстанавливает **состояние** объекта, не раскрывая его внутренности (undo / redo) |
| | [Flyweight](https://refactoring.guru/design-patterns/flyweight) — общие данные выносятся и переиспользуются многими объектами → экономия памяти | [Observer](https://refactoring.guru/design-patterns/observer) — подписка на события (`addEventListener`, Redux `subscribe`) |
| | [Proxy](https://refactoring.guru/design-patterns/proxy) — объект-заместитель, контролирует доступ к другому (JS `Proxy`, Immer, кэш, ленивая загрузка) | [Strategy](https://refactoring.guru/design-patterns/strategy) — взаимозаменяемые алгоритмы за одним интерфейсом (человек шёл, потом пересел на велосипед: перемещается, но иначе) |
| | | [State](https://refactoring.guru/design-patterns/state) — поведение меняется вместе с состоянием (state machine) |
| | | [Template Method](https://refactoring.guru/design-patterns/template-method) — скелет алгоритма в базовом классе, шаги переопределяют наследники |
| | | [Visitor](https://refactoring.guru/design-patterns/visitor) — новая операция над объектами без изменения их классов (обход AST в Babel / ESLint) |

> [!warning] Fix
> Исправлено в таблице: **Prototype** — это клонирование объектов, а не «методы в прототипе» (это прототипное наследование JS). **Memento** — сохранение состояния (undo), а не кэширование / мемоизация. **Bridge** — разделение абстракции и реализации.

### SOLID

#### Single Responsibility Principle

Одна сущность — одна причина для изменения, иначе получается god object. React: логика данных — в custom hook, отображение — в компоненте.

#### Open Closed Principle

Открыт для расширения, закрыт для изменения: код написан и протестирован, новое поведение добавляем поверх, не меняя старое → не нужно заново проверять, что ничего не сломалось. React: `variant`-пропы, композиция через `children` вместо правки компонента под каждый случай.

#### Liskov Substitution Principle

Наследник должен дополнять, а не ломать поведение родителя: везде, где ждут родителя, можно подставить наследника. React: обёртка `<Button>` принимает все пропы нативной кнопки и ведёт себя как кнопка.

#### Interface Segregation Principle

Сущности не должны зависеть от методов, которые они не используют → маленькие интерфейсы. Более предсказуемо и понятно. React: компонент получает только нужные пропы, а не весь объект `user`.

#### Dependency Inversion Principle

Классы должны зависеть от абстракций, а не от конкретных деталей, чтобы изменение в одной части не ломало другую. Между ними — абстракция («пульт управления»).

- A. Модули верхних уровней не должны зависеть от модулей нижних уровней. Оба должны зависеть от абстракций.
- B. Абстракции не должны зависеть от деталей. Детали должны зависеть от абстракций.

React: компонент использует `api`-сервис / React Query hook, а не `fetch` напрямую → легко замокать в тестах.

## Software Development Methodologies

### User stories

Пользовательская история — наименьшая единица работы в Agile: «As a `<role>`, I want `<goal>`, so that `<benefit>`».

![[Evernote/Core js 2/assets/image 4.png|image 4.png]]

### Waterfall

Долгая последовательная методология от начала до конца: этапы идут друг за другом, один передаёт эстафету следующему, параллельно почти ничего не делается.

![[Evernote/Core js 2/assets/image 2.png|image 2.png]]

1. _Анализ требований._ Определяются требования к системе.
2. _Проектирование._ Техническая характеристика системы, детализация.
3. _Реализация._ Написание кода.
4. _Тестирование._ Тестовая эксплуатация продукта.
5. _Интеграция._ Установка и официальная приёмка продукта.
6. _Поддержка._ Техническая помощь после запуска в коммерческую эксплуатацию.

### Agile

Гибкая философия разработки: выпустить версию и потом её дорабатывать; главное — люди, общение, быстрая реакция на запросы пользователей. Agile Manifesto:

1. Люди и взаимодействие важнее процессов и инструментов.
2. Работающий продукт важнее исчерпывающей документации.
3. Сотрудничество с заказчиком важнее согласования условий контракта.
4. Готовность к изменениям важнее следования плану.

### Scrum

Гибкий фреймворк управления проектами на основе итераций фиксированной длины — **спринтов**. Роли: Product Owner, [Scrum Master](https://www.atlassian.com/agile/scrum/scrum-master) и Developers.
Backlog — все задачи. На grooming (refinement) задачи оцениваются по сложности в story points. Команда знает свою **velocity** (сколько поинтов делает за спринт) и берёт столько задач. На Sprint Review — демо того, что сделано; на ретроспективе — что было хорошо, что плохо, проблемы, а в следующем спринте — отчёт, как их исправили.

![[Evernote/Core js 2/assets/image 3.png|image 3.png]]

Полный ответ на английском (роли, события, артефакты): [[13-soft-skills-process#🔴 CORE What is Scrum? Roles, events, artifacts|13 · Scrum]].

### Kanban

Гибкий метод (не равен Scrum, может применяться поверх него) управления потоком работы. Всё на доске; освободился — берёшь следующую задачу из To Do (pull). **WIP limit** ограничивает число задач в колонке In Progress. Релиз — когда готово, жёсткого дедлайна спринта нет.

### Estimation techniques

1. **Planning Poker** — все одновременно показывают карту (Fibonacci), крайние объясняют, переголосование
2. **T-Shirt Size** — XS…XL вместо story points; быстро и грубо
3. **Dot Voting** — участники распределяют точки между задачами (больше для приоритизации)
4. [**3-Point Estimating**](https://www.projectmanagement.com/wikis/368763/3-points-estimating) — `(O + 4M + P) / 6`

Подробно и с ситуациями: [[13-soft-skills-process#Estimation|13 · Estimation]].

## Testing

По сути тесты — описание того, как должен работать код и соответствовать требованиям; своего рода документация.

### Types of testing

| Функциональное | Нефункциональное |
| --- | --- |
| **Unit** — маленькие независимые части системы (функции, методы, хуки) | нагрузочное (load, stress, spike, soak) |
| **Integration** — модули в связке (компонент + store + API mock) | безопасности (security) |
| **E2E** — пользовательский сценарий в реальном браузере: нажать кнопку, заполнить и отправить форму | доступности (a11y) |
| **Visual / screenshot** — сравнение вёрстки со старыми скриншотами | регрессионное — что старое не сломалось (это цель, может быть любого уровня) |

### Test pyramid

Снизу вверх: много **unit** (~70%, быстрые и дешёвые) → меньше **integration** (~20%) → мало **E2E** (~10%, медленные, дорогие, хрупкие, но дают больше уверенности). Альтернатива — **testing trophy** (Kent C. Dodds): основа — статический анализ (TS, ESLint), больше всего ценности — в integration-тестах.

> [!warning] Fix
> В старой таблице: «Screenshots — 70–80%» и «Unit — 70–80%» одновременно. Скриншотные тесты — не основа пирамиды; основа — unit.

### TDD and BDD

- **TDD** — сначала тест (красный) → минимальная реализация (зелёный) → рефакторинг
- **BDD** — описываем поведение системы на языке бизнеса. **Gherkin**: Given — When — Then

```gherkin
Scenario: Add item to cart
  Given I am on the product page
  When I click on the "Add to Cart" button for product "ABC"
  Then the item "ABC" should be added to my cart
```

## Web Communication Protocols

### HTTP vs HTTPS

HTTPS = HTTP поверх **TLS** (раньше SSL): шифрование, целостность и проверка подлинности сервера по сертификату.
TLS handshake (упрощённо):

1. Client Hello → Server Hello + **сертификат** сервера; браузер проверяет сертификат по цепочке до доверенного CA
2. Асимметричное шифрование (публичный ключ сервера шифрует / подписывает, приватный есть только у сервера) используется, чтобы безопасно договориться о **сессионном ключе**
3. Дальше всё общение шифруется быстрым **симметричным** сессионным ключом

![[Evernote/Core js 2/assets/image 6.png|image 6.png]]

> [!warning] Fix
> «Тройное рукопожатие» (SYN → SYN-ACK → ACK) — это **TCP**, оно идёт до TLS. У TLS свой handshake (выше). В TLS 1.3 ключи обычно согласуются через (EC)DHE, а не шифрованием публичным ключом — но идея «асимметрия для договорённости, симметрия для данных» та же.

### HTTP methods, headers and status codes

URL — Uniform Resource Locator.

```http
GET /path HTTP/1.1          // стартовая строка: метод, путь, версия
Host: www.youtube.com       // обязательный заголовок в HTTP/1.1
Content-Type: application/json
Content-Length: 26
```

CRUD — create read update delete → `POST`, `GET`, `PUT` / `PATCH`, `DELETE`.

- `GET` — получить (безопасный, идемпотентный, кэшируется)
- `POST` — создать / действие (не идемпотентный)
- `PUT` — заменить ресурс целиком (идемпотентный)
- `PATCH` — частично изменить
- `DELETE` — удалить (идемпотентный)

Заголовки — дополнительная информация и инструкции (тип данных, авторизация, кэширование, CORS).
Статусы: `2xx` успех (200, 201, 204) · `3xx` редирект / кэш (301, 302, 304) · `4xx` ошибка клиента (400, 401 — не аутентифицирован, 403 — нет прав, 404, 409, 422, 429) · `5xx` ошибка сервера (500, 502, 503).

### REST

Representational State Transfer — архитектурный стиль, набор правил, как удобно и правильно использовать HTTP.

- клиент — сервер
- без состояния (stateless): каждый запрос несёт всю нужную информацию
- кэширование
- единый интерфейс (ресурсы по URL, методы HTTP, `/users/1`)
- многослойная система (прокси, балансировщики незаметны клиенту)

## Common Web Security

### CORS

Браузер по **Same-Origin Policy** не даёт JS читать ответы с другого origin (схема + домен + порт). CORS — сервер явно разрешает это заголовками.

```http
Origin: https://javascript.info                       // в запросе (ставит браузер)
Access-Control-Allow-Origin: https://javascript.info  // в ответе сервера
```

Для «непростых» запросов (`PUT`, `DELETE`, `Content-Type: application/json`, свои заголовки) браузер сначала шлёт **preflight** `OPTIONS`. Защищает пользователя в браузере, а не сервер. XSS, CSRF, CSP, OWASP: [[06-web-platform#Security (A2 Novice, but always asked), Pass 1|06 · Security]].

### Authentication JWT and OAuth

- **JWT** — `header.payload.signature` в base64url. Подписан, но **не зашифрован**: payload читает любой. Сервер проверяет подпись и `exp`; обычно короткий access token + долгий refresh token
- **OAuth 2.0** — протокол **делегирования доступа** («войти через Google»): приложение получает access token от сервера авторизации, не видя пароль. Для SPA — Authorization Code flow + **PKCE**. **OIDC** — слой поверх OAuth для аутентификации (id_token)

## Coding Task Examples

### Polyfills and Extensions

#### bind polyfill

```js
function bind(func, context, ...boundArgs) {
  return function (...args) {
    return func.apply(context, [...boundArgs, ...args]);
  };
}

Function.prototype.myBind = function (context, ...boundArgs) {
  const targetFunc = this;
  return function (...args) {
    return targetFunc.apply(context, [...boundArgs, ...args]);
  };
};
```

> [!warning] Fix
> В старой версии: `apply(rightContext, ...args, ...additionalArgs)` — `apply` принимает **массив** вторым аргументом, а не спред; и не было `return`, поэтому функция всегда возвращала `undefined`.

#### Object.create polyfill

```js
function create(proto, descriptors) {
  if (typeof proto !== 'object' && typeof proto !== 'function') {
    throw new TypeError('Object prototype may only be an Object or null');
  }
  function F() {}
  F.prototype = proto;
  const obj = proto === null ? Object.setPrototypeOf({}, null) : new F();
  if (descriptors !== undefined) Object.defineProperties(obj, descriptors);
  return obj;
}
```

> [!warning] Fix
> Старая версия: проверка `typeof proto !== 'object' && proto !== null` пропускала `null` неправильно, а второй аргумент копировался как обычные значения (`{ ...props }`), хотя по спецификации это **дескрипторы** свойств.

#### Array.flat polyfill

```js
function flat(array, depth = 1) {
  if (!depth) return array;
  return array.reduce(
    (acc, el) => [...acc, ...(Array.isArray(el) ? flat(el, depth - 1) : [el])],
    []
  );
}

function flat2(arr, n) {
  const result = [];
  function recursiveFlat(array, depth) {
    array.forEach((el) => {
      if (Array.isArray(el) && depth) recursiveFlat(el, depth - 1);
      else result.push(el);
    });
  }
  recursiveFlat(arr, n);
  return result;
}

const a = [2, 34, 65, 6, [234, [45, 2, 4, 65, { a: 'sdfg' }, [4]]]];
console.log(flat(a, 3));
console.log(flat(a));
console.log(flat(a, 2));
```

#### Array.reduce polyfill

Task: implement a polyfill for `Array.reduce`.

```js
Array.prototype.myReduce = function (callback, ...init) {
  let i = 0;
  let acc;
  if (init.length) acc = init[0];
  else {
    if (!this.length) throw new TypeError('Reduce of empty array with no initial value');
    acc = this[0];
    i = 1;
  }
  for (; i < this.length; i++) {
    if (i in this) acc = callback(acc, this[i], i, this);
  }
  return acc;
};
```

#### String repeat

Task: a method that repeats a string N times, like `String.prototype.repeat`.

#### Custom join

Task: build a string from an array of numbers, separated by a given delimiter.

### Functional Programming Challenges

#### Functional expressions

Task: functions for expressions like `five(plus(seven(minus(three()))))`.

```js
function makeNum(num, func) {
  return func === undefined ? num : func(num);
}
const zero  = (func) => makeNum(0, func);
const one   = (func) => makeNum(1, func);
const two   = (func) => makeNum(2, func);
const three = (func) => makeNum(3, func);
const four  = (func) => makeNum(4, func);
const five  = (func) => makeNum(5, func);
const six   = (func) => makeNum(6, func);
const seven = (func) => makeNum(7, func);
const eight = (func) => makeNum(8, func);
const nine  = (func) => makeNum(9, func);

const plus      = (right) => (left) => left + right;
const minus     = (right) => (left) => left - right;
const times     = (right) => (left) => left * right;
const dividedBy = (right) => (left) => left / right;

eight(minus(three())); // 5
six(dividedBy(two())); // 3
two(plus(five()));     // 7
seven(times(five()));  // 35
four(plus(nine()));    // 13
```

#### Curried addition

Task: `add(5)(9)(-4)(1)`, an add that works with any number of calls.

### Timing Functions

#### Periodic output

Task: log the elapsed time in ms at regular intervals.

#### Extended periodic output

Task: log the elapsed time at intervals that grow each time (chained `setTimeout`, not `setInterval`).

### Working with Promises

#### Sequential promise execution

Task: run an array of functions that return promises **one after another**, each waits for the previous one.

```js
async function runSequentially(tasks) {
  const results = [];
  for (const task of tasks) {
    results.push(await task()); // next task starts only after the previous one
  }
  return results;
}
// or: tasks.reduce((p, task) => p.then((acc) => task().then((r) => [...acc, r])), Promise.resolve([]))
```

> [!warning] Fix
> Старое решение `queuPromises` получало уже **запущенные** промисы и ждало их через `forEach(async …)` — это параллельное выполнение, а не последовательное (по сути `Promise.allSettled`). Для последовательности нужны **функции**, которые вызываются по одной после `await`.

#### Promise.all polyfill

Task: implement a function that behaves like `Promise.all`.

#### Race condition handling

Task: resolve or reject with the first promise that settles (`Promise.race`).

#### Promise chaining and error handling

Task: chain promises for data fetching and catch any error.

#### Promisify Node-style callbacks

Task: turn `fn(args, (err, result) => {})` functions into ones that return promises.
