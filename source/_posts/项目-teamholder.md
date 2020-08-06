---
title: 项目介绍 协同管理平台
top: false
cover: false
toc: true
mathjax: false
date: 2020-03-22 00:12:51
tags:
password:
summary:
categories: ['项目经验']
---
# 协同管理平台
> [线上Mock版 预览地址](/about/teamholder) *无需输入表单*

## 业务简介
本系统 模仿Tower.im，面向建模开发者团队 实现安排工作任务，管理项目进度等功能。

## 技术栈
- `Vue`前端开发框架.vuex、vue-router
- `ant-design-vue`UI库
- `scss`css预处理
- `eslint`airbnb标准
- `mockJs`拦截Ajax请求 生成随机数据
- `axios`promis库 前后端对接
- `gulp`vinyl-ftp自动部署静态资源到服务器
- `vue-cli`脚手架
- `clipboard/moment/namedavatar`第三方库

## 项目目录
```shell
    |-dist*
    |-public                    # 静态文件
    |-src
        |-main.js               # webpack 的入口文件；
        |-App.vue               # 根组件
        |-app                   # 存放项目业务代码
            |-...
        |-api                   # 存放api请求(文件名与模型名称基本一致,文件名使用小驼峰, 方法名称与后端restful控制器一致)
        |-components            # 存放项目共用的组件,通常是一些可复用的组件会单独存放在该目录
        |-assets                # 存放项目共用的代码以外的资源，如：图片、图标、视频、字体 等
            |-img
        |-styles                # 全局通用样式文件
        |-router                # 存放前端路由相关配置
        |-store                 # vuex的目录
        |-layout                # 整体布局
        |-mock                  # mockJs文件
    |-static                    # 第三方静态引用库
    |-package.json              # npm包配置文件，里面定义了项目的npm脚本，依赖包等信息
    |-README.md                 # 项目说明文件
    |-babel.config.js           # babel 的配置文件
    |-vue.config.js             # vue-cli的额外自定配置文件
```

## 页面组件嵌套逻辑
![思维导图](http://assets.processon.com/chart_image/5e770d14e4b03b99652aaf31.png)

## 重点Mark