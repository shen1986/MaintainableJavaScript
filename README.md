<!--
 * @Description: Readme
 * @Author: shenxf
 * @Date: 2019-03-19 11:07:42
 -->
# MaintainableJavaScript

边看书边做笔记，这个仓库主要是参照《编写可维护的JavaScript》这本书，来总结自己的学习笔记。
只是自己的总结，和书本不一样，如果觉得不错，你可以购买这本书自己研究。

## 编程风格

1. 基本的格式化

    - [缩进层级](#缩进层级)
    - [语句结尾](#语句结尾)
    - [行的长度](#行的长度)
    - [换行](#换行)

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
我们应该规定我们一行的长度不要超过80个字符。超过80个字符应该折行。这是因为很多编辑是
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


## 补足

[图片识别](https://github.com/gsdgdf/Java_OCR)

[ocr windows安装](https://github.com/UB-Mannheim/tesseract/wiki)

也可以直接把pdf转成png文件，然后用ocr抓取文字。

抓取英文 `tesseract xxx.png output_1 -l eng`

抓取中文 `tesseract xxx.png output_1 -l chi_sim`