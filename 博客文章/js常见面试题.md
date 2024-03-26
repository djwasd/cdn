# **闭包**

•闭包就是能够读取其他函数内部变量的函数

•闭包是指有权访问另一个函数作用域中变量的函数，创建闭包的最常见的方式就是在一个函数内创建另一个函数，通过另一个函数访问这个函数的局部变量,利用闭包可以突破作用链域

•闭包的特性：函数内再嵌套函数内部函数可以引用外层的参数和变量参数和变量不会被垃圾回收机制回收

**说说你对闭包的理解**

•使用闭包主要是为了设计私有的方法和变量。闭包的优点是可以避免全局变量的污染，缺点是闭包会常驻内存，会增大内存使用量，使用不当很容易造成内存泄露。在js中，函数即闭包，只有函数才会产生作用域的概念

•闭包 的最大用处有两个，一个是可以读取函数内部的变量，另一个就是让这些变量始终保持在内存中

•闭包的另一个用处，是封装对象的私有属性和私有方法

•好处：能够实现封装和缓存等；

•坏处：就是消耗内存、不正当使用会造成内存溢出的问题

**使用闭包的注意点**

•由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露

•解决方法是，在退出函数之前，将不使用的局部变量全部删除\

•常用场景就是[**防抖节流**](https://blog.csdn.net/YZY010313/article/details/126817841)

# **事件模型**

事件流分为三个阶段：捕获阶段、目标阶段和冒泡阶段。

1.**捕获阶段（Capture Phase）**：事件从最外层的父节点开始向下传递，直到达到目标元素的父节点。在捕获阶段，事件会经过父节点、祖父节点等，但不会触发任何事件处理程序。

2.**目标阶段（Target Phase）**：事件到达目标元素本身，触发目标元素上的事件处理程序。如果事件有多个处理程序绑定在目标元素上，它们会按照添加的顺序依次执行。

3.**冒泡阶段（Bubble Phase）**：事件从目标元素开始向上冒泡，传递到父节点，直到传递到最外层的父节点或根节点。在冒泡阶段，事件会依次触发父节点、祖父节点等的事件处理程序。

事件流的默认顺序是从目标元素的最外层父节点开始的捕获阶段，然后是目标阶段，最后是冒泡阶段。但是可以通过事件处理程序的绑定顺序来改变事件处理的执行顺序。

```
<div id="outer">
  <div id="inner">
    <button id="btn">Click me</button>
  </div>
</div>
let outer = document.getElementById('outer');
let inner = document.getElementById('inner');
let btn = document.getElementById('btn');

outer.addEventListener('click', function() {
  console.log('Outer div clicked'); 
}, true); // 使用捕获阶段进行事件监听

inner.addEventListener('click', function() {
  console.log('Inner div clicked'); 
}, false); // 使用冒泡阶段进行事件监听

btn.addEventListener('click', function() {
  console.log('Button clicked');
}, false); // 使用冒泡阶段进行事件监听
```

当点击按钮时，事件的执行顺序如下：

1.捕获阶段：触发外层div的捕获事件处理程序。

2.目标阶段：触发按钮的事件处理程序。

3.冒泡阶段：触发内层div的冒泡事件处理程序。

```
Outer div clicked
Button clicked
Inner div clicked
```

这个示例展示了事件流中捕获阶段、目标阶段和冒泡阶段的执行顺序。

可以通过**addEventListener**方法的第三个参数来控制事件处理函数在捕获阶段或冒泡阶段执行，true表示捕获阶段，false或不传表示冒泡阶段

# **New的原理**

new 关键词的主要作用就是执行一个构造函数、返回一个实例对象，在 new 的过程中，根据构造函数的情况，来确定是否可以接受参数的传递。下面我们通过一段代码来看一个简单的 new 的例子

```
function Person(){
   this.name = 'Jack';
}
var p = new Person(); 
console.log(p.name)  // Jack
```

这段代码比较容易理解，从输出结果可以看出，p 是一个通过 person 这个构造函数生成的一个实例对象，这个应该很容易理解

new 操作符可以帮助我们构建出一个实例，并且绑定上 this，内部执行步骤可大概分为以下几步：

•创建一个新对象

•对象连接到构造函数原型上，并绑定 `this`（this 指向新对象）

•执行构造函数代码（为这个新对象添加属性）

•返回新对象

在第四步返回新对象这边有一个情况会例外:

```
function Person(){
  this.name = 'Jack';
}
var p = Person();
console.log(p) // undefined
console.log(name) // Jack
console.log(p.name) // 'name' of undefined
```

•从上面的代码中可以看到，我们没有使用 `new` 这个关键词，返回的结果就是 `undefined`。其

中由于 `JavaScript` 代码在默认情况下 `this` 的指向是 `window`，那么 `name` 的输出结果就为

`Jack`，这是一种不存在 `new` 关键词的情况。

•那么当构造函数中有 `return` 一个对象的操作，结果又会是什么样子呢？我们再来看一段在上面的基础上改造过的代码。

```
function Person(){
   this.name = 'Jack'; 
   return {age: 18}
}
var p = new Person(); 
console.log(p)  // {age: 18}
console.log(p.name) // undefined
console.log(p.age) // 18
 
```

通过这段代码又可以看出，当构造函数最后 return 出来的是一个和 this 无关的对象时，new 命令会直接返回这个新对象，而不是通过 new 执行步骤生成的 this 对象

但是这里要求构造函数必须是返回一个对象，如果返回的不是对象，那么还是会按照 new 的实现步骤，返回新生成的对象。接下来还是在上面这段代码的基础之上稍微改动一下

```
function Person(){
   this.name = 'Jack'; 
   return 'tom';
}
var p = new Person(); 
console.log(p)  // {name: 'Jack'}
console.log(p.name) // Jack
```

可以看出，当构造函数中 return 的不是一个对象时，那么它还是会根据 new 关键词的执行逻辑，生成一个新的对象（绑定了最新 this），最后返回出来

因此我们总结一下：new 关键词执行之后总是会返回一个对象，要么是实例对象，要么是 return 语句指定的对象

![img](https://cdn.jsdelivr.net/gh/djwasd/cdn@v0.2/images/20210414151626.png)

```
手工实现New的过程
function create(fn, ...args) {
  if(typeof fn !== 'function') {
    throw 'fn must be a function';
  }
	// 1、用new Object() 的方式新建了一个对象obj
  // var obj = new Object()
	// 2、给该对象的__proto__赋值为fn.prototype，即设置原型链
  // obj.__proto__ = fn.prototype

  // 1、2步骤合并
  // 创建一个空对象，且这个空对象继承构造函数的 prototype 属性
  // 即实现 obj.__proto__ === constructor.prototype
  var obj = Object.create(fn.prototype);

	// 3、执行fn，并将obj作为内部this。使用 apply，改变构造函数 this 的指向到新建的对象，这样 obj 就可以访问到构造函数中的属性
  var res = fn.apply(obj, args);
	// 4、如果fn有返回值，则将其作为new操作返回内容，否则返回obj
	return res instanceof Object ? res : obj;
};
 
```

•使用 `Object.create` 将 `obj 的`proto `指向为构造函数的原型`；

•使用 `apply` 方法，将构造函数内的 `this` 指向为 `obj`；

•在 `create` 返回时，使用三目运算符决定返回结果。

我们知道，`构造函数如果有显式返回值，且返回值为对象类型，那么构造函数返回结果不再是目标实例`

```
function Person(name) {
  this.name = name
  return {1: 1}
}
const person = new Person(Person, 'lucas')
console.log(person)
// {1: 1}
//使用create代替new
function Person() {...}
// 使用内置函数new
var person = new Person(1,2)

// 使用手写的new，即create
var person = create(Person, 1,2)
 
```

**new 被调用后大致做了哪几件事情**

•让实例可以访问到私有属性；

•让实例可以访问构造函数原型（`constructor.prototype`）所在原型链上的属性；

•构造函数返回的最后结果是引用数据类型。

# **原型/原型链**

`**__proto__和prototype关系**`：__proto__和constructor是**对象**独有的。prototype属性是**函数**独有的

> 在 js 中我们是使用构造函数来新建一个对象的，每一个构造函数的内部都有一个 prototype 属性值，这个属性值是一个对象，这个对象包含了可以由该构造函数的所有实例共享的属性和方法。当我们使用构造函数新建一个对象后，在这个对象的内部将包含一个指针，这个指针指向构造函数的 prototype 属性对应的值，在 ES5 中这个指针被称为对象的原型。一般来说我们是不应该能够获取到这个值的，但是现在浏览器中都实现了 proto 属性来让我们访问这个属性，但是我们最好不要使用这个属性，因为它不是规范中规定的。ES5 中新增了一个 Object.getPrototypeOf() 方法，我们可以通过这个方法来获取对象的原型。

当我们访问一个对象的属性时，如果这个对象内部不存在这个属性，那么它就会去它的原型对象里找这个属性，这个原型对象又会有自己的原型，于是就这样一直找下去，也就是原型链的概念。原型链的尽头一般来说都是 `Object.prototype` 所以这就是我们新建的对象为什么能够使用 `toString()` 等方法的原因。

> 特点：JavaScript 对象是通过引用来传递的，我们创建的每个新对象实体中并没有一份属于自己的原型副本。当我们修改原型时，与 之相关的对象也会继承这一改变

•原型(`prototype`): 一个简单的对象，用于实现对象的 属性继承。可以简单的理解成对象的爹。在 `Firefox` 和 `Chrome` 中，每个 `JavaScript`对象中都包含一个 `__proto__`(非标准)的属性指向它爹(该对象的原型)，可 `obj.__proto__`进行访问。

•构造函数: 可以通过 `new`来 新建一个对象 的函数。

•实例: 通过构造函数和 `new`创建出来的对象，便是实例。 实例通过 `__proto__`指向原型，通过 `constructor`指向构造函数。

> 以Object为例，我们常用的Object便是一个构造函数，因此我们可以通过它构建实例。

```
// 实例
const instance = new Object()
```

> 则此时， 实例为instance, 构造函数为Object，我们知道，构造函数拥有一个prototype的属性指向原型，因此原型为:

```
// 原型
const prototype = Object.prototype
```

**这里我们可以来看出三者的关系:**

•`实例.__proto__ === 原型`

•`原型.constructor === 构造函数`

•`构造函数.prototype === 原型`

```
// 这条线其实是是基于原型进行获取的，可以理解成一条基于原型的映射线
// 例如: 
// const o = new Object()
// o.constructor === Object   --> true
// o.__proto__ = null;
// o.constructor === Object   --> false
实例.constructor === 构造函数
 
```

![img](https://cdn.jsdelivr.net/gh/djwasd/cdn@v0.2/images/112.png)

**原型链**

> 原型链是由原型对象组成，每个对象都有 __proto__ 属性，指向了创建该对象的构造函数的原型，__proto__ 将对象连接起来组成了原型链。是一个用来实现继承和共享属性的有限的对象链

•属性查找机制: 当查找对象的属性时，如果实例对象自身不存在该属性，则沿着原型链往上一级查找，找到时则输出，不存在时，则继续沿着原型链往上一级查找，直至最顶级的原型对象 `Object.prototype`，如还是没找到，则输出 `undefined`；

•属性修改机制: 只会修改实例对象本身的属性，如果不存在，则进行添加该属性，如果需要修改原型的属性时，则可以用: `b.prototype.x = 2`；但是这样会造成所有继承于该对象的实例的属性发生改变。

**js 获取原型的方法**

•`p.proto`

•`p.constructor.prototype`

•`Object.getPrototypeOf(p)`

**总结**

![img](https://cdn.jsdelivr.net/gh/djwasd/cdn@v0.2/images/105.png)

•每个函数都有 `prototype` 属性，除了 `Function.prototype.bind()`，该属性指向原型。

•每个对象都有 `__proto__` 属性，指向了创建该对象的构造函数的原型。其实这个属性指向了 `[[prototype]]`，但是 `[[prototype]]`是内部属性，我们并不能访问到，所以使用 `_proto_`来访问。

•对象可以通过 `__proto__` 来寻找不属于该对象的属性，`__proto__` 将对象连接起来组成了原型链。

# **继承**![img](https://cdn.jsdelivr.net/gh/djwasd/cdn@v0.2/images/20210414142754.png)

> 涉及面试题：原型如何实现继承？Class 如何实现继承？Class 本质是什么？

首先先来看下 `class`，其实在 `JS`中并不存在类，`class` 只是语法糖，本质还是函数

```
class Person {}
Person instanceof Function // true
```

**组合继承**

> 组合继承是最常用的继承方式

```
function Parent(value) {
  this.val = value
}
Parent.prototype.getValue = function() {
  console.log(this.val)
}
function Child(value) {
  Parent.call(this, value)
}
Child.prototype = new Parent()

const child = new Child(1)

child.getValue() // 1
child instanceof Parent // true
```

•以上继承的方式核心是在子类的构造函数中通过 `Parent.call(this)` 继承父类的属性，然后改变子类的原型为 `new Parent()` 来继承父类的函数。

•这种继承方式优点在于构造函数可以传参，不会与父类引用属性共享，可以复用父类的函数，但是也存在一个缺点就是在继承父类函数的时候调用了父类构造函数，导致子类的原型上多了不需要的父类属性，存在内存上的浪费

**寄生组合继承**

> 这种继承方式对组合继承进行了优化，组合继承缺点在于继承父类函数时调用了构造函数，我们只需要优化掉这点就行了

```
function Parent(value) {
  this.val = value
}
Parent.prototype.getValue = function() {
  console.log(this.val)
}

function Child(value) {
  Parent.call(this, value)
}
Child.prototype = Object.create(Parent.prototype, {
  constructor: {
    value: Child,
    enumerable: false,
    writable: true,
    configurable: true
  }
})

const child = new Child(1)

child.getValue() // 1
child instanceof Parent // true
```

> 以上继承实现的核心就是将父类的原型赋值给了子类，并且将构造函数设置为子类，这样既解决了无用的父类属性问题，还能正确的找到子类的构造函数。

**Class 继承**

> 以上两种继承方式都是通过原型去解决的，在 ES6 中，我们可以使用 class 去实现继承，并且实现起来很简单

```
class Parent {
  constructor(value) {
    this.val = value
  }
  getValue() {
    console.log(this.val)
  }
}
class Child extends Parent {
  constructor(value) {
    super(value)
    this.val = value
  }
}
let child = new Child(1)
child.getValue() // 1
child instanceof Parent // true
```

> class 实现继承的核心在于使用 extends 表明继承自哪个父类，并且在子类构造函数中必须调用 super，因为这段代码可以看成 Parent.call(this, value)。

**ES5 和 ES6 继承的区别：**

•ES6 继承的子类需要调用 `super()` 才能拿到子类，ES5 的话是通过 `apply` 这种绑定的方式

•类声明不会提升，和 `let` 这些一致

```
function Super() {}
Super.prototype.getNumber = function() {
  return 1
}

function Sub() {}
Sub.prototype = Object.create(Super.prototype, {
  constructor: {
    value: Sub,
    enumerable: false,
    writable: true,
    configurable: true
  }
})
let s = new Sub()
s.getNumber()
 
```

> 几种常见的继承方式

**1. 方式1: 借助call**

```
 function Parent1(){
    this.name = 'parent1';
  }
  function Child1(){
    Parent1.call(this);
    this.type = 'child1'
  }
  console.log(new Child1);
 
```

这样写的时候子类虽然能够拿到父类的属性值，但是问题是父类原型对象中一旦存在方法那么子类无法继承。那么引出下面的方法。

**2. 方式2: 借助原型链**

```
 function Parent2() {
    this.name = 'parent2';
    this.play = [1, 2, 3]
  }
  function Child2() {
    this.type = 'child2';
  }
  Child2.prototype = new Parent2();

  console.log(new Child2());
```

看似没有问题，父类的方法和属性都能够访问，但实际上有一个潜在的不足。举个例子：

```
var s1 = new Child2();
var s2 = new Child2();
s1.play.push(4);
console.log(s1.play, s2.play);
[1,2,3,4]   [1,2,3,4]  
```

可以看到控制台只改变了s1的play属性，为什么s2也跟着变了呢？很简单，因为两个实例使用的是同一个原型对象。

**3. 方式3：将前两种组合**

```
  function Parent3 () {
    this.name = 'parent3';
    this.play = [1, 2, 3];
  }
  function Child3() {
    Parent3.call(this);
    this.type = 'child3';
  }
  Child3.prototype = new Parent3();
  var s3 = new Child3();
  var s4 = new Child3();
  s3.play.push(4);
  console.log(s3.play, s4.play);
  [1,2,3,4]   [1,2,3] 
```

可以看到控制台：

> 之前的问题都得以解决。但是这里又徒增了一个新问题，那就是Parent3的构造函数会多执行了一次（Child3.prototype = new Parent3();）。这是我们不愿看到的。那么如何解决这个问题？

**4. 方式4: 组合继承的优化1**

```
  function Parent4 () {
    this.name = 'parent4';
    this.play = [1, 2, 3];
  }
  function Child4() {
    Parent4.call(this);
    this.type = 'child4';
  }
  Child4.prototype = Parent4.prototype;
```

这里让将父类原型对象直接给到子类，父类构造函数只执行一次，而且父类属性和方法均能访问，但是我们来测试一下：

```
var s3 = new Child4();
var s4 = new Child4();
console.log(s3)
```

![img](https://cdn.jsdelivr.net/gh/djwasd/cdn@v0.2/images/20210309103358.png)

**5. 方式5(最推荐使用): 组合继承的优化2**

```
 function Parent5 () {
    this.name = 'parent5';
    this.play = [1, 2, 3];
  }
  function Child5() {
    Parent5.call(this);
    this.type = 'child5';
  }
  Child5.prototype = Object.create(Parent5.prototype);
  Child5.prototype.constructor = Child5;
```

这是最推荐的一种方式，接近完美的继承，它的名字也叫做寄生组合继承。

**6. ES6的extends被编译后的JavaScript代码**

> ES6的代码最后都是要在浏览器上能够跑起来的，这中间就利用了babel这个编译工具，将ES6的代码编译成ES5让一些不支持新语法的浏览器也能运行。

那最后编译成了什么样子呢？

```
function _possibleConstructorReturn(self, call) {
    // ...
    return call && (typeof call === 'object' || typeof call === 'function') ? call : self;
}

function _inherits(subClass, superClass) {
    // ...
    //看到没有
    subClass.prototype = Object.create(superClass && superClass.prototype, {
        constructor: {
            value: subClass,
            enumerable: false,
            writable: true,
            configurable: true
        }
    });
    if (superClass) Object.setPrototypeOf ? Object.setPrototypeOf(subClass, superClass) : subClass.__proto__ = superClass;
}


var Parent = function Parent() {
    // 验证是否是 Parent 构造出来的 this
    _classCallCheck(this, Parent);
};

var Child = (function (_Parent) {
    _inherits(Child, _Parent);

    function Child() {
        _classCallCheck(this, Child);

        return _possibleConstructorReturn(this, (Child.__proto__ || Object.getPrototypeOf(Child)).apply(this, arguments));
    }

    return Child;
}(Parent));
```

> 核心是_inherits函数，可以看到它采用的依然也是第五种方式————寄生组合继承方式，同时证明了这种方式的成功。不过这里加了一个Object.setPrototypeOf(subClass, superClass)，这是用来干啥的呢？

答案是用来继承父类的静态方法。这也是原来的继承方式疏忽掉的地方。

**追问: 面向对象的设计一定是好的设计吗？**

> 不一定。从继承的角度说，这一设计是存在巨大隐患的。

# **那些操作会造成内存泄漏？**

> JavaScript 内存泄露指对象在不需要使用它时仍然存在，导致占用的内存不能使用或回收

•未使用 `var` 声明的全局变量

•闭包函数(`Closures`)

•循环引用(两个对象相互引用)

•控制台日志(`console.log`)

•移除存在绑定事件的 `DOM`元素(`IE`)

•`setTimeout` 的第一个参数使用字符串而非函数的话，会引发内存泄漏

•垃圾回收器定期扫描对象，并计算引用了每个对象的其他对象的数量。如果一个对象的引用数量为 `0`（没有其他对象引用过该对象），或对该对象的惟一引用是循环的，那么该对象的内存即可回收

下面是一些常见操作可能导致内存泄漏的示例代码：

```
未使用 var 声明的全局变量：
function foo() {
  bar = 'global variable'; // 没有使用 var 声明
}
闭包函数（Closures）：
function outer() {
  var data = 'sensitive data';
  return function() {
    // 内部函数形成了闭包
    console.log(data);
  };
}
var inner = outer();
inner(); // 闭包引用了外部函数的变量，导致变量无法被释放

循环引用：
function createObjects() {
  var obj1 = {};
  var obj2 = {};
  obj1.ref = obj2;
  obj2.ref = obj1;
  // 对象之间形成循环引用，导致无法被垃圾回收
}
createObjects();
控制台日志（console.log）：
function processData(data) {
  console.log(data); // 控制台日志可能会引用数据，阻止垃圾回收
  // 处理数据的逻辑
}

移除存在绑定事件的 DOM 元素（IE）：
var element = document.getElementById('myElement');
element.onclick = function() {
  // 处理点击事件
};
// 移除元素时没有显式地解绑事件处理程序，可能导致内存泄漏（在 IE 浏览器中）
element.parentNode.removeChild(element);
使用字符串作为 setTimeout 的第一个参数：
setTimeout('console.log("timeout");', 1000);
// 使用字符串作为参数，会导致内存泄漏（不推荐
```

# **XML和JSON的区别？**

`XML`（可扩展标记语言）和 `JSON`（JavaScript对象表示法）是两种常用的数据格式，它们在以下几个方面有一些区别：

数据体积方面：

•`JSON`相对于XML来说，数据的体积小，因为JSON使用了较简洁的语法，所以传输的速度更快。

数据交互方面：

•`JSON`与JavaScript的交互更加方便，因为JSON数据可以直接被JavaScript解析和处理，无需额外的转换步骤。

•`XML`需要使用DOM操作来解析和处理数据，相对而言更复杂一些。

数据描述方面：

•`XML`对数据的描述性较强，它使用标签来标识数据的结构和含义，可以自定义标签名，使数据更具有可读性和可扩展性。

•`JSON`的描述性较弱，它使用简洁的键值对表示数据，适合于简单的数据结构和传递。

传输速度方面：

•`JSON`的解析速度要快于 `XML`，因为 `JSON`的语法更接近JavaScript对象的表示，JavaScript引擎能够更高效地解析JSON数据。

需要根据具体的需求和使用场景选择合适的数据格式，一般来说，如果需要简单、轻量级的数据交互，并且与JavaScript紧密集成，可以选择JSON。而如果需要较强的数据描述性和扩展性，或者需要与其他系统进行数据交互，可以选择XML。

# **模块化**

> js 中现在比较成熟的有四种模块加载方案：

•第一种是 CommonJS 方案，它通过 require 来引入模块，通过 module.exports 定义模块的输出接口。这种模块加载方案是服务器端的解决方案，它是以同步的方式来引入模块的，因为在服务端文件都存储在本地磁盘，所以读取非常快，所以以同步的方式加载没有问题。但如果是在浏览器端，由于模块的加载是使用网络请求，因此使用异步加载的方式更加合适。

•第二种是 AMD 方案，这种方案采用异步加载的方式来加载模块，模块的加载不影响后面语句的执行，所有依赖这个模块的语句都定义在一个回调函数里，等到加载完成后再执行回调函数。require.js 实现了 AMD 规范

•第三种是 CMD 方案，这种方案和 AMD 方案都是为了解决异步模块加载的问题，sea.js 实现了 CMD 规范。它和require.js的区别在于模块定义时对依赖的处理不同和对依赖模块的执行时机的处理不同。

•第四种方案是 ES6 提出的方案，使用 import 和 export 的形式来导入导出模块

> 在有 Babel 的情况下，我们可以直接使用 ES6的模块化

```
// file a.js
export function a() {}
export function b() {}
// file b.js
export default function() {}

import {a, b} from './a.js'
import XXX from './b.js'
```

**CommonJS**

> CommonJs 是 Node 独有的规范，浏览器中使用就需要用到 Browserify解析了。

```
// a.js
module.exports = {
    a: 1
}
// or
exports.a = 1

// b.js
var module = require('./a.js')
module.a // -> log 1
```

在上述代码中，module.exports 和 exports 很容易混淆，让我们来看看大致内部实现

```
var module = require('./a.js')
module.a
// 这里其实就是包装了一层立即执行函数，这样就不会污染全局变量了，
// 重要的是 module 这里，module 是 Node 独有的一个变量
module.exports = {
    a: 1
}
// 基本实现
var module = {
  exports: {} // exports 就是个空对象
}
// 这个是为什么 exports 和 module.exports 用法相似的原因
var exports = module.exports
var load = function (module) {
    // 导出的东西
    var a = 1
    module.exports = a
    return module.exports
};
 
```

> 再来说说 module.exports 和exports，用法其实是相似的，但是不能对 exports 直接赋值，不会有任何效果。

> 对于 CommonJS 和 ES6 中的模块化的两者区别是：

•前者支持动态导入，也就是 `require(${path}/xx.js)`，后者目前不支持，但是已有提案,前者是同步导入，因为用于服务端，文件都在本地，同步导入即使卡住主线程影响也不大。

•而后者是异步导入，因为用于浏览器，需要下载文件，如果也采用同步导入会对渲染有很大影响

•前者在导出时都是值拷贝，就算导出的值变了，导入的值也不会改变，所以如果想更新值，必须重新导入一次。

•但是后者采用实时绑定的方式，导入导出的值都指向同一个内存地址，所以导入值会跟随导出值变化

•后者会编译成 `require/exports` 来执行的

**AMD**

> AMD 是由 RequireJS 提出的

**AMD 和 CMD 规范的区别？**

•第一个方面是在模块定义时对依赖的处理不同。AMD推崇依赖前置，在定义模块的时候就要声明其依赖的模块。而 CMD 推崇就近依赖，只有在用到某个模块的时候再去 require。

•第二个方面是对依赖模块的执行时机处理不同。首先 AMD 和 CMD 对于模块的加载方式都是异步加载，不过它们的区别在于模块的执行时机，AMD 在依赖模块加载完成后就直接执行依赖模块，依赖模块的执行顺序和我们书写的顺序不一定一致。而 CMD在依赖模块加载完成后并不执行，只是下载而已，等到所有的依赖模块都加载好后，进入回调函数逻辑，遇到 require 语句的时候才执行对应的模块，这样模块的执行顺序就和我们书写的顺序保持一致了。

```
// CMD
define(function(require, exports, module) {
  var a = require("./a");
  a.doSomething();
  // 此处略去 100 行
  var b = require("./b"); // 依赖可以就近书写
  b.doSomething();
  // ...
});

// AMD 默认推荐
define(["./a", "./b"], function(a, b) {
  // 依赖必须一开始就写好
  a.doSomething();
  // 此处略去 100 行
  b.doSomething();
  // ...
})
```

•**AMD**：`requirejs` 在推广过程中对模块定义的规范化产出，提前执行，推崇依赖前置

•**CMD**：`seajs` 在推广过程中对模块定义的规范化产出，延迟执行，推崇依赖就近

•**CommonJs**：模块输出的是一个值的 `copy`，运行时加载，加载的是一个对象（`module.exports` 属性），该对象只有在脚本运行完才会生成

•**ES6 Module**：模块输出的是一个值的引用，编译时输出接口，`ES6`模块不是对象，它对外接口只是一种静态定义，在代码静态解析阶段就会生成。

**谈谈对模块化开发的理解**

•我对模块的理解是，一个模块是实现一个特定功能的一组方法。在最开始的时候，js 只实现一些简单的功能，所以并没有模块的概念，但随着程序越来越复杂，代码的模块化开发变得越来越重要。

•由于函数具有独立作用域的特点，最原始的写法是使用函数来作为模块，几个函数作为一个模块，但是这种方式容易造成全局变量的污染，并且模块间没有联系。

•后面提出了对象写法，通过将函数作为一个对象的方法来实现，这样解决了直接使用函数作为模块的一些缺点，但是这种办法会暴露所有的所有的模块成员，外部代码可以修改内部属性的值。

•现在最常用的是立即执行函数的写法，通过利用闭包来实现模块私有作用域的建立，同时不会对全局作用域造成污染。

# **Promise**

•`Promise` 是 `ES6` 新增的语法，解决了回调地狱的问题。

•可以把 `Promise`看成一个状态机。初始是 `pending` 状态，可以通过函数 `resolve` 和 `reject`，将状态转变为 `resolved` 或者 `rejected` 状态，状态一旦改变就不能再次变化。

•`then` 函数会返回一个 `Promise` 实例，并且该返回值是一个新的实例而不是之前的实例。因为 `Promise` 规范规定除了 `pending` 状态，其他状态是不可以改变的，如果返回的是一个相同实例的话，多个 `then` 调用就失去意义了。 对于 `then` 来说，本质上可以把它看成是 `flatMap`

**1. Promise 的基本情况**

> 简单来说它就是一个容器，里面保存着某个未来才会结束的事件（通常是异步操作）的结果。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息

一般 Promise 在执行过程中，必然会处于以下几种状态之一。

•待定（`pending`）：初始状态，既没有被完成，也没有被拒绝。

•已完成（`fulfilled`）：操作成功完成。

•已拒绝（`rejected`）：操作失败。

> 待定状态的 Promise 对象执行的话，最后要么会通过一个值完成，要么会通过一个原因被拒绝。当其中一种情况发生时，我们用 Promise 的 then 方法排列起来的相关处理程序就会被调用。因为最后 Promise.prototype.then 和 Promise.prototype.catch 方法返回的是一个 Promise， 所以它们可以继续被链式调用

关于 Promise 的状态流转情况，有一点值得注意的是，内部状态改变之后不可逆，你需要在编程过程中加以注意。文字描述比较晦涩，我们直接通过一张图就能很清晰地看出 Promise 内部状态流转的情况

![promise流程图](https://cdn.jsdelivr.net/gh/djwasd/cdn@v0.2/images/20210414175500.png)

从上图可以看出，我们最开始创建一个新的 `Promise` 返回给 `p1` ，然后开始执行，状态是 pending，当执行 `resolve`之后状态就切换为 `fulfilled`，执行 `reject` 之后就变为 `rejected` 的状态

**2. Promise 的静态方法**

•`all 方法`

•我们来看下业务的场景，对于下面这个业务场景页面的加载，将多个请求合并到一起，用 all 来实现可能效果会更好，请看代码片段

```
// 在一个页面中需要加载获取轮播列表、获取店铺列表、获取分类列表这三个操作，页面需要同时发出请求进行页面渲染，这样用 `Promise.all` 来实现，看起来更清晰、一目了然。


//1.获取轮播数据列表
function getBannerList(){
  return new Promise((resolve,reject)=>{
      setTimeout(function(){
        resolve('轮播数据')
      },300) 
  })
}
//2.获取店铺列表
function getStoreList(){
  return new Promise((resolve,reject)=>{
    setTimeout(function(){
      resolve('店铺数据')
    },500)
  })
}
//3.获取分类列表
function getCategoryList(){
  return new Promise((resolve,reject)=>{
    setTimeout(function(){
      resolve('分类数据')
    },700)
  })
}
function initLoad(){ 
  Promise.all([getBannerList(),getStoreList(),getCategoryList()])
  .then(res=>{
    console.log(res) 
  }).catch(err=>{
    console.log(err)
  })
} 
initLoad()
```

•allSettled 方法

•`Promise.allSettled` 的语法及参数跟 `Promise.all` 类似，其参数接受一个 `Promise` 的数组，返回一个新的 `Promise`。`唯一的不同在于，执行完之后不会失败`，也就是说当 `Promise.allSettled` 全部处理完成后，我们可以拿到每个 `Promise` 的状态，而不管其是否处理成功

```
const resolved = Promise.resolve(2);
const rejected = Promise.reject(-1);
const allSettledPromise = Promise.allSettled([resolved, rejected]);
allSettledPromise.then(function (results) {
  console.log(results);
});
// 返回结果：
// [
//    { status: 'fulfilled', value: 2 },
//    { status: 'rejected', reason: -1 }
// ]
```

从上面代码中可以看到，Promise.allSettled 最后返回的是一个数组，记录传进来的参数中每个 Promise 的返回值，这就是和 all 方法不太一样的地方。

•`any` 方法

•语法： `Promise.any（iterable）`

•参数： `iterable` 可迭代的对象，例如 `Array`。

•描述： `any` 方法返回一个 `Promise`，只要参数 `Promise` 实例有一个变成 `fulfilled`状态，最后 `any`返回的实例就会变成 `fulfilled` 状态；如果所有参数 `Promise` 实例都变成 `rejected` 状态，包装实例就会变成 `rejected` 状态。

```
const resolved = Promise.resolve(2);
const rejected = Promise.reject(-1);
const anyPromise = Promise.any([resolved, rejected]);
anyPromise.then(function (results) {
  console.log(results);
});
// 返回结果：
// 2
```

> 从改造后的代码中可以看出，只要其中一个 Promise 变成 fulfilled状态，那么 any 最后就返回这个p romise。由于上面 resolved 这个 Promise 已经是 resolve 的了，故最后返回结果为 2

race 方法

•语法： Promise.race（iterable）参数： iterable 可迭代的对象，例如 Array。描述： race方法返回一个 Promise，只要参数的 Promise 之中有一个实例率先改变状态，则 race 方法的返回状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给 race 方法的回调函数

•我们来看一下这个业务场景，对于图片的加载，特别适合用 race 方法来解决，将图片请求和超时判断放到一起，用 race 来实现图片的超时判断。请看代码片段。

```
//请求某个图片资源
function requestImg(){
  var p = new Promise(function(resolve, reject){
    var img = new Image();
    img.onload = function(){ resolve(img); }
    img.src = 'http://www.baidu.com/img/flexible/logo/pc/result.png';
  });
  return p;
}
//延时函数，用于给请求计时
function timeout(){
  var p = new Promise(function(resolve, reject){
    setTimeout(function(){ reject('图片请求超时'); }, 5000);
  });
  return p;
}
Promise.race([requestImg(), timeout()])
.then(function(results){
  console.log(results);
})
.catch(function(reason){
  console.log(reason);
});


// 从上面的代码中可以看出，采用 Promise 的方式来判断图片是否加载成功，也是针对 Promise.race 方法的一个比较好的业务场景
```

![img](https://cdn.jsdelivr.net/gh/djwasd/cdn@v0.2/images/20210414210720.png)

```
promise手写实现
function myPromise(constructor){
    let self=this;
    self.status="pending" //定义状态改变前的初始状态
    self.value=undefined;//定义状态为resolved的时候的状态
    self.reason=undefined;//定义状态为rejected的时候的状态
    function resolve(value){
        //两个==="pending"，保证了状态的改变是不可逆的
       if(self.status==="pending"){
          self.value=value;
          self.status="resolved";
       }
    }
    function reject(reason){
        //两个==="pending"，保证了状态的改变是不可逆的
       if(self.status==="pending"){
          self.reason=reason;
          self.status="rejected";
       }
    }
    //捕获构造异常
    try{
       constructor(resolve,reject);
    }catch(e){
       reject(e);
    }
}
// 定义链式调用的then方法
myPromise.prototype.then=function(onFullfilled,onRejected){
   let self=this;
   switch(self.status){
      case "resolved":
        onFullfilled(self.value);
        break;
      case "rejected":
        onRejected(self.reason);
        break;
      default:   
   }
}
```

# **Generator**

> Generator 是 ES6中新增的语法，和 Promise 一样，都可以用来异步编程。Generator函数可以说是Iterator接口的具体实现方式。Generator 最大的特点就是可以控制函数的执行。

•`function*` 用来声明一个函数是生成器函数，它比普通的函数声明多了一个 `*`,`*`的位置比较随意可以挨着 `function` 关键字，也可以挨着函数名

•`yield` 产出的意思，这个关键字只能出现在生成器函数体内，但是生成器中也可以没有 `yield` 关键字，函数遇到 `yield` 的时候会暂停，并把 `yield` 后面的表达式结果抛出去

•`next`作用是将代码的控制权交还给生成器函数

```
function *foo(x) {
  let y = 2 * (yield (x + 1))
  let z = yield (y / 3)
  return (x + y + z)
}
let it = foo(5)
console.log(it.next())   // => {value: 6, done: false}
console.log(it.next(12)) // => {value: 8, done: false}
console.log(it.next(13)) // => {value: 42, done: true}
```

上面这个示例就是一个Generator函数，我们来分析其执行过程：

•首先 Generator 函数调用时它会返回一个迭代器

•当执行第一次 next 时，传参会被忽略，并且函数暂停在 yield (x + 1) 处，所以返回 5 + 1 = 6

•当执行第二次 next 时，传入的参数等于上一个 yield 的返回值，如果你不传参，yield 永远返回 undefined。此时 let y = 2 * 12，所以第二个 yield 等于 2 * 12 / 3 = 8

•当执行第三次 next 时，传入的参数会传递给 z，所以 z = 13, x = 5, y = 24，相加等于 42

`yield`实际就是暂缓执行的标示，每执行一次 `next()`，相当于指针移动到下一个 `yield`位置

![Generator函数其执行过程](https://cdn.jsdelivr.net/gh/djwasd/cdn@v0.2/images/20210408100806.png)

> 总结一下，Generator函数是ES6提供的一种异步编程解决方案。通过yield标识位和next()方法调用，实现函数的分段执行

**遍历器对象生成函数，最大的特点是可以交出函数的执行权**

•`function` 关键字与函数名之间有一个星号；

•函数体内部使用 `yield`表达式，定义不同的内部状态；

•`next`指针移向下一个状态

> 这里你可以说说 Generator的异步编程，以及它的语法糖 async 和 awiat，传统的异步编程。ES6 之前，异步编程大致如下

•回调函数

•事件监听

•发布/订阅

> 传统异步编程方案之一：协程，多个线程互相协作，完成异步任务。

```
// 使用 * 表示这是一个 Generator 函数
// 内部可以通过 yield 暂停代码
// 通过调用 next 恢复执行
function* test() {
  let a = 1 + 2;
  yield 2;
  yield 3;
}
let b = test();
console.log(b.next()); // >  { value: 2, done: false }
console.log(b.next()); // >  { value: 3, done: false }
console.log(b.next()); // >  { value: undefined, done: true }
```

从以上代码可以发现，加上 *的函数执行后拥有了 next 函数，也就是说函数执行后返回了一个对象。每次调用 next 函数可以继续执行被暂停的代码。以下是 Generator 函数的简单实现

```
// cb 也就是编译过的 test 函数
function generator(cb) {
  return (function() {
    var object = {
      next: 0,
      stop: function() {}
    };

    return {
      next: function() {
        var ret = cb(object);
        if (ret === undefined) return { value: undefined, done: true };
        return {
          value: ret,
          done: false
        };
      }
    };
  })();
}
// 如果你使用 babel 编译后可以发现 test 函数变成了这样
function test() {
  var a;
  return generator(function(_context) {
    while (1) {
      switch ((_context.prev = _context.next)) {
        // 可以发现通过 yield 将代码分割成几块
        // 每次执行 next 函数就执行一块代码
        // 并且表明下次需要执行哪块代码
        case 0:
          a = 1 + 2;
          _context.next = 4;
          return 2;
        case 4:
          _context.next = 6;
          return 3;
		// 执行完毕
        case 6:
        case "end":
          return _context.stop();
      }
    }
  });
}
 
```

# **async/await**

> Generator 函数的语法糖。有更好的语义、更好的适用性、返回值是 Promise。

•await 和 promise 一样，更多的是考笔试题，当然偶尔也会问到和 promise 的一些区别。

•await 相比直接使用 Promise 来说，优势在于处理 then 的调用链，能够更清晰准确的写出代码。缺点在于滥用 await 可能会导致性能问题，因为 await 会阻塞代码，也许之后的异步代码并不依赖于前者，但仍然需要等待前者完成，导致代码失去了并发性，此时更应该使用 Promise.all。

•一个函数如果加上 async ，那么该函数就会返回一个 Promise

> async => *await => yield

```
// 基本用法

async function timeout (ms) {
  await new Promise((resolve) => {
    setTimeout(resolve, ms)  
  })
}
async function asyncConsole (value, ms) {
  await timeout(ms)
  console.log(value)
}
asyncConsole('hello async and await', 1000)
 
var a = 0
var b = async () => {
  a = a + await 10
  console.log('2', a) // -> '2' 10
  a = (await 10) + a
  console.log('3', a) // -> '3' 20
}
b()
a++
console.log('1', a) // -> '1' 1
```

•首先函数 `b` 先执行，在执行到 `await 10` 之前变量 `a` 还是 `0`，因为在 `await` 内部实现了 `generators` ，`generators` 会保留堆栈中东西，所以这时候 `a = 0` 被保存了下来

•因为 `await` 是异步操作，遇到 `await`就会立即返回一个 `pending`状态的 `Promise`对象，暂时返回执行代码的控制权，使得函数外的代码得以继续执行，所以会先执行 `console.log('1', a)`

•这时候同步代码执行完毕，开始执行异步代码，将保存下来的值拿出来使用，这时候 `a = 10`

•然后后面就是常规执行代码了

**优缺点：**

> async/await的优势在于处理 then 的调用链，能够更清晰准确的写出代码，并且也能优雅地解决回调地狱问题。当然也存在一些缺点，因为 await 将异步代码改造成了同步代码，如果多个异步代码没有依赖性却使用了 await 会导致性能上的降低。

**async原理**

> async/await语法糖就是使用Generator函数+自动执行器来运作的

```
// 定义了一个promise，用来模拟异步请求，作用是传入参数++
function getNum(num){
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(num+1)
        }, 1000)
    })
}

//自动执行器，如果一个Generator函数没有执行完，则递归调用
function asyncFun(func){
  var gen = func();

  function next(data){
    var result = gen.next(data);
    if (result.done) return result.value;
    result.value.then(function(data){
      next(data);
    });
  }

  next();
}

// 所需要执行的Generator函数，内部的数据在执行完成一步的promise之后，再调用下一步
var func = function* (){
  var f1 = yield getNum(1);
  var f2 = yield getNum(f1);
  console.log(f2) ;
};
asyncFun(func);
```

•在执行的过程中，判断一个函数的 `promise`是否完成，如果已经完成，将结果传入下一个函数，继续重复此步骤

•每一个 `next()` 方法返回值的 `value` 属性为一个 `Promise` 对象，所以我们为其添加 `then` 方法， 在 `then` 方法里面接着运行 `next` 方法挪移遍历器指针，直到 `Generator`函数运行完成 ![img](https://cdn.jsdelivr.net/gh/djwasd/cdn@v0.2/images/20210414210917.png)

# **事件循环**

![事件循环](https://cdn.jsdelivr.net/gh/djwasd/cdn@v0.2/images/20210516161332.png)

•默认代码从上到下执行，执行环境通过 `script`来执行（宏任务）

•在代码执行过程中，调用定时器 `promise` `click`事件...不会立即执行，需要等待当前代码全部执行完毕

•给异步方法划分队列，分别存放到微任务（立即存放）和宏任务（时间到了或事情发生了才存放）到队列中

•`script`执行完毕后，会清空所有的微任务

•微任务执行完毕后，会渲染页面（不是每次都调用）

•再去宏任务队列中看有没有到达时间的，拿出来其中一个执行

•执行完毕后，按照上述步骤不停的循环

**例子**

![img](https://cdn.jsdelivr.net/gh/djwasd/cdn@v0.2/images/20210516162034.png)

![img](https://cdn.jsdelivr.net/gh/djwasd/cdn@v0.2/images/20210516162330.png)自动执行的情况 会输出 listener1 listener2 task1 task2

![img](https://cdn.jsdelivr.net/gh/djwasd/cdn@v0.2/images/20210516162704.png)

![img](https://cdn.jsdelivr.net/gh/djwasd/cdn@v0.2/images/20210516163038.png)

如果手动点击click 会一个宏任务取出来一个个执行，先执行click的宏任务，取出微任务去执行。会输出 listener1 task1 listener2 task2

```
console.log(1)

async function asyncFunc(){
  console.log(2)
  // await xx ==> promise.resolve(()=>{console.log(3)}).then()
  // console.log(3) 放到promise.resolve或立即执行
  await console.log(3) 
  // 相当于把console.log(4)放到了then promise.resolve(()=>{console.log(3)}).then(()=>{
  //   console.log(4)
  // })
  // 微任务谁先注册谁先执行
  console.log(4)
}

setTimeout(()=>{console.log(5)})

const promise = new Promise((resolve,reject)=>{
  console.log(6)
  resolve(7)
})

promise.then(d=>{console.log(d)})

asyncFunc()

console.log(8)

// 输出 1 6 2 3 8 7 4 5
```

**1. 浏览器事件循环**

> 涉及面试题：异步代码执行顺序？解释一下什么是 Event Loop ？

> JavaScript的单线程，与它的用途有关。作为浏览器脚本语言，JavaScript的主要用途是与用户互动，以及操作DOM。这决定了它只能是单线程，否则会带来很复杂的同步问题。比如，假定JavaScript同时有两个线程，一个线程在某个DOM节点上添加内容，另一个线程删除了这个节点，这时浏览器应该以哪个线程为准？所以，为了避免复杂性，从一诞生，JavaScript就是单线程，这已经成了这门语言的核心特征，将来也不会改变

> js代码执行过程中会有很多任务，这些任务总的分成两类：

•同步任务

•异步任务

> 当我们打开网站时，网页的渲染过程就是一大堆同步任务，比如页面骨架和页面元素的渲染。而像加载图片音乐之类占用资源大耗时久的任务，就是异步任务。，我们用导图来说明：

![img](https://cdn.jsdelivr.net/gh/djwasd/cdn@v0.2/images/10.png)

**我们解释一下这张图：**

•同步和异步任务分别进入不同的执行"场所"，同步的进入主线程，异步的进入Event Table并注册函数。

•当指定的事情完成时，Event Table会将这个函数移入Event Queue。

•主线程内的任务执行完毕为空，会去Event Queue读取对应的函数，进入主线程执行。

•上述过程会不断重复，也就是常说的Event Loop(事件循环)。

> 那主线程执行栈何时为空呢？js引擎存在monitoring process进程，会持续不断的检查主线程执行栈是否为空，一旦为空，就会去Event Queue那里检查是否有等待被调用的函数

以上就是js运行的整体流程

**面试中该如何回答呢？ 下面是我个人推荐的回答：**

•首先js 是单线程运行的，在代码执行的时候，通过将不同函数的执行上下文压入执行栈中来保证代码的有序执行

•在执行同步代码的时候，如果遇到了异步事件，js 引擎并不会一直等待其返回结果，而是会将这个事件挂起，继续执行执行栈中的其他任务

•当同步事件执行完毕后，再将异步事件对应的回调加入到与当前执行栈中不同的另一个任务队列中等待执行

•任务队列可以分为宏任务对列和微任务对列，当当前执行栈中的事件执行完毕后，js 引擎首先会判断微任务对列中是否有任务可以执行，如果有就将微任务队首的事件压入栈中执行

•当微任务对列中的任务都执行完成后再去判断宏任务对列中的任务。

```
setTimeout(function() {
  console.log(1)
}, 0);
new Promise(function(resolve, reject) {
  console.log(2);
  resolve()
}).then(function() {
  console.log(3)
});
process.nextTick(function () {
  console.log(4)
})
console.log(5)
```

•第一轮：主线程开始执行，遇到 `setTimeout`，将setTimeout的回调函数丢到宏任务队列中，在往下执行 `new Promise`立即执行，输出2，then的回调函数丢到微任务队列中，再继续执行，遇到 `process.nextTick`，同样将回调函数扔到微任务队列，再继续执行，输出5，当所有同步任务执行完成后看有没有可以执行的微任务，发现有then函数和 `nextTick`两个微任务，先执行哪个呢？`process.nextTick`指定的异步任务总是发生在所有异步任务之前，因此先执行process.nextTick输出4然后执行then函数输出3，第一轮执行结束。

•第二轮：从宏任务队列开始，发现setTimeout回调，输出1执行完毕，因此结果是25431

> JS 在执行的过程中会产生执行环境，这些执行环境会被顺序的加入到执行栈中。如果遇到异步的代码，会被挂起并加入到 Task（有多种 task） 队列中。一旦执行栈为空，Event Loop 就会从 Task 队列中拿出需要执行的代码并放入执行栈中执行，所以本质上来说 JS 中的异步还是同步行为

```
console.log('script start');

setTimeout(function() {
  console.log('setTimeout');
}, 0);

console.log('script end');
```

不同的任务源会被分配到不同的 Task 队列中，任务源可以分为 微任务（microtask） 和 宏任务（macrotask）。在 ES6 规范中，microtask 称为 jobs，macrotask 称为 task

```
console.log('script start');

setTimeout(function() {
  console.log('setTimeout');
}, 0);

new Promise((resolve) => {
    console.log('Promise')
    resolve()
}).then(function() {
  console.log('promise1');
}).then(function() {
  console.log('promise2');
});

console.log('script end');
// script start => Promise => script end => promise1 => promise2 => setTimeout
 
```

以上代码虽然 setTimeout 写在 Promise 之前，但是因为 Promise 属于微任务而 setTimeout 属于宏任务

**微任务**

•`process.nextTick`

•`promise`

•`Object.observe`

•`MutationObserve`

![微任务](https://cdn.jsdelivr.net/gh/djwasd/cdn@v0.2/images/20210516165328.png)

**宏任务**

•`script`

•`setTimeout`

•`setInterval`

•`setImmediate`

•`I/O` 网络请求完成、文件读写完成事件

•`UI rendering`

•用户交互事件（比如鼠标点击、滚动页面、放大缩小等）

> 宏任务中包括了 script ，浏览器会先执行一个宏任务，接下来有异步代码的话就先执行微任务

![宏任务](https://cdn.jsdelivr.net/gh/djwasd/cdn@v0.2/images/20210414213126.png)

**所以正确的一次 Event loop 顺序是这样的**

•执行同步代码，这属于宏任务

•执行栈为空，查询是否有微任务需要执行

•执行所有微任务

•必要的话渲染 UI

•然后开始下一轮 `Event loop`，执行宏任务中的异步代码

> 通过上述的 Event loop 顺序可知，如果宏任务中的异步代码有大量的计算并且需要操作 DOM 的话，为了更快的响应界面响应，我们可以把操作 DOM 放入微任务中

•JavaScript 引擎首先从宏任务队列（macrotask queue）中取出第一个任务

•执行完毕后，再将微任务（microtask queue）中的所有任务取出，按照顺序分别全部执行（这里包括不仅指开始执行时队列里的微任务），如果在这一步过程中产生新的微任务，也需要执行；

•然后再从宏任务队列中取下一个，执行完毕后，再次将 microtask queue 中的全部取出，循环往复，直到两个 queue 中的任务都取完。

![img](https://cdn.jsdelivr.net/gh/djwasd/cdn@v0.2/images/20210414211816.png)

总结起来就是：一次 Eventloop 循环会处理一个宏任务和所有这次循环中产生的微任务。

# **Proxy代理**

> proxy在目标对象的外层搭建了一层拦截，外界对目标对象的某些操作，必须通过这层拦截

```
var proxy = new Proxy(target, handler);
```

new Proxy()表示生成一个Proxy实例，target参数表示所要拦截的目标对象，handler参数也是一个对象，用来定制拦截行为

```
var target = {
   name: 'poetries'
 };
 var logHandler = {
   get: function(target, key) {
     console.log(`${key} 被读取`);
     return target[key];
   },
   set: function(target, key, value) {
     console.log(`${key} 被设置为 ${value}`);
     target[key] = value;
   }
 }
 var targetWithLog = new Proxy(target, logHandler);
 
 targetWithLog.name; // 控制台输出：name 被读取
 targetWithLog.name = 'others'; // 控制台输出：name 被设置为 others
 
 console.log(target.name); // 控制台输出: others
```

•`targetWithLog` 读取属性的值时，实际上执行的是 `logHandler.get` ：在控制台输出信息，并且读取被代理对象 `target` 的属性。

•在 `targetWithLog` 设置属性值时，实际上执行的是 `logHandler.set` ：在控制台输出信息，并且设置被代理对象 `target` 的属性的值

```
// 由于拦截函数总是返回35，所以访问任何属性都得到35
var proxy = new Proxy({}, {
  get: function(target, property) {
    return 35;
  }
});

proxy.time // 35
proxy.name // 35
proxy.title // 35
```

**Proxy 实例也可以作为其他对象的原型对象**

```
var proxy = new Proxy({}, {
  get: function(target, property) {
    return 35;
  }
});

let obj = Object.create(proxy);
obj.time // 35
```

> proxy对象是obj对象的原型，obj对象本身并没有time属性，所以根据原型链，会在proxy对象上读取该属性，导致被拦截

**Proxy的作用**

> 对于代理模式 Proxy 的作用主要体现在三个方面

•拦截和监视外部对对象的访问

•降低函数或类的复杂度

•在复杂操作前对操作进行校验或对所需资源进行管理

**Proxy所能代理的范围--handler**

> 实际上 handler 本身就是ES6所新设计的一个对象.它的作用就是用来 自定义代理对象的各种可代理操作 。它本身一共有13中方法,每种方法都可以代理一种操作.其13种方法如下

```
// 在读取代理对象的原型时触发该操作，比如在执行 Object.getPrototypeOf(proxy) 时。
handler.getPrototypeOf()

// 在设置代理对象的原型时触发该操作，比如在执行 Object.setPrototypeOf(proxy, null) 时。
handler.setPrototypeOf()

 
// 在判断一个代理对象是否是可扩展时触发该操作，比如在执行 Object.isExtensible(proxy) 时。
handler.isExtensible()

 
// 在让一个代理对象不可扩展时触发该操作，比如在执行 Object.preventExtensions(proxy) 时。
handler.preventExtensions()

// 在获取代理对象某个属性的属性描述时触发该操作，比如在执行 Object.getOwnPropertyDescriptor(proxy, "foo") 时。
handler.getOwnPropertyDescriptor()

 
// 在定义代理对象某个属性时的属性描述时触发该操作，比如在执行 Object.defineProperty(proxy, "foo", {}) 时。
andler.defineProperty()

 
// 在判断代理对象是否拥有某个属性时触发该操作，比如在执行 "foo" in proxy 时。
handler.has()

// 在读取代理对象的某个属性时触发该操作，比如在执行 proxy.foo 时。
handler.get()

 
// 在给代理对象的某个属性赋值时触发该操作，比如在执行 proxy.foo = 1 时。
handler.set()

// 在删除代理对象的某个属性时触发该操作，比如在执行 delete proxy.foo 时。
handler.deleteProperty()

// 在获取代理对象的所有属性键时触发该操作，比如在执行 Object.getOwnPropertyNames(proxy) 时。
handler.ownKeys()

// 在调用一个目标对象为函数的代理对象时触发该操作，比如在执行 proxy() 时。
handler.apply()

 
// 在给一个目标对象为构造函数的代理对象构造实例时触发该操作，比如在执行new proxy() 时。
handler.construct()
```

**为何Proxy不能被Polyfill**

•如class可以用 `function`模拟；`promise`可以用 `callback`模拟

•但是proxy不能用 `Object.defineProperty`模拟

> 目前谷歌的polyfill只能实现部分的功能，如get、set https://github.com/GoogleChrome/proxy-polyfill

```
// commonJS require
const proxyPolyfill = require('proxy-polyfill/src/proxy')();

// Your environment may also support transparent rewriting of commonJS to ES6:
import ProxyPolyfillBuilder from 'proxy-polyfill/src/proxy';
const proxyPolyfill = ProxyPolyfillBuilder();

// Then use...
const myProxy = new proxyPolyfill(...);
```