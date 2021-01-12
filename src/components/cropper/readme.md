1、安装依赖
```javascript
  npm install --save vue-cropper
```

2、引入方法
```javascript
  import cropper from '@/components/cropper'
```

3、使用
```javascript
// 简单使用
cropper.show({
  backImg (img) {
    console.log(img)
  }
}

// 全部参数
cropper.show({
  backImg (img) { // 裁剪图片后，点击完成的回调函数 参数:{fileName, stream(base64)}
    console.log(img)
  },
  file, // van-uploader组件after-read自定义事件参数（result-type: dataUrl）
  autoCropWidth: 200, // 默认生成截图框宽度 0~max
  autoCropHeight: 100, // 默认生成截图框高度 0~max
  fixed: false, // 是否开启截图框宽高固定比例 true | false
  fixedNumber: [1, 2], // 截图框的宽高比例 [宽度, 高度]
  fixedBox: false, // 固定截图框大小 不允许改变 true | false
  canMove: true, // 上传图片是否可以移动 true | false
  canMoveBox: true // 截图框能否拖动 true | false
})

// 关闭
cropper.hide()

// 是否显示 返回boolean
cropper.isVisible()
```