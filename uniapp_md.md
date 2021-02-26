# 记录基于vue开发的管理系统工作流程

-----------------------------`以下记录从零到完成登录功能已完成并经过测试`---------------------------

● **步骤一：**终端通过 vue ui （要先安装一些包，见vue项目构建和element笔记）打开项目创建页面，手动配置项目，其中

babel 、router(路由，不使用历史版本)、

linter/formatter（做代码格式化，选择标准配置、ESLint + Standard config）

使用配置文件（单独实用配置文件避免 .json 文件过于饱满）为必选，其他根据需求选择

最后创建项目，根据需求选择是否保存预设，记得先试试项目是否能够跑起来 **npm run serve**

**● 步骤二：**安装项目所需插件 element-ui 界面美化、所需依赖 axios 请求发送接收

**● 步骤三：**在马云官网通过教程生成公钥，并添加，然后将本机机器加入可信列表

● **步骤四：**码云创建仓库，cd到本地的项目文件夹，查看状态 git status ，将文件全选 git add . 并提交到本地 git commit -m "first"，按照码云仓库提示进行推送到在线仓库

● **步骤五：**配置后台项目支持前端开发，安装 MySQL 数据库和 Node.js ，将数据库文件导入，测试后台接口是否可以正常运行，根据后端提供的 package.json 文件通过 npm install 安装所有依赖包，通过执行 node app.js 查看后台项目是否可以正常运行不报错，跑起来之后随便写点请求测试是否可以正常请求到服务器，确定接口正常之后完成此步骤，经过自己一顿折腾，成功运行开启数据库和运行服务器，并且实现请求访问，安装详情见（通过brew安装mysql）

● **步骤六：**初始化git，如果已经初始化就不必，将项目文件先提交到主分支 master ，然后通过 git checkout -b login 创建新的分支 ，做登录页面，git branch 查看分支

● **步骤七：**删除项目中不必要的模板内容，和杂七杂八的组件，就可以开始真正做项目了

```
// 记录到此碰到的问题 
// 删除默认的模板内容，一堆报错...
// 解决方案，直接把模板内容所需的组件和路由全删了，后面再建立
// 然后隐藏.eslintrc.js 中的 ’@vue/standard’ ，重新启动项目，就好，就是这里太坑了
// 通过 ssh 推送到码云时，也很坑，要求输入什么短语，其实就是系统密码
```

● **步骤八**：在 components 组件目录创建登录组件，并且在 route 路由目录的 index.js 文件中配置路由，记得在根组件中加上 <router-view/>  路由占位符，到此登录路由创建完毕，通过 /login 访问实验

```html
// components 目录中 Login 组件
<template>
    <div>登录组件</div>
</template>

<script>
export default {
    
}
</script>

<style lang="less" scoped>

</style>

// App.vue 根组件占位符
  	<!-- 登录路由占位符 -->
    <router-view></router-view>
```

```javascript
// route 目录中 index.js 路由配置
import Vue from 'vue'
import VueRouter from 'vue-router'
import Login from '../components/Login.vue'

Vue.use(VueRouter)

const routes = [
    // 访问'/'根路径时，自动跳转到，Login组件
    { path: '/', redirect: '/login' },
    { path: '/login', component: Login }
]

const router = new VueRouter({
    routes
})

export default router
```

然后开始写样式，注意这里的 .vue 格式无法读取成 html ，不能被插件读取并且格式化，可以在 vscode 设置文件 将 .vue 格式读取成 .html，写起来舒服点，(后面发现该方法也不友好，不支持less语法格式化，解决方案是通过插件vetur在vscode配置格式化)

然后还有些问题，写样式的时候或者其他地方，例如 less ，如果报错，是没装 加载器的原因，配置下载下 less 和 less-loader 就好，如果装完还是报错，将加载器版本降低到和 less 差不多，即可解决

对于 .vue 格式代码的格式化，安装插件 vetur ，并在vscode中设置官网所需的配置即可

● **步骤九：**使用element 表单

**1.**通过在表单身上进行属性绑定一个数据对象，data中定义该对象数据内容，分别在每个表单上通过 v-model 实现数据双向绑定

**2.**通过在表单身上进行属性绑定一个校验对象，在校验规则中定义好校验规则，最后在表单 item项中通过 prop 制定不同的校验规则

