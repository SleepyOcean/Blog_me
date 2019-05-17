# java解惑

## 1. 子类的构造函数问题

```java
public class Constructor {
	public static void main(String[] args) {
      	new foo3();
      	new foo3("Aloha");
	}
}

class foo{
  foo(){System.out.println("foo...");}
  foo(String s){System.out.println("foo..."+s);}
}

class foo1 extends foo{
  foo1(){System.out.println("foo1...");}
  foo1(String s){System.out.println("foo1..."+s);}
}

class foo2 extends foo1{
  foo2(){System.out.println("foo2...");}
  foo2(String s){System.out.println("foo2..."+s);}
}

class foo3 extends foo2{
  foo3() {System.out.println("foo3...");}	
  foo3(String s){System.out.println("foo3..."+s);}
}
```

**运行结果：**

```
foo...
foo1...
foo2...
foo3...
foo...
foo1...
foo2...
foo3...Aloha
```

**解释：**

​	当子类创建新对象时首先调用父类的无参构造方法。

​	`new foo3();`构造方法调用顺序为foo()-->foo1()-->foo2()-->foo3();

​	`new foo3("Aloha");`构造方法调用顺序为foo()-->foo1()-->foo2()-->foo3("Aloha");

​	我的理解：构造方法中的隐藏了`super();`方法在自己的构造方法体中（即隐式调用父类无参构造方法），如下

```java
foo3(String s){
  super();		//显式调用
  System.out.println("foo3..."+s);
}
```

​	即子类构造方法默认调用父类的无参构造方法。

​	*Attention：*

​	如果显式调用父类的构造方法必须将父类构造方法放在子类构造方法句首，不能像这样：

```java
foo3(String s){
  System.out.println("foo3..."+s);
  super(s);		//编译将会无法通过
}
```

​	



