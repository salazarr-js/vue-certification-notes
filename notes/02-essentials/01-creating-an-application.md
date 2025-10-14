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

✅ `createApp` receive an component <br/>
✅ Every app requires a root component, that will contain other components as its children <br/>
✅ An application instance won't render anything until its `.mount()` method is called <br/>
✅ The container can be an actual DOM element or a selector string like `#app`<br/>
✅ The content of the app's root component will be rendered inside the container element<br/>

## App Configurations

✅ The application instance exposes a `.config` object that allows us to configure a few app-level options <br/>

```ts
/* Defining an app-level error handler */ 
app.config.errorHandler = (err) => {
  /* handle error */
}

/** Registering app-scoped assets like components */
app.component('TodoDeleteButton', TodoDeleteButton)
```

## Multiple application instances

✅ The createApp API allows multiple Vue applications to co-exist on the same page, each with its own scope for configuration and global assets

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

> [Creating a vue Application](https://vuejs.org/guide/quick-start.html#creating-a-vue-application)
