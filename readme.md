# NodeJS

## day1-1

### 复习
* 模块化
    * 为什么要模块化
        * 维护迭代
        * 分工
    * 分类
        * 内置模块
            * fs
            * http
            * url
            * path
        * 自定义模块
            * 定义模块
                * module.exports
                * exports
            * 引用模块
                * require
        * 第三方模块
            * express
    * 规范
        * commonJS        NodeJS
        * AMD             require.js
        * CMD             sea.js
        * ES Module       ES6

* 开启http服务器


* 安装第三方模块
    > npm install

### 知识点
* 开启http服务器
    * http
    * fs
    * url
    * path
* mime类型

* 全局变量
    * __dirname: 当前文件所在目录
        > D:\class\gzh2008\w1_NodeJS\img
    * __filename: 当前文件的绝对路径
        > D:\class\gzh2008\w1_NodeJS\img\jj.png
* express


## day1-2

### 复习
* http静态服务器
    > html,css,js,图片,媒体文件等文件
    * 原生node
        * http,fs,url,path
        * mime（默认纯文本：text/plain）
    * express
* 模块化开发
    * 分类
        * 内置
        * 自定义模块
        * 第三方模块
            * 安装：npm install xxx
    * 规范
        * commonJS      nodeJS          同步
        * AMD           require.js      异步
        * CMD           sea.js          异步
        * ES Module      ES6            同步
    * 使用
        * 导出（定义）
            * module.exports
            * exports
        * 引入
            * require()
* 环境变量
    * 用户变量
    * 系统变量
* 安装第三方模块
    > npm install module
    * 参数
        * --save        -S  （在npm4后默认为--save）
            > 把安装信息记录在package.json中的`dependencies`
        * --save-dev    -D
            > 把安装信息记录在package.json中的`devDependencies`
        

### 知识点
* express中间件
    > 中间件是一个封装了某些处理数据功能的**函数**
    * 使用
        > 格式：`app.use([path],...middlewares)`
    * 分类
        * 内置中间件
            * express.static()
* 接口(路由)
    * RESTful规范
        * 利用不同的请求类型实现不同的接口
        * 利用不同的访问路径实现不同的接口
    * 模块化 + 中间件
        * express.Router()
    * 请求类型
        * get               查
        * post              增
        * put/patch         改
        * delete            删
* 传参
    * 动态路由： /goods/:id
        > 获取：req.params.id
    * url查询参数：/goods/list?page=1&size10&category=phone
        > 一般用于get请求，获取：req.query.page
    * 请求体：利用express中间express.urlencoded(),express.json()
        > 获取：req.body
    * 请求头
        > req.get(name)


## day1-3

### 复习
* express
    * 中间件middleware
        * 是一个函数
        * 分类
            * 内置中间件
                * express.static()
                * express.Router()
                * express.urlencoded()
                * express.json()
            * 自定义中间件
            * 第三方中间件
        * 使用： app.use([path],中间件1,中间件2...中间件n)
    * 路由router（接口）
        ```js
            app.use(function(req,res,next){
                // 任何请求都会进入这里
            });

            app.use('/goodslist',function(req,res,next){
                // 请求路由为/goodslist时，执行这里的代码
            })

            app.get('/goodslist',function(req,res,next){
                // 请求路由为/goodslist且为get请求时，执行这里的代码
            })

            app.post('/goodslist',function(req,res,next){
                // 请求路由为/goodslist且为post请求时，执行这里的代码
            })

            // 获取商品信息
            app.get('/goods/:id',function(req,res,next){
                // 请求路由为/goodslist且为get请求时，执行这里的代码
            })
            // 删除商品信息
            app.delete('/goods/:id',function(req,res,next){
                // 请求路由为/goodslist且为get请求时，执行这里的代码
            })
        ```
            * 动态路由
* 请求类型
    * get           查
    * post          增
    * put/patch     该
    * delete        删
    * ...
* RESTfull规范
    > 遵循RESTfull规范的接口称为RESTfull api
    * 根据不同的**请求类型**实现不同的接口
    * 根据不同的**请求地址**实现不同的接口
* 路由模块化
    > 利用commonJS模块化思想+中间件实现路由模块化
    ```js
        // user.js
        const express = require('express');
        const router = express.Router();

        // get /api/user/login
        router.get('/login',function(req,res){

        })

        // post /user/reg
        router.post('/reg',function(req,res){

        })
        module.exports = router;


        // index.js
        const express = require('express');
        const router = express.Router();

        const userRouter = require('./user');

        // /api/user
        router.use('/user',userRouter)
        router.use('/goods',goodsRouter)
        router.use('/cart',cartRouter)

        module.exports = router;


        // server.js
        const express = require('experss');

        const app = express();
        const rootRouter = require('./api');

        app.user('/api',rootRouter)

        app.listener(2008,function(){
            console.log('服务器启动成功...')
        })
    ```
* 传参
    * 传参方式
        * url
            * 动态路由: /goods/:id
            * 查询参数：?username=laoxie&password=123
        * 请求头
        * 请求体
    * 接收方式
        * 动态路由：req.params
        * 查询参数（get请求）：req.query
        * 请求头：req.get('User-Agent')
        * 请求体（post/put/patch）：后端如何接收请求体的数据，取决于前端通过什么类型传递参赛
            * content-type
                * formData: application/x-www-form-urlencode        express.urlencoded()
                * json:     application/json                        express.json()
                * multipart/form-data                               multer
            * req.body: 只有使用了相应中间件处理后在req.body中才能得到数据
            ```html
                <form action="http://localhost:2008/api/user/login" method="post">
                    <input type="text" name="username" />
                    <input type="password" name="password" />
                    <select name="gender">
                        <option value="male">男</option>
                        <option value="female">女</option>
                    </select>
                    <button>登录</button>
                </form>

            ```
### 知识点
* mysql
    * 增：`insert into user(username,password,age) values('laoxie','123654',18)`;
    * 删：`delete from user where username='jingjing'`
        * 删除名字中有`jing`: delete from user where username like "%jing%"
    * 改：`update user set age=38 where username='jingjing'`
    * 查：
        * `select * from user`
        * `select * from user where age>30 and gender='女'`
    * 条件
        * where
        * order by 
            * asc   升序（默认）
            * desc  降序
        * limit index,qty
            * 年龄前5名用户：`select * from user order by age*1 litmit 0,5`
* 在nodejs中使用mysql
    1. 安装：npm i mysql
    2. 引入：require('mysql')
* 统一前后端交互数据格式
    > {code,data,msg}

## day1-4

### 面试题
* 事件传播
    * 冒泡：
        * 事件委托
    * 捕获：
        btn.addEventListener('click',function(){},true)
    * 事件源对象：触发事件的节点对象
        > 在事件传播过程中不会改变： event.target
    * 阻止事件传播
        > event.stopPropagation(), event.cancelBubble=true;
* ES6中的扩展运算符: `...`
    * 扩展操作
    * 剩余操作
    ```js
        var goods = {name:'goods1',price:998,qty:10};
        var good2 = {...goods,price:1998}

        function sum(a,...b){

        }
        sum(10,20,30);
        sum(10,20,30,40);

        let {name,...o} = goods;

        let arr = [10,6,98,2,3,22,44,12,6,2,44];
        Math.max(...arr);// 等效于Math.max(10,6,98,2,3,22,44,12)

        [...new Set(arr)]
    ```

### 复习
* mysql
    * 增删该查（CRUD）
        * C: insert into 表(field1,field2....) values()
        * D: delete from 表 where条件
        * U: update 表 set field=value where条件
        * R: select * from 表
    * 在Node中使用mysql
        > 第三方模块：mysql
        * 连接数据库
            * 连接对象
            * 连接池
### 知识点
* mongoDB
* 在NodeJS中使用mongoDB：驱动
    * mongodb
    * mongoose


* mongoDB与mySQL对比
类型            数据库              表              数据
mysql           database            table           row
mongoDB         database            collection      document

* 在Nodejs中操作mongo
    * mongodb
* 封装增删该查
    * create()
    * remove()
    * update()
    * find()
* 统一前后端交互的数据格式
    > {code,data,msg}
    * code: 
        * 200：成功
        * 400：失败
    * data: 数据，没有数据时为空数组
    * msg：提示信息

## day1-5

### 知识点
* ObjectId： mongodb.ObjectId
    > 5fe44705b66bed4848f34799 -> ObjectId("5fe44705b66bed4848f34799")
* 搭建项目
    1. 创建目录
        * doc           文档目录
        * src           源码（后端）
        * public        静态资源服务器（前端）
        * package.json  项目配置文件
    2. 安装依赖
        > --save/-S, --save-dev/-D  -> npm install --production
        * express
        * mongodb
        * bootstrap
    3. 编写服务器
    4. 编写路由模块：接口

## day2-1

### 复习
* 全端（全栈）
    * 前端：javascript=ECMAScript+BOM+DOM
        > 客户端语言：代码在客户端执行执行
        * alert
    * 后端：NodeJS = ECMAScript+服务器概念（request+response）
        > nodejs是后端语言，在服务器执行的js
        * console
        * JSON
        * npm模块
            * fs
* 利用mongodb来编写数据接口
    * 路由模块化
    * RESTful API
    * mongodb操作封装

