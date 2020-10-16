## react-app 生成

1. yarn add create-react-app -g
2. yarn eject 释放配置
3. yarn start 运行项目

## react-app 配置移动端适配

> utils>rem.js

// 设置 rem 函数

```js
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



> 下载插件

```shell
yarn add postcss-pxtorem --dev      
```

> 使用插件 webpack.config.js

```js
require("postcss-pxtorem")({
              rootValue: 16,
              propList: ['*'],

 }),
```

## 配置别名

```js
 alias: {
        "@": path.resolve("src"),
        "static": path.resolve("src/static")
  }
```

## 封装路由 路由懒加载

> router/RouterView.js

```js
import React from 'react'
import {Switch,Redirect,Route} from "react-router-dom"

/**
 * 
 * @param {*} props 参数
 */
export default function RouterView(props) { 
    // 分别筛选组件路由和重定向路由
    let components=props.routes.filter(item=>item.component);
    let redirects=props.routes.filter(item=>item.redirect);
    return (
        <Switch>
            {
                //遍历处理路由
                components.map(item=>{                    
                    return <Route path={item.path} key={item.path} render={(propsRoute)=>{
                        // 处理children 
                        let children=item.children?{routes:item.children}:{}
                        return <item.component {...propsRoute} {...children}></item.component>
                    }}></Route>
                })
            }
            {
                // 遍历处理重定向
                redirects.map((item,index)=>{
                    return <Redirect to={item.redirect} key={index}></Redirect>
                })
            }
        </Switch>
    )
}

```

> router/RouterConfig.js

```js
//路由懒加载的模块
import loadable from "react-loadable"
import React from "react"
//路由表
function Loading(){
    return <div>loading...</div>
}
const routes=[
    {
        path:"/",
        redirect:"/home"
    },
    {
        path:"/home",
        component:loadable({
            loader:()=>import("@/view/home"),//懒加载组件
            loading:Loading//loading动画
        })       
    },{
        path:"/car",
        component:loadable({
            loader:()=>import("@/view/car"),
            loading:Loading
        }) 
    },{
        path:"/color",
        component:loadable({
            loader:()=>import("@/view/color"),
            loading:Loading
        }) 
    }
]
export default routes;
```

## 自动加载reducer

```js
function getReduce(){
    let files=require.context("./reducers/",true,/\.js$/);
    return files.keys().reduce((prev,cur)=>{
        let key=cur.match(/\.\/(\w+)\.js$/)[1]
        prev[key]=files(cur).default
        return prev
    },[])
}
let store =createStore(combineReducers(getReduce()),applyMiddleware(thunk))
```

## style.module.scss

 webpack提供的 

```react
import style from "./style.module.scss"
 <div className={style["list-content"]}></div>
```

![image-20200910191704717](C:\Users\Suejiang\AppData\Roaming\Typora\typora-user-images\image-20200910191704717.png)

## 拆分action

>  使用的组件内

```js
//bindActionCreators是redux的一个自带函数，作用是将单个或多个ActionCreator转化为dispatch(action)的函数集合形式
import { bindActionCreators } from "redux"
//引入全部的action
import * as Actionshome from "@/store/actions/home"
//映射全部dispatch到this上 且拆分出来的action携带dispatch
(dispatch) => {
    return bindActionCreators(Actionshome, dispatch)
}
```

> store下的home action

```js
//引入type
import * as actionType from "@/type/actionType"
import api from "../../api/index"
export const setList = () => {
    return (dispatch) => {
        api.getList().then(res =>
             //diapatch action给reducer
            dispatch({
                type: actionType.setListType,
                data: res.data
            }),
        )
    }
}
```

> reducer

```react
import * as actionType from "@/type/actionType"
export default (state={datalist:{}},action)=>{
    switch (action.type){
            //给datalist赋值
        case actionType.setListType:{
            let {data}=action
            //格式化数据
            let keys = [...new Set(data.map(item => item.Spelling.substr(0, 1)))];
            let result = keys.reduce((prev, key) => {
                prev[key] = data.filter(item => item.Spelling.substr(0, 1) === key);
                return prev
            }, {})
            return{
                ...state,
                datalist:result

            }
        }
        default:{
            return state
        }
    }
}
```

> 对象渲染 Object.keys(obj)   Object.values(obj)

```js
<div className="home-main">
       <LazyImage className="container" ref={el => this.scrollWrap = el}
          options={{
               imgSrc:"data-src"
             }}
               ref="scrollImg"
   >
         <section>
            {
              Object.keys(datalist).map(itemLetter => {
                   return ...
               })
            }  </section>
      </LazyImage>
 <div className="letters">   
         <span onClick={() => this.scrollTop()}>#</span>
            {
             Object.keys(datalist).map((itemLetter,index) => {
                return ...
               })
    }
                  
 </div>
