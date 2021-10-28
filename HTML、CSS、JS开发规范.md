[TOCM]

# (一)、HTML规范
#### 1、img标签要写alt属性
如果图片显示不出来，就直接alt提示的内容。
```html
<img src="company-logo.svg"  />    // ✗ avoid
<img src="company-logo.svg" alt="ABC CompanyLogo"/> // ✓  ok  
```
#### 2、自定义属性要以data-开头
自己添加的非标准的属性要以data-开头，否则w3c会认为是不规范的
```html
<div count="5"></div>    // ✗ avoid
<div data-count="5"></div> // ✓  ok  
```
#### 3、不要在https的链接里写http的图片
只要https的网页请求了一张http的图片，就会导致浏览器地址栏左边的小锁没有了，一般不要写死，写成根据当前域名的协议去加载，用//开头：
```html
// ✓  ok
<img src=”//static.chimeroi.com/hello-world.jpg”>
```
#### 4、ul/ol的直接子元素只能是li，同样，tr的直接子元素都应该是td
```html
// ✗ avoid
<ol>          
    <span>123</span>
    <li>a</li>
    <li>b</li>
</ol>
```
```html
// ✓  ok
<ol>          
    <li>a</li>
    <li>b</li>
</ol>
```
#### 5、不推荐使用属性设置样式,能用CSS设置的就用CSS  （canvas除外）
属性上设置的样式可能会在safari上面是不支持的
```html
// ✗ avoid

// img 属性设置其宽高
<img src="test.jpg" alt width="400" height="300" /> 

// table 设置边框
<table border="1"></table>
```

```html
// ✓  ok

// 如果你用CSS设置的话它会变成拉伸，变得比较模糊
<canvas width="800" height="600"></canvas>
```
#### 6、行内元素里面不可使用块级元素
行内元素内部套块元素，可能导致行内元素标签无法正常点击，如果非要行内套块，则可通过display将行内元素改为块元素
```html
// ✗ avoid
<a href="/listing?id=123">
    <div></div>
</a>
```
```html
// ✓  ok
<a href="/listing?id=123" style="display:block;">
    <div></div>
</a>
```
#### 7、每个页面要写<!DOCType html>,设置页面的渲染模式为标准模式
如果没有写，则会变成怪异模式，从而影响页面的布局

#### 8、不要在DOM中穿插style与script，script脚本放在文件结尾
script不要写在head标签里面，会阻碍页面加载
```html
// ✗ avoid
<body>
  <div></div>
  <script>
  ...
  </script>
  <p></p>
</body>  
```
```html
// ✓  ok
<body>
  <div></div>
  <p></p>
</body>  
<script>
  ...
</script>
```
#### 9、样式不能写在body里,统一放在head标签里

```html
   样式不能写在body里，写在body里会导致渲染两次，特别是写得越靠后，可能会出现闪屏的情况，比如上面的已经渲染好了，突然遇到一个style标签，导致它要重新渲染，这样页面就闪了一下，影响用户体验。

   css如果不多，也推荐写成style标签直接嵌在页面上，因为如果搞个外链，浏览器需要先做域名解析，然后再建立连接，接着才是下载，这一套下来可能已经过了0.5s/1s，甚至更多。而写在页面的CSS虽然无法缓存，但是本身它也不会很大，再加gzip压缩，基本上在50k以内。
```
#### 10、html要加上lang的属性
```html
// ✓  ok

//英文的网页
<html lang="en">     //表示是英文的网页
<html lang="en-US">  //表示是美国英语的网页

//中文的网页
<html lang="zh-CN">
```
#### 11、在head标签写上charset的meta标签，指定编码格式
```html
// ✓  ok
<head>
   <meta charset="utf-8">
</head>
```
#### 12、img中src为空的问题
有时候可能你需要在写一个空的img标签，然后在JS里面动态地给它赋src，因此可能会这么写：
```html
// ✗ avoid
<img src="" alt>
```
但是这样写会有问题，如果你写了一个空的src，会导致浏览器认为src就是当前页面链接，**然后会再一次请求当前页面**，就跟你写一个a标签的href为空类似。如果是background-image也会有类似的问题。这个时候怎么办呢？
```html
// ✓  ok

// 第一种是把src写成about:blank,去加载一个空白页面
<img src="about:blank" alt>

// 第二种是写一个1px的透明像素的base64
<img src="data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7">
```


