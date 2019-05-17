# JavaScript笔记

* ###字符串的操作：

  * 多行字符

    * ```javascript
      `这是一个
      多行
      字符串`;
      ```

    * *注意*：反引号在键盘的ESC下方，数字键1的左边：

      ```
      ┌─────┐ ┌─────┬─────┬─────┬─────┐
      │ ESC │ │ F1  │ F2  │ F3  │ F4  │
      │     │ │     │     │     │     │
      └─────┘ └─────┴─────┴─────┴─────┘
      ┌─────┬─────┬─────┬─────┬─────┐
      │  ~  │  !  │  @  │  #  │  $  │
      │  `  │  1  │  2  │  3  │  4  │
      ├─────┴──┬──┴──┬──┴──┬──┴──┬──┘
      │        │     │     │     │
      │  tab   │  Q  │  W  │  E  │
      ├────────┴──┬──┴──┬──┴──┬──┘
      │           │     │     │
      │ caps lock │  A  │  S  │
      └───────────┴─────┴─────┘
      ```

  * 字符串的操作

    * toUpperCase , toLowerCase , indexOf , substring

      ```javascript
      var s = 'Hello js';

      s[0];	//'H'
      s[4];	//'o'
      s[21];	//undefined 超出范围的索引不会报错，但一律返回undefined

      s.toUpperCase();	//'HELLO JS'
      s.toLowerCase();	//'hello js'
      s.indexOf('js');	//6
      s.indexOf('JS');	//没有找到指定子串，返回-1

      s.substring(0,5);	//'Hello',从索引0开始到5（不包括5）
      s.substring(6);		//'js',从6开始到结束
      ```

  ​

* ###数组

  * indexOf , slice , put&pop , unshift&shift , sort , reverse , splice , concat , join

    ```javascript
    var arr = ['Microsoft', 'Apple', 'Yahoo', 'AOL', 'Excite', 'Oracle'];

    arr.indexOf('Microsoft');	//0
    arr.indexOf('Baidu');		//-1，元素未找到

    arr.slice(0,3);				//['Microsoft', 'Apple', 'Yahoo']
    arr.slice(3);				//['AOL', 'Excite', 'Oracle']

    //push()向末尾添加若干元素，pop()是将最后的元素返回并删除。注：空数组继续pop不会报错，而是返回undefined
    arr.push('a','b');		//['Microsoft', 'Apple', 'Yahoo', 'AOL', 'Excite', 'Oracle','a','b']
    arr.pop();				//'b'
    arr.pop();				//'a'

    //unshift()向头部添加若干元素，shift()将第一个元素返回并删除
    arr.unshift('a','b');	//['a','b','Microsoft', 'Apple', 'Yahoo', 'AOL', 'Excite', 'Oracle']
    arr.shift();			//'a'
    arr.shift();			//'b'

    arr.sort();				
    arr;					//['AOL','Apple','Excite','Microsoft','Oracle','Yahoo']

    arr.reverse();			
    arr;					//['Yahoo','Oracle','Microsoft','Excite','Apple','AOL']

    arr.splice(2,3,'Google','Facebook');	//返回删除元素['Microsoft','Excite','Apple']
    arr;					//['Yahoo','Oracle','Google','Facebook','AOL']
    arr.splice(2,2);		//返回删除元素['Google','Facebook']，只删除不添加
    arr;					//['Yahoo','Oracle','AOL']
    arr.splice(2,0,'Google','Facebook');	//返回[]，只添加不删除		
    arr;					//['Yahoo','Oracle','Google','Facebook','AOL']

    var added = arr.concat(['Amazon','Alibaba']);
    added;		//['Yahoo','Oracle','Google','Facebook','AOL','Amazon','Alibaba']

    arr.join('-');		//'Yahoo-Oracle-Google-Facebook-AOL-Amazon-Alibaba'
    ```



* ### 函数

  1. 函数也是变量，可以传递

  2. 参数传递时可以和函数定义不匹配。即参数缺失以undefined补，参数多出的部分可以通过arguments来获取，也可以`function(a , b , ...rest)`的函数定义来接收更多的参数。

  3. 对象中的函数称之为方法

  4. 高阶函数：一个函数可以接收另一个函数作为参数

     ```javascript
     'use strict';
     
     function pow(x) {
         return x * x;
     }
     
     //map()的用法。注：map的参数默认为一个，即数组的元素。
     var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
     var res_pow = arr.map(pow); 		// [1, 4, 9, 16, 25, 36, 49, 64, 81]
     
     var res_double = arr.map(function(x){
         return 2*x;
     });									//[2,4,6,8,10,12,14,16,18]
     ```


     //reduce()的用法。注：reduce的参数默认为两个，即当前索引的本元素和下个元素。
     arr.reduce(function(x,y){
         return x+y;
     });									//45,1到9求和
    
     //filter()的用法。注：filter的参数默认为一个，即数组的元素。传入的函数返回值类型为bool值，true保留该元素，false丢弃该元素。
     arr = [1, 2, 4, 5, 6, 9, 10, 15];
     var r = arr.filter(function (x) {
         return x % 2 !== 0;
     });									// [1, 5, 9, 15]
    
     arr = ['A', 'B', 'C'];
     r = arr.filter(function (element, index, self) {
         console.log(element); // 依次打印'A', 'B', 'C'
         console.log(index); // 依次打印0, 1, 2
         console.log(self); // self就是变量arr
         return true;
     });
    
     //sort()的高阶用法
     arr = [10, 20, 1, 2];
     arr.sort(function (x, y) {
         if (x < y) {
             return -1;
         }
         if (x > y) {
             return 1;
         }
         return 0;
     });								 // [1, 2, 10, 20]
    
     ```

  5. 闭包：将函数作为结果值返回。特点：延迟调用

     ```javascript
     'use strict'

     function lazy_sum(arr) {
         var sum = function () {
             return arr.reduce(function (x, y) {
                 return x + y;
             });
         }
         return sum;
     }

     var f1 = lazy_sum([1, 2, 3, 4, 5]);
     var f2 = lazy_sum([1, 2, 3, 4, 5]);
     f1 === f2;					// false，当我们调用lazy_sum()时，每次调用都会返回一个新的函数，即使传入相同的参数
     f1(); 						// 15

     function count() {
         var arr = [];
         for (let i=1; i<=3; i++) {		//var定义i时，f1,f2,f3的结果都是16，因为当所有函数都被加载完成时，i的值是4。当let定义i时,i的值只在本循环内有效，所以i的值在被载入时就已经确定，所以f1,f2,f3的结果依次时1,4,9
             arr.push(function () {
                 return i * i;
             });
         }
         return arr;
     }

     var results = count();
     var f1 = results[0];
     var f2 = results[1];
     var f3 = results[2];
     f1();					//1
     f2();					//4
     f3();					//9

     (function (x) {
         return x * x;
     })(3); 		// 9,创建一个匿名函数并立刻执行
     /**
       *上述的循环按下述写法，也可得到1,4,9的结果
       *for (var i=1; i<=3; i++) {
       *     arr.push((function (n) {
       *         return function () {
       *             return n * n;
       *         }
       *     })(i));
       * }
     **/
       
     ```

  6. 箭头函数：类似于Java中的Lambda表达式

     ```javascript
     'use strict'

     //下述两个函数变量的功能一样。
     var f = function (x) {
         return x * x;
     };
     var fn = x => x * x;
     ```

  7. generator：可以多次返回值

     ```javascript
     'use strict'

     function* fib(max) {
         var
             t,
             a = 0,
             b = 1,
             n = 0;
         while (n < max) {
             yield a;
             [a, b] = [b, a + b];
             n ++;
         }
         return;
     }

     var f = fib(5);
     f.next(); // {value: 0, done: false}
     f.next(); // {value: 1, done: false}
     f.next(); // {value: 1, done: false}
     f.next(); // {value: 2, done: false}
     f.next(); // {value: 3, done: false}
     f.next(); // {value: undefined, done: true}
     ```

     ​





