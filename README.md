<p align="center"><img width="140"src="https://raw.githubusercontent.com/SortableJS/Vue.Draggable/master/logo.png"></p>
<h1 align="center">Vue.Draggable</h1>

[![GitHub open issues](https://img.shields.io/github/issues/SortableJS/Vue.Draggable.svg?maxAge=2592000)](https://github.com/SortableJS/Vue.Draggable/issues?q=is%3Aopen+is%3Aissue)
[![npm download](https://img.shields.io/npm/dt/vuedraggable.svg?maxAge=30)](https://www.npmjs.com/package/vuedraggable)
[![npm download per month](https://img.shields.io/npm/dm/vuedraggable.svg)](https://www.npmjs.com/package/vuedraggable)
[![npm version](https://img.shields.io/npm/v/vuedraggable.svg?maxAge=30)](https://www.npmjs.com/package/vuedraggable)
[![Package Quality](http://npm.packagequality.com/shield/vuedragablefor.svg)](http://packagequality.com/#?package=vuedraggable)
[![vue2](https://img.shields.io/badge/vue-2.x-brightgreen.svg)](https://vuejs.org/)
[![MIT License](https://img.shields.io/github/license/SortableJS/Vue.Draggable.svg)](https://github.com/SortableJS/Vue.Draggable/blob/master/LICENSE)

允许拖放时同步视图模型数组的Vue组件(Vue.js 2.0)或指令(Vue.js 1.0)。

基于并提供[Sortable.js](https://github.com/RubaXa/Sortable)的所有功能

## 示例

![demo gif](https://raw.githubusercontent.com/SortableJS/Vue.Draggable/master/example.gif)

## 现场演示

https://david-desmaisons.github.io/draggable-example/

## 特性

* 完全支持[Sortable.js](https://github.com/RubaXa/Sortable)的所有特性：
    * 支持触摸设备
    * 支持拖动手柄和可选文本
    * 智能自动滚动
    * 支持不同列表之间拖放
    * 不依赖jQuery
* 保持同步HTML和视图模型列表
* 兼容Vue.js2.0的transition-group
* 取消支持
* 在需要完全控制时报告任何更改的事件
* 重用现有的UI组件库 (例如：[vuetify](https://vuetifyjs.com), [element](http://element.eleme.io/)，或者[vue material](https://vuematerial.io) 等) 并给他们设置props属性，属性值为`element` 和 `componentData`，使他们可以拖动

## 支持者

 <a href="https://flatlogic.com/admin-dashboards">
 <img width="190" style="margin-top: 10px;" src="https://flatlogic.com/assets/logo-d9e7751df5fddd11c911945a75b56bf72bcfe809a7f6dca0e32d7b407eacedae.svg">
 </a>

使用Vue，React和Angular制作的管理仪表板的模板。

## 安装

### 使用 npm 安装或者 yarn 安装

```bash
yarn add vuedraggable

npm i -S vuedraggable
```

**请注意在Vue2.0中使用的是vuedraggable，而不是在Vue1.0中使用的vue-draggable**

### 使用 <script> 全局引用
```html

<script src="//cdnjs.cloudflare.com/ajax/libs/vue/2.5.2/vue.min.js"></script>
<!-- CDNJS :: Sortable (https://cdnjs.com/) -->
<script src="//cdn.jsdelivr.net/npm/sortablejs@1.7.0/Sortable.min.js"></script>
<!-- CDNJS :: Vue.Draggable (https://cdnjs.com/) -->
<script src="//cdnjs.cloudflare.com/ajax/libs/Vue.Draggable/2.17.0/vuedraggable.min.js"></script>

```

[cf示例部分](https://github.com/SortableJS/Vue.Draggable/tree/master/examples)

## 对于Vue.js 2.0

使用draggable组件:

### 典型用途:
``` html
<draggable v-model="myArray" :options="{group:'people'}" @start="drag=true" @end="drag=false">
   <div v-for="element in myArray" :key="element.id">{{element.name}}</div>
</draggable>
```
.vue文件:
``` js
  import draggable from 'vuedraggable'
  ...
  export default {
        components: {
            draggable,
        },
  ...
```

### 使用`transition-group`:
``` html
<draggable v-model="myArray">
    <transition-group>
        <div v-for="element in myArray" :key="element.id">
            {{element.name}}
        </div>
    </transition-group>
</draggable>
```

Draggable组件应直接包装可拖动元素，或者 `transition-component` 包含可拖动元素。


### 带脚部插槽：
``` html
<draggable v-model="myArray" :options="{draggable:'.item'}">
    <div v-for="element in myArray" :key="element.id" class="item">
        {{element.name}}
    </div>
    <button slot="footer" @click="addPeople">Add</button>
</draggable>
```
### 带头部插槽：
``` html
<draggable v-model="myArray" :options="{draggable:'.item'}">
    <div v-for="element in myArray" :key="element.id" class="item">
        {{element.name}}
    </div>
    <button slot="header" @click="addPeople">Add</button>
</draggable>
```

### 带Vuex：

```html
<draggable v-model='myList'>
``` 

```javascript
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


### Props
#### value
类型：`Array`<br>
是否是必须要的属性：`false`<br>
默认值：`null`

输入到draggable组件中的数组，通常与内部元素使用v-for指令引用的数组相同<br>
这是使用Vue.draggable的首选方法，因为它与Vuex兼容<br>
它不应该直接使用，只能通过`v-model`指令：
```html
<draggable v-model="myArray">
```

#### list
类型：`Array`<br>
是否是必须要的属性：`false`<br>
默认值：`null`

替代props中的value属性, list是一个与拖放同步的数组<br>
主要区别在于`list` prop由可拖动组件使用splice方法更新，而`value`是不可变的<br>
**不要与value prop一起使用**

#### options
类型：`Object`<br>
是否是必须要的属性：`false`

用于初始化可排序对象的选项，请参阅：[sortable option documentation](https://github.com/RubaXa/Sortable#options)<br>
请注意，所有以“on”开头的方法都将被忽略，因为可拖动组件通过事件公开相同的API

例如，可以使用此方法绑定添加拖动句柄 `:options="{handle:'.handle'}"` ，阅读链接文档，了解可用的其他选项。

#### element
类型：`String`<br>
默认值：`'div'`

HTML node type of the element that draggable component create as outer element for the included slot.<br>
It is also possible to pass the name of vue component as element. In this case, draggable attribute will be passed to the create component.<br>
See also [componentData](#componentdata) if you need to set props or event to the created component.

#### clone
类型：`Function`<br>
是否是必须要的属性：`false`<br>
默认值：`(original) => { return original;}`<br>

当clone选项为true时，函数调用源组件克隆元素。The unique argument is the viewModel element to be cloned and the returned value is its cloned version.<br>
By default vue.draggable reuses the viewModel element, so you have to use this hook if you want to clone or deep clone it.

#### move
类型：`Function`<br>
是否是必须要的属性：`false`<br>
默认值：`null`<br>

If not null this function will be called in a similar way as [Sortable onMove callback](https://github.com/RubaXa/Sortable#move-event-object).
Returning false will cancel the drag operation.

```javascript
function onMoveCallback(evt, originalEvent){
   ...
    // return false; — for cancel
}
```
evt object has same property as [Sortable onMove event](https://github.com/RubaXa/Sortable#move-event-object), and 3 additional properties:
 - `draggedContext`:  context linked to dragged element
   - `index`: dragged element index
   - `element`: dragged element underlying view model element
   - `futureIndex`:  potential index of the dragged element if the drop operation is accepted
 - `relatedContext`: context linked to current drag operation
   - `index`: target element index
   - `element`: target element view model element
   - `list`: target list
   - `component`: target VueComponent

HTML:
```HTML
<draggable :list="list" :move="checkMove">
```
javascript:
```javascript
checkMove: function(evt){
    return (evt.draggedContext.element.name!=='apple');
}
```
See complete example: [Cancel.html](https://github.com/SortableJS/Vue.Draggable/blob/master/examples/Cancel.html), [cancel.js](https://github.com/SortableJS/Vue.Draggable/blob/master/examples/script/cancel.js)

#### componentData
类型：`Object`<br>
是否是必须要的属性：`false`<br>
默认值：`null`<br>

This props is used to pass additional information to child component declared by [element props](#element).<br>
Value:
* `props`: props to be passed to the child component
* `on`: events to be subscribe in the child component

Example (using [element UI library](http://element.eleme.io/#/en-US)):
```HTML
<draggable element="el-collapse" :list="list" :component-data="getComponentData()">
    <el-collapse-item v-for="e in list" :title="e.title" :name="e.name" :key="e.name">
        <div>{{e.description}}</div>
     </el-collapse-item>
</draggable>
```
```javascript
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

### Events

* Support for Sortable events:

  `start`, `add`, `remove`, `update`, `end`, `choose`, `sort`, `filter`, `clone`<br>
  Events are called whenever onStart, onAdd, onRemove, onUpdate, onEnd, onChoose, onSort, onClone are fired by Sortable.js with the same argument.<br>
  [See here for reference](https://github.com/RubaXa/Sortable#event-object-demo)

  Note that SortableJS OnMove callback is mapped with the [move prop](https://github.com/SortableJS/Vue.Draggable/blob/master/README.md#move)

HTML:
```HTML
<draggable :list="list" @end="onEnd">
```

* change event

  `change` event is triggered when list prop is not null and the corresponding array is altered due to drag-and-drop operation.<br>
  This event is called with one argument containing one of the following properties:
  - `added`:  contains information of an element added to the array
    - `newIndex`: the index of the added element
    - `element`: the added element
  - `removed`:  contains information of an element removed from to the array
    - `oldIndex`: the index of the element before remove
    - `element`: the removed element
  - `moved`:  contains information of an element moved within the array
    - `newIndex`: the current index of the moved element
    - `oldIndex`: the old index of the moved element
    - `element`: the moved element

### Slots

Limitation: neither header or footer slot works in conjunction with transition-group.

#### Header
Use the `header` slot to add none-draggable element inside the vuedraggable component.
Important: it should be used in conjunction with draggable option to tag draggable element.
Note that header slot will always be added before the default slot regardless its position in the template.
Ex:

``` html
<draggable v-model="myArray" :options="{draggable:'.item'}">
    <div v-for="element in myArray" :key="element.id" class="item">
        {{element.name}}
    </div>
    <button slot="header" @click="addPeople">Add</button>
</draggable>
```

#### Footer
Use the `footer` slot to add none-draggable element inside the vuedraggable component.
Important: it should be used in conjunction with draggable option to tag draggable element.
Note that footer slot will always be added after the default slot regardless its position in the template.
Ex:

``` html
<draggable v-model="myArray" :options="{draggable:'.item'}">
    <div v-for="element in myArray" :key="element.id" class="item">
        {{element.name}}
    </div>
    <button slot="footer" @click="addPeople">Add</button>
</draggable>
```

### Gotchas
  - Drag operation with empty list:

    To be able to drag items on an empty draggable component, set a min-height greater than 0 on the `draggable` component or the `transition-group` if any and ensure the transition group has display: block; otherwise height won't work.

### Fiddle

- Simple:
https://jsfiddle.net/dede89/sqssmhtz/

- Two Lists:
https://jsfiddle.net/dede89/32ao2rpm/

- Example with list clone:
https://jsfiddle.net/dede89/t3m5krea/

- Example with transition-group:
https://jsfiddle.net/dede89/m2v0orcn/

- Example with table:
https://jsfiddle.net/dede89/L54yu3L9/

- Example with remove button
 on list elements
 https://jsfiddle.net/dede89/5Leuhh1n/
 
 ### Full demo example

[draggable-example](https://github.com/David-Desmaisons/draggable-example)

## For Vue.js 1.0

[See here](documentation/Vue.draggable.for.ReadME.md)

```
