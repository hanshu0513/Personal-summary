项目流程

1. 客户产生一个想法
2. 产品预测 设计原型图
3. 
    a.设计评估开发所需时间
    b.技术评估开发所需时间
4. 
    a.前端开发 技术选型 实现项目中的难点 基础项目的搭建 路由划分 复用组件的封装
    b.完成设计任务
    c.后端建立数据库 开发接口
5.  a.后端提供接口文档 前端接口联调 
    b.设计提供.psd文件 切图 前端实现排排版效果
6. 前端自测
    a.保证排版和设计图一样
    b.保证项目能跑通
7. 提测
    a.QA测试
    b.设计根据设计图第一轮验收
    c.产品根据原型图第一轮验收
8. 切预上线环境 准备真实数据 
9. 再次测试
    a.QA测试
    b.设计根据设计图第二轮验收
    c.产品根据原型图第二轮验收
10. 切上线环境 上线
11. 项目复盘 总结项目遇到的问题难点

##  三种跨域方式


1. 
   
    > vue.config.js文件中
    
    ```js
    module.exports={  
        devServer: {
            proxy: {
                "/api": {
                target: "http://bb.shoujiduoduo.com",
                pathRewrite: {"^/api" : ""}
                }
            }
        }
    }
    ```
    
2. cors后端跨域
    > egg项目中
    >
    > config/plugin.js
    
    ```javascript
    
    exports.cors = {
    enable: true,
    package: 'egg-cors',
    }
    
    ```
    
    >config.default.js
    
    ```javascript
    exports.cors = {
     origin: '*',
     allowMethods: 'GET,HEAD,PUT,POST,DELETE,PATCH'
    };
    ```
    
    
    
3. jsonp 利用src的跨域能力
    a.动态创建script标签
    b.给src赋值
    c.页面添加script标签
    d.回调函数
    
    ```js
     function ad_callback(data){
         console.log(data)
     }
    ```
    
##  移动端项目

1. 几种单位

   - rem     相对单位  相对的只是HTML根元素的字体大小 font-size
   - em      相对单位   相对父元素的字体大小
   - px        逻辑单位
   - pt         物理单位 

2. vue项目中如何使用rem

   1. ```shell
      yarn add postcss-pxtorem -D 
      ```

   2. >postcss.config.js文件 把px=>rem

      ```js
      module.exports = { 
          plugins: { 
          'autoprefixer':
              { 
          overrideBrowserslist: ['Android >= 4.0', 'iOS >= 7'] },
          'postcss-pxtorem': { 
          rootValue: 16,//结果为：设计稿元素尺寸/16，比如元素宽320px,最终页面会换算成 20rem 
          propList: ['*'] } 
          }
      }
      ```

   3. > src 目录下新建utils/rem.js 切换分辨率时自动适配

      ```js
      // 设置 rem 函数
      function setRem () {
      
          // 320 默认大小16px; 320px = 20rem ;每个元素px基础上/16
          let htmlWidth = document.documentElement.clientWidth || document.body.clientWidth;
          //得到html的Dom元素
          let htmlDom = document.getElementsByTagName('html')[0];
          //设置根元素字体大小
          htmlDom.style.fontSize= htmlWidth/20 + 'px';
      }
      // 初始化
      setRem();
          // 改变窗口大小时重新设置 rem
      window.onresize = function () {
          setRem()
      }
      ```

      

   4. > src目录下新建utils/rem.js在main.js中引入

      ```js
      import './utils/rem.js';
      ```

##  代码规范 eslint代码检查工具

1. 配置规则： .eslintrc.js 或 package.json

2. process.env.NODE_ENV 开发环境/线上环境  获取环境

   - set NODE_ENV=production  设置线上环境

   - set NODE_ENV=development  设置开发环境


##  目录规范

  |-- src 
    |-- App.vue     根组件
    |-- main.js     入口文件
    |-- api         请求api
    |-- assets      静态资源
    |-- components  公用组件
    |-- directives  自定义指令
    |-- utils       公用的方法       
    |-- mixins      mixin
    |-- router      路由
    |-- store       vuex
    |   |-- index.js
    |-- views       视图组件

##  真机测试

1. 确保电脑和手机在同一个局域网内
2. 通过ipcongfig查看ip  IPv4 
3. 草料二维码生成二维码 /二维码插件				