#### 13、行内元素之间，代码换行引起的空格问题
行内与块、块与块之间换行不会引起空格问题
```html
<form>
    <label>Email:</label>
    <input type="email">
</form>
```
如上，在label和input中间会有一个空格，这个空格是两个行内元素之间换行引起的。

解决办法如下：
```html
//第一种，将两个行内元素放一起，不要换行（不推荐，不利于阅读）
<form>
    <label>Email:</label><input type="email">
</form>
```
```html
//第二种，可以给父级font-size：0 然后再给子元素设置自己的font-size;
<form style="font-size:0;"> 
    <label style="font-size:13px;">Email:</label>
    <input style="font-size:13px;"  type="email">
</form>
```
```html
//第三种，子元素浮动，父级清除浮动;
<form style="overflow:hidden;"> 
    <label style="float:left;">Email:</label>
    <input style="float:left;"  type="email">
</form>
```
#### 14、类的命名使用小写字母加中划线连接
```html
// ✗ avoid
<div class="userinfo"></div>
<div class="userInfo"></div>
<div class="user_info"></div>
```
```html
// ✓  ok
<div class="user-info"></div>
```
#### 15、防止同一标签上同一属性的重复出现
```html
// ✗ avoid
<input class="books" type="text" name="books" class="valid">
```
```html
// ✓  ok
<input class="books valid" type="text" name="books">
```




# (二)、CSS规范
#### 1、CSS书写顺序
1、位置属性(position, top, right, z-index, display, float等)
2、大小(width, height, padding, margin)
3、文字系列(font, line-height, letter-spacing, color- text-align等)
4、背景(background, border等)
5、其他(animation, transition等)
```css
// ✗ avoid
.example {
  color: red;
  z-index: 1;
  background-color: #dfdfdf;
  display: inline-block;
  font-size: 20px;
  height: 100px;
}
```
```css
// ✓  ok
.example {
  color: red;
  z-index: 1;
  background-color: #dfdfdf;
  display: inline-block;
  font-size: 20px;
  height: 100px;
}
```
#### 2、使用CSS缩写属性
```css
// ✗ avoid
.list-box {
  font-family: serif;
  font-size: 20px;
  line-height: 1.5;
  padding-bottom: 10px;
  padding-top: 10px;
  padding-left: 5px;
  padding-right: 5px;
}
```
```css
// ✓  ok
.list-box {
  font: 20px/1.5 serif;
  padding: 10px 5px;
}
```
#### 3、去掉小数点前面的0
```css
.example {
  letter-space: .5em;    // ✓  ok
  padding-top: 0.8em;    // ✗ avoid 
}
```
#### 4、命名语义化，可简写，但前提是不影响语义化
```css
// ✗ avoid
.author{
  color: #dfdfdf;
}

// 简写为
.atr{
  color: #dfdfdf; 
}
```
```css
// ✓  ok
.navigation {
  font-size: 20px;
}

// 简写为
.nav {
  font: 20px/1.5 serif;
  padding: 10px 5px;
}
```
#### 5、命名语义化，多个单词用中横线相连

```css
// ✗ avoid
.maintitle {
  color: #dfdfdf;
}
.mainTitle {
  color: #dfdfdf; 
}
.main_title {
  color: #dfdfdf; 
}
```
```css
// ✓  ok
.main-title {
  color: #dfdfdf;
}
```
#### 6、id是唯一的，不可重复

#### 7、若选择器表示状态, 可为选择器添加状态前缀
比如下图是添加了“.is-”前缀。
```css
// ✗ avoid
.active {
  color: 'red';
}
```
```css
// ✓  ok
.is-active {
  color: 'red';
}
```
#### 8、常用的CSS命名字段
```
 头：header                内容：content/container
 尾：footer                导航：nav
 侧栏：sidebar             栏目：column
 左右中：left right center 登录条：loginbar
 标志：logo                广告：banner
 页面主体：main             热点：hot
 新闻：news                下载：download
 子导航：subnav             菜单：menu
 子菜单：submenu            搜索：search
 友情链接：friendlink       页脚：footer
 版权：copyright            滚动：scroll
 内容：content              标签：tags
 文章列表：list             提示信息：msg
 小技巧：tips               栏目标题：title
 加入：joinus               指南：guild
 服务：service              注册：regsiter
 状态：status               投票：vote
 合作伙伴：partner          页面外围控制整体布局宽度：wrapper
 按钮：btn 
```
#### 9、CSS文件命名
- 1. 一律小写;
- 2. 尽量用英文;
- 3. 多个单词用中横线相连

