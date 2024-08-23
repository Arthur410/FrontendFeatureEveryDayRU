#### Фича дня: Композиционные функции в Vue 3
##### Что это?
**Композиционные функции** (composable functions) - это функции, которые позволяют извлекать и повторно использовать логику между компонентами в Vue 3. В отличие от mixins, которые могут создавать конфликты имен, композиционные функции предлагают более чистый и гибкий способ организации кода.

Пример использования:
Предположим, у вас есть логика, которая управляет таймером, и вы хотите использовать ее в нескольких компонентах.
```ts
// useTimer.ts
import { ref, onMounted, onUnmounted } from 'vue';

export function useTimer() {
  const seconds = ref(0);
  let interval: number;

  onMounted(() => {
    interval = setInterval(() => {
      seconds.value++;
    }, 1000);
  });

  onUnmounted(() => {
    clearInterval(interval);
  });

  return { seconds };
}
```
Затем вы можете использовать эту композиционную функцию в любом компоненте:

```ts
<template>
  <div>Прошло {{ seconds }} секунд</div>
</template>

<script setup lang="ts">
import { useTimer } from './useTimer';

const { seconds } = useTimer();
</script>
```
#### Почему это полезно?
Композиционные функции делают код более читаемым и переиспользуемым, что особенно важно в крупных проектах. Они позволяют легко извлекать логику и повторно использовать её в разных компонентах без риска возникновения конфликтов.