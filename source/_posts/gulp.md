---
title: gulp
top: false
cover: false
toc: true
mathjax: true
date: 2019-08-29 10:21:15
tags: ['打包']
password:
summary:
categories:
---

# 静态资源打包工具 Gulp

## 介绍



```js
// 使用gulp自动 将静态文件目录 上传到服务器
const gulp = require('gulp');
const ftp = require( 'vinyl-ftp' );

gulp.task('upload-ftp', function() {
  const conn = ftp.create( {
    host: '192.168.0.11',
    user: 'files',
    password: 'hw123',
    parallel: 10,
    port: 21
  } );
  return gulp.src( 'dist/**/*')
      .pipe(conn.newer('/EBM')) // only upload newer files
      .pipe(conn.dest('/EBM'))
      .pipe(conn.clean('/EBM/**', './dist/'));
});
```
