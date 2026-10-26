# Core js 1

### Advanced Expressions

- `Object.is`

	like `===` but `NaN = NaN` and `-0 != 0`

- **Differences** between `let`, `var`, and `const`
	- var всплывает и индифицируется в undefined. Всплывает и в For, {}
	- присваивается глобальному объекту window
	- повторное объявление без ошибки
	- var только функциональная область видимости
- Exploring the Temporal Dead Zone.
	- Зона, в которой нельзя обратится к переменной до того, пока до неё не дойдёт код. Будет ошибка Refference Error (кроме function declaration and vars).
- Concepts of Hoisting.
	- Концепт поднятия всех переменных (и let с const) и функций на самый верх. Первым делом код объявит их. Только к нельзя обращаться до объявления к let и const.
- The role of polyfills.
	- Замена нового неподдерживаемого старыми браузерами код на поддерживаемую теми браузерами реализацию

### Functions

- Differences and uses of arrow functions, function expressions, and function declarations.
	- function declarations всплывает и можно использовать до объявления
	- function expressions не всплывает
	- arrow functions не имеет this, arguments, new, super (она получает this во время создания)

### Functional Scope

по сути scope это концепция и в js её реализует lexical Enviroment

- Global scope vs. functional scope
	- global scope это window или global ( последний в цепочке областей видимости )
	- функциональный в функции
- Variable visibility areas.
	- let const в любой блочной или функциональной области видимости
- Working with nested scopes.
	- с наружного в внутренний доступа нет, с внутреннего  к наружному - есть

### Functions Parameters / Arguments

- Defining function parameters.
	- function (a, b, c). Параметры создаются во время выполнения функции. аргументы - значения, которые передаются функции
- Differences in parameters passing by value and by reference.
	- значения копируются, на ссылочные  копируется ссылка. Опасность в мутации
- Handling a dynamic amount of function parameters.
	- arguments или ...rest

### Closures Advanced

- **Understanding** execution context and lexical environments.
	- контекст выполнения бывает глобальным и функциональным ( то есть создаётся только для глобального кода и каждой функции. Потом по мере выполнения кода эти объекты кладутся в call stack (можно увидеть структуру в дебаггере))
	- контекст выполнения состоит из стадия создания и стадия выполнения
	- на создании создаётся объект, который содержит лексическое окружение, this и где код сейчас
	- lexical environments - контейнер с переменным ( record ) и ссылкой на наружный lexical environments ( outer). Он реализует концепт области видимости в js!
	- всем функциям данной области присваивается текущий `[[Environment]]`

Для заумных

- As of ES6 it's not true in general that the variable environment contains "the variables and functions". It only contains variables declared with var, as let and const use the lexical environment.
- <https://stackoverflow.com/questions/23948198/variable-environment-vs-lexical-environment>
- <https://tc39.es/ecma262/#sec-block-runtime-semantics-evaluation>
- <https://262.ecma-international.org/5.1/#sec-10.3>

```typescript
ExecutionContext: // for global and every function. Create Execution Context Stack or Call stack
(1) LexicalEnvironment: //function scope, for let and const
        d -> nothing // let const. Before initialization, "nothing" causes an error
        outer: global // outer is taken from [[Enviroment]] property (Enviroment in which func were created) of the function. It is set when function creates
            LexicalEnvironment //block scope (for, while, if, {}). Creates on a moment of block execution where func lexical env sets to outer. For let and const too
                b -> 2
                outer: (1)
    VariableEnvironment: // function and ineer block scopes for function declaration and var
        a -> 1, c -> undefined, func -> {} // function and also all inner block scopes for function declaration and var (var and function declarations doesn't have doesn't have block scope)
        outer: global
    ...
    This: global //How you call function. Object before method
    [where the code interpretator now]: 134:14
```

![[Evernote/Core js 1/assets/image.png|image.png]]
В этом моменте когда интерпретатор доходит до блока, то он для него создаёт новое лексическое оружение и ставит его как выполняемый для функции/глобального кода. А в конце выходит из него и ставит опять наружный
Все вары и декларации функции залетают в окружение переменных

Differences between scope and context.

- this никак не относится в области видимости и вычисляется (не ищется в области) в зависимости от того, как вызвана функция

- The mechanism of lexical environment traversal.
	- идёт как по областям видимости, с внутренней на наружную. Это же концепт области видимости
- Connection between function and its lexical environment.
	- функция -> свой контекст выполнения -> своё лексическое окружение

### Advanced Functions

- `this` in functions

	Зависит от как вызвана функция:

- конструктор - новый возвращаемый объект
- обычная функция - глобальный объект либо undefined в strict mode
- методом - объект перед точкой, для который вызывается метод
- call, apply, bind - что дали первым параметром

