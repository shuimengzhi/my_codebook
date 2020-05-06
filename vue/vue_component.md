# 组件基础
```
// 定义一个名为 button-counter 的新组件
Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
})
```
```
<div id="components-demo">
  <button-counter></button-counter>
</div>
```
```
new Vue({ el: '#components-demo' })
```

# 组件的data必须是一个函数
正确的写法
```
data: function () {
  return {
    count: 0
  }
}
```
错误的写法
```
data: {
  count: 0
}
```

# 全局注册组件
```
Vue.component('my-component-name', {
  // ... options ...
})
```

# 组件自定义参数props

```
Vue.component('blog-post', {
  props: ['title'],
  template: '<h3>{{ title }}</h3>'
})
```

```
<blog-post title="My journey with Vue"></blog-post>
<blog-post title="Blogging with Vue"></blog-post>
<blog-post title="Why Vue is so fun"></blog-post>
```

# 组件上的v-model

```
<input v-model="searchText">
```
=>
```
<input
  v-bind:value="searchText"
  v-on:input="searchText = $event.target.value"
>
```