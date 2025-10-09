# 🚀 Bootstrap a Vue App

## Vue SPAs

```bash
npm create vue@latest
```

> ✅ Scaffolds a Vue [**Single Page Application**](https://vuejs.org/guide/extras/ways-of-using-vue#single-page-application-spa) <br/>
> ✅ Build setup based on [**Vite**](https://vitejs.dev/) <br/>
> ✅ Allows using Vue [**Single-File Components**](https://vuejs.org/guide/scaling-up/sfc) (SFCs) <br/>
> ✅ The generated project uses the [Composition API](https://vuejs.org/guide/introduction#composition-api) via `<script setup>` <br/>

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

> ✅ No build step involved <br/>
> ❌ Can't use Single-File Component (SFC) syntax <br/>
> ⚠️ The `<script setup>` syntax requires build tools. <br/>


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