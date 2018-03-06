ajax请求到的样例json:

```json

{
	"lvl_id": "000001",
	"sys_name": "预算编审",
	"remark": "101",
	"data": [{
		"lvl_id": "000001",
		"remark": "101",
		"sys_name": "预算编审",
		"sup_name": "中期规划查询表"
	}],
	"errorCode": "0",
	"sup_name": "中期规划查询表"
}

```

如果页面要显示lvl_id对应的值,可以 

datas.lvl_id(datas中存在即可用)    或者 datas.data[i].lvl_id    --i 表示第几个data数组,i从0开始


