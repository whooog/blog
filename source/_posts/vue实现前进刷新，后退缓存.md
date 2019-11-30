---
title: vue实现前进刷新，后退缓存
date: 2019-11-29 12:04:16
tags:
categories:
  - vue
---

**使用技术vuex + keepalive**

##### A=> B=> C=> B： A页面跳到B页面刷新数据,B页面跳转到C页面，再从C页面跳转到B页面缓存数据

#### app.vue 

```vue

    <transition >
      <keep-alive :include='includePage'>
        <router-view v-if="$route.meta.keepAlive" class='router-view'></router-view>
        <router-view v-if="!$route.meta.keepAlive" class='router-view'></router-view>
      </keep-alive>
    </transition>

```

#### vuex

```javascript

import * as types from './types.js'

const store = new Vuex.Store({
    // 要缓存数据的页面
    includePage:[]
})

const actions = {
}

getters: {
   includePage: state => state.includePage;
}

mutations: {
   [types.UPDATE_INCLUDE_PAGE] (state, obj){
        if(obj.flag){
            state.includePage.push(obj.pageName);
        }else{
            state.includePage.splice(state.includePage(obj.pageName),1)
        }
   }
}

```

types.js

```js

export const UPDATE_INCLUDE_PAGE = 'UPDATE_INCLUDE_PAGE'  //更新routerview缓存页面

```

#### A 页面

```js

mounted(){
    if (this.includePage.indexOf("cdata") != -1) {
        // 注：cdata其实就是c页面的name，可以在路由设置name
        //console.log('有缓存，清除缓存')
        this.$store.commit('UPDATE_INCLUDE_PAGE', {pageName: 'cdata', flag: false});
    };
}

```

#### B 页面

```js

mounted() {
    if (this.includePage.indexOf("cdata") == -1) {
      //如果EditAddress没有缓存，就设置缓存
      //this.UPDATE_INCLUDE_PAGE({pageName: 'cdata', flag: true})
      this.$store.commit('UPDATE_INCLUDE_PAGE', {pageName: 'cdata', flag: true});
    };
  }

```

#### app.vue , A页面, B页面

```vue
import { mapGetters } from 'vuex';

computed:{
   ...mapGetters([
     "includePage"
   ])
 }


```


注：此文章参照 [https://www.jianshu.com/p/c24529e5a743](https://www.jianshu.com/p/c24529e5a743)
