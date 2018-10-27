## node.js之express起步

### 准备
- node.js环境

```shell
$ node -v
v10.4.1
$ npm -v
6.1.0
```
### 开始
> 创建一个目录

```shell
$ mkdir express-demo
$ cd express-demo
```

> 使用`npm init`命令为应用程序创建`package.json`文件

```shell
$ npm init
```
输入此命令之后会提示输入若干项，例如应用程序的名称和版本。如果没有特殊要求都可以使用默认设置。

> 在当前应用程序目录下安装express,然后将其保存在依赖项列表中。

```shell
$ npm install express --save
```

**采用 --save 选项安装的 Node 模块已添加到 package.json 文件中的 dependencies 列表。 今后运行 app 目录中的 npm install 将自动安装依赖项列表中的模块。**

> 在express-demo目录中，创建名为app.js的文件，编写代码如下

```javascript
var express = require('express');
var app = express();

app.get('/', function (req, res) {
    res.send('Hello World!');
});

app.listen(3000, function () {
    console.log('Example app listening on port 3000!');
});
```

> 运行应用程序

```shell
$ node app.js
```

应用程序会启动服务器，并且监听3000端口，我们可以通过访问根URL(/)来测试应用程序是否可以用，对于其他的URL访问，将会提示Cannot GET /xxx。