### 知识点
* 开发环境
    > git管理，在本地服务器中
    * 本地环境: 
        * 本地开发环境：development
            * 前后端分离
            * 自测没问题后才提交代码
        * 本地测试环境: test 
            * 业务逻辑
    * UAT环境(线上测试环境): UAT
        * 测试团对进行线上测试
    * 生产环境（正式环境）：production
        * 上线
* 跨域解决方案
    * jsonp
        > 利用script标签请求数据（js代码为全局函数的执行，并传递数据）
        * 关键点
            * 全局函数
            * script标签发起请求，并传递**函数名**
            * 后端返回js代码
        * 缺点
            * 只能get请求
            * 不是ajax请求
            * 需要编写全局函数
    * CORS：跨域资源共享（Cross Origin Resource Sharing）
        * 简单请求与复杂请求（修改了请求头的请求）
            * 复杂请求下浏览器会自动发起OPTIONS预请求
    
    > 以上两种跨域解决方案，都需要服务器的支持

    * 服务器代理
        > 利用服务器没有跨域限制的特性，在自己的服务器中向目标服务器请求数据，得到数据后，响应给前端
    * 爬虫
        > 获取html结构，筛选需要的内容
        * 分析html结构
        * 正则表达式
        * 爬取图片
* 页面渲染模式
    * 客户端渲染BSR(Browser Side Rendering)：html结构在浏览器生成
        * 前后端分离
        * 一般会有数据接口
        * 请求步骤
            1. 请求html页面，返回空的html页面
            2. 请求js文件，返回js内容
            3. 解释js代码，发起ajax请求，并返回数据
            4. 在浏览器端生成html结构并写入页面
            5. 浏览器渲染html
        * 优点
            * 用户体验更好
            * 更好分工与维护
    * 服务端渲染SSR(Server Side Rendering)：html结构在服务器生成
        * 前后端不分离
        * 请求步骤
            1. 请求html页面，返回完整html结构
            2. 浏览器渲染html
        * 优点：
            * 速度快，一般用于首页优化

## day2-2

### 复习
* 爬虫
    * request
    * cheerio   
    * 爬取图片
### 知识点
* fs模块
    * fs.readFile()
    * fs.writeFile()
    * 目录操作
        * mkdir()
    * Stream：流，一般用于大文件的读写
        * 读取流：`fs.createReadStream(path)`
        * 写入流：`fs.createWriteStream(path)`
        * 管道流：pipe()

        ```js
            gulp.src('./src/js/*.js')
            .pipe(uglify())
            .pipe()
        ```
* 编码
    * 图片base64：`data:image/png;base64,编码`

* 协议
    * http(s)：
        * 短连接
        * 无状态
        * 只能客户端主动，服务器被动
    * socket
        * 长连接
        * 有状态
        * 服务器可以主动

* websocket
    * 服务端
        > 使用第三方模块`ws`
    * 客户端
        > 需要支持websocket的浏览器

## day2-3

### 复习
* 协议
    * ip协议
        * IPv4：255.255.255.255
        * IPv6:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF
    * 通讯协议(共同语言，共同遵守的规则)
        * TCP
            * 三次握手
            * 四次挥手
        * http/https
            > s: SSL
            * 短连接
            * 客户端主动，服务端被动
            * 无状态
        * ws/wss
            * 长连接
            * 客户端和服务器都能主动
            * 有状态
        * UDP
            > 不保证对方能收到信息
* WebSocket
    * 客户端
    * 服务端
* 应用
    * 多人聊天室
        * 服务端：广播
        * 客户端：
            * 定义发送消息格式
    * 数据可视化
        * echarts: canvas
        * higchart: svg

### 知识点
* 前端三大框架
    * AngularJS->Angular(ng): 2009
        * MVC
        * 依赖注入
    * React: 2013
        * VirtualDOM 虚拟DOM
    * Vue: 2014
        > 结合了angular与react的优点，并做了优化
        * 特点：上手简单，易用
* 开发习惯的改变
    * 以前：节点操作、
        > 节点频繁操作会影响页面性能（节点操作不可避免，但可以减少）
    * 现在：数据驱动（修改数据后，框架会帮我们自动更新相应节点）

* 架构模式
    * MVC：最经典的分层
        * M（Model）: 数据层
        * V（View）：视图层
        * C（Controller）：控制层
    * MVP
        * M（Model）: 数据层
        * V（View）：视图层
        * P（Presenter）：理解为松散的控制器
    * MVVM
        * M（Model）: 数据层
        * V（View）：视图层
        * VM（ViewModel）: 类似于Presenter的控制器，在vue框架中，框架就是扮演VM的角色

* 模块化开发规范
    * CommonJS      Node
    * AMD
    * CMD
    * ESModule

    * UMD：AMD+CMD+script全局引入 

* Vue的使用
    * 数据绑定
        * 单向绑定：
            * {{}}：绑定在内容上
            * v-text
            * v-html
            * v-bind：绑定在属性上
                > 格式：v-bind:attr="data"
        * 双向绑定: v-model
            > 单向绑定+事件， v-model的原理：v-bind:value + v-on:input
            1. 数据层->视图层
            2. 视图层->数据层
    * 数据驱动
        > 逻辑思维的转变：从节点操作改成数据操作
    * 指令：html的自定义属性
        * v-bind    单项数据绑定（属性）
        * v-model   双向数据绑定
        * v-on      绑定事件
        * v-for     遍历数据
        * v-show    是否显示（显示与隐藏，如有频繁的显示隐藏操作，建议使用v-show，否则建议使用v-if）
        * v-if      实现显示（创建与删除，）
* 响应式属性
    > 数据的改变会更新视图

    * 属性特性
        * 存储器属性
            * configurable  可配置性（Boolean）
            * enumerable    可枚举性（Boolean）
            * getter        监听属性的读取
            * setter        监听属性的修改
        * 值属性
            * configurable  可配置性（Boolean）
            * enumerable    可枚举性（Boolean）
            * writable      可写性
            * value         值
    > 传统方式**添加**的属性，默认属性特性为true；通过Object.defineProperty()**添加**的属性，默认属性特性为false
* 数据绑定
    > v-on
    * 事件传参

## day2-4

### 复习
* 架构模式
    * MVC
    * MVP
    * MVVM
    * 分层
        * Model         数据层
        * View          视图层
        * Controller    控制器
        * Presenter     松耦合的控制器
        * ViewModel
* 响应式属性
    > 实例化Vue时，会遍历data中所有属性，把它们转成存储器属性（getter&setter），并写入Vue的实例

* 数据绑定
    * 单向
        * {{}}
        * v-text
        * v-html
        ```js
            <div v-text="username">用户名：</div>
            <div>用户名：{{username}}</div>
        ```
        * v-bind
    * 双向数据绑定：v-model
        * 视图->数据：事件（input,change...）
        * 数据->视图：getter&setter
* 指令：vue定义的html属性
    * v-text
    * v-html
    * v-bind: :
    * v-model
    * v-on: @
    * v-for
    * v-show
    * v-if/v-else-if/v-else
    ```js
        show = 10;
        <div v-if="show===1">1</div>
        <div v-else-if="show===2">2</div>
        <div v-else>3</div>
    ```
*  数据驱动
    > 逻辑思维的转变：从节点操作改成数据操作

### 知识点
* todolist
    * 查：遍历显示
    * 增：添加
    * 删：删除数据
    * 改：修改数据

* 特殊属性
    * ref
        > 给原生添加ref属性后，可以通过`this.$refs`获取所有的ref
* 事件
    * 绑定：v-on 简写：@
    * event
        * 默认为事件处理函数的第一个参数
        * 如果事件处理函数传递了参数，则需要手动传递$event
    * 事件修饰符：对事件的过滤


## day3-1

### 复习
* ref
    > this.$refs
* 事件处理函数
    * event：默认为事件处理函数的第一个参数，如需传参，则event对象需要手动通过`$event`传输
    * 传参
    * 修饰符
        > 格式：v-on:keyup.enter, v-on:click.stop

### 知识点
* 模块化开发
    * 维护
    * 分工
* 组件化开发
    > 如果说指令为html属性，组件就是创建一个html标签
    * 优点
        * 复用
        * 维护
        * 分工

* template
    * vue实例如果配置了tempalte模板，则以tempalte作为视图（替换掉el和它的内容：outerHTML）,如没有配置template，则默认以el所在的元素作为模板
* data
    > 在组件中data只能是函数形式

* 组件通讯
    * 组件结构：父子结构
    * 通讯方式
        * 父->子：props
            1. 在父组件模板中给子组件定义属性，并传递数据
            2. 子组件通过`props`接收
        * 子->父：自定义事件（也可以把方法传到子组件中执行）
            1. 在父组件模板中给子组件定义事件，事件处理函数为父组件的方法
            2. 在子组件中触发这个事件，并传递数据
        * 兄弟组件通讯
            1. 组件A->父组件（子->父）
            2. 父组件->组件B（父->子）
        * 深层次组件通讯
            * 逐层传递（不推荐）
                * 缺点：麻烦、结构的改变会导致通讯失效
            * 事件Bus
                1. 创建一个Vue实例Bus，用于事件绑定与触发
                2. 数据接收方：给Bus绑定自定义事件
                    > $on()
                3. 数据发送方：触发Bus的自定义事件，并传递参赛
                    > $emit()
                > 任意组件都可以绑定/触发自定义事件，在不创建Bus的前提下，可以使用$root取代Bus的作用
    * 数据传输：为单向数据流
        > 所有数据传到子组件后，不能在子组件修改数据，所以，在设计数据是，要遵循**谁的数据谁修改的原则**(修改数据的方法要放在数据所在的组件)
