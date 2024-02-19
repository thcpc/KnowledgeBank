---
tags:
  - python
  - programming
  - pywebview
  - vue
description: pywebview 中 初始化 vue 组件
---
# 问题描述
pywebview 中要调用 python 代码必须要到window时间 "pywebviewready" 完成后才能调用。
否则 调用 "window.pywebview.api" 会提示无法找到api 对象。

vue 中要改变 data数据，必须在vue 对象方法内加载，否则无法修改数据对象
# 解决方案
把Vue的实例方法注册到 window 事件监听

## v-model 绑定 select
```js
<select class="form-select" aria-label="选择环境" v-model="environment">
    <option selected>选择环境</option>
    <option v-for="option in environmentOptions" :value="option.value">
		{{ option.text }}
	</option>
</select>
```

## 初始化 Vue 对象

```js
const app = createApp({
   setup() {
            const message = ref('Hello vue!')
                return {
                    message
                }
            },
            methods: {
                selectFile() {
                    this.system = 'ctms'
                },
                pywebviewready() {
	                // 调用 python 方法
                    pywebview.api.environmentOptions().then(result => {
                        this.environmentOptions = result
                    });
                }
            },
            data() {
                return {
                    environment: '选择环境',
                    folder: '',
                    sql: '',
                    environmentOptions: []
                }
            },
            mounted() {
	            // window监听事件
                window.addEventListener('pywebviewready', this.pywebviewready)
            },

        });
        app.mount('#app');
```

![[Pasted image 20240219165756.png]]