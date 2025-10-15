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

ℹ️ No need to use `.value` inside templates <br/>
✅ Refs are automatically unwrapped in templates <br/>

### How Vue's Reactivity System Works

✅ Vue uses a **dependency-tracking reactivity system** <br/>
✅ Tracks every ref used in templates during render <br/>
✅ When a ref **changes**, Vue **triggers** a re-render and updates the DOM <br/>
✅ Vue **intercepts get/set operations** with **getters and setters** <br/>
✅ `.value` lets Vue **detect ref access or mutation** <br/>
✅ Vue **tracks** in the getter and **updates** in the setter <br/>
✅ Refs can be **passed through functions** while staying reactive <br/>
✅ `ref()` wraps the value in a special reactive object <br/>

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

🔗 [Reactivity in Depth](https://vuejs.org/guide/extras/reactivity-in-depth.html)

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

✅ Refs can hold any type value like objects, arrays, maps, etc. <br/>
✅ Non-primitive values become reactive proxies via `reactive()` <br/>

🔗 [Reduce Reactivity Overhead for Large Immutable Structures](https://vuejs.org/guide/best-practices/performance#reduce-reactivity-overhead-for-large-immutable-structures) <br/>
🔗 [Integration with External State Systems](https://vuejs.org/guide/extras/reactivity-in-depth#integration-with-external-state-systems) <br/>

### DOM Update Timing

✅ The DOM updates automatically when reactive state changes <br/>
⚠️ Updates are **buffered** until the next tick of the update cycle <br/>

```ts
import { nextTick } from 'vue'

async function increment() {
  count.value++
  await nextTick()
  // DOM is now updated
}
```

## `reactive()`

```ts
import { reactive } from 'vue'

const state = reactive({ count: 0 })
```

```html
<button @click="state.count++">
  {{ state.count }}
</button>
```

✅ `reactive()` makes the object itself reactive <br/>
✅ Vue intercepts all property access and mutations <br/>
✅ It converts nested objects deeply into proxies <br/>
✅ Used internally when a ref holds an object <br/>

🔗 [Proxy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)

### Reactive Proxy vs. Original

✅ `reactive()` returns a **proxy**, not the original object <br/>
⚠️ Mutating the original object won’t trigger updates <br/>
✅ Nested objects are also reactive proxies <br/>

```ts
const raw = {}
const proxy = reactive(raw)

// proxy is NOT equal to the original.
console.log(proxy === raw) // false

// calling reactive() on the same object returns the same proxy
console.log(reactive(raw) === proxy) // true

// calling reactive() on a proxy returns itself
console.log(reactive(proxy) === proxy) // true
```

### Limitations of `reactive()`

⚠️ Works only with objects, arrays, or collections (`Set`, `Map`) — not primitives <br/>
⚠️ You can’t replace the entire object — must keep the same reference <br/>
⚠️ Destructuring reactive objects breaks reactivity <br/>
✨ `ref()` is recommended for most reactive state declarations <br/>

## Additional Ref Unwrapping Details

### As Reactive Object Property

✅ A ref is unwrapped when accessed or mutated as a reactive property <br/>
⚠️ Assigning a new ref replaces the previous one <br/>

```ts
const count = ref(0)
const state = reactive({ count })

console.log(state.count) // 0
state.count = 1
console.log(count.value) // 1

const otherCount = ref(2)
state.count = otherCount
console.log(state.count) // 2
console.log(count.value) // 1 (disconnected)
```

### Caveat in Arrays and Collections

⚠️ Elements in arrays or collections are **not unwrapped** <br/>

```ts
const books = reactive([ref('Vue 3 Guide')])
// need .value here
console.log(books[0].value)

const map = reactive(new Map([['count', ref(0)]]))
// need .value here
console.log(map.get('count').value)
```

### Caveat When Unwrapping in Templates

⚠️ Template ref unwrapping only works for **top-level** refs <br/>

```ts
const count = ref(0)
const object = { id: ref(1) }
```

```html
{{ count + 1 }} ✅
{{ object.id + 1 }} ❌
```

⚠️ The result will be `[object Object]1` because `object.id` isn’t unwrapped inside the expression.

---

🔗 [Reactivity Fundamentals](https://vuejs.org/guide/essentials/reactivity-fundamentals.html)
