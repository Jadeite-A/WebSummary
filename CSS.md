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

#### ★CSS3弹性盒模型(flexbox)

弹性盒子由弹性容器(Flex container)和弹性子元素(Flex item)组成。

弹性容器通过设置 display 属性的值为 flex 或 inline-flex将其定义为弹性容器。

弹性容器内包含了一个或多个弹性子元素。

**注意：**  - 弹性容器外及弹性子元素内是正常渲染的。弹性盒子只定义了弹性子元素如何在弹性容器内布局。

- 弹性子元素通常在弹性盒子内一行显示。默认情况每个容器只有一行。

##### (1).flex-direction:指定了弹性子元素在父容器中的位置

**语法**
`flex-direction: row | row-reverse | column | column-reverse`

**flex-direction的值:""

- row：横向从左到右排列（左对齐），默认的排列方式。
- row-reverse：反转横向排列（右对齐，从后往前排，最后一项排在最前面。
- column：纵向排列。
- column-reverse：反转纵向排列，从后往前排，最后一项排在最上面。

##### (2).justify-content 属性

内容对齐（justify-content）属性应用在弹性容器上，把弹性项沿着弹性容器的主轴线（main axis）对齐。

**语法：**
`justify-content: flex-start | flex-end | center | space-between | space-around`
**各个值解析:""

- flex-start：
弹性项目向行头紧挨着填充。这个是默认值。第一个弹性项的main-start外边距边线被放置在该行的main-start边线，而后续弹性项依次平齐摆放。

- flex-end：
弹性项目向行尾紧挨着填充。第一个弹性项的main-end外边距边线被放置在该行的main-end边线，而后续弹性项依次平齐摆放。

- center：
弹性项目居中紧挨着填充。（如果剩余的自由空间是负的，则弹性项目将在两个方向上同时溢出）。

- space-between：
弹性项目平均分布在该行上。如果剩余空间为负或者只有一个弹性项，则该值等同于flex-start。否则，第1个弹性项的外边距和行的main-start边线对齐，而最后1个弹性项的外边距和行的main-end边线对齐，然后剩余的弹性项分布在该行上，相邻项目的间隔相等。

- space-around：
弹性项目平均分布在该行上，两边留有一半的间隔空间。如果剩余空间为负或者只有一个弹性项，则该值等同于center。否则，弹性项目沿该行分布，且彼此间隔相等（比如是20px），同时首尾两边和弹性容器之间留有一半的间隔（1/2*20px=10px）。

##### (3).align-items 属性:设置或检索弹性盒子元素在侧轴（纵轴）方向上的对齐方式
**语法**
`align-items: flex-start | flex-end | center | baseline | stretch`

**各个值解析:**

- flex-start：弹性盒子元素的侧轴（纵轴）起始位置的边界紧靠住该行的侧轴起始边界。
- flex-end：弹性盒子元素的侧轴（纵轴）起始位置的边界紧靠住该行的侧轴结束边界。
- center：弹性盒子元素在该行的侧轴（纵轴）上居中放置。（如果该行的尺寸小于弹性盒子元素的尺寸，则会向两个方向溢出相同的长度）。
- baseline：如弹性盒子元素的行内轴与侧轴为同一条，则该值与'flex-start'等效。其它情况下，该值将参与基线对齐。
- stretch：如果指定侧轴大小的属性值为'auto'，则其值会使项目的边距盒的尺寸尽可能接近所在行的尺寸，但同时会遵照'min/max-width/height'属性的限制。

##### (4).flex-wrap 属性:用于指定弹性盒子的子元素换行方式
**语法**
`flex-wrap: nowrap|wrap|wrap-reverse|initial|inherit;`
**各个值解析:**

- nowrap:默认， 弹性容器为单行。该情况下弹性子项可能会溢出容器。
- wrap:弹性容器为多行。该情况下弹性子项溢出的部分会被放置到新行，子项内部会发生断行
- wrap:reverse -反转 wrap 排列。

##### (5).align-content 属性:用于修改 flex-wrap 属性的行为
**语法**
`align-content: flex-start | flex-end | center | space-between | space-around | stretch`

**各个值解析:**

- stretch: 默认。各行将会伸展以占用剩余的空间。
- flex-start: 各行向弹性盒容器的起始位置堆叠。
- flex-end: 各行向弹性盒容器的结束位置堆叠。
- center: 各行向弹性盒容器的中间位置堆叠。
- space-between: 各行在弹性盒容器中平均分布。
- space-around: 各行在弹性盒容器中平均分布，两端保留子元素与子元素之间间距大小的一半。

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

### 6.CSS选择符有哪些？哪些属性可以继承？

> id选择器（ # myid）
> 类选择器（.myclassname）
> 标签选择器（div, h1, p）
> 相邻选择器（h1 + p）
> 子选择器（ul > li）
> 后代选择器（li a）
> 通配符选择器（ * ）
> 属性选择器（a[rel = "external"]）
> 伪类选择器（a:hover, li:nth-child）

***可继承的样式:*** font-size font-family color, UL LI DL DD DT;
***不可继承的样式：*** border padding margin width height;

### 7.CSS优先级

** CSS 的继承性**:指应用在一个标签上的那些 CSS 属性被传到其子标签上

** 当网页比较复杂,HTML 结构嵌套较深时,一个标签的样式将深受其祖先标签样式的影响。**影响的规则如下：

- ** CSS 优先规则1：** 最近的祖先样式比其他祖先样式优先级高

- ** CSS 优先规则2：**"直接样式"比"祖先样式"优先级高

- ** CSS 优先规则3：** 优先级关系：内联样式 > ID 选择器 > 类选择器 = 属性选择器 = 伪类选择器 > 标签选择器 = 伪元素选择器

- ** CSS 优先规则4：** 计算选择符中ID选择器的个数（a),计算选择符中类选择器、属性选择器以及伪类选择器的个数之和(（)b),计算选择符中标签选择器和伪元素选择器的个数之和（c）。按 a、b、c 的顺序依次比较大小，大的则优先级高，相等则比较下一个。若最后两个的选择符中 a、b、c 都相等，则按照"就近原则"来判断。

- **CSS 优先规则5：** 属性后插有 !important 的属性拥有最高优先级。若同时插有 !important，则再利用规则 3、4 判断优先级

### 8.居中div

给div设置一个宽度，然后添加`margin:0 auto`属性
```javascript
div{
    width:200px;
    margin:0 auto;
 }
```
#### (1).居中一个浮动元素

```javascript
确定容器的宽高 宽500 高 300 的层
设置层的外边距

.div {
      width:500px ; height:300px;//高度可以不设
      margin: -150px 0 0 -250px;
      position:relative;         //相对定位
      background-color:pink;     //方便看效果
      left:50%;
      top:50%;
}
```

#### (2)让绝对定位的div居中
```javascript
position: absolute;
width: 1200px;
background: none;
margin: 0 auto;
top: 0;
left: 0;
bottom: 0;
right: 0;
```
