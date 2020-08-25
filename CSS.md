# CSS

### 1.谈谈前端中的浮动，绝对定位，相对定位
#### (1).浮动

**浮动最初创建的时候是为了解决文字环绕效果，而文字环绕效果会对父元素的高度造成破坏，从而塌陷。简而言之：浮动具有破坏性，导致父元素高度塌陷**

#### (2).相对定位：position:relation

相对定位,就是微调元素位置，让元素相对自己原来的位置,进行位置的微调，也就是说,如果一个盒子想进行位置调整,那么就要使用相对定位了

```javascript
position:relative;   → 必须先声明，自己要相对定位了，
left:100px;       → 然后进行调整。
top:150px;       → 然后进行调整。
```

##### ①相对定位的特性 - 不脱标,老家留坑,形影分离

##### ②相对定位的用途

相对定位有坑,所以一般不用于做"压盖"效果.页面中,效果极小.就两个作用:
- 微调元素
- 做绝对定位的参考,子绝父相(绝对定位中详细讲)

##### ③相对定位的定位值

可以用left,right来描述盒子右,左的移动
可以用top,bottom来描述盒子的下,上的移动.
```javascript
position: relative;
right: 100px;   → 往左边移动
top: 100px;

position: relative;	
right: 100px;
bottom: 100px;    → 移动方向是向上。
```

#### (3).绝对定位

绝对定位之后,标签就不区分所谓的行内元素,块级元素了,不需要`display:block`；就可以设置宽高了。
```javascript
span{
	position: absolute;
	top: 100px;
    left: 100px;
	width: 100px;
	height: 100px;
	background-color: pink;
}
```
##### ①参考点

- 绝对定位的参考点，如果用top描述，那么定位参考点就是页面的左下角，而不是浏览器的左上角
- 如果用bottom描述，那么就是浏览器首屏窗口尺寸，对应的页面的左下角

##### ②以盒子为参考点 - 子绝父相

- 一个绝对定位的元素，如果父辈元素中出现了也定位了的元素，那么将以父辈这个元素，为参考点
- 子绝父绝，子绝父相，子绝父固，都是可以给儿子定位的。但是，工程上，子绝，父绝，没有一个盒子在标准文档流中，所以页面就不稳固，没有任何实战用途
```javascript
<div class=”box1”>  → 绝对定位
	<div class=”box2”>  → 相对定位
		<div class=”box3”>  → 没有定位
			<p></p>  → 绝对定位，以box2为参考定位。
		</div>
	</div>
</div>
```
- 绝对定位的儿子，无视参考的那个盒子的padding

##### ③绝对定位的盒子居中

绝对定位之后,所有标准流的规则，都不适用了。所以`margin:0 auto`失效
```javascript
width: 600px;
height: 60px;
position: absolute;
left: 50%;
top: 0;
margin-left: -300px;   → 宽度的一半
```
**非常简单,当做公式记一下来.就是`left:50%;margin-left:负的宽度的一半`**

### 3.清除浮动

***清除浮动原因:***

- 浮动的盒子脱标, 会造成没有主动设置高度的父盒子, 失去高度
- 浮动的元素对其他元素有影响, 这种影响会让两者相互贴靠,这种影响有的时候会让页面布局混乱

#### ★Ⅰ.父级div定义伪类：after和zoom(推荐)

```javascript
<style type="text/css">
   .div1{background:#000080;border:1px solid red;}
   .div2{background:#800080;border:1px solid red;height:100px;margin-top:10px}

   .left{float:left;width:20%;height:200px;background:#DDD}
   .right{float:right;width:30%;height:80px;background:#DDD}

   /*清除浮动代码*/  .clearfloat:after{display:block;clear:both;content:"";visibility:hidden;height:0}
   .clearfloat{zoom:1}
   </style> 
<div class="div1 clearfloat"> 
<div class="left">Left</div> 
<div class="right">Right</div> 
</div>
<div class="div2">
   div2
   </div>
```

- 原理：IE8以上和非IE浏览器才支持:after，原理和方法2有点类似，zoom(IE转有属性)可解决ie6,ie7浮动问题
- 优点：浏览器支持好，不容易出现怪问题（目前：大型网站都有使用，如：腾迅，网易，新浪等等）
- 缺点：代码多，不少初学者不理解原理，要两句代码结合使用，才能让主流浏览器都支持
- 建议：推荐使用，建议定义公共类，以减少CSS代码

#### 补充：伪类和伪元素

伪类和伪元素的引入：css引入伪类和伪元素的概念是为了格式化文档树以外的信息。
伪类是添加到选择器的关键字,指定元素处于特殊状态时的样式效果.

在CSS1和CSS2中对伪类和伪选择器没有做出很明显的区别定义，而二者在语法是一样的，都是以`:`开头，这造成很多人会将某些伪元素误认为是伪类，如:`before`，`:after`；

- 伪类用于选择DOM树之外的信息，或是不能用简单选择器进行表示的信息。前者包含那些匹配指定状态的元素，比如:`visited`，`:active`；后者包含那些满足一定逻辑条件的DOM树中的元素，比如:`first-child`，`:first-of-type`，`：target`。
- 伪元素为DOM树没有定义的虚拟元素。不同于其他选择器，它不以元素为最小选择单元，它选择的是元素指定内容。比如`::before`表示选择元素内容的之前内容，也就是`""`；`::selection`表示选择元素被选中的内容

