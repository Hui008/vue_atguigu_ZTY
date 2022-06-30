# vue_atguigu_ZTY
vue
### 1. 初识Vue

1. 想让Vue工作，就必须创建一个Vue实例，且要传入配置对象
2. root容器中的代码依然符合html规范，只不过混入了一些特殊的Vue语法
3. root容器里的代码被称为【Vue模板】
4. Vue实例和容器是一一对应的
5. 真实开发中只有一个Vue实例，并且会配合着组件一起使用
6. {{xxx}}中的xxx要写`JS`表达式，且xxx可以自动读取到data中的所有属性
7. 一旦data中的数据发生改变，那么模板中用到该数据的地方也会自动更新

`初识Vue`

```html
<script src="../js/vue.js"></script>

<body>
  <div id="root">
    <!-- 插值语法 -->
    <h1>Hello,{{name}}</h1>
    <h1>My age is {{age}}</h1>
  </div>
  <script type="text/javascript">
    Vue.config.productionTip = false // 阻止Vue的生产提示

    // 创建Vue实例
    new Vue({
      el: '#root', // el用于指定当前Vue实例为哪个容器服务，值通常为css选择器字符串
      data: {
        name: '尚硅谷',
        age: 20
      }
    })
  </script>
</body>
```

---

### 2. 模板语法

Vue模板语法有2大类

1. 插值语法
   * 功能：用于解析标签体内容
   * 写法：{{xxx}}，xxx是表达式，且可以直接读取到data中的所有属性
2. 指令语法
   * 功能：用于解析标签(包括：标签属性，标签体内容，绑定事件...)
   * 举例：`v-bind:href='xxx'` 或 简写为 `:href="xxx"`, xxx同样要写js表达式，且可以直接读取到data中的所有属性
   * 备注：Vue中的很多指令的形式都是`v-???`

`模板语法.html`

```html
<script src="../js/vue.js"></script>

<body>
  <div id="root">
    <h1>插值语法</h1>
    <h3>你好,{{name}}</h3>
    <h1>指令语法</h1>
    <!-- <a v-bind:href="url">点我去尚硅谷</a> -->
    <a :href="url">点我去尚硅谷</a>
  </div>

  <script type="text/javascript">
    Vue.config.productionTip = false

    new Vue({
      el:'#root',
      data:{
        name: 'jack',
        url: 'http://www.atguigu.com'
      }
    })
  </script>
</body>
```

---

### 3. 数据绑定

Vue中有2中数据绑定方式：

1. 单向绑定(v-bind): 数据只能从data中流向页面
2. 双向绑定(v-model): 数据不仅能从data中流向页面，而且可以从页面中流向data
   * 双向绑定一般用于表单元素上(如： input，select等)
   * v-model: value 可以简写为v-model, 因为v-model默认收集的就是value值

`数据绑定`

```html
<script src="../js/vue.js"></script>

<body>
  <div id="root">
    <!-- 普通写法 -->
<!--     单向数据绑定: <input type="text" v-bind:value = "name"><br>
    双向数据绑定: <input type="text" v-model:value = "name"> -->

    <!-- 简写 -->
    单向数据绑定: <input type="text" :value = "name"><br>
    双向数据绑定: <input type="text" v-model = "name">
  </div>

  <script type="text/javascript">
    Vue.config.productionTip = false

    new Vue({
      el:'#root',
      data: {
        name: '尚硅谷atguigu'
      }
    })
  </script>
</body>
```

****

### 4. el与data的两种写法

1. el 的两种写法
   1. new Vue的时候配置el属性
   2. 先创建Vue实例，随后再通过`vm.$mount('#root')指定el`的值
2. data 的两种写法
   1. 对象式
   2. 函数式

> 由Vue管理的函数不要写箭头函数，一旦写了箭头函数，this就不再是Vue实例了

`el与data两种写法`

```html
<script src='../js/vue.js'></script>

<body>
  <div id='root'>
    <h1>我是{{name}}</h1>
  </div>

  <script type='text/javascript'>
    Vue.config.productionTip = false // 取消Vue的生产提示

    // el的两种写法
    const vm = new Vue({
      // el: '#root', // 第一种el写法
      data: {
        name: '张伟zs'
      }
    })
    setTimeout(() => {
      vm.$mount('#root') // 第二种el写法
    },1000)

    // data第一种写法
    new Vue({
      el: '#root',
      data: {
        name: '张伟'
      }
    })

    // data第二种写法
    new Vue({
      el: '#root',
      data: function() {
        return {
          name: '张伟'
        }
      }
    })
  </script>
</body>
```

