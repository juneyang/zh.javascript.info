# solution

这里是解析。

1. 它是一个常规的方法调用。
2. 同样，括号没有改变执行的顺序，点符号总是先执行。
3.  这里我们有一个更复杂的 `(expression)()` 调用。这个调用就像被分成了两行（代码）一样：

    ```js
    f = obj.go; // 计算函数表达式
    f();        // 调用
    ```

&#x20;  这里的 `f()` 是作为一个没有（设定）`this` 的函数执行的。

4. 与 `(3)` 相类似，在括号 `()` 的左边也有一个表达式。

要解释 `(3)` 和 `(4)` 得到这种结果的原因，我们需要回顾一下属性访问器（点符号或方括号）返回的是引用类型的值。

除了方法调用之外的任何操作（如赋值 `=` 或 `||`），都会把它转换为一个不包含允许设置 `this` 信息的普通值。
