## Node.js之Sequelize起步

### 开始
> 作为一门后端语言，`Node.js`也有访问数据库的需求，而在`node.js`中使用较多的是`Sequelize`，`Sequelize`是一个基于`promise`的`ORM`框架，支持多种数据库方言，并且提供事务支持，读取复制的功能。

提前安装好Node，MySQL

> 使用npm初始化一个node项目
```shell
$ mkdir sequelize-demo
$ cd sequelize-demo
$ npm init
```

> 添加相关依赖

```shell
$ npm install --save sequelize
$ npm install --save mysql2
```

> 项目结构如下

```
sequelize-demo/
|
+- config.js <-- MySQL配置文件
|
+- app.js <-- 入口js
|
+- package.json <-- 项目描述文件
|
+- node_modules/ <-- npm安装的所有依赖包
```

接下来，就可以在app.js中操作MySQL了

> 创建一个sequelize对象实例，也就是根据config.js中我们已经写好的配置，创建一个数据库连接实例

```javascript
const Sequelize = require('sequelize');
const config = require('./config')

const sequelize = new Sequelize(config.database, config.username, config.password, {
    host: config.host,
    dialect: 'mysql',
    pool: {
        max: 5,
        min: 0,
        acquire: 30000,
        idle: 10000
    }
});
```

> 这一步之后可以测试数据库连接，是否可以成功

```javascript
// 测试连接
sequelize
    .authenticate()
    .then(() => {
        console.log('和MySQL建立数据库连接成功...')
    })
    .catch((err) => {
        console.log('数据库连接建立失败...', err)
    });
```

> 编写第一个Model，相当于定义好和数据库字段之间的映射关系，方便初始化数据库表，这和Java中的jpa很类似

```javascript
// 定义model时，传入名称user，默认的表名就是users。
// 第二个参数指定列名和数据类型，如果是主键，需要更详细地指定。
// 第三个参数是额外的配置，我们传入{ timestamps: false }是为了关闭Sequelize的自动添加timestamp的功能。
const User = sequelize.define('user', {
    id: { type: Sequelize.INTEGER, primaryKey: true },
    username: { type: Sequelize.STRING },
    password: { type: Sequelize.STRING }
});

// force: true will drop the table if it already exists
// 创建表，使用promise方式
User.sync({ force: true }).then(() => {
    // Table created
    return User.create({
        id: 1,
        username: 'steven rogers',
        password: '123456'
    }).then(p => {
        console.log('created.' + JSON.stringify(p));
    }).catch(function (err) {
        console.log('failed: ' + err);
    });
});

// await方式
// (async () => {
//     var user = await User.create({
//         id: 1,
//         username: 'steven rogers',
//         password: '123456'
//     });
//     console.log('created.' + JSON.stringify(user));
// })();
```
这里在初始化表的时候调用了sequelize的create方法，用于创建一条数据

> 查询数据，使用`findAll()`方法，同时可以进行条件查询

```javascript
// 查询数据
(async () => {
    const users = await User.findAll({
        // where: {
        //     username: 'luo'
        // }
    });
    console.log(`find ${users.length} users:`);
    for (let u of users) {
        console.log(JSON.stringify(u));
    }
})();
```

> 更新数据，使用的`save()`方法

```javascript
// 更新数据
(async () => {
    const user = await User.findById(1);
    user.username = 'hulk';
    await user.save();
})();
```

> 删除数据，使用`destroy()`方法

```javascript
// 删除数据
(async () => {
    const user = await User.findById(2);
    if (user !== null) {
        await user.destroy();
    }
})();
```

### 最后
Sequelize的使用还是比较简单，如果之前使用过Java进行web开发很快就可以上手，源码[在此](https://github.com/LuoLiangDSGA/node-learning/tree/master/sequelize-demo)。
