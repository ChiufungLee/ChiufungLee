---
title: 基于Python下Splinter的易班自动抢讲座脚本
date: 2019-10-23 15:06:50
categories: [学习笔记,Python]
---



最近发现身边的朋友一直被**易班抢讲座**所困扰着，每次抢讲座总有一波1s内就填完一大堆信息的“大神”，于是突发奇想想自己写一个自动抢讲座的脚本。最后测试成功了，现在记录一下。

先来看一下最终的效果：

![](https://raw.githubusercontent.com/ChiufungLee/BlogBackup/master/BlogPictures/ybPage.jpg)

<!--more-->



![](https://raw.githubusercontent.com/ChiufungLee/BlogBackup/master/BlogPictures/picShell.jpg)

##### 环境or软件要求

1. Python3.7
2. Splinter
3. Chrome浏览器



> Splinter 是用 Python 开发的一个开源web自动化测试的工具集。 它可以帮你自动化浏览器的行为，比如浏览 URLs 并和页面进行交互。Splinter在Selenium上又封装了一层，使得接口更为简单。
>



###### 1. 安装Python

​	到[Python官网](https://www.python.org/)下载安装Python，并加Python添加到系统环境变量中。添加完之后，打开cmd命令提示符窗口，输入python显示对应的版本号则成功。

![1582014385583](C:\Users\lzfdd\AppData\Roaming\Typora\typora-user-images\1582014385583.png)

###### 2. 安装谷歌浏览器

安装[Google *Chrome* 浏览器](https://www.google.cn/intl/zh-CN/chrome/)

###### 3. 安装Chromedriver

到[ChromeDriver首页-WebDriver for Chrome](https://sites.google.com/a/chromium.org/chromedriver/)下载对应操作系统的最新的chromedriver



将驱动文件复制到谷歌浏览器的安装目录下，并将此目录添加到系统环境变量中

![1571060299376](C:\Users\lzfdd\AppData\Roaming\Typora\typora-user-images\1571060299376.png)

![1571920456899](C:\Users\lzfdd\AppData\Roaming\Typora\typora-user-images\1571920456899.png)

<img src="C:\Users\lzfdd\AppData\Roaming\Typora\typora-user-images\1571920481680.png" alt="1571920481680" style="zoom:200%;" />



###### 4. 安装Splinter模块

Splinter模块是python egg，下载很简单，直接`pip install splinter`就好了。由于基于selenium，所以，FireFox和Chrome的驱动，都依赖于`pip install selenium`，不过好像执行`pip install splinter`之后默认就已经安装了，没有的话再安装一下。

打开命令提示符窗口，输入

```shell
pip install splinter

#如安装失败请加镜像地址再安装
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple splinter
```

关于Splinter的使用说明，可以查看官方文档。-> [Splinter — Splinter 0.7.6 中文文档](https://splinter-docs-zh-cn.readthedocs.io/)



##### 脚本说明

###### 1. 个人信息

```python
class lecture(object):
	driver_name = 'chrome'
	browser = Browser(driver_name)
	# 易班账号
	username = '**********'
	password = '**********'
	# 讲座信息
	# go=后面加报名链接
	login_url = 'http://www.yiban.cn/login?go=https://q.yiban.cn/app/index/appid/***'
	# 报名链接
	baoming_url = 'https://q.yiban.cn/app/index/appid/***'
	# 报名时间
	apply_time = '2019-10-25 19:00:00'
	# 个人信息
	department = '学院'
	myclass = '班级'
	stu_number = '学号'
	mobile_num = '手机号'
	profession = '专业'
```



###### 2. **用户登录**

这里本来打算用全自动的，但是后来发现自动识别的验证码错误率较高，以我目前的能力还没有办法解决，所以这里的验证码需要自己手动输入。最终抢讲座的时候脚本需要提前打开登录系统，到时间点会自动进行操作。

查找元素

- visit(url)

  通过visit()打开url

- find_by_xpath(' ')

  > Splinter提供了6种方法用于查找在页面中的元素，并用于每个选择器类型： `css`, `xpath`, `tag`, `name`, `id`, `value`, `text`. 

  这里使用xpath选择器定位账号和密码输入框，使用`click_link_by_id()`点击按钮

- fill(' ') 

  填充数据

```python
def login(self):
		# browser = Browser(self.driver_name)
		self.browser.visit(self.login_url)
		self.browser.find_by_xpath('//*[@id="account-txt"]').fill(self.username)
		self.browser.find_by_xpath('//*[@id="password-txt"]').fill(self.password)
		self.browser.click_link_by_id('login-btn')
		print(u"请自行输入验证码登录...\n")
        
		while True:
			if self.browser.url != self.baoming_url:
				time.sleep(1)
			else:
				print('登录成功！\n')
				break
```

![1582015384626](C:\Users\lzfdd\AppData\Roaming\Typora\typora-user-images\1582015384626.png)

###### 3. 自动报名

登录易班后，获取当前时间和报名开始时间，循环判断是否到报名的时间点。到点则退出循环，刷新页面，自动点击并填充数据进行报名。

```python
def getLecture(self):
	...

    target_time = datetime.datetime.strptime(self.apply_time, '%Y-%m-%d %H:%M:%S')
    start_time = 10
    print("离开始时间还有：\n")
    while start_time != 0:
        now_time = datetime.datetime.now()
        start_time = (target_time - now_time).seconds
        print(start_time)
        time.sleep(1)

    self.browser.reload()
    print(datetime.datetime.now())
    try:
        self.browser.click_link_by_text('开始报名')
        self.browser.find_by_xpath('//*[@id="enroll-view"]/div[2]/ul/li[1]/div[2]/textarea').fill(self.department)
        link_text1 = self.browser.find_by_xpath('//*[@id="enroll-view"]/div[2]/ul/li[1]/div[4]/a').text
        self.browser.click_link_by_text(link_text1)

        self.browser.find_by_xpath('//*[@id="enroll-view"]/div[2]/ul/li[2]/div[2]/textarea').fill(self.myclass)
        link_2 = self.browser.find_by_xpath('//*[@id="enroll-view"]/div[2]/ul/li[2]')
        link_2.find_by_tag('a').click()

        self.browser.find_by_xpath('//*[@id="enroll-view"]/div[2]/ul/li[3]/div[2]/textarea').fill(self.stu_number)
        link_3 = self.browser.find_by_xpath('//*[@id="enroll-view"]/div[2]/ul/li[3]')
        link_3.find_by_tag('a').click()

        self.browser.find_by_xpath('//*[@id="enroll-view"]/div[2]/ul/li[4]/div[2]/textarea').fill(self.mobile_num)
        link_4 = self.browser.find_by_xpath('//*[@id="enroll-view"]/div[2]/ul/li[4]')
        link_4.find_by_tag('a').click()

        self.browser.find_by_xpath('//*[@id="enroll-view"]/div[2]/ul/li[5]/div[2]/textarea').fill(self.profession)
        link_text5 = self.browser.find_by_xpath('//*[@id="enroll-view"]/div[2]/ul/li[5]/div[4]/a').text
        self.browser.click_link_by_text(link_text5)

        print('报名成功！')
     	...
```



###### 4. 运行脚本

讲座开始前直接双击脚本文件运行，<u>**运行前请注意更新相关的时间等信息**</u>。



完整的代码已上传至`GitHub`上：https://raw.githubusercontent.com/ChiufungLee/BlogBackup/master/dataBackup/



###### 5. 注意事项

- 每次讲座脚本修改下面三个位置，报名链接和时间

- 验证码需要手动输入，所以请提前1分钟运行脚本登录易班

- 运行脚本时浏览器窗口和命令窗口最好不要最小化




![1571986721699](C:\Users\lzfdd\AppData\Roaming\Typora\typora-user-images\1571986721699.png)



<u>能力有限，欢迎大家提出建议 ~ _  ~</u>