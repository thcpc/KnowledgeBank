---
tags:
  - python
  - programming
  - pywebview
  - vue
  - faq
description: npm run build 的文件,pywebview加载时无法找到js资源文件
---
## 现象
![[Pasted image 20240220114344.png]]

## 原因
css 和 js 文件都会出现这个问题，因为vue 打包后，会压缩css 和  js 文件，在index.html中写入路径
的导入方式，就会提示无法找到资源
## 解决
### css
在main.css中导入 css文件，不要在index.html中导入
![[Pasted image 20240220114854.png]]
### Js
- 1. 如果是ES模块的JS 文件，可在main.js 中使用import 解决
- 2.如果不是，类似 bootstrap.min.js 之类的文件，就只有在pywebview的路径文件拷贝一份
![[Pasted image 20240220135831.png]]