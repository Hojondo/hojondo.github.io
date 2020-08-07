---
title: Vue-iview
top: false
cover: false
toc: true
mathjax: false
date: 2019-09-19 19:40:56
tags: ["vue", "UI库"]
password:
summary:
categories: ["笔记"]
---

# iview

## 通用类别

`../iview/src/styles/custom.less` [定制主题 颜色/字体](https://www.iviewui.com/docs/guide/theme)

_属性值都可绑定变量 :loading="istrue"_

### `Button`

> `Button` 的 属性 props
>
> 1. `[type=""]`
>
>    `default` `primary` `dashed` `text` `info` `success` `warning` `error`
>
> 2. `[ghost]` _幽灵按钮将其他按钮的内容反色，背景变为透明，常用在有色背景上_
>
> 3. `[shape=""]`
>
>    `circle` `circle-outline`
>
> 4. `[size=""]`
>
>    `small` `default` `large`
>
> 5. `[long]`
>
> 6. `[disabled]`
>
> 7. `[icon=""]` _使用`Button`的 icon 属性，图标位置将在最左边，如果需要自定义位置，需使用`Icon`组件_
>
> 8. `[loading]`
>
> 9. `[to=""], [replace], [target=""]` _实现点击按钮直接跳转 a_
>
> `ButtonGroup[vertical]>Button`

### `Icon`

> 属性 props
>
> 1. `[type=""]` _[所有图标](https://www.iviewui.com/components/icon)_
> 2. `[size=""]` _Number | String 图标的大小，单位是 px_
> 3. `[color=""]`
> 4. `[custom=""]` 自定义图标。 Icon 支持使用第三方自定义图标，你可以引入任意的字体文件库来使用
> 5. todo

## 布局

### Grid

### Layout

### Card

### Collapse

### Split

### Divider

### Cell

## 导航

### Menu

### Tabs

### Dropdown

### Page

### BreadCrumb

### Badge

### Anchor

### LoadingBar

## 表单

### Input

### Radio

### CheckBox

### Switch

### Table

### Select

### AutoComplete

### Slider

### DatePicker

### TimePicker

### Cascader 级联选择

### Transfer 穿梭框

### InputNumber 数字输入框

### Rate

### Upload

### ColorPicker

### Form

## 视图

### Alert

### Message 全局提示

### Notice 通知提醒

### Modal

### Drawer 抽屉

### Tree 树形列表

### ToolTip

### PopTip

### Progress

### Avatar

### Tag

### Carousel

### TimeLine

### Time 相对时间

## 其他

### Circle 进度环

### Affix 图钉

### BackTop

### Spin

### Scroll
