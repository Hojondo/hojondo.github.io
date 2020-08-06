---
title: 项目介绍 后台应用管理系统
top: false
cover: false
toc: true
mathjax: false
date: 2020-03-21 23:48:27
tags:
password:
summary:
categories: ['项目经验']
---

# 后台应用管理系统
> [线上Mock版 预览地址](/about/ebm)

## 业务简介
本系统 服务于企业内部存储和分享建模模型仓库的人员、部门及模型库管理。分4大方面：员工管理；建模软件权限分配；模型库管理及统计；企业信息及本系统设置

## 技术栈
- `Vue`前端开发框架.vuex、vue-router
- `iview`UI库
- `scss`css预处理
- `eslint`google代码规范
- `mockJs`拦截Ajax请求 生成随机数据
- `axios`promis库 前后端对接
- `gulp`vinyl-ftp自动部署静态资源到服务器
- `webpack`自搭建.webpack-devServer、webpack-merge、html-webpack-plugin、resolve.alias
- `babel` stage-3、ployfill

## 项目目录
```shell
    |-dist*
    |-build                     # webpack 配置文件
    |-config                    # 与项目构建相关的常用的配置选项
        |-index.js              # 主配置文件
        |-dev.env.js            # 开发环境变量
        |-prod.env.js           # 生产环境变量
    |-src
        |-main.js               # webpack 的入口文件；
        |-api                   # 存放api请求(文件名与模型名称基本一致,文件名使用小驼峰, 方法名称与后端restful控制器一致)
        |-components            # 存放项目共用的组件,通常是一些可复用的组件会单独存放在该目录
        |-router                # 存放前端路由相关配置
        |-store                 # vuex的目录
        |-directive             # 存放vue自定义指令
        |-filters               # 存放vue过滤器
        |-app/view              # 存放项目业务代码
            |-App.vue           # 根组件
            |-layout            # 整体布局
            |-...
        |-common                # 存放项目共用的资源，如：常用的图片、图标，样式，常量文件等等
            |-assets            # 存放项目共用的代码以外的资源，如：图片、图标、视频、字体 等
            |-libraries         # 存放自己封装的或者引用的库,如momentJS,clipboard ...
            |-styles            #
                |-index.scss    # 全局样式，出口文件
                |-mixin.scss    # 混合指令 定义可重复使用的样式（字体通用样式
                |-constant.scss # 存放scss的常量（主题相关通用颜色
                |-...
            |-...
    |-static                    # 存放不会被webpack处理，它们会被拷贝到输出目录下。适合与 webpack 不兼容的库，或在构建输出中必须使用特定名称的文件，
    |-package.json              # npm包配置文件，里面定义了项目的npm脚本，依赖包等信息
    |-README.md                 # 项目说明文件
    |-index.html                # HTML模板,webpack插件 html-webpack-plugin使用
    |-.babelrc                  # babel 的配置文件
    |-.editorconfig             # 编辑器的配置文件；可配置如缩进、空格、制表类似的参数
    |-.eslintrc.json              # eslint 的配置文件
    |-.eslintignore             # eslint 的忽略规则
```

