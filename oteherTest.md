# 用于测试的md文档

### 基础部分01

**安装 vue脚手架(作用，快速生成项目基础架构)：**npm install -g @vue/cli

**创建的方式1：**vue create vue_proj_01 

**创建的方式2：**vue ui ，在ui界面操作，选项和上面一样

```javascript
// 手动选择安装方式，终端配置如下
Vue CLI v4.5.11
? Please pick a preset: Manually select features
? Check the features needed for your project: Choose Vue version, Babel, Router, Linter
? Choose a version of Vue.js that you want to start the project with 2.x
? Use history mode for router? (Requires proper server setup for index fallback in production) No
? Pick a linter / formatter config: Standard
? Pick additional lint features: Lint on save
? Where do you prefer placing config for Babel, ESLint, etc.? In dedicated config files
? Save this as a preset for future projects? (y/N) n

//项目构建完毕，可以先切换到目录，然后执行项目
 $ cd vue_proj_01
 $ npm run serve
```



**运行项目之后自动打开项目及地址配置：** 创建 vue.config.js 文件，并进行配置

```javascript
// 配置 vue 打包后的地址端口号，以及是否自动打开
module.exports = {
    devServer: {
        // 自动打开浏览器
        open: true,
        port: 8888
    }
}
```



**element-ui安装：**npm i element-ui -S

```javascript
// 手动配置element - ui  // 在入口文件中使用以下代码进行配置
import elementui from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css'
Vue.use(elementui) //安装到vue
```



**通过 vue 的可视化界面去安装 element：**

**1.**先创建项目

**2.**打开项目的插件面板，添加 vue-cli-plugin-element 插件

**3.**配置插件，将默认修改为按需导入

**4.**然后就可以在项目中使用了



**vue的模拟路由：**定义每个页面的组件，并添加到页面，通过获取哈希值的方式 + switch 判断，实现页面跳转效果

```html
<body>
    <div id="app">
        <a href="#/zhuye">zhuye</a>
        <a href="#/keji">keji</a>
        <a href="#/caijing">caijing</a>
        <a href="#/yule">yule</a>
        <component :is="comName"></component>
    </div>

    <script src="js/vue.js"></script>
    <script>
        // 定义需要被切换的组件
        const zhuye1 = {
            template: '<h1>主页信息</h1>'
        }
        const keji2 = {
            template: '<h1>科技信息</h1>'
        }
        const caijing3 = {
            template: '<h1>财经信息</h1>'
        }
        const yule4 = {
            template: '<h1>娱乐信息</h1>'
        }

        const vm = new Vue({
            el: '#app',
            data: {
                comName: 'zhuye1'
            },
            // 注册私有组件
            components: {
                zhuye1,
                keji2,
                caijing3,
                yule4
            }
        })

        // 获取 a 链接的 hash值也就是  #号开头部分的值
        window.onhashchange = function() {
            // location.hash 监听最新的 hash 值
            console.log(location.hash);

            // 判断并修改 vue 的值
            switch (location.hash.slice(1)) {
                case '/zhuye':
                    vm.comName = 'zhuye1'
                    break;
                case '/keji':
                    vm.comName = 'keji2'
                    break;
                case '/caijing':
                    vm.comName = 'caijing3'
                    break;
                case '/yule':
                    vm.comName = 'yule4'
                    break;
            }
        }
    </script>
</body>
```

```javascript
// 设置自动生成vue页面

{
"html5-vue": {
        "prefix": "vh", // 对应的是使用这个模板的快捷键
        "body": [
            "<!DOCTYPE html>",
            "<html lang=\"zh-en\">\n",
            "<head>",
            "<meta charset=\"UTF-8\">",
            "<meta http-equiv=\"X-UA-Compatible\" content=\"IE=edge\">",
            "<meta name=\"viewport\" content=\"width=device-width, initial-scale=1.0\">",
            "<title>Document</title>",
            "</head>\n",
            "<body>",
            "<!-- vue实例对象 el 的挂载区域 -->",
            "<div id=\"app\"></div>\n\n",
            "<script src=\"./js/vue.js\"></script>",
            "<script>",
            "// 创建 vm 实例对象",
            "const vm = new Vue({",
            "el: '#app',",
            "data: {}",
            "})",
            "</script>\n</body>\n",
            "</html>",
        ],
        "description": "HT-H5" // 模板的描述
    }
}
```



**vue Router vue官方的路由管理器**：详情见vue框架学习的 day02

### 其他记录02