**3.**重置表单方法，为表单添加 ref 属性，值就是该组件的 $refs 对象，然后在函数中可以通过  this.$refs.loginFormRef 获取到该引用对象，最后调用 element 的 resetFields() 通过点击触发就好

4.调整格式化文档时，自动加分号，单引号变双引号的问题，根目录新建一个 .prettierrc 文件

```json
// 该文件是调整格式化是默认加分号和双引号的问题
{
    "semi":false,
    "singleQuote":true
}
```

```html
<!--html 部分  :model 绑定一个model属性 v-bind:model="loginForm"，rules 一样
ref="loginformref" 是对应该表单的实例对象-->
<el-form :model="loginForm" :rules="loginFormRule"  ref="form" label-width="0px" class="login_form">
    <!--用户名 prop对应规则验证的名称-->
    <el-form-item prop="username">
        <!--绑定 数据帮绑定，绑定到 loginForm中定义的对应名称-->
        <!--阿里字体图标，class用法 结合 element-->
        <el-input v-model="loginForm.username" prefix-icon="iconfont icon-yonghu"></el-input>
    </el-form-item>
</el-form>
```

```javascript
// 这是 默认出口 返回该表单的数据  
export default {
  data() {
	return {
      // 登录表单的数据绑定，将该对象用 v-bind:model="loginForm" 绑定到表单上，输入框通过
			// v-model="loginForm.username"，model="loginForm.username"，进行数据双向绑定
      loginForm: {
        username: "admin",
        password: "123456",
      },
      // 表单内容的验证
      loginFormRule: {
        // 用户名的验证
        username: [
          { required: true, message: "请输入登录名", trigger: "blur" },
          { min: 3, max: 8, message: "长度在 3 到 8 个字符", trigger: "blur" },
        ],
        // 密码的验证
        password: [
          { required: true, message: "请输入密码", trigger: "blur" },
          { min: 6, max: 15, message: "长度在 6 到 5 个字符", trigger: "blur" },
        ],
      },
    };
  },
  // 对表单内部的方法处理
  methods: {
    // 重置表单函数
    resetloginForm: function () {
      // console.log(this);
      this.$refs.loginFormRef.resetFields();
    },

    // 表单登录前的预验证 也是通过表单定义的 ref ，对该引用对象进行验证
    login: function () {
      // 这是element-ui中的表单预验证 validate返回值为布尔值 验证如果不是 true,就结束本次操作
      this.$refs.loginFormRef.validate(async (valid) => {
        if (!valid) return;
        // 调用axios发送请求
        // 直接解构请求到的数据，拿到其中的data 并命名 res
        const { data: res } = await this.$http.post("login", this.loginForm);
        // let meta = response.data.meta;
        // 调用ele组件中的提示，进行反馈用户
        if (res.meta.status !== 200) return this.$message.error(res.meta.msg);
        this.$message.success(res.meta.msg);
        
        // 在window对象下面的sessionstorag 保存用户token，类似session 记录用户的登录状态
        let token = res.data.token;
        window.sessionStorage.setItem("token", token); // 能保存，applocation看不到
        // 跳转到首页组件
        this.$router.push("/home");
      });
    },
  },
};
```

```javascript
// 路由守卫功能，当检测不到用户 token 时，不允许直接访问除登录外的其他页面
router.beforeEach((to,from,next)=>{
// to 是 将要访问的页面路径
// from 是从那个页面路径跳转而来
 // next 是放行函数 可以直接放行 next() 也可以指定地点
})

// 在路由文件中挂载文件
// 挂载路由导航守卫
router.beforeEach((to, from, next) => {
// 判断路径是否为 /login 如果是，就直接放行
    if (to.path === '/login') return next();
  
// 判断路径是否有 token 如果有，就直接放行，没有，就指定到登录页
    const token = window.sessionStorage.token
    if (!token) return next('/login')
    next()
})
```

```javascript
// 完成登录功能后，需要在首页添加一个退出功能，也就是清空token，并用编程式导航跳转到 login
// 清空token，在点击退出按钮时触发
  methods: {
    logout: function () {
      // 清空token
      window.sessionStorage.clear()
      //   重定向到 登录组件
      this.$router.push('/login')
    },
  },
```

