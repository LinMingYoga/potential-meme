# 面试

---
## VUE
<img src="https://upload-images.jianshu.io/upload_images/1694678-a839a0c606ff4780.png?imageMogr2/auto-orient/strip|imageView2/2/w/554/format/webp" />

1. ### 谈谈你对Vue的理解吧

   - Vue的语法很简单,很容易上手,可以说就是对着模板来填充内容.想要动态绑定的数据,那么就在data中填充内容.想要具体的method,那么就在methods属性中填充内容.想要显示变量就用双花括号.可以说是很简单了.

     

   - Vue使用的是MVVM模式,并且使用简单的命令加数据来进行DOM操作,让我避开了进行繁杂的获取,创建和删除DOM元素的操作.

   

   - 借助vue的vue-router插件,可以很方便的实现单页面应用的搭建.并且可以实现浏览器中的回退效果.

   

   - Vue的组件化开发可以说很方便的就实现了组件的复用.之前想要写复用组件的时候,一方面要写重复的html,另一方面把对应的CSS文件和JS文件独立开来,需要的时候进行引入.但是vue把三者放在同一个文件中,可以说非常的贴心,修改也很方便.

2. ### 你刚刚说到了MVVM，能详细说说吗？

   MVVM分为`Model`、`View`、`ViewModel`三者
   
   - `Model`:
     代表数据模型，数据和业务逻辑都在Model层中定义
   - `view`:
     代表UI视图，负责数据的展示
   - `ViewModel`：
     负责监听Model中数据的改变并且控制视图的更新
   
   总结：`MVVM`模式简化了界面与业务的依赖，解决了数据频繁更新。`MVVM` 在使用当中，利用双向绑定技术，使得 `Model` 变化时，`ViewModel `会自动更新，而 `ViewModel `变化时，`View`也会自动变化
   
3. ### 那vue中是如何检测数组变化的呢？

   数组就是使用 `object.defineProperty` 重新定义数组的每一项，那能引起数组变化的方法我们都是知道的， `pop `、 `push` 、 `shift `、 `unshift` 、 `splice `、 `sort` 、 reverse 这七种，只要这些方法执行改了数组内容，我就更新内容就好了，是不是很好理解。
   
   1. 是用来函数劫持的方式，重写了数组方法，具体呢就是更改了数组的原型，更改成自己的，用户调数组的一些方法的时候，走的就是自己的方法，然后通知视图去更新。
   
   2. 数组里每一项可能是对象，那么我就是会对数组的每一项进行观测，（且只有数组里的对象才能进行观测，观测过的也不会进行观测）
   
   
   vue3：改用 proxy ，可直接监听对象数组的变化。
   
4. ### 那你说说Vue的事件绑定原理吧

   1.  原生 DOM 的绑定：Vue在创建真实DOM时会调用 createElm ，默认会调用 invokeCreateHooks 。会遍历当前平台下相对的属性处理代码，其中就有 updateDOMListeners 方法，内部会传入 add（） 方法
   
   2. 组件绑定事件，原生事件，自定义事件；组件绑定之间是通过Vue中自定义的 $on 方法实现的。
   
   
   （可以理解为：组件的 nativeOnOn  等价于 普通元素on 组件的on会单独处理）
   
5. ### v-model中的实现原理及如何自定义v-model

   `v-model` 可以看成是 `value + input `方法的语法糖（组件）。原生的` v-model` ，会根据标签的不同生成不同的事件与属性。解析一个指令来。
   
   自定义：自己写 `model `属性，里面放上 `prop `和 `event`
   
6. ### 为什么Vue采用异步渲染呢

   异步方法，异步渲染最后一步，与JS事件循环联系紧密。主要使用了宏任务微任务（setTimeout、promise那些），定义了一个异步方法，多次调用nextTick会将方法存入队列，通过异步方法清空当前队列。
   