---

###  5. MVVM模型

1. M：模型(model)，data中的数据
2. V：视图(View)，模板代码
3. VM：视图模型(ViewModel)，Vue实例

> 1. data中的所有属性，最后都出现在vm身上
> 2. vm身上的所有属性以及Vue原型上的所有属性，在Vue模板中都可以直接使用

---

### 6. 数据代理

1. Vue中的数据代理：
   	通过vm对象来代理data对象中属性的操作（读/写）

2. Vue中数据代理的好处：

   ​	更加方便的操作data中的数据

3.  基本原理
   1. 通过Object.defineProperty()把data对象中的所有属性添加到vm上
   2. 为每一个添加到vm上的属性，都指定一个getter/setter
   3. 在getter/setter内部去操作（读/写）data中对应的属性=

---

### 7. 事件处理

1. 事件的基本使用
   1. 使用v-on:xxx 或者 @xxx 绑定事件，其中xxx是事件名
   2. 事件的回调函数需要配置在methods对象中，最终会在vm上
   3. methods中配置的函数，不要用箭头函数！否则this的指向就不是vm了
   4. methods中配置的函数，都是被Vue所管理的函数，this的指向是vm 或者 组件实例对象
   5. @click = "demo" 和 @click = "demo($event)" 效果一直，但是或者可以传递参数

`事件的基本使用`

```html
<script src='../js/vue.js'></script>
<body>
  <div id='root'>
    <h1>欢迎来到{{name}}</h1>
    <button v-on:click="showInfo1">点我提示信息(不传参数)</button>
    <button @click="showInfo2(66, $event)">点我提示信息(传参数)</button>
  </div>

  <script type='text/javascript'>
    Vue.config.productionTip = false // 取消Vue的生产提示

    const vm = new Vue({
      el:'#root',
      data: {
        name: '尚硅谷'
      },
      methods: {
        showInfo1(event) {
          console.log(event.target.innerText);
          alert('你好啊!')
        },
        showInfo2(number, event) {
          console.log(event.target.innerText);
          console.log(number);
          alert('Hello')
        }
      }
    })
  </script>
</body>
```

2. 事件修饰符
   1. prevent：阻止默认事件(常用)
   2. stop：阻止事件冒泡(常用)
   3. once：事件只触发一次(常用)
   4. capture：使用事件的捕获模式
   5. self：只有event.target是当前操作元素时才触发事件
   6. passive：事件的默认行为立即执行，无需等待事件回调执行完毕

`事件修饰符`

```html
 <script src='../js/vue.js'></script>
  <style>
    * {
      margin-top: 20px;
    }
    .demo1 {
      background-color: pink;
      height: 60px;
    }
  </style>
<body>
  <div id='root'>
    <h2>欢迎来到{{name}}</h2>
    <!-- 阻止默认事件(常用) -->
    <a href="http://www.baidu.com" @click.prevent = 'showInfo'>点我去百度</a>
    <!-- 阻止事件冒泡(常用) -->
    <div class="demo1" @click="showInfo">
      <button @click.stop="showInfo">点我提示信息</button>
    </div>
    <!-- 事件只触发(常用)一次 -->
    <button @click.once="showInfo">点我提示信息</button>
  </div>

  <script type='text/javascript'>
    Vue.config.productionTip = false // 取消Vue的生产提示

    new Vue({
      el:'#root',
      data: {
        name: '尚硅谷'
      },
      methods: {
        showInfo(event) {
          alert('Hello')
        }
      }
    })
  </script>
</body>
```

3. 键盘事件
   1. 常用的按键别名
      1. 回车 => enter
      2. 删除 => delete(捕获“删除”和“退格”键)
      3. 退出 => esc
      4. 空格 => space
      5. 换行 => tab(特殊，必须配合keydown)
      6. 上 => up
      7. 下 => down
      8. 左 => left
      9. 右 => right
   2. Vue未提供别名的按钮，可以使用按键原始的key值去绑定，但是要注意转为kebab-case(短横线命名)
   3. 系统修饰键(用法特殊)：ctrl，alt，shift，meta
      1. 配合keyup使用：按下修饰键的同时，再按下其他键，随后释放其他键，事件才被触发
      2. 配合keydown使用：正常触发事件
   4. 也可以使用keyCode去指定具体的按键(不推荐)
   5. Vue.config.keyCodes.自定义键名 = 键码，可以定制按键别名

