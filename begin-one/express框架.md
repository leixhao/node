# express框架

```
产生的原因:
    1.http模块在处理路由这块比较鸡肋
    2.http模块在处理静态资源时比较麻烦
    3.http在获取浏览器的提交过来的参数时很麻烦
    4.express能解决url中有中文时,express能自动解码

基本概念:
express是nodejs发送网络请求的第三方框架,是NodeJS中一个优秀的 web 解决方案

地址:
https://www.npmjs.com/package/express
https://github.com/expressjs/express

安装:
    npm i/install express --save

```

------

# express使用步骤

```
利用express开启web服务器的步骤：
1、在当前项目中使用 npm i/install express --save
2、导入express包
3、利用express对象创建一个application对象 app
4、在app对象上就有一系列方法（get,post）还可以可以分别设置请求路由
5、利用app.listen()监听端口

GET方法获取参数：
    直接从 req.query中就可以获取,非常简单

POST方法获取参数:
    使用一个第三方包 body-parser

```

------

# express的路由

```
/man/xz
/man/ld

/woman/qz
/woman/sw

如何使用:
    1.将某一类路由规则放入到一个js文件中,写好相应的代码,并且暴露出去
        const express = require('express');

        let route = express.Router(); //创建一个路由
        路由的处理...

        module.exports = route; //将创建的路由对象暴露出去

    2.在启动服务器的js文件中,导入对应的路由,并且调用app.use方法使用即可
        const route = require('路由的路径');

        app.use('路由规则',route); //哪些路由规则适用于该路由


开发注意事项:
    设置路由一定要写在入口文件的代码后面一些,最好写在app.listen(xx) 的前一行即可;

```

------

# express之next方法

```
使用方式:(有两种)

    第一种:连写方式,用得比较少
        app.get('/',(req,res,next)=>{
            res.write('1.0 ');
            next();
        },(req,res,next)=>{
            res.write('2.0 ');
            next();
        },(req,res)=>{
            res.end('3.0 end');
        });

    第二种:分开写,用得比较多
        app.get('/',(req,res,next)=>{

            res.write('1.0');

            //触发下一个同样的路由的回调函数
            next('route');
        });

        app.get('/',(req,res)=>{

            res.end('2.0');
        });

调用的过程分析:
    当浏览器请求 http://127.0.0.1:8888/
    触发第一个回调函数，当在第一个回调函数中手工执行next()的时候
    就会触发第二个回调函数，一直下去直到最后一个回调函数被执行，那么最后
    这个回调函数可以不用写next参数

使用注意:
    触发下一个同样的路由的回调函数,路由必须一致

```

------

# express之通配符方法(all)与next方法联合起来做权限验证

```
使用方式:
    app.all('路由',(req,res,next)=>{

        if(登录过){
            next();  //继续往下执行真正的请求
        }else{
            res.end("请登录"); //提示用户登录
        }
    });

注意点:

```

## app.all(xxx); 一定要在所有路由的最前面

# express静态资源处理

```
使用方式:
    app.use(express.static('静态资源路径')); 

访问时的注意点:
    如果浏览器要访问statics下面的site.css
    url应该是 http://127.0.0.1:8888/site.css 
    如果是http://127.0.0.1:8888/statics/site.css 反而报错
    如果是statics下面的字母中的静态资源，则一定要在url中加上子目录

注意点:
    设置在路由或是app.use(路由),之前
    要在两个地方写,一个是html里面,还有一个地方是入口的js文件
```