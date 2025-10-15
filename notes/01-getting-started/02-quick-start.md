# Quick Start

## Creating a Vue Application

```bash
npm create vue@latest
```

✅ Creates a Vue [**Single Page Application**](https://vuejs.org/guide/extras/ways-of-using-vue#single-page-application-spa) <br/>
ℹ️ Uses [**Vite**](https://vitejs.dev/) as the build setup <br/>
✅ Supports Vue [**Single-File Components**](https://vuejs.org/guide/scaling-up/sfc) (SFCs) <br/>
✅ Uses the [Composition API](https://vuejs.org/guide/introduction#composition-api) with `<script setup>` <br/>

## Using Vue from CDN

```html
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
```

### Using the ES Module Build + Import Maps

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

✅ No build step required <br/>
❌ Single-File Components (SFCs) not supported <br/>
⚠️ `<script setup>` syntax needs build tools <br/>

---

🔗 [Quick Start](https://vuejs.org/guide/quick-start.html)
