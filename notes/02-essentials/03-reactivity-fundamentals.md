# Reactivity Fundamentals

## Declaring Reactive State

### `ref()`

```html
<template>
  <div>{{ count }}</div>
</template>

<script setup>
import { ref } from 'vue'

const count = ref(0)
console.log(count.value) // 0
</script>
```

> ℹ️ No need to append `.value` when using the ref in the template <br />
> Refs are automatically unwrapped when used inside templates

### How Vue's reactivity system works.

✅ Vue uses a **dependency-tracking reactivity system** <br/>
✅ Vue **tracks** every ref used in templates during render <br/>
✅ When a ref **changes or mutates**, Vue **triggers** a re-render and updates the DOM <br/>
✅ Vue **intercepts get and set operations** of object properties using **getters and setters** <br/>
✅ The `.value` property lets Vue **detect when a ref is accessed or mutated** <br/>
✅ Vue **tracks** refs in the getter and **triggers updates** in the setter <br/>
✅ Refs can be **passed to functions** while keeping their **reactivity** <br/>

```ts
// pseudo code, not actual implementation
const myRef = {
  _value: 0,
  get value() {
    track()
    return this._value
  },
  set value(newValue) {
    this._value = newValue
    trigger()
  }
}
```

- [Reactivity in depth](https://vuejs.org/guide/extras/reactivity-in-depth.html)

### Deep Reactivity

```ts
import { ref } from 'vue'

const obj = ref({
  nested: { count: 0 },
  arr: ['foo', 'bar']
})

function mutateDeeply() {
  // these will work as expected.
  obj.value.nested.count++
  obj.value.arr.push('baz')
}
```

✅ Refs can hold any type value including deeply nested objects, arrays, or built-in data structures like `Map` <br/>
✅ Non-primitive values are turned into reactive proxies via `reactive()` <br/>

- [Reduce Reactivity Overhead for Large Immutable Structures](https://vuejs.org/guide/best-practices/performance#reduce-reactivity-overhead-for-large-immutable-structures)
- [Integration with External State Systems](https://vuejs.org/guide/extras/reactivity-in-depth#integration-with-external-state-systems)

### DOM Update Timing

✅ The DOM updates automatically when reactive state changes <br/>
✅ Updates are **buffered** until the next tick in the update cycle <br/>  

```ts
import { nextTick } from 'vue'

async function increment() {
  count.value++
  await nextTick()
  // Now the DOM is updated
}
```

## `reactive()`


---

- [Reactivity Fundamentals](https://vuejs.org/guide/essentials/reactivity-fundamentals.html)
