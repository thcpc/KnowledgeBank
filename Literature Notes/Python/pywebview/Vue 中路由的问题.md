## 现象
pywebview有的版本无法正确解析加载的Url, 导致客户端启动的时候，无法加载页面
配置如下
```js
const router = createRouter({  
  history: createWebHistory(import.meta.env.BASE_URL),  
  routes: [  
    {  
      path: '/',  
      name: 'home',  
      component: HomeView,  
    }
  ],  
})
```
App.Vue 如下
```js
<script setup>  
</script>  
  
<template>  
  <div id="app">  
    <router-view/>  </div>  
  
</template>  
  
<style scoped>  
</style>
```
## 解决
- 方法1
配置默认路由到主页
```js
const router = createRouter({  
  history: createWebHistory(import.meta.env.BASE_URL),  
  routes: [  
    {  
      path: '/',  
      name: 'home',  
      component: HomeView,  
    },  
    {  
      path: '/:pathMatch(.*)*', // 匹配所有未知路径  
      component: HomeView // 默认加载的 404 页面  
    }  
  ],  
})
```
- 方法2
```python
class MainApp:  
    def __init__(self, web_view):  
        self.web_view = web_view  
  
    def index(self):  
        root = File.RootPath("dev")  
        return str(root.joinpath("static").joinpath("view").joinpath("index.html"))
if __name__ == '__main__':  
    main_window = webview.create_window('proCheck', url=app.index(), width=1400, height=900)    
    webview.start(debug=True)
```
这时启动的默认路由未 /index.html, 可以配置这个路由路径
```js
const router = createRouter({  
  history: createWebHistory(import.meta.env.BASE_URL),  
  routes: [  
    {  
      path: '/index.html',  
      name: 'home',  
      component: HomeView,  
    },  
    // {  
    //   path: '/:pathMatch(.*)*', // 匹配所有未知路径  
    //   component: HomeView // 默认加载的 404 页面  
    // }  
  ],  
})  
  
export default router
```