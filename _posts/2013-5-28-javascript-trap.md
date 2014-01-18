---

title: JavaScript 陷阱

layout: post

---
鉴于 JavaScript(JS) 实在太多小毛病，现将其整理到这个帖子，便于日后翻看纠错。

##**注释**

JS 使用 // 进行单行注释，使用 /* */ 进行多行注释。

##**变量**

JS 支持一条语句声明多个变量：

{% gist 8487914 js0.js %}

**建议所有变量都使用 var 进行声明**，避免如下错误：

{% gist 8487914 js1.js %}

**在函数内加var关键字进行声明的变量为局部变量，除此之外所有的变量声明都是全局变量的**。

##**数据类型**

JS 共有Undefined、Null、Boolean、Number、String、Array、Object对象七种数据类型，动态类型支持相同变量的不同类型赋值：

{% gist 8487914 js2.js %}

####**Undefined**

Undefined 类型只有一个特殊值 undefined，通过 typeof 判断变量类型，若得到的结果是 Undefined 类型，则变量可能已声明未赋值，也可能变量不存在，因此建议对每个变量进行显式初始化。

####**Null**

Null 类型也只有一个特殊值 null，表示空对象，因此使用 typeof 判断会返回 object 类型。**实际上， undefined 派生自 null，因此 null == undefined**。

**尽管声明变量时无需显示赋值为 undefined，对于对象变量却建议赋值为 null，有助于区分 null 和 undefined**。

####**Boolean**

Boolean 类型有两个字面值 true 和 false，其他数据类型的值都可以转换成 Boolean 类型，**其中，除了空字符串（String）、0/NaN（Number）、null（Object）、undefined（Undefined）转换为 false，其他值都为 true**。

####**Number**

**1，表示**

Number 使用 IEEE754 标准表示整数和浮点数，除了基本的十进制，也支持八进制（0开头）和十六进制（0x开头）表示，**若字面值超出范围则忽略前导作十进制计**。

{% gist 8487914 js3.js %}

**2，NaN**

NaN 是一个特殊的数值，为了不抛出错误 JS 增加了这个值，**首先，任何涉及NaN的操作 (例如 NaN/10) 都会返回 NaN ，这个特点在多步计算中有可能导致问题。其次，NaN 与任何值都不相等，包括 NaN 本身**。isNaN() 函数接受任意类型的参数以确定这个参数是否“不是数值”。

**3，数值转换**

Number()、parseInt() 和 parseFloat() 三个函数将其他类型的值转换成数值。

**不建议使用 Number() 函数，转换整数使用 parseInt()，转换浮点数使用 parseFloat()，其中，为了防止对于不同进制数的转换错误，parseInt() 函数还可以指定基数作为第二个参数，而 parseFloat() 只解析十进制值**。

{% gist 8487914 js4.js %}

####**String**

String 类型可以用单引号或者双引号表示，**字符串是不可变的**，因此：

{% gist 8487914 js5.js %}

这个操作实际上还生成并销毁了一个 "Script" 字符串。

字符串转换可以使用转型函数 String()，也可以使用每个类型的 toString() 方法。

####**Object**

JS 拥有诡异的面向对象特性，JS 中的所有事物都是对象：字符串、数值、数组、函数... 同时提供多个内建对象，比如 String、Date、Array 等等，对象只是带有属性和方法的特殊数据类型，同时，JS 允许自定义对象。

{% gist 8487914 js6.js %}

##**操作符**

**1，递增递减操作符**

借鉴自 C，同时支持字符型、布尔值、浮点数和对象。

{% gist 8487914 js7.js %}

**2，逻辑操作符**

{% gist 8487914 js8.js %}

可以通过这样赋值以避免 null 或 undefined 值。

**3，关系操作符**

**字符串比较的是两个字符串中对应位置的每个字符的字符编码值**，如：

{% gist 8487914 js9.js %}

**4，相等操作符**

**相等（==）和不相等（!=）会先转换再比较，全等（===）和不全等（!==）仅比较不转换**。

{% gist 8487914 js10.js %}

##**函数**

**1，参数**

**JS 中所有参数传递的都是值，不可能通过引用传递参数，同时，参数的个数并不做限制，也不影响运行，或者说，JS 的函数是没有参数的**。

{% gist 8487914 js11.js %}

**另外， arguments 对象可以与命名参数一起使用**，如：

{% gist 8487914 js12.js %}

上例中，由于 num1 的值与 arguments[0] 的值相同，因此它们可以互换使用。同时，由于没有传递值的命名参数将自动被赋予 undefined 值，因此，如果只给 doAdd() 函数传递了一个参数，则 num2 中就会保存 undefined 值。

**2，没有重载**

JS 的参数特性注定了它无法实现像 Java 语言那样的重载，如果在 JS 中定义了两个名字相同的函数，则该名字只属于后定义的函数。

...