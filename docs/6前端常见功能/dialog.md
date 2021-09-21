---
sidebar_position: 1
---

# 弹框

<img src="/img/6前端常见功能/dialog/dialog.gif" width="375" height="812"/>

完整 Demo: [https://github.com/fangmd/wiki-demo/blob/master/dialog/index.html](https://github.com/fangmd/wiki-demo/blob/master/dialog/index.html)

## 功能拆解

1. 点击按钮，显示弹框
2. 点击弹框中确定和取消按钮，隐藏弹框
3. 弹框显示的时候需要半透明背景色
4. 弹框显示的时候背景不能滚动和点击
5. 点击背景色的时候可以隐藏弹框（或者不隐藏）

## 实现

```html
<div class="mask"></div>
<div class="dialog">
  <div class="title">提示</div>
  <div class="content">退出登录？</div>
  <div class="btn-wrapper">
    <div class="cancel">取消</div>
    <div class="confirm">确定</div>
  </div>
</div>
```

1. mask 实现透明背景色
2. dialog 为具体弹框

---

```html
<style>
  .mask {
    display: none;
    position: fixed;
    left: 0;
    top: 0;
    background: rgba(0, 0, 0, 0.2);
    width: 100vw;
    height: 100vh;
    z-index: 100;
  }

  .dialog {
    display: none;
    position: fixed;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    z-index: 110;
    background: #fff;
  }

  .dialog .title {
    height: 40px;
    line-height: 40px;
    width: 300px;
    text-align: center;
    font-weight: 600;
  }

  .dialog .content {
    height: 40px;
    line-height: 40px;
    width: 300px;
    text-align: center;
  }

  .dialog .btn-wrapper {
    display: flex;
    justify-content: center;
  }

  .dialog .btn-wrapper .cancel {
    height: 40px;
    line-height: 40px;
    width: 80px;
    text-align: center;
  }

  .dialog .btn-wrapper .confirm {
    box-sizing: border-box;
    height: 40px;
    width: 80px;
    line-height: 40px;
    text-align: center;
    margin-left: 60px;
  }
</style>
```

1. 弹框显示和隐藏使用 `display` 控制，`none` 为隐藏，`block` 表示显示
2. `mask` 和 `dialog` 使用 `position: fixed` 脱离文档流。同时配合 `transform: translate(-50%, -50%)` 实现相对窗口居中效果

---

```js
function hideDialog() {
  document.querySelector(".mask").style.display = "none"
  document.querySelector(".dialog").style.display = "none"
}
function showDialog() {
  document.querySelector(".mask").style.display = "block"
  document.querySelector(".dialog").style.display = "block"
}
```

1. 显示和隐藏函数，修改 css 样式即可

---

```html
document.querySelector('.mask').addEventListener('click', () => {
console.log('cancel'); hideDialog() })
document.querySelector('.cancel').addEventListener('click', () => {
console.log('cancel'); hideDialog() })
document.querySelector('.confirm').addEventListener('click', (event) => {
console.log('confirm'); hideDialog() })
```

添加点击事件

## 背景禁止滚动和点击

方案：弹框隐藏时允许滚动，显示的时候禁止滚动。

设置样式：

```
body{
    height: 100%;
    overflow: auto;
}
```

在弹框显示的时候设置禁止滚动：`document.body.style.overflow = 'hidden';`

```js
function hideDialog() {
  document.body.style.overflow = "auto"
  document.body.style.height = "100%"
  document.querySelector(".mask").style.display = "none"
  document.querySelector(".dialog").style.display = "none"
}
function showDialog() {
  document.body.style.overflow = "hidden"
  document.querySelector(".mask").style.display = "block"
  document.querySelector(".dialog").style.display = "block"
}
```
