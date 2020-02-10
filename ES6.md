# PowerShell

>  使用技巧: 
>
> >  打开快捷键: `Win + X` 再按`A`		
> >
> > 清屏快捷键: `Ctrl  + L` 或者	命令行输入 `clear`																												

# ES6

> JavaScript三部分组成
>
> 1. DOM - 操作HTML
> 2. BOM - 操作浏览器
> 3. ECMAScript - 制定标准

> ECMAScript 6 是 JavaScript 语言的国际化标准

## let

> 1. 不存在变量提升
>
> 2. 暂时性死区  -- let 在声明之前都是不可用的
>
>    1. ```js
>       var a = 1;
>       if(1){  //块级作用域
>          a = 2; //在声明之前使用
>          let a; 
>       }
>       //报错: Uncaught ReferenceError : Cannot access "a" Before initialization
>       ```
>
> 3. 不允许重复声明

> ES6 之后 `typeof`不再是百分之百不报错了
>
> ```js
> typeof b;  //在声明之前使用
> let b;
> //报错: Uncaught ReferenceError : Cannot access "a" Before initialization
> ```

## const

> 1. 具备let的所有特性
> 2. 常量-声明后不可改变
> 3. 声明的同时必须初始化

## 作用域

> 1. 全局作用域
> 2. 函数作用域
> 3. **块级作用域  -- ES6 引入**
>    1. 数据不互通, 
>    2. 内部可以访问外部



## 字符串数组扩展

> 数组拼接

```js
arr1 = [1,2,3];
arr2 = [4,5,6];
//通过apply实现
// Array.prototype.push.apply(arr1,arr2);
//通过concat实现
// arr1 = arr1.concat(arr2); 
//通过`...`打包拆包实现
// arr1 = [...arr1,...arr2];
arr1.push(...arr2);
console.log(arr1);
```

### Set()

> Set() - 类似于数组成员的值是唯一的

> 属性:
>
>  	1. add
>  	2. delete
>  	3. has
>  	4. clear

### Array.from()

> 类数组对象,转化成数组 , 必须要有`length`属性

```js
let obj = {
    0 : 'a',
    1 : 'b',
    2 : 'c',
    length : 3
};
let arr = Array.from(obj);
arr; //['a','b','c']
```

> 把集合转换成数组

```js
let set = new Set(1,2.3);
let arr = Array.from(set);
//或者
let arr = [...res];
```



## 解构赋值

> 等号两边的模式相同 , 左边的变量就会被赋予对应的值

```js
let [a,b,c,d] = [1,2,3,4]
```

## 箭头函数

