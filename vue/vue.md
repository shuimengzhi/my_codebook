# v-once
```
<span v-once>这个将不会改变: {{ msg }}</span>
```
只插入一次msg

# v-bind
```
<button v-bind:disabled="isButtonDisabled">Button</button>

<div v-bind:class="{ active: isActive }"></div>
```
通常绑定class，如果`isButtonDisabled`的值为真，则`disabled`渲染出来。

isActive为真则
```
<div class="active"></div>
```
`v-bind`缩写`:`

# v-on
`v-on` 缩写`@`
```
<button v-on:click="warn('Form cannot be submitted yet.', $event)">
  Submit
</button>
```
v-on:click之后是方法名

# computed 大量计算用计算属性而不用方法

```
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  }
})
```
`computed` vs `methods`
computed有缓存，性能更优。是根据响应式依赖发生改变时才触发computed方法，否则都是读之前的计算结果（缓存）

# 侦听器
当需要在数据变化时执行异步或开销较大的操作时使用
```
watch:{},
```
watch与methods、computed一样用

# v-if
```
<h1 v-if="awesome">Vue is awesome!</h1>
<h1 v-else>Oh no 😢</h1>
```
如果awesome为真则执行第一行，否则执行第二行。

# v-else-if
```
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>
```

# v-if vs v-show
v-if遇到条件才渲染出来，v-show一次性加载好，根据情况显示出来

一般来说，v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件很少改变，则使用 v-if 较好。

# v-for
```
<ul id="example-1">
  <li v-for="item in items" :key="item.message">
    {{ item.message }}
  </li>
</ul>
var example1 = new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
```

# v-model
输入绑定
```
<input v-model="message" placeholder="edit me">
<p>Message is: {{ message }}</p>
```
