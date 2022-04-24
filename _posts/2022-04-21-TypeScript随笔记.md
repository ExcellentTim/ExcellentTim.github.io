---
layout: post
title: TypeScript随笔记
renderNumberedHeading: true
grammar_cjkRuby: true
date:   2022-04-21 12:00:00
---

<h3>2022-04-21</h3>

``` reasonml
type Person = {
  name: string,
  age: number
}
type PersonKey = keyof Person  // PersonKey得到的类型为 'name' | 'age'
const getValue = (p:Person, k:keyof Person) => {
  return p[k]  // 如果k不如此定义，则无法以p[k]的代码格式通过编译
}
```
详见博客 https://blog.csdn.net/weixin_39288299/article/details/116330589