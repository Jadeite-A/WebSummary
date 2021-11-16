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

## 实例方法      
### `Array.prototype.at()`
### `Array.prototype.concat()`   
&#8195;  **`concat()`** 方法用于合并两个或多个数组。此方法不会更改现有数组，而是返回一个新数组。    
#### 语法   
&#8195;   `var new_array = old_array.concat(value1[, value2[, ...[, valueN]]])`    
##### 参数        
**`valueN  `** 可选      
> 数组和/或值，将被合并到一个新的数组中。如果省略了所有 valueN 参数，则 concat 会返回调用此方法的现存数组的一个浅拷贝。   
##### 返回值    
&#8195;  一个新的 Array 实例。
#### 描述    
`concat` 方法创建一个新的数组，它由被调用的对象中的元素组成，每个参数的顺序依次是该参数的元素（如果参数是数组）或参数本身（如果参数不是数组）。它不会递归到嵌套数组参数中。      

`concat` 方法不会改变this或任何作为参数提供的数组，而是返回一个浅拷贝，它包含与原始数组相结合的相同元素的副本。      
原始数组的元素将复制到新数组中，如下所示：    
- 对象引用（而不是实际对象）：concat将对象引用复制到新数组中。 原始数组和新数组都引用相同的对象。 也就是说，如果引用的对象被修改，则更改对于新数组和原始数组都是可见的。 这包括也是数组的数组参数的元素    
- 数据类型如字符串，数字和布尔（不是String，Number 和 Boolean 对象）：concat将字符串和数字的值复制到新数组中。    
> **注意：** 数组/值在连接时保持不变。此外，对于新数组的任何操作（仅当元素不是对象引用时）都不会对原始数组产生影响，反之亦然。    
#### 示例      
```` JavaScript
var alpha = ['a', 'b', 'c'],
    num1 = [1, 2, 3],
    num2 = [4, 5, 6],
    num3 = [7, 8, 9],
    num4 = [[1]],
    num5 = [2, [3]],
    num6 = [5,[6]];
       
//连接两个数组   
var nums = num1.concat(num2);
console.log(nums);    // [1, 2, 3, 4, 5, 6]

//连接三个数组    
var nums = num1.concat(num2, num3);
console.log(nums);    // [1, 2, 3, 4, 5, 6, 7, 8, 9] 

//将值连接到数组
var alphaNumeric = alpha.concat(1, [2, 3]);
console.log(alphaNumeric);    // ['a', 'b', 'c', 1, 2, 3]

//合并嵌套数组
var nums = num4.concat(num5);
console.log(nums);     // [[1], 2, [3]]
var nums2=num1.concat(4,num6);
console.log(nums2)     // results is [[1], 4, 5,[6]]

num4[0].push(4);     // 修改num4的第一个元素

console.log(nums);    // results is [[1, 4], 2, [3]]
````

### `Array.prototype.copyWithin()`    
**`copyWithin() `** 方法浅复制数组的一部分到同一数组中的另一个位置，并返回它，不会改变原数组的长度。    
#### 语法   
&#8195;   *arr* .copyWithin(*target*, *start*, *end*)    
##### 参数        
**`target  `**      
> 0 为基底的索引，复制序列到该位置。如果是负数，`target` 将从末尾开始计算。    
       
> 如果 `target` 大于等于 `arr.length`，将会不发生拷贝。如果 `target` 在 `start` 之后，复制的序列将被修改以符合 `arr.length`。    

**`start`**   
> 0 为基底的索引，开始复制元素的起始位置。如果是负数，`start` 将从末尾开始计算。

> 如果 `start` 被忽略，`copyWithin` 将会从0开始复制。

**`end`**   
> 0 为基底的索引，开始复制元素的结束位置。`copyWithin` 将会拷贝到该位置，但不包括 `end` 这个位置的元素。如果是负数， `end` 将从末尾开始计算。  
        
> 如果 end 被忽略，`copyWithin` 方法将会一直复制至数组结尾（默认为 `arr.length`）。
##### 返回值    
&#8195;  改变后的数组。     
#### 描述     
参数 target、start 和 end 必须为整数。     