**常用伪类**

| Selector  | Meaning |
| ------------- | ------------- |
| :active  | 选择正在被激活的元素  |
| :hover  | 选择被鼠标悬浮着元素  |
| :link  | 选择未被访问的元素 |
| :visited  | 选择已被访问的元素  |
| :first-child  | 选择满足是其父元素的第一个子元素的元素  |
| :focus  | 选择拥有键盘输入焦点的元素  |
| :lang  | 选择带有指定 lang 属性的元素  |

**常用伪元素**

| Selector  | Meaning |
| ------------- | ------------- |
| ::first-letter  | 选择指定元素的第一个单词  |
| ::first-line  | 选择指定元素的第一行  |
| ::after  | 在指定元素的内容前面插入内容  |
| ::before  | 在指定元素的内容后面插入内容  |

[前端---css通俗易懂理解伪类和伪元素的区别][anchor-id]
[anchor-id]: https://blog.csdn.net/weixin_42504145/article/details/82990739

#### Ⅱ.在结尾处添加空div标签clear:both
```javascript
<style type="text/css">
   .div1{background:#000080;border:1px solid red}
   .div2{background:#800080;border:1px solid red;height:100px;margin-top:10px}

   .left{float:left;width:20%;height:200px;background:#DDD}
   .right{float:right;width:30%;height:80px;background:#DDD}

   /*清除浮动代码*/
   .clearfloat{clear:both}
   </style> 
<div class="div1"> 
<div class="left">Left</div> 
<div class="right">Right</div>
<div class="clearfloat"></div>
</div>
<div class="div2">
   div2
   </div>
```
- 原理：添加一个空div，利用css提高的clear:both清除浮动，让父级div能自动获取到高度
- 优点：简单，代码少，浏览器支持好，不容易出现怪问题
- 缺点：不少初学者不理解原理；如果页面浮动布局多，就要增加很多空div，让人感觉很不爽
- 建议：不推荐使用，但此方法是以前主要使用的一种清除浮动方法

#### Ⅲ.父级div定义height
```javascript
<style type="text/css">
   .div1{background:#000080;border:1px solid red;/*解决代码*/height:200px;}
   .div2{background:#800080;border:1px solid red;height:100px;margin-top:10px}

   .left{float:left;width:20%;height:200px;background:#DDD}
   .right{float:right;width:30%;height:80px;background:#DDD}
   </style> 
<div class="div1"> 
<div class="left">Left</div>
<div class="right">Right</div> 
</div>
<div class="div2">
   div2
   </div>
```
- 原理：父级div手动定义height，就解决了父级div无法自动获取到高度的问题
- 优点：简单，代码少，容易掌握
- 缺点：只适合高度固定的布局，要给出精确的高度，如果高度和父级div不一样时，会产生问题
- 建议：不推荐使用，只建议高度固定的布局时使用

#### Ⅳ.父级div也一起浮动
```javascript
<style type="text/css"> 
   .div1{background:#000080;border:1px solid red;/*解决代码*/width:98%;margin-bottom:10px;float:left}
   .div2{background:#800080;border:1px solid red;height:100px;width:98%;/*解决代码*/clear:both}
   
   .left{float:left;width:20%;height:200px;background:#DDD}
   .right{float:right;width:30%;height:80px;background:#DDD}
   </style> 
<div class="div1"> 
<div class="left">Left</div> 
<div class="right">Right</div>
</div>
<div class="div2">
   div2
   </div>
```
- 原理：所有代码一起浮动，就变成了一个整体
- 优点：没有优点
- 缺点：会产生新的浮动问题。
- 建议：不推荐使用，只作了解

### 4.盒子模型(Box Model)

**标准的CSS的盒子模型？低版本IE的盒子模型有什么不同的？**

（1）有两种， IE 盒子模型、W3C 盒子模型
（2）盒模型： 内容(content)、填充(padding)、边界(margin)、 边框(border)
（3）区  别： IE的content部分把 border 和 padding计算了进去

[菜鸟教程：CSS盒模型](https://www.runoob.com/css/css-boxmodel.html)

### 5.盒子垂直水平居中
1、定位 盒子宽高已知
```javascript
position: absolute; 
left: 50%; top: 50%; 
margin-left:-自身一半宽度; 
margin-top: -自身一半高度;
```

2、table-cell布局

```javascript
//父级
display: table-cell; 
vertical-align: middle; 
```
```javascript
//子级
margin: 0 auto;
```
3、定位 + transform ; 适用于 子盒子 宽高不定时；
```javascript
position: relative / absolute;
/*top和left偏移各为50%*/
   top: 50%;
   left: 50%;
/*translate(-50%,-50%) 偏移自身的宽和高的-50%*/
transform: translate(-50%, -50%); 这里启动了3D硬件加速 会增加耗电量
```
4、flex 布局
```javascript
父级： 
/*flex 布局*/
display: flex;
/*实现垂直居中*/
align-items: center;
/*实现水平居中*/
justify-content: center;
```
5.水平方向上居中 ：
```javascript
margin-left : 50% ;
transform: translateX(-50%);
```