```

## 点击字母列表滑动到对应字母标题位置

> 点击的span加click事件

```js
 <span
   ind={index}
   onClick={(e) => {
   this.scrollToElement(e)
  }}
>{itemLetter}</span>
```

> scrollToElement

```js
scrollToElement=(e)=>{
      //获取自定义属性的值
      let ind=e.target.getAttribute("ind");
      //把自定义属性的值传递给LazyImage组件里
      this.refs.scrollImg.scrollToElement(ind);
}
```

> LazyImage组件里

```js
 scrollToElement =(ind)=>{
     //获取所有盒子 取到点击字母索引的盒子
        let divs=this.scrollImg.querySelectorAll("div")[ind];
     //使container滚动到 子盒子距offsetParent顶部的位置 
        this.scrollImg.scrollTop=divs.offsetTop;
    }
```



## 图片懒加载—>滑动到可视区才加载

> 封装 LazyImage组件 父组件中使用

```js
  <LazyImage className="container"}
  ref="scrollImg" 
    //传递当前图片的data-src属性值
      options={{imgSrc:"data-src"}}
  >
    <section>
     {...
      <img className="img"
      //统一展示加载中图片
       src={require("../../static/1.png")} 
       //定义data-*属性 储存真正的图片链接
       data-src={info.CoverPhoto} alt="" />
       <span>{info.Name}</span>
       </li>
 } 
    </section>
  </LazyImage>
```

> 子组件

```js
export default class LazyImage extends Component {
    render() {
        return (
            //通过children 取得父组件中 子组件中的部分
            <div ref="scrollImg" className={this.props.className} id="box1">
                {this.props.children}
            </div>
        )
    }
    scroll=()=>{
        //获取所有的图片
        let imgs=[...this.scrollImg.querySelectorAll("img")]
        //判断图片数组是否存在
        if(!imgs.length){
            return;
        }
        //获取图片真正的链接
        let {imgSrc}=this.props.options;
        //滚动过程中判scrollImg盒子的高度+可视窗口>图片距父元素的距离 就赋值真正的src 实现加载出来
        imgs.forEach(imgDom=>{
            if(this.scrollImg.scrollTop+this.clientHeight>imgDom.offsetTop){
                let url =imgDom.getAttribute(imgSrc)
                imgDom.setAttribute("src",url)
            }
        })
    }
    //组件挂载前
    componentDidMount(){
        //获取scrollImg dom
        this.scrollImg=this.refs.scrollImg;
        //获取scrollImg盒子的可视窗口高度
        this.clientHeight=this.scrollImg.clientHeight;
        //添加监听事件
        this.scrollImg.addEventListener("scroll",this.scroll);
    }
    //组件卸载前清除监听事件
    componentWillUnmount(){
        this.scrollImg.removeEventListener("scroll",this.scroll);
    }
    //滚动到指定字母标题
    scrollToElement =(ind)=>{
        let divs=this.scrollImg.querySelectorAll("div")[ind];
        this.scrollImg.scrollTop=divs.offsetTop;
    }
}

```

## react+ts配置别名

> webpack.config.js

```js
 "@":path.resolve(__dirname,"../src")
```

> tsconfig.json

```js
 "baseUrl": ".",
  "paths": {
      "@/*":["./src/*"]
    }
```