* 特殊属性
    * ref
    * is
    * key  

* 生命周期函数
    * created：创建完成

* 数据类型校验
    * props接收时定义数据类型、必填、默认值等操作
* 定义了props但不接受

* v-for
    * 数组
    * 对象
    * 数字
    * 字符串

## day3-2

### 复习
* 组件: 为了复用、分工、维护
    > 组件也是一个Vue实例, template, data必须为函数
    * 定义方式
        * 全局
        * 局部
    * 组件通讯
        * 父->子：props
            * 父组件定义
            * 子组件接收
        * 子->父：
            * 自定义事件
                * 绑定
                    * v-on
                    * $on()
                * 触发
                    * $emit()
                * 移除
                    * $off()
            * 把父组件的函数传到子组件执行
        * 兄弟->兄弟
        * 深层次组件通讯
            * 逐层传递（不推荐）
            * 事件总线：Bus, $root
    * 组件层级
        * 父组件：$parent
        * 子组件: $children
        * 根组件：$root
        * 应用
            * 子组件获取父组件的数据、方法：
                * props传递
                * $parent.show()
            * 父组件获取子组件的数据、方法
                * $children[0].show()
                * ref
                    * 给html元素设置ref属性，得到元素节点
                    * 给组件设置ref属性，得到组件实例
* 数据流：与组件层级有关
    * 单向数据流：主流框架全部采用单项数据流
    * 双向数据流：angularJS
* 数据绑定：与分层有关
    * 单向绑定
    * 双向绑定
* 数据类型校验
    * props:['idx']
    * props:{
        idx:[Number,String]
    }
    * 默认值：default
    * 必填: required
    * 自定义校验规则: validator(){}

### 知识点
* Vue-cli
    * 安装：` npm i -g @vue/cli`
    * 创建项目：`vue create projectName`
    * 目录结构
        * node_modules      项目依赖（第三方模块）
        * public            网站根目录
        * src               源码
            * main.js       入口文件
        * .gitignore        git过滤清单
        * babel.config.js   babel的配置文件
        * package.json      项目配置文件
        * package-lock.json 锁定依赖版本
    * 启动项目
        * npm script    npm脚本命令
            * 运行：`npm run xxx`

* 模块化开发规范
    * 分类
        * commonJS
        * AMD
        * CMD
        * ESModule
    * 导入
        * commonJS: require()
        * AMD/CMD: require()
        * ESModule: import，静态引入
            > 格式： import xxx from url; import {xxx} from url
    * 导出
        * commonJS: module.exports
        *  AMD/CMD: define()
        * ESMoudle: export
            > export后只能跟`function`、`class`、`var`、`let`、`const`、`default`、`{}`

* Vue单文件组件
    > 后缀为：`.vue`
* webpack
* ESlint
* Vue生命周期函数
    > 了解生命周期函数在什么时候执行，每个生命周期函数中适合做什么操作
    * beforeCreate
    * created
    * beofreMount
    * mounted
    * beforeUpdate
    * updated
    * beforeDestroy
    * destroyed

## day3-3

### 面试题
* 模块化开发规范与区别
    * commonJS      NodeJS              同步
    * ESMoudle      ES6                 同步（静态引入）
    * AMD/CMD       require.js/sea.js   异步
* Vue的computed与methods、data的区别
    ```js
        new Vue({
            data:{
                total:100
            },
            computed:{ // 缓存
                //sum(){ // getter
                    // 花费10s
                //},
                sum:{
                    get(){
                        return this.total;
                    },
                    set(newVal){
                        this.total = newVal
                    }
                }
            },
            methods:{
                cal(){
                    // 花费10s
                }
            }
        })

        cal();
        cal();
        sum
        sum
    ```
* jQuery中链式调用的原理
```js
    $('button').addClass('btn')
    $('button').on('click',function(){})
    $('button').attr('type','submit')

    $('button').addClass('btn').on('click',function(){}).attr('type','submit')
    // 在每个方法内返回实例this

    addClass(){

        return this
    },
    on(){

        return this
    },
    attr(){

        return this;
    }
```
### 复习
* Vue-cli
    * 安装：全局安装（为了在命令行使用，需要配置环境变量）
    * 使用：创建vue项目
        ```bash
            vue create projectName
        ```
* ESModule
    * 导出：定义模块并导出
        > 模块对象，ESModule把一个文件当作一个模块，一个模块就有一个模块对象
        * export: 给模块对象添加属性
            * function
            * let
            * const
            * var
            * class
            * default   给模块对象添加默认属性default
            * {}        给模块对象批量添加属性
            ```js
                export {}
                export default {}
            ```
        * as 改名
        ```js
            exoprt { laoxie as jingjing}
        ```
    * 导入：引入别的模块
        > 具有缓存特性：多次导入不会影响性能
        * import:
            * `import xx from <url> `   引入模块对象中的default属性
            * `import {xx} from <url>`  引入模块对象中的xx属性
            * `import {xx as xxx} from <url>`  引入模块对象中的xx属性，并赋值给xxx变量
            * `import * as xx  from <url>` 引入模块对象中所有属性并复制给xx变量

* vue单文件组件
    > 把html结构，js代码，css样式放在同一个文件中，后缀名为`.vue`
* vue生命周期
    * 生命周期函数
        * 实例化阶段
            1. beforeCreate     创建前（无法获取实例属性）
            2. created          创建后（可以获取实例属性）
            3. beforeMount      挂载前（数据挂载到视图前）
            4. mounted          挂载后（数据成功挂载到视图）
        * 更新阶段
            1. beforeUpdate     更新前
            2. updated          更新后
        * 销毁阶段
            1. beforeDestory    销毁前
            2. destoryed        销毁后
### 知识点
* 虚拟DOM（VirtualDOM）: 虚拟节点的集合（虚拟节点是结构类似与真实DOM节点的js对象）
    * 真实DOM：浏览器在解析html结构时，按照层级把每个元素渲染成Node节点，所有节点组合成一个树状结构的形状的**DOM树**
        > 节点的频繁操作会影响页面性能，节点操作不可避免，但可以减少
    * Vue是如何减少节点操作的呢：VirtualDOM
        * diff算法：在一个更新周期内，对比虚拟DOM前后状态，找出差异项，然后跟新真实DOM节点
            * key: 唯一且稳定
    ```js
        <div class="datalist">
            <h1></h1>
            <ul class="list">
                <li></li>
                <li></li>
            </ul>
        </div>

        {
            type:'div',
            props:{className:'datalist'},
            children:[
                {type:'h1',props:null,children:[]}
                {type:'ul',props:{className:'list'},children:[
                    {type:'li',children:'' props:{key:1}}
                    {type:'li',children:'' props:{key:2}}
                ]}
            ]
        }


        {
            type:'div',
            props:{className:'datalist clearfix'},
            children:[
                {type:'h1',props:null,children:[]}
                {type:'ul',props:{className:'list'},children:[
                    {type:'li',children:'', props:{key:2}}
                    {type:'li',children:'', props:{key:1}}
                ]}
            ]
        }
        

        <div>{{username}}</div>

        vm.username = 'laoxie'
        vm.username = 'jj'
        vm.username = 'jj'
        vm.username = 'laoxie'

    ```
* 插槽slot
    * 定义：组件内容->组件模板（由组件外传入组件内）
        * 默认插槽: default
        * 命名插槽: name="xxx"
            > 命名插槽可以让我们更精确地放置数据位置
        ```js
            // 默认插槽
            <slot/>
            // 命名插槽
            <slot name="list" />
        ```
    * 使用：v-slot（简写：#）
        > v-slot只能写在template或组件上
        ```js
            <template v-slot:list>
                // 这里的代码会写入到组件内的list命名插槽上
            </template>
        ```
    * 作用域插槽（有组件内传向组件外）
        > 为了实现可定制化
* 事件
    * v-on绑定在组件上的事件，默认无法生效

## day3-4

### 复习
* VirtualDOM
    * diff算法
    * key
* 插槽slot： v-slot
    * 外->内（组件内容->组件模板）
        * 默认插槽: name="default"
        * 命名插槽：name="xx"
            ```js
                <slot/>
                <slot name="xx" />
            ```
        * 使用
            ```js
                <mycomponent>
                    // 这里的内容会插入到默认插槽中
                    <template v-slot:xx>
                        // 这里的内容会插入到xx命名插槽中
                    </template
                </mycomponent>
            ```
    * 内->外：作用域插槽
        > 可定制化结构
        * 定义
            ```js
                <slot name="a" psw="b" />
                <slot name="xx" name="c" psw="d" />
            ```
        * 使用
            ```js
                <mycomponent>
                    <template v-slot:default="{name,psw}">

                    </template>
                    <template v-slot:xx="scope">
                        // 这里的内容会插入到xx命名插槽中
                    </template
                </mycomponent>
            ````
* 指令与修饰符
```js
    <mycomponent username="laoxie" class="btn" a="10" b="20" c="30" v-bind="{a:10,b:20,c:30}" />

    {
        name:'mycomponent',
        // props:['username'],// 接收props会成为组件实例的属性，不接收会成为组件根元素的属性（自动合并）
    }
