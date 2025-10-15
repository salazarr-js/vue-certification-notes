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

✅ for complex logic that includes reactive data <br/>
✅ computed properties tracks its reactive dependencies <br/>
✅ expects to be passed a getter function, and the returned value is a computed ref. <br/>
✅ are also auto-unwrapped in templates <br/>
✅ computed properties are cached based on their reactive dependencies <br/>


## Computed Caching vs Methods

```html
<p>{{ calculateBooksMessage() }}</p>
```

```ts
// in component
function calculateBooksMessage() {
  return author.books.length > 0 ? 'Yes' : 'No'
}
```

✅ A computed property will only re-evaluate when some of its reactive dependencies have changed. <br/>
✅ In cases where you do not want caching, use a method call instead <br/>


```ts
//  This will never update, because Date.now() is not a reactive dependency
const now = computed(() => Date.now())
```

## Writable Computed

✅ Computed properties are by default getter-only <br/>
✅ you can create a writable one by providing both a getter and a setter <br/>


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

### Getters should be side-effect free

ℹ️ computer getter should only performs pure computations <br />
❌ dont mutate other state <br />
❌ dont make async request <br />
❌ dont mutate the DOM <br />
✅ its only responsibility should be computing and returning that value <br />

### Avoid mutating computed value

ℹ️ The returned value from a computed property is derived state. <br />
⚠️ a computed return value should be treated as read-only and never be mutated <br />