[练习题](https://fanyoufu.github.io/test/es6-ji-chu-ce-shi.html )

# node.js

> 使用ES6 和 node.js 写接口



## 模块化开发

> 一个JavaScript就是一个模块 , 模块内部定义的变量和函数默认情况下在外部无法得到
>
> 模块内部可以使用exports(出口)对象进行成员导出, 使用require(需要)方法导入其他模块

### b.js 引入 a.js

>  代码演示
>
> ```js
> //a.js文件内容
> let a = 11;
> let say = () => "a-say";
> 
> exports.a=a;
> module.exports.a=a;
> //上面两种写法都能实现导出,如果两个引用指向不一样以后者为准
> exports.say=say;
> 
> //b.js文件内容
> let a = require('./a.js'); //文件后缀名`.js`可以省略
> console.log(a.a); //11
> console.log(a.say()); //a-say
> ```
>
> 

### exports和module.exports的区别

> 用一句话说明:
>
> > require方能看到的只有module.exports这个对象, 它是看不到exports对象的,而我们在编写模块是用到的exports对象实际上只是对module.exports的引用

## 系统模块

### fs - 文件操作

> 引入fs模块
>
> ```js
> const fs = require('fs');
> ```
>
> 
>
> 读取文件内容
>
> ````js
> fs.readFile('文件路径' , [文件编码] , callback);
> ````
>
> ```js
> fs.readFile('./b.js(','utf-8',(err,doc) => {
>     if (err == null) {
>         //在控制台输出文件的内容
>         console.log(doc);
>     }
> });
> ```
>
> 写入文件内容
>
> ```js
> fs.writeFile('文件路径' , '写入的数据' , callback);
> ```
>
> ```js
> const content = '<h3> 正在使用fs.writeFile 写入文件内容 </h3>';
> fs.writeFile("./aaa.txt" , content , err => {
>     if(err != null) {
>         console.log(err);
>         return;
>     }
>     console.log("文件写入成功");
> });
> ```
>
> 

### path - 路径操作

> 为什么要进行路径拼接
>
> > 不同操作系统的路径分隔符不统一
> >
> > Windows 上是 \ /
> >
> > Linux 上是 /     -- 考虑 Linux 是因为服务器的系统可能是Linux

> 路径拼接语法
>
> ```js
> path.join('路径','路径',...);
> ```
>
> ```js
> //引入path
> const path = require('path');
> console.log(path.join('d:',"a","b")); //d:/a/b
> ```
>
> 

### 相对路径 vs 绝对路径

> 大多数情况下使用绝对路径 , 因为相对路径有时候相对的是命令行工具的当前工作目录
>
> 在读取文件或者设置文件路径时都会选择绝对路径
>
> 使用`__dirname`获取当前文件所在的绝对路径

## 第三方模块

### 什么是第三方模块

> > 别人写好的,具有特定功能的 .我们能直接使用的模块即为第三方模块 , 由于第三方模块通常都是由多个文件组成并且被放置在一个文件夹中,所以又名包.

### 第三方模块的两种存在形式:

> > 以 js 文件的形式存在, 提供实现项目具体功能的API接口.
> >
> > 以命令行工具形式存在 , 辅助项目开发

### 获取第三方模块

> > `npmjs.com `:第三方模块管理网站
> >
> > > `npm`
> > >
> > > > 下载: 命令行输入`npm install `模块名称
> > > >
> > > > > 下载时提示警告信息没有关系, 只有不报红色错误信息就是下载成功了
> > > >
> > > > 卸载 :`npm uninstall `模块名称

### 全局安装与本地安装

> > 命令行工具:全局安装
> >
> > 库文件: 本地安装

### 第三方模块 `nodemon`

> > nodemon 是一个命令行工具 , 用以辅助项目开发.
> >
> > 在Node.js中 , 每次修改文件都要在命令行工具中重新执行该文件 , 非常繁琐.
>
> 使用步骤:
>
> > 使用`npm install modemon -g` 下载  -- 表示全局安装 , 意味着在任何项目都可以使用这个工具了
> >
> > 在命令行工具中用`nodemon`命令替代`node`命令执行文件

### 第三方模块 `nrm`

> > `nrm` (npm registry manager) : `npm` 下载地址切换工具
> >
> > `npm` 默认的下载地址在国外 , 国内下在速度慢
> >
> > 使用步骤
> >
> > > 使用`npm install nrm -g`下载
> > >
> > > 查询可用下载地址列表 `nrm ls`
> > >
> > > 切换`npm`下载地址 ` nrm use 下载地址名称`
> > >
> > > 如: 切换为淘宝的地址: `nrm use taobao`

### 第三方模块 `Gulp`

> > 基于 `node`平台开发的前端构建工具
> >
> > 将机械化操作编写成任务 , 想要执行机械化操作时执行一个命令行命令任务就能自动执行了
> >
> > 用机械代替手工, 提升效率
> >
> > `Gulp`能做什么
> >
> > > 项目上线 , HTML . CSS . JS 文件压缩合并
> > >
> > > 语法转换(es6 , less...)
> > >
> > > 公共文件抽离
> > >
> > > 修改文件浏览器自动刷新
> >
> > 使用
> >
> > >   
>
> 