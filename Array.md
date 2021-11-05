# **Array**
JavaScript的**Array**对象是用于构造数组的全局对象，数组是类似于列表的高阶对象。
<br>
## 描述
数组是一种类列表对象，它的原型中提供了遍历和修改元素的相关操作。JavaScript 数组的长度和元素类型都是非固定的。因为数组的长度可随时改变，并且其数据在内存中也可以不连续，所以 JavaScript 数组不一定是密集型的，这取决于它的使用方式。一般来说，数组的这些特性会给使用带来方便，但如果这些特性不适用于你的特定使用场景的话，可以考虑使用类型数组***TypedArray***。       
     
只能用整数作为数组元素的索引，而不能用字符串。后者称为 关联数组。使用非整数并通过 方括号 或 点号 来访问或设置数组元素时，所操作的并不是数组列表中的元素，而是数组对象的 属性集合 上的变量。数组对象的属性和数组元素列表是分开存储的，并且数组的遍历和修改操作也不能作用于这些命名属性。

## 构造器
### `Array()` :用于创建 Array 对象。
#### 语法
```JavaScript
//字面量创建
[element0, element1, ..., elementN]

// 元素创建
new Array(element0, element1, ..., elementN)

// 长度创建
new Array(arrayLength)
```
##### 参数说明    
**elementN**     
&#8195;即数组元素。Array 构造器会根据给定的元素创建一个 JavaScript 数组，但是当仅有一个参数且为数字时除外。注意，后者仅适用于用 Array 构造器创建数组，而不适用于用方括号创建的数组字面量。       

**arrayLength**     
&#8195;即数组长度。是一个正整数，此时将返回一个 length 的值等于 arrayLength 的数组对象。如果传入的参数不是有效值，则会抛出 RangeError 异常

## 静态属性    
###`Array[@@species]` :访问器属性返回 Array 的构造函数。    
#### 语法    
&#8195;`Array[Symbol.species]`
##### 返回值   
&#8195;`Array`的构造函数。
#### 描述   
&#8195;species 访问器属性返回 Array 对象的默认构造函数。子类的构造函数可能会覆盖并改变构造函数的赋值。
#### 示例    
`species` 属性返回默认构造函数, 它用于 `Array` 对象的构造函数 `Array`:    
```JavaScript 
Array[Symbol.species]; // function Array()
```
在继承类的对象中（例如你自定义的数组 MyArray），MyArray 的 species 属性返回的是 MyArray 这个构造函数. 然而假如想要覆盖它，以便在 MyArray 中返回父类的构造函数 Array :
```JavaScript
class MyArray extends Array {
  // 重写 MyArray 的 species 属性到父类 Array 的构造函数
  static get [Symbol.species]() { return Array; }
}
```

## 静态方法    
### **Array.from()** :从一个类似数组或可迭代对象创建一个新的，浅拷贝的数组实例   
#### 语法   
&#8195;`Array.from(arrayLike[, mapFn[, thisArg]])`     
##### 参数说明   
**arrayLike**      
&#8195;想要转换成数组的伪数组对象或可迭代对象。     
**mapFn** ***可选***   
&#8195;如果指定了该参数，新数组中的每个元素会执行该回调函数。    
**thisArg** ***可选***   
&#8195;可选参数，执行回调函数 `mapFn` 时 `this` 对象。     
##### 返回值     
&#8195;一个新的数组实例       
#### 示例    
从 `String` 生成数组     
```` JavaScript
Array.from('foo');
// [ "f", "o", "o" ]
````    
从 `Set` 生成数组    
```` JavaScript
const set = new Set(['foo', 'bar', 'baz', 'foo']);
Array.from(set);
// [ "foo", "bar", "baz" ]
````      
从类数组对象（arguments）生成数组     
```` JavaScript
function f() {
  return Array.from(arguments);
}   

f(1, 2, 3);   

// [ 1, 2, 3 ]   
```` 
### **Array.isArray()**：用来判断某个变量是否是一个数组对象      
#### 示例
```` JavaScript
Array.isArray(Array.prototype);
// true
Array.isArray(true);
// false
````
##### `instanceof` 和 `isArray`
当检测Array实例时, `Array.isArray` 优于 `instanceof`,因为Array.isArray能检测`iframes`.
```` JavaScript
var iframe = document.createElement('iframe');
document.body.appendChild(iframe);
xArray = window.frames[window.frames.length-1].Array;
var arr = new xArray(1,2,3); // [1,2,3]

Array.isArray(arr);  // true
arr instanceof Array; // false
````
#### 兼容性   
假如不存在 `Array.isArray()`，则在其他代码之前运行下面的代码将创建该方法。
```` JavaScript
if (!Array.isArray) {
  Array.isArray = function(arg) {
    return Object.prototype.toString.call(arg) === '[object Array]';
  };
}
````
### **Array.of()**：根据一组参数来创建新的数组实例，支持任意的参数数量和类型   
##### **`Array.of()`** 和 **`Array`** 构造函数
&#8195;他们之间的区别在于处理整数参数：**`Array.of(7)`** 创建一个具有单个元素 7 的数组，而 **`Array(7)`** 创建一个长度为7的空数组（**注意：** 这是指一个有7个空位(empty)的数组，而不是由7个`undefined`组成的数组）。

```` JavaScript
Array.of(7);       // [7]
Array.of(1, 2, 3); // [1, 2, 3]

Array(7);          // [ , , , , , , ]
Array(1, 2, 3);    // [1, 2, 3]
````
#### 示例
```` JavaScript
Array.of(undefined); // [undefined]
Array.of('',''); // ['', '']
Array.of(null,null); // [null, null]
````
#### 兼容旧环境    
如果原生不支持的话，在其他代码之前执行以下代码会创建 `Array.of()` 。
```` JavaScript
if (!Array.of) {
  Array.of = function() {
    return Array.prototype.slice.call(arguments);
  };
}
````
## 实例属性    
### `Array.prototype.length`
**`length`** 是Array的实例属性。返回或设置一个数组中的元素个数。该值是一个 0 到 2<sup>32</sup>-1 的整数，并且总是大于数组最高项的下标。   

#### Array.length 属性的属性特性：        
1. **writable** 默认值：`true` 如果设置为`false`，该属性值将不能被修改       
2. **enumerable** 默认值：`false` 如果设置为`true` ，属性可以通过迭代器`for`或`for...in`进行迭代       
3. **configurable** 默认值：`false` 如果设置为`false`，删除或更改任何属性都将会失败        

### `Array.prototype[@@unscopables]`    
Symbol 属性 **`@@unscopable`** 包含了所有 ES2015 (ES6) 中新定义的、且并未被更早的 ECMAScript 标准收纳的属性名。这些属性被排除在由 [with](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/with) 语句绑定的环境中。