```

### 知识点
* 内置组件
    * slot
    * component 组件容器
        * is
    * keep-alive
* vue版本
    * 完整版
        > 完整版=运行时版本+编译器(template->render)
    * 运行时版本（runtime-only）

* 多页面应用: MPA(Multiple Page Application)
    > 通过a标签跳转
* 单页面应用：SPA(Single Page Application)
    > 整个应用只有一个index.html页面
    * 路由：实现单页面多视图
        * 通过hash实现：锚点
* VueRouter
    * 路由类型
        * hash路由：根据hash值不同，渲染不同的内容（开发阶段推荐）
        * history路由（生产阶段推荐）
    * 使用
        1. 安装与引用: import
        2. 使用：Vue.use()
        3. 实例化路由并配置
            ```js
                new VueRouter({
                    routes:[{
                        path,
                        component
                    }]
                })
            ```
        4. 注入根实例
    * 内置组件
        * 显示路由内容：`<router-view/>`
        * 跳转页面：`<router-link/>`
    * 导航
        * 声明式导航
        * 编程式导航
            * $router   路由对象，一般用于路由跳转
            * $route    当前路由信息，保存当前页面的信息

* watch
    > 监听属性修改，可以监听子属性

## day3-5

### 面试题
* 生命周期函数执行过程
    1. beforeCreate
    2. created
    3. beforeMount
    4. mounted
* props与data
    * props为父组件传入的数据
    * data为组件的数据


### 复习
* VueRouter
    * hash路由（默认）： #
    * 使用步骤
        1. 安装引用
        2. 使用Vue.use()
        3. 实例化路由并配置
            * mode: hash（默认）,history
            * routes
                * path
                * component
        4. 注入实例
        5. 使用
            * 显示：<router-view/>
            * 导航
                * 声明式导航：<router-link/>
                    * to
                    * event
                    * tag
                    * active-class/exact-active-class
                    * replace
                * 编程式导航
                    * $router   路由对象，一般用与路由跳转
                        * push()
                        * replace()
                        * go()/back()/forward()
                    * $route    当前路由信息
* watch
    > 监听实例属性，还可以监听实例属性的子属性
* SPA: 
* UI框架
    * ElementUI 饿了么团队出品
    * iView 腾讯出品
    * ant-design  蚂蚁金服
    * vue-strap twitter
    * VantUI    有赞（移动端电商）

### 知识点
* axios
* 路由传参
    * 动态路由
        * 路径上使用变量：`/goods/:id`
        * 接收：$route.params
        > 如果通过params传递id，则必须使用name跳转
    * url参数
        > 写在问号后的参数：`/goods?id=2`
        * 接收：$route.query
    * params传参
        > 跳转是传参：$router.push({path:'/goods',params:{}})，刷新后消失
        * 接收：$route.params

## day4-1
### 复习
* 路由跳转
    * path
    * name
* 路由传参
    * 跳转时传参
        * params: 
            > 通过$route.prams获取，但刷新后消失（动态路由例外）
            * 动态路由: 配置路由时路径为变量`/goods/:id`
                > 获取：$route.prams.id
        * query
            > 通过this.$route.query获取，刷新后数据依然存在
            ```js
                this.$router.push({
                    path:'/goods',
                    params:{
                        id:123
                    },
                    query:{
                        username:"lx"
                        psw:'123456'
                    }
                })
                // 特殊params:动态路由
                this.$router.push({
                    name:'goods',// 等效于：path:/goods/123
                    params:{
                        id:123
                    },
                    query:{

                    }
                })
            ```
    * 定义时传参：props
* 数据持久化
    * url参数
        * path
        * query
    * 本地存储
    * 服务器存储
### 知识点
* 路由多视图
* 动态路由
    * 动态路由的改变，并更不会使组件的产生创建与销毁
        * watch
        * 路由守卫：路由生命周期函数
            * 全局守卫：写在路由配置文件中
                > 任何的跳转都会执行的函数
                * beforeEach()
                * afterEach()
            * 组件内的守卫：写在路由组件内
                * beforeRouteEnter()
                * beforeRouteUpdate()
                * beforeRouteLeave()
            * 路由独享的守卫: 写在路由配置中
                * beforeEnter()
            * 应用：页面访问权限
        * 路由跳转
            * 激活组件: to
            * 失活的组件: from
        
## day4-2

### 面试题
* 虚拟DOM
    * diff算法
    * key: 唯一且稳定
        * 没有key：采用复用原则
* 子组件在哪个生命周期函数最先获取到props传入的数据
    > created
* 父子组件的生命周期执行顺序
    1. 父组件beforeCreate
    2. 父组件created
    3. 父组件beforeMount
    4. 子组件beforeCreate
    5. 子组件created
    6. 子组件beforeMount
    7. 子组件mounted
    8. 父组件mounted
* 如何让代码停留5s
    ```js
        function sleep(time){
            return new Promise((resolve)=>{
                setTimeout(()=>{
                    resolve()
                },time)
            })
        }
        console.log('start')
        // 停留5s
        await sleep(5000);
        
        console.log('end');
    ```
### 复习
* 路由多视图
* watch
    > $route
* 路由守卫
    * 全局守卫
        > 是路由实例的方法，写在路由配置文件中
        * router.beforeEach()
        * router.beforeResolve()
        * router.afterEach()
    * 路由独享
        > 写在路由配置中
        * beforeEnter
    * 组件内的守卫
        * beforeRouteEnter
        * beforeRouteUpdate
        * beforeRouteLeave
    * 路由守卫参数
        * to
        * from
        * next()
* 路由重定向: redirect
    * / -> /home
    * 404
### 知识点
* 子路由: children
* ref
    * 写在html元素上：得到html元素节点
    * 写在组件上：得到组件实例
* axios二次封装
    * baseUrl
* Node环境变量：process.env.NODE_ENV
    * development
    * production
* 组件局部样式：scoped
    * 原理：自定义属性`data-v-[hash]`配合属性选择器实现

## day4-3

### 面试题
* watch,computed,method,data
    ```js
        {{data.username}}
        // can not read the property username of undefined
        // 不能读取undefined的username属性
        data(){
            return {
                data:{}
            }
        },
        created(){
            let data = ajax()
            this.data = {username:'laoxie'}
        }
        beforeMounte(){}
        // 报错
        mounted()
        beforeUpdate(){}

        updated(){}
    ```
* 在父组件中如何调用子组件的方法
    * 组件层级: 父组件实例.children[0]
    * ref: 父组件实例.$refs.xxx
### 知识点
* 加密解密
    * 单向加密
    * 双向加密（需要加密和解密）
        * 对称加密
            > 加密和解密同一把钥匙
        * 非对称加密
            > 有两把钥匙：公钥+私钥
            * 解密
                * 公钥加密，私钥解密
                * 私钥加密，公钥解密
        * https
            * ssl证书
* vuex
    > 是一个状态(state数据)管理工具
    * 核心配置
        * state     数据, 类似与组件中的data
        * getters   类似与组件中的computed
        * mutations 修改state的唯一方式，类似于组件中的methods
            > 调用mutation方法格式：`$store.commit('changeUser',{username:"laoxie",password:123456})`

## day4-4

### 面试题
* Vue组件通讯方式
    * 父->子：props
    * 子->父：
        * 自定义事件
            * v-on
                ```js
                    <sub-component v-on:add="addItem" ref="sub"></sub-component>
                    // 父组件代码
                    this.$refs.sub.$on('add',addItem);
                    this.$children[0].$on('add',addItem);
                    this.$refs.sub.show()

                    // sub-component代码
                    this.$on('add',addItem)
                    this.show()
                ```
            * $emit()
        * 把父组件的方法传到子组件执行
    * 深层次组件组件通讯
        * 逐层传递
        * Bus总线
        * 根实例：this.$root
        * Vuex
        * 注入系统：provide / inject
            1. 父组件利用provide共享数据
            2. 子组件通过inject接收数据
                > 接收后，可以通过this.xxx访问
* 父组件中如何调用子组件方法
    * ref
    * 组件层级
* 专业术语
    * BE（Backend）   后端
    * FE（Frontend）  前端

### 复习
* Vuex: 状态管理工具，用于数据共享
    * 使用步骤
        1. 安装引入vuex
        2. 使用vuex
        3. 实例化store，并配置参数
        4. 注入根实例
        5. 在组件中通过this.$store使用vuex
    * 核心配置
        * state     共享数据（类似与组件中的data）
        * getters   对数据的进一步处理（类似与组件中的computed）
            * 参数：state
        * mutations 修改state的唯一方式（类似与组件中的methods）
            * 参数
                * state     原来的数据
                * payload   传递的参数
            * 调用方式：$store.commit('mutation',参数)
        * actions: 一般用于异步操作（类似于mutations）
            * 调用方式：$store.dispatch('action',参数)
    * 组件中使用
        * 使用state: this.$store.state
        * 使用getters: this.$store.getters
        * 修改state: this.$store.commit('mutation',payload)
### 知识点
* vuex模块化（store模块化）
    * modules
        > 在默认情况下，模块化只会影响state的获取，其他不影响
        ```js
            // 模块化前
            this.$store.state.userInfo
            this.$store.getters.totalPrice
            this.$store.commit('add')
            this.$store.dispatch('initCart')
            // 模块化后(默认情况下，模块内部的 action、mutation 和 getter 是注册在全局命名空间的)
            this.$store.state.user.userInfo
            this.$store.getters.totalPrice
            this.$store.commit('add')
            this.$store.dispatch('initCart')
        ```
        * namespace命名空间： 解决了getter,mutations,actions的同名问题
            ```js
                new Vuex.Store({
                    modules:{
                        user:{
                            state:{
                                userInfo:{}
                            },
                            // 默认情况下，模块内部的 action、mutation 和 getter 是注册在全局命名空间的，对他们的访问和模块化前没有区别
                            getters:{
                                username(){

                                }
                            },
                            mutations:{
                                login(){},
                                logout(){

                                }
                            },
                            actions:{
                                login(){

                                }
                            }
                        },
                        cart:{
                            namespaced: true,
                            state:{
                                goodslist:{}
                            },
                            getters:{
                                // 之前：this.$store.getters.username
                                // 之后：this.$store.getters['cart/username']
                                username(state,getter,rootState,rootGetters){

                                }
                            },
                            mutations:{
                                // 之前：this.$store.commit('add')
                                // 之后：this.store.commit('cart/add')
                                add(state,payload){

                                },
                                login(){

                                }
                            }
                        },
                    }
                })
            ```
    * 映射
        > 把Vuex中的数据映射到组件实例内，用于简化代码操作
        * mapState([namespaced],state)              属性，映射到组件的computed
        * mapGetters([namespaced],getters)          属性，映射到组件的computed
            > 不支持函数写法
        * mapMutations([namespaced],mutations)      方法，映射到组件的methods
        * mapActions([namespaced],actions)          方法，映射到组件的methods

## day5-1

### 知识点
* 后台管理系统
    > 管理数据的系统
    * 商品
        * 添加
        * 删除
        * 修改
        * 查询
    * 用户
    * 订单
    * 物流
    * 票据
    * 活动
    * ....
* 上传
    * 前端
        * FormData: 表单数据，是一个构造函数
        ```js
            //<from action="http://localhost:2008/api/uploads/avatar" method="post" enctype="multipart/form-data">
            <input type='file' id="" />
            //</form>

            const data = new FormData()
            // 添加数据1： set()
            data.set('username','laoxie')

            // 添加数据2：append()
            data.append('hobby','小保健')
            data.append('hobby','大保健')
            data.append('hobby','保健pro')

        ```
        
    * 后端
        ```js
            // 编写上传接口

        ```

## day5-5

### 知识点
* 页面访问权限控制（路由守卫）
    * 购物车、我的等页面需要登录后才能访问
    * token: 令牌
        1. 登录成功，生成一个token，设置有效期，并返回到前端存储
            * jsonwebtoken
        2. 每次访问需要权限的页面，把token发到后端校验，校验成功放行，否则不允许访问
            * 失效
            * 篡改
* 单页面应用
    > 一个应用只有一个页面：index.html
    * hash路由
    * history: 刷新后404
        > 解决方案：除静态资源外所有的请求都响应index.html内容

* 项目验收
    * 编写readme.md
        > 放在项目根目录
        * 项目名称
        * 演示
            * 官网
            * 上线网址
            * git仓库地址
        * 团队与分工
            * 格式：组长：xxx，成员：xxx,xxx
            * 负责模块说明
        * 项目页面截图
            > 截有代表性得页面，不少于3张
    * 更新最新代码到github的master/main
    * 发邮件
        * 发送：xiejinrong@1000phone.com
        * 抄送: zhangjianze@1000phone.com,wuwenjing@1000phone.com
    * 录视频
        > 上传百度网盘
    * 提交到http://manage.qfh5.cn


## day6-1

### 面试题
* call,apply,bind三则的区别
    * 共同点：
        * 都是函数原型方法
        * 都能改变this指向
    * 不同点
        * call与apply参数不同
        * call&apply会自动执行函数，bind不会执行函数，而是会返回一个改变了this指向后的函数
        ```js
            //map,filter
            //arr.map(function(){})
            nodeList.map();// map is not a function
            Array.prototype.map.call(nodeList,function(item,idx){})
        ```
* 判断数据类型的方法
    * typeof
    * Object.prototype.toString

    ```js
        var a = 10;
        a.toString();//'10'
        Object.prototype.toString.call(a);
        function type(data){
            return Object.prototype.toString.call(data).slice(8,-1).toLowerCase();
        }
    ```
* this只能在函数中使用，表示当前对象，根据所处的环境不同代表不同的东西
    1. 是否通过 new 调用
    2. 是否通过 dot 调用
        * 包含事件绑定
    3. 是否被（call,apply,bind）修改过
    4. 指向window
* 事件中target与currentTarget的区别
    * target: 事件源对象（触发事件的元素）
    * currentTarget: 绑定事件的元素
### 知识点
* React
    * 版本：
        * cjs: commonJS
        * esm: ESModule
        * umd: 通用模块
    ```js
        new Vue({
            el:'#app',
            template:'',
            render(createElement){
                return createElement()
            }
        })
    ```
    * 创建节点：React.createElement()
    * 渲染节点：ReactDOM.render()
* JSX
    * babel编译
    * 语法
        * 不能使用js关键字
        * 使用驼峰
        * 结束标签
        * 使用变量必须使用{}
        * style内联样式必须使用对象写法
    ```js
        let data3 = 'data3'
        let list = <ul className="list">
            <li>data1</li>
            <li>data2</li>
            <li>{'data3'.toUpperCase()}</li>
        </ul>
    ```
* 组件化开发
    * vue
        ```js
            // 全局组件
            Vue.component('list',{render(createElement){}})

            // 局部组件
            components:{}
        ```
    * react
        > 首字母大写
        * 定义
            * 函数组件：无状态组件、UI 组件
                > 优先使用函数组件
            * 类组件：状态组件、容器组件
                * state
                * this
                    * 默认只有constructor,render,生命周期函数中可以直接使用this
                    * 其他自定义方法默认没有this指向，需要使用bind改变this指向
            ```js
                function List(){
                    return <div></div>
                }
            ```
        * 使用
            ```js
                <List></List>
            ```
* 数据渲染方式
    * js表达式：{}
    * 列表循环
        * map
    * state
        > 只能在状态组件中使用
        * 获取：this.state.xxx
        * 修改：this.setState()
            > 不建议直接修改数据，必须通过setState修改，setState为异步
            * setState(newState,callback)
                * newState: Object|Function
                * callback: 回掉函数，数据被修改后执行
    * 事件绑定
        * 驼峰
        * 事件处理函数默认没有this指向
    * props
        * 接收
            * 函数组件：函数的第一个参数
            * 类组件：this.props
    * ref
        * 字符串（丢弃）
        * 回调函数
        * createRef()
* 组件通讯
    * 父->子：props
    * 子->父：把父组件的方法传递到子组件执行

## day6-2

### 面试题
* Object.assign(target,obj1,obj2,...objN)
    * 把后面的对象属性扩展到target
    * 应用
        * 扩展对象属性
        * 浅复制
* webpack与gulp的区别
    * gulp基于任务的构建工具
    * webpack是基于配置的构建工具

### 复习
* 依赖
    * react
    * react-dom         webApp
    * react-native      nativeApp
* 创建虚拟节点：React.createElement(type,props,children)
* 渲染虚拟节点：ReactDOM.render(vNode,target)
* JSX
    * babel编译：html->js
    * 规则
* 组件
    * 函数组件（无状态组件，UI组件）
    * 类组件（状态组件，容器组件）
        * state
        * this
        * 生命周期函数
            * constructor() 初始化
            * render()      渲染函数
* 组件数据渲染方式
    * props
        * 函数组件：函数的第一个参数就是props
        * 类组件：
            * constructor的第一个参数
            * this.props
    * state: 只能在类组件中使用
        > state的修改，组件会自动刷新
        * 获取：this.state.xxx
        * 设置：this.setState(newState,callback)
            * newState: Object|Function
        ```js
            this.state = {
                datalist:[],
                idx:0,
                current:2
            }
            
            this.setState({
                datalist:[10,20,30],
                
            })

            this.setState(prevState=>{
                // prevState.idx:0
                return {idx:1}
            })
            this.setState(prevState=>{
                // prevState:1
                return {idx:2}
            })
        ```
    * 事件
        > 事件名采用驼峰写法
        * event
    * ref
        * 回调函数
            ```js
                <input ref={el=>this.el=el} />      
            ```
        * React.createRef()
            ```js
                this.el = React.createRef();
                <input ref={this.el} />

                // 获取：this.el.current
            ```



### 知识点
* 组件刷新的场景
    * state更新
* 事件
    * event
    * 传参
    * 改变this指向
        * bind: 多次bind只生效第一次
* 组件通讯
    * 父->子：props
    * 子->父：利用props把父组件的**方法**传到子组件执行，并传递数据
    * 深层次组件
        * 逐层传递
        * Context
            1. 创建 Context
                > 提供默认值
            2. 父组件 Provider共享数据
            3. 子组件接收
                * 函数组件
                    * Consumer
                * 类组件
                    * Consumer
                    * this.context
                        > 设置组件`contextType`静态属性
* 从0配置基于webpack的React环境
    * 目录创建
        * public
        * src
            * main.js
        * webpack.config.js
        * package.json
    * 依赖
        * react + react-dom
        * webpack + webpack-cli + webpack-dev-server
        * @babel/preset-react + babel-loader + @babel/core
        * html-webpack-plugin
    * webpack配置
        * entry
        * output
        * devServer
        * loader
            > module.rules
        * plugins
* webpack的工作流程
    从入口开始，分析整个项目的所有依赖，利用配置的loader和plugin编译相应的文件，并把它们打包成浏览器识别的文件

## day6-3

### 面试题
* hash路由原理
    > 核心：hashchange
    ```js
        window.onhashchange = function(){

        }
    ```
* hash路由如何改为history路由
    * 前端：mode改为'history'
    * 后端支持：让除静态资源与数据接口外的所有请求响应index.html
    ```js
        // node
        app.use(express.static('../public'))
        app.use('/api',allRouter)
        app.use((req,res)=>{
            fs.readFile('../public/index.html',(err,content)=>{
                res.set('content-type','text/html;charset=utf-8')
                res.send(content.toString())
            })
        })
    ```

### 复习
* Context
    > 解决跨组件通讯
    1. 创建Contenxt: 
        ```js
            const MyContext = React.createContext(defaultValue)
        ```
    2. 父组件共享数据：
        ```js
            <MyContext.Provider value={}>

            </MyContext.Provider>
        ```
    3. 子组件接收
        * 函数组件
            * Consumer
                ```js
                    <MyContext.Consumer>
                        {
                            (value)=>{
                                return (

                                )
                            }
                        }
                    </MyContext.Consumer>
                ```
            * Hook
        * 类组件
            * Consumer
            * this.context
                > 添加静态属性contextType
* Webpack
    * 什么是webpack：是一个构建工具（打包、合并、压缩、编译...）
        * gulp
        * grunt
    * webpack的工作原理: 从入口开始，分析整个项目的所有依赖，利用配置的loader和plugin编译相应的文件，并把它们打包成浏览器识别的文件
    * webpack与gulp的区别：
        * gulp任务驱动
        * webpack配置驱动
    * webpack的常用配置
        * entry     入口
        * output    出口
        * loader    加载器
            > module.rules
            * babel-loader
                * @babel/core
                * @babel/preset-react
            * css-loader + style-loader
            * sass-loader
        * devServer 测试服务器
            > webpack-dev-server
        * plugins   插件
### 知识点
* 移植TodoList
    * 模块化开发
    * 组件化开发
* @babel/plugin-proposal-class-properties
    * 静态属性
    * 箭头函数
* 练习：手动配置基于webpack的Vue环境
* props
    * children      类似于Vue中的slot
        * String    文本
        * Object    虚拟节点
        * Array     多个虚拟节点
    * Render Props
        > 使用一个值为函数的 prop 共享代码的简单技术（类似于 Vue 中的作用域插槽）
    * props校验
        1. 引入prop-types模块
        2. 设置组件的propTypes静态属性
            * 设置props的校验规则
    * props默认值
        > 设置组件的defaultProps静态属性
* state与props的区别
    * state为组件内部数据，可以通过setState()修改
    * props为父组件传入的数组，不能修改

* 生命周期函数
    > 只有在类组件中才存在生命周期函数，生命周函数中的this指向组件实例
    * 定义类组件
        * ES6 class
        * React.createClass()
    * Initial: 初始化阶段
        * constructor()
    * Mounting：挂载阶段
        * componentWillMount(弃用)
        * componentDidMount
    * Updating：更新阶段
        > 执行setState()会进入更新阶段
        * componentWillUpdate(弃用)
        * componentDidUpdate
    * Unmounting：卸载阶段
        * componentWillUnmount
    * 特殊生命周期函数
        * componentWillReceiveProps (弃用)
        * shouldComponentUpdate
* 组件刷新的场景: 所谓的刷新就是执行render函数
    * state更新
    * props更新

## day6-4

### 复习
* props
    * 普通数据
    * Render Props
    * children
        * Null      <Button></Button>,<Button/>
        * String    <Button>dddd</Button>
        * Object    <Button><span>dddd</span></Button>
        * Array     <Button><span>dddd</span><strong>dddd</strong></Button>
        * Function  <Button>{()=>{}}</Button>
    * props类型校验
        > prop-types
        * 设置propTypes静态属性
    * props默认值
        * 设置defaultProps静态属性
* 生命周期函数
    > 只有类组件才有生命周期
    * 初始化阶段
        * constructor()
    * 挂载阶段
        * componentWillMount（弃用）-> UNSAFE_componentWillMount
        * componentDidMount
    * 更新阶段
        * componentWillUpdate（弃用）UNSAFE_componentWillUpdate
        * componentDidUpdate
    * 销毁阶段
        * componentWillUnmount
    * 特殊生命周函数
        * componentWillReceiveProps（弃用）UNSAFE_componentWillReceiveProps
        * shouldComponentUpdate(nextProps,nextState)
### 知识点
* 生命周期函数的执行过程
* 生命周期函数适合做哪些操作
    * constructor
        * 初始化state
        * 改变函数this指向
    * componentDidMount
        * ajax
        * 定时器
        * DOM 节点的操作
        * 读取本地存储数据
    * componentDidUpdate(prevProps,prevState)
        > 在该生命周期函数中修改state时，要注意避免死循环
        * ajax
    * shouldComponentUpdate(nextProps,nextState)
        > 该生命周期函数一定要返回Boolean（true:允许渲染（默认），false：不允许渲染），一般用于优化组件性能
        * PureComponent: 内部已经进行shouldComponentUpdate优化的组件，内部不能使用shouldComponentUpdate生命周期函数
    * componentWillUnmount
        * 取消ajax
        * 清除定时器

* 组件刷新的场景
    > 所谓的刷新组件，就是执行组件的render函数
    * state改变
        * nextState（将要改变的值）与this.state（当前值）
    * props改变
        * nextProps（将要改变的值）与this.props（当前值）
    * 父组件刷新
        > 子组件也会跟着刷新（但这种刷新没有必要，可以利用shouldComponentUpdate优化，更简单的优化方式：PureComponent）
    * 强制刷新
        this.forceUpdate()
    ```js
        state = {
            num:1,
            page:1
        }
        this.setState({
            num:2
        })

        shoulComponentUpdate(nextProps,nextState){
            // this.state.num: 1
            // nextState.num:2
            if(this.props.xxx === nextProps.xxx){
                return false;
            }
        }
        render(){}
        componentDidUpdate(prevProps, prevState){
            this.state.num:2
            prevState.num:1
            // 在该生命周期函数中修改state一定要慎重，避免死循环，一般需要条件判断
        }
    ```

* React-router
    > 组件化
    * 路由类型
        * hash： HashRouter
        * history: BrowserRouter
    * 路由配置
        * Route
            * path
            * component
            * render
            * exact
        * Redirect
        * Switch

* 导航
    * 声明式导航
        * Link
        * NavLink
    * 编程式导航
        > 要使用编程式导航，必须拿到history、location、match对象
        * 如何获取history,location,match对象？
            * 通过<Route>组件的component属性渲染组件
                > 会自动往组件内传入location.history,match
            * withRouter高阶组件
                > 高阶组件并不是一个React组件，而是一个包装函数
            ```js
                <Home location="xxx" history="xxx" match="xxx" />
                <Route component={Home}>
            ```
* ReactUI框架
    * ant-design

## day6-5

### 面试题
* 以最快速度找出数组中最大值
    ```js
        let arr = [10,20,2,23,21]
        Math.max(...arr);
        Math.max.apply(null,arr)
        // arguments.map(function(){})
        Array.prototype.map.call(arguments,function(){})
        [].map.call()

        // 以最快的速度找出数组中两个数相加等于13的数
        let arr = [1,2,3,4,5,6,7,8,9,10]
    ```
* call,apply,bind三者的区别
* review 
* 环境
    * development
    * production
    * UAT
* Vue组件通讯
    * 父->子：props
    * 子->父：
        * 自定义事件
        * 父组件的方法传到子组件执行
    * 多层级组件通讯
        * Bus
        * Vuex
        * 注入系统：provide / inject
* 函数柯里化
### 复习
* ReactRouter路由
    > 组件化
    * 路由类型
        * HashRouter
        * BrowserRouter
    * 路由渲染
        * Route
        * Redirect
        * Switch
    * 路由跳转
        * 声明式导航
            * Link
            * NavLink
        * 编程式导航
            > history,location,match
            * 获取
                * Route渲染组件
                * withRouter高阶组件
### 知识点
* 高阶组件HOC�������High Order Component）
    > 高阶组件并不是React组件，而是一个高阶函数，也是一个**纯函数**
    * 纯函数：不改变传入的参数，固定的输入有固定的输出
        ```js
            function sum(a,b){
                return a+b;
            }
            sum(1,2);//3
            sum(1,2);//3
            function randomNumber(min,max){
                return parseInt(Math.random()*(max-min+1))+min
            }
        ```
    * 自定义高阶组件
        * 属性代理
        * 反向继承
            > 只适用于类组件

## day7-1

### 复习
* 高阶组件：纯函数
    * withRouter
    * 自定义高阶组件
        * withUser
        ```js
            Login = withUser(Login)
        ```
        * withStorage
        ```js
            // withStorage返回值才是高阶组件
            Login = withStorage('currentUser')(Login)
        ```
* ES7装饰器：@
    > 依赖插件 `@babel/plugin-proposal-decorators`

### 知识点
* 路由传参
    * 动态路由: `/iq/:id`
        > 获取：match.params
    * 查询参数：`search="?a=10&b=20"`
        > 获取：location.search，可以配合`querystring`模块实现操作
    * state: 
        > 获取：`location.state`，刷新后消失
* 事件
    * event
        > 事件处理函数的最后一个参数
* 嵌套路由
    > Route的嵌套
    ```js
        // /discover/vue
        // /discover/react
        // /discover/node
        // 嵌套路由
        <Route path='/discover' component={Discover}>
        // Discover
        <Route path={match.path + '/vue'} component={Vue}>
        <Route path={match.path + '/react'} component={React}>

        // 动态路由
        <Route path='/discover/:cat' component={Discover}>
    ```
* 路由监听
    ```js
        history.listen((location)=>{

        })
    ```


## day7-2

### 知识点
* redux
    > 类似的状态管理工具：Flux, Mobx...
    * 核心
        * Store
            > 通过`createStore(reducer,initState)`创建
        * Reducer
            > 用于修改state，是一个纯函数，必须返回一个新的state
            * 参数
                * state
                * action
        * State
            > 状态，全局共享的数据
        * Action
            > 格式：{type:'xxx'}，通过store.dispatch(action)触发
    * 常用方法
        * 获取state
            > store.getState()，得到最新的state
        * 修改
            > store.dispatch(action)
        * 监听
            > store.subscribe(listener)
    * 使用步骤
        1. 安装
            ```bash
                # npm
                npm i -D redux
                # yarn
                yarn add redux --dev
            ```
        2. 引用、创建store，并定义reducer
            ```js
                import {createStore} from 'redux'
                const reuducer = function(state=initState,action){
                    switch(action.type){
                        case xxx:
                            return {}
                        case xxx:
                            return {}
                        default:
                            return state
                    }

                    
                }
                const store = createStore(reducer,initState)

                // store为一个对象，包含以下方法
                // {getState(),dispatch()}
            ```
        3. 操作
            * 获取：store.getState()
            * 修改：store.dispach(action)
            * 监听：store.subscribe(listener)
* 组件刷新场景
    * state改变
    * props改变
    * 父组件刷新
    * 强制刷新
* 封装一个高阶组件，实现redux与react组件的完美结合
    * withStore
* react-redux
    * `<Provider/>`   用于共享redux数据
    * `connect()`     高阶组件，用于向组件中传入数据
    * 使用步骤
        1. 利用`<Provider/>`共享store
            > 原理：`<Context.Provider/>`
        2. 利用`connect()`高阶组件在组件中定义需要的redux数据
            * mapStateToProps: 映射数据到组件props
            * mapDispatchToProps: 映射修改方法到组件props

## day7-3

### 复习
* redux
    * 使用步骤
        1. 安装redux
        2. 创建store
            ```js
                import {createStore} from 'redux'
                const store = createStore(reducer,initState)
            ```
        3. 操作
            * 获取：store.getState()
            * 修改：store.dispatch(action)
            * 监听：store.subscribe(fn)

    * 核心
        * store
        * reducer：用于修改state
            * 参数
                * state
                * action
        * state: 状态（共享数据）
        * action：动作/命名
* react-redux桥接工具
    * <Provider/>
        > 原理：利用Context技术共享redux数据
    * connect(mapStateToProps,mapDispatchToProps)
        > 原理：利用高阶组件代理redux属性到目标组件
        * mapStateToProps：映射数据到组件
        * mapDispatchToProps：映射修改方法到组件
            > 默认映射dispatch到组件
* 简化版redux
* 查看源码

### 知识点
* 模块化reducer
    > 利用combineReducers合并多个reducer，并组合成一个大的reducer，影响state的获取，但影响state的修改
* 页面访问权限控制
    > 高阶组件

* Action Creator: action构造器（定义创建action的函数）
    > 简化action编写
    * bindActionCreators：简化所有dispatch的操作

## day7-4

### 面试题
* 发版
* 估时间
* 联调
* 副作用 effect
* 单元测试 Unit Test
    * 测试用例

### 复习
* redux模块化：combineReducers
* Action Creator
    > 用于创建Action的函数，实现简化操作
    * bindActionCreators
* 调试程序
    * react developer tools
    * redux dev tools
        > 需要安装`redux-devtools-extension`，并配合中间件实现调试

### 知识点
* redux
    * state
    * reducer
    * 中间件

* VueX
    * state
    * getters
    * mutations
    * actions
    ```js
        actions:{
            atest(context){
                axios().then(res=>{
                    // ...很多代码
                    context.commit('xxxx')
                })
            }
        }

        $store.dispatch('atest')

    ```
* redux中间件
    * redux-chunk
    * redux-promise
    * redux-saga
        * generator 生成器函数
        * Iterator  迭代器
            * for...of  能遍历具有迭代特性的数据

    ```js
        function sum(){
            
        }
        sum();// undefined

        async function sum(){
        
        }
        sum();// Promise对象

        function * sum(){
            
        }
        sum();// Iterator
    ```
* 使用redux-saga步骤
    1. 安装与引入saga
        ```js
            import createSagaMiddleware from 'redux-saga';
        ```
    2. 创建saga中间件
        ```js
            const sagaMiddleware = createSagaMiddleware();
        ```
    3. 将 sagaMiddleware 连接至 Store
        ```js
            let enhancer = applyMiddleware(sagaMiddleware)
            const store = createStore(reducer,composeWithDevTools(enhancer))
        ```
    4. 引入并运行自定义Saga配置
        ```js
            import mySaga from './saga'
            sagaMiddleware.run(mySaga);
        ```

* 项目
    * 按需加载
    * 准备工作
        * 找项目
        * 爬数据
        * 写接口
        * webApp
    * 要求
        * 在组织中创建项目
        * 使用git
        
## day8-2

### 知识点

* 组件类型
    * 函数组件（无状态组件）
    * 类组件（状态组件）
        * state
        * 生命周期函数
        * this
* 组件刷新场景
    > 类组件：执行render方法；函数组件：从头到尾执行所有代码
    * props改变
    * 父组件刷新
    * state改变
        > this.setState()
    * 强制刷新
        > this.foreUpdate()
* Hook
    > 为函数组件而推出的功能
    * useState
        > 取代类组件中的State
    * useEffect
        > 取代类组件中的生命周期函数


## day8-3

### 复习
* HooK
    > Hook为一个函数
    * useState
        > 实现类组件state功能，返回一个数组：[数据,数据修改方法]，数据修改方法能实现函数组件的自我刷新
        ```js
            let [user,changeUser] = useState({
                    username:'xxx',
                    pasw:123,
                })
            let [score,changeScore] = useState({
                        en:100,math:120,cn:140
                    })
            changeUser({
                ...user,
                score:{
                    ...user.score,
                    en:90
                }
            })
        ```
    * useEffect
        > 在初始化时且组件渲染后执行一次
        * 默认用法
            > 初始化和数据更新时执行
            ```js
                console.log(0)
                useEffect(()=>{
                    console.log(1)
                })
                console.log(2)
            ```
        * 指定依赖
            > 初始化和依赖修改时执行
            ```js
                useEffect(()=>{
                    console.log(1)
                },[qty])
            ```
        * 空依赖
            > 只有初始化时执行
            ```js
                useEffect(()=>{
                    console.log(1)
                },[])
            ```
        * 返回一个函数
            > 返回的函数在组件被销毁时执行
            ```js
                useEffect(()=>{
                    // 
                    console.log(1)
                    return function(){
                        // 这里的代码在组件销毁后执行
                    }
                })

            ```
### 知识点
* useMemo
    > 返回计算结果
* useCallback
    > 返回传入的函数
* useContext
    > 简化context获取
* useReducer
    > 利用useReducer+useContext实现redux的功能（注意唯一数据源的问题）
* useRef
* useLayoutEffect   useEffect的同步版本

* 第三方Hook
    * react-router
        ```js
            function Home(props){
                const history = props.history
                const history = useHistory()
            }

            Home = withRouter(Home)
        ```
    * react-redux
        ```js
         function Home(props){
             const dispatch = useDispatch()
             const goodslist = useSelector((state)=>state.cart.goodslist)

             dispatch({type:'add',goods})
         }
         Home = connect(function(state){
             return {
                 goodslist:state.cart.goodslist
             }
         })(Home)
        ```
* 自定义Hook

## day9-1

### 知识点：小程序
* webApp
* NativeApp
* 小程序
    * 文件类型
        * .json         配置文件
        * .js           逻辑文件
        * .wxss         样式文件
        * .wxml         结构文件
    * 按作用范围分类
        * 全局通用文件
            > 放在项目根目录
        * 页面级别文件
            > 页面目录
* js逻辑文件
    * 类型
        * App   应用（全局）
            > 注册一个小程序
        * Page  页面（局部）
            > 注册小程序中的一个页面
            * 配置参数
                * data  初始数据
                    > 修改数据：this.setData()，数据修改后页面会自动刷新
* wxml常用语法
    * 数据绑定
        * 单向：{{}}
        * 双向：
            * 单向+事件
    * 事件绑定
        * bind
        * catch
        * event
        * 传参
            > 使用自定义属性`data-*`实现传参
    * 列表循环
        * wx:for
        * wx:key
            * 设置一个唯一**属性名**
            ```js
                classList:[{id:1,name:'2008'},{id:2,name:'2009'},{id:3,name:'2010'},]

                v-bind:key="item.id"
                key={item.id}
                wx:key="id"
            ```

* 小程序生命周期函数的执行顺序
* 导航
    * 声明式导航: <navigator>
    * 编程式导航: 接口
        * wx.navigateTo()

## day9-2

### 知识点
* 小程序页面类型
    * tabbar页面  
    * 普通页面
* tabbar配置
    > 由于收到很多限制，不够灵活，实际开发中一般很少使用tabbar配置，而是采用自定义tabbar（配置`custom:true`）
* 路由跳转
    * 声明式导航: `<navigator>`
        * open-type
    * 编程式导航: 接口
        * wx.navigateTo() 
        * wx.redirectTo()
        * wx.navigateBack()
        * wx.switchTab()
        * wx.reLaunch()
* 自定义tabbar
    > 自定义组件，自动出现在tabbar页面中
    1. custom:true
    2. custom-tab-bar
        * index.json
        * index.js
        * index.wxss
        * index.wxml
* 自定义组件
    * 定义
        * 构造器：Component()
        * 配置：component: true
    * 使用：`usingComponents`
        * 全局组件:在`app.json`中配置
        * 局部组件：在页面或组件中配置

* 内置组件

## day9-3

### 知识点
* 模块化
    * CommonJS      NodeJS
    * ESModule      ES6
    * AMD/CMD       require.js/sea.js
* 小程序模块化开发
    > 支持commonJS与ESModule
    * 使用npm模块
        > 在小程序中使用npm模块，必须重新构建（编译成小程序支持的格式）
    * wxs模块
        ```js
            <script>xxx</script>
            <script type="module"></script>
        ```
* wxml模板
    * 定义与使用：`<template>`
        * name      定义模板
        * is        使用模板
        * data      给模板传入数据
    * 引入
        * `<import/>`   引入template所有内容
            * src
        * `<include/>`  引入除tamplate和wxs外当所有内容
            * src
* 第三方组件库
    * weui-miniprogram
    * 使用步骤
        1. 安装
            > 必须配置`package.json`
        2. 构建
            > 把第三方模块编译成小程序支持的模块
        3. 引入
* ajax请求
    > wx.request() + Promise 封装ajax请求
    * 需要https合法域名


## day9-4

### 知识点
* 云开发
    * 准备工作
    * 资源
        * 数据库
        * 存储空间
        * 云函数(类似与服务器，用于编写数据接口)
* 使用云开发
    * 小程序端
        1. 初始化
            > 一个小程序只需要初始化一次，所以一般放在app.js中执行
            ```js
                wx.cloud.init({
                    env: 'qf-52690b'
                })
            ```
        2. 操作
            * 数据库
                ```js
                    // 获取数据库
                    const db = wx.cloud.database()

                    // 获取集合
                    const col = db.collection('city')

                    // 操作集合中的数据
                    // 1. 获取
                    col.get()
                    // 2. 增加
                    col.add({data:添加的数据)
                    // 3. 删除
                    col.where(条件).remove()
                    // 3. 修改
                    col.where(条件).update({data:修改的数据})

                    // 过滤与筛选

                    // 操作符
                    const _ = db.command
                ```
            * 存储文件
                ```js
                    // 获取文件真实地址
                    wx.cloud.getTempFileURL({
                        fileList:[文件id]
                    })

                    // 上传
                     wx.chooseImage({
                        success(res){
                            console.log('res=',res);
                            wx.cloud.uploadFile({
                                cloudPath: 'jj.png', // 文件名称
                                filePath: res.tempFilePaths[0], // 文件路径
                            }).then(res=>{
                                console.log(res.fileID)
                            })
                        }
                    })

                    // 下载

                ```
            * 云函数
                ```js
                    // 在小程序端调用云函数（可以规避一些权限问题）
                    const result = await wx.cloud.callFunction({
                        name:'xxx',
                        data
                    })
                ```

    * 服务端
        > 在服务端必须使用官方sdk（wx-server-sdk）获取cloud对象
        1. 初始化
            ```js
                const cloud = require('wx-server-sdk')
                cloud.init({
                    env: cloud.DYNAMIC_CURRENT_ENV 
                })

                // 定义云函数
                // 云函数入口：调用云函数，会自动执行这里的代码
                exports.main = function(){

                }
            ```
        2. 操作
            * 数据库
                ```js
                    const db = cloud.database();
                    const col = db.collection('category');
                ```
            * 存储文件
                
            * 云函数
                ```js
                    //在云函数中调用云函数
                ```

## day9-5

### 知识点
* 小程序开发模式
    * 使用独立服务器
        > 在小程序中通过ajax请求获取独立服务器的数据（必须使用https）
    * 云开发
        > 直接调用云函数获取数据
    * 独立服务器与云开发混合使用
        > 在自己的服务器中通过httpApi调用云开发接口（需要用户授权）
        ```js
            // 1. 获取access_token
            get https://api.weixin.qq.com/cgi-bin/token?grant_type=client_credential&appid=APPID&secret=APPSECRET

            // 2. 调用云开发http接口
            POST https://api.weixin.qq.com/tcb/databasequery?access_token=ACCESS_TOKEN
        ```

* 上线流程


## day10-1

### 知识点
* 小程序跨平台框架
    > 实现的原理：编译器
    * uni-app： 利用Vue开发小程序
        * HBuilder
        * VueCLI
    * mpVue: 利用Vue开发小程序
    * wePY: 采用类似于Vue的语法来开发小程序
    * taro
        * React
        * Vue
* 小程序限制
    * 不超过2M
    * 分包机制
        > 1.7.3推出分包机制，并把小程序总体大小提高到20M，但每个包的大小依然不能超过2m
        * 主包
        * 分包
    ```js
        ├── app.js
        ├── app.json
        ├── app.wxss
        ├── pages
        │   ├── index
        │   ├── mine
        │   └── logs
        ├── package-class
        │   ├── class
        │   ├── list
        ├── package-manage
        │   ├── class
        │   ├── user
        │   ├── category
        │   ├── city
        └── utils
        {
            "pages":[
                "pages/index/index",
                "pages/mine/mine",
                "pages/class/class",
                "pages/list/list"
            ],

            "subpackages":[
                // 分包配置
                {
                    "root": "package-class", // 分包根目录
                    "name":"class", // 分包别名，一般用于预加载
                    "pages":[
                        "class/class",
                        "list/list"
                    ]
                },
                {
                    "root": "package-manage", // 分包根目录
                    "name":"manage", // 分包别名，一般用于预加载
                    "pages":[
                        "class/class",
                        "user/user",
                        "category/category",
                        "city/city"
                    ],
                    "independent": true // 设置为独立分包
                }
            ]
        }

    ```
    
## day10-2

### 知识点
* hybridApp
    > 半Native半web的开发模式
    * 团队开发成员
        * **后端开发工程师**：数据接口联调
        * **Native工程师**：硬件接口联调
        * **前端开发工程师**：界面与业务逻辑

* Hybrid常见开发模式
    * Native原生主导开发（主流）
        1. Native工程师通过**window对象**提供接口，然后在js中调用
        2. web工程师通过**window对象**提供方法，然后在native（）中调用 
    * H5主导的混合开发模式
        > 团队中没有native工程师，借助一些第三方封装好的工具（如下）来实现应用的**打包**及**调用设备**的原生功能
        
        * 打包方式
            * 云平台
                > 提供了硬件接口与打包环境
                * DCloud平台的5+Runtime
                * APICloud
            * 本地打包
                * Phonegap/Cordova

    
## day10-3

### 知识点
* DCloud H5+接口调用
    * Camera
    * Barcode
    * Gallery
    * IO
    * NativeUI
    * Webview
* 打包
    * 下载安装

* React开发
    * webApp
        * react
        * react-dom
    * NativeApp
        > 利用web技术开发Native应用
        * react
        * react-native


## day10-4

### 知识点
* HTML5新特性
    * 语义标签
    * 增强型表单
    * 视频Video和音频Audio
    * SVG绘图
    * Canvas绘图
    * FileReader
    * FormDate
    * Geolocation
    * draggable拖放API
    * Web Worker
    * Web Storage
    * WebSocket
* Typescript
    ```js
        let username = 'laoxie'
        // 此处省略1万行代码...
        username = 18;
        username.length;// 6
        username.toUpperCase()

        // typescript
        let username:string = 'laoxie'
        username = 18; // 报错
    ```
    * ts的解决了js哪些痛点
        1. 类型校验：js无变量类型校验，容易出现一些安全隐患
        2. 调试:js的错误提示不是很完善，出现问题调式比较麻烦
        3. 提示：开发工具提示不完善
        4. 兼容：js兼容性问题
            * javascript
            * jscript
            * ECMAScript
    * 变量类型
        > 静态类型
        * 基础类型
            * string
            * number
            * boolean
        * 特殊类型
            * null
            * undefined
            * void
            * never
            * any
        * 字面量
        * 引用类型
            * 函数
                ```js
                    function getData(url:string,type:string='get'){

                    }
                ```
            * 数组
            * 对象

* typescript声明文件：`.d.ts`

* typescript编译
    * 手动编译：全局安装typescript，然后在命令行中通过tsc命令实现编译
        ```js
            tsc 文件 参数
        ```
    * webpack环境编译
        * 依赖
            * webpack + webpack-cli + webpack-dev-server
            * typescript + ts-loader
                > 一般需要配置tsconfig.json

            * @babel/core + babel-loader + @babel/preset-env

            ```js
                 module: {
                    rules: [
                        { test: /\.tsx?$/, use: [
                            {
                                loader:'babel-loader',
                                options:{
                                    presets:[],
                                    plugins:[]
                                }
                            },
                            "ts-loader"
                            ] 
                        },
                    ]
                }

            ```