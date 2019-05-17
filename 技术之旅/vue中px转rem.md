## 1. 设置根节点HTML的font-size

1. 新建px2rem.js文件，放在项目根目录utils目录下，代码如下：

   ```js
   // 基准大小，设置20px为基准，便于自己写代码时计算rem的值
   const baseSize = 20;
   // 设置 rem 函数
   function setRem () {
   	// 当前页面宽度相对于 768 宽的缩放比例，可根据自己需要修改。
   	const scale = document.documentElement.clientWidth / 768;
   	// 设置页面根节点字体大小
   	document.documentElement.style.fontSize = (baseSize * Math.min(scale, 2)) + 'px';
   }
   // 初始化
   setRem();
   // 改变窗口大小时重新设置 rem
   window.onresize = function () {
   	setRem();
   };
   ```

2. 在main.js中引入px2rem.js

   ```js
   import '../utils/px2rem'
   ```

   这时页面HTML的font-size已被自动添加。

## 2. 使用postcss-pxtorem将px自动转为rem

1. 安装postcss-pxtorem

   ```bash
   $ npm i postcss-pxtorem -S
   ```

2. 修改根目录.postcssrc.js文件，在找到 `plugins` 属性新增pxtorem的设置

   ```js
   "postcss-pxtorem": {
     "rootValue": 20, //与px2rem.js中的baseSize保持一致
     "propList": ["*"]
   }
   ```



**注意：此方法支持import 和 .vue单文件中style。暂不支持style中使用@import url()以及内联样式中的style**