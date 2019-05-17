# 循环 条件 模板 事件处理 引用 Filter过滤器

标签（空格分隔）： 百度小程序

---

## **s-for**
通过循环渲染列表是常见的场景，在元素上作用 s-for 指令，我们可以渲染一个列表。

**默认情况**
默认不写情况下，下标索引是为 index，数组当前变量名默认为 item。
```
<!-- for-demo.swan-->
<view>
    <view s-for="persons">
        {{index}}: {{item.name}}
    </view>
</view>
// for-demo.js
Page({
    data: {
        persons: [
            {name: 'Curry'},
            {name: 'Thompson'},
            {name: 'Durant'},
            {name: 'Green'},
            {name: 'Cousins'}
        ]
    }
});
```

**简写**
通过简写的方式，指定下标索引和数组当前变量名。
```
<!-- for-demo.swan-->
<view>
    <view s-for="p,index in persons">
        {{index}}: {{p.name}}
    </view>
</view>
```
也可以通过使用 s-for-index 来指定下标索引，s-for-item 来指定数组当前变量名。
```
<!-- for-demo.swan-->
<view>
    <view s-for="persons" s-for-index="idx" s-for-item="p">
        {{idx}}: {{p.name}}
    </view>
</view>
```

# **条件**
## **s-if**
通过 s-if 指令我们可以实现以下操作：

· 为元素指定条件:当条件成立时元素可见，当条件不成立时元素不存在。

· 为 s-if 增加一个额外条件分支块。

· 为 s-if 增加一个不满足条件的分支块。s-else 指令没有值。

· **s-if 与 s-for 不可在同一标签下同时使用。**
```
<!-- if-demo.swan-->
<view s-if="is4G">4G</view>
<view s-elif="isWifi">Wifi</view>
<view s-else>Other</view>
// if-demo.js
Page({
    data: {
        is4G: true,
        isWifi: false
    }
});
```
**block s-if**
block 虚拟组件，在渲染时不会包含自身，只会渲染其内容。可以用来渲染一组组件或者标签。
```
<!-- if-demo.swan-->
<block s-if="flag">
  <view> name </view>
  <view> age </view>
</block>
```

# 模板

定义模板
name 属性，定义了模板的名字。<template>内定义代码片段，如：
```
<!-- template-demo.swan-->
<template name="person-card">
    <view>
        <text>位置: {{pos}}</text>
        <text>姓名: {{name}}</text>
    </view>
</template>
```
使用模板
通过 is 属性，声明需要使用的模板，data 是所需要传入到模板的值，注意对象字面量的使用方法，对象字面量是三个大括号包裹。
```
<!-- template-demo.swan-->
<template is="person-card" data="{{{...person}}}" />
// template-demo.js
Page({
    data: {
        person: {name: 'Lebron James', pos: 'SF', age: 33}
    }
});
```
# 事件处理
冒泡事件如下列表展示，不在列表展示的事件均为非冒泡事件。

事件类型 | 触发时机
-----|-----
tap | 触摸后马上离开
longtap | 触摸后超过350ms再离开（推荐使用 longpress 事件代替）
longpress | 触摸后超过350ms再离开，如果是指定了事件回调函数并触发了这个事件，tap 事件将不被触发
touchstart | 触摸开始时
touchmove | 触摸后移动时
touchcancel | 触摸后被打断时，如来电等
touchend | 触摸结束时
touchforcechange | 支持 3D Touch 的 iPhone 设备，重按时会触发。

## 事件对象
默认当组件触发事件时，逻辑层绑定事件的处理函数会收到一个默认参数，即事件对象。

下面是事件对象详细属性列表：

属性 | 类型 | 说明
---|----|---
type | String | 事件的类型
timeStamp | Integer | 事件触发的时间戳（毫秒）
target | Object | 触发事件的组件的属性值集合，详细属性参见 target
currentTarget | Object | 当前组件的一些属性值集合，详细属性参见 currentTarget
detail | Object | 自定义事件对象属性列表，详细属性参见 detail
touches | Array | 触摸事件类型存在，存放当前停留在屏幕中的触摸点信息的数组，touch 详细属性参见 touch
changedTouches | Array | 触摸事件类型存在，存放当前变化的触摸点信息的数组, changedTouch changedTouch
下面是事件对象的属性为属性值集合时的详细信息。

target
属性 | 类型 | 说明
---|----|---
id | String | 触发事件组件的 id
tagName | String | 触发事件组件的类型
dataset | Object | 触发事件组件上由data-开头的自定义属性组成的集合,详细属性参见 dataset
currentTarget
属性 | 类型 | 说明
---|----|---
id | String | 事件绑定的组件的 id
tagName | String | 事件绑定的组件的类型
dataset | Object | 事件绑定的组件上由data-开头的自定义属性组成的集合,详细属性参见 dataset
detail
是自定义事件所携带的数据，具体详见组件定义中各个事件的定义。

