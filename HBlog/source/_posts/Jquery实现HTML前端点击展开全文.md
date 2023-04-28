---
title: Jquery实现前端点击展开全文
date: 2019-11-23 15:48:38
categories: [学习笔记,Web前端,Jquery]
comments: true
toc: false
---

最近在做动态分享功能，发现微博的样式挺不错的，于是仿微博做了一个。记录一下。

![](https://lizhaofeng.cn/%E5%B1%95%E5%BC%80%E5%85%A8%E6%96%87.jpg)

<!--more-->

![](https://lizhaofeng.cn/%E6%94%B6%E8%B5%B7%E5%85%A8%E6%96%87.jpg)



```html
<!-- HTML结构 -->
<div class="ri_detail">
    <div class="rz_title">
        <span class="rz_name">{{ atc.author.username }}</span>
        <span class="rz_time">{{ atc.created_time|timesince_zh }}</span>
    </div>
    <div class="content-body">
        {% autoescape off %}
        {{ atc.content }}
        {% endautoescape %}
    </div>
    <a class="zhankai">...&nbsp;展开全文</a>
</div>
```

```css
/* CSS样式 */
<style>
    .zhankai{
		padding: 8px 20px;
		color: red;
	}
	.zhankai:hover{
		cursor: pointer;
	}
    
	.content-body{
		overflow: hidden;
		white-space: nowrap;
		text-overflow: ellipsis;
	}
</style>
```

```javascript
//script实现
<script>
	//遍历每个.content-body，如果高度小于300则隐藏按钮“展开全文/收起全文”；否则设置高度为300px。
	$('.content-body').each(function(){
		if($(this).height()<300){
			$(this).parent().children('.zhankai').hide();
		}else{
			$(this).css("height","300px");
			$(this).parent().children('.zhankai').show();
		}
	});
	$('.zhankai').on('click',function () {
		var htm = $(this).html();		//点击按钮时获取按钮文本，判断当前是点击前还是后的状态
		if (htm == "...&nbsp;展开全文") {
			$(this).html('收起全文');
			$(this).parent().children('.content-body').css('height', 'auto');
		} else {
			$(this).html('...&nbsp;展开全文');
			$(this).parent().children('.content-body').css('height', '300px');
		}
	});
</script>
```

