# AGENT.md — Vue 3 + TypeScript Sandbox (`sandbox-src`)

## 1. Призначення та архітектура
Ця папка (`sandbox-src`) — ізольована пісочниця для вивчення та тестування інтерактивних модулів на стеку **Vue 3 (Composition API) + TypeScript + Vite** в межах проєкту `Open-Educational-Platform`.

* **Вихідний код (Source):** зберігається тут (`sandbox-src/src/`). Ця папка **не публікується** напряму на GitHub Pages.
* **Зібраний проєкт (Build Output):** генерується у папку `../docs/sandbox/`. Саме звідти GitHub Pages роздає готову статику за окремим URL (`/sandbox/`).

---

## 2. Критичні правила конфігурації (`vite.config.ts`)
Під час зміни конфігурації Vite обов'язково зберігати два параметри:
1. `base: '/Open-Educational-Platform/sandbox/'` (або `'/sandbox/'`, якщо використовується кастомний домен) — щоб шляхи до JS/CSS чанків не ламалися на GitHub Pages.
2. `build.outDir: '../docs/sandbox'` та `build.emptyOutDir: true` — щоб білд автоматично оновлював підпапку деплою, не зачіпаючи основний сайт у `../docs/`.

---

## 3. Робочий процес (Workflow)
Усі команди виконуються з директорії `sandbox-src/`:

* **Локальна розробка (з Hot-Reload):**
  ```bash
  npm run dev
  ```
* **Перевірка типів TypeScript та збірка для деплою:**
  ```bash
  npm run build
  ```
  *(Після `npm run build` з'являться оновлені файли в `docs/sandbox/`, які потрібно закомітити та запушити в Git).*

---

## 4. Структура папок усередині `sandbox-src`
* `index.html` — точка входу HTML для SPA-додатку.
* `src/main.ts` — ініціалізація Vue-додатку (`createApp(App).mount('#app')`).
* `src/App.vue` — кореневий компонент пісочниці.
* `src/components/` — окремі інтерактивні компоненти та мінімодулі.
* `src/assets/` — стилі та статичні ресурси.

---

## 5. Шпаргалка по Vue 3 + TypeScript (для швидкого старту)
Усі нові компоненти пишемо у форматі **SFC (Single File Component)** з `<script setup lang="ts">`:

```vue
<script setup lang="ts">
import { ref, reactive, computed } from 'vue'

// 1. Props із суворою типізацією
interface Props {
  title: string
  initialCount?: number
}
const props = withDefaults(defineProps<Props>(), {
  initialCount: 0
})

// 2. Реактивність
const count = ref<number>(props.initialCount) // примітиви через ref (.value у TS)
const state = reactive({ isActive: true })    // об'єкти через reactive (без .value)

// 3. Обчислювані властивості
const doubleCount = computed(() => count.value * 2)

// 4. Методи
function increment() {
  count.value++
}
</script>

<template>
  <div class="module-card">
    <h2>{{ title }}</h2>
    <p>Значення: {{ count }} (подвоєне: {{ doubleCount }})</p>
    <button @click="increment">+1</button>
  </div>
</template>

<style scoped>
/* Ізольовані стилі лише для цього компонента */
.module-card {
  padding: 1rem;
  border: 1px solid #ccc;
}
</style>
```
