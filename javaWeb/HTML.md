# 前端基本

## HTML

### 基础标签

h1-h6	定义标题

font(淘汰)	定义文本的字体，尺寸，颜色

b	定义粗体

i	定义斜体

u	定义下划线

center(淘汰)	定义文本居中

p	定义段落

br	定义换行

hr	定义水平线

------

img	定义图片

audio	定义音频

video	定义视频

a	超链接标签

ol 	有序列表

ul	无序列表

li	列表项

------

tb	表格标签

tr	定义行

td	定义单元格

### 表单标签

form：定义表单

​		action：规定当提交表单时向何处发送表单数据，URL

​		method：规定用于发送表单数据的方式

- get:浏览器会将数据直接附在表单的action URL之后，大小有限制
- post：浏览器会将数据放到http请求消息体中，大小无限制

- 表单数据要想被提交，则必须指定其name属性

------

input	表单项，通过type属性控制输入形式

select	定义下拉列表,option	定义列表项

textarea	文本域



## CSS

### CSS选择器

分类：

1.元素选择器

元素名称{}

2.id选择器

#id属性值{}

3.类选择器

.class属性值{}





## JavaScript

### 引入方式

内部脚本：将js代码定义在HTML页面中

外部脚本：将js代码定义在外部js文件中，然后引入

```html
<script src=""></script>
```



### 基础语法

输出语句

```javascript
window.alert()	//弹出警告框
document.write()	//写入HTML
console.log()	//写入控制台
```



变量

使用var关键字来声明变量，js是一门弱类型语言，变量可以存放不同类型的值。

注意：

1.在函数外使用var定义的变量是全局变量

2.let关键字定义的变量只在{}代码块内有效

3.const关键字可以用来声明一个只读的常量

```javascript
var test=20;
{
    let test=20;
}
```



数据类型

js中分为：原始类型和引用类型

5种原始类型分别为：number，string，boolean，null，undefined



js对象

Array用于定义数组。js中的数组相当于java中的集合，变长变类型

```javascript
var arr=[1,2,3]	//var arr=new Array(1,2,3);
```



String用于定义字符串

```javascript
var str="hello"	//var str=new String("hello")
```

trim()方法用于去除字符串前后两端的空格



自定义对象

```js
var person={
    name:"zhangsan",
    age:23
    eat:function(){
        alert("吃饭");
    }
}
```



### BOM对象

BOM即浏览器对象模型

Window：浏览器窗口对象

属性：获取其他BOM对象

history：历史记录

Navigator：

Screen：

location：地址栏对象。href	设置或返回完整的URL



方法：

alert()	

confirm()	显示一段消息以及确认按钮和取消按钮的对话框

setInterval()	按照指定的周期(以毫秒计)来调用函数或计算表达式

setTimeout()	在指定的毫秒数后调用函数或计算表达式



### DOM对象

DOM即文档对象模型。将标记语言的各个组成部分封装为对象

- Document：整个文档对象
- Element：元素对象
- Attribute：属性对象
- Text：文本对象
- Comment：注释对象



#### 获取Element

获取：使用Document对象的方法来获取

1. getElementById：
2. getElementsByTagName：
3. getElementsByName：
4. getElementsByClassName：



#### Element常见使用

element.innerHTML：设置或返回元素的内容

elemetn.style：设置或返回元素的style属性

element.checked：用来设置或返回checkbox是否被选中



### 事件监听

事件绑定——事件绑定有两种方式

主要是通过DOM元素属性绑定

```html
<input type="button" name="" id="btn">
document.getElementById(btn).onclick=function(){
    alert("click");
}
```



### 正则表达式

正则表达式是和语言无关的。很多语言都支持正则表达式，Java语言也支持，只不过正则表达式在不同的语言中的使用方式不同，js 中需要使用正则对象来使用正则表达式。

正则表达式常用的规则如下：

- ^：表示开始
- $：表示结束
- [ ]：代表某个范围内的单个字符，比如： [0-9] 单个数字字符
- .：代表任意单个字符，除了换行和行结束符
- \w：代表单词字符：字母、数字、下划线(_)，相当于 [A-Za-z0-9_]
- \d：代表数字字符： 相当于 [0-9]

量词：

- +：至少一个
- *：零个或多个
- ？：零个或一个
- {x}：x个
- {m,}：至少m个
- {m,n}：至少m个，最多n个

**代码演示：**

```js
// 规则：单词字符，6~12
//1,创建正则对象，对正则表达式进行封装
var reg = /^\w{6,12}$/;

var str = "abcccc";
//2,判断 str 字符串是否符合 reg 封装的正则表达式的规则
var flag = reg.test(str);
alert(flag);
```

### 