##  分析需求

1. 划分模块

2. 划分路由

3. 划分组件
   -   视图组件（不进行复用，只负责展示视图）
   - 逻辑复用组件 element-ui tab组件 弹框组件
   - 业务复用组件 ajax请求


##    路由守卫

- 全局守卫
- 组件内守卫
- 路由内守卫

##     自动化注册全局组件

**require.context()** webpack提供的api 加载目录的 编译时加载

- 参数

1. 路径
2. 是否递归文件 true/fasle
3. 匹配的文件

```js
import Vue from "vue";
//利用require.context处理components下的组件
let files=require.context("./",true,/index.vue$/);
//遍历 挂载到vue上
files.keys().forEach(item=>{    
    let name=files(item).default.name;
    Vue.component('my-'+name,files(item).default)
})

```

```js

import Vue from "vue"
//自动化获取目录下的文件
let files = require.context("@/components", true, /index\.(js|vue)$/)
//抛出
export default () => {
    console.log(files.keys());
    //遍历
    files.keys().forEach(item => {
        //获取对象
        let file = files(item).default;
        //获取name
        let name = files(item).default.name
        //是.js 使用插件
        if (typeof file === "function") {
            Vue.use(file)
            //是.vue 挂载全局
        } else {
            Vue.component("my-" + name, file)
        }

    })
}
```





## 自动化加载API

```js
import Vue from "vue"
//自动化引入所有api模块
let files=require.context("./",false,/\.js$/);
//把函数统一解构
let $api=files.keys().filter(item=>!item.includes("index")).reduce((prev,next)=>{
    return{
        ...prev,...files(next).default
    }
},[])
//统一挂载请求到$http上
Vue.prototype.$http=$api;

```

##  http:超文本传输协议

​       *浏览器和服务器通信的协议*

1. ####       同意的资源标识 http://localhost:9090/api/list?id=1#top

   - *http* 协议
   - *localhost* 域名 
   - *9090* 端口号
   - 服务器 没有显示器的电脑

2. #### 如何发起http请求

   - #### script的src属性  img的src属性  a的href属性

   - #### ajax 

   - #### fetch

5. 


## vue组件传参

1. #### 父组件向子组件传参

   ####    *父组件通过属性的方式向子组件传值，子组件通过props来接受*

   1. 在父组件的子组件标签中绑定自定义属性

   2. 在子组件中使用props(可以是数组也可以是对象)接受即可   

       **子组件中不能直接修改父组件的值,简单数据类型可拷贝后修改,引用类型不要轻易修改,父组件中也会改变

