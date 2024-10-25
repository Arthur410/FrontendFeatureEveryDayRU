### Что такое Debounce и Throttle?

---

В JavaScript `debounce` и `throttle` — это техники оптимизации вызова функций, которые предотвращают их слишком частое выполнение. Они полезны, когда функция вызывается часто, например, при скролле, ресайзе окна или вводе текста.

### Разница между debounce и throttle:
`debounce` — откладывает выполнение функции до тех пор, пока она не перестанет вызываться в течение определенного времени. Используется, когда важно дождаться "затишья" перед вызовом функции, например, при отправке поискового запроса по мере ввода текста.

`throttle` — ограничивает вызов функции, разрешая её выполнение не чаще чем через указанный интервал. Используется, когда нужно выполнять функцию с регулярным интервалом, например, при обновлении позиции при скролле или ресайзе окна.

### debounce

```ts
function debounce(func, delay) {
  let timeout;
  
  return (...args) => {
    if (timeout) 
      clearTimeout(timeout)
    
    timeout = setTimeout(() => {
      func(...args)
    }, delay)
  }
}

// Использование debounce
const handleInput = debounce((event) => {
  console.log("Отправка запроса:", event.target.value);
}, 500);

document.getElementById("searchInput").addEventListener("input", handleInput);
```

### throttle

```ts
function throttle(func, interval) {
  let lastCall = 0;
  
  return (...args) => {
    const currentCall = Date.now();
    
    if (currentCall - lastCall >= interval) {
      func(...args)
      lastCall = currentCall;
    }
  }
}

// Использование throttle
const handleScroll = throttle(() => {
  console.log("Позиция скролла обновлена");
}, 200);

window.addEventListener("scroll", handleScroll);
```