* ### 知识点补充

  * ```javascript
    'use strict'	//解决不使用var定义变量造成的全局变量冲突，由ECMA推出的规范
    ```

  * JavaScript中一个语句要以；结尾，不然会出现断句错误的情况。

  * 判断对象类型：  

    ```javascript
    //方法一使用typeof方法。 
    console.log(typeof str); //string 
    console.log(typeof arr); //object 
    console.log(typeof obj); //object 
    console.log(typeof num); //number 
    console.log(typeof b); //boolean 
    console.log(typeof n); //null是一个空的对象 
    console.log(typeof u); //undefined 
    console.log(typeof fn); //function
    ```
  * 声明变量: `var` `let` `const`
	* `var`: 声明一个变量，可赋一个初始值；
	* `let`: 声明一个块作用域的局部变量，可赋一个初始值；
	* `const`: 声明一个块作用域的只读常量；<br>
	ps: 用`var`和`let`语句声明的变量，如果没赋初始值，则其值为`undefined`
  * `undefined` `NaN` `null`的区别与联系
	* `undefined`<br>
		在布尔类型环境下，`undefined`会被当做`false`;在数组类型环境中，`undefined`会被转换为`NaN`；
	* `NaN`<br>
	* `null`<br>
		在数值环境下，`null`会被当做0对待；在布尔类型环境中，`null`会被当做`false`；