---

### 8. 计算属性

1. 定义：要用的属性不存在，通过已有的属性计算得来
2. 原理：底层借助`Object.defineproperty`方法提供的getter和setter
3. get函数什么时候执行？
   * 初次读取时会执行一次
   * 当依赖的数据发生改变时会被再次调用
4. 优势：与methods相比，内部有缓存机制(服用).效率更高，调式方便

> 1. 计算属性最终会出现在vm上，直接读取使用即可
> 2. 如果计算属性被修改，那么必须写set函数去相应修改。且set中要引起计算时以来的数据发生变化

---

### 9. 监视属性

1. 当被监视的属性发生变化时，回调函数自动调用，进行相关操作

2. 监视的属性必须存在，才能进行监视

3. 监视的两种写法

   1. new Vue时传入watch配置
   2. 通过vm.$watch监视

   ```vue
   <script type='text/javascript'>
   
   const vm = new Vue({
         el:'#root',
         data: {
           isHot: true
         },
         computed: {
           info() {
             return this.isHot ? '炎热' : '凉爽'
           }
         },
         methods: {
           changeWeather() {
             this.isHot = !this.isHot
           }
         },
         watch: {
           // isHot: {
           //   // 初始化执行一次handler，默认为false
           //   immediate: true,
           //   // isHot 发生改变时，handler被调用
           //   handler(newValue, oldValue) {
           //     console.log('isHot被修改',newValue, oldValue);
           //   }
           // },
           info: {
             immediate: true,
             handler(newValue, oldValue) {
               console.log('isHot被修改',newValue, oldValue);
             }
           }
         }
       })
   vm.$watch('isHot', {
       // 初始化执行一次handler，默认为false
       immediate: true,
       // isHot 发生改变时，handler被调用
       handler(newValue, oldValue) {
       console.log('isHot被修改',newValue, oldValue);
       }
   })
   </script>
   ```

4. 深度监视

   1. Vue中的watch默认不监测对象内部值的改变(一层)
   2. 配置的deep: true;可以监测到对象内部值的改变(多层)

   > 1. Vue自身可以监测对象内部值的改变，但Vue提供的watch默认不可以！
   > 2. 使用watch时根据数据的具体结构，决定是否采用深度监视

   ```vue
   <script type='text/javascript'>    
   	const vm = new Vue({
         el:'#root',
         data: {
           numbers: {
             a: 1,
             b: 2
           }
         },
         watch: {
           'numbers.a': {
             handler() {
               console.log('a被改变了');
             }
           },
           // 监视多级结构中所有属性的变化
           'numbers': {
             // 开启深度监视
             deep: true,
             handler() {
               console.log('numbers被改变了');
             }
           }
         }
       })
    </script>
   ```

   `监视属性简写`

   ```vue
   <script type='text/javascript'>
     Vue.config.productionTip = false // 取消Vue的生产提示
   
     const vm = new Vue({
       el:'#root',
       data: {
         isHot: true
       },
       computed: {
         info() {
           return this.isHot ? '炎热' : '凉爽'
         }
       },
       methods: {
         changeWeather() {
           this.isHot = !this.isHot
         }
       },
       watch: {
       // 完整写法1
         // isHot: {
         //   // 初始化执行一次handler，默认为false
         //   // immediate: true,
         //   // 开启深度监视
         //   // deep: true,
         //   // isHot 发生改变时，handler被调用
         //   handler(newValue, oldValue) {
         //     console.log('isHot被修改',newValue, oldValue);
         //   }
         // },
       // 简写形式1
         // isHot(newValue, oldValue) {
         //   console.log('isHot被修改',newValue, oldValue);
         // }
       }
     })
     // 完整写法2
     vm.$watch('isHot', {
       // 初始化执行一次handler，默认为false
         // immediate: true,
         // 开启深度监视
         // deep: true,
         // isHot 发生改变时，handler被调用
         handler(newValue, oldValue) {
           console.log('isHot被修改',newValue, oldValue);
         }
     })
     // 简写形式2
     // vm.$watch('isHot', function (newValue, oldValue) {
     //   console.log('isHot被修改',newValue, oldValue);
     // })
   </script>
   ```

   **computed和watch的区别**

   1. computed能完成的功能，watch都能完成
   2. watch能完成的功能，computed不一定能完成，例如：watch可以完成一部操作

   > 1. 所被Vue管理的函数，最好写成普通函数，这样this的指向才是`vm`或者`组件实例对象`
   > 2. 所有不被Vue所管理的函数(如定时器的回调函数，ajax的回调函数，Promise的回调函数等)，最好写成ajax函数，这样this的指向才是`vm`或者`组件实例对象`

