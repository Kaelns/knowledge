# Node

`node -v`   — действующая версия ноды

	### npm - packet manager
	

`npm -h`      — показывает список всех доступных команд

	### nvm - node version manager
	

`nvm help`
`nvm ls`      — все установленные ноды

- REPL (Read-Eval-Print-Loop)

`node` and `.help`

* * *

- `process.stdin` - input stream, it works with data in binary format (`Buffer` object). To work with data it's needed to convert it `toString()`

Прослушка на
```typescript
stdin.on("data", (data) => stdout.write(data));
```

- `process.stdout` - output stream

Без \\n
```typescript
stdout.write("Node.js");
```

- `process.stderr` - error stream as a variation of the output stream
		### `process.exit()`
	

```typescript
process.on("exit", (exitCode) => stdout.write("Good luck learning Node.js!"));
```
takes an optional argument `exitCode`, represented by an integer.
By default, this method is called with `exitCode === 0`. This type of process termination indicates that the program executed successfully and ran without errors.
Exiting the process with any other `exitCode` signifies that the program terminated with an error.

- `process.argv`

![[Evernote/Programming/assets/image.png]]
И флаг, и аргументы

- command line options (CLI options) ставятся перед путём после node. Даёт разную функциональность как отключение \_\_proto\_\_
- Environment variables have a syntax like `variable_name=variable_value` and are placed before `node ....`

```
PRODUCTION=false node test
```
`const productionMode = process.env.PRODUCTION === "true";`

- \_\_dirname
- \_\_filename

* * *

- `const path = require("path");`

- Retrieving file data

```
path.basename(__filename); // index.js - file name on Windows, full file path on POSIX systems
path.dirname(__filename); // C:\Users\Admin\Desktop\nodejs-basic - folder name
path.extname(__filename); // .js - file extension
path.parse(__filename)  // returns an object specifying the disk root, folder name, file name, file extension, file name without extension
```

- `path.join()` concatenates the specified path segments together, using the separator for the specific platform
- `path.resolve()` converts a sequence of paths or path segments into an absolute path from right to left and normalizes it: if some path segments have slashes while others don't, it will still generate the correct path.

* * *

- `const fs = require("fs");`

- `fs.mkdir(path.join(__dirname, "notes"), (err) => {});`
- `fs.writeFile( путь к файлу, текст, коллбэк с ошибкой);` перезаписывает всё в файле на "тексте"
- `fs.appendFile( путь к файлу, текст, коллбэк с ошибкой);` добавляет в конец файла "текст"
- `fs.readFile( путь к файлу, ['utf-8',] callback(err, data));` данные вернуться объектом Buffer, поэтому лучше использовать utf-8 (или data.toString())
- `fs.rename( путь к файлу, путь к фалу с новым именем] callback(err));`

- The `Events` module allows to create and generate your own events.

```
const EventEmitter = require("events");
const emitter = new EventEmitter();

emitter.on("start", (first, second) => console.log(`${first} and ${second}`));

emitter.emit("start", 1, 2); // 1 and 2
```

- Один и тот же обработчик может быть назначен много раз и так же вызовется столько же раз
- Чтобы поместить обработчик в начало очереди используется `prependListener` метод вместо `on`
- Чтобы сработал только один обработчик используется `once` вместо `on`
- `.off('event', handler)` удаляет обработчик события, `emit` данного события  после `off`больше работать не будет

* * *

- `Streams`

- Запись и чтение обычно производится `chunks`, это позволяет сэкономить память при работе с относительно большими файлами
- Потоки используют управляемые ивентами интерфейсы, наследуемые от `EventEmitter`
- `Readable stream`

```typescript
const fs = require("fs");

const stream = fs.createReadStream("source.txt", "utf-8");

let data = "";

stream.on("data", (chunk) => (data += chunk));
stream.on("end", () => console.log("End", data.length));
stream.on("error", (error) => console.log("Error", error.message));
```
	`data` ивент происходит, когда стрим считывает чанк и готов его отдать потребителю

- `Writable stream`

```typescript
const fs = require("fs");

const input = fs.createReadStream("source.txt", "utf-8");
const output = fs.createWriteStream("destination.txt");

input.on("data", (chunk) => output.write(chunk));
input.on("error", (error) => console.log("Error", error.message));
```

		## `pipe()` метод есть у каждого стрима,  может использоваться для комбинирования одного стрим с другим. Может создаваться цепочка  комбинирования стримов. Вместо подписок на ивент data в примере выше можно просто написать так:
	

```typescript
input.pipe(output);
```

- Можно сжимать чанки и передавать их другому стриму

```typescript
const { createGzip } = require("zlib");
const gzip = createGzip();

input.pipe(gzip).pipe(output);
```

- `pipeline` функция позволяет объединить множество стримов и использовать один общий обработчик ошибок

```typescript
pipeline(input, gzip, output, (err) => {
  if (err) {
    // handle errors
  }
});
```

* * *

Devtools

- [devrools dom](https://developer.chrome.com/docs/devtools/dom/#scroll1)

* * *

Asynchronius and Event loop

- Rendering every 16 mls (60 fps)

JS can be in time to handle couple of handlers

	

macrotask - 1
animation callback - all that was in queue
microtasks - all in queue and new in them while the queue is not empty

- for await of

* * *

## React

- `<Button title={obj.title} name={obj.name}>`
- `<Button {...obj}>`
- ![[Evernote/Programming/assets/image 1.png]]
- ![[Evernote/Programming/assets/image 2.png]]
- Even when a component is memoized, it will still re-render when a context that it’s using changes. Memoization only has to do with props that are passed to the component from its parent.
- ![[Evernote/Programming/assets/image 4.png]]![[Evernote/Programming/assets/image 5.png]]
	
- ![[Evernote/Programming/assets/image 3.png]]
- с помощью `useRef` можно хранить значение в current между ререндерами
	<https://youtu.be/QQYeipc_cik?si=wCfXo-vtxVQ6bO-8>
	
	

```
React.ComponentProps<'img'>
```

* * *

# Redux

<https://www.youtube.com/watch?v=YROz0WYExww&t=269s>

- useAppSelector - использовать на самых маленьких единицах и со сложностью O(1) без каких либо вычислений и логики внутри, т.к. селектор вызывается на каждое изменения стора и сравнивает с предыдущим значением. То есть логика будет срабатывать даже чаще, чем перерендер, и если что-то пойдёт не так - просадка производительности
- последния функция - обработчик, которая вызывается только при изменении селекторов параметров. Первые функции - селекторы, вторым+ параметром идут свои агрументы. Работает лучше, чем кастомный мемо, ибо он срабатывает единожды, а не множественно для каждого компонента![[Evernote/Programming/assets/image 6.png]]

* * *

BlurHash
