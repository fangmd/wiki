---
sidebar_position: 1
---

# MacOS

## “XXXX” is damaged and can’t be opened. You should move it to the Trash.

非苹果商店下载的 app 可能会触发苹果安全机制，使用下面指令处理后可以打开。

```
xattr -cr /path/to/application.app
```

etc.

```
xattr -cr /Applications/Sip.app
```
