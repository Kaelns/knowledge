### Modules in JavaScript

- Purpose and concept of a module/module pattern.

Улучшает читабельность, поддержку и переиспользование кода путём разделения большого кода на маленькие файлы, между которыми можно делится данным функционалом или объединять в один большой. Также позволяет инкапсулировать данные в модуле, пока их не экспортируют

- Types of modules (AMD, ES6, CommonJS, UMD).
	- `AMD` - Asynchronous Module Definition - асинхронный вид загружать модули. Среди различных реализаций AMD, самый нормальный - это [RequireJS](https://requirejs.org/). Используется в редких случаях

```
// math.js
define(function() {
    return {
        add: function(a, b) {
            return a + b;
        },
        multiply: function(a, b) {
            return a * b;
        }
    };
});
---
// Файл: calculator.js
define(['math'], function(math) {
    console.log(math.add(1, 2)); // Выводит: 3
    console.log(math.multiply(3, 4)); // Выводит: 12
});
```
Чтобы подключить `RequireJS`  и начать загрузку модулей из HTML-файла, добавляем `data-main` атрибут в тег `<script>`, указывая точку входа приложения:
```
<!-- index.html чтобы подключить RequireJS --> 
<script data-main="scripts/main" src="scripts/require.js"></script>
```

- `CommonJS`  - Популярен в \`Node.js\` , используются на серверной стороне. Каждый файл это модуль, поэтому к ним сразу прикручен `commonJS` .  Часто используется, чтобы экспортировать встроенные модули как `fs` , `path`

```
module.exports = { myFunction, myVariable }
---
const { myFunction, myVariable } = require('./myModule');
```

- `ES6` import, export, динамические import(). Самый популярный, считай стандарт модулей в JS
- `UMD` - это JavaScript-файл, который во время выполнения пытается угадать, в какой модульной системе он используется, а затем действует как такой определённый модуль. Таким образом, вы можете загрузить файл обычным <скриптом>, или вы можете загрузить его из загрузчика модулей AMD, или вы можете загрузить его как модуль Node.js, и он всегда будет делать что-то разумное.

- Syntax and common features (export default, named exports, exports as).

```
export default class User {}
import User from "./"

export class User {}
import {User}

export default class User {} // import {User as default}
export class NotUser {} 
import {NotUser as FakeUser, default as User} from "./"

import * as say from './say.js';

// re-export
export {login, logout} from './helpers.js';
export * from "module" (doesn’t re-export default).
export {default [as y]} from "module" (re-export default).

import "module"
```

- Dynamic imports. - specified as syntax - That means that import doesn’t inherit from `Function.prototype`

```
let {hi, bye, default: say} = await import('./say.js');

import(modulePath)
  .then(obj => <module object>)
  .catch(err => <loading error, e.g. if no such module>)
```

### Functional Patterns

- Callbacks and IIFE.

каллбэк - функция, которая передаётся аргументом в другую функцию, где может в последующем вызваться
iife - immediatly  involver func express - сразу вызываемая функция и считай удаляемая функция, так как на неё не будет в дальнейшем ссылки и сборщик мусора её проглотит

- Limitations of callbacks (e.g., callback hell).
	- читаемость - сложно следить за порядком выполнения и логикой приложения
	- поддерживаемость - сложно поддерживать непонятный непоследовательный тяжело связанный код
	- callback hell - в случае выполнения одних и тех же последовательный действий друг за другом и передачей созданной сразу же функции обработчика код может расти в размерах вправо ( хотя я с таким не сталкивался )

![[Evernote/Core js 2/assets/image 5.png|image 5.png]]

- Function currying and partial functions.
	- карирование - собирание параметров по 1 с помощью замыкания, пока не будет достигнуто нужного кол-ва для первоначальной функции `f(a, b, c), а как f(a)(b)(c).`

```
const carry = (a) => (b) => (c) => func(a,b,c)
---
function curry(func) {
  let argsRes = [];
  return function curried(...args) {
    argsRes.push(...args);
    if (argsRes.length >= func.length) {
      return func(...argsRes);
    } else {
      return curried;
    }
  };
}
```

- partial - функция, которая создаёт на основе другой функцию с частично заполненными параметрами

```typescript
function multiply(a, b) 
  return a * b;
}
function partial(func, ...args) {
  return func(...args);
}
const double = partial(multiply, 2);
double(5); // Result: 10
double(20); // Result: 40
```

### Object-Oriented Programming (OOP)

- \`new\` keyword functionality.
	- специальный синтаксис, который позволяет создавать инстансы класса или функции конструктора, или точнее - объекты с конструктора объектов
	- нету у стрелочной функции
	- вернуть с конструктора можно либо this объект, либо другой объект
- Constructor functions and their properties.
	- методы конструкторы нужно записывать в прототип, чтобы они не создавались для каждого инстанса
	- [new.target](http://new.target) - проверка на вызов new

```
function User(name) {
  // this = {};  (неявно)
  // добавляет свойства к this
  this.name = name;
  this.isAdmin = false;
  // return this;  (неявно)
}

