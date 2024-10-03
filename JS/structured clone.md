# structuredClone

Created: October 3, 2024 8:52 PM

Вы знали, что теперь в JavaScript есть нативный способ делать глубокие копии объектов? Это стало возможным с помощью функции `structuredClone`, встроенной в среду выполнения JavaScript:

```jsx
const calendarEvent = {
  title: "Builder.io Conf",
  date:new Date(123),
  attendees: ["Steve"]
}

// 😍
const copied = structuredClone(calendarEvent)
```

Вы заметили, что в этом примере мы скопировали не только объект, но и вложенный массив, и даже объект Date?

И код работает именно так, как мы и ожидали:

```jsx
copied.attendees // ["Steve"]
copied.date // Date: Wed Dec 31 1969 16:00:00
cocalendarEvent.attendees === copied.attendees // false
```

`structuredClone` может делать не только вышеперечисленное, но и также:

- Клонировать бесконечно вложенные объекты и массивы.
- Клонировать циклические ссылки.
- Клонировать широкий спектр типов JavaScript, таких как: `Date`*,* `Set`*,* `Map`*,* `Error`*,* `RegExp`*,* `ArrayBuffer`*,* `Blob`*,* `File`*,* `ImageData` и [многие другие](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Structured_clone_algorithm#supported_types).
- Передавать любые [передаваемые объекты](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Transferable_objects).

Это безумие даже будет работать так, как мы и ожидали:

```jsx
const kitchenSink = {
  set:new Set([1, 3, 3]),
  map:new Map([[1, 2]]),
  regex: /foo/,
  deep: { array: [new File(someBlobData, 'file.txt') ] },
  error:new Error('Hello!')
}
kitchenSink.circular = kitchenSink

// ✅ Выполнено полное глубокое копирование
const clonedSink = structuredClone(kitchenSink)
```

### **Почему бы просто не сделать object spread?**

Важным отметить, что мы говорим о глубоком копировании. Если же нужно просто выполнить поверхностное копирование, то есть копирование без включения вложенных объектов или массивов, то можно просто выполнить spread объекта:

```jsx
const simpleEvent = {
  title: "Builder.io Conf",
}
// ✅ нет вложенных объектов или массивов
const shallowCopy = {...calendarEvent}
```

Или даже один из этих вариантов, если хотите:

```jsx
const shallowCopy = Object.assign({}, simpleEvent)
const shallowCopy = Object.create(simpleEvent)
```

Но как только появляются вложенные элементы, мы сталкиваемся с проблемой:

```jsx
const calendarEvent = {
  title: "Builder.io Conf",
  date:new Date(123),
  attendees: ["Steve"]
}

const shallowCopy = {...calendarEvent}

// 🚩 упс - мы добавили "Bob" и в копию и в воригинальное событие
shallowCopy.attendees.push("Bob")

// 🚩 упс - мы обновили дату копии и исходного события
shallowCopy.date.setTime(456)
```

Как видно, мы не сделали полную копию этого объекта.

Вложенные дата и массив по-прежнему являются общей ссылкой для оригинала и «копии». Это может привести к проблеме – если мы захотим отредактировать их, думая, что обновляем только скопированный объект события календаря.

### **Почему не JSON.parse(JSON.stringify(x))?**

На самом деле это отличный хак и на удивление производительный, но с некоторыми недостатками, которые устраняет `structuredClone`.

Возьмем для примера:

```jsx
const calendarEvent = {
  title: "Builder.io Conf",
  date:new Date(123),
  attendees: ["Steve"]
}

// 🚩 JSON.stringify преобразовал дату в строку
const problematicCopy = JSON.parse(JSON.stringify(calendarEvent))
```

Если вывести `ProblematicCopy`, мы получим:

```jsx
{
  title: "Builder.io Conf",
  date: "1970-01-01T00:00:00.123Z"
  attendees: ["Steve"]
}
```

Мы хотели не этого. `date` должен быть не строкой, а объектом `Date`.

Это произошло потому, что `JSON.stringify` может обрабатывать только базовые объекты, массивы и примитивы. Любой другой тип может быть обработан непредсказуемым образом. Например, Dates преобразуются в string. Но `Set` просто преобразуется в `{}`.

Что-то `JSON.stringify` даже игнорирует – например, `undefined` или функции.

Скажем, если мы скопируем пример `kitchenSink` с помощью этого метода:

```jsx
const kitchenSink = {
  set:new Set([1, 3, 3]),
  map:new Map([[1, 2]]),
  regex: /foo/,
  deep: { array: [new File(someBlobData, 'file.txt') ] },
  error:new Error('Hello!')
}

const veryProblematicCopy = JSON.parse(JSON.stringify(kitchenSink))
```

То мы получим:

```jsx
{
  "set": {},
  "map": {},
  "regex": {},
  "deep": {
    "array": [
      {}
    ]
  },
  "error": {},
}
```

Фу!

И да, пришлось удалить циклическую ссылку, которая у нас изначально для этого была, поскольку `JSON.stringify` просто выдает ошибки, если встречается с одной из них.

Метод `JSON.stringify` удобен, в случае если наши требования соответствуют его возможностям. Однако с помощью `StructuredClone` можно сделать многое из того, чего не может `JSON.stringify`.

### **Почему не _.cloneDeep?**

До сих пор распространенным решением этой проблемы была функция `cloneDeep` библиотеки Lodash.

Она действительно работает так, как ожидается:

```jsx
import cloneDeepfrom 'lodash/cloneDeep'

const calendarEvent = {
  title: "Builder.io Conf",
  date:new Date(123),
  attendees: ["Steve"]
}

// ✅ Все в порядке
const clonedEvent = structuredClone(calendarEvent)
```

Но с одной оговоркой. Согласно данным работы расширения [Import Cost](https://marketplace.visualstudio.com/items?itemName=wix.vscode-import-cost) в IDE, которое выводит вес в Кб всего, что я импортирую, эта функция занимает 17,4 Кб в сжатом виде (5,3 Кб в архиве):

![https://habrastorage.org/r/w1560/getpro/habr/upload_files/403/0f5/bf1/4030f5bf1f55dbce849c4f44a4e9d1b8.png](https://habrastorage.org/r/w1560/getpro/habr/upload_files/403/0f5/bf1/4030f5bf1f55dbce849c4f44a4e9d1b8.png)

Это предполагает, что вы импортируете только эту функцию. Если вместо этого импортировать более распространенным способом, не принимая в расчет, что tree shaking не всегда работает так, как ожидается, можно случайно импортировать до [25 Кб](https://bundlephobia.com/package/lodash@4.17.21) только для этой одной функции.

![https://habrastorage.org/r/w1560/getpro/habr/upload_files/d66/506/31c/d6650631c32fdf87698d68320062a544.jpeg](https://habrastorage.org/r/w1560/getpro/habr/upload_files/d66/506/31c/d6650631c32fdf87698d68320062a544.jpeg)

Хотя это и не станет концом света, в нашем случае это просто не нужно – не тогда, когда браузеры уже имеют встроенный `structuredClone`.
