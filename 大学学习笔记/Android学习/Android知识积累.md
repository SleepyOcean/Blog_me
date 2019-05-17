# Android知识积累

 ## 1. Activity相关知识

* __不同生命周期调用`finish()`时会调用哪些生命周期函数?__

  1. `onCreate()`中调用：

     ![alt 1](E:\我的技术文档\知识笔记\Android学习\素材\1.png)

  2. `onStart()`中调用：

     ![alt 2](E:\我的技术文档\知识笔记\Android学习\素材\2.png)

  3. `onResume()`中调用：

     ![alt 3](E:\我的技术文档\知识笔记\Android学习\素材\3.png)

  4. `onPause()`中调用：

     ![alt 4](E:\我的技术文档\知识笔记\Android学习\素材\4.png)

  5. `onStop()`中调用：

     ![alt 5](E:\我的技术文档\知识笔记\Android学习\素材\5.png)

  6. `onRestart()`中调用：

     ![alt 6](E:\我的技术文档\知识笔记\Android学习\素材\6.png)

  7. `onDestroy()`中调用：

     ![alt 7](E:\我的技术文档\知识笔记\Android学习\素材\7.png)

## 2. 四大组件

1. Activity

2. Service
3. Broadcast Receiver
4. Content Provider