2. #### 子组件向父组件传参

   - 子组件绑定一个事件 通过this.$emit()来触发

     > 子组件

     ```javascript
     <button @click="changeParentName">改变父组件的name</button>
     export default {
         methods: {
             //子组件的事件
             changeParentName: function() {
                 this.$emit('handleChange', 'Jack') // 触发父组件中handleChange事件并传参Jack
                 // 注：此处事件名称与父组件中绑定的事件名称要一致
             }
       
     ```

     > 父组件中定义并绑定

     ```javascript
     <child @handleChange="changeName"></child>
     methods: {
         changeName(name) {  // name形参是子组件中传入的值Jack
             this.name = name
         }
     }
     ```

   - 通过callback函数

     > 先在父组件中定义一个callback函数，并把 callback 函数传过去 

     ```javascript
     <child :callback="callback"></child>
     methods: {
         callback: function(name) {
             this.name = name
         }
     }
     ```

     > 在子组件中接收 并执行callback函数

     ```javascript
     <button @click="callback('Jack')">改变父组件的name</button>
     props: {
         callback: Function,
     }
     ```

   - 通过$parent/$children 或 $refs访问组件实例

     > 子组件

     ```javascript
     export default {
       data () {
         return {
           title: '子组件'
         }
       },
       methods: {
         sayHello () {
             console.log('Hello');
         }
       }
     }
     ```

     > 父组件

     ```js
     <template>
       <child ref="childRef" />
     </template>
     <script>
       export default {
         created () {
           // 通过 $ref 来访问子组件
           console.log(this.$refs.childRef.title);  // 子组件
           this.$refs.childRef.sayHello(); // Hello
           
           // 通过 $children 来调用子组件的方法
           this.$children.sayHello(); // Hello 
         }
       }
     </script>
     ```

     *注意 : 这种方式的组件通信不能跨级*

   - $arrs/$listeners  通常配合 inheritAttrs 一起使用  默认值为true

     **$attrs**：包含了父作用域中不被认为 (且不预期为) props 的特性绑定 (class 和 style 除外)，并且可以通过 v-bind=”$attrs” 传入内部组件。当一个组件没有声明任何 props 时，它包含所有父作用域的绑定 (class 和 style 除外)。

     **$listeners**：包含了父作用域中的 (不含 .native 修饰符) v-on 事件监听器。它可以通过 v-on=”$listeners” 传入内部组件。它是一个对象，里面包含了作用在这个组件上的所有事件监听器，相当于子组件继承了父组件的事件。

     > father.vue

     ```javascript
     <template>
     　　 <child :name="name" :age="age" :infoObj="infoObj" @updateInfo="updateInfo" @delInfo="delInfo" />
     </template>
     <script>
         import Child from '../components/child.vue'
     
         export default {
             name: 'father',
             components: { Child },
             data () {
                 return {
                     name: 'Lily',
                     age: 22,
                     infoObj: {
                         from: '上海',
                         job: 'policeman',
                         hobby: ['reading', 'writing', 'skating']
                     }
                 }
             },
             methods: {
                 updateInfo() {
                     console.log('update info');
                 },
                 delInfo() {
                     console.log('delete info');
                 }
             }
         }
     </script>
     ```

     > child.vue

     ```javascript
     <template>
         <grand-son :height="height" :weight="weight" @addInfo="addInfo" v-bind="$attrs" v-on="$listeners"  />
         // 通过 $listeners 将父作用域中的事件，传入 grandSon 组件，使其可以获取到 father 中的事件
     </template>
     <script>
         import GrandSon from '../components/grandSon.vue'
         export default {
             name: 'child',
             components: { GrandSon },
             props: ['name'],
             data() {
               return {
                   height: '180cm',
                   weight: '70kg'
               };
             },
             created() {
                 console.log(this.$attrs); 
     　　　　　　　// 结果：age, infoObj, 因为父组件共传来name, age, infoObj三个值，由于name被 props接收了，所以只有age, infoObj属性
                 console.log(this.$listeners); // updateInfo: f, delInfo: f
             },
             methods: {
                 addInfo () {
                     console.log('add info')
                 }
             }
         }
     </script>
     ```

     > grandSon.vue

     ```javascript
     <template>
         <div>
             {{ $attrs }} --- {{ $listeners }}
         <div>
     </template>
     <script>
         export default {
             ... ... 
             props: ['weight'],
             created() {
                 console.log(this.$attrs); // age, infoObj, height 
                 console.log(this.$listeners) // updateInfo: f, delInfo: f, addInfo: f
                 this.$emit('updateInfo') // 可以触发 father 组件中的updateInfo函数
             }
         }
     </script>         
     ```

3. #### 兄弟组件间传参

   - $emit和props结合

   - $bus

     > EventBus.js 暴露一个vue实例

     ```javascript
     import Vue from 'Vue'
     export default new Vue()
     ```

     在main.js/使用组件的中引入该文件

     

     > 组件内

     ```javascript
     EventBus.$emit("事件名",参数)
     ```

     

     > 接收的组件内

     ```js
     EventBus.$on("事件名",(参数)=>{})
     ```

   - vuex