---

### 10. 绑定样式

1. class样式

   * 写法:class="xxx" xxx可以是字符串、对象、数组。

     1. 字符串写法适用于：类名不确定，要动态获取。

     1. 对象写法适用于：要绑定多个样式，个数不确定，名字也不确定。

     1. 数组写法适用于：要绑定多个样式，个数确定，名字也确定，但不确定用不用。

2. style样式

   		1.  :style="{fontSize: xxx}"其中xxx是动态值。
   		1.  :style="[a,b]"其中a、b是样式对象。

`绑定样式`

```html
<style>
  .basic{
    width: 400px;
    height: 100px;
    border: 1px solid black;
  }
  
  .happy{
    border: 4px solid red;;
    background-color: rgba(255, 255, 0, 0.644);
    background: linear-gradient(30deg,yellow,pink,orange,yellow);
  }
  .sad{
    border: 4px dashed rgb(2, 197, 2);
    background-color: gray;
  }
  .normal{
    background-color: skyblue;
  }

  .atguigu1{
    background-color: yellowgreen;
  }
  .atguigu2{
    font-size: 30px;
    text-shadow:2px 2px 10px red;
  }
  .atguigu3{
    border-radius: 20px;
  }
</style>
<body>
  <div id='root'>
    <!-- 绑定class样式--字符串写法，适用于：样式的类名不固定，需要动态指定 -->
    <div class="basic" :class="mood" @click="changeMood">{{name}}</div> <br><br>

    <!-- 绑定class样式--数组写法，适用于：绑定的样式个数名字不确定 -->
    <div class="basic" :class="classArr">{{name}}</div> <br><br>

    <!-- 绑定class样式--对象写法，适用于：绑定的样式个数名字确定，但是动态决定用不用 -->
    <div class="basic" :class="classObj">{{name}}</div> <br><br>

    <!-- 绑定style样式--对象写法 -->
    <div class="basic" :style="styleObj">{{name}}</div>
  </div>

  <script type='text/javascript'>
    Vue.config.productionTip = false // 取消Vue的生产提示

    const vm = new Vue({
      el:'#root',
      data: {
        name: '尚硅谷',
        mood: 'normal',
        classArr: ['atguigu1', 'atguigu2', 'atguigu3'],
        classObj: {
          atguigu1: false,
          atguigu2: false
        },
        styleObj: {
          fontSize: '30px',
          color: 'red'
        }
      },
      methods: {
        changeMood() {
          const arr = ['happy', 'sad', 'normal']
          let index = Math.floor(Math.random()*3)
          this.mood = arr[index]
        }
      },
    })
  </script>
</body>
```

---

### 11. 条件渲染

1. v-if

   * 写法：

     * v-if = "表达式"
     * v-else-if = "表达式"
     * v-else = "表达式"

   * 适用于切换频率比较低的场景

   * 特点：不展示的DOM元素直接被移除

     > v-if 和 v-else-if 和 v-else 一起使用时，结构不能被打断

2. v-show

   * 写法：v-show = "表达式"
   * 适用于切换频率较高的场景
   * 特点：不展示的DOM元素未被移除，只是使用样式被隐藏

3. 注意：使用 v-if 时，元素可能无法被获取到；而使用 v-show 一定可以获取到，

---

### 12. 列表渲染

1. 使用v-for指令
   1. 用于展示列表数据
   2. 语法：v-for = "(item, index) in xxx" :key = "yyy"
   3. 可遍历：数组，对象，字符串(低频)，指定次数(低频)

`基本列表`

```html
<body>
  <div id='root'>
    <!-- 遍历数组 -->
    <h2>{{title}}</h2>
    <ul>
      <li v-for="item in personArr" :key="item.id">{{item.name}}-{{item.age}}</li>
    </ul>

    <!-- 遍历对象 -->
    <h2>遍历对象</h2>
    <ul>
      <li v-for="(value, key) in car" :key="key">{{key}}--{{value}}</li>
    </ul>

    <!-- 遍历字符串 -->
    <h2>遍历字符串</h2>
    <ul>
      <li v-for="(char, index) in str" :key="index">{{char}}--{{index}}</li>
    </ul>
  </div>

  <script type='text/javascript'>
    Vue.config.productionTip = false // 取消Vue的生产提p示

    const vm = new Vue({
      el:'#root',
      data: {
        title: '人员列表',
        personArr: [
          {id: '001', name: '张三', age: '18'},
          {id: '002', name: '李四', age: '19'},
          {id: '003', name: '王五', age: '20'}
        ],
        car: {
          name: '擎天柱',
          price: '800w',
          color: 'red'
        },
        str: 'HelloWorld'
      }
    })
  </script>
</body>
```