7. ### 说说Vue的生命周期吧

   `beforeCreate `：实例初始化之后，数据观测之前调用
   
   `created`：实例创建万之后调用。实例完成：数据观测、属性和方法的运算、 watch/event 事件回调。无 $el .
   
   `beforeMount`：在挂载之前调用，相关 render 函数首次被调用
   
   `mounted`：了被新创建的vm.$el替换，并挂载到实例上去之后调用改钩子。
   
   `beforeUpdate`：数据更新前调用，发生在虚拟DOM重新渲染和打补丁，在这之后会调用改钩子。
   
   `updated`：由于数据更改导致的虚拟DOM重新渲染和打补丁，在这之后会调用改钩子。
   
   `beforeDestroy`：实例销毁前调用，实例仍然可用。
   
   `destroyed`：实例销毁之后调用，调用后，Vue实例指示的所有东西都会解绑，所有事件监听器和所有子实例都会被移除
   
   
   
   - 每个生命周期内部可以做什么？
   
       `created`：实例已经创建完成，因为他是最早触发的，所以可以进行一些数据、资源的请求。
       
       `mounted`：实例已经挂载完成，可以进行一些DOM操作。
       
       `beforeUpdate`：可以在这个钩子中进一步的更改状态，不会触发重渲染。
       
       `updated`：可以执行依赖于DOM的操作，但是要避免更改状态，可能会导致更新无线循环。
       
       `destroyed`：可以执行一些优化操作，清空计时器，解除绑定事件。
       
       
   
   - `ajax`放在哪个生命周期？：一般放在 `mounted `中，保证逻辑统一性，因为生命周期是同步执行的， `ajax `是异步执行的。单数服务端渲染 `ssr `同一放在 `created `中，因为服务端渲染不支持 `mounted `方法。
   - 什么时候使用`beforeDestroy`？：当前页面使用` $on `，需要解绑事件。清楚定时器。解除事件绑定， `scroll mousemove `。
   
8. ### 父子组件生命周期调用顺序（简单）

   - 渲染顺序：先父后子，完成顺序：先子后父

   - 更新顺序：父更新导致子更新，子更新完成后父

   - 销毁顺序：先父后子，完成顺序：先子后父

     

9. ### Vue组件通信

   - 父子间通信:父亲提供数据通过属性 props传给儿子；儿子通过 $on 绑父亲的事件，再通过 $emit 触发自己的事件（发布订阅）

   - 利用父子关系 $parent 、 $children ，

   - 获取父子组件实例的方法。

   - 父组件提供数据，子组件注入。 provide 、 inject ，插件用得多。

   - ref 获取组件实例，调用组件的属性、方法

   - 跨组件通信 Event Bus  （Vue.prototype.bus=newVue）其实基于bus = new Vue）其实基于bus=newVue）其实基于on与$emit

   - vuex  状态管理实现通信

10. ### Vuex 工作原理

    1. vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**。

    2. 状态自管理应用包含以下几个部分：

    - state，驱动应用的数据源；
    - view，以声明方式将 state 映射到视图；
    - actions，响应在 view 上的用户输入导致的状态变化。下图单向数据流

    <img src="https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/aecf55d9a396473c95dff6eca9ecfb7b~tplv-k3u1fbpfcp-watermark.image" />

    3. vuex，多组件共享状态，因-单向数据流简洁性很容易被破坏：

     - 多个视图依赖于同一状态

    - 来自不同视图的行为需要变更同一状态。

      <img src="https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b17eb2230b3f46729cbfbe7144873065~tplv-k3u1fbpfcp-watermark.image" />

11. ### 如何从真实DOM到虚拟DOM

    涉及到Vue中的模板编译原理，主要过程：

    1. 将模板转换成` ast` 树，` ast` 用对象来描述真实的JS语法（将真实DOM转换成虚拟DOM）
    2. 优化树
    3. 将` ast` 树生成代码

12. ### 用VNode来描述一个DOM结构

    虚拟节点就是用一个对象来描述一个真实的`DOM元素`。首先将 `template `（真实DOM）先转成 `ast `， `ast `树通过 `codegen `生成 `render `函数， `render `函数里的 `_c` 方法将它转为虚拟dom