如果 start 为负，则其指定的索引位置等同于 length+start，length 为数组的长度。end 也是如此。     

`copyWithin` 方法不要求其 this 值必须是一个数组对象；除此之外，copyWithin 是一个可变方法，它可以改变 this 对象本身，并且返回它，而不仅仅是它的拷贝。   

`copyWithin` 就像 C 和 C++ 的 `memcpy` 函数一样，且它是用来移动 Array 或者 TypedArray 数据的一个高性能的方法。复制以及粘贴序列这两者是为一体的操作;即使复制和粘贴区域重叠，粘贴的序列也会有拷贝来的值。     

`copyWithin` 函数被设计为通用式的，其不要求其 `this` 值必须是一个数组对象。   

`copyWithin` 是一个可变方法，它不会改变 this 的长度 length，但是会改变 this 本身的内容，且需要时会创建新的属性。

#### 例子
```` JavaScript
var arr1 = [1, 2, 3, 4, 5],
    arr2 = [1, 2, 3, 4, 5],
    arr3 = [1, 2, 3, 4, 5],
    arr4 = [1, 2, 3, 4, 5],
    
arr1.copyWithin(-2);
arr2.copyWithin(0, 3)
arr3.copyWithin(0, 3, 4)
arr4.copyWithin(-2, -3, -1)

console.log(arr1);     // [1, 2, 3, 1, 2]
console.log(arr2);     // [4, 5, 3, 4, 5]
console.log(arr3);     // [4, 2, 3, 4, 5]
console.log(arr4);     // [1, 2, 3, 3, 4]
````
#### 源码
假如不存在 `Array.prototype.copyWithin()`，则在其他代码之前运行下面的代码将创建该方法。
```` JavaScript
if (!Array.prototype.copyWithin) {
  Array.prototype.copyWithin = function(target, start, end) {
  
    if (this == null) {
      throw new TypeError('this is null or not defined');
    }

    var O = Object(this);

    // 拿到数组length并进行零填充右位移运算
    var len = O.length >>> 0;

    // 拿到第一个参数并进行有符号右位移运算，结果作为target参数
    var relativeTarget = target >> 0;

    var to = relativeTarget < 0 ?
      Math.max(len + relativeTarget, 0) :
      Math.min(relativeTarget, len);

    // 拿到第二个参数并进行有符号右位移运算
    var relativeStart = start >> 0;
    
    // 确定开始复制元素的起始位置
    var from = relativeStart < 0 ?
      Math.max(len + relativeStart, 0) :
      Math.min(relativeStart, len);

    // 拿到第三个参数并进行有符号右位移运算
    var end = arguments[2];
    var relativeEnd = end === undefined ? len : end >> 0;

    // 确定开始复制元素的结束位置
    var final = relativeEnd < 0 ?
      Math.max(len + relativeEnd, 0) :
      Math.min(relativeEnd, len);

    // 确定复制的个数
    var count = Math.min(final - from, len - to);

    // 标志，决定是从前往后，还是从后往前拷贝
    var direction = 1;

    if (from < to && to < (from + count)) {
      direction = -1;
      from += count - 1;
      to += count - 1;
    }

    // 开始拷贝
    while (count > 0) {
      if (from in O) {
        O[to] = O[from];
      } else {
        delete O[to];
      }
      from += direction;
      to += direction;
      count--;
    }

    // 结果返回
    return O;
  };
}
````
### `Array.prototype.entries()`     
**`entries() `** 方法返回一个新的 **Array Iterator** 对象，该对象包含数组中每个索引的键/值对。    
#### 语法   
&#8195;   *arr* .entries()    
##### 返回值       
**`target  `**      
&#8195;  一个新的 `Array` 迭代器对象。Array Iterator是对象，它的原型（__proto__:Array Iterator）上有一个next方法，可用用于遍历迭代器取得原数组的[key,value]。   
### 示例   
**1、Array Iterator**    
```` JavaScript
var arr = ["a", "b", "c"];
var iterator = arr.entries();
console.log(iterator);

