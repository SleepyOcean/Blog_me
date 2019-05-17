# VUE样式污染问题解决方法

## 1. 使用scoped限定

* Q1： 想要修改组件内部的样式问题

  A1： 通过`>>>`深度解析

  ```css
  .detail-tabs >>> .el-tabs__header {
  	height: 40px;
  }
  
  .detail-tabs >>> .el-tabs__content {
  	padding: 5px;
  	height: calc(100% - 40px - 10px);
  }
  ```


