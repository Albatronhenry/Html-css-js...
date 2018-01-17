*   FreeMarker是一款模板引擎： 即一种基于模板和要改变的数据， 并用来生成输出文本（HTML网页、电子邮件、配置文件、源代码等）的通用工具。 
它不是面向最终用户
的，而是一个Java类库，是一款程序员可以嵌入他们所开发产品的组件。

*   FreeMarker是免费的，基于Apache许可证2.0版本发布。其模板编写为FreeMarker Template Language（FTL），属于简单、专用的语言。
需要准备数据在真实编程语
言中来显示，比如数据库查询和业务运算， 之后模板显示已经准备好的数据。


工作原理

--------

假设在一个应用系统中需要一个HTML页面如下：

```html

<html>
    <head>
        <title>Welcome!</title>
    </head>
    <body>
        <h1>Welcome Big Joe!</h1>
        <p>Our latest product:
        <a href="products/greenmouse.html">green mouse</a>!
    </body>
</html>

```

页面中的用户名（即上面的“Big Joe”）是登录这个网页的访问者的名字， 并且最新产品的数据应该来自于数据库才能随时更新。所以，不能直接在HTML页面中输入
“Big Joe”、“greenmouse”及链接， 不能使用静态HTML代码。可以使用要求输出的模板来解决，模板和静态页面是相同的，只是它会包含一些FreeMarker将它们
变成动态内容的指令：

```html

<html>
    <head>
        <title>Welcome!</title>
    </head>
    <body>
        <h1>Welcome ${user}!</h1>
        <p>Our latest product:
        <a href="${latestProduct.url}">${latestProduct.name}</a>!
    </body>
</html>

```

模板文件存放在Web服务器上，当有人来访问这个页面，FreeMarker就会介入执行，然后动态转换模板，用最新的数据内容替换模板中${...}的部分，之后将结果
发送到访问者的Web浏览器中。访问者的Web浏览器就会接收到例如第一个HTML示例那样的内容（也就是没有FreeMarker指令的HTML代码），访问者也不会察觉到
服务器端使用的FreeMarker。（存储在Web服务器端的模板文件是不会被修改的；替换也仅仅出现在Web服务器的响应中。）

为模板准备的数据整体被称作为数据模型。数据模型是树形结构（就像硬盘上的文件夹和文件)，在视觉效果上， 数据模型可以是（这只是一个形象化显示，数据模
型不是文本格式，它来自于Java对象）：

```html
	
(root)
  |
  +- user = "Big Joe"
  |
  +- latestProduct
      |
      +- url = "products/greenmouse.html"
      |
      +- name = "green mouse"
      

```     

早期版本中，可以从数据模型中选取这些值，使用user和latestProduct.name表达式即可。类比于硬盘的树形结构，数据模型就像一个文件系统，“(root)”
和latestProduct就对应着目录（文件夹），而user、url和name就是这些目录中的文件。总体上，模板和数据模型是FreeMarker来生成输出所必须的组成部
分：模板 + 数据模型 = 输出


基本语法

----------

    ${...}：FreeMarker将会输出真实的值来替换大括号内的表达式，这样的表达式被称为interpolation（插值）。
    注释：注释和HTML的注释也很相似，但是它们使用<#-- and -->来标识。不像HTML注释那样，FTL注释不会出现在输出中（不出现在访问者的页面中），
    因为FreeMarker会跳过它们。
    FTL标签（FreeMarker模板的语言标签）：FTL标签和HTML标签有一些相似之处，但是它们是FreeMarker的指令，是不会在输出中打印的。这些标签的名字
    以#开头。（用户自定义的FTL标签则需要使用@来代替#）
    
    
    
    
    在所有采用网页静态化手段的网站中，FreeMarker使用的比例大大的超过了其他的一些技术。HTML静态化也是某些缓存策略使用的手段，对于系统中频繁使用
数据库查询但是内容更新很小的应用，可以使用FreeMarker将HTML静态化。比如一些网站的公用设置信息，这些信息基本都是可以通过后台来管理并存储在数据库中，
这些信息其实会大量的被前台程序调用，每一次调用都会去查询一次数据库，但是这些信息的更新频率又会很小，因此也可以考虑将这部分内容进行后台更新的时候进行
静态化，这样就避免了大量的数据库访问请求，从而也就提高了网站的性能

    与JSP相比，FreeMarker的一个优点在于不能轻易突破模板语言开始编写Java代码，因此降低了领域逻辑漏进视图层的危险几率。但缺点是需要一点附加配置来将
其平稳地集成到应用程序中，一些IDE（集成开发环境）可能并不完全支持它，当然还有开发者或设计者也许需要学习一门陌生的模板语言。相关的JAR文件将要添加到
WEB-INF/lib（在需要的时候，它们包含在Spring中）