常用文件名
```
 主要的 master.css              模块 module.css
 基本共用 base.css              布局、版面 layout.css
 主题 themes.css                专栏 columns.css
 文字 font.css                  表单 forms.css
 补丁 mend.css                  打印 print.css
```

# (三)、 Javascript 开发规范
#### 1、 变量与函数名统一采用小写驼峰命名、常量使用全部大写，字母之间用下划线分割
```javascript
function my_function () { }    // ✗ avoid
function myFunction () { }     // ✓ ok

var my_var = 'hello'           // ✗ avoid
var myVar = 'hello'            // ✓ ok

const MAXSTOCKCOUNT = 1000        // ✗ avoid
const MAX_STOCK_COUNT = 1000        // ✓ ok
```
#### 2、使用两个空格进行缩进
```javascript
function hello (name) {
  console.log('hi', name)
}
```
#### 3、除需要转义的情况外，字符串统一使用单引号
```javascript
console.log('hello there')   // 无需转义
$("<div class='box'>")       // 需要转义
```
#### 4、不要定义未使用的变量
```javascript
function myFunction () {
  var result = something()   // ✗ avoid (result 无任何地方调用)
}
```
#### 5、始终使用 === 替代 ==
```javascript
if (name === 'John') { } // ✓ ok
if (name == 'John') { }    // ✗ avoid

if (name !== 'John') { }    // ✓ ok
if (name != 'John') { }    // ✗ avoid
```
#### 6、字符串拼接操作符 (Infix operators) 之间要留空格
```javascript
var x = 2
var message = 'hello, ' + name + '!'   // ✓ ok
var message2 = 'hello, '+name+'!'      // ✗ avoid
```
#### 7、逗号后面加空格
```javascript
// ✓ ok
var list = [1, 2, 3, 4]
function greet (name, options) { ... }
```
```javascript
// ✗ avoid
var list = [1,2,3,4]
function greet (name,options) { ... }
```
#### 8、函数声明时括号与函数名间加空格。
```javascript
function name (arg) { ... }   // ✓ ok
function name(arg) { ... }    // ✗ avoid
```
#### 9、else 关键字要与花括号保持在同一行
```javascript
// ✓ ok
if (condition) {
  // ...
} else {
  // ...
}
```
```javascript
// ✗ avoid
if (condition)
{
  // ...
}
else
{
  // ...
}

```
#### 10、多行 if 语句的的括号不能省
```javascript
// ✓ ok
if (options.quiet !== true) console.log('done')
```
```javascript
// ✓ ok
if (options.quiet !== true) {
  console.log('done')
}
```
```javascript
// ✗ avoid
if (options.quiet !== true)
  console.log('done')
```

