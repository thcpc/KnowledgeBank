## 问题现象
$().modal 提示找不到函数
## 1. 正确配置 vite.config.js
```js
export default defineConfig({  
  plugins: [  
    vue(),  
    vueDevTools(),  
  ],  
  resolve: {  
    alias: {  
      '@': fileURLToPath(new URL('./src', import.meta.url)),  
      '@codemirror/state': path.resolve(__dirname, 'node_modules/@codemirror/state'),  
      '@codemirror/view': path.resolve(__dirname, 'node_modules/@codemirror/view'),  
      '@codemirror/language': path.resolve(__dirname, 'node_modules/@codemirror/language'),  
      'jquery': path.resolve(__dirname, 'src/assets/summernote/jquery-3.4.1.slim.min.js')  //引入文件路径
    },  
  },  
  build: {  
    assetsInlineLimit: 0, // 禁止内联字体文件  
  },  
  optimizeDeps: {  
    include: ['@codemirror/state', '@codemirror/view', '@codemirror/language']  //预编译jquery
  }  
})
```

## 2. 正确main.js的导入顺序
```js
//必须在tabler.min.js之前，因为它实际是bootstrarp4继承过来
import "./assets/summernote/jquery-3.4.1.slim.min.js" 
import "./assets/draggable/draggable.bundle.js"  
import "./assets/tabler/js/demo-theme.min.js"  
import "./assets/tabler/js/tom-select.base.min.js"  
import "./assets/tabler/js/nouislider.min.js"  
import "./assets/tabler/js/litepicker.js"  
import "./assets/tabler/js/tabler.min.js"  
import "./assets/tabler/js/demo.min.js"  
import "./assets/summernote/summernote-lite.min.js"  
  
  
import './assets/main.css'  
// import 'codemirror/lib/codemirror.css';  
// import 'codemirror/mode/javascript/javascript';  
import { createApp } from 'vue'  
import { createPinia } from 'pinia'  
  
import App from './App.vue'  
import router from './router'  
  
import { EventBus } from "@/core/eventBus.js";  
import '@toast-ui/editor/dist/toastui-editor.css'; // Editor's Style  
import '@toast-ui/editor/dist/toastui-editor-viewer.css';  
  
const app = createApp(App)  
app.provide('eventBus', EventBus);  
app.use(createPinia())  
app.use(router)  
  
app.mount('#app')
```