- Reference Type & losing `this`
	- потерять this можно, когда записываешь метод объекта в отдельную переменную и потом вызываешь функцию без синтаксиса с точкой как func()
	- . - оператор, который передаёт функции специальный объект с this. Если перед точкой не объект, а (), this может потерятmся
- Understand difference between function and method
	- метод - функция ассоциируемая с свойством объекта. Разницы так-то нет, ибо глобальные функции это методы глобального объекта
- Understand how \`this\` works, realize \`this\` possible issues
	- . - оператор, который передаёт функции специальный объект с this. Если перед точкой не объект, а (), this может потерятmся
- Manage `this`
	- ниже
- Be able to replace `this` value
	- bind( this, arg1...)
- Be able to use `call` and `apply` Function built-in methods
	- apply(this, \[arg1, arg2\]). Call без массива
- Know how to bind `this` scope to function
	- bind( this, arg1...)
- Binding, binding one function twice
	- невозможно со встроенным Bind. Только если использовать такую реализацию

```Typescript
Function.prototype.bind = function (context) {
    const targetFunc = this
    return function (...args) {  
        const rightContext = this === global ? context : this;
        return targetFunc.apply(rightContext, args);
    };
};
```

### ECMAScript Intermediate

- Function default parameters.
	- ( b, a = 4 )
- Using spread operator for function arguments.
	- (a, b, ...rest) Только последним, ругается js
- Comparing `arguments` and `rest parameters`
	- `arguments` - array like.
	- rest не меняются, когда меняют аргументс
- Array concatenation with spread operator.
	- \[...arr, ...arr2\]
- Destructuring assignments for variables and function arguments.
	- (\[arg1, arg2\]) ({arg1, arg2})
- \`for..of\` loop (optional).
	- for (const a of str/Object.keys()/array){}

### Objects Built-in Methods

- Utilizing \`Object.keys\` and \`Object.values\`.

	

- Working with static Object methods.

	freeze, fromEnteries, Enteries, create(proto, descriptors)

- Property flags and descriptors.

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

- Creating iterable objects and using \`Symbol.iterator\` (optional).

### ECMAScript Data Types & Expressions

- Working with Object computed properties.
	- {\[var + var2\]: value}

### Arrays Built-in Methods

map forEach reduce push pop shift unshift flat sort join split slice splice

- Copying and modifying arrays.
	- slice(), concat, \[...\], map, arr\[i\]?
- Flattening nested arrays
	- flat(deepth)

### Arrays Iterating, Sorting, Filtering

- Sorting and custom sorting arrays

```Typescript
function compare(a, b) { // return a - b или наоборот если работает с числами
  if (a меньше b по некоторому критерию сортировки) {
    return -1; // a ставит слева, б справа
  }
  if (a больше b по некоторому критерию сортировки) {
    return 1; // б ставит слева, а справа
  }
  // a должно быть равным b
  return 0;  // оставляет как есть
}
```

## JavaScript in Browser:

### Global Object Window

- Understanding the Document Object Model (DOM).

### Events Basics

- Types of DOM Events.
	- мыши.
	- на элементе управления (submit, input, focus, change. blur)
	- документа (load beforeunload unload DomContentLoaded)
	- css (transitionend)
- Working with Mouse and Keyboard Events.
- Handling Form and Input Events.
	- submit, input, focus, change
- Event Listeners.
	- on(event), addEventListener.
- Event Phases and their differences.

	погружение, фаза цели или захвата (target), всплытие. Остановка всплытия event.stopPropagation

- Custom events (optional).

	(элемент, на котором вызывает событие).dispatchEvent(new Event((название ивента: string))

### Events Propagation / Preventing

\- Event propagation cycle.
	погружение, фаза цели или захвата (target), всплытие. Остановка всплытия
\- Stopping event propagation.
	event.stopPropagation
\- Preventing default browser behavior.
	event.preventDefault
\- Event delegation and its pros/cons.
	1 обработчик на родителя и проверка на нужные элемент вместо кучи слушателей на дочерние элементы. Так же вместо 100 удалений слушателей с детей просто удалить с родителя

### Timers

\- Usage of \`setTimeout\` / \`setInterval\`.
\- Clearing timers with \`clearTimeout\` / \`clearInterval\`.
	setTimeoutб setTimeout возвращают индетификатор. Их передать в  `clearTimeout` / `clearInterval`

### Web Storage API & Cookies

\- Differences between LocalStorage, SessionStorage, and Cookies.
	на заметку: LocalStorage, SessionStorage безопаснее, так как не передаётся на сервер и хранятся у пользователя

### Date & Time (optional)

\- Working with the Date object.
\- Timezones and Internationalization in JavaScript (Intl).
	многие методы имеют UTC

## Design Patterns

Intermediate Knowledge

\- Understanding and applying KISS, DRY, and YAGNI principles.
