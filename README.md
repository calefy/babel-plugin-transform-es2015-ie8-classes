# Transform ES2015 Classes [Adapt IE8]

ES2015 classes在转换中，如果没有对象方法，在转换时，会生成一个同名的命名函数表达式，在内部引用时，IE8会报找不到变量的错误。

该插件重写了部分转换代码，在只有一个构造函数时，将命名函数表达式的名称替换为一个新的名字。

## Babel 原生转换类定义

With `babel-plugin-transform-es2015-classes`:

```js
class Abc {
    constructor() {
        this.name = 'bar';
    }
}
```

Transform result:

```js
function _classCallCheck(instance, Constructor) { if (!(instance instanceof Constructor)) { throw new TypeError("Cannot call a class as a function"); } }

let Abc = function Abc() {
    _classCallCheck(this, Abc);

    this.name = 'Hello';
};
```

## Adapt IE8

IE8 中因为对命名函数表达式的实现不标准，函数内部不能访问命名函数的名称，因此该插件修改命名函数名，函数体中的`_classCallCheck` 使用函数表达式赋值后的对象。

Transform result with this plugin:

```js
function _classCallCheck(instance, Constructor) { if (!(instance instanceof Constructor)) { throw new TypeError("Cannot call a class as a function"); } }

let Abc = function _Abc() {
    _classCallCheck(this, Abc);

    this.name = 'Hello';
};
```


## 参考

- [命名函数表达式问题](http://www.jb51.net/onlineread/named-function-expressions-demystified/#jscript-bugs)
- [Babel插件手册](https://github.com/thejameskyle/babel-handbook/blob/master/translations/zh-Hans/plugin-handbook.md)