13. ### diff算法

    - 时间复杂度： 个树的完全 diff 算法是一个时间复杂度为 O(n*3） ，vue进行优化转化成 O(n) 。

    理解：


    最小量更新， key 很重要。这个可以是这个节点的唯一标识，告诉 diff 算法，在更改前后它们是同一个DOM节点
    
    扩展 v-for 为什么要有 key ，没有 key 会暴力复用，举例子的话随便说一个比如移动节点或者增加节点（修改DOM），加 key 只会移动减少操作DOM。


    只有是同一个虚拟节点才会进行精细化比较，否则就是暴力删除旧的，插入新的。


    只进行同层比较，不会进行跨层比较。


    diff算法的优化策略：四种命中查找，四个指针


    旧前与新前（先比开头，后插入和删除节点的这种情况）


    旧后与新后（比结尾，前插入或删除的情况）


    旧前与新后（头与尾比，此种发生了，涉及移动节点，那么新前指向的节点，移动到旧后之后）


    旧后与新前（尾与头比，此种发生了，涉及移动节点，那么新前指向的节点，移动到旧前之前）

14. ### Computed watch 和 method

    - computed：默认computed也是一个watcher具备缓存，只有当依赖的数据变化时才会计算, 当数据没有变化时, 它会读取缓存数据。如果一个数据依赖于其他数据，使用 computed

    - watch：每次都需要执行函数。  watch 更适用于数据变化时的异步操作。如果需要在某个数据变化时做一些事情，使用watch。

    - method：只要把方法用到模板上了,每次一变化就会重新渲染视图，性能开销大

15. ### v-if 和 v-show 区别

    - `v-if` 如果条件不成立不会渲染当前指令所在节点的DOM元素
    - `v-show` 只是切换当前DOM的显示与隐藏

16. ### v-for和v-if为什么不能连用

    `v-for` 会比 `v-if` 的优先级更高，连用的话会把` v-if` 的每个元素都添加一下，造成性能问题。

17. ### v-html 会导致哪些问题（简单）

    - `XSS` 攻击
    - `v-html` 会替换标签内部的元素

18. ### 描述组件渲染和更新过程

    渲染组件时，会通过` vue.extend()` 方法构建子组件的构造函数，并进行实例化。最终手动调用` $mount()` 进行挂载。更新组件时会进行` patchVnode` 流程，核心就是` diff` 算法。

19. ### 组件中的data为什么是函数

    避免组件中的数据互相影响。同一个组件被复用多次会创建多个实例，如果 data 是一个对象的话，这些实例用的是同一个构造函数。为了保证组件的数据独立，要求每个组件都必须通过 data 函数返回一个对象作为组件的状态。

20. ### 为什么要使用异步组件？

    1. 节省打包出的结果，异步组件分开打包，采用jsonp的方式进行加载，有效解决文件过大的问题。
    2. 核心就是包组件定义变成一个函数，依赖` import（）` 语法，可以实现文件的分割加载。

21. ### action 与 mutation 的区别

    - `mutation` 是同步更新，` $watch` 严格模式下会报错
    - `action` 是异步操作，可以获取数据后调用` mutation` 提交最终数据

22. ## 插槽与作用域插槽的区别

    1. ### 插槽

        - 创建组件虚拟节点时，会将组件儿子的虚拟节点保存起来。当初始化组件时，通过插槽属性将儿子进行分类` {a:[vnode],b[vnode]}`
        - 渲染组件时会拿对应的` slot` 属性的节点进行替换操作。（插槽的作用域为父组件）
        
    2. ### 作用域插槽

        - 作用域插槽在解析的时候不会作为组件的孩子节点。会解析成函数，当子组件渲染时，会调用此函数进行渲染。
        - 普通插槽渲染的作用域是父组件，作用域插槽的渲染作用域是当前子组件。

23. vue中相同逻辑如何抽离

    - 其实就是考察` vue.mixin` 用法，给组件每个生命周期，函数都混入一些公共逻辑。

24. ### 谈谈对keep-alive的了解

    `keep-alive` 可以实现组件的缓存，当组件切换时不会对当前组件进行卸载。常用的2个属性` include/exclude` ，2个生命周期` activated` ，` deactivated`

25. ### Vue性能优化

    **编码优化**：

    - 事件代理
    - `keep-alive`
    - 拆分组件
    - `key` 保证唯一性
    - 路由懒加载、异步组件
    - 防抖节流

    **Vue加载性能优化**

    - 第三方模块按需导入（` babel-plugin-component` ）
    - 图片懒加载

    **用户体验**

    - `app-skeleton` 骨架屏
    - `shellap` p壳
    - `pwa`

    **SEO优化**

    - 预渲染

---

## 防抖与节流

- ### 防抖

  概念：延迟要执行的东站，若在延时的这段时间内，再次出发了，则取消之前开启的东站，重新计时

  实现：定时器

  应用：搜索时等用户完整输入内容后再发送请求。

  ``` javascript
  let input = document.getElementById('input')
  let id
  input.addEventListener('keyWord', () =>{
      let data = input.value
      clearTimer(id)
      id = setTimeout(() => {
          // ajax code
      }, 500)
  })
  ```

- ### 节流

  概念：设定一个特定的时间，让函数再特定的时间内只执行一次，不会频繁执行

  实现：定时器、标识

  ``` javascript
  handleClick() {
      clearTimeout(this.timeout)
      this.timeout = setTimeout(() => {
        this.name++
      }, 1000)
  }
  ```

---

## JavaScript

- ### 讲讲JS的数据类型？

  `JS的数据类型有8种`

  1. 在ES5的时候，我们认知的数据类型确实是 6种：`Number`、`String`、`Boolean`、`undefined`、`object`、`Null`。
  2. ES6 中新增了一种 `Symbol `。这种类型的对象永不相等，即始创建的时候传入相同的值，可以解决属性名冲突的问题，做为标记。
  3. 谷歌67版本中还出现了一种 `bigInt`。是指安全存储、操作大整数。（但是很多人不把这个做为一个类型）。

  JS数据类型：JS 的数据类型有几种？

     8种。`Number`、`String`、`Boolean`、`Null`、`undefined`、`object`、`symbol`、`bigInt`。

- ### var let const 的区别

  - var 会变量提升 let 和 const 不会
  - var 在全局命名的变量会挂载到 window 上，let const 不会
  - let 和 const 有块级作用域（暂时性死区），var 没有
  - let const 不允许重复命名

- ### 解决跨域问题

  1. JSONP：利用`<script>`标签不受跨域限制的特点，缺点是只能支持 get 请求
  2. 服务端设置请求头：` CORS: Access-Control-Allow-Origin：*`
  3. postMessage
  
- ### Javascript 的作用域和作用域链

  1. 作用域： 作用域是定义变量的区域，它有一套访问变量的规则，这套规则来管理浏览器引擎如何在当前作用域以及嵌套的作用域中根据变量（标识符）进行变量查找。
  2. 作用域链：作用域链的作用是保证对执行环境有权访问的所有变量和函数的有序访问，通过作用域链，我们可以访问到外层环境的变量和 函数。

  作用域链的本质上是一个指向变量对象的指针列表。变量对象是一个包含了执行环境中所有变量和函数的对象。作用域链的前 端始终都是当前执行上下文的变量对象。全局执行上下文的变量对象（也就是全局对象）始终是作用域链的最后一个对象。

  

  当我们查找一个变量时，如果当前执行环境中没有找到，我们可以沿着作用域链向后查找。

  作用域链的创建过程跟执行上下文的建立有关....

- ### 谈谈你对this、call、apply和bind的理解

  1. 在浏览器里，在全局范围内this 指向window对象；

  2. 在函数中，this永远指向最后调用他的那个对象；

  3. 构造函数中，this指向new出来的那个新的对象；

  4. call、apply、bind中的this被强绑定在指定的那个对象上；

  5. 箭头函数中this比较特殊,箭头函数this为父作用域的this，不是调用时的this.要知道前四种方式,都是调用时确定,也就是动态的,而箭头函数的this指向是静态的,声明的时候就确定了下来；

  6. apply、call、bind都是js给函数内置的一些API，调用他们可以为函数指定this的执行,同时也可以传参。

     <img src="https://user-gold-cdn.xitu.io/2020/5/29/1725c096bb763965?imageView2/0/w/1280/h/960/format/webp/ignore-error/1" />

- ### js 获取原型的方法？

  - p.**proto**
  - p.constructor.prototype
  - Object.getPrototypeOf(p)

- ###  什么是闭包，为什么要用它？

  **闭包是指有权访问另一个函数作用域内变量的函数**，创建闭包的最常见的方式就是在一个函数内创建另一个函数，创建的函数可以 访问到当前函数的局部变量。

  

  闭包有两个常用的用途。

  - 闭包的第一个用途是使我们在函数外部能够访问到函数内部的变量。通过使用闭包，我们可以通过在外部调用闭包函数，从而在外部访问到函数内部的变量，可以使用这种方法来创建私有变量。

  - 函数的另一个用途是使已经运行结束的函数上下文中的变量对象继续留在内存中，因为闭包函数保留了这个变量对象的引用，所以这个变量对象不会被回收。

    ```javascript
    function a(){
        var n = 0;
        function add(){
           n++;
           console.log(n);
        }
        return add;
    }
    var a1 = a(); //注意，函数名只是一个标识（指向函数的指针），而（）才是执行函数；
    a1();    //1
    a1();    //2  第二次调用n变量还在内存中
    ```

    其实闭包的本质就是作用域链的一个特殊的应用，只要了解了作用域链的创建过程，就能够理解闭包的实现原理。

  - ### js数组和字符串有哪些原生方法,列举一下

    <img src="https://user-gold-cdn.xitu.io/2020/7/2/1730ee989aa2b15f?imageslim" />

    <img src="https://user-gold-cdn.xitu.io/2020/6/2/1727407aed442084?imageslim" />

  - ### ECMAScript 2015（ES6）有哪些新特性？

    - 块作用域
    - 类
    - 箭头函数
    - 模板字符串
    - 加强的对象字面
    - 对象解构
    - Promise
    - 模块
    - Symbol
    - 代理（proxy）Set
    - 函数默认参数
    - rest 和展开

---

### git 代码提交格式

|    值    |                             描述                             |
| :------: | :----------------------------------------------------------: |
|   feat   |                         新增一个功能                         |
|   fix    |                         修复一个 Bug                         |
|   docs   |                           文档变更                           |
|  style   |       代码格式（不影响功能，例如空格、分号等格式修正）       |
| refactor |                           代码重构                           |
|   perf   |                           改善性能                           |
|   test   |                             测试                             |
|  build   | 变更项目构建或外部依赖（例如 scopes: webpack、gulp、npm 等） |
|    ci    | 更改持续集成软件的配置文件和 package 中的 scripts 命令，例如 scopes: Travis, Circle 等 |
|  chore   |                    变更构建流程或辅助工具                    |
|  revert  |                           代码回退                           |

---

## Webpack

- 说一下 Webpack 的热更新原理吧

  `Webpack` 的热更新又称热替换（`Hot Module Replacement`），缩写为 `HMR`。 这个机制可以做到不用刷新浏览器而将新变更的模块替换掉旧的模块。

  HMR的核心就是客户端从服务端拉去更新后的文件，准确的说是 chunk diff (chunk 需要更新的部分)，实际上 WDS 与浏览器之间维护了一个 `Websocket`，当本地资源发生变化时，WDS 会向浏览器推送更新，并带上构建时的 hash，让客户端与上一次资源进行对比。客户端对比出差异后会向 WDS 发起 `Ajax` 请求来获取更改内容(文件列表、hash)，这样客户端就可以再借助这些信息继续向 WDS 发起 `jsonp` 请求获取该chunk的增量更新。

  后续的部分(拿到增量更新之后如何处理？哪些状态该保留？哪些又需要更新？)由 `HotModulePlugin` 来完成，提供了相关 API 以供开发者针对自身场景进行处理，像`react-hot-loader` 和 `vue-loader` 都是借助这些 API 实现 HMR。

---

### 页面默认布局是 static

![image-20210421094203392](C:\Users\lm\AppData\Roaming\Typora\typora-user-images\image-20210421094203392.png)

### 3个使用this的典型应用

 1. 在html原生事件属性中使用，如：

    ``` html
    <input type=”button” onclick=”showInfo(this);” value=”点击一下”/>
    ```

	2. 构造函数

    ```javascript
    function Animal(name, color) {
    　　this.name = name;
    　　this.color = color;
    }
    ```

	3. input 点击，获取值

    ``` html
    <input type="button" id="text" value="点击一下" />
    <script type="text/javascript">
        var btn = document.getElementById("text");
        btn.onclick = function() {
            alert(this.value);    //此处的this是按钮元素
        }
    </script>
    ```

## 什么是回流，什么是重绘，有什么区别？

- ### html 加载时发生了什么

  在页面加载时，浏览器把获取到的HTML代码解析成1个DOM树，DOM树里包含了所有HTML标签，包括display:none隐藏，还有用JS动态添加的元素等。
   浏览器把所有样式(用户定义的CSS和用户代理)解析成样式结构体
   DOM Tree 和样式结构体组合后构建render tree, render tree类似于DOM tree，但区别很大，因为render tree能识别样式，render tree中每个NODE都有自己的style，而且render tree不包含隐藏的节点(比如display:none的节点，还有head节点)，因为这些节点不会用于呈现，而且不会影响呈现的，所以就不会包含到 render tree中。我自己简单的理解就是DOM Tree和我们写的CSS结合在一起之后，渲染出了render tree。

- ### 什么是回流

  当render tree中的一部分(或全部)因为元素的规模尺寸，布局，隐藏等改变而需要重新构建。这就称为回流(reflow)。每个页面至少需要一次回流，就是在页面第一次加载的时候，这时候是一定会发生回流的，因为要构建render tree。在回流的时候，浏览器会使渲染树中受到影响的部分失效，并重新构造这部分渲染树，完成回流后，浏览器会重新绘制受影响的部分到屏幕中，该过程成为重绘。

- ### 什么是重绘

  当render tree中的一些元素需要更新属性，而这些属性只是影响元素的外观，风格，而不会影响布局的，比如background-color。则就叫称为重绘。

- ### 区别:

  他们的区别很大：
   回流必将引起重绘，而重绘不一定会引起回流。比如：只有颜色改变的时候就只会发生重绘而不会引起回流
   当页面布局和几何属性改变时就需要回流
   比如：添加或者删除可见的DOM元素，元素位置改变，元素尺寸改变——边距、填充、边框、宽度和高度，内容改变