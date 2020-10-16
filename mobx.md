

1. > 下载

   ```shell
   yarn add mobx mobx-react 
   ```

2. > 仓库模块

   ```js
   import {observable,action} from 'mobx';
   import userapi from "../../../service/user"
   import Cooike from "js-cookie";
   export default class User{
       //声明状态
       @observable usertoken = "";
       //声明行为
       @action
       login=async (params:any)=>{    
       }
   }
   ```

3. > 仓库入口

   ```js
   let files = require.context("./moduls", true, /\.ts$/)
   let SotoreModls:any ={}
   files.keys().forEach((item:any) => {
       let name = item.split("/")[1];
       let StoreClass = files(item).default;
       SotoreModls[name]=new StoreClass
   })
   export default SotoreModls
   ```

   

4. > context index.js

   ```js
   import {createContext} from "react"
   import store from "../store";
   export default  createContext(store)
   ```

5. > 组件中

   ```js
   import React,{useContext} from 'react'
   import contextStore from "../../context"
   import {observer} from "mobx-react-lite"
   let Home =observer((props:Iprops)=>{
       let {Vote}=useContext(contextStore)    
       return (
           </div>
       )
   }) 
   ```

   