# Creating a Vue Application

## The Application Instance

```html
<div id="app"></div>
```

```ts
import { createApp } from 'vue'
import App from './App.vue' // Root component

const app = createApp(App)

app.mount('#app')
```

✅ `createApp` receives a component <br/>
✅ Every app needs a root component that contains all child components <br/>
✅ The app won’t render anything until `.mount()` is called <br/>
✅ The container can be a DOM element or a selector string like `#app` <br/>
✅ The root component’s content is rendered inside the container <br/>

## App Configurations

ℹ️ The app instance exposes a `.config` object to define app-level options <br/>

```ts
/* Define a global error handler */ 
app.config.errorHandler = (err) => {
  /* handle error */
}

/* Register global components */
app.component('TodoDeleteButton', TodoDeleteButton)
```

## Multiple Application Instances

✅ The `createApp` API allows multiple Vue apps on the same page, each with its own configuration and global scope <br/>

```ts
const app1 = createApp({
  /* ... */
})
app1.mount('#container-1')

const app2 = createApp({
  /* ... */
})
app2.mount('#container-2')
```

---

🔗 [Creating a Vue Application](https://vuejs.org/guide/quick-start.html#creating-a-vue-application)
