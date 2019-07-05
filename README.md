# wxutil ![build](https://img.shields.io/badge/build-passing-00d508.svg) ![license](https://img.shields.io/badge/license-MIT-3963bc.svg) 


wxutil工具使用promise语法封装了小程序官方的高频API，以及常用的开发方法。


## 快速上手
在需要使用的位置引入wxutil
```js
const wxutil = require("/utils/wxutil.js")
```


## 工具模块
- [网络请求](#网络请求)
- [文件请求](#文件请求)
- [socket通信](#socket通信)
- [图片操作](#图片操作)
- [提示](#提示)
    - [showToast](#showToast)
    - [showModal](#showModal)
    - [showLoading](#showLoading)
    - [showActionSheet](#showActionSheet)
- [缓存](#缓存)
- [其他工具](#其他工具)


## 网络请求
封装微信小程序wx.request()方法实现五大http请求方法

### get
使用promise语法异步获取请求数据或捕获请求异常信息
```js
const handler = {
    url: url,
    data: data,
    header: header
} 

wxutil.request.get(handler).then((data) => {
    console.log(data)  // 业务处理
}).catch((error) => {
    console.log(error) // 异常处理
})
```

亦可用更快速的方法请求
```js
wxutil.request.get(url).then((data) => {
    console.log(data)
})
```

### post
```js
const handler = {
    url: url,
    data: {},
    header: {}
}

wxutil.request.post(handler).then((data) => {
    console.log(data)
})
```

亦可用一下方式请求
```js
wxutil.request.post({url: url, data: data}).then((data) => {
    console.log(data)
})
```

### put
```js
wxutil.request.put({url: url, data: data}).then((data) => {
    console.log(data)
})
```

### patch
```js
wxutil.request.patch({url: url, data: data}).then((data) => {
    console.log(data)
})
```

### delete
```js
wxutil.request.delete({url: url, data: data}).then((data) => {
    console.log(data)
})
```

## 文件请求
封装微信小程序wx.downloadFile()和wx.uploadFile()方法

### download
```js
wxutil.file.download({url}).then((data) => {
    console.log(data)
})
```

### upload
```js
wxutil.file.upload({
    url: url,
    fileKey: fileKey,
    filePath: filePath,
    data: {},
    header: {}
}).then((data) => {
    console.log(data)
})
```

## socket通信
封装微信小程序的websocket部分方法，实现整个socket流程如下
```js
let socketOpen = false  // socket连接标识
wxutil.socket.connect(url)

// 监听socket通信
wx.onSocketMessage(function(res) {
    console.log(res)  
}

wx.onSocketOpen(function(res) {
    socketOpen = true
    if (socketOpen) {
        // 发送socket消息
        wxutil.socket.send("hello wxutil").then((data) => {
            console.log(data)
        })
    }

    // 关闭socket连接
    wxutil.socket.close(url)
})
```


## 图片操作
封装微信小程序的wx.saveImageToPhotosAlbum()、wx.previewImage()、wx.chooseImage()方法，用于保存图片到本机相册、预览图片以及从相机或相册选择图片

### save
```js
wxutil.image.save(path).then((data) => {
    console.log(data)
})
```

### preview
```js
wxutil.image.preview(["img/1.png"])
```

### choose
参数：count, sourceType
```js
wxutil.image.choose(1).then((data) => {
    console.log(data)
})
```


## 提示
封装微信小程序的wx.showToast()、wx.showModal()、wx.showLoading()和wx.showActionSheet()方法，用于给用户友好提示

### showToast
```js
wxutil.showToast("hello")
```

### showModal
```js
wxutil.showModal("提示", "这是一个模态弹窗")
```
亦可传入参数并在回调函数中处理自己的业务
```js
wxutil.showModal(title: title, content: content, handler = {
    showCancel: showCancel,
    cancelText: cancelText,
    confirmText: confirmText,
    cancelColor: cancelColor,
    confirmColor: confirmColor
}).then((data) => {
    console.log(data)
})
```

### showLoading
```js
wxutil.showLoading("加载中")
```

### showActionSheet
```js
wxutil.showActionSheet(['A', 'B', 'C']).then((data) => {
    console.log(data)
})
```


## 缓存
封装微信小程序的wx.setStorageSync()和wx.getStorageSync()方法，异步设置缓存和获取缓存内容，并可以设置缓存过期时间

### setStorage
```js
wxutil.setStorage("userInfo", userInfo)
```

亦可为缓存设置过期时间，单位：秒
```js
wxutil.setStorage("userInfo", userInfo, 86400)
```

### getStorage
```js
wxutil.getStorage("userInfo")
```


## 其他工具
封装了常用的小程序方法，便于高效开发，也可以增加自己的工具方法在下方

### isNotNull
判断字符串是否为空、空格回车等，不为空返回true
```js
wxutil.isNotNull("text")
```

### getDateTime
获取当前日期时间，格式：yy-mm-dd hh:MM:ss
```js
const datetime = wxutil.getDateTime()
console.log(datetime)
```

### getTimestamp
获取当前时间戳
```js
const timestamp = wxutil.getTimestamp()
console.log(timestamp)
```

### autoUpdate
小程序自动更新，在app.js中引用该方法，可以在小程序发布新版本后自动更新
```js
wxutil.autoUpdate()
```

### getLocation
获取用户的地理位置
```js
wxutil.getLocation().then((data) => {
    console.log(data)
})
```

亦可通过传入可选参数打开微信小程序的地图
```js
wxutil.getLocation(watch = true).then((data) => {
    console.log(data)
})
```
