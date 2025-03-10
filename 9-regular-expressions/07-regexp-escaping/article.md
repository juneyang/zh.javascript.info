# 转义，特殊字符

正如我们所看到的，反斜杠 `pattern:\` 用于表示字符类，例如 `pattern:\d`。所以它是正则表达式中的一个特殊字符（就像在常规字符串中一样）。

还存在其它特殊字符，这些字符在正则表达式中有特殊的含义，例如 `pattern:[ ] { } ( ) \ ^ $ . | ? * +`。它们用于执行更强大的搜索。

现在并不需要尝试记住它们 —— 当我们分别学习它们的时候，你自然而然就会记住了。

## 转义

假如我们想要找到一个点号 `.`。

要将特殊字符用作常规字符，请在其前面加上反斜杠：`pattern:\.`。

这就是所谓的“转义字符”。

例如：

```js
alert( "Chapter 5.1".match(/\d\.\d/) ); // 5.1（匹配了！）
alert( "Chapter 511".match(/\d\.\d/) ); // null（寻找一个真正的点 \.）
```

括号也是特殊字符，所以如果我们想要查找它们，我们应该使用 `pattern:\(`。下面的例子会查找一个字符串 `"g()"`：

```js
alert( "function g()".match(/g\(\)/) ); // "g()"
```

如果我们想查找反斜杠 `\`，我们就应该使用两个反斜杠：

```js
alert( "1\\2".match(/\\/) ); // '\'
```

## 一个斜杠

斜杠符号 `'/'` 并不是一个特殊字符，但是它被用于在 Javascript 中开启和关闭正则匹配：`pattern:/...pattern.../`，所以我们也应该转义它。

下面是搜索斜杠 `'/'` 的表达式：

```js
alert( "/".match(/\//) ); // '/'
```

从另一个方面看，如果我们没使用 `pattern:/.../`，而是使用另一种 `new RegExp` 的方式创建正则表达式，则不需要转义斜杠：

```js
alert( "/".match(new RegExp("/")) ); // 找到了 /
```

## new RegExp

如果我们使用 `new RegExp` 创建正则表达式，那么我们不必转义 `/`，但需要进行一些其他转义。

例如，考虑下面这个示例：

```js
let reg = new RegExp("\d\.\d");

alert( "Chapter 5.1".match(reg) ); // null
```

在之前的示例中我们使用 `pattern:/\d\.\d/` 进行类似的搜索没问题，但 `new RegExp("\d\.\d")` 不起作用，为什么？

因为反斜杠被字符串“消耗”了。我们可能还记得，常规字符串有自己的特殊字符，例如 ，反斜杠用于转义。

下面是 "\d.\d" 的感知形式：

```js
alert("\d\.\d"); // d.d
```

在字符串中的反斜杠表示转义或者类似  这种只能在字符串中使用的特殊字符。这个引用会“消耗”并且解释这些字符，比如说：

* &#x20;—— 变成一个换行字符，
* `\u1234` —— 变成该编码所对应的 Unicode 字符，
* ……而当没有特殊含义时：如 `pattern:\d` 或者 `pattern:\z`，碰到这种情况时则会自动移除反斜杠。

所以调用 `new RegExp` 会获得一个没有反斜杠的字符串。这就是搜索不起作用的原因！

如果要修复这个问题，我们需要双斜杠，因为引用会把 `\\` 变为 `\`：

```js
*!*
let regStr = "\\d\\.\\d";
*/!*
alert(regStr); // \d\.\d（现在对了）

let regexp = new RegExp(regStr);

alert( "Chapter 5.1".match(regexp) ); // 5.1
```

## Summary

* 要在字面意义上搜索特殊字符 `pattern:[ \ ^ $ . | ? * + ( )`，我们需要在它们前面加上一个反斜杠 `\`（“转义它们”）。
* 如果在 `pattern:/.../` 内（但不在 `new RegExp` 内），我们还需要转义 `/`。
* 当将字符串传递给给 `new RegExp` 时，我们需要双反斜杠 `\\`，因为字符串引号会消耗一个反斜杠。
