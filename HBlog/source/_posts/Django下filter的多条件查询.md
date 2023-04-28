---
title: Django下filter的多条件查询
categories: [学习笔记,Python,Django]
comments: true
toc: false
date: 2019-10-23 11:18:40
---

exact 精确等于      like 'aaa'
iexact 精确等于    忽略大小写 ilike 'aaa'

contains 包含 like '%aaa%'

icontains 包含        忽略大小写 ilike '%aaa%'

gt 大于

gte 大于等于

<!--more-->

lt 小于

lte 小于等于

in 存在于一个list范围内

startswith 以...开头
istartswith 以...开头 忽略大小写

endswith 以...结尾
iendswith 以...结尾，忽略大小写

range 在...范围内
year 日期字段的年份

month 日期字段的月份
day 日期字段的日

isnull=True/False