**通过 babel 体验es6模块化、安装 babel 依赖包：**
npm install --save-dev @babel/core @babel/cli @babel/preset-env @babel/node

npm install --save @babel/polyfill

在项目根目录中创建 babel.config.js 文件，并进行配置并开放出口：

```javascript
const presets = [
    ["@babel/env", {
        targets: {
            edge: "17",
            firefox: "60",
            chrome: "67",
            safari: "11.1"
        }
    }]
]

module.exports = { presets };
```



### 功能使用03

**创建好功能并做好出口：**做好出口，并从 index.js 中引入，npx babel-node 输出文件，在一个文件中，只能有有一次默认出口，但却可以有 n 次的按需出口

```javascript
// m1.js 做好功能出口
let a = 10;
let d = 20;
let c = 30

function show() {
    console.log(123);
}

// 默认出口
export default {
    a,
    c,
    show
}

// 按需出口
export let aaa = 'aaa';
export let bbb = 'bbb';
export let ccc = function() {
    console.log(ccc);
}

// m2.js 单纯的一个值
for (let i = 0; i < 10; i++) {
    console.log(i);
}

// index.js 引入功能文件，并通过 npx babel-node index.js 输出结果    
// 默认引入
// import m1 from './m1.js'
// console.log(m1);

// 按需引入
// import { aaa } from './m1.js'
// console.log(aaa);

// 综合写法 ,号隔开 可以用 ele as ele 取别名，结果不变
//import m1, { aaa } from './m1.js'
//console.log(aaa, m1);

// 直接去执行某个文件的模块
import './m2.js'

```

 

**node安装jquery：** npm i jquery -s

 项目直接通过 src 链接到页面中（*import* $ *from* 'jquery'），并不受支持，所以需要将愈发进行转换，

**这里用到该依赖包：**npm i webpack-cli -D ，还需要在初始化文件中的
scripts中配置（  "dev": "webpack"）

**Mac安装 webpack 和webpack-cli 中遇到很多报错问题，下面是一位良心道友的解决方案：**

1. 前往Node.js的官网下载并安装它。（建议选择LTS版本，以前了解过这个也是一个前端框架，是用来在服务器端使用JS的，需要借助它的npm来安装webpack）

2. 查看webpack官方文档，说是建议采用本地安装，兜兜转转我最终还是用了全局安装，输入以下命令：

   sudo -s

   npm install webpack -g

   （sudo -s是为了获取安装权限，不使用它会出现一大堆error，然后安装完之后会有一些警告，不用管，那个貌似是说缺少一些组件）

3. 千万不要以为现在就能进行打包了，打包命令都是以webpack开头的，现在进行打包会报错：Command not found要想能够使用webpack命令，还得安装CLI：

   npm install -D webpack-cli

   （注意：必须在上一步产生的webpack文件夹中运行指令，路径user/local/lib/node_modules/webpack）

4. 这个时候仍然无法运行webpack命令，会报错：Cannot find module 'webpack'，运行一下指令即可解决：

   npm link webpack

5. 现在可以正常使用webpack了

```javascript
//创建 webpack 配置文件
const path = require('path')
module.exports = {
    // 编译模式
    mode: 'production', // development 单纯转换   production 压缩转换
    entry: path.join(__dirname, './src/index.js'), //文件在哪？ 使用绝对路径

    output: {
        path: path.join(__dirname, './dist'), // 输出位置设置
        filename: 'bundle.js' // 输出名称
    }
}

// 在 pabkage.json 文件中添加 dev 脚本
  "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1",
        "dev": "webpack"
    }
// 终端通过 npm run dev 执行打包，文件会自动输出到 dist/main.js
```



**6.**（除了mac安装需要获取权限外）在一些相关配置之后，就可以尝试打包了，通过 npm run dev  命令自动打包，并创建文件夹

**自动打包工具：**npm i webpack-dev-server -g -D

```javascript
// 到处都是坑，注意版本不一样可能无法运行 webpack-dev-server ,实测mac 可用版本搭配
 "dependencies": {
        "webpack": "^5.21.2",
        "webpack-dev-server": "^3.11.2"
    },
    "devDependencies": {
        "webpack-cli": "^3.3.12"
    }

// 如果使用这个自动打包刷新，需要将文件的引入方式更改为 绝对路径
    <script src="/bundle.js"></script>
```



**打开链接自动预览首页工具(打开webpack-dev-server打包后提供的链接，直接显示首页)：**npm install html-webpack-pligin ，以下是配置方法

