<!--
 * @Description: Readme
 * @Author: shenxf
 * @Date: 2019-03-19 11:07:42
 -->
# MaintainableJavaScript

边看书边做笔记，这个仓库主要是参照《编写可维护的JavaScript》这本书，来总结自己的学习笔记。
只是自己的总结，和书本不一样，如果觉得不错，你可以购买这本书自己研究。书本还涉及到JSLint，JSHint
中的区别。实例代码从哪里取材，各种组织或公司的代码风格，并进行比较以及总结。对于某些写法的观点有时有些模棱两可，我自己的总结没有这些东西，更偏向与自己怎么写。

最后，我写这篇东西也不容易，如果感觉不错，请点`Star`,谢谢大家。

## 编程风格

1. 基本的格式化

    - [缩进层级](#缩进层级)
    - [语句结尾](#语句结尾)
    - [行的长度](#行的长度)
    - [换行](#换行)
    - [空行](#空行)
    - [命名](#命名)
        + [变量和函数](#变量和函数)
        + [常量](#常量)
        + [构造函数](#构造函数)
    - [直接量](#直接量)
        + [字符串](#字符串)
        + [数字](#数字)
        + [null](#null)
        + [undefined](#undefined)
        + [对象直接量](#对象直接量)
        + [数组直接量](#数组直接量)

2. 注释

    - [单行注释](#单行注释)
    - [多行注释](#多行注释)
    - [使用注释](#使用注释)
        + [难以理解的代码](#难以理解的代码)
        + [可能被误认为错误的代码](#可能被误认为错误的代码)
        + [游览器特性hack](#游览器特性hack)
    - [文档注释](#文档注释)

3. 语句和表达式

    - [花括号的对齐方式](#花括号的对齐方式)
    - [块语句的间隔](#块语句的间隔)
    - [switch语句](#switch语句)
        + [缩进](#缩进)
        + [case语句的“连续执行”](#case语句的连续执行)
        + [default](#default)
    - [with语句](#with语句)
    - [for循环](#for循环)
    - [for-in循环](#for-in循环)

4. 变量、函数和运算符

    - [变量声明](#变量声明)
    - [函数声明](#函数声明)
    - [函数调用间隔](#函数调用间隔)
    - [立即调用的函数](#立即调用的函数)
    - [严格模式](#严格模式)

### 缩进层级

- 缩进层级非常重要，弄的不好，容易造成很多误解。
下这段代码 (为了演示,.这里故意修改了示例代码的缩进〕。

```javascript
if (wl && wl.length) {
            for (i = 0, 1 = wl.length; i < 1; ++i) {
        p = wl[i];
        type = Y.Lang.type(r[p]);
        if (s.hasOwnProperty(p)) { if (merge && type == 'object') {

    Y.mix(r(p], s{p]);
} else if (ov || !(p in r)) {
                    r[p] = s[p];
            }
        }
    }
}    
```

- 快速读憧这段代码井不容易。 这里的缩进并不统一,一眼看去 else 是对应到第 1
行的if语句。 但实际上这个 else 和代码第 5行的if语句相对应。 

```javascript
if (wl && wl.length) {
    for (i = 0, 1 = wl.length; i < 1; ++i) {
        p = wl[i];
        type = Y.Lang.type(r[p]);

        if (s.hasOwnProperty(p)) {
            if (merge && type == 'object') {
                Y.mix(r[p], s[p]);
            } else if (ov || !(p in r)) {
                r[p] = s[p];
            }
        }
    }
}
```

- 有两种主张
    + 使用制表符进行缩进
        * 就是[tab]进行缩进
    + 使用空格进行缩进
        * 一般是以4个空格进行缩进

- 以上两种方法都可以，但是不要两种方法混用。

[返回顶部](#编程风格)

### 语句结尾

- 在javascript里面可以不以分号结尾。分析器会自动在结尾加上分号。
- 但是不推荐这种做法。

```javascript
// 原始代码
function getData(){
    return
    {
        title: "Maintainable JavaScript",
        author: "Nicholas C. Zakas"
    }
}

// 分析器会将它理解成
function getData(){
    return;
    {
        title: "Maintainable JavaScript",
        author: "Nicholas C. Zakas"
    };
}

```

- 以上面的例子来讲，函数将返回一个undefined，这不是我们要的结果。
虽然可以将 `{` 放到 `return` 之后，但是分析器的判断比较复杂，不能保证我们
能考虑到所有的情况。
- 所有我们应该更加倾向于使用它们，而不是省略它们。

- 上面是书的作者的看法，我确实看到一种风格是结尾一律不加分号，这种写法要求在语句是以`[`,`(`,`开头的时候，在前面加上分号。
- 而且推荐这种做法的人认为，不加分号反而不容易出错。像类似于上面的错误是不会被忽略到上线阶段的，当你创建了一个方法，没有得到你想要的结果，应该在开发的时候就立即被发现了。
- 就算使用了加了分号的规范，有时也会因为不小心而漏掉（用检测工具的没有这个问题）
- 最后，使用的第三方代码不一定是加分号的风格。有可能还是不得不在第一个位置加上分号。
```javascript
// 这些语句与上一句没有分号的语句连接的话会直接报错，所以必须加分号

;[1,2,3].foreach(function(){
    // 处理内容
})

;`shenxf${nickname}`.length

// 很多代码都习惯于在圆括号之前加一个分号，这是由于之前引入的代码可能有漏了分号的情况，
// 为了以防万一，都会有类似于下面的代码。
;(function(){
    // 处理内容。
})()
```

- 我比较喜欢不省略分号，如果想采用省略分号的规范，切记在`[`,`(`,`之前加上分号。

[返回顶部](#编程风格)

### 行的长度

- 如果一行的内容太长，编辑窗口就会出现滚动条。这样不利于我们查看代码，也比较变扭。
我们应该规定我们一行的长度不要超过80个字符。超过80个字符应该折行。这是因为很多编辑器是
在80个字符以后出现滚动条

[返回顶部](#编程风格)

### 换行

- 当一行的长度太长我们就会进行折行。通常要在运算符后面换行，这是因为分析器不会再运算符后面自动加分号。
不容易出错。

```javascript
// 好的做法：在运算符后面换行。 第二行追加二个缩进
callAFunction(document, element, window, "some string value", true, 123,
        navigator);

// 不好的做法第二行只有一个缩进
callAFunction(document, element, window, "some string value", true, 123,
    navigator);

// 不好的做法运算符之前换行了
callAFunction(document, element, window, "some string value", true, 123
    , navigator);
```

- 为什么要二个缩进

```javascript
    if (isLeapYear && isFebruary && day && day ==29 && itsYourBirthdady &&
            noPlans) {
        
        waitAnotherFourYears();
    }
```

- 从上面的代码可以看出，二个缩进正好和条件内部语句的一个缩进错开，这样更容易阅读。
- 但是有一个例外。在赋值语句后面的第二行位置最好要对齐

```javascript
    var result = something + anotherThing + yetAnotherThing + someThingElse +
                 anotherSomethingElse;
```

[返回顶部](#编程风格)

### 空行

- 空行常常被忽略，代码看起来应该像一段一段可读的代码，而不是全部糅合在一块。拿缩进层级的例子来说

```javascript
if (wl && wl.length) {
    for (i = 0, 1 = wl.length; i < 1; ++i) {
        p = wl[i];
        type = Y.Lang.type(r[p]);
        if (s.hasOwnProperty(p)) {
            if (merge && type == 'object') {
                Y.mix(r[p], s[p]);
            } else if (ov || !(p in r)) {
                r[p] = s[p];
            }
        }
    }
}
```

- 给这段代码加上空行

```javascript
if (wl && wl.length) {

    for (i = 0, 1 = wl.length; i < 1; ++i) {
        p = wl[i];
        type = Y.Lang.type(r[p]);

        if (s.hasOwnProperty(p)) {

            if (merge && type == 'object') {
                Y.mix(r[p], s[p]);
            } else if (ov || !(p in r)) {
                r[p] = s[p];
            }
        }
    }
}
```

- 这样看上去能够更加流畅的阅读，一般来讲下面的场景添加空行是不错的注意
    + 在方法之间。
    + 在方法中的局部变量和第一条语句之间。
    + 在多行或则单行注释之前。
    + 在方法的逻辑片段之间插入空行，提高可读性。

[返回顶部](#编程风格)

### 命名

- JavaScript语言核心是ECMAScript，遵照了驼峰式大小写命名法（这个太有名了我就不解释了）
- 一般是遵循语言核心所采用的命名规范，因此大部分JavaScript程序员使用驼峰命名法给变量和函数命名。

[返回顶部](#编程风格)

#### 变量和函数

- 变量的命名前缀应该是名词，这能后和函数的命名规则区分开，函数应该以动词来做前缀。

```javascript
// 好的写法
var count = 10;
var myName = "shenxf";
var found = true;

// 不好的写法: 变量看起来像函数
var getCount = 10;
var isFound = true;

// 好的写法
function getName() {
    return myName;
}

// 不好的写法
function theName() {
    return myName;
}

```

- 命名应该要尽量短小精干，例如，count，length和size一看就知道是数字类型，name，title，和message一看就知道是字符串类型
- i，j，k通常在循环处理中使用。
- 要经量避免写无意义的命名。

- 对于方法的命名，第一个单词应该是动词。下面是一些常见的约定

动词 | 含义 
-|-
can | 函数返回一个布尔值
has | 函数返回一个布尔值
is | 函数返回一个布尔值
get | 函数返回一个非布尔值
set | 函数用来保存一个值

- 按照上面的写法，可读性会很好

```javascript

if (isEnabled()) {
    setName("shenxf");
}

if (getName() === "shenxf") {
    doSomething();
}
```

[返回顶部](#编程风格)

#### 常量

- 在ECMAScript6之前，JavaScript没有真正常量的概念。为了区分普通的变量和常量，
用类似于C语言方法来命名，用大写字母和下划线来命名，下划线是用来分隔单词的。

```javascript
var MAX_COUNT = 10;
var URL = "http://shenxf.top";
```

- 来看一下代码

```javascript
if (count < MAX_COUNT) {
    doSomething();
}
```

[返回顶部](#编程风格)

#### 构造函数

- 构造函数遵照大驼峰命名法。主要是为了和函数与变量的命名法进行区分。

```javascript
// 好的做法
function Person(name) {
    this.name = name;
}

Person.prototype.sayName = function() {
    alert(this.name);
}

var me = new Person("shenxf");
```

- 这样写可以快速的发现问题，看一下下面的代码

```javascript
var me = Person("shenxf"); // 这里缺少了new
var you = getPerson("xx");
```

[返回顶部](#编程风格)

### 直接量

- JavaScript里面的原始值包括：字符串、数字、布尔值、null和undefined。也包含对象和数组的直接量。
- 其中只有布尔值是自解释的，其他的多少要考虑下如何能准确地表达其中的含义。

[返回顶部](#编程风格)

#### 字符串

- 这里原书的作者提出可以用双引号和单引号。

```javascript
// 下面都是合法的javascript代码
var name = "shenxf says, \"Hi.\"";
var name = 'shenxf says, "Hi."';
```

- 书的作者经常使用Java，Java是用双引号来表示字符串。所以作者推荐使用双引号，因为可以和Java语言保持一致。
- 我个人推荐使用单引号，本人之前使用的很多编码规范插件都是以单引号来声明变量的。我已经习惯了，所以我推荐使用单引号。不管使用哪一种，自己的代码风格保持一致我觉得就可以了。

- 当字符串换行的时候，应该使用运算符来连接。

```javascript
// 不好的写法
var longString = "Here's the story, of a man \
named Brady.";

// 好的写法
var longString = "Here's the story, of a man" +
                 "named Brady.";
```

[返回顶部](#编程风格)

#### 数字

- 直接给列子

```javascript
// 整数
var count = 10;

// 小数
var price = 10.0;
var price = 10.00;

// 不推荐小数的写法：没有小数部分
var pricee = 10.;

// 不推荐小数的写法：没有整数部分
var price = .1;

// 不推荐的写法：八进制写法已经被弃用
var num = 010;

// 十六进制写法
var num = 0xA2;

// 科学计数法
var num = 1e23;
```

- 不推荐写法的前两个感觉特别变扭，不知道是刻意写的还是漏掉的。为了避免歧义，所以不推荐。
- 八进制的写法也很容易产生歧义，很有可能被认为是整数`10`，其实是`8`,所以不推荐。

[返回顶部](#编程风格)

#### null

- null比较特殊经常和undefined混淆。

- 应该使用null的场景
    + 用来初始化一个变量，这个变量可能被赋值为一个对象
    + 用来和一个已经初始化的变量比较，这个变量可以是也可以不是一个对象
    + 当函数的参数期望是对象时，用作参数传入
    + 当函数的返回值期望是对象时，用作返回值传出
- 不应该使用null的场景
    + 不要使用null来检测是否传入了某个参数
    + 不要用null来检测一个未初始化的变量

- 例子

```javascript

// 好的用法
var person = null;

// 好的用法
function getPerson() {
    if (condition) {
        return new Person("shenxf");
    } else {
        return null;
    }
}

// 好的用法
var person = getPerson();
if (person !== null) {
    doSomething();
}

// 不好的写法：用来和未初始化的变量进行比较
var person;
if (person != null) {
    doSomething();
}

// 不好的写法：检测是否传入了参数
function doSomething(arg1, arg2, arg3, arg4) {
    if (arg4 != null) {
        doSomethingElse();
    }
}
```

- 理解`null`最好的方法是把它理解成一个占位符

[返回顶部](#编程风格)

#### undefined

- 最让人困惑的是 `null == undefined` 的结果是`true`,然而，这2个值的用法各不相同。
- 那些没有被初始化的变量都有一个初始值，即`undefined`,表示这个变量等待被赋值。

```javascript
// 不好的写法
var person;
console.log(person === undefined); //true
```

- 虽然这是正常的代码，但是我们应该经量避免`undefined`.
- 因为`undefined`有很多让人费解的地方，比如`typeof`

```javascript
// foo未被声明
var person;
console.log(typeof person); // "undefined"
console.log(typeof foo);    // "undefined"
```

- 这段代码看似没什么，但是在某些场景下面有天壤之别（在语句中foo会报错，而person则不会）
- 通过禁止使用特殊值`undefined`,可以使得`typeof`出来的结果只有一种可能出现`undefined`。那就是
变量未声明的时候。

```javascript
// 好的做法
var person = null;
console.log(person === null); // true
```

- `typeof null`值返回的是`Object`,这样可以避开`undefined`,这样就区分开了。

[返回顶部](#编程风格)

#### 对象直接量

- 有直接量的东西，最好不要先创建然后再赋值。

```javascript
// 不好的写法
var book = new Object();
book.title = "shenxfsbook";
book.author = "shenxf";
```

- 直接量可以高效的完成非直接量相同的任务。
- 当定义直接量的时候，第一行包含左花括号，每一个属性的名值对都独占一行，并保持一个缩进，最后花括号也独占一行

```javascript
// 好的写法
var book = {
    title: "shenxfsbook",
    author: "shenxf"
};
```

[返回顶部](#编程风格)

#### 数组直接量

- 和对象的直接量类似，不建议使用非直接量。

```javascript
// 不好的写法
var colors = new Array("red", "green", "blue");
var numbers = new Array(1, 2, 3, 4);

// 好的写法
var colors = ["red", "green", "blue"];
var numbers = [1, 2, 3, 4];
```

[返回顶部](#编程风格)

### 单行注释

```javascript
// 这是一句单行注释
```

- 双斜杠后面最好预留一个空格
- 单行注释 有3中使用方法
    + 独占一行注释，用来解释下一行代码，这行注释之前总是有一个空行，且缩进层级和下一行代码保持一致
    + 在代码行的尾部的注释。代码结束到注释之间至少有一个缩进。注释（包括之前的代码部分）不应当超过单行的最大字符数限制，如果超过了，就应该将这行注释放在代码行上方。
    + 被注释掉的大段代码（编译器自带的批量注释多行代码）

- 单行注释不应该用于多行，除非是注释的大段代码。

```javascript
// 好的写法
if (condition) {

    // 如果代码执行到这里，则表示通过了左右的安全性检查
    allowed();
}

// 不好的写法：注释之前没有空行
if (condition) {
    // 如果代码执行到这里，则表示通过了左右的安全性检查
    allowed();
}

// 不好的写法：错误的缩进
if (condition) {
// 如果代码执行到这里，则表示通过了左右的安全性检查
    allowed();
}

// 好的写法
var result = something + somethingElse; // somethingElse不应当取值为null

// 不好的写法：代码和注释之间没有间隔
var result = something + somethingElse;// somethingElse不应当取值为null

// 好的写法
// if (condition) {
//     dosomething();
//     thenDoSomethingElse();
// }

// 不好的写法:这里应当用多行注释
// 接下来的这段代码非常难，那么，让我详细的解释下
// 这段代码首先判断条件是否为真
// 。。。。。。。
if (condition) {
    // 如果代码执行到这里，则表示通过了左右的安全性检查
    allowed();
}
```

[返回顶部](#编程风格)

### 多行注释

- 合法的多行注释

```javascript
/* 我的注释 */
/* 另一段注释
这个注释包含2行 */
/*
又是一段注释
这段注释同样包含2行
*/
```

- 这些注释都是合法但是看得不清晰，推荐使用类似于java的注释方法
- 第一是`/*`,最后一行是`*/`，当中每一行都以`*`开头，且空开一个空格

```javascript
/*
 * 另一段注释
 * 这个注释包含2行
 */
```

- 多行注释应该和单行注释一样注释之前要有空行，注释用来说明下面行的代码，并与下一行保持同样的缩进

```javascript
// 好的写法
if (condition) {

    /*
     * 如果代码执行到这里
     * 说明通过了所有安全性检查
     */
    allowed();
}

// 不好的写法：注释之前无空行
if (condition) {
    /*
     * 如果代码执行到这里
     * 说明通过了所有安全性检查
     */
    allowed();
}

// 不好的写法：星号之后没有空格
if (condition) {
    /*
     *如果代码执行到这里
     *说明通过了所有安全性检查
     */
    allowed();
}

// 不好的写法：错误的缩进
if (condition) {
/*
 * 如果代码执行到这里
 * 说明通过了所有安全性检查
 */
    allowed();
}

// 不好的写法：代码尾部不要用多行注释
var result = something + somethingElse; /*somethingElse不应当取值为null*/
```

[返回顶部](#编程风格)

### 使用注释

- 代码不够清晰时应该添加注释，代码很明了的时候不应该添加注释，有点画蛇添足。

```javascript
// 不好的写法：注释并没有提供有价值的信息

// 初始化count
var count = 10;

// 好的写法

// 改变这个值可能会使它变成青蛙
var count = 10;
```

- 因此，添加注释的原则是需要让代码变的更清晰的时候。

[返回顶部](#编程风格)

#### 难以理解的代码

- 难以理解的代码应该添加注释

```javascript
// 好的写法

if (mode) {

    /*
     * 当 mode 为2时
     * 用来执行原型合并的操作。。。。
     * 。。。。
     */
    if (mode === 2) {
        Y.mix(receiver.prototype, supplier.prototype, overwrite,
                whitelis, 0, merge);
    }

     /*
      * 根据模式的类型。。。
      * 。。。。
      */
    from = mode === 1 || mode === 3 ? supplier.prototype : supperlier;
    to   = mode === 1 || mode === 4 ? receiver.prototype : receiver;

    /*
     * .......
     * ......
     */
    if (!from || !to) {
        from = supperlier;
        to   = receiver;
    }
} else {
    from = supplier;
    to = receiver;
}
```

[返回顶部](#编程风格)

#### 可能被误认为错误的代码

- 团队里面会有一些好心人，自主的把一个看上去错误代码改正，但是这段错误的往往并不是错误的源头
- 修改了这段代码会导致另外的bug

```javascript
while (element && (element = element[asix])) { // 赋值操作
    if( (all || element[TAG_NAME]) &&
        (!fn || fn(element)) ) {
        
        return element;
    }
}
```

- 在这里例子里面作者写明了他是赋值操作，虽然这不是一种标准用法，检测工具可能会检测出问题
- 很容易被误解成一个错误。这种注释就表明作者是有意为之。从而避免了不必要的误解。

[返回顶部](#编程风格)

#### 游览器特性hack

- 为了要兼容低版本的游览器，JavaScript程序员往往会写一些“可能被误认为错误的代码“。

```javascript
var ret = false;

if ( !needle || !element || !needle[NODE_TYPE] || !element[NODE_TYPE]) {
    ret = false;
} else if (element[CONTAINS]) {
    // 如果needle不是ELEMENT_NODE时，IE和Safari下会有错误
    if (Y.UA.opera || needle[NODE_TYPE] === 1) {
        ret = element[CONTAINS](needle);
    } else {
        ret = Y_DOM._bruteContains(element, needle);
    }
} else if (element[COMPARE_DOCUMENT_POSITION]) { // gecko
    if (element === needle || !!(element[COMPARE_DOCUMENT_POSITION](needle) & 16)) {
        ret = true
    }
}
```

- 第六行有一段重要的注释，尽管IE和Safari中都有内置的方法contains(),但是needle不是一个元素是，这个方法会报错
- 所以只有当浏览器是Opera的时候才能用这个方法。这里说明了为什么要一个if语句。这样不会被别人误改动，而且以后浏览器要做相应的兼容性改动是也能快速的定位到这段代码，对自己的维护也是有好处的。

[返回顶部](#编程风格)

### 文档注释

- 虽然不是JavaScript的组成部分但是现在广泛的被运用，一般文档注释有多种格式，最流行的是源于JavaDoc文档格式：
    + 多行注释以单斜线双星号`/**`开始，接下来是描述信息，其中`@`符号来表示一个或多个属性。

```javascript
/**
返回一个对象，这个对象包含被提供对象的所有属性。
后一个对象的属性会覆盖前一个对象的属性。
传入一个单独对象。。。。
@method merge
@param {Object} 被合并的一个或多个对象
@param {Object} 一个新的合并后的对象
**/
Y.merge = function() {
    var args    = arguments,
        i       = 0,
        len     = args.length,
        result  = {};
    
    for (; i<len; ++i) {
        Y.mix(result, args[i], true);
    }

    return result;
};
```

- 你应当确保对如下的内容添加注释
    + 所有的方法
        * 应当对方法，期望的参数和可能的返回值添加注释描述。
    + 所有的构造函数
        * 应当对自定义类型和期望的参数添加注释描述。
    + 所有包含文档化方法的对象
        * 如果一个对象包含一个或多个附带文档注释的方法，那么这个对象也应当适当地针对文档生成工具添加文档注释。
        * 当然，注释的详细格式和用法最终还是由你所选择的文档生成工具决定的。

[返回顶部](#编程风格)

### 花括号的对齐方式

- 有2中风格
    + 第一个风格是，将左括号放置在第一句代码的末尾
        * 这种风格来自java
    ```javascript
    if (condition) {
        doSomething();
    } else {
        doSomethingElse();
    }
    ```

    + 第二种风格是将左括号放置于块语句首行的下一行。
    ```javascript
    if (condition)
    {
        doSomething();
    }
    else
    {
        doSomethingElse();
    }
    ```
    + 这种风格是C#的风格，这种写法会导致，自动插入分号位置错误，所以JavaScript里不推荐用这种风格。

[返回顶部](#编程风格)

### 块语句的间隔

- 有三种主要的风格
    + 第一种风格，在语句名，圆括号和左花括号之间没有空格间隔。
        * 这种风格可读性不好。
    ```javascript
    if(condition){
        doSomething();
    }
    ```

    + 第二种风格,在左圆括号之前和右圆括号之后各添加一个空格
        * 这种风格最流行。
    ```javascript
    if (condition) {
        doSomething();
    }
    ```

    + 第三种风格，在左圆括号后和右圆括号前各添加一个空格
        * 这种写法可读性最高。
    ```javascript
    if ( condition ) {
        doSomething();
    }
    ```

    + 本人和书的作者一样比较推荐第二种风格。

[返回顶部](#编程风格)

### switch语句

- JavaScript中的`switch`语句的行为和在其他语言中是不一样的：switch语句中可以使用任意类型值，任何表达式都可合法地用于case从句。但在其他语言中则必须使用原始值和常量。

[返回顶部](#编程风格)

#### 缩进

- 很多人用java的风格
```javascript
    switch(condition) {
        case "first":
            // 代码
            break;

        case "second":
            // 代码
            break;

        case "third":
            // 代码
            break;
        
        default:
            // 代码
    }
```

- 独特之处
    + 每条`case`语句相对于`switch`关键字都缩进一个层级。
    + 从第二条豫剧开始，每条`case`语句前后各有一个空行。

- 另一种风格
```javascript
    switch(condition) {
    case "first":
        // 代码
        break;
    case "second":
        // 代码
        break;
    case "third":
        // 代码
        break;
    default:
        // 代码
    }
```
- 不同之处是`case`关键字与`switch`保持左对齐，

- 我和作者都喜欢java的风格。

[返回顶部](#编程风格)

#### case语句的“连续执行”

- `case`后面不加`break`，就会连续执行下面的条件，这个成为很多系统bug的原罪。
- 但是还是有许多人接受这种连续执行的编程方法。但是逻辑一定要写的清晰。
```javascript
    switch(condition) {

        // 明显的依次执行
        case "first":
        case "second":
            // 代码
            break;

        case "third":
            // 代码
            
            /* fall through */
        default:
            // 代码
    }
```
- 前面2行代码是个很明显的连续执行，这是合理的。
- `third`里面由于添加了注释，说明这是有意为之。这也是合理的。

[返回顶部](#编程风格)

#### default

- 比较有争论的议题是，是否需要`default`，很多人不论何时都不省略`default`，尽管它什么也不做。
```javascript
    switch(condition) {
        case "first":
            // 代码
            break;

        case "second":
            // 代码
            break;

        default:
            // default中没有逻辑
    }
```

- 作者比较倾向于，没有默认行为，并且写了注释的情况下可以省略
```javascript
    switch(condition) {
        case "first":
            // 代码
            break;

        case "second":
            // 代码
            break;

        // 没有 default
    }
```
- 这样即表明了没有默认行为，又省下了字节。

- 我比较喜欢第一种，你可以只写`default:`不写注释。这样也清楚的知道没有默认行为，以后要加默认行为的时候可以少写一行代码。其实2种方法都可以，就看你喜欢哪一种。

[返回顶部](#编程风格)

### with语句

- 在没看这本书之前，我根本不知道这个东西，如果你和我一样，请跳过这一章（不知道也就不会用），这不是什么好东西。

```javascript
var book = {
    title: "shenxf javascrpt",
    author: "shenxf"
};

var message = " The book is ";

with (book) {
    message += title;
    message += " by " + author;
}
```

- 虽然写法上它可以让我们省略一些字节，但是这东西有坏处，我们很难分辨`title`和`author`出现在哪个位置，也难分辨出`message`到底是局部变量还是`book`的一个属性。从程序上来讲，JavaScript引擎和压缩工具无法对这段代码进行优化，应为它们无法猜出代码的真正含义。

[返回顶部](#编程风格)

### for循环

- for循环有两种，一种是传统的for循环，另外一种是for-in循环
```javascript
    var values = [ 1, 2, 3, 4, 5, 6, 7],
        i, len;
    for (i=0, len=values.length; i < len; i++) {
        process(values[i]);
    }
```
- for循环中可以使用`break`和`continue`语句，相信大家肯定知道怎么用。
- 有一种风格中说不要用`continue`，用`continue`还不如用条件语句，逻辑更清晰。
```javascript
    var values = [ 1, 2, 3, 4, 5, 6, 7],
        i, len;
    for (i=0, len=values.length; i < len; i++) {
        if (i !== 2) {
            process(values[i]);
        }
    }
```
- 书的作者没有明确表态，我认为这个不至于，想用还是可以用的。这个语句本身是没有问题的。

[返回顶部](#编程风格)

### for-in循环

- 相信大家都会用。for-in循环不仅仅遍历对象的实例属性，从原型链继承来的属性也会遍历。当遍历自定义对象的属性时往往会因为意外的结果而终止。因此，最好使用`hasOwnProperty()`方法来为for-in循环过滤出实例属性。

```javascript
var prop;

for (prop in object) {
    if (object.hasOwnProperty(prop)) {
        console.log("Property name is " + prop);
        console.log("Property value is " + object[prop]);
    }
}
```

- 一般都会加上`hasOwnProperty`，但是如果你就是想查找原型链，请加上注释。

```javascript
var prop;

for (prop in object) { // 包含对原型链的遍历
    console.log("Property name is " + prop);
    console.log("Property value is " + object[prop]);
}
```

- for-in循环是用来遍历对象的，请不要用它来遍历数组。

```javascript
// 不好的用法
var values = [ 1, 2, 3, 4, 5 ],
    i;

for (i in values) {
    process(item[i]);
}
```

[返回顶部](#编程风格)


### 变量声明

- 声明会被提升到当前作用域顶部。

```javascript
function doSomething() {
    
    var result = 10 + value;
    var value = 10;
    return result;
}
```
- 上面代码的理解
```javascript
function doSomething() {
    var result;
    var value

    result = 10 + value;
    value = 10;

    return result;
}
```
- 这个函数得出的结果是NaN值。 

- 作者倾向的风格,把所有定义放在作用域开始部分，用逗号分隔，可以节省写`var`的字节。
```javascript
    function doSomethingWithItems(items) {

        var value = 10,
            result = value + 10,
            i,
            len;
        
        for (i=0, len=items.length; i < len; i++) {
            doSomething(items[i]);
        }
    }
```

[返回顶部](#编程风格)

### 函数声明

- 函数声明也会被javaScript引擎提前。
```javascript
    // 不好的写法
    doSomething();

    function doSomething() {
        alert('Hello world!');
    }
```
- javaScript会把代码解释为
```javascript
    function doSomething() {
        alert('Hello world!');
    }

    doSomething();
```
- 我们应该先声明函数再使用
```javascript
    function doSomethingWithItems(items) {

        var i,len,
            value = 10,
            result = value + 10;
        
        function doSomething(item) {
            // 代码逻辑
        }

        for (i=0, len=items.length; i < len; i++) {
            doSomething(items[i]);
        }
    }
```
- 另外，函数不应该出现在语句块之内。
```javascript

    // 不好的写法
    if (condition) {
        function doSomething() {
            alert('hi');
        }
    } else {
        function doSomething() {
            alert('Yo');
        }
    }
```
- 因为函数的声明会被提前，所以不管这里的条件满足与否，第二个同名的函数会把第一个函数覆盖。所以上面的代码不会按照我们的意图来执行。

[返回顶部](#编程风格)

### 函数调用间隔

```javascript
    // 好的写法
    doSomething(item);

    // 不好的写法：看起来像一个语句块
    doSomething (item);

    // 用来做对比的块语句
    while (item) {
        // 代码逻辑
    }
```

[返回顶部](#编程风格)

### 立即调用的函数

```javaScript
    // 一般的写法
    var doSomething = function() {
        // 函数体
    };

    // 不好的写法 : 被误解为函数的赋值，其实是被赋值为一个对象
    var value = function() {

        // 函数体

        return {
            message: "Hi"
        }
    }();

    // 好的写法
    var value = (function() {

        // 函数体
        
        return {
            message: "Hi"
        }
    }());
```

[返回顶部](#编程风格)

### 严格模式
- `use strict`

```javascript
    // 不好的写法 - 全局严格模式
    "use strict"
    function doSomething() {
        // 代码
    }

    // 好的写法
    function doSomething() {
        "use strict"
        // 代码
    }
```

- 如果希望在多个函数中应用严格模式而不必谢很多行的话，可以使用立即执行函数

```javascript
    // 好的写法
    (function() {
        "use strict"
        function doSomething() {
            // 代码
        }
        function doSomethingElse() {
            // 代码
        }
    })();
```

- 为什么不要全局使用严格模式？
    + 全局使用严格模式的情况下，其他文件中的（非严格模式下的）代码很可能会报错。


[返回顶部](#编程风格)

## 补足

[图片识别](https://github.com/gsdgdf/Java_OCR)

[ocr windows安装](https://github.com/UB-Mannheim/tesseract/wiki)

也可以直接把pdf转成png文件，然后用ocr抓取文字。

抓取英文 `tesseract xxx.png output_1 -l eng`

抓取中文 `tesseract xxx.png output_1 -l chi_sim`