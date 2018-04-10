[window.location.href和window.location.replace的区别](https://blog.csdn.net/github_37483541/article/details/59481084)
--------------------------

在处理一个具有返回功能的按钮时花了一些时间，就是返回到用户搜索的前一页，用的是 window.location.go(-1)，这就是正正经经的浏览器返回

到前一条浏览记录的做法。可是有几个返回按钮却一直失灵，点击之后，可以看到浏览器刷新，但是还是停留在原来的页面。 

仔细查看代码，终于发现了原因。就是用户从上一个页面 a 跳转到目前看到的这一个页面 c，其实中间还有一个页面 b，只是用户看不到，因为从 a 

跳转到 b 后，执行 b 的脚本，b 的脚本有下面这句：

```js

window.location.href = "c.html";

```

所以页面嗖的一下就跳到了 c 页面，但是浏览器记录却记录了三个页面：a, b, c. 

这个时候点击返回按钮从 c 就是返回到 b，而 b 又会马上跳转到 c，所以就出现了点击返回按钮，页面刷新后，还是停留在原来页面的情况。 

解决方案就是把上面重定位的脚本换成下面这句：

```js


window.location.replace("c.html");

```

* replace 解释

```  
 window.location.replace(url) replaces the current location in the address bar by a new one. The page that was calling 
 
 the function,won’t be included in the browser history. Therefore, on the new location, clicking the back button in your 
 
 browser would make you go back to the page you were viewing before you visited the document containing the redirecting 
 
 JavaScript. 
  
```

`window.location.replace(url)`将目前浏览器的地址替换掉，调用这个方法的网页，将不会被写入浏览记录。所以，网页 b 不会被写入浏览记录，
从网页 a 经过 b 跳转到 c，再点击返回按钮将跳转到 a.