#### 12、使用浏览器全局变量时加上 window. 前缀
不包括: document, console and navigator.
```javascript
window.alert('hi')   // ✓ ok
```
#### 13、不允许有连续多行空行。
```javascript
// ✓ ok
var value = 'hello world'
console.log(value)
```
```javascript
// ✗ avoid
var value = 'hello world'


console.log(value)
```
#### 14、对于三元运算符** `?` 和 `:` 与他们所负责的代码处于同一行
```javascript
// ✓ ok 
var location = env.development ? 'localhost' : 'www.api.com'
```
```javascript
// ✓ ok
var location = env.development
  ? 'localhost'
  : 'www.api.com'
```
```javascript
// ✗ avoid
var location = env.development ?
  'localhost' :
  'www.api.com'
```
#### 15、每个 var 关键字单独声明一个变量
```javascript
// ✓ ok
var silent = true
var verbose = true
```
```javascript
// ✗ avoid
var silent = true, verbose = true
```
```javascript
// ✗ avoid
var silent = true,
    verbose = true
```
#### 16、单行代码块两边加空格
```javascript
function foo () {return true}    // ✗ avoid
function foo () { return true }  // ✓ ok
```
#### 17、对象不允许有多余的行末逗号，始终将逗号置于行末
```javascript
// ✗ avoid
var obj = {
  name:'Jony',
  message: 'hello',   // 多余行未逗号
}
var obj = {
  name:'Jony'
  ,message: 'hello'  // 逗号未始终放于行末
}
```
```javascript
// ✓  ok
var obj = {
  name:'Jony',
  message: 'hello'   
}
```
#### 18、点号操作符须与属性需在同一行
```javascript
console.
  log('hello')  // ✗ avoid

console.log('hello') // ✓ ok
```
#### 19、函数调用时标识符与括号间不留间隔
```javascript
console.log ('hello') // ✗ avoid
console.log('hello')  // ✓ ok
```
#### 20、键值对当中冒号与值之间要留空白
```javascript
var obj = { 'key' : 'value' }    // ✗ avoid
var obj = { 'key' :'value' }     // ✗ avoid
var obj = { 'key':'value' }      // ✗ avoid
var obj = { 'key': 'value' }     // ✓ ok
```
#### 21、构造函数要以大写字母开头
```javascript
function animal () {}
var dog = new animal()    // ✗ avoid

function Animal () {}
var dog = new Animal()    // ✓ ok
```
#### 22、无参的构造函数调用时要带上括号
```javascript
function Animal () {}
var dog = new Animal    // ✗ avoid
var dog = new Animal()  // ✓ ok
```
#### 23、子类的构造器中一定要调用 super
```javascript
class Dog {
  constructor () {
    super()   // ✗ avoid
  }
}

class Dog extends Mammal {
  constructor () {
    super()   // ✓ ok
  }
}
```
#### 24、定义变量时使用数组字面量而不是构造器
```javascript
var nums = new Array(1, 2, 3)   // ✗ avoid
var nums = [1, 2, 3]            // ✓ ok
```
#### 25、对象字面量中不要定义重复的属性
```javascript
var user = {
  name: 'Jane Doe',
  name: 'John Doe'    // ✗ avoid
}
```
#### 26、同一模块有多个导入时一次性写完
```javascript
import { myFunc1 } from 'module'
import { myFunc2 } from 'module'          // ✗ avoid

import { myFunc1, myFunc2 } from 'module' // ✓ ok
```
#### 27、避免不必要的布尔转换
```javascript
const result = true
if (!!result) {   // ✗ avoid
  // ...
}

if (result) {     // ✓ ok
  // ...
}
```
#### 28、switch 一定要使用 break 来将条件分支正常中断
```javascript
// ✗ avoid
switch (filter) {
  case 1:
    doSomething()    
  case 2:
    doSomethingElse()
}
```
```javascript
// ✓ ok
switch (filter) {
  case 1:
    doSomething()
    break         
  case 2:
    doSomethingElse()
}
```
#### 29、不要省去小数点前面的0
```javascript
const discount = .5      // ✗ avoid
const discount = 0.5     // ✓ ok
```
#### 30、正确使用 ES6 中的字符串模板
```javascript
const message = 'Hello ${name}'   // ✗ avoid
const message = `Hello ${name}`   // ✓ ok
```
#### 31、用 throw 抛错时，抛出 Error 对象而不是字符串
```javascript
throw 'error'               // ✗ avoid
throw new Error('error')    // ✓ ok
```
#### 32、不要使用 undefined 来初始化变量
```javascript
let name = undefined    // ✗ avoid

let name
name = 'value'          // ✓ ok

```
#### 33、return，throw，continue 和 break 后不要再跟代码
```javascript
function doSomething () {
  return true
  console.log('never called')     // ✗ avoid
}
```
#### 34、禁止多余的构造器
```javascript
class Car {
  constructor () {      // ✗ avoid
  }
}
```
#### 35、遇到分号时, 空格要留分号后面，不留前面
```javascript
for (let i = 0 ;i < items.length ;i++) {...}    // ✗ avoid
for (let i = 0; i < items.length; i++) {...}    // ✓ ok
```
#### 36、针对接口返回的数据，必须做数据格式的验证
###### 36.1、为什么要验证
     在对接口返回数据处理时，浏览器常见的报错如下：
     `xxx is not a function`
     `xxx is not property of xxx`
     这种报错，大多是由于返回的数据类型与预期的数据类型不一致，而你没有对数据类型进行判定，就调用了相应数据类型的方法。
###### 36.2、常见的数据类型判定方法
- 数组类型Array： 
  ```javascript
  Array.isArray(参数)、参数.constructor === Array
  ```
- 对象类型Object：
  ```javascript
  参数.constructor === Object
  ```
- 字符串String: 
  ```javascript
  参数.constructor === String  、 typeof 参数 === 'string'
  ```
- 数字Number:
  ```javascript
  参数.constructor === Number  、 typeof 参数 === 'number'
  ```
- 布尔值Boolean: 
  ```javascript
  参数.constructor === Boolean  、 typeof 参数 === 'boolean'
  ```