# Computed Properties

## Basic Example

```html
<script setup>
import { reactive, computed } from 'vue'

const author = reactive({
  name: 'John Doe',
  books: [
    'Vue 2 - Advanced Guide',
    'Vue 3 - Basic Guide',
    'Vue 4 - The Mystery'
  ]
})

// a computed ref
const publishedBooksMessage = computed(() => {
  return author.books.length > 0 ? 'Yes' : 'No'
})
</script>

<template>
  <p>Has published books:</p>
  <span>{{ publishedBooksMessage }}</span>
</template>
```

✅ Use for complex logic involving reactive data <br/>
✅ Computed properties track their reactive dependencies <br/>
✅ Expect a getter function and return a computed ref <br/>
✅ Auto-unwrapped in templates <br/>
✅ Cached based on reactive dependencies <br/>

## Computed Caching vs. Methods

```html
<p>{{ calculateBooksMessage() }}</p>
```

```ts
// in component
function calculateBooksMessage() {
  return author.books.length > 0 ? 'Yes' : 'No'
}
```

✅ Computed properties re-evaluate **only** when their dependencies change <br/>
⚙️ Use methods instead when you don’t need caching <br/>

```ts
// This will never update, because Date.now() is not reactive
const now = computed(() => Date.now())
```

## Writable Computed

✅ Computed properties are read-only by default <br/>
⚙️ You can make them writable by adding a getter and setter <br/>

```ts
import { ref, computed } from 'vue'

const firstName = ref('John')
const lastName = ref('Doe')

const fullName = computed({
  get() {
    return firstName.value + ' ' + lastName.value
  },
  set(newValue) {
    [firstName.value, lastName.value] = newValue.split(' ')
  }
})
```

## Best Practices

### Getters Should Be Side-Effect Free

ℹ️ A computed getter should perform **pure computations only** <br/>
❌ Don’t mutate state <br/>
❌ Don’t make async requests <br/>
❌ Don’t manipulate the DOM <br/>
✨ Its only role is to **compute and return** a value <br/>

### Avoid Mutating Computed Values

ℹ️ The returned value from a computed property is **derived state** <br/>
⚠️ It should be treated as **read-only** and never mutated <br/>

---

🔗 [Computed Properties Guide](https://vuejs.org/guide/essentials/computed.html)
