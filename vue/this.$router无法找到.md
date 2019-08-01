vue项目axios调用操作中使用
```js
if (code == "1") {//签到成功 router到成功界面
           // vm.$message.success(response.data.msg);
            this.$router.push("/success");
          }
```
结果怎么都无法路由，控制台报找不到该方法，后查询原因是axios需要绑定this,即添加 `.bind(this)`
```js
  this.$axios
        .post("http://10.168.2.145:9999/user/sign", {
            phone: values.phone
        })
        .then(function(response) {
         // console.log(response);
          const result = response.data;
          console.log(response.data);
          const code = response.data.statu;
          if (code == "1") {//签到成功 router到成功界面
           // vm.$message.success(response.data.msg);
            this.$router.push("/success");
          } else {
            vm.$message.error(response.data.msg);
          }
        }.bind(this) //特别注意，这里增加了.bind(this) )
        .catch(function(error) {
          console.log(error);
        });

```
