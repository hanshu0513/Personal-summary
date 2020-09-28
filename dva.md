##### 下载 安装

```shell
npm install dva-cli -g
```

##### 创建项目

```shell
dva new xxx
```

##### 启动项目

```shell
npm start
```

##### 使用antd

```shell
npm install antd babel-plugin-import --save
```

> .webpackrc.js

```js
{
 "extraBabelPlugins": [
   ["import", { "libraryName": "antd", "libraryDirectory": "es", "style": "css" }] 
   ]
}
```

##### 封装fecth

```js
import fetch from 'dva/fetch';
function parseJSON(response) {
  return response.json();
}
function checkStatus(response) {
  if (response.status >= 200 && response.status < 300) {
    return response;
  }
  const error = new Error(response.statusText);
  error.response = response;
  throw error;
}
//get请求拼接参数
function formatURL(url, params) {
  if (url.includes("?")) {
    return url
  }
  url=url+"/api?"
  Object.keys(params).forEach(key => {
    url += `${key}=${params[key]}&`
  })
  return url.slice(0, -1)
}
/**
 * Requests a URL, returning a promise.
 *
 * @param  {string} url       The URL we want to request
 * @param  {string} method    请求方式
 * @param  {string} params    请求携带的参数
 * @return {object}           An object containing either "data" or "err"
 */
function request(url, method, params) {
  let str= method === "GET" ? formatURL(url,params) :"/api"+ url
  url =str
  const options = {
    method,
    headers: {
      //token
    },
  }
  if (method === "POST" || method === "PUT" || method === "DELETE") {
    if (params instanceof FormData) {
       options.headers = {
         ...options.headers,
         "content-type": "multipart/form-data"
       },
         options.body = params
    } else {
      options.headers = {
        ...options.headers,
        "content-type": "application/json"
      },
        options.body = JSON.stringify(params)
    }
  }
  return fetch(url, options)
    .then(checkStatus)
    .then(parseJSON)
    .then(data => ({ data }))
    .catch(err => ({ err }));
}
export default {
  /**
   * 
   * @param {*} url  请求接口
   * @param {*} params 请求携带的参数
   */
  get(url, params) {
    return request(url, "GET", params)
  },
  post(url, params) {
    return request(url, "POST", params)
  }
}
```

##### 自动化引入模块

> 拆分app

```
import dva from 'dva';
// 1. 创建应用
const app = dva();
export default app;
```

> registryModels.js

```js
import app from '@/app'
function registryModels(){
    let modules =require.context("@/models",true,/\.js$/);
    modules.keys().forEach(item=>{
       let info=modules(item).default
       app.model(info)
    })
}
export default registryModels
```

##### 路由 插件 react-router-config使用

> router/index.js

```js
import {renderRoutes} from "react-router-config"
 function RouterConfig({ history }) {
  return (
    <Router history={history}>
      {renderRoutes(routes)}
    </Router>
  );
}
export default RouterConfig;

```

##### 路由懒加载

```js
mport dynamic from 'dva/dynamic';

const UserPageComponent = dynamic({
  app,
  models: () => [
    import('./models/users'),
  ],
  component: () => import('./routes/UserPage'),
});
```

##### 登录判断 高阶组件

```js
//是否登录
const isLogin = () => {
    return cookie.get("infoToken") && cookie.get("infoToken").split(".").length === 3
}
//高阶组件
const isLoginCom = (Com) => {
    if (isLogin()) {
        return <Com />
    } else {
        return <Redirect to="/login"></Redirect>
    }
}
```

