## Html-css-js...
#### Update 框架里面套框架解决
 
```html
 <script type="text/javascript">
 ```

>  method -- 1  

```html
if(top.location != self.location){
		top.location = self.location;  	//防止页面被框架包含
}
```

>  method -- 2  

 
```html
if( self.location != top.location){
	    top.location =self.location;      	//防止页面被框架包含
   }
```

>   以下代码死循环重定向（错误代码

 ```html
 if( self.location != top.location){
	    self.location =top.location;    	 //防止页面被框架包含
   }
</script>
```
