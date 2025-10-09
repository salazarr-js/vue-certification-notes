# 🚀 Bootstrap a Vue App

## Vue SPA's

```bash
npm create vue@latest
``` 

> ✅ Scaffold a Vue [**Single Page Application**](https://vuejs.org/guide/extras/ways-of-using-vue#single-page-application-spa) <br/>
> ✅ Build setup based on [**Vite**](https://vitejs.dev/) <br/>
> ✅ Allow to use Vue [**Single-File Components**](https://vuejs.org/guide/scaling-up/sfc) (SFCs) <br/>
> ✅ generated project are written using the [Composition API ](https://vuejs.org/guide/introduction#composition-api) `<script setup>`



## Using Vue from CDN

```html
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
```

### Using the ES Module Build + Import maps
```html
<script type="importmap">
  {
    "imports": {
      "vue": "https://unpkg.com/vue@3/dist/vue.esm-browser.js"
    }
  }
</script>

<div id="app">{{ message }}</div>

<script type="module">
  import { createApp, ref } from 'vue'

  createApp({
    setup() {
      const message = ref('Hello Vue!')
      return {
        message
      }
    }
  }).mount('#app')
</script>
```

> ✅ no "build step" involved <br/> 
> ❌ won't be able to use the Single-File Component (SFC) syntax <br/> 
> ⚠️ `<script setup>` syntax requires build tools


## The Application Instance

```html
<div id="app"></div>
```

```ts
import { createApp } from 'vue'

const app = createApp(/* ROOT COMPONENT */)
app.mount('#app')
```

--- 

- [Creating a vue Application](https://vuejs.org/guide/quick-start.html#creating-a-vue-application)
- [Using Vue from CDN](https://vuejs.org/guide/quick-start.html#using-vue-from-cdn)
- [The Application Instance](https://vuejs.org/guide/essentials/application.html#the-application-instance)