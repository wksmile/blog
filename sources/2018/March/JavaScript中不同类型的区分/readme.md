<table>
<tr>
<td> Value </td><td> function </td><td> typeOf </td>
</tr>
<tr>
<td> "foo" </td><td> String </td><td> string </td>
</tr>
<tr><td>new String("foo")</td><td>   String</td> <td> object</td></tr>
<tr><td>1.2</td> <td>  Number</td><td> number</td></tr>
<tr><td>new Number(1.2) </td><td>  Number </td><td> object</td></tr>
<tr><td>true       </td><td>    Boolean</td><td>  boolean</td></tr>
<tr><td>new Boolean(true) </td><td> Boolean</td><td>  object</td></tr>
<tr><td>new Date()</td><td>   Date</td><td> object</td></tr>
<tr><td>new Error() </td><td> Error</td><td>object</td></tr>
<tr><td>[1,2,3]</td><td>Array</td><td> object</td></tr>
<tr><td>new Array(1, 2, 3)</td><td>Array </td><td>object</td></tr>
<tr><td>new Function("")</td><td>Function</td><td> function</td></tr>
<tr><td>/abc/g </td><td> RegExp </td><td> object</td></tr>
<tr><td>new RegExp("meow")</td><td> RegExp</td><td> object</td></tr>
<tr><td>{}    </td><td> Object </td><td>object</td></tr>
<tr><td>new Object() </td><td> Object</td><td> object</td></tr>
<tr><td>null</td><td> Object</td><td> object</td></tr>
<tr><td>undefined </td><td></td><td>  undefined</td></tr>
</table>

所有可以通过构造函数创建的对象都可以用 instanceof 检查类型

Object.prototype.toString.call(x)   来检查x的类型，可以区分Boolean, Number, String, Function, Array, Date, RegExp, Object, Error等等。

Javascript中有5种基本类型(除了symbol，es6中增加了symbol类型)，以及对象类型，相信很多朋友像我一样，用了很久的js还是有点分不清怎么判断一个数据的类型。

### typeof 运算符：

    // Numbers
    console.log(typeof 37 === 'number');
    console.log(typeof 3.14 === 'number');
    console.log(typeof Math.LN2 === 'number');
    console.log(typeof Infinity === 'number');
    console.log(typeof NaN === 'number'); // 尽管NaN是"Not-A-Number"的缩写,意思是"不是一个数字"
    console.log(typeof Number(1) === 'number'); // 不要这样使用!
      
    // Strings
    console.log(typeof "" === 'string');
    console.log(typeof "bla" === 'string');
    console.log(typeof (typeof 1) === 'string'); // console.log(typeof返回的肯定是一个字符串
    console.log(typeof String("abc") === 'string');  //不要这样使用!
      
    // Booleans
    console.log(typeof true === 'boolean');
    console.log(typeof false === 'boolean');
    console.log(typeof Boolean(true) === 'boolean'); // 不要这样使用!
      
    // Symbols
    console.log(typeof Symbol() === 'symbol');
    console.log(typeof Symbol('foo') === 'symbol');
    console.log(typeof Symbol.iterator === 'symbol');
      
    // Undefined
    console.log(typeof undefined === 'undefined');
    console.log(typeof blabla === 'undefined'); // 一个未定义的变量,或者一个定义了却未赋初值的变量
      
    // Objects 使用Array.isArray或者Object.prototype.toString.call方法可以从基本的对象中区分出数组类型
    console.log(typeof {a:1} === 'object');
    console.log(typeof [1, 2, 4] === 'object');
    console.log(typeof /^[a-zA-Z]{5,20}$/ === 'object');
    console.log(typeof {name:'wenzi', age:25} === 'object');
    console.log(typeof null === 'object');//true
      
    // 下面的容易令人迷惑，不要这样使用！
    console.log(typeof new Boolean(true) === 'object');
    console.log(typeof new Number(1) === 'object');
    console.log(typeof new Date() === 'object');
    console.log(typeof new String("abc") === 'object');
    console.log(typeof new Error() === 'object');
      
    // 函数
    console.log(typeof function(){} === 'function');
    console.log(typeof Math.sin === 'function');

通过上面例子我们可以很明显的看到，除了基本类型以外的类型，都是对象，但是有例外：null 的 typeof 值是 'object' 【坑1】, 函数的 typeof 值是 'function' ! (函数对象的构造函数是 Function，也就继承了 Function 的原型)【坑2】，NaN 的类型也是 'number'【坑3】

注意：本文的测试在现在最新浏览器上进行，老版本浏览器可能有所不同。比如Safari 3.X中typeof /^\d*$/;为'function'【坑4:兼容性复杂】。

不是所有对象都是返回 object，而且还有 null 捣乱，那我们如何判断一个值的类型呢

### instanceof运算符：   instanceof又叫关系运算符，可以用来判断某个构造函数的prototype属性是否存在另外一个要检测对象的原型链上

    var str = new String("hello world");
    console.log(str instanceof String);//true
    console.log(String instanceof Function);//true
    console.log(str instanceof Function);//false

### constructor属性   在使用instanceof检测变量类型时，我们是检测不到number, 'string', bool的类型的

    function Person(){
      
    }
    var Tom = new Person();
      
    console.log(Tom.constructor === Person);//true

### Object.prototype.toString.call
使用toString()方法来检测对象类型

    function Type() { };
      
    var toString = Object.prototype.toString;
    console.log(toString.call(new Date) === '[object Date]');//true
    console.log(toString.call(new String) ==='[object String]');//true
    console.log(toString.call(new Function) ==='[object Function]');//true
    console.log(toString.call(Type) ==='[object Function]');//true
    console.log(toString.call('str') ==='[object String]');//true
    console.log(toString.call(Math) === '[object Math]');//true
    console.log(toString.call(true) ==='[object Boolean]');//true
    console.log(toString.call(/^[a-zA-Z]{5,20}$/) ==='[object RegExp]');//true
    console.log(toString.call({name:'wenzi', age:25}) ==='[object Object]');//true
    console.log(toString.call([1, 2, 3, 4]) ==='[object Array]');//true
    //Since JavaScript 1.8.5
    console.log(toString.call(undefined) === '[object Undefined]');//true
    console.log(toString.call(null) === '[object Null]');//true

###  Array.isArray() 用于确定传递的值是否是一个 Array

    // 下面的函数调用都返回 true
    Array.isArray([]);
    Array.isArray([1]);
    Array.isArray(new Array());
    // 鲜为人知的事实：其实 Array.prototype 也是一个数组。
    Array.isArray(Array.prototype); 
    
    // 下面的函数调用都返回 false
    Array.isArray();
    Array.isArray({});
    Array.isArray(null);
    Array.isArray(undefined);
    Array.isArray(17);
    Array.isArray('Array');
    Array.isArray(true);
    Array.isArray(false);
    Array.isArray({ __proto__: Array.prototype });
    instanceof 和 isArray
    
当检测Array实例时, Array.isArray 优于 instanceof,因为Array.isArray能检测iframes.
通过如下代码可以创建该方法

    if (!Array.isArray) {
      Array.isArray = function(arg) {
        return Object.prototype.toString.call(arg) === '[object Array]';
      };
    }