/*Array Iterator {}
         __proto__:Array Iterator
         next:ƒ next()
         Symbol(Symbol.toStringTag):"Array Iterator"
         __proto__:Object
*/
````
**2、iterator.next()**    
```` JavaScript
var arr = ["a", "b", "c"];
var iterator = arr.entries();
console.log(iterator.next());

/*{value: Array(2), done: false}
          done:false
          value:(2) [0, "a"]
           __proto__: Object
*/
// iterator.next()返回一个对象，对于有元素的数组，是next{ value: Array(2), done: false }；next.done 用于指示迭代器是否完成：在每次迭代时进行更新而且都是false，直到迭代器结束done才是true。next.value是一个["key","value"]的数组，是返回的迭代器中的元素值。
````
**3、iterator.next方法实例**    
```` JavaScript
var arr = ["a", "b", "c"];
var iter = arr.entries();
var a = [];

// for(var i=0; i< arr.length; i++){   // 实际使用的是这个
for(var i=0; i< arr.length+1; i++){    // 注意，是length+1，为了查看遍历迭代器结束时的done，所以比数组的长度大
    var tem = iter.next();             // 每次迭代时更新next
    console.log(tem.done);             // 这里可以看到更新后的done都是false
    if(tem.done !== true){             // 遍历迭代器结束done才是true
        console.log(tem.value);
        a[i]=tem.value;
    }
}

console.log(a);                         // 遍历完毕，输出next.value的数组
````
**4、二维数组按行排序**    
```` JavaScript
function sortArr(arr) {
    var goNext = true;
    var entries = arr.entries();
    while (goNext) {
        var result = entries.next();
        if (result.done !== true) {
            result.value[1].sort((a, b) => a - b);
            goNext = true;
        } else {
            goNext = false;
        }
    }
    return arr;
}

var arr = [[1,34],[456,2,3,44,234],[4567,1,4,5,6],[34,78,23,1]];
sortArr(arr);

/*(4) [Array(2), Array(5), Array(5), Array(4)]
    0:(2) [1, 34]
    1:(5) [2, 3, 44, 234, 456]
    2:(5) [1, 4, 5, 6, 4567]
    3:(4) [1, 23, 34, 78]
    length:4
    __proto__:Array(0)
*/
````
**5、使用for…of 循环**    
```` JavaScript
var arr = ["a", "b", "c"];
var iterator = arr.entries();

for (let e of iterator) {
    console.log(e);
}

// [0, "a"]
// [1, "b"]
// [2, "c"]
```` 
### `Array.prototype.fill()`   
**`fill() `** 方法用一个固定值填充一个数组中从起始索引到终止索引内的全部元素。不包括终止索引。    
#### 语法   
&#8195;   *arr* .fill(value, start, end)   
##### 参数    
**`value  `**      
> 用来填充数组元素的值。

**`end ` ` 可选`**      
>起始索引，默认值为0。

**`end`` 可选`**   
> 终止索引，默认值为 `arr.length`。
##### 返回值    
&#8195;  修改后的数组。
#### 描述    
**`fill`** 方法接受三个参数 `value`, `start` 以及 `end`, `start` 和 `end` 参数是可选的, 其默认值分别为 `0` 和 `this` 对象的 `length` 属性值。   

如果 `star`t 是个负数, 则开始索引会被自动计算成为 `length+start`, 其中 `length` 是 `this` 对象的 `length` 属性值。如果 `end` 是个负数, 则结束索引会被自动计算成为 `length+end`。   

**`fill`** 方法故意被设计成通用方法, 该方法不要求 `this` 是数组对象。

**`fill`** 方法是个可变方法, 它会改变调用它的 this 对象本身, 然后返回它, 而并不是返回一个副本。

