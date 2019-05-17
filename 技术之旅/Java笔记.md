# Java笔记

###1.  `object.equals(null)`和`object == null`的区别

```java

Object object = null;

object.equals(null)；		// 会抛出异常，后续代码不执行

object == null;		// true，继续执行后续代码

```



