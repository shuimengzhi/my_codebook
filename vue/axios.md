# 解决传输给后端格式问题
请求后端是用`application/json`格式请求，后端能接收到的都是form data `key/value`格式，需要添加头部
请求头添加
```
headers: {
    'Content-Type': 'application/x-www-form-urlencoded'
  }
```
安装qs，为了将json格式转为`key/value`
```
npm install qs
```
引入qs
```
import qs from 'qs'
```
json转form data
```
qs.stringify()
```