2. **key的原理**

   面试题：react、vue中的key有什么作用？（key的内部原理）
   1. 虚拟DOM中key的作用：
   					key是虚拟DOM对象的标识，当数据发生变化时，Vue会根据【新数据】生成【新的虚拟DOM】, 
   					随后Vue进行【新虚拟DOM】与【旧虚拟DOM】的差异比较，比较规则如下：
   
      2.对比规则：
   				(1).旧虚拟DOM中找到了与新虚拟DOM相同的key：
   							①.若虚拟DOM中内容没变, 直接使用之前的真实DOM！
   							②.若虚拟DOM中内容变了, 则生成新的真实DOM，随后替换掉页面中之前的真实DOM。
   
   ​				(2).旧虚拟DOM中未找到与新虚拟DOM相同的key创建新的真实DOM，随后渲染到到页面。
   
   3. 用index作为key可能会引发的问题：
   							1. 若对数据进行：逆序添加、逆序删除等破坏顺序操作: 会产生没有必要的真实DOM更新 ==> 界面效果没问题, 但效率低。
   	
   2. 如果结构中还包含输入类的DOM：
   										会产生错误DOM更新 ==> 界面有问题。
   	
   4. 开发中如何选择key?:
   	      1.最好使用每条数据的唯一标识作为key, 比如id、手机号、身份证号、学号等唯一值。
   		  2.如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，仅用于渲染列表用于展示，使用index作为key是没有问题的。

`列表过滤`

```html
<body>
  <div id='root'>
    <h2>人员列表</h2>
    <input type="text" placeholder="请输入姓名" v-model="keyWord">
    <ul>
      <li v-for="item in newPersonArr" :key="item.id">{{item.name}}-{{item.age}}</li>
    </ul>
  </div>

  <script type='text/javascript'>
    Vue.config.productionTip = false // 取消Vue的生产提示

    const vm = new Vue({
      el:'#root',
      data: {
        keyWord: '',
        personArr: [
          {id:'001', name: '马冬梅', age: 18},
          {id:'002', name: '周冬雨', age: 20},
          {id:'003', name: '周杰伦', age: 25},
          {id:'004', name: '温兆伦', age: 24}
        ],
        // newPersonArr: []
      },
      // 用watch实现
      // #region
      // watch: {
      //   keyWord: {
      //     immediate: true,
      //     handler(newValue) {
      //         this.newPersonArr = this.personArr.filter((p) => {
      //           return p.name.indexOf(newValue) !== -1
      //       })
      //     }
      //   }
      // }
      // #endregion
      computed: {
        newPersonArr() {
          return this.personArr.filter((p) => {
            return p.name.indexOf(this.keyWord) !== -1
          })
        }
      }
    })
  </script>
</body>
```

`列表排序`

```html
<body>
  <div id='root'>
    <h2>人员列表</h2>
    <button @click="sortType = 2">年龄升序</button>
    <button @click="sortType = 1">年龄降序</button>
    <button @click="sortType = 0">原顺序</button>
    <input type="text" placeholder="请输入姓名" v-model="keyWord">

    <ul>
      <li v-for="item in newPersonArr" :key="item.id">{{item.name}}-{{item.age}}</li>
    </ul>
  </div>

  <script type='text/javascript'>
    Vue.config.productionTip = false // 取消Vue的生产提示

    const vm = new Vue({
      el:'#root',
      data: {
        keyWord: '',
        sortType: 0,
        personArr: [
          {id:'001', name: '马冬梅', age: 18},
          {id:'002', name: '周冬雨', age: 20},
          {id:'003', name: '周杰伦', age: 25},
          {id:'004', name: '温兆伦', age: 24}
        ]
      },
      computed: {
        newPersonArr() {
          const arr = this.personArr.filter((p) => {
            return p.name.indexOf(this.keyWord) !== -1
          })
          // 判断是否需要排序
          if(this.sortType) {
            arr.sort((p1, p2) => {
              return this.sortType === 2 ? p1.age - p2.age : p2.age - p1.age
            })
          }
          return arr
        }
      }
    })
  </script>
</body>
```

