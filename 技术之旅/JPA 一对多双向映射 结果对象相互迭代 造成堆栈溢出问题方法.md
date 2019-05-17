##问题：

<font color=red>JPA 在双向映射时，会相互包含对方的实例，相互引用，造成递归迭代，堆栈溢出（java.lang.StackOverflowError）。</font>





## 分析:

<font color=blue>在后端向前端传递的时候会将数据序列化，转为json，这时会出现循环引用造成堆栈溢出</font>



## 解决方案：

解决方法就是在==转换json时忽略循环字段==。首先确定项目使用的json包是哪一个（jackjson 或 fastjson），寻找相应的注解忽略某一字段。

jackson包对应的相关注解： `@JsonIgnoreProperties`、`@JsonIgnore`

fastjson包对应的相关注解： `@JSONField(serialize = false)`

<font color=red>__注意:__</font> 在使用注解时一定要注意引入的包是否正确，如果和自己使用的json包不对应的话，注解是不生效的。



附代码：

```java
// 我的项目使用的alibaba的fastjson包

@Data
class SOStudent {
    
    
    // ... ...省略其他字段
        
    @ManyToOne(cascade = {CascadeType.REFRESH}, fetch = FetchType.LAZY)
    @JoinColumn(name="classId")
    @JSONField(serialize = false)
    private SOClass class;
}

@Data
class SOClass {
    @Id
    @GeneratedValue
    private Long id
        
    // ... ...省略其他字段
        
	@OneToMany(cascade={CascadeType.ALL},fetch = FetchType.EAGER)
    private List<SOStudent> students;
}
```