dataset
在组件的事件被触发时，也可以传递自定义的数据。
书写方式： 以 data- 开头，多个单词由连字符-链接，不能有大写(大写会自动转成小写)，最终的 - 在 dataset 中会将连字符转成驼峰式写法。
如组件上data-car-color属性值的读取方式是: event.currentTarget.dataset.carColor。

touch
属性 | 类型 | 说明
---|----|---
identifier | Number | 触摸点的标识符
clientX, clientY | Number | 距离页面可显示区域（屏幕除去导航条）左上角的X轴与Y轴的距离
pageX, pageY | Number | 距离文档左上角的X轴与Y轴的距离
changedTouch
数据格式同 touches，指的是有变化的触摸点，如 touchstart（开始），touchmove（变化），touchend，touchcancel（结束）等。

点击事件的 detail 带有的 x, y 同 pageX, pageY 代表距离文档左上角的距离。

# 引用
SWAN 可以通过import和include来引用文件。

import
通过import和template配合使用，可以将代码分离以及复用。
```
<!-- personCard.swan-->
<template name="person-card">
    <view>
        <text>位置: {{pos}}</text>
        <text>姓名: {{name}}</text>
    </view>
</template>
```
在personCard.swan里定义了一个模板，在index.swan里引用文件，并使用它的模板。
```
<!-- index.swan-->
<import src="./person-card.swan" />
<template is="person-card" data="{{person}}" />
```
## include
通过include可以将目标模板，整个(除了 template)引入到当前的位置，相当于inline。
```
<!-- detail.swan-->
<include src="header.swan" />
<view class="detail">body</view>
<!-- header.swan-->
<view class="header">header</view>
```

include 可以将目标文件除了 `<template/>`外的整个代码引入，相当于是拷贝到 include 位置，如：
```
<!-- index.swan -->
<include src="header.swan"/>
<view> body </view>
<include src="footer.swan"/>
<!-- header.swan -->
<view> header </view>
<!-- footer.swan -->
<view> footer </view>
```
# Filter过滤器
Filter 是小程序的过滤器，结合 SWAN 模版，可以构建出页面的结构。

说明：
1. Filter 文件命名方式为:模块名.filter.js;
2. Filter 通过 export default 方式对外暴露其内部的私有函数;
3. Filter 只能导出function函数;
4. Filter 函数不能作为组件的事件回调;
5. Filter 可以创建独立得模块，也可以通过内联的形式;
6. Filter 不支持全局变量;
7. 多个 filter 标签不能出现相同的 src 属性值， module 属性的值也是标识模块的唯一 id 。
## 数据处理示例
```
// page.js
Page({
  data: {
    array: [1, 3, 6, 8, 2, 0]
  }
});
// test.filter.js
export default {
    maxin: arr => {
        var max = undefined;
        for (var i = 0; i < arr.length; ++i) {
            max = max === undefined ?
            arr[i] :
            (max >= arr[i] ? max : arr[i]);
        }
        return max;
    }
};
<!-- swan模版 -->
<view>{{swan.maxin(array)}}</view>
<filter src="./test.filter.js" module="swan"></filter>
```
## 页面输出：

8
Filter模块
Filer 代码可以编写在 swan 文件中的标签内，或以 .filter.js 为后缀名的文件内或者通过内联的形式`<filter></filter>`。 每一个 .filter.js 文件和`<filter></filter>`标签都是一个单独的模块。 每个模块都有自己独立的作用域。即在一个模块里面定义的变量与函数，默认为私有的，对其他模块不可见。
.filter.js 文件

示例：
```
export default {
    Foo: () => {
        return 'swan-foo-filter';
    },
    Bar: () => {
        return 'swan-bar-filter';
    }
}
```
`<filter>` 标签
标签可以是双闭合 `<filter></filter>` 或者单闭合 `<filter/>`，带有src属性的标签，过滤器代码要写到相应的文件里，不带有src属性的标签，过滤器代码写在标签内。



属性名 | 类型 | 说明
------- | ------- | -------
src | String | 引用 .filter.js 文件的相对路径。
module | String | 当前标签的模块名,必填字段。
**页面渲染示例**
```
<!-- swan -->
<view> {{swan.message()}} </view>
<filter module="swan">
    export default {
        message: function() {
            return 'Hello world';
        }
    }
</filter>
```
页面输出：

Hello world
注释
Filter 的注释与swan模版文件的注释方式相同。

示例代码:
```
<!-- <filter module="swan">
    function Foo() {
        return 'programer';
    }

    export default {
        test: Foo
    };
</filter> -->
```
运算符&语句&数据类型&基础类库
Filter支持javascript所有运算符、语句、数据类型和基础类库。




























