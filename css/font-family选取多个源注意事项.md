# font-family选取多个源注意事项
### 基本使用
font-family基本参数就不累述了，可以参见[MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-family)。下面阐述font-family选取多个字体的策略。

### 多个源的选取
> font-family 属性指定的是一个优先级从高到低的可选字体列表。 字体的选定不是在发现用户计算机上安装的列表中的第一个字体时停止。相反，对字体的选择是逐字进行的。也就是说即使某个字符周围都在某个字体中可以显示，但该字符在当前的字体文件中没有适合的图形，那么会继续尝试列表中靠后的字体。不过这在Internet Explorer 6以及之前的版本中不适用。

所以，和我们想象的不同，它并不是在客户端找到一个可用的源之后，就忽略剩余的了。这样就弥补了`@font-face`只能选取一个源的不足了。