```javascript
const path = require('path'); // 拼接路径模块
const htmlWebpackPligin = require('html-webpack-plugin'); // 引入预览工具
const htmlPligin = new htmlWebpackPligin({
    template: './src/index.html', // 复制谁
    filename: 'index.html' // 复制名子？
})

module.exports = {
    // 编译模式
    mode: 'development', // development 单纯转换   production 压缩转换
    entry: path.join(__dirname, './src/index.js'), //文件在哪？ 使用绝对路径

    output: {
        path: path.join(__dirname, './dist'), // 输出位置设置
        filename: 'bundle.js' // 输出名称
    },
    plugins: [htmlPligin] // 将插件的构造函数引入
}
```



**打包完成之后，自动打开文件：**--open  配置地址和端口    --host 127.0.0.1  --port 8888"

**设置方法，我觉得不太好用：**"dev": "webpack-dev-server --open --host 127.0.0.1  --port 8888"

web默认可以打包处理es6以下的 js 文件，如果是 es6 的 js 文件 会查看是否有 babel 这个 laoder 加载器去处理，如果是其他格式的文件，优先查找是否有合适的 loader ，有就由它来执行，没有就报错

**在配置打包loader的时候：** use数组中的指定loader是固定的，且调用顺序是从后往前，打包过程中出现报错情况，先调整版本，例如： less 和less-loader，版本不同会报错，调整版本相同后不报错

**打包处理css：**npm install style-loader css-loader -D 

**打包处理less：**npm install less-loader@4.1 less@4.1 -D ，

**打包处理sass：**npm install sass-loader node-sass -D

**自动添加css的兼容前缀的插件和loader：**npm i postcss-loader autoprefixer -D

**解决样式表中打包背景图(还有html页面中的图片)报错的问题：**npm i url-loader file-loader -D

**打包处理 js：**npm i  babel-loader @babel/core @babel/runtime -D  

**语法插件相关：**npm i @babel/preset-env @babel/plugin-transform-runtime @babel/plugin-proposal-class-properties -D

**打包处理 .vue 文件：**npm i vue-loader vue-template-compiler -D

```javascript
// 配置babel，并开放出口
module.exports = {
//预设
    presets: ['@babel/preset-env', ],
//插件
    plugins: ['@babel/plugin-transform-runtime', '@babel/plugin-proposal-class-properties']
}
```

```javascript
// 配置 postcss.config.js ,并开放出口
const autoprefixer = require('autoprefixer');

module.exports = {
    plugins: [autoprefixer]
}
```

```javascript
const path = require('path'); // 拼接路径模块
const htmlWebpackPligin = require('html-webpack-plugin'); // 引入预览工具
const htmlPligin = new htmlWebpackPligin({
    template: './src/index.html', // 复制谁
    filename: 'index.html' // 复制名子？
});

// 处理 vue 文件的插件
const vueLoaderPlugin = require('vue-loader/lib/plugin')

module.exports = {
    // 编译模式
    mode: 'development', // development 单纯转换   production 压缩转换
    entry: path.join(__dirname, './src/index.js'), //文件在哪？ 使用绝对路径

    output: {
        path: path.join(__dirname, './dist'), // 输出位置设置
        filename: 'bundle.js' // 输出名称
    },
 		// 将 .vue插件的构造函数 new 一下，放到插件数组中
    plugins: [htmlPligin, new vueLoaderPlugin()], // 将插件的构造函数引入
    module: {
        rules: [
            // 配置css打包文件以及浏览器兼容问题
            { test: /\.css$/, use: ['style-loader', 'css-loader', 'postcss-loader'] },
            // 配置less文件打包
            { test: /\.less$/, use: ['style-loader', 'css-loader', 'less-loader'] },
            // 配置 sass文件
            { test: /\.scss/, use: ['style-loader', 'css-loader', 'sass-loader'] },
            // 配置图片打包大小和格式、limit图片大于这个限制大小则以64进制转换
            { test: /\.jpg|png|gif|bmp|svg|webp$/, use: 'url-loader?limit=20480' },
            // 配置 babel 、exclude：指只转换自己写的js
            { test: /\.js$/, use: 'babel-loader', exclude: /node_modules/ },
            // 配置vue
            { test: /\.vue$/, use: 'vue-loader' }
        ]
    }
}
```



**在webpack项目中使用 vue：**

**1.**先安装vue，npm i vue -s 

**2.**在 src 的 index.js 入口文件中，通过 import Vue from 'vue'  ，导入 vue 构造函数