let user = new User("Jack");
```

- Public, private, and static members in OOP.
	- public -  есть доступ снаружи в внутри объекта
	- private - доступ только внутри
	- protected - в классах и наследниках
	- static - поля, к которым можно обращаться только через blueprint, или если точнее - через класс в js. Свойства - с 20 года
	- readonly ?

### ECMAScript Classes

- Class declaration and syntax.

Позволяет задать щаблон объекта, а методы записать в прототип
```
class CoffeeMachine {
  static #PRIVATE_STATIC_FIELD;
  #privateField; // since 2020
  _waterAmount = 0; // the amount of water inside 

  constructor(power) {
    this.#privateField = 42;
    this.power = power;
    alert( `Created a coffee-machine, power: ${power}` );
  }

  set waterAmount(value) {
    if (value < 0) {
      value = 0;
    }
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

- Differences between \`class\` declarations and constructor functions.

Классы - синтаксический сахар для функций конструкторов. Главное различие в том, что классы могут быть наследуемы

- код внутри классов всегда в строгом режиме
- новая фича - приватные поля #
- классы не высплывают

- Usage of \`super()\`.
	- специальный синтаксис
	- обязательный вызов родительского конструктора в дочернем с параметрами, ибо дочерний класс ждёт, пока родитель передаст ему созданный this

```
  // автоматически, если нет конструктора у дочернего
  constructor(...args) { 
    super(...args);
  }
```

- вызов родительских методов в дочерних. Не работает в стрелочных функциях, так как this
	Когда функция объявлена как метод внутри класса или объекта, её свойство `[[HomeObject]]` становится равно этому объекту.
	
	Затем `super` использует его, чтобы получить прототип родителя и его методы.
	
	`[[HomeObject]]` - навсегда, используется только в `super`, поэтому могут быть проблемы
	

### Prototypal Inheritance

- Properties like \`\_\_proto\_\_\` and \`prototype\`.

Ну здесь я всё знаю

- Using \[Object.create\] to define prototypes explicitly.
	- `Object.getPrototypeOf(rabbit)`
	- `Object.setPrototypeOf(rabbit, {})`
	- `Object.create(prototype[, obj])`

### ECMAScript Advanced Data Types & Expressions

- \`Set/Map\` and \`WeakSet/WeakMap\`.
- [Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map) – это коллекция ключ/значение, как и `Object`. Но основное отличие в том, что `Map` позволяет использовать ключи любого типа.
	- `new Map()` – создаёт коллекцию.
	- `map.set(key, value)` – записывает по ключу `key` значение `value`.
	- `map.get(key)` – возвращает значение по ключу или `undefined`, если ключ `key` отсутствует.
	- `map.has(key)` – возвращает `true`, если ключ `key` присутствует в коллекции, иначе `false`.
	- `map.delete(key)` – удаляет элемент (пару «ключ/значение») по ключу `key`.
	- `map.clear()` – очищает коллекцию от всех элементов.
	- `map.size` – возвращает текущее количество элементов.
	- `map.keys()` – возвращает итерируемый объект по ключам (Array`.from` чтобы получить массив),
	- `map.values()` – возвращает итерируемый объект по значениям,
	- `map.entries()` возвращает итерируемый объект по парам вида `[ключ, значение]`, этот вариант используется по умолчанию в `for..of`
		
	- `let map = new Map(Object.entries(obj));`
	- `let obj = Object.fromEntries(map);`
	- Array`.from(`map`.keys());`

`Set` - вид коллекции, где (без ключей) присутствуют уникальные  значения
```typescript
set.forEach((value, valueAgain, set) => {
  alert(value);
});
```

- ключи в `WeakMap` должны быть объектами, а не примитивными значениями. Если удалить ссылку на объект, то и свойство в weekmap исчезнет
	- `weakMap.get(key)`
	- weakMap.set(key, value)
	- weakMap.delete(key)
	- weakMap.has(key)
- Коллекция `WeakSet` ведёт себя похоже:
	- Она аналогична `Set`, но мы можем добавлять в `WeakSet` только объекты (не примитивные значения).
	- Объект присутствует в множестве только до тех пор, пока доступен где-то ещё.
	- Как и `Set`, она поддерживает `add`, `has` и `delete`, но не `size`, `keys()` и не является перебираемой.

### JavaScript Errors

- Error handling techniques including \`try..catch\`.

```
try {
  // код...// c синтаксисом ошибки не ловит, 
  // работает синхронно 
} catch (err) {
  // обработка ошибки
} finally {
  // выполняем всегда при любом выходе из try..catch, в том числе и return.
  // поэтому до return в try catch выполнится return здесь
}

---

window.onerror
window.addEventListener('error' ...

---
.catch(err => alert(err))
```

- Understanding and implementing custom errors.

```
class Error {
  constructor(message) {
    this.message = message;
    this.name = "Error"; // (разные имена для разных встроенных классов ошибок)
    this.stack = <стек вызовов>; // нестандартное свойство, но обычно поддерживается
  }
---
class ValidationError extends Error {
  constructor(message) {
    super(message); // (1)
    this.name = "ValidationError"; // (2)
  }
}
```

### ECMAScript Advanced

- Promises, async/await, and the event loop.
	- `queueMicrotask`
	- `MutationObserver` предоставляет возможность получать уведомления об изменении определённых [DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model)\-элементов

- Garbage collection basics.

нету ссылк на объект - он недостижим - удалён. Исходящие ссылки не имеют значения. Только входящие ссылки
Это как если дереву обрубить ветку, то у дерева не будет доступа к листочкам на той ветке

## JavaScript in Browser:

### Global object window

- Understanding browser's location and history API.
	- History - объект window, дающий доступ к браузерному сессионной истории и работой с ней

```
history.length
history.back();
history.forward();
history.go(-1);
history.go(1);
history.replaceState({ page: 3 }, "title of page", "?page=3"); // history.state - { page: 3 } - replaces url and state but not reload
history.pushState(state, title, "string to add in utl or use http:/...") // add to hisory page, change url, but not reload. "?page=3" adds to example.html?page=3

window.addEventListener("popstate", (event) => {
  alert(
    `location: ${document.location}, state: ${JSON.stringify(event.state)}`,
  );
});
```

- location - возвращает текущую url

```javascript
location.protocol // https:
hostname // developer.mozilla.org without port :2034
host // developer.mozilla.org:4097
pathname // /en-US/docs/Web/API/Location/pathname - after - developer.mozilla.org
search // all after ?
hash // after #
// переход на новую страницу
location.assign("https://www.mozilla.org"); // or location = url
location.replace(url) // like assign, but doesn't safe current page ro rhe history
location.reload();
```

- User agent parsing and platform/browser detection.
	- navigator - представляет состояние и особенности пользовательского агента

```javascript
navigator.userAgent // информация о текущем браузере
navigator.platform // информация о платформе
navigator.language // 'ru-RU'
navigator.onLine // true / false
```

### Page Lifecycle

- Parsing, reflow, repaint, and the critical rendering path.
	- Html парсится и разбивается на токены;
	- Токены преобразуются в Ноды;
	- Ноды собираются в DOM (Document Object Model) дерево;
	- Параллельно с этим процессом происходит парсинг CSS правил и строится CSSOM (CSS Object Mode );
	- CSSOM и DOM объединяются в Render Tree, похожее на DOM дерево, в котором скрываются невидимые элементы (`meta`, `script`, `link и display: none` ) и добавляются элементы которых нет в DOM (псевдоэлементы :before, :after);
	- Далее начинаются процессы: Reflow, Repaint (выполняются в основном потоке) и Composition (происходит в отдельном потоке композитора)![[Evernote/Core js 2/assets/image 1.png|image 1.png]]
	- этапы
	- Layout (для первой операции) и Repaint (для последующей)

расчёт размеров и позиций элементов. Идёт по рендер дереву с корня, определяет координаты каждого элемента. Для элементов с дисплей none вычислений не будет, но будет для псевдоэлементов и скрытых. Проход по дереву может быть многоразовым, ибо могут быть элементы, которые влияют на предыдущие

- Paint / repaint - заполнение пикселями бокса. При изменении размеров бокса также производится перерисовка
- Composite - не перерисовывает и не пересчитывает всё, а всего лишь в отдельном слое растягивает картинку с gpu. `Will-change: transform` позволяет отдельно вынести в слой отдельный блок и перерисовать только его

Так как этап **Composite** происходит в отдельном потоке композитора, а не в основном, то вычисления в JS никак не влияют на него. Даже если вся страница подвиснет из-за бесконечного цикла в JS, анимация которая реализована с помощью transform, opacity продолжит свое выполнение
Благодаря тому что элементы расположены на отдельных слоях, **Reflow** и **Repaint** для элементов одного слоя не затрагивают элементы на остальных слоях
Нельзя злоупотреблять этапом composite и создавать слой на каждый элемент, так как для хранения каждого слоя используется память
![[Evernote/Core js 2/assets/image.png]]

### Network requests

- Using \`Fetch\` and understanding \`XMLHTTPRequest\`.

```
let response = await fetch(url, [options])
let code = await response.json();

let controller = new AbortController();
const options = 
{
  method: 'POST', // HTTP-метод
  headers: {
    'Content-Type': 'application/json;charset=utf-8'
  },
  body: JSON.stringify(user),
  signal: controller.signal
 
}
controller.abort();
```

- response.ok ( 200-299 )
- responce.status

```
// 1. Создаём новый XMLHttpRequest-объект
let xhr = new XMLHttpRequest();

// 2. Настраиваем его: GET-запрос по URL /article/.../load
xhr.open('GET', '/article/xmlhttprequest/example/load');

xhr.timeout = 10000; 
// Если запрос не успевает выполниться в установленное время, то он прерывается
xhr.responseType = 'json';

// 3. Отсылаем запрос
xhr.send([formData]); // formData если юзается пост

// 4. Этот код сработает после того, как мы получим ответ сервера
xhr.onload = function() {
  if (xhr.status != 200) { // анализируем HTTP-статус ответа, если статус не 200, то произошла ошибка
    alert(`Ошибка ${xhr.status}: ${xhr.statusText}`); // Например, 404: Not Found
  } else { // если всё прошло гладко, выводим результат
    alert(`Готово, получили ${xhr.response.length} байт`); // response -- это ответ сервера
  }
};

xhr.onprogress = function(event) {
  if (event.lengthComputable) {
    alert(`Получено ${event.loaded} из ${event.total} байт`);
  } else {
    alert(`Получено ${event.loaded} байт`); // если в ответе нет заголовка Content-Length
  }
};

xhr.onerror = function() {
  alert("Запрос не удался");
};
```
`xhr.abort();`
`xhr.setRequestHeader('Content-Type', 'application/json');`
```
UNSENT = 0; // исходное состояние
OPENED = 1; // вызван метод open
HEADERS_RECEIVED = 2; // получены заголовки ответа
LOADING = 3; // ответ в процессе передачи (данные частично получены)
DONE = 4; // запрос завершён

xhr.onreadystatechange = function() {
  if (xhr.readyState == 3) {
    // загрузка
  }
  if (xhr.readyState == 4) {
    // запрос завершён
  }
};
```

### Timers

- Differences between \`setTimeout\` and \`requestAnimationFrame\`.

requestAnimationFrame - requests the browser to call a user-supplied callback function before the next repaint. Используется для анимации

## TypeScript:

- Writing efficient TypeScript using basic types, enums, interfaces, and generics:
	-    basic types

				any

|     |     |
| --- | --- |
| Примитивные |     |
| boolean | array |
| number / bigint | enum - перечисление |
| string | class |
| symbol | interface |
| void | object |
| never - для функций, которые никогда не завершаться либо выкинут ошибку |     |
| turple |     |
| unknown |     |
| null и undefined |     |

-    enums

По сути удобный объект, специальный под typescript, с именованными константами для например описания каких-то состояний. Можно использовать как тип и определять переменные только из значений набора констант enum
числовые перечисления получают обратные свойства такие как value: key
```
{
  "0": "ERROR",
  "1": "WARN",
  "2": "INFO",
  "ERROR": 0,
  "WARN": 1,
  "INFO": 2,
  "DEBUG": "sdfc"
}
```

-    type / interface, differences between them

- интерфейсы можно расширять повторно открывая их и их нельзя многоразово наследовать. Также обычно интерфейсы принято использовать для объектов и используются только для публичных свойств. Типы - для всего остального

```
interface ICar extends IMovable
```

- типы
	- intersection (пересечения) & - объединение
	- union | перечисление либо этот, либо тот
	-    using interfaces with optional properties, read-only properties, etc...

```
interface IUser {
    readonly id: number; 
    name?: string; 
    // (name: string, surname: string): string; // функция и своства в ней
    authenticate(): void;
}
```

-    function types

(num: number) => void

-    [utitily types (optional)](https://www.typescriptlang.org/docs/handbook/utility-types.html)
-    typeguards `(optional)`

- `instanceof` - проверяет появляется ли прототип конструктора в прототипной цепочке объекта
- `typeof`
	`in` - свойство в цепочке прототипов
	
	`is` -  если функция true - pet is Cat
	

```
function isCat(pet: Dog | Cat): pet is Cat {
    return (pet as Cat).meow !== undefined;
}

let pet: Dog | Cat;
// Using the 'is' keyword
if (isCat(pet)) { // засовывается в проверку и утвеждает для блока кода ниже, что 
pet is Cat если true 
    pet.meow();
} else {
    pet.bark();
}
```

-    creating custom types
-    generic types (concept)

- Understanding the module system in ES6 and TypeScript.

## Design Patterns:

- Familiarity with Creational, Structural, and Behavioral Design Patterns.

Паттерны проектирования - шаблонное решение частой архитектурной проблемы

|     |     |     |
| --- | --- | --- |
| Creational - помогают решать вопросы с созданием объектов | Structural - помогают решать задачи по совмещению и сочетанию сущностей | Behavioral - распределяют ответственность между модулями и определяют, как именно будет происходить общение |
| [Factory Method](https://refactoring.guru/design-patterns/factory-method) - создание почти что одинаковых объектов с разными отличиями (средство передвижения - лодка самолёт, грузовик | [Adapter](https://refactoring.guru/design-patterns/adapter) - позволяет с помощью прослойки общаться между двумя несовместимыми объектами. Допустим преобразовать данные с апи | [Chain of Responsibility](https://refactoring.guru/design-patterns/chain-of-responsibility) - передаёт запрос по цепочке, и если хоть кто-то может его решить - решает, нет - передаёт |
| [Abstract Factory](https://refactoring.guru/design-patterns/abstract-factory) - позволяет в общем описать вид / тип предметов, но не создавать их. Фабрика  фабрик (интерфейс для классов | [Bridge](https://refactoring.guru/design-patterns/bridge) - разделение большого godObject на маленькие с композицией. Допустим постройка формы занимается один класс, покраской - другой | [Command](https://refactoring.guru/design-patterns/command) - объединение действий в один объект такой как редъюсер или в пример фетч |
| [Builder](https://refactoring.guru/design-patterns/builder) - позволяет выбирать какие элементы добавлять пошагово, а затем методом построить объект | [Composite](https://refactoring.guru/design-patterns/composite) - постройка дерева по сути по каким-то параметрам ( как категории товаровю Приходит с сервера массив категорий - с них дерево по родитель - потомок ) | [Iterator](https://refactoring.guru/design-patterns/iterator) - все способы обхода дерева в одном отдельном месте |
| [Prototype](https://refactoring.guru/design-patterns/prototype) - все методы (и определённые константы) каждого построенного объекта находятся в прототипе, чтобы не копировать логику в каждый объект отдельно | [Decorator](https://refactoring.guru/design-patterns/decorator) - добавляет к существующей функциональности дополнительную. Дебаунс или | [Mediator](https://refactoring.guru/design-patterns/mediator) - контроллер, которые рулит над остальными и общение только через него. Своего рода светофор на перекрёстке |
| [Singleton](https://refactoring.guru/design-patterns/singleton) - один экземпляр класса на проект. Если создать новый - вернётся экземпляр самого первого | [Facade](https://refactoring.guru/design-patterns/facade) - прячет сложную логику других модулей, представляя простые обобщённые методы | [Memento](https://refactoring.guru/design-patterns/memento) - кэширование или мемоизация |
|     | [Flyweight](https://refactoring.guru/design-patterns/flyweight) - выделяет данные в отдельную датабазу. Позволяет экономить оперативу на сокращении данных в объектах | [Observer](https://refactoring.guru/design-patterns/observer) - подписка на ивент Как addEventListener. Родитель и дочерний элемент имеют определённые интерфейсы. И в каком-то случае вызывает ивент |
|     | [Proxy](https://refactoring.guru/design-patterns/proxy) - промежуточный модуль, предоставляет интерфейс к какому-либо другому модулю. Например возвращает те же имена и почту, но уже в uppercase | [Strategy](https://refactoring.guru/design-patterns/strategy) - озволяет выбирать и даже менять алгоритм работы в зависимости от ситуации. Человек идёт - потом пересел на велосипед. По сути он передвегается, но другим способом |
|     |     | [State](https://refactoring.guru/design-patterns/state) - изменение поведения объекта в зависимости от состояния |
|     |     | [Template Method](https://refactoring.guru/design-patterns/template-method) - интерфейс с определённой структурой, который позволяет изменить поведение классов без изменения этой структуры |
|     |     | [Visitor](https://refactoring.guru/design-patterns/visitor) |

- SOLID principles and design patterns in project contexts.
	- single responsibility principle \- принцип единственной ответственности

1 сущность - 1 задача ибо получается godObject. Короче делегирование

- open close principle - открытости закрытости

открыты для расширения, закрыты для изменения. Код написан и протестирован, остальное основывать на нём. Не нужно регрессионное тестирование или тестирования предыдущего кода для проверки, что ничего не сломано

- Liskov Substitution Principle - принцип подстановки Барбары лисков

наследуемый класс должен дополнять, а не замещать функционал родительского класса

- Interface Segregation Principle - принцип разделения интерфейса

программные сущности не должны зависеть от методов, которые они не используют. То есть разделять опять же. Более предсказуемая работа и понятный код

- dependency inversion principle - принцип инверсии зависимостей

классы должны зависеть от абстракций, а не от конкретных деталей. То есть для работы между родительским классами и дочерними (деталями) не должно быть связи, чтобы если в одной что-то поменялось - поломалось другое. Должна быть абстракция, пульт управления между ними, в котором происходит общение между ними

- A. Модули верхних уровней не должны зависеть от модулей нижних уровней. Оба типа модулей должны зависеть от абстракций.
- B. Абстракции не должны зависеть от деталей. Детали должны зависеть от абстракций.

## Software Development Methodologies:

- Agile, Scrum, Kanban, and Waterfall methodologies.

Пользовательская история — это наименьшая единица работы в методике agile
![[Evernote/Core js 2/assets/image 4.png|image 4.png]]

- Waterfall - от начала до конца долгая и последовательная методология разработки. Обычно не всё делается параллельно и 1 разработчик передаёт так сказать эстафету другому

![[Evernote/Core js 2/assets/image 2.png|image 2.png]]

1. _Анализ требований проекта._ Определяются программные требования для информационной предметной области системы.
2. _Проектирование._ Разрабатывается и формулируется логически последовательная техническая характеристика программной системы. Детализация системы.
3. _Реализация ПО._ Воплощение полноценного проекта.
4. _Тестирование продукта._ Тестовая эксплуатация продукта
5. _Интеграция системы._ Включает установку и официальную приёмку продукта.
6. _Поддержка._ Предоставление технической помощи по продукту после запуска а коммерческую эксплуатацию.
	
	- Agile - гибкая быстрая философия разработки приложения. Выпустить версию приложения и потом допиливать её. Не концентрироваться на документации, главное - люди и общение и параллельная работа, быстрый ответ на запросы пользователей
	
	1. Люди и взаимодействие важнее процессов и инструментов.
	2. Работающий продукт важнее исчерпывающей документации.
	3. Сотрудничество с заказчиком важнее согласования условий контракта.
	4. Готовность к изменениям важнее следования плану.

- Scrum - то гибкая методика управления проектами, основанная на итерациях с фиксированной продолжительностью, называемых спринтами. Владелец продукта, [scrum-мастер](https://www.atlassian.com/ru/agile/scrum/scrum-master) и команда разработчиков. Backlog - все задания. Полный grooming с оценкой каждого таска по сложности и время затратности в story points. Есть какое-то кол-во поинтов на спринт и такое кол-во заданий может взять команда на него. В спринт ревью есть демо митинг, на котором все показывают что делали, на ретросперктиве - оценка спринта, плюсы и минусы. Обсуждаются проблемы и потом в конце след спринта отчитываются как в этом разобрались и исправили

![[Evernote/Core js 2/assets/image 3.png|image 3.png]]

- Kanban — это гибкая методика (метод, не равно скраму, а может ставится по верх) управления проектами, в которой задачи сопоставляются с ресурсами команды и которую внедряют для того, чтобы максимально ускорить работу. Всё находится в To Do и люди, закончившие одно могут приступать сразу же к другому. Work in progress - лимит, ограничивающий кол-во заданий в столбы in progress. Потом в какой-то момент решают когда выпускать версию, жёсткого дедлайна нет

- Project estimation techniques.
	1. ### Planni**ng Po**ker
		
	2. ### T-Shirt Size Estimation - своего рода замена story points покера на размер
		
	3. ### Dot Voting
		
	4. ### [3-Points Estimating](https://www.projectmanagement.com/wikis/368763/3-points-estimating)
		

## Testing:

по сути тестирование - описание как должен работать код, соответствовать требованиям, своего рода документация

- Understanding different types of testing (Integration, E2E, Performance).

|     |     |
| --- | --- |
| Функциональное | Нефункциональное |
| E2E - 10% тестирование пользовательского опыта. Нажатие на кнопку, заполнение формы и её последующая отправка | нагрузочное тестирование |
| Integration - 20-30% в связке, общение модулей | безопасности |
| Screenshots - (70-80%) проверка вёрстки страницы со старыми скриншотами | регрессионное |
| Unit - (70-80%)  пишется на отдельные маленькие независимые кусочки системы (функции, методы) |     |

- Test Pyramid.
- Test-driven development (TDD) and behavior-driven development (BDD).
	- Test-driven development - сначала пишутся тесты, потом реализация под них
	- Behavior Driven Development - использует поведение системы, чтобы описать как создавать приложение. **Gherkin syntax -** Given - When - Then

```
 Scenario: Add item to cart
    Given I am on the product page
    When I click on the "Add to Cart" button for product "ABC"
    Then the item "ABC" should be added to my cart
```

## Web Communication Protocols:

- Differences between HTTP and HTTPS.

Используется ssl/tls - шифрование
Тройное рукопожатие:

1. Браузер подтверждает SSL сертификат при  Server hello
2. публичный ключ может только зашифровать, приватный на стороне сервера - расшифровать
3. после обмена общение происходит через сессионный ключ

![[Evernote/Core js 2/assets/image 6.png|image 6.png]]

- Understanding HTTP methods, headers, responses, and status codes.

Uniform Resource Locator
```
GET /path HTTP/1.1 //стартовая строка
Host: www.youtube.com // обязательный заголовок
Content-Type: application/json
Content-Length: 26
```
CRUD - create read update delete

- get
- post
- put
- patch
- delete

Заголовки - доп информация и инструкции

- Concepts of RESTful APIs. Representational State Transfer

архитектурный стиль, набор правил как наиболее правильно и удобно использовать http

- Концепции
	- клиент - сервер
	- независимые запросы, нет состояния ( полная инфа как в первый раз общение)
	- кеширование
	- единый интерфейс
	- многослойная система

## Common Web-Security Knowledge:

- Basic security concepts including CORS, XSS, and OWASP Top 10.

```
Origin: https://javascript.info

Access-Control-Allow-Origin: https://javascript.info
```

- Authentication mechanisms (JWT, OAuth).

# Coding Task Examples:

### Polyfills and Extensions

1\. `Function.prototype.bind` Polyfill

```
function bind(func, context){
    return function(…args) { 
        return func.apply(context, args); 
    } 
}

Function.prototype.bind = function(context, ...args){
    let targetFunc = this;
    return function (...additionalArgs) {
        const rigthContext = this === global || this === undefined ? context : this;
        targetFunc.apply(rigthContext, ...args, ...additionalArgs)
    }
}
```

2\. `Object.create` Polyfill

```
function create(proto, props) {
  if (typeof proto !== 'object' && proto !== null) {
    throw new Error('Invalid proto');
  }
  if (typeof props !== 'object' && props !== undefined) {
    throw new Error('Invalid props');
  }
  return { ...props, __proto__: proto };
}
const a = { apple: true };
const b = { green: true };
console.log(create(a, b));
console.log(create(a));
console.log(create(null, b));
```

3\. `Array.flat` Polyfill

```
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
            if (Array.isArray(el) && depth) {
                recursiveFlat(el, depth - 1);
            } else {
                result.push(el);
            }
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

4\. `Array.reduce` Polyfill

-     Implement a polyfill for \`Array.reduce\`.

5\. String Repeating Function

-     Create a method that allows a string to be repeated a certain number of times, similar to the native \`String.prototype.repeat\`.

6\. Custom Join Function

-     Implement a function that constructs a string from an array of numbers, separated by a specified delimiter.

### Functional Programming Challenges

7\. Functional Expressions

-     Implement a series of functions that allow for functional expressions such as \`five(plus(seven(minus(three()))))\`.

```
function makeNum(num, func) {
  if (func === undefined) {
		return num;
	} else {
		return func(num);
	}
}
function zero(func) {
	return makeNum(0,func);
}
function one(func) {
	return makeNum(1,func);
}
function two(func) {
	return makeNum(2,func);
}
function three(func) {
	return makeNum(3,func);
}
function four(func) {
	return makeNum(4,func);
}
function five(func) {
	return makeNum(5,func);
}
function six(func) {
	return makeNum(6,func);
}
function seven(func) {
	return makeNum(7,func);
}
function eight(func) {
	return makeNum(8,func);
}
function nine(func) {
	return makeNum(9,func);
}
function plus(right) {
  return function(left) { return left + right; };
}
function minus(right) {
  return function(left) {
		return left - right;
	};
}
function times(right) {
  return function(left) { return left * right; };
}
function dividedBy(right) {
  return function(left) { return left / right; };
}
eight(minus(three())); // return 5
six(dividedBy(two())); // return 3
two(plus(five())); // return 7
five(plus(two())); // return 7
seven(times(five())); // return 35
console.log(four(plus(nine()))); // must return 13
```
8\. Curried Addition Function

-     Write a function that supports adding numbers in a curried form, such as \`add(5)(9)(-4)(1)\`.

Timing Functions

9\. Periodic Output

-     Write a function that logs the elapsed time in milliseconds at regular intervals.

10\. Extended Periodic Output

- Implement a function that logs the elapsed time in milliseconds at intervals that increase incrementally.

Working with Promises

11\. Sequential Promise Execution

- Write a function to execute an array of functions that return promises sequentially, ensuring each promise resolves before the next begins.

```
function queuPromises(promises) {
  let counter = 0;
  const promisesLength = promises.length;
  const resolvedPromises = new Array(promises.length);
  return new Promise((resolve) => {
    promises.forEach(async (promise, index) => {
      const value = await promise.catch(() => null);
      resolvedPromises[index] = value;
      counter += 1;
      if (counter >= promisesLength) {
        resolve(resolvedPromises.join(''));
      }
    });
  });
}
```
12\. Promise.all Polyfill

- Implement a function that behaves like \`Promise.all\`.

13\. Race Condition Handling with Promises

- Create a function to handle race conditions, resolving or rejecting with the value of the first promise that settles.

14\. Promise Chaining and Error Handling

- Write a function that handles promise chaining and catches any errors, useful for data fetching scenarios.

15\. Promisify Node-style Callbacks

- Convert traditional Node.js callback-style functions to return promises.
