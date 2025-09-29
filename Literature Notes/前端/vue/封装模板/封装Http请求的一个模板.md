自我封装的一个模板,使用axios ,包含了动态URL,跨域,JWT的处理办法
http.js

```js
import axios from 'axios'  
import {userAuthStore} from "@/stores/tokenManager.js";  
import eventBus from "@/core/eventBus.js";  
import {ErrCode} from "@/core/enums.js";  
import Cookies from 'js-cookie';  

// 动态加载URL
const backendURI = import.meta.env.VITE_API_BASE_URL  
  
const api = axios.create({  
  baseURL: backendURI,  // 替换为你的 Django 后端地址  
  withCredentials: true,  // 允许跨域请求携带 Cookie
});  
  
// 在发送请求之前做些什么  
api.interceptors.request.use(  
  (config) => {  
    // JWT 请求  
    let user = userAuthStore()  
    if(user.token){  
      config.headers['Authorization'] = `Bearer ${user.token}`;  
    }

	// 跨域请求 
    const csrfToken = Cookies.get('csrftoken');  
    config.headers['X-CSRFToken'] = csrfToken;  
    // 访问时显示读取框
    document.getElementById('overlay').classList.remove('d-none')  
    return config  
  },  
  (error) => {  
    // 对请求错误做些什么
    // 关闭读取框  
    document.getElementById('overlay').classList.add('d-none')  
    return Promise.reject(error)  
  },  
)  
// 对响应数据做些什么  
api.interceptors.response.use(  
  (response) => {  
    //关闭读取框
    document.getElementById('overlay').classList.add('d-none')  
    return response  
  },  
  (error) => {  
    //关闭读取框  
    document.getElementById('overlay').classList.add('d-none')  
    return Promise.reject(error)  
  },  
)  
// get 请求
export function httpGet(url, params, callBack, errCallBack) {  
  api  
    .get(url, {  
      params: params,  
    })  
    .then((response) => {  
      httpResponse(response, callBack, errCallBack)  
    })  
    .catch((error) => {  
      // console.error(error)  
      alert(error)  
    })  
    .finally(() => {})  
}  
// delete 请求
export function httpDelete(url, params, callBack, errCallBack) {  
  api  
    .delete(url, {  
      params: params,  
    })  
    .then((response) => {  
      httpResponse(response, callBack, errCallBack)  
    })  
    .catch((error) => {  
  
      alert(error)  
    })  
}  
// Post Json 请求
export function httpPostJson(url, data, callBack, errCallBack) {  
  api.defaults.headers.post['Content-Type'] = 'application/json'  
  api  
    .post(url, data)  
    .then((response) => {  
      httpResponse(response, callBack, errCallBack)  
    })  
    .catch((error) => {  
  
      alert(error)  
    })  
}  
// Post Form Data 请求
export function httpPostForm(url, formData, callBack, errCallBack) {  
  api  
    .post(url, formData, {  
      headers: {  
        'Content-Type': 'multipart/form-data', // 必须设置  
      },  
    })  
    .then((response) => {  
      httpResponse(response, callBack, errCallBack)  
    })  
    .catch((error) => {  
  
      alert(error)  
    })  
}  

// 封装的 Response 处理
export function httpResponse(response, callBack, errCallBack) {  
  if (response.data.code === 200) {  
    callBack(response.data.payload)  
  } else if(response.data.code===ErrCode.InvalidToken || response.data.code===ErrCode.TokenTimeOut){  
      // JWT的处理
      if(errCallBack){ errCallBack() }  
      eventBus.$emit('JwtTokenException', { config: response.config, callBack: callBack, errCode: response.data.code })  
    }  
}  
  
export function httpRetry(config, callBack){  
  api.request(config).then(response=>{  
    httpResponse(response, callBack)  
  }).catch(error=>{  
    alert(error)  
  })  
}
```