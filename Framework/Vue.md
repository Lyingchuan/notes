#### 计算属性
```
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>
```
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
计算属性适用于属性关系对应的情况,不能改变其它不相关属性.

侦听器watch则可以改变任何无逻辑相关的属性,并且可以异步操作.

##### v-else v-else-if
用于在v-if为false的时候显示

#### 数组
由于 JavaScript 的限制，Vue 不能检测以下变动的数组：

当你利用索引直接设置一个项时，例如：vm.items[indexOfItem] = newValue

当你修改数组的长度时，例如：vm.items.length = newLength

此时应该用Vue.set(object, key, value)方法;vm.$set()和Vue.set()相同

#### $event
在@click中,可以用特殊变量 $event 把它传入方法
```
<button v-on:click="warn('Form cannot be submitted yet.', $event)">
  Submit
</button>
```
```
methods: {
  warn: function (message, event) {
    // 现在我们可以访问原生事件对象
    if (event) event.preventDefault()
    alert(message)
  }
}
```

#### 事件修饰符
```
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即元素自身触发的事件先在此处处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>

2.14新增
<!-- 点击事件将只会触发一次 -->
<a v-on:click.once="doThis"></a>
```
使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。因此，用 v-on:click.prevent.self 会阻止所有的点击，而 v-on:click.self.prevent 只会阻止对元素自身的点击。

#### .number
```
<input v-model.number="age" type="number">
```
以上可以将VUE表单绑定成数字形式(默认字符串)

### 子组件向父组件传递事件
```
//子组件
<button v-on:click="$emit('enlarge-text')">
  Enlarge text
</button>
```
```
//父组件
<blog-post
  ...
  v-on:enlarge-text="postFontSize += 0.1"
></blog-post>
```

#### 使用事件抛出一个值
```
<button v-on:click="$emit('enlarge-text', 0.1)">
  Enlarge text
</button>
```
父组件通过$event访问该值
```
<blog-post
  ...
  v-on:enlarge-text="postFontSize += $event"
></blog-post>
```
如果事件是一个函数,则传递的值默认为第一个参数
```
<blog-post
  ...
  v-on:enlarge-text="onEnlargeText"
></blog-post>
```
```
methods: {
  onEnlargeText: function (enlargeAmount) {
    this.postFontSize += enlargeAmount
  }
}
```

#### DOM解析问题
```
<table>
  <blog-post-row></blog-post-row>
</table>
```
因为在HTML中,table标签不能包含自定义组件,因此以上会渲染错误,要这样
```
<table>
  <tr is="blog-post-row"></tr>
</table>
```

