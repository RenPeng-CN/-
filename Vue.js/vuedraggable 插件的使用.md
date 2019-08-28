

### Vue.Draggable学习总结

##### Draggable为基于Sortable.js的vue组件，用以实现拖拽功能

#### 特性

- 支持触摸设备
- 支持拖拽和选择文本
- 支持智能滚动
- 支持不同列表之间的拖拽
- 不以jQuery为基础
- 和视图模型同步刷新
- 和vue2的国度动画兼容
- 支持撤销操作
- 当需要完全控制时，可以抛出所有变化
- 可以和现有的UI组件兼容



github上的解释 [地址](https://github.com/SortableJS/Vue.Draggable#tag)  

Vue-draggable 所有功能示例 [地址](https://sortablejs.github.io/Vue.Draggable/#/custom-clone)



#### 安装

```js
npm i -S vuedraggable  // 通过 npm 安装
yarn add vuedraggable  // 通过 yarn 安装
```



#### 引入

```js
import draggable from 'vuedraggable'
```



#### 基础用法

定义一个json串 `list`，实现它的拖拽排序。

```vue
<template>
  <div>
    <!-- 调用组件  -->
    <draggable element="ul" v-model="list">
      <li v-for="item in list">{{item.name}}</li>
    </draggable>
    <!-- 输出list数据 -->
    {{list}}
  </div>
</template>

<script>
// 引入拖拽组件
import draggable from 'vuedraggable'
export default {
  name: 'demo',
  components: {
    //调用组件
    draggable,
  },
  data () {
    return {
      list:[
        {id: 1, name: 'a'},
        {id: 2, name: 'b'},
        {id: 3, name: 'c'},
        {id: 4, name: 'd'},
        {id: 5, name: 'e'},
        {id: 6, name: 'f'},
      ]
    }
  },
}
</script>

```



####另外一个典型使用方法

```vue
<template>
  <div class="ToDoList">
    <!-- 调用组件  -->
    <draggable v-model="myArray" group="people" @start="drag=true" @end="drag=false">
      <div v-for="element in myArray" :key="element.id">{{ element.id }}{{ element.name }}</div>
    </draggable>
    <!-- 输出list数据 -->
    {{list}}
  </div>
</template>
<script>
// 引入 拖拽组件
import draggable from 'vuedraggable'

export default {
  components: {
    // 声明组件
    draggable
  },
  data() {
    return {
      myArray: [
        { id: 1, name: '张三' },
        { id: 2, name: '李四' },
        { id: 3, name: '王五' }
      ]
    }
  }
}
</script>
<style lang="scss" scoped>
.ToDoList {
  margin-top:50px;
  padding-top: 50px;
  font-size: 30px;
  width:300px;
  height:300px;
  text-align: center;
  border: red 1px solid;
}
</style>
```



####加 过渡效果

```vue
<draggable v-model="myArray">
    <transition-group>
        <div v-for="element in myArray" :key="element.id">
            {{element.name}}
        </div>
    </transition-group>
</draggable>
```



#### 属性

### value

Array, 非必须，默认为null

- 用于实现拖拽的list，通常和内部v-for循环的数组为同一数组。
- 最好使用vuex来实现传入。
- 不是直接使用，而是通过`v-model`引入。

```vue
<draggable v-model="myArray">
```



#### list  （列表）

Array，非必须，默认为null

- 就是`value`的替代品。
- 和`v-model`不能共用
- 从表现上没有看出不同

除了值prop之外，list是一个要与拖放同步的数组。
主要的区别是list prop是由使用splice方法的draggable组件更新的，而value是不可变的。

不要与value prop一起使用。

可排序选项可以直接设置为vue。自2.19版以来可拖动的道具。

这意味着所有sortable选项都是有效的sortable道具，但值得注意的是，所有以“on”开头的方法除外，因为draggable组件通过事件公开相同的API。

kebabb -case propery是受支持的:例如，幽灵类的道具将被转换为ghostClass sortable选项。

示例设置句柄、可排序和组选项:

```vue
<draggable
        v-model="list"
        handle=".handle"
        :group="{ name: 'people', pull: 'clone', put: false }"
        ghost-class="ghost"
        :sort="false"
        @change="log"
      >
      <!-- -->
</draggable>
```





#### element

String，默认div

- 就是`<draggable>`标签在渲染后展现出来的标签类型
- 也是包含拖动列表和插槽的外部标签
- 可以用来兼容UI组件



#### options

Object

- 配置项

- group: string or array 分组用的，同一组的不同list可以相互拖动

- sort: boolean 定义在本组内，是否可以拖放排序 ， false 为在本组内不可以拖放排序

- delay:number  同组内，拖放第一个单元后，间隔多久才可以拖放第二个单元

- touchStartThreshold:number (不清楚)

- disabled: boolean 定义是否此sortable对象是否可用，为true时sortable对象不能拖放排序等功能

- store:  //@see Store

- animation: number 单位:ms 动画时间

- handle: selector 格式为简单css选择器的字符串，handle: '.my-handle', 使列表单元中符合选择器的元素成为拖动的手柄，只有按住拖动手柄才能使列表单元进行拖动

- filter: selector 格式为简单css选择器的字符串，定义哪些列表单元不能进行拖放，可设置为多个选择器，中间用“，”分隔

- preventOnFilter: true, //  当拖动`filter`时是否触发`event.preventDefault()`默认触发

- draggable: selector 格式为简单css选择器的字符串，定义哪些列表单元可以进行拖放

- ghostClass: selector 格式为简单css选择器的字符串，当拖动列表单元时会生成一个副本作为影子单元来模拟被拖动单元排序的情况，此配置项就是来给这个影子单元添加一个class，我们可以通过这种方式来给影子元素进行编辑样式

- chosenClass: selector 格式为简单css选择器的字符串，目标被选中时添加

- dragClass:selector 格式为简单css选择器的字符串，目标拖动过程中添加

- forceFallback: boolean 如果设置为true时，将不使用原生的html5的拖放，可以修改一些拖放中元素的样式等

- fallbackClass： string 当forceFallback设置为true时，拖放过程中鼠标附着单元的样式

- dataIdAttr： `data-id` (试验后，不知道有什么用)

- scroll：boolean当排序的容器是个可滚动的区域，拖放可以引起区域滚动

- scrollFn：function(offsetX, offsetY, originalEvent, touchEvt, hoverTargetEl) { … } 用于自定义滚动条的适配

- scrollSensitivity: number 就是鼠标靠近边缘多远开始滚动.默认30

- scrollSpeed: number 滚动速度




函数配置

- setData: 设置值时的回调函数
- onChoose: 选择单元时的回调函数
- onStart: 开始拖动时的回调函数
- onEnd: 拖动结束时的回调函数
- onAdd: 添加单元时的回调函数
- onUpdate: 排序发生变化时的回调函数
- onRemove: 单元被移动到另一个列表时的回调函数
- onFilter: 尝试选择一个被filter过滤的单元的回调函数
- onMove: 移动单元时的回调函数
- onClone: clone时的回调函数
- 以上函数对象的属性
  - to: 移动到的列表的容器
  - from：来源列表容器
  - item: 被移动的单元
  - clone: 副本的单元
  - oldIndex：移动前的序号
  - newIndex：移动后的序号

```js
 animation: 150, // 单位:ms 毫秒，排序时移动物品的动画速度，`0`则表示无动画，（试验后，看不出差别）
        group: { // 多个数组 组成了一个群，群里可互相拖放 { name: "...", pull: [true, false, clone], put: [true, false, array] }
          name: 'components', // 群组的 名称
          pull: 'clone', // 在群里 使用 克隆的方法 进行 拖
          put: false // 好像 改成 true, 效果也一样，有待确认
        },
        disabled: false, // 定义是否可以 拖放，为true时，就不能拖放了
        ghostClass: 'ghost', // 当拖放列表单元时，在本组内的原位置上，会生成一个副本作为影子单元来模拟被拖动单元排序的位置，设置该影子的样式
        sort: false, // 定义在 本组内，是否可拖放排序， false 为停止本组拖放排序
        touchStartThreshold: 0, // 像素，在多少像素移动范围内课取消延迟拖动事件 (试验不出差别来)
        store: null, // 还不清楚做什么用
        preventOnFilter: true, // 触发`filter`时调用`event.preventDefault()`。
        chosenClass: 'sortable-chosen', // 选中要拖放的单元时，该单元会变成 你自己设定的样式
        dragClass: 'sortable-drag', // 拖放单元时，该单元会变成 你自己设定的样式
        // forceFallback: false, // 忽略HTML5 DnD行为并强制回退使用,如果设置为true时，将不使用原生的html5的拖放，可以修改一些拖放中元素的样式等
        // fallbackClass: 'sortable-fallback', // 使用forceFallback时的克隆DOM元素的类名。
        // fallbackOnBody: false, // 使用forceFallback时,将克隆的DOM元素追加到Document中Body 。
        // fallbackTolerance: 0, // 以像素为单位指定鼠标在被视为拖动之前应移动多远
        // scroll: true, // 当排序的容器是个可滚动的区域，拖放可以引起区域滚动
        // scrollFn: function(offsetX, offsetY, originalEvent, touchEvt, hoverTargetEl) { ... }, // 如果你有自定义滚动条scrollFn可用于自动滚动
        // scrollSensitivity: 30, // 鼠标必须靠近边缘多少px才能开始滚动。
        // scrollSpeed: 10, // 滚动速度。px
        // dataIdAttr: 'data-id', // (试验后，不知道有什么用)
        // draggable: '.item', // 指定元素中的哪些项应该是可拖动的。（还不确定怎样使用，设置后，不能拖放了）
        // filter: '.ignore-elements', // 选择不支持拖动的选择器（String或Function）(试验后 无效)
        // handle: '.my-handle', // 列表项中拖动手柄选择，可以设置列表中item中的某个DOM节点为拖动的依据(试验，设置后，不可拖动单元了)
        delay: 0, // 同组内，拖放第一个单元后，间隔多久才可以拖放第二个单元

        // 以下为 函数配置
        // setData 是 设置值时的回调函数
        setData: function(/** DataTransfer */dataTransfer, /** HTMLElement*/dragEl) {
          console.log('setData')
          dataTransfer.setData('Text', dragEl.textContent) // `dataTransfer` object of HTML5 DragEvent
        },
        // onChoose 是 选择单元时的回调函数
        onChoose: function(/** Event*/evt) {
          console.log('onchoose')
          evt.oldIndex // element index within parent
        },
        // onStart 开始拖动时的回调函数
        onStart: function(/** Event*/evt) {
          evt.oldIndex // element index within parent
        },
        // onEnd拖动结束时的回调函数
        onEnd: function(/** Event*/evt) {
          var itemEl = evt.item // dragged HTMLElement
          evt.to // target list
          evt.from // previous list
          evt.oldIndex // element's old index within old parent
          evt.newIndex // element's new index within new parent
        },
        // 添加单元时的回调函数
        onAdd: function(/** Event*/evt) {
        // same properties as onEnd
        },

        // 排序发生变化时的回调函数
        onUpdate: function(/** Event*/evt) {
        // same properties as onEnd
        },

        // Called by any change to the list (add / update / remove)
        onSort: function(/** Event*/evt) {
        // same properties as onEnd
        },

        //  单元被移动到另一个列表时的回调函数
        onRemove: function(/** Event*/evt) {
        // same properties as onEnd
        },

        //  尝试选择一个被filter过滤的单元的回调函数
        onFilter: function(/** Event*/evt) {
          var itemEl = evt.item // HTMLElement receiving the `mousedown|tapstart` event.
        },
        // Event when you move an item in the list or between lists 移动单元时的回调函数
        onMove: function(/** Event*/evt, /** Event*/originalEvent) {
        // Example: http://jsbin.com/tuyafe/1/edit?js,output
          evt.dragged // dragged HTMLElement
          evt.draggedRect // TextRectangle {left, top, right и bottom}
          evt.related // HTMLElement on which have guided
          evt.relatedRect // TextRectangle
          originalEvent.clientY // mouse position
        // return false; — for cancel
        },

        // Called when creating a clone of element clone时的回调函数
        onClone: function(/** Event*/evt) {
          var origEl = evt.item
          var cloneEl = evt.clone
        }
        // 以上函数对象的属性
        // - to: 移动到的列表的容器
        // - from：来源列表容器
        // - item: 被移动的单元
        // - clone: 副本的单元
        // - oldIndex：移动前的序号
        // - newIndex：移动后的序号
```





### :clone

Type：Function  类型：方法

Required：false    是否必须 : 否

Default : （original) => { return original; }

- 这一项要配合着`options`的`group`项的`pull`项处理，当`pull:'clone`时的拖拽的回调函数’
- 就是克隆的意思。
- 可以理解为正常的拖拽变成了复制。
- 当为`true`时克隆



### move

Type: Function   类型：方法

Required: false   是否必须 ： 否

Default: null  默认：空

- 就是拖拽项时调用的函数

- 用来确定拖拽是否生效

- 返回null时可以生效

- 可以通过函数判断

- 有一个参数:`evt` 

  - `evt`为object
  - draggedContext: 被拖拽元素的上下文
    - index:拖拽元素的指针
    - element: 拖拽数据本身
    - futureIndex: 拖动后的index
  - relatedContext: 拖入区域的上下文 
    - index: 目标元素的index
    - element:目标数据本身
    - list: 拖入的列表
    - component:目标组件

  

  ```.vue
  <draggable element="ul" v-model="list" :move='allow'>
  ...
  methods: {
    allow(evt) {
      console.log(evt.draggedContext.index)
      console.log(evt.draggedContext.element)
      console.log(evt.draggedContext.futureIndex)
      console.log(evt.relatedContext.index)
      console.log(evt.relatedContext.element)
      console.log(evt.relatedContext.list)
      console.log(evt.relatedContext.component)
      return (evt.draggedContext.element.name!== 'b')
    }
  }
  ```



#### componentData

Type：Object  类型:对象

Required: false  是否必须：否

Default：null  默认 ：空

- 用来结合UI组件的，可以理解为代理了UI组件的定制信息
- 包含两项: props 和 on
  - `props`用来代理UI组件需要绑定的属性（:）
  - `on`用来代理UI组件需要绑定的事件（@）



```vue
<draggable element="el-collapse" :list="list" :component-data="getComponentData()">
  <el-collapse-item v-for="e in list" :title="e.title" :name="e.name" :key="e.name">
    <div>{{e.description}}</div>
   </el-collapse-item>
</draggable>

methods: {
  handleChange() {
    console.log('changed');
  },
  inputChanged(value) {
    this.activeNames = value;
  },
  getComponentData() {
    return {
      on: {
        change: this.handleChange,
        input: this.inputChanged
      },
      props: {
        value: this.activeNames
      }
    };
  }
}
```



#### events 事件

有以下几种

每当Sortable.js用相同的参数触发onStart、onAdd、onRemove、onUpdate、onEnd、onselect、onunselect、onSort、onClone时，都会调用事件。

```js
start, add, remove, update, end, choose, sort, filter, clone
```

change event  改变事件 

当 list props 不为空且相应数组由于拖放操作而更改时，将触发change事件。
调用此事件时，一个参数包含以下属性之一:

参数带有如下属性：

​      add: 包含被添加到列表的元素 

​                 newIndex: 添加后的新索引

​                 element: 被添加的元素

​       removed: 从列表中移除的元素

​                  oldIndex: 移除前的索引

​                   element: 被移除的元素

​      moved: 内部移动的 

​                    newIndex: 改变后的索引

​                    oldIndex: 改变前的索引

​                    element: 被移动的元素

```vue
<draggable :list="list" @end="onEnd">
```







#### slot  插槽

提供一个footer插槽，在排序列表之下

永远位于最下方

```vue
<draggable v-model="myArray" :options="{draggable:'.item'}">
    <div v-for="element in myArray" :key="element.id" class="item">
        {{element.name}}
    </div>
    <button slot="footer" @click="addPeople">Add</button>
</draggable>

```



 提供一个header插槽，在排序列表之上

```vue
<draggable v-model="myArray" draggable=".item'">
    <div v-for="element in myArray" :key="element.id" class="item">
        {{element.name}}
    </div>
    <button slot="header" @click="addPeople">Add</button>
</draggable>
```



#### 用于 Vuex

```vue
<draggable v-model='myList'>
  
  
  computed: {
    myList: {
        get() {
            return this.$store.state.myList
        },
        set(value) {
            this.$store.commit('updateList', value)
        }
    }
}
```



#### Props  （支柱、道具、支撑、建造）

Value   

Type：Array    类型：数组

Required: false   是否必须: 否

Default: null    默认: 空

输入数组到可拖动组件。通常与内部元素v-for指令引用的数组相同。

这是使用Vue的首选方法。可拖动，因为它与Vuex兼容。

#####它不应该直接使用，但只能通过v-model指令:

```vue
<draggable v-model="myArray">
```





#### tag   （标签 、标记、关键字）

Type: `String`  类型：字符串 
Default: `'div' `    默认: 'div'

可拖动组件创建的元素的HTML节点类型，作为包含插槽的外部元素。还可以将vue组件的名称作为元素传递。在本例中，draggable属性将传递给create组件。
如果需要将道具或事件设置为创建的组件，请参见componentData。



























