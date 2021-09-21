---
sidebar_position: 2
---

# 常用CSS


## 解决1px边框变粗的问题

```
.dom{
  height: 1px;
  background: #dbdbdb;
  transform:scaleY(0.5);
}
```

>出现1px变粗的原因，比如在2倍屏时1px的像素对应2个物理像素。

## 文字超出部分显示...

```
overflow: hidden;
text-overflow: ellipsis;
display: -webkit-box;
-webkit-line-clamp: 1;
-webkit-box-orient: vertical;
```

- `-webkit-line-clamp` 可以控制行数

## 解决IOS页面跳卡顿

```
body,html{
    -webkit-overflow-scrolling: touch;
}
```

touch：滚动回弹效果，当手指从触摸屏上移开，内容会保持一段时间的滚动效果，继续滚动的速度和持续的时间和滚动手势的强烈程度成正比。同时也会创建一个新的堆栈上下文。

