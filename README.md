<!--
 * @Description: Readme
 * @Author: shenxf
 * @Date: 2019-03-19 11:07:42
 -->
# MaintainableJavaScript

边看书边做笔记，这个仓库主要是参照《编写可维护的JavaScript》这本书，来总结自己的学习笔记。
只是自己的总结，和书本不一样，如果觉得不错，你可以购买这本书自己研究。
我写这篇东西也不容易，如果感觉不错，请点`Star`,谢谢大家。

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

- JavaScript语言核心是EXMAScript，遵照了驼峰式大小写命名法（这个太有名了我就不解释了）
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
    + 用来好一个已经初始化的变量比较，这个变量可以使也可以不是一个对象
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

## 补足

[图片识别](https://github.com/gsdgdf/Java_OCR)

[ocr windows安装](https://github.com/UB-Mannheim/tesseract/wiki)

也可以直接把pdf转成png文件，然后用ocr抓取文字。

抓取英文 `tesseract xxx.png output_1 -l eng`

抓取中文 `tesseract xxx.png output_1 -l chi_sim`