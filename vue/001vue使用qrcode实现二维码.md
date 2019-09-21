[参考](https://blog.csdn.net/u010345462/article/details/81699910)
```js
安装
npm install qrcodejs2 --save

页面组件引入
import QRCode from 'qrcodejs2'

对应vue增加
<div id="qrcode" ref="qrcode"></div>

对应vue的methods添加方法
qrcode(){
    let qrcode = new QRCode('qrcode',{
        width: 200, // 设置宽度，单位像素
        height: 200, // 设置高度，单位像素
        text: 'https://www.baidu.com'   // 设置二维码内容或跳转地址
    })
}

mount初始化中调用该方法

this.qrcode();

```