当一个对象被传递给 **`fill`** 方法的时候, 填充数组的是这个对象的引用。
#### 示例   
```` JavaScript
[1, 2, 3].fill(4);               // [4, 4, 4]
[1, 2, 3].fill(4, 1);            // [1, 4, 4]
[1, 2, 3].fill(4, 1, 2);         // [1, 4, 3]
[1, 2, 3].fill(4, 1, 1);         // [1, 2, 3]
[1, 2, 3].fill(4, 3, 3);         // [1, 2, 3]
[1, 2, 3].fill(4, -3, -2);       // [4, 2, 3]
[1, 2, 3].fill(4, NaN, NaN);     // [1, 2, 3]
[1, 2, 3].fill(4, 3, 5);         // [1, 2, 3]
Array(3).fill(4);                // [4, 4, 4]

// Objects by reference.
var arr = Array(3).fill({});  // [{}, {}, {}]

// 需要注意如果fill的参数为引用类型，会导致都执行都一个引用类型
console.log(arr[0] === arr[1])  // true
arr[0].hi = "hi"; 
console.log(arr)// [{ hi: "hi" }, { hi: "hi" }, { hi: "hi" }]
````
##### 源码
```` JavaScript
if (!Array.prototype.fill) {
  Object.defineProperty(Array.prototype, 'fill', {
    value: function(value) {

      // Steps 1-2.
      if (this == null) {
        throw new TypeError('this is null or not defined');
      }

      var O = Object(this);

      // Steps 3-5.
      var len = O.length >>> 0;

      // Steps 6-7.
      var start = arguments[1];
      var relativeStart = start >> 0;

      // Step 8.
      var k = relativeStart < 0 ?
        Math.max(len + relativeStart, 0) :
        Math.min(relativeStart, len);

      // Steps 9-10.
      var end = arguments[2];
      var relativeEnd = end === undefined ?
        len : end >> 0;

      // Step 11.
      var final = relativeEnd < 0 ?
        Math.max(len + relativeEnd, 0) :
        Math.min(relativeEnd, len);

      // Step 12.
      while (k < final) {
        O[k] = value;
        k++;
      }

      // Step 13.
      return O;
    }
  });
}
````
### `Array.prototype.find()`
**`find() `** 方法返回数组中满足提供的测试函数的第一个元素的值。否则返回 `undefined`。

另请参见  `findIndex()` 方法，它返回数组中找到的元素的索引，而不是其值。

如果需要找到一个元素的位置或者一个元素是否存在于数组中，使用`Array.prototype.indexOf()` 或 `Array.prototype.includes()`。
#### 语法   
&#8195;   *arr* .find(*callback*, *thisArg*) 
##### 参数    
**`callback  `**      
> 在数组每一项上执行的函数，接收 3 个参数：
- **`element `:** 当前遍历到的元素。   
- **`index `:** 当前遍历到的索引。   **`可选`**
- **`array `:** 数组本身。     **`可选`**

**`thisArg ` ` 可选`**      
> 执行回调时用作this 的对象
##### 返回值    
&#8195;  数组中第一个满足所提供测试函数的元素的值，否则返回 `undefined`。
#### 描述
find方法对数组中的每一项元素执行一次 `callback` 函数，直至有一个 `callback` 返回 `true`。当找到了这样一个元素后，该方法会立即返回这个元素的值，否则返回 `undefined`。注意 `callback` 函数会为数组中的每个索引调用即从 `0` 到 `length - 1`，而不仅仅是那些被赋值的索引，这意味着对于稀疏数组来说，该方法的效率要低于那些只遍历有值的索引的方法。    

`callback`函数带有3个参数：当前元素的值、当前元素的索引，以及数组本身。   

如果提供了 `thisAr`g参数，那么它将作为每次 `callback`函数执行时的`this` ，如果未提供，则使用 `undefined`。   

`find`方法不会改变数组。   

在第一次调用 `callback`函数时会确定元素的索引范围，因此在 `find`方法开始执行之后添加到数组的新元素将不会被 `callback`函数访问到。如果数组中一个尚未被`callback`函数访问到的元素的值被`callback`函数所改变，那么当`callback`函数访问到它时，它的值是将是根据它在数组中的索引所访问到的当前值。被删除的元素仍旧会被访问到，但是其值已经是`undefined`了。
