---
title: Python读写Excel文件
categories: [学习笔记,Python]
comments: true
toc: false
date: 2020-05-07 13:50:48
---

##### 用xlsxwriter包写入Excel文件

xlsxwriter这个模块，它生成的文件后缀名为.xlsx，最大能够支持1048576行数据，16384列数据

1.1用法

<!-- more-->

```python
#引用包
import xlsxwriter
```



1.2、创建工作簿

```python
workbook = xlsxwriter.Workbook('demo1.xlsx')
#创建一个excel文件
```



1.3、创建sheet

```python
worksheet = workbook.add_worksheet(u'sheet1')
#在文件中创建一个名为TEST的sheet,不加名字默认为sheet1
```



1.4、设置每个单元格里面的值

```python
 worksheet.write(3,0,35.5)
    #第4行的第1列设置值为35.5
```



1.5、关闭工作簿

```python
workbook.close()
```



1.6、源码示例

```python
import xlsxwriter
#写Excel

def write_excel(): 
	workbook = xlsxwriter.Workbook('chat.xlsx')#创建一个excel文件
	worksheet = workbook.add_worksheet(u'sheet1')#在文件中创建一个名为TEST的sheet,不加名字默认为sheet1

	worksheet.set_column('A:A',20)#设置第一列宽度为20像素
    bold= workbook.add_format({'bold':True})#设置一个加粗的格式对象
    worksheet.write('A1','HELLO')#在A1单元格写上HELLO
    worksheet.write('A2','WORLD',bold)#在A2上写上WORLD,并且设置为加粗
    worksheet.write('B2',U'中文测试',bold)#在B2上写上中文加粗

    worksheet.write(2,0,32)#使用行列的方式写上数字32,35,5
    worksheet.write(3,0,35.5)#使用行列的时候第一行起始为0,所以2,0代表着第三行的第一列,等价于A4
    worksheet.write(4,0,'=SUM(A3:A4)')#写上excel公式
    workbook.close()

if __name__ == '__main__':
    # 写入Excel
    write_excel();
    print ('写入成功')
```