4. #### 多层父子组件通信

   #### provide/inject

   *简单来说就是在父组件中通过provider来提供变量，然后在子组件中通过inject来注入变量，不管组件层级有多深，在父组件生效的生命周期内，这个变量就一直有效*

   > 父组件 

   ```js
   export default {
     provide: { // 它的作用就是将 **name** 这个变量提供给它的所有子组件。
       name: 'Jack'
     }
   }
   ```

   > 子组件

   ```js
   export default {
     inject: ['name'], // 注入了从父组件中提供的name变量
     mounted () {
       console.log(this.name);  // Jack
     }
   }
   ```

   *注：**provide 和 inject 绑定并不是可响应的**。即父组件的name变化后，子组件不会跟着变*

   

   #### 总结

   -  父子通信： 父向子传递数据是通过 props，子向父是通过 $emit；通过 $parent / $children 通信；$ref 也可以访问组件实例；provide / inject ；[$attrs / $listeners](https://www.cnblogs.com/dhui/p/12931953.html)； 

   -  兄弟通信： EventBus；Vuex； 

   -  跨级通信： EventBus；Vuex；provide / inject ；[$attrs / $listeners](https://www.cnblogs.com/dhui/p/12931953.html)； 

## Vue生命周期

1. #### beforeCreate    实例创建前

2. #### created             实例创建完成后立即调用 

     绑定data methods computed 到this上

3. #### baforeMonut   挂载前 

4. #### mounted          挂载完成   

     可以访问到真实dom

5. #### boforeUpdate  数据更新前 

6. #### updated            数据更新完成

7. #### beforeDestory  实例销毁前 

8. #### destoryed          实例销毁后

     ###### <font color=red> 补充两个：</font>

9. #### activated          被 keep-alive 缓存的组件激活时调用 

10. #### deactivated      被 keep-alive 缓存的组件停用时调用 

 ![img](https://user-gold-cdn.xitu.io/2020/4/29/171c6658166b75eb?imageslim) 
## vue中内置的方法 属性运行顺序
   （methods、computed、data、watch、props)

​     ![img](https://user-gold-cdn.xitu.io/2020/4/29/171c66a19596ecdb?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)      

​            `props` ->`methods` ->`data` -> `computed` -> `watch`
## vm.$nextTick( [callback] )

​     将回调延迟到下次 DOM 更新循环之后执行。在修改数据之后立即使用它，然后等待 DOM 更新。它跟全局方法 `Vue.nextTick` 一样，不同的是回调的 `this` 自动绑定到调用它的实例上 可在mouted和updated中调用

## 封装mixin 上拉加载

> mixins/loadScroll.js

```js
//引入防抖节流函数
import debounce from "@/utils/util";
export default {
    methods: {
        
        loadScroll(scrollbox, callback) {
            //父元素盒子的高度
            let clientHeight = scrollbox.clientHeight;
            //内容的高度
            let pageHeight = this.$refs.pagelist.offsetHeight;
            scrollbox.scrollFun = debounce(() => {
                //scrollbox.scrollTop为盒子滚动的距离    
                if (clientHeight + scrollbox.scrollTop >= pageHeight) {
                    //  盒子的高度 + 盒子滚动的距离  >=内容部分高度 就触发回调函数
                    //需要判断回调函数是否存在
                    callback && callback()
                }
            }, 1000)
            //父元素添加滚动监听 滚动调用该函数
            scrollbox.addEventListener('scroll', scrollbox.scrollFun)
        },
        beforeDestroy() {
            //获取父元素
            let scrollbox = this.$refs.pagelist.parentNode;
            //销毁时移除滚动监听函数
            scrollbox.scrollFun && scrollbox.removeEnentListener("scroll", scrollbox.scrollFun)
        },
    },
}

```

> 在需要的组件中 引入 

```js
import loadscroll from '@/mixins/loadScroll'
```

> 加载mixin文件

```js
 mixins:[loadscroll]
```

> 在 mounted中引入

```js
 let scrollBox=this.$refs.pagelist.parentNode;
     this.loadScroll(scrollBox,()=>{
       this.pagesize+=10;
       this.getList();
     })  
```

## vue中路由传参方式

- query  会显示在url后面

  ```js
  this.$router.push({
      path:"/detail",
      query:{ }
  })
  ```

-  params 

  ```js
  this.$router.push({
  path:"/detail",
    params:{ }
  })
  ```

  

-  params  动态路由 

  1. 路由后面绑定参数

     ```js
     path:"/detail/:id"
     ```

  2. 传参

     ```js
     this.$router.push(`/detail/${id}`)
     ```

## vue中使用ref

> 在元素节点上

```js
<div ref="divDom"><div>
```

> 在mouthed中通过refs获取

```js
this.$refs.divDom
```

## VUEX
“单向数据流”理念的简单 

![1597630234211](C:\Users\Suejiang\AppData\Roaming\Typora\typora-user-images\1597630234211.png)

![1597630436488](C:\Users\Suejiang\AppData\Roaming\Typora\typora-user-images\1597630436488.png)


## VUE extend注册组件

> alert.vue

```js
<template>
    <div class="alert-box" v-if="isShow">
         {{tip}}
    </div>
</template>

<script>
export default {
    name:'alert',
    props:["tip","time"],
    data() {
        return {
            timer:null,
            isShow:true
        }
    },
    mounted(){
       this.timer= setTimeout(()=>{
            this.isShow=false
       },this.time*1000)
    },
    beforeDestroy(){
        clearTimeout(this.timer)
    }
}
```

> plugins/alert.js

```js
import Vue from "vue";
import MyAlert from "@/components/alert"

let $alert =(tip,time=3)=>{
  let com =Vue.extend(MyAlert);

  let instance= new com({
      propsData:{
          tip,
          time
      }
  })
  instance.$mount();

  document.body.appendChild(instance.$el)
}
export {$alert as myAlert}
export default {
     install(Vue){
         Vue.prototype.$myAlert=$alert;                 
     }
}

```

> main.js 需要安装

```js
import alert from  "@/plugins/alert"
Vue.use(alert)
```

> .vue 调用

```js
this.$myAlert()
```

## VUE 自定义指令 directive

> directives/timeupdate.js

```js
import Vue from 'vue';
const timeupdate={
    //只在绑定元素的时候绑定一次
    bind(el,config,vnode){
        let {value}=config;
        console.log(value);
        el.currentTime=value/1000;
        el.fun=(e)=>{       
          vnode.context.curTime=e.target.currentTime*1000;
          
        }
        el.addEventListener("timeupdate",el.fun)   
    },
    updated(el,config) {
       console.log("===",el,config);
        let {value}=config;
      el.currentTime=value/1000;
     },
    update(el,config,vnode){
        
        el.removeEventListener("timeupdate",el.fun)   
    }
         
}
//全局注册指定
Vue.directive("timeupdate",timeupdate)
```

> 组件中元素上

```js
<audio ref="aud" src="http://cdnbbbd.shoujiduoduo.com/bb/audio/a48/141/3577141.aac   " controls v-timeupdate="curTime"></audio>
```

> /main.js

```js
import "@/directives/timeupdate"
```

## 播放器歌词加载

> 歌词部分处理

```js
//created中获取数据 
let res = await request.get("/getlyric");
    this.lyricList = this.format(res);
 
 //封装方法处理歌词数据 也就是格式化
    format(str) {
      //把尖括号部分替换成空格
      str = str.replace(/\<\d+\,\-?\d+\>/g, "");
      //把时间和歌词分组
      let reg = /\[(\d{2}\:\d{2}\.\d{2})\]([^\[]+)/g;
      let list = [];
      str.replace(reg, ($1, $2, $3) => {
        //$1指全部 $2指分组1 $2指分组2
        //切割分秒
        let [f, m] = $2.split(":");
        //拼接到list数组
        list.push({
          time: parseInt(parseInt(f * 60 * 1000) + parseFloat(m * 1000)),
          text: $3,
        });
       
      });
      //防止bug  拼接一个无限小数
       list.push({
          time:Infinity,
          text:""
        })
        //返回数组
      return list;
```

> 外部html 

```js
<div class="dialog">
    <h3 @click="$emit('dialogclose')">&lt;</h3>
      <audio ref="aud" src="http://cdnbbbd.shoujiduoduo.com/bb/audio/a48/141/3577141.aac
              " controls v-timeupdate="curTime" class="src"></audio>
    <div class="box" ref="content" >
      <ul >
        <li
          v-for="(item,i) in lyricList"
          :key="i"
          :ref="'lis'+i"
           :class="{'active':curIndex===i}"
          @click="change(item.time,i)"
        >{{item.text}}</li>
      </ul>
    </div>   
  </div>
```

> 监听curIndex curTime

```js
data() {
    return {
      curTime: 0,
      curIndex: -1,
      lyricList: [],
    };
  },
  watch: {
    curTime(val) {
      this.lyricList.forEach((item, key) => {
        if (item.time <= val && val < this.lyricList[key + 1].time) {
          this.curIndex = key;
        }
      });
    },
   curIndex(val){
      //获取盒子高度
      let contentHeight=this.$refs.content.offsetHeight     
      //获取当前歌词距离顶部的距离
      let curoffsetTop=this.$refs['lis'+val][0].offsetTop      
        this.$refs.content.scrollTop=(curoffsetTop-contentHeight/2-this.$refs.content.offsetTop)
    }
  },
```

> 绑定的方法

```js
 change(time,i) {
      this.curTime = time;
      this.curIndex=i;     
        this.$refs.aud.currentTime=time/1000      
    },
```

## 配置webpack scss

```js
css: {
    loaderOptions: {
      scss: {
        prependData: `@import "~@/assets/scss/index.scss";`
      },
    }
  }

```

## 高德地图api 定位  拖拽 手动定位

> vue.config.js 配置 externals

```js
configureWebpack: {
    externals: {
      'AMap': 'AMap',
    'AMapUI': 'AMapUI'
    }
  },
```

> utils map.js

```js
import AMap from 'AMap'
/**
 * @function getGeolocation
 * @param {*} config 
 */
// 高度定位
export const getGeolocation = function (config = {}) {
  return new Promise((res, rej) => {
    AMap.plugin('AMap.Geolocation', function () {
      var geolocation = new AMap.Geolocation({
        // 是否使用高精度定位，默认：true
        enableHighAccuracy: true,
        // 设置定位超时时间，默认：无穷大
        timeout: 3000,
        ...config
      })
      geolocation.getCurrentPosition()
      AMap.event.addListener(geolocation, 'complete', onComplete)
      AMap.event.addListener(geolocation, 'error', onError)
      function onComplete(data) {
        // data是具体的定位信息
        res(data)
      }

      function onError(data) {
        // 定位出错
        rej(data)
      }
    })

  })
}
//手动定位
export const auto = function (keyword, city = '北京') {
  return new Promise((resolve, reject) => {
    AMap.plugin('AMap.Autocomplete', function () {
      // 实例化Autocomplete
      var autoOptions = {
        //city 限定城市，默认全国
        city
      }
      var autoComplete = new AMap.Autocomplete(autoOptions);
      autoComplete.search(keyword, function (status, result) {
        // 搜索成功时，result即是对应的匹配数据
        if (status === "complete") {
          //成功
          resolve(result)
        } else {
          //失败
          reject(result)
        }
      })
    })
  })
}

```

> 自动定位

```js
import {getGeolocation} from '@/utils/map'
 async  created() {       
       try {
           let positionData=await getGeolocation();
           localStorage.adress=positionData.formattedAddress;
           this.$router.push("/main")
       } catch (error) {
            console.log(error);
            this.$router.push("/setpost")
       }
    },
```

> 手动定位 输入提示与POI搜索

```js
import { auto } from "@/utils/map";
async keyword() {
      if (this.keyword) {
        let res = await auto(this.keyword);
        this.tiplist = res.tips;
      } else {
        this.tiplist = [];
      }
    },
```

> 拖拽定位 

```js
import AMap from "AMap";
import AMapUI from "AMapUI";
async mounted() {
    AMapUI.loadUI(["misc/PositionPicker"], (PositionPicker) => {
      var map = new AMap.Map("container", {
        resizeEnable: true,
        zoom: 16,
        scrollWheel: false,
        city:"北京"
      });
      var positionPicker = new PositionPicker({
        mode: "dragMap", //设定为拖拽地图模式，可选'dragMap'、'dragMarker'，默认为'dragMap'
        map: map, //依赖地图对象
      });
      //TODO:事件绑定、结果处理等
      positionPicker.on("success", (positionResult) => {
        this.piosList = positionResult.regeocode.pois.slice(0,10);
        
      });
      positionPicker.on("fail", (positionResult) => {
        // 海上或海外无法获得地址信息
         this.piosList = positionResult.regeocode.pois.slice(0,10);
      });
      positionPicker.start();
      map.panBy(0, 1);
    });
  },
  methods: {
    toHome(val){
      localStorage.adress = val.name + val.address;
     let msg=val.name + val.address;
      this.$router.push({path:"/setadress",query:{msg}})      
    }
  },
```

## rbac权限

> 权限表  侧边栏菜单 asideMenu.js

```js
[
    {
        name: "outline", //tab
        title: "概要",   
        identity: ["admin", "consumer"], //tab对应权限
        children: [
            //二级路由
            {
                name: "outline",//对应二级路由的name
                title: "今日概要",
                identity: ["admin", "consumer"],//对应二级路由的权限
            }
        ]
    },
     {
        name: "banner",
        title: "banner管理",
        identity: ["admin"],
        children: [
            {
                name: "banner",
                title: "banner列表",
                identity: ["admin"],
            }
        ]
    }, 
    {
        name: "product",
        title: "商品管理",
        identity: ["admin", "consumer"],
        children: [
            {
                name: "productlist",
                title: "商品列表",
                identity: ["admin", "consumer"],
            },
            {
                name: "addProduce",
                title: "新增商品",
                identity: ["consumer"],
            },
            {
                name: "productTypes",
                title: "商品分类",
                identity: ["admin"],
            }
        ]
    },    
]
```

> 二级路由表

```js
[
        {
          path:"/main",
          name:"shouye",
          redirect:"/main/outline"
        },
        {
          //今日概要
          path:"/main/outline",
          name:"outline",
          meta:{title:"概要"},
          component: () => import(/* webpackChunkName: "main/outline" */ '../views/main/outline'),
        },
        {
          //轮播
          path:"/main/banner",
          name:"banner",
          meta:{title:"轮播图管理"},
          component: () => import(/* webpackChunkName: "main/banner" */ '../views/main/banner'),
        },
        {
          //商品列表
          path:"/main/productlist",
          name:"productlist",
          meta:{title:"商品管理"},
          component:()=>import(/* webpackChunkName: "main/product" */ '../views/main/productlist'),       
        },
        {
          //商品增加
          path:"/main/addProduce",
          name:"addProduce",
          meta:{title:"添加商品"},
          component:()=>import(/* webpackChunkName: "main/product" */ '../views/main/addProduce'),       
        },
        {
          //商品分类
          path:"/main/productTypes",
          name:"productTypes",
          meta:{title:"添加商品种类"},
          component:()=>import(/* webpackChunkName: "main/product" */ '../views/main/productTypes'),       
        },
        {
          //订单列表
          path:"/main/orderlist",
          name:"orderlist",
          meta:{title:"订单列表"},
          component:()=>import(/* webpackChunkName: "main/order" */ '../views/main/orderlist'),   
        },
        {
          //售后管理
          path:"/main/afterSale",
          meta:{title:"售后管理"},
          name:"afterSale",
          component:()=>import(/* webpackChunkName: "main/order" */ '../views/main/afterSale'), 
        },
        {
          //评价系统
          path:"/main/evaluation",
          name:"evaluation",
          meta:{title:"评价系统"},
          component:()=>import(/* webpackChunkName: "main/order" */ '../views/main/evaluation'), 
        } 
    ]

```

> store/user.js

```js
//过滤视图
function filterView(name){
   return identityAsideMenu.filter(item=>item.identity.includes(name)).map(item=>{
      item.children= item.children.filter(val=>{
          return val.identity.includes(name)
      })
      return item
  })
  
}
//筛选路由信息
function format(data){   
    return data.map(item=>{  
       let children= item.children.map(val=>{           
          let {path,meta}=  mainChildren.find(info=>info.name===val.name)
            return{...val, path, meta}
        })
        return { ...item,children }
    })
}
```

## 全局路由守卫

> 设置页面title

```js
 document.title= to.matched[0].meta.title||'多点超市'; 
```

> 登录验证 实现跳转回跳来的路由

```js
if(to.meta.isAuthority){
     let token=localStorage.token;
     if(token){
       next()
     }else{
      next({
        name:"login",
        query:{
          preRoute:to.path
        }
      })
     }
   }else{
     next()
   }
```

> login.vue

```js
if (res.code === 1) {
     // 登录成功存储token
     localStorage.token = res.data.token;
     //触发 存储user_id
     this.$store.dispatch("SET_USER_ID")
     //跳转转来的路由或者main
     let path=preRoute?preRoute:'/main'
     //跳转
     this.$router.replace(path);
}
```

## vue transition

> 1 用transition包裹组件

```js
<transition
    enter-active-class="animate__animated animate__fadeIn"
    leave-active-class="animate__animated animate__fadeOut"
  >
    <div class="modal-wrap" v-show="show" @click.self="$emit('close')">
      <transition
        enter-active-class="animate__animated animate__fadeIn"
        leave-active-class="animate__animated animate__fadeOut"
      >
        <div class="modal-content" v-show="show" >
          <slot>
            <ul class="address-box">
              <li v-for="(item,index) in tiplist " :key="index" @click="change(item)">
                <p>{{item.district.slice(0,2)}}{{item.name}}</p>
                <p>{{item.district}}</p>
              </li>
            </ul>
          </slot>
        </div>
      </transition>
    </div>
  </transition>
```

> 2.类名

- enter-active-class 进入动画类名
- leave-active-class  退出动画类名

## animate 动画

- 下载

  ```js
  yarn add animate.css
  ```

- 引入

- class=animate __ animated  animate __ fadeOut


## keep-alive

- keep-alive 包裹组件
-  include--->需要缓存的组件 and exclude --->不需要缓存的组件
-  actived 里触发不需要缓存的赋值
-   max 最大缓存数

## slot

- 具名插槽
- 默认插槽

## v-model原理

- input上

  ```js
  <input v-model="sth" />
  <input v-bind:value="sth" v-on:input="sth = $event.target.value" />
  <input :value="sth" @input="sth = $event.target.value" />
  ```

- 组件上

  > 子组件

  ```js
  <input 
          type="checkbox"
          @change="$emit('input', $event.target.checked)"
          :checked="value"
   />
   props: ['value'],
  ```

  >  父组件

  ```js
  <currency-input v-model="price"></currentcy-input>
  ```



## 图片上传

> 借助emement-ui

```js
 <el-upload
      class="avatar-uploader"
      action="fakeaction"
      :show-file-list="false"
      :http-request="upLoadImage" //因为跨域 用http-request代替默认的上传方式
 >
      <img v-if="bannerData.image" :src="bannerData.image" class="avatar" />
      <i  v-else class="el-icon-plus avatar-uploader-icon"></i>
    </el-upload>
```

> 借助FormData对象

```js
 async upLoadImage(e) {
      //图片的文件信息
      let file = e.file;
      //借助FormData对象上传文件 
      let formData = new FormData();
      //FormData携带图片对象
      formData.append("file", file);
      //请求接口 图片对象以二进制流的方式传递
      let res = await this.$http.upImage(formData);
      //展示图片
      if(res.code===1){       
         this.bannerData.image = process.env.VUE_APP_IMAGEURL + res.data.filename;
          
        //  根据数据刷新节点 不然无法及时更新视图 原因===>可能是组件嵌套太深
          this.$forceUpdate()
      }
     
```

> 图片地址处理 .env.production

```js
VUE_APP_IMAGEURL=http://localhost:7002
```

> .env.development

```js
VUE_APP_IMAGEURL=http://localhost:7002
```



## 添加和编辑 公用弹框处理

> 编辑传递props参数 添加处理为空

```js
 <bannerDialog
      v-if="dialogVisible" //不加会有显示bug
      :dialogVisible="dialogVisible"
      @close="closeFun"
      :propobj="propobj"
    ></bannerDialog>

 //编辑banner
    async handleEdit(obj) {
      this.propobj = obj; //
      this.dialogVisible = true;
    },
        
  //增加banner
    addBanner() {
      this.dialogVisible = true;
    },
    //关闭弹框
    async closeFun() {
      this.dialogVisible = false;
      this.getbannerList();
      this.propobj = {}; //清空反显
    },     
        
```

> 请求接口处理

```js
 async submit() {
      //id存在 则为编辑
      let fun=this.propobj.id?'editBanner':'addBanner'
      //读取信息
      let {active_name, image, url,start_time,end_time } = this.bannerData;      
      let flag=this.propobj.id    
      // 为编辑则拼接id
      let id= flag?{id:flag}:null;
      // 请求对应接口
      let res = await this.$http[fun]({
          image,
          start_time,
          end_time,
          user_id: 6,
          active_name,
          url,
          ...id
        });
        let str=(flag?'编辑':"增加")
        if (res.code === 1) {
          this.$myAlert(str+"成功");
          this.$emit("close");//触发 关闭弹框事件
        }else{
          this.$myAlert(str+"失败");
        }
    },
```

