[vue+Springboot跨域请求](https://www.cnblogs.com/zircon/p/9091225.html)

vue端需要实现如下：
```js
main.js中添加如下片段：

import axios from 'axios'
Vue.prototype.$axios = axios

Vue.config.productionTip = false
axios.defaults.withCredentials = false//这个默认即为false，如果改为true，可以传递session信息，后端要做相应修改来放行，本例中我们用false简单测试

对应.vue文件方法中添加：

methods: {    
    listall(){
        this.$axios.get('http://localhost:9999/user/listAll')
            .then(function (response) {
              console.log(response.data)
            })
            .catch(function (error) {
              console.log(error)
            })
    }
  },
  mounted(){
    this.login();
  }

启动中或者调用处调用该方法
```

Springboot端控制类添加注解实现：
```java
@CrossOrigin//跨域请求注解,加在控制类或者控制类的方法上均可

```