## 页面组件嵌套逻辑
![思维导图](http://assets.processon.com/chart_image/5e764d4ee4b092510f6aa817.png)

## 重点Mark
### 自定义实现跨父子多层分发机制
```javascript
/**
 * 后代改祖先数据
 */
Vue.prototype.$dispatch = function(eventName, data) {
  let parent = this.$parent;
  while (parent) {
    parent.$emit(eventName, data);
    parent = parent.$parent;
  }
};
/**
 * 祖先改后代数据
 */
// 递归向上传播
function boardcast(eventName, data) {
  this.$children.forEach((child)=>{
    // 每一个子组件
    child.$emit(eventName, data);
    if (child.$children.length) {
      boardcast.call(child, eventName, data);
    }
  });
}
Vue.prototype.$boardcast = function(eventName, data) {
  boardcast.call(this, eventName, data);
};
```

### 路由守卫实现loading,各页面document.title等赋值操作
```javascript
router.beforeEach((to, from, next) => {
  // 路由发生变化修改页面title
  if (to.meta.title) {
    document.title = 'EBM - ' + to.meta.title;
  }
  iView.LoadingBar.start();
  next();
});
router.afterEach((route) => {
  iView.LoadingBar.finish();
});
```

### 通过`scss`实现通用风格样式整合和切换整体主题
颜色：
```scss
// common color
$color_adjunctive_sub: #808695;
$color_adjunctive_disabled: #C5C8CE;
$color_adjunctive_border: #DCDEE2;
$color_adjunctive_divider: #E8EAEC;

$color_intersperse_info: #2DB7F5;
$color_intersperse_success: #19BE6B;
$color_intersperse_warning: #FA9600;
$color_intersperse_error: #ED4014;

$color_adjunctive_foreground : #FFFFFF;

/**
 theme class: theTheme values vary in [theme_cyan, theme_blue, theme_red]
 */
$theTheme: theme_cyan;

// primary
.theme_color_primary{
  .theme_cyan &{
    color: #2D8CF0;
  }
  .theme_blue &{
    color: #156ECC;
  }
  .theme_red &{
    color: #D4575A;
  }
}
.theme_bg_primary{
  .theme_cyan &{
    background: #2D8CF0!important;
    border-color: #2D8CF0!important;
  }
  .theme_blue &{
    background: #156ECC!important;
    border-color: #156ECC!important;
  }
  .theme_red &{
    background: #D4575A!important;
    border-color: #D4575A!important;
  }
}
// lightPrimary
.theme_color_lightPrimary{
  .theme_cyan &{
    color: #5CADFF;
  }
  .theme_blue &{
    color: #3F90E6;
  }
  .theme_red &{
    color: #F27578;
  }
}
.theme_bg_lightPrimary{
  .theme_cyan &{
    background: #5CADFF!important;
    border-color: #5CADFF!important;
  }
  .theme_blue &{
    background: #3F90E6!important;
    border-color: #3F90E6!important;
  }
  .theme_red &{
    background: #F27578!important;
    border-color: #F27578!important;
  }
}
// darkPrimary
.theme_color_darkPrimary{
  .theme_cyan &{
    color: #2B85E4;
  }
  .theme_blue &{
    color: #0B55A4;
  }
  .theme_red &{
    color: #BE3F42;
  }
}
.theme_bg_darkPrimary{
  .theme_cyan &{
    background: #2B85E4!important;
    border-color: #2B85E4!important;
  }
  .theme_blue &{
    background: #0B55A4!important;
    border-color: #0B55A4!important;
  }
  .theme_red &{
    background: #BE3F42!important;
    border-color: #BE3F42!important;
  }
}
// label
.theme_color_label{
  .theme_cyan &{
    color: #E9F3FE;
  }
  .theme_blue &{
    color: #E7F0FA;
  }
  .theme_red &{
    color: #FBEEEE;
  }
}
.theme_bg_label{
  .theme_cyan &{
    background: #E9F3FE!important;
    border-color: #E9F3FE!important;
  }
  .theme_blue &{
    background: #E7F0FA!important;
    border-color: #E7F0FA!important;
  }
  .theme_red &{
    background: #FBEEEE!important;
    border-color: #FBEEEE!important;
  }
}


// adjunctive_content
.theme_color_adjunctive_content{
  .theme_cyan &{
    background: #515A6E!important;
    border-color: #515A6E!important;
  }
  .theme_blue &{
    background: #313C53!important;
    border-color: #313C53!important;
  }
  .theme_red &{
    background: #534D4D!important;
    border-color: #534D4D!important;
  }
}
// adjunctive_darkContent
.theme_color_adjunctive_darkContent{
  .theme_cyan &{
    background: #363E4F!important;
    border-color: #363E4F!important;
  }
  .theme_blue &{
    background: #253048!important;
    border-color: #253048!important;
  }
  .theme_red &{
    background: #4A4444!important;
    border-color: #4A4444!important;
  }
}
// adjunctive_background
.theme_color_adjunctive_background{
  .theme_cyan &{
    background: #F2F4F5!important;
    border-color: #F2F4F5!important;
  }
  .theme_blue &{
    background: #F2F4F5!important;
    border-color: #F2F4F5!important;
  }
  .theme_red &{
    background: #F5F2F2!important;
    border-color: #F5F2F2!important;
  }
}

// intersperse_combined
.theme_color_intersperse_combined{
  .theme_cyan &{
    background: linear-gradient(to bottom, #D7DEE9, #A2ADC1)!important;
  }
  .theme_blue &{
    background: linear-gradient(to bottom, #4770BB, #13387A)!important;
  }
  .theme_red &{
    background: linear-gradient(to bottom, #D4575A, #8E1317)!important;
  }
}

:export {
  // the :export directive is the magic sauce for webpack
  theTheme: $theTheme;
}

```
改变主题：`document.getElementById('app').setAttribute('class', theTheme)`
字体
```scss
/**
全局字体定义
 */

html {
    //font-size: 100px;
    font-size: 625%; //基准 1rem = 100px
}

// 字体大小变量 规定引用font时 内部参数必须用该变量
$font_size_xl: .18rem;
$font_size_lg: .16rem;
$font_size_md: .14rem;
$font_size_sm: .12rem;
// 字体
@mixin font_title {
    font-size: .32rem;
    font-weight: bold;
    color: #17233D;
}


/*
标题
 */

@mixin font_head {
    font-weight: bold;
    color: #464C5B;
}

.font_mainHead {
    font-size: $font_size_xl;
    @include font_head;
}

.font_subHead {
    font-size: $font_size_lg;
    @include font_head;
}

.font_smallHead {
    font-size: $font_size_md;
    @include font_head;
}

//反
@mixin font_inverse_head {
    font-weight: bold;
    color: #FFFFFF;
}

.font_inverse_mainHead {
    font-size: $font_size_xl;
    @include font_inverse_head;
}

.font_inverse_subHead {
    font-size: $font_size_lg;
    @include font_inverse_head;
}

.font_inverse_smallHead {
    font-size: $font_size_md;
    @include font_inverse_head;
}


/*
正文
 */

@mixin font_text_color {
    color: #657180;
}
.font_xlargeText{
    font-size:$font_size_xl;
    @include font_text_color;
}
.font_largeText {
    font-size: $font_size_lg;
    @include font_text_color;
}

.font_text {
    font-size: $font_size_md;
    @include font_text_color;
}

.font_smallText {
    font-size: $font_size_sm;
    @include font_text_color;
}

//反
@mixin font_inverse_text_color {
    color: #FFFFFF;
}

.font_inverse_largeText {
    font-size: $font_size_lg;
    @include font_inverse_text_color;
}

.font_inverse_text {
    font-size: $font_size_md;
    @include font_inverse_text_color;
}

.font_inverse_smallText {
    font-size: $font_size_sm;
    @include font_inverse_text_color;
}


/*
辅助
 */

@mixin font_help_color {
    color: #9EA7B4;
}

.font_help {
    font-size: $font_size_md;
    @include font_help_color;
}

.font_smallHelp {
    font-size: $font_size_sm;
    @include font_help_color;
}


/*
失效
 */

@mixin font_disabled_color {
    color: #C3CBD6;
}

.font_disabled {
    font-size: $font_size_md;
    @include font_disabled_color;
}

.font_smallDisabled {
    font-size: $font_size_sm;
    color: #C3CBD6FF;

}


/*
链接
 */

@mixin font_link_color {
    color: #3399FF;
}

.font_link {
    font-size: $font_size_md;
    @include font_link_color;
}

.font_smallLink {
    font-size: $font_size_sm;
    @include font_link_color;
}


/*
提示
 */

.font_tipError {
    color: #ED4014;
    font-size: $font_size_sm;
}

.font_tipWarning {
    color: #FA9600;
    font-size: $font_size_sm;
}

.font_tipSuccess {
    color: #19BE6B;
    font-size: $font_size_sm;
}
```