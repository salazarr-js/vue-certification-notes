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
✅ ref wraps the inner value in a special object <br/>

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

```ts
import { reactive } from 'vue'

const state = reactive({ count: 0 })
```

```html
<button @click="state.count++">
  {{ state.count }}
</button>
```

✅ Unlike refs, `reactive()` makes reactive the object itself <br/>
✅ Vue intercept the access and mutation of all properties of a reactive object <br/>
✅ Reactive objects are JavaScript Proxies, Vue intercepts <br/>
✅ converts the object deeply: nested objects are also wrapped with `reactive()` when accessed. <br/>
✅ reactive is used internally when ref value is an object<br/>

> [Proxy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)

### Reactive Proxy vs. Original

✅ reactive() value is a proxy of the original object, proxied value and original are diferent <br/>
✅ mutating the original object wont trigger updates <br/>
✅ Nested objects inside a reactive object are also proxies <br/>

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

✅ limited value types to objects arrays, collections like `Set` and `Map`, doesn't accept primitives <br/>
✅ cant replace entire object, need to mantain the same reference to the reactive object, or will lost reactivity connection <br/>
✅ cant destructure the object, will lose reactivity connection<br/>
✅ ref is recommended as primary api for declaring reactive state<br/>

## Additional Ref Unwrapping Details

### As Reactive Object Property

✅ A ref is automatically unwrapped when accessed or mutated as a property of a reactive object <br/>
✅ If a new ref is assigned to a property linked to an existing ref, it will replace the old ref <br/>

```ts
const count = ref(0)
const state = reactive({
  count
})

console.log(state.count) // 0

state.count = 1
console.log(count.value) // 1

const otherCount = ref(2)

state.count = otherCount
console.log(state.count) // 2
// original ref is now disconnected from state.count
console.log(count.value) // 1
```

### Caveat in Arrays and Collections

✅ Array or collection elements aren't unwrapped <br/>


```ts
const books = reactive([ref('Vue 3 Guide')])
// need .value here
console.log(books[0].value)

const map = reactive(new Map([['count', ref(0)]]))
// need .value here
console.log(map.get('count').value)
```

### Caveat when Unwrapping in Templates

✅ Ref unwrapping in templates only applies if the ref is a top-level property in the template render context. <br/>


```ts
const count = ref(0)
const object = { id: ref(1) }
```

```html
{{ count + 1 }} ✅
{{ object.id + 1 }} ❌
```
> The rendered result will be [object Object]1 because object.id is not unwrapped when evaluating the expression and remains a ref object

---

> [Reactivity Fundamentals](https://vuejs.org/guide/essentials/reactivity-fundamentals.html)
