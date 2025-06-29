# Ripplex

[![npm version](https://img.shields.io/npm/v/ripplex-core.svg)](https://www.npmjs.com/package/ripplex-core)
[![bundle size](https://img.shields.io/bundlephobia/minzip/ripplex-core)](https://bundlephobia.com/result?p=ripplex-core)
[![typescript](https://img.shields.io/badge/TypeScript-supported-blue.svg)](#typescript-support)
[![license](https://img.shields.io/npm/l/ripplex-core)](LICENSE)

**Ripplex** is a lightweight, reactive state management library for **React** with built-in signals, event-driven architecture, and automatic async state handling — all with zero dependencies.

---

## Why Ripplex?

- **Reactive**: Trigger component updates using `ripple()` and `useRipple()`
- **Async-ready**: Auto-manage loading and error states with `useRippleEffect()`
- **Event-driven**: Decouple logic with built-in `emit()` and `on()`
- **Tiny**: <1.2KB gzipped, tree-shakable
- **Type-safe**: Fully written in TypeScript
- **Zero-config**: No provider or context setup needed

---

## Installation

```bash
npm install ripplex-core
```

---

## ⚡ Quick Start

```tsx
import { ripple, useRipple, emit, useRippleEffect } from "ripplex-core";

const counterStore = {
  count: ripple(0),
  loading: ripple(false),
  error: ripple(null),
};

function Counter() {
  const count = useRipple(counterStore.count);
  const loading = useRipple(counterStore.loading);

  useRippleEffect(
    "increment:async",
    async () => {
      await new Promise((res) => setTimeout(res, 1000));
      counterStore.count.value += 1;
    },
    counterStore
  );

  return (
    <div>
      <h2>{count}</h2>
      <button onClick={() => emit("increment:async")}> 
        {loading ? "Loading..." : "+1 Async"}
      </button>
      <button onClick={() => (counterStore.count.value -= 1)}>-1</button>
    </div>
  );
}
```

---

## Core Concepts

### Reactive State

```ts
const userStore = {
  name: ripple(""),
  isLoggedIn: ripple(false),
};
```

### useRipple()

```tsx
function UserProfile() {
  const name = useRipple(userStore.name);
  const loggedIn = useRipple(userStore.isLoggedIn);

  return <div>{loggedIn ? `Hello ${name}` : "Please login"}</div>;
}
```

### Event-Driven Logic

```ts
import { emit, on } from "ripplex-core";

on("user:login", (data) => {
  userStore.name.value = data.name;
  userStore.isLoggedIn.value = true;
});

emit("user:login", { name: "Kavya" });
```

### Async State Handling

```ts
const todoStore = {
  todos: ripple([]),
  loading: ripple(false),
  error: ripple(null),
};

useRippleEffect(
  "fetch:todos",
  async () => {
    const res = await fetch("/api/todos");
    todoStore.todos.value = await res.json();
  },
  todoStore
);

emit("fetch:todos");
```

---

## Store Pattern

```ts
export const appStore = {
  user: ripple(null),
  todos: ripple([]),
  loading: ripple(false),
  error: ripple(null),
};

function App() {
  const user = useRipple(appStore.user);
  const loading = useRipple(appStore.loading);

  useRippleEffect(
    "load:user",
    async (userId) => {
      const res = await fetch(`/api/users/${userId}`);
      appStore.user.value = await res.json();
    },
    appStore
  );

  return (
    <div>
      {loading ? "Loading..." : user?.name}
      <button onClick={() => emit("load:user", 123)}>Load User</button>
    </div>
  );
}
```

---

## TypeScript Support

```ts
interface User {
  id: number;
  name: string;
}

const userStore = {
  currentUser: ripple<User | null>(null),
  users: ripple<User[]>([]),
};
```

---

## API Reference

| Function                          | Description                                      |
|----------------------------------|--------------------------------------------------|
| `ripple(initialValue)`           | Create a reactive signal                         |
| `useRipple(signal)`              | React hook to subscribe to a signal              |
| `emit(event, payload?)`          | Emit a custom event                              |
| `on(event, handler)`             | Listen for custom events                         |
| `useRippleEffect(event, fn, store?)` | Handle async logic with loading/error tracking |

---

## Comparison

| Feature              | Ripplex         | Zustand        | Redux           |
|---------------------|------------------|----------------|------------------|
| Reactive signals     | ✅ Built-in       | ⚠️ External    | ❌ Manual        |
| Event bus            | ✅ Built-in       | ❌             | ⚠️ Middleware    |
| Async loading/error  | ✅ Auto-managed   | ❌ Manual       | ⚠️ Middleware    |
| Bundle size          | < 1.2 KB         | ~1.2 KB        | 15–20 KB         |
| Boilerplate-free     | ✅ Yes            | ✅ Yes          | ❌ No            |
| DevTools             | Ὢ7 Coming soon   | ✅ Yes          | ✅ Yes           |

---

## Contributing

We welcome contributions!

```bash
# Setup project
npm install

# Run build
npm run build

# Start dev (if needed)
npm run dev
```

Open a PR with your enhancements or bugfixes. Let’s improve this together!

---

## License

MIT — [View License](./LICENSE)

---