**3.**创建 vue 实例对象，并且制定需要挂载的 el 区域

**4.**通过 render 函数渲染 App 根组件

```javascript
// 在 index.js 入口文件中的配置
import Vue from 'vue'

// 导入单文件组件 //html 只设置一个挂载点，具体内容在单组件中完成
import App from './components/App.vue'

const vm = new Vue({
    el: '#app',
    render: h => h(App)
})
```



**上线之前 通过webpack 整体打包：**npm run build

```javascript
  "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1",
        "dev": "webpack-dev-server",
        // 通过该命令进行整体打包
        "build": "webpack -p"
    },
```

### 使用公共数据 04

1. 全局安装项目所需的 vue-cli  `npm install -g @vue/cli`

2. 创建 uni app 项目 `vue create -p dcloudio/uni-preset-vue my-project`

3. 文件生成完毕之后，可通过 `npm run dev:mp-weixin`执行打包，在微信开发者工具通过打开打包完成的文件进行预览小程序

4. 完成了以上基本步骤就可以开始快乐的敲代码了，注意要用小程序语法进行开发

```javascript
// 在 index.js 入口文件中的配置
import Vue from 'vue'

// 导入单文件组件 //html 只设置一个挂载点，具体内容在单组件中完成
import App from './components/App.vue'

const vm = new Vue({
    el: '#app',
    render: h => h(App)
})
```

打开Terminal，输入vi ./.bash_profile,回车，打开./.bash_profile文件：回车

现在已经打开了./.bash_profile文件，但是还处于查看模式，不能编辑。输入“i”，进入insert模式

这时就可以添加环境变量了，例如：

编辑完成，点击“esc键，退出insert模式”, 然后输入“:wq!”,回车，保存成功。 

1. 输入“source ./.bash_profile”，让环境变量生效。
2. 输入”echo $PATH”,查看环境变量，发现添加成功。 
3. 重新打开终端，环境变量就会生效了。

通过软件 sequel 链接到本地数据库，用户名搞对，就可以正确链接



### 安装工具06

打开Terminal，输入vi ./.bash_profile,回车，打开./.bash_profile文件：回车

现在已经打开了./.bash_profile文件，但是还处于查看模式，不能编辑。输入“i”，进入insert模式

这时就可以添加环境变量了，例如：

编辑完成，点击“esc键，退出insert模式”, 然后输入“:wq!”,回车，保存成功。 

1. 输入“source ./.bash_profile”，让环境变量生效。
2. 输入”echo $PATH”,查看环境变量，发现添加成功。 
3. 重新打开终端，环境变量就会生效了。

通过软件 sequel 链接到本地数据库，用户名搞对，就可以正确链接

**记录将本地服务器启动的办法：**

**1.**安装好 mysql

```
#安装mysql
brew install mysql
#启动mysql
brew services start mysql@5.7
# 关闭 mysql
brew services stop mysql
# 重启 mysql
brew services restart mysql
```

2.下载数据库可视化界面 sequel pro ，通过数据库目录下的配置文件中的 default.json 文件中的 “db_config”中的

### 一些问题07

l挂载点**：通过el获取元素，来将 data 中的数据对象信息添加到指定元素中

**data简单数据类型：**可以直接通过 {{message}} 的方式调用数据

**data复杂数据类型：**对象可以直接通过 ele. 的方式选取，数组可以直接通过 ele[0] 的方式选取

代码示例：


```html
<body>
    <div id="app">
        {{message}}
        <h2>
            {{school.mobile}}
        </h2>
        <h4>
            {{campus[0]}}
        </h4>
    </div>


    <!-- 引入vue -->
    <script src="js/vue.js"></script>
    <script>
        let app = new Vue({
            el: '#app',
            data: {
                message: '我是迪迦',
                school: {
                    name: '黑马程序员',
                    mobile: '400-618-9090',
                },
                campus: ['迪迦', '高斯', '阿古茹']
            }
        })
    </script>
</body>
```


**vue指令：**v- 开头的特殊语法

**v-text：**通过自定义属性的方式在标签中引入 data 数据中的属性内容，这种方式引入的值，在标签再写内容会无，如果只是替换部分内容，可以用 {{message}} 这种方式，不会覆盖原内容

**v-html：**使用场景，需要将 html 结构加入到页面中，就是innerHtml，就使用该方式，如果只是文本，则使用 v-text

**v-on：**为元素绑定事件，以下是

代码示例：