● **步骤十**：完成登录功能，通过 git status 查看状态，并全选文件，先提交到本地暂存区，切换到 主分支，git checkout master ，通过 git merge login 将登录功能合并到主分支，然后推送到远程仓库，git push master

最后切换回登录分支，将登录分支内容也推送到远程仓库， 因为是不同的分支，所以推送是的分支也不一样，需要重新定义 git push -u origin login

-----------------------------`以上记录从零到完成登录功能已完成并经过测试`---------------------------



-----------------------------`以下记录从零到完成首页功能已完成并经过测试`---------------------------

● **步骤一：**将 element UI 中对应的菜单组件拿过来，并根据需求添加到 home 组件中，通过 axios 请求拦截器使用 `Authorization字段提供 token 令牌

```javascript
// 配置需要授权的 api 所需的 token 令牌，可能会出现报错问题，
//是自动生成的引入，删除这个就好了 import { config } from 'vue/types/umd'
axios.interceptors.request.use(config => {
    config.headers.Authorization = window.sessionStorage.getItem('token')
    console.log(config);
    return config;
});
```

● **步骤二：**通过v-for将数据渲染到页面时，可能会出现在 element 上添加 v-for 报错情况，是因为 vetur 配置问题，在 vscode 的 setting设置下就好   "vetur.validation.template": true 改成 false

```javascript
// 通过 axios 根据文档提供的地址获取参数（如果状态为 200），并在data中保存
getMenuList:async function(){
        const {data:menu}=await this.$http.get('menus');
        if(menu.meta.status !== 200) return this.$message.error(menu.meta.msg);
        this.menulist = menu.data
        console.log(this.menulist);
     }
```

● **步骤三：**由于每个菜单的图标都不一样，通过字体图标类名添加时，将所有类名在 vue data 中创建一个数组，然后刚好菜单是通过 v-for 循环而来，循环时，取到每个菜单的 index 

动态创建属性类名  `                      <i :*class*="iconArr[index]"></i>`，将图标按顺序添加到每个菜单前面

菜单展开时，可以每个都展开，但是需求是每次只可以展开一个，那么通过查看 element ui 文档，可以给 Menu 的  Attribute 属性添加 unique-opened 只保持一个菜单展开

```html
// 以下是通过 element ui 循环获取数据，以及添加对应属性
<el-aside width="200px">
                <el-menu background-color="#333744" text-color="#fff" active-text-color="#409eff" unique-opened>

                  <!-- 由于index 值相同会同时打开所有菜单，因此需要生成动态不同的数据，且不能为 数值，拼接成字符串即可  -->
                  <el-submenu :index="item.id +'' " v-for="(item,index) in menulist" :key="item.id"> 
                      <!-- 一级菜单模板区域 -->
                    <template slot="title">
                        <!-- 图标 -->
                      <i :class="iconArr[index]"></i>
                      <!-- 文本 -->
                      <span>{{item.authName}}</span>
                    </template>

                    <!-- 二级菜单选项 -->
                    <el-menu-item :index="subitem.id + ''" v-for="subitem in item.children" :key="subitem.id">
                        <template slot="title">
                            <!-- 图标 -->
                            <i class="el-icon-menu"></i>
                            <!-- 文本 -->
                            <span>{{subitem.authName}}</span>
                        </template>
                    </el-menu-item>
                  </el-submenu>
                    
                </el-menu>
            </el-aside>
```

● **步骤四：**不必每个菜单都创建一个新的路由，可以观察数据中每个对象是否具有 path 路径，然后开启 element-ui 的router模式，将 index 作为跳转的 path 进行跳转

```html
<!--在 menu 添加 router 属性 直接写就是true 也可以 :router="true"，加 : 才是动态元素-->
 <el-menu  router>
<!--  设置二级菜单的 index 的跳转路径，由于数据中的 path 没有斜线开头，所以不加-->
<!-- 二级菜单选项 -->
<el-menu-item :index="'/'+subitem.path" v-for="subitem in item.children" :key="subitem.id">
      <template slot="title">
          <!-- 图标 -->
          <i class="el-icon-menu"></i>
          <!-- 文本 -->
          <span>{{subitem.authName}}</span>
      </template>
</el-menu-item>
 </el-menu>
```

-----------------------------`以上记录从零到完成首页功能已完成并经过测试`---------------------------

