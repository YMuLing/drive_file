# 使用uniapp组件制作小程序项目流程

- ### 启程篇章

1. 全局安装项目所需的 vue-cli  `npm install -g @vue/cli`

2. 创建 uni app 项目 `vue create -p dcloudio/uni-preset-vue my-project`

3. 文件生成完毕之后，可通过 `npm run dev:mp-weixin`执行打包，在微信开发者工具通过打开打包完成的文件进行预览小程序

4. 完成了以上基本步骤就可以开始快乐的敲代码了，注意要用小程序语法进行开发



- ### 小程序基础配置

1. 我碰到了很恶心的一个坑，直接影响到第二步，来自官网的推荐安装方法，就是开发依赖，安装`node sass`以及`sass loader`，而且两者版本最好相距不大，官网推荐的名字都是错的...

2. 生成微信开发工具碰到的坑，在终端使用`npm run dev:mp-weixin`,不管这里怎么运行始终报错，

   问题出自第一步，以及需要在`pages.json`文件中`pages`数组上新增以下配置

```javascript
    "easycom": {
        "autoscan": true,
        "custom": {
            // uni-ui 规则如下配置
            "^uni-(.*)": "@dcloudio/uni-ui/lib/uni-$1/uni-$1.vue"
        }
    },
```

3. 在`pages.json`文件中`globalStyle`下进行，底部导航的基本配置，其他的看文档

```javascript
 "tabBar": {
        "color": "#8a8a8a",
        "selectedColor": "#d4237a",
        "backgroundColor": "#fff",
        "position": "bottom",
        "borderStyle": "black",
        "list": [{
                "pagePath": "pages/home/index",
                "text": "首页",
                "iconPath": "./static/icons/_home.png",
                "selectedIconPath": "./static/icons/home.png"
            },
            {
                "pagePath": "pages/video/index",
                "text": "视频",
                "iconPath": "./static/icons/_videocamera.png",
                "selectedIconPath": "./static/icons/videocamera.png"
            }
        ]
    }
```

4. 然后注意一下文件夹中文件的存放就好，像`static`什么的还是和pc端一样只存放静态文件
5. 最后才开始写就出现了10次找不到文件的问题，都是模块文件夹`node_modules`莫名其妙的找不到文件，最有效的解决方案就是终端`rm -rf  node_modules` 直接删除这个所有文件，然后终端`npm  i`全部重新装，因为没找到丢失文件的具体原因



```html
 <el-main class="main" :style="mainColor">
      <!-- (记录)) -->
      <div class="detail_container">
        <el-row :gutter="40" type="flex">
          <el-col :span="17">
            <div class="detail_cont" :style="bcColor + border">
              <div v-html="markDown" class="markdown-body " v-hljs></div>
            </div>
          </el-col>
          <el-col :span="7">
            <div class="detail_nav" :style="bcColor + border">234</div>
          </el-col>
        </el-row>
      </div>
    </el-main>
```



```css
/* Tomorrow Night Theme */


/* http://jmblog.github.com/color-themes-for-google-code-highlightjs */


/* Original theme - https://github.com/chriskempson/tomorrow-theme */


/* http://jmblog.github.com/color-themes-for-google-code-highlightjs */


/* Tomorrow Comment */

.hljs-comment,
.hljs-quote {
    color: #969896 !important;
}


/* Tomorrow Red */

.hljs-variable,
.hljs-template-variable,
.hljs-tag,
.hljs-name,
.hljs-selector-id,
.hljs-selector-class,
.hljs-regexp,
.hljs-deletion {
    color: #cc6666 !important;
}


/* Tomorrow Orange */

.hljs-number,
.hljs-built_in,
.hljs-builtin-name,
.hljs-literal,
.hljs-type,
.hljs-params,
.hljs-meta,
.hljs-link {
    color: #de935f !important;
}


/* Tomorrow Yellow */

.hljs-attribute {
    color: #f0c674 !important;
}


/* Tomorrow Green */

.hljs-string,
.hljs-symbol,
.hljs-bullet,
.hljs-addition {
    color: #b5bd68 !important;
}


/* Tomorrow Blue */

.hljs-title,
.hljs-section {
    color: #81a2be !important;
}


/* Tomorrow Purple */

.hljs-keyword,
.hljs-selector-tag {
    color: #b294bb !important;
}

pre {
    display: block;
    overflow-x: auto;
    background: #1d1f21 !important;
    color: #c5c8c6 !important;
    padding: 0.5em;
}

.hljs-emphasis {
    font-style: italic;
}

.hljs-strong {
    font-weight: bold;
}
```

- ### 测试导航1

```javascript
function setColor(color1, color2, color3, color4, color5, color6, color7, color8) {
    // 主题状态、 块背景色、 底部背景色、 标题文字、 内容文字、 提示文字、 底线、 上线、 全线

    return {
        bcColor: `background-color:${color1}; transition:all .3s;`,
        mainColor: `background-color:${color2}; transition:all .3s;`,
        titleColor: `color:${color3};`,
        textColor: `color:${color4};`,
        tipText: `color: ${color5}`,
        borderBottom: `border-bottom: 1px solid ${color6}`,
        borderTop: `border-top: 1px solid ${color6}`,
        border: `border: 1px solid ${color6}`,
        navColor: `color:${color7}`,
        inpBc: `background-color: ${color8};`
    }


}

export default {
    setColor
}
```



### 

```css
/* Tomorrow Night Theme */


/* http://jmblog.github.com/color-themes-for-google-code-highlightjs */


/* Original theme - https://github.com/chriskempson/tomorrow-theme */


/* http://jmblog.github.com/color-themes-for-google-code-highlightjs */


/* Tomorrow Comment */

.hljs-comment,
.hljs-quote {
    color: #969896 !important;
}


/* Tomorrow Red */

.hljs-variable,
.hljs-template-variable,
.hljs-tag,
.hljs-name,
.hljs-selector-id,
.hljs-selector-class,
.hljs-regexp,
.hljs-deletion {
    color: #cc6666 !important;
}


/* Tomorrow Orange */

.hljs-number,
.hljs-built_in,
.hljs-builtin-name,
.hljs-literal,
.hljs-type,
.hljs-params,
.hljs-meta,
.hljs-link {
    color: #de935f !important;
}


/* Tomorrow Yellow */

.hljs-attribute {
    color: #f0c674 !important;
}


/* Tomorrow Green */

.hljs-string,
.hljs-symbol,
.hljs-bullet,
.hljs-addition {
    color: #b5bd68 !important;
}


/* Tomorrow Blue */

.hljs-title,
.hljs-section {
    color: #81a2be !important;
}


/* Tomorrow Purple */

.hljs-keyword,
.hljs-selector-tag {
    color: #b294bb !important;
}

pre {
    display: block;
    overflow-x: auto;
    background: #1d1f21 !important;
    color: #c5c8c6 !important;
    padding: 0.5em;
}

.hljs-emphasis {
    font-style: italic;
}

.hljs-strong {
    font-weight: bold;
}
```

- ### 测试导航2

### 