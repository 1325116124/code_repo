# HTML

## 字符集问题

```html
<html>
	<head>
		<meta charset="GBK">
		<title>�ٶ�һ��,���֪��123</title>
	</head>
	<body>
		�ܿ�������?
</body>
</html>
```

```html
<html>
	<head>
		<meta charset="UTF-8" />
		<title>百度一下,你就知道123</title>
	</head>
	<body>
		能看到我吗?
	</body>
</html>
```



##  DTD文档声明

主要作用是告诉浏览器的解析器使用哪种HTML规范或者XHTML规范来解析页面。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>百度一下,你就知道123</title>
	</head>
	<body>
		能看到我吗?
	</body>
</html>
```

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>百度一下,你就知道123</title>
		<style type="text/css">
			p {
				font-size: 50px;
				color: red;
				font-family: "微软雅黑";
			}
		</style>
	</head>
	<body>
		<font size="7" color="red" face="微软雅黑">能看到我吗?</font>
		<u>我是文字</u>
		<p>我是PPPP</p>
	</body>
</html>
```



## H标签-hr标签-p标签

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>H系列标签和P标签和Hr标签</title>
</head>
<body>
    <h1>我是标题1</h1>
    <h2>我是标题2</h2>
    <h3>我是标题3</h3>
    <h4>我是标题4</h4>
    <h5>我是标题5</h5>
    <h6>我是标题6</h6>
    <h7>我是标题7</h7>
    我是普通文本

    <hr />
    <hr>

    <p>我是一段文本</p>
    <p>我是一段文本</p>
    我是一段普通文本
    我是一段普通文本

</body>
</html>
```



## img标签

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>img标签</title>
</head>
<body>
<!--
1.img标签中的img其实是英文image的缩写
所以img标签的作用, 就是告诉浏览器我们需要显示一张图片

2.img标签格式: <img src="">
其实img标签中的src是英文source的缩写
所以img标签中的src就是用来告诉img标签, 需要显示的图片名称

3.注意点
- 和H系列标签/p标签还有Hr标签不一样, img标签不会独占一行
- 如果我们手动指定了img标签显示的图片的宽度和高度, 有可能会导致图片变形, 那么如果又想指定宽度和高度, 又不想让图片变形. 我们可以只指定宽度和高度其中的一个值即可
- 只要指定了高度, 系统会自动根据高度计算出宽度, 只要指定了宽度, 系统会自动根据宽度计算出高度, 并且都是等比拉伸的, 也就是说不会变形

4.img中的其它属性
width: 宽度
height: 高度
所以在img标签中width/height这两个属性的作用, 就是用来告诉img标签将来需要显示的图片有多宽有多高
如果img标签没有指定需要显示的图片的宽高, 那么系统会按照图片默认的宽高来显示
如果img标签指定的宽高, 那么系统会按照指定的宽高来显示

title: 用于告诉浏览器, 当鼠标悬停在图片上时, 需要弹出的描述框中显示什么内容

alt其实是英文alternate的缩写, 它的作用就是用于告诉浏览器, 当需要显示的图片找不到时显示什么内容

-->

<img src="images/QRCode.jpg">

<img src="images/QRCode.jpg" width="300" height="478">

<img src="images/QRCode.jpg" width="100" height="478">

<img src="images/QRCode.jpg" height="178">
<img src="images/QRCode.jpg" width="100">

<img src="images/QRCode.jpg" width="100" title="这个是江哥的微信二维码图片">

<img src="images/QRCode1.jpg" width="100" alt="对不起, 你需要查看的图片不见了">
</body>
</html>
```



## br标签

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>br标签</title>
</head>
<body>
<!--
br标签, 如何在HTML中换行? 可以使用br标签
1. br标签作用: 换行
2. br标签格式: <br>
3. br标签的注意点:
3.1多个br标签可以连续使用, 使用了多少个br标签就会换多少行
3.2由于HTML的作用就是用来给文本添加语义, 而br标签的语义是不另起一个段落换行, 而在企业开发中一般情况下需要换行都是因为需要另起一个段落, 所以在企业开发中很少使用br标签
-->

<!--只设置了img标签的src属性<br><br><br>-->
<!--<img src="QRCode.jpg"><br>-->
<!--设置了img标签的src和width以及height属性<br>-->
<!--<img src="QRCode.jpg" width="300" height="478"><br>-->
<!--设置了img标签的src和width以及height, 但是宽高不是等比拉伸的<br>-->
<!--<img src="QRCode.jpg" width="100" height="478"><br>-->
<!--设置了img标签的src和height属性<br>-->
<!--<img src="QRCode.jpg" height="178"><br>-->
<!--设置了img标签的src和width属性<br>-->
<!--<img src="QRCode.jpg" width="100"><br>-->
<!--设置了img标签的src和width以及title属性<br>-->
<!--<img src="QRCode.jpg" width="100" title="这个是江哥的微信二维码图片"><br>-->
<!--设置了img标签的src和width以及alt属性<br>-->
<!--<img src="QRCode1.jpg" width="100" alt="对不起, 你需要查看的图片不见了"><br>-->


    <p>只设置了img标签的src属性</p>
    <p><img src="images/QRCode.jpg"></p>
    <p>设置了img标签的src和width以及height属性</p>
    <p><img src="images/QRCode.jpg" width="300" height="478"></p>
    <p>设置了img标签的src和width以及height, 但是宽高不是等比拉伸的</p>
    <p><img src="images/QRCode.jpg" width="100" height="478"></p>
    <p>设置了img标签的src和height属性</p>
    <p><img src="images/QRCode.jpg" height="178"></p>
    <p>设置了img标签的src和width属性</p>
    <p><img src="images/QRCode.jpg" width="100"></p>
    <p>设置了img标签的src和width以及title属性</p>
    <p><img src="images/QRCode.jpg" width="100" title="这个是江哥的微信二维码图片"></p>
    <p>设置了img标签的src和width以及alt属性</p>
    <p><img src="images/QRCode1.jpg" width="100" alt="对不起, 你需要查看的图片不见了"></p>

</body>
</html>
```



## 路径问题

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>路径问题</title>
</head>
<body>

<!--
路径问题
其实想给src属性赋值有两种方式
1.通过相对路径赋值(掌握)
相对路径就是每次都从.html文件所在的文件夹开始查找, 我们称之为相对路径

1.1同级
同级就是"图片"和".html文件"存储在同一个文件夹中
格式: src="QRCode.jpg"
含义: 在.html文件所在的文件夹中查找名称叫做QRCode.jpg的图片

1.2下级
下级就是"存储图片的文件夹"和".html文件"在同一个文件夹中
格式: src="images/QRCode.jpg"
含义: 在.html文件所在的文件夹中找到名称叫做images的文件夹
然后再在images文件夹中找到名称叫做QRCode.jpg的图片

1.3上级
上级就是"存储图片的位置"和存"储代码的文件夹"在同一个文件夹中
格式: src="../QRCode.jpg"
含义：在.html文件所在的文件夹中找到这个文件夹的上一级文件夹, 然后再在上一级文件夹中找到名称叫做QRCode.jpg
其中../代表找到当前文件夹的上一级文件夹

2.通过绝对路径赋值(了解)
绝对路径就是每次都从指定的盘符开始查找

格式: src="C:\Users\Jonathan_Lee\Desktop\HTML基础\QRCode.jpg"
含义: 在C盘下找到Users文件夹, 然后在Users文件夹中找到Jonathan_Lee文件夹, 然后在Jonathan_Lee文件夹中找到Desktop文件夹, 然后在Desktop文件夹中找到HTML基础文件夹, 然后在HTML基础文件夹中找到名称叫做QRCode.jpg的图片

注意:
1.路径中不要出现中文, 否则可能出现未知问题
2.如果是通过"相对路径"来指定图片, 不能跨盘符
2.1例如.html文件在C盘, 那么不能去查找D盘图片
-->
<img src="QRCode.jpg">
<img src="images/QRCode.jpg">
<img src="../QRCode.jpg">
<img src="C:\Users\Jonathan_Lee\Desktop\HTML基础\QRCode.jpg" >
</body>
</html>
```



## a标签

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>10-a标签基本使用</title>
</head>
<body>
<!--
什么是a标签?
a标签的作用: 就是用于控制页面与页面之间跳转的
a标签的格式: <a href="指定需要跳转的目标界面">需要展现给用户查看的内容</a>

a标签中有一个叫做target属性, 这个属性的作用就是专门用于控制如何跳转
_self: 用于在当前选项卡中跳转, 也就是不新建界面跳转. 默认就是_self
_blank: 用于在新的选项卡中跳转, 也就是新建界面跳转

a标签中还有一个属性, 叫做title. a标签中的title和img标签中的title一样, 都是用来控制鼠标悬停时显示的提示文本的

注意点:
1.a标签不仅可以让文字可以点击, 还可以让图片也能够被点击
2.一个a标签必须有一个href属性, 否则a标签不知道要跳转到什么地方
3.如果通过a标签的href属性指定一个URL地址, 那么必须在地址前面加上http://或https://
4.a标签的href属性除了可以指定一个网络地址以外, 还可以指定一个本地地址
-->

<a href="https://www.nuomi.com/?cid=002540">糯米</a>

<a href="https://binjiejw.tmall.com/category-1080022346.htm?search=y&parentCatId=1080019249&parentCatName=%A1%A4?catName=??%A1%A4???????spm=a220o.1000855.w5001-10945243352.20.Fq84rf&scene=taobao_shop&ali_trackid=30_e27f91873d121ea29b062a117ba2991b&spm=a21bo.50862.201862-4.1.9GysHu#bd">
    <img src="images/QRCode.jpg" width="150">
</a>

<a href="08-br标签.html">08-br标签.html</a>
<a href="test/demo/10-路径练习.html">10-路径练习.html</a>

<a href="https://www.nuomi.com/?cid=002540" target="_self">糯米</a>
<a href="https://www.nuomi.com/?cid=002540" target="_blank">糯米</a>


<a href="https://www.nuomi.com/?cid=002540" target="_blank" title="江哥提示: 会跳转到百度糯米的首页">糯米</a>

</body>
</html>
```



## base标签

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>11-base标签</title>
    <base target="_blank">
</head>
<body>
<!--
base标签就是专门用来统一的指定当前网页中所有的超链接(a标签)需要如何打开

注意点:
1.base标签必须写在head标签的开始标签和结束标签之间
2.如果既在base中指定了target又在a标签中指定了target,那么浏览器会按照a标签中指定的来执行
-->
<a href="https://www.nuomi.com/?cid=002540" target="_self">糯米</a>
<a href="http://news.baidu.com/">新闻</a>
<a href="https://www.hao123.com/">hao123</a>
<a href="http://map.baidu.com/">地图</a>
<a href="http://v.baidu.com/">视频</a>
<a href="http://tieba.baidu.com/">贴吧</a>

</body>
</html>
```



## 假链接

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>12-假链接</title>
</head>
<body>
<!--
1.什么是假链接?
就是点击之后不会跳转的链接我们称之为假链接

2.假链接存在的意义:
在企业开发前期, 其它界面都没有写出来, 那么我们就不知道应该跳转到什么地方, 所以就只能使用假链接来代替. 当项目后期其它界面都已经完成时再将假链接体会为真链接

3.假链接的格式:
1.#
2.javascript:

4.两者之间的区别:
#的假链接会自动回到网页的顶部, 而javascript:的假链接不会自动回到网页顶部
-->
<h1>我是顶部</h1>
<a href="#">点我丫1</a>
<a href="javascript:">点我丫2</a>
</body>
</html>
```



## 锚点

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>13-锚点</title>
</head>
<body>

<!--
1.要想通过a标签跳转到指定的位置, 那么必须告诉a标签一个独一无二的身份证号码, 这样a标签才能在当前界面中找到需要跳转到的目标位置

2.如果和HTML中的标签绑定一个独一无二的身份证号码呢?
在HTML中, 每一个标签都有一个名称叫做id的属性, 这个属性就是用来给标签指定一个独一无二的身份证号码的

3.所以要想实现通过a标签跳转到指定的位置分为两步
3.1给目标位置的标签添加一个id属性, 然后指定一个独一无二的值
3.2告诉a标签你需要跳转到的目标标签对应的独一无二的身份证号码是多少
格式:
<a href="#center">跳转到中部</a>
<h2 id="center">我是中部</h2>

注意点:
1.通过我们的a标签跳转到指定的位置, 是没有过度动画的, 是直接一下子就跳转到了指定位置
2.a标签除了可以跳转到当前界面的指定位置以外, 还可以在跳转到其它界面的时候直接跳转到其它界面的指定位置
格式:
<a href="13-锚点测试界面.html#bottom" target="_blank">跳转到锚点测试界面</a>
<h2 id="bottom">我是锚点测试界面33333</h2>
-->
<a href="13-锚点测试界面.html#bottom" target="_blank">跳转到锚点测试界面</a>
<h2>我是顶部</h2>
<a href="#center">跳转到中部</a>
<h2 id="center">我是中部</h2>
<h2>我是底部</h2>
</body>
</html>
```



## 无序列表

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>14-无序列表</title>
    <style type="text/css">
        ul {
            list-style: none;
        }
        li {
            float: left;
            background-color: yellow;
            width: 150px;
            height:50px;
            text-align: center;
            line-height: 50px;
        }
    </style>
</head>
<body>
<!--
1.什么是列表标签?
列表标签的作用: 给一堆数据添加列表语义, 也就是告诉搜索引擎告诉浏览器这一堆数据是一个整体

2.HTML中列表标签的分类
2.1无序列表(最多)(unordered list)
2.2有序列表(最少)(ordered list)
2.3定义列表(其次)(definition list)

3.无序列表作用:
给一堆数据添加列表语义, 并且这一堆数据中所有的数据都没有先后之分

什么叫有先后之分?
例如: 排行榜
什么叫没有先后之分?
例如: 中国有哪些城市

4.无序列表格式:
<ul>
    <li>需要显示的条目内容</li>
</ul>
li其实是英文list item的缩写
list是列表的意思
item是条目的意思
所以结合起来就是 列表条目的意思

5.注意点:
1.一定要记住ul标签是用来给一堆数据添加列表语义的, 而不是用来给他们添加小圆点的
2. ul标签和li标签是一个整体, 是一个组合. 所以一般情况下ul标签和li标签都是一起出现, 不会单个出现. 也就是说不会单独使用一个ul标签或者单独使用一个li 标签, 都是结合在一起使用
3.由于ul标签和li标签是一个组合, 所以ul标签中不推荐包含其它标签, 也就是说以后你在ul标签中只会看到li标签

无序列表应用场景:
1.新闻列表
2.商品列表
3.导航条

-->
    
<!--
无序列表中的li标签中除了可以添加其它标签来丰富我们的界面以外, 还可以添加ul标签来丰富我的界面
也就是说ul中有li, li中又可以有ul

在webstorm中如何快速编写一个ul的格式
ul>li
含义: 生成一对ul标签, 然后在这对ul标签中再生成一对li标签

ul>li*3
含义: 生成一对ul标签, 然后在这对ul标签中再生成3对li标签
-->

<h2>中国的城市有哪些</h2>
<ul>
    <li>广州</li>
    <li>北京</li>
    <li>上海</li>
    <li>武汉</li>
</ul>
</body>
</html>
```



## 有序列表

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>18-有序列表</title>
</head>
<body>
<!--
什么是有序列表?
有序列表的作用: 给一堆数据添加列表语义, 并且这一堆数据中所有的数据都有先后之分

有序列表格式:
<ol>
    <li></li>
</ol>

其它用法和ul都差不多, 也就是应用场景/注意点都和ul差不多

-->

<ol>
    <li>演员</li>
    <li>小丑</li>
    <li>女人不应该让男人太累</li>
    <li>男人不应该让女人流泪</li>
</ol>
</body>
</html>
```



## 定义列表

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>19-定义列表</title>
</head>
<body>
<!--
1.定义列表的作用:
1.1给一堆数据添加列表语义
1.2先通过dt标签定义列表中的所有标题, 然后再通过dd标签给每个标题添加描述信息

2.定义列表的格式:
<dl>
    <dt></dt>
    <dd></dd>
    <dt></dt>
    <dd></dd>
</dl>
其实dt和dd都是英文的缩写
dt是英文definition title的缩写, 所以dt的含义就是用来定义列表中的标题
dd是英文definition description的缩写, 所以dd的含义就是用来定义标题对应的描述的

3.定义列表的应用场景
1.做网站尾部的相关信息
2.做图文混排

4.定义列表的注意点
4.1和ul/ol一样, dl和dt/dd是一个整体, 所以他们一般情况下不会单独出现, 都是一起出现
4.2和ul/ol一样, 由于dl和dt/dd是一个组合标签, 所以dl中建议只放dt和dd标签
4.3一个dt可以没有对应的dd,也可以有多个对应的dd, 但是无论有或者没有dd都不推荐使用.
推荐使用一个dt对应一个dd
4.4和li标签一样, 当需要丰富界面时, 我们可以在dt和dd标签中继续添加其它标签
-->

<dl>
    <dt>北京</dt>
    <dd>中国的首都</dd>
    <dd>雾都, 雾霾比较严重</dd>
    <dt>上海</dt>
    <dd>富人集中地</dd>
</dl>
</body>
</html>
```



## 定义列表练习

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>20-定义列表练习(了解)</title>
    <style type="text/css">
        *{ margin:0; padding:0; border: 0;}
        dl{ width:400px; height:200px; background:#ccc; margin:100px auto;}
        dl dt{ width:150px; height:200px; float:left;}
        dl dd{ width:250px; height:200px; float:right;}
        dl dd h2{ line-height:50px; color:orange; margin:0 10px; font-size:16px;}
        dl dd p{ line-height:19px; color:#000; font-size:12px; margin:0 10px;}
    </style>
</head>
<body>

<!--
dl>dt+dd

<dl>
    <dt></dt>
    <dd></dd>
</dl>
-->
<dl>
    <dt>
        <img src="images/NJ.jpg" width="150px" height="200px">
    </dt>
    <dd>
        <h2>李南江（NJ）</h2>
        <p>多年iOS、Android、HTML开发及教学经验, 对NativeApp、HybridApp、WebApp开发有独到见解和深入的研究, 除此之外李老师还精通JavaScript、AngularJS、 NodeJS 、Ajax、jQuery、Cordova、React Native等多种Web前端技术及Java、PHP等服务端技术</p>
    </dd>
</dl>
</body>
</html>
```



## 表格标签

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>21-表格标签基本使用</title>
</head>
<body>
<!--
其实在过去表格标签用的非常非常的多, 绝大多数的网站都是使用表格标签来制作的, 也就是说表格标签是一个时代的代表

1.什么是表格标签?
表格标签作用: 用来给一堆数据添加表格语义
其实表格是一种数据的展现形式, 当数据量非常大的时候, 表格这种展现形式被认为是最为清晰的一种展现形式

2.表格标签格式:
<table>
    <tr>
        <td>需要显示的内容</td>
    </tr>
</table>
其实表格标签中的table代表整个表格, 也就是一堆table标签就是一个表格
其实表格标签中的tr标签代表整个表格中的一行数据, 也就是说一对tr标签就是表格中的一行
其实表格标签中的td标签代表表格中一行中的一个单元格, 也就是说一对td标签就是一行中的一个单元格

3.注意点
3.1表格标签有一个边框属性, 这个属性决定了边框的宽度. 默认情况下这个属性的值是0, 所以看不到边框
3.2表格标签和列表标签一样, 它是一个组合标签, 所以table/tr/td要么一起出现, 要么一起不出现, 不会单个出现
-->

<!--要求写一个表格, 这个表格中有2行3列-->
<table border="1">
    <tr>
        <td>1.1</td>
        <td>1.2</td>
        <td>1.3</td>
    </tr>
    <tr>
        <td>2.1</td>
        <td>2.2</td>
        <td>2.3</td>
    </tr>
</table>
</body>
</html>
```



## 表格标签的属性

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>22-表格标签的属性</title>
</head>
<body>
<!--
1.宽度和高度的属性
可以给table标签和td标签使用
1.1表格的宽度和高度默认是按照内容的尺寸来调整的, 也可以通过给table标签设置width/height属性的方式来手动指定表格的宽度和高度
1.2如果给td标签设置widht/height属性, 会修改当前单元格的宽度和高度, 不会影响整个表格的宽度和高度

2.水平对齐和垂直对齐的属性
其中水平对齐可以给table标签和tr标签和td标签使用
垂直对齐只能给tr标签和td标签使用
2.1给table标签设置align属性, 可以控制表格在水平方向的对齐方式
2.2给tr标签设置align属性, 可以控制当前行中所有单元格内容的水平方向的对齐方式
2.3给td标签设置align属性, 可以控制当前单元格中内容在说方向的对齐方式
注意点: 如果td中设置了align属性, tr中也设置了align属性, 那么单元格中的内容会按照td中设置的来对齐

2.4给tr标签设置valign属性, 可以控制当前行中所有单元格中的内容, 在垂直方向的对齐方式
2.5给td标签设置valign属性, 可以控制当前单元格中的内容在垂直方向的对齐方式
注意点:  如果td中设置了valign属性, tr中也设置了valign属性, 那么单元格中的内容会按照td中设置的来对齐

3.外边距和内边距的属性
只能给table标签使用
3.1.外边距就是单元格和单元格之间的距离, 我们称之为外边距
默认情况下单元格和单元格之间的外边距的距离是2px

3.2 内边距就是单元格的边框和文字之间的间隙, 我们称之为内边距
默认情况下内边距是1


注意: 以上讲解的内容仅仅作为了解, 以后在企业开发中所有控制样式的都是通过CSS
-->

<!--编写一个2行2列的表格-->
<table border="1" width="500px" height="300px" align="right" cellspacing="12px" cellpadding="1">
    <tr valign="bottom">
        <td width="150px" height="50px">1.1</td>
        <td valign="top">1.2</td>
    </tr>
    <tr align="center">
        <td align="right">2.1</td>
        <td>2.2</td>
    </tr>
</table>
</body>
</html>
```



## 细线表格

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>23-细线表格</title>
</head>
<body>

<!--
在表格标签中想通过指定外边距为0来实现细线表格是不靠谱的, 其实它是将2条线合并为了一条线, 所以看上去很不舒服

细线表格的制作方式:
1.给table标签设置bgcolor
2.给tr标签设置bgcolor
3.给table标签设置cellspacing = "1px"

注意点:
table标签和tr标签以及td标签都支持bgcolor属性
但是以上内容仅仅作为了解, 因为样式以后都是通过css来控制
-->
<table bgcolor="black" cellspacing="1px">
    <tr bgcolor="white">
        <td bgcolor="red">1.1</td>
        <td>1.2</td>
    </tr>
    <tr bgcolor="white">
        <td>2.1</td>
        <td>2.2</td>
    </tr>
</table>
</body>
</html>
```



## 表格标签中的其他标签

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>24-表格标签的其它标签</title>
</head>
<body>
<!--
1.表格标题
在表格标签中提供了一个标签专门用来设置表格的标题, 这个标签叫做caption. 只要将标题写在caption标签中, 那么标题就会自动相对于表格的宽度居中

2.caption标签的注意点:
2.1caption一定要写在table标签中, 否则无效
2.2caption一定要紧跟在table标签后面

3.标题单元格标签
3.1.在表格标签中提供了一个标签专门用来存储每一列的标题, 这个标签叫做th标签, 只要将当前列的标题存储在这个标签中就会自动居中+加粗文字

3.2.到此为止我们就发现, 其实表格中有两种单元格, 一种是td, 一种是th. td是专门用来存储数据的, th是专门用来存储当前列的标题的
-->

<table bgcolor="black" cellspacing="1px" width="800px" align="center">
    <caption>
        <h2>今日小说排行榜</h2>
    </caption>
    <tr bgcolor="#a9a9a9">
        <th>排名</th>
        <th>关键词</th>
        <th>趋势</th>
        <th>今日搜索</th>
        <th>最近七日</th>
        <th>相关链接</th>
    </tr>
    <tr bgcolor="white" align="center">
        <td>1</td>
        <td align="left">暴走大事件</td>
        <td>
            <img src="images/up.jpg">
        </td>
        <td>623557</td>
        <td>4088311</td>
        <td>
            <a href="#">贴吧</a>
            <a href="#">图片</a>
            <a href="#">百科</a>
        </td>
    </tr>
    <tr bgcolor="white" align="center">
        <td>1</td>
        <td align="left">暴走大事件</td>
        <td>
            <img src="images/up.jpg">
        </td>
        <td>623557</td>
        <td>4088311</td>
        <td>
            <a href="#">贴吧</a>
            <a href="#">图片</a>
            <a href="#">百科</a>
        </td>
    </tr>
    <tr bgcolor="white" align="center">
        <td>1</td>
        <td align="left">暴走大事件</td>
        <td>
            <img src="images/down.jpg">
        </td>
        <td>623557</td>
        <td>4088311</td>
        <td>
            <a href="#">贴吧</a>
            <a href="#">图片</a>
            <a href="#">百科</a>
        </td>
    </tr>
    <tr bgcolor="white" align="center">
        <td>1</td>
        <td align="left">暴走大事件</td>
        <td>
            <img src="images/down.jpg">
        </td>
        <td>623557</td>
        <td>4088311</td>
        <td>
            <a href="#">贴吧</a>
            <a href="#">图片</a>
            <a href="#">百科</a>
        </td>
    </tr>
    <tr bgcolor="white" align="center">
        <td>1</td>
        <td align="left">暴走大事件</td>
        <td>
            <img src="images/up.jpg">
        </td>
        <td>623557</td>
        <td>4088311</td>
        <td>
            <a href="#">贴吧</a>
            <a href="#">图片</a>
            <a href="#">百科</a>
        </td>
    </tr>
    <tr bgcolor="white" align="center">
        <td>1</td>
        <td align="left">暴走大事件</td>
        <td>
            <img src="images/up.jpg">
        </td>
        <td>623557</td>
        <td>4088311</td>
        <td>
            <a href="#">贴吧</a>
            <a href="#">图片</a>
            <a href="#">百科</a>
        </td>
    </tr>
    <tr bgcolor="white" align="center">
        <td>1</td>
        <td align="left">暴走大事件</td>
        <td>
            <img src="images/up.jpg">
        </td>
        <td>623557</td>
        <td>4088311</td>
        <td>
            <a href="#">贴吧</a>
            <a href="#">图片</a>
            <a href="#">百科</a>
        </td>
    </tr>
</table>
</body>
</html>
```



## 表格的结构

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>25-表格的结构</title>
</head>
<body>
<!--
1.由于表格中存储的数据比较复杂, 为了方便管理和阅读以及提升语义, 我们可以对表格中存储的数据进行分类
表格中存储的数据可以分为4类
1.1表格的标题
1.2.表格的表头信息
1.3.表格的主体信息
1.4.表格的页尾信息

2.表格的完整结构
<table>
    <caption>表格的标题</caption>
    <thead>
        <tr>
            <th>每一列的标题</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>数据</td>
        </tr>
    </tbody>
    <tfoot>
        <tr>
            <td>数据</td>
        </tr>
    </tfoot>
</table>

caption作用: 指定表格的标题
thead作用: 指定表格的表头信息
tbody作用: 指定表格的主体信息
tfoot作用: 指定表格的附加信息

3.注意点:
3.1.如果我们没有编写tbody, 系统会系统给我们添加tbody
3.2.如果指定了thead和tfoot, 那么在修改整个表格的高度时, thead和tfoot有自己默认的高度, 不会随着表格的高度变化而变化
-->

<table border="1px" width="500px" height="500px">
    <caption>学生信息</caption>
    <thead>
        <tr>
            <th>姓名</th>
            <th>年龄</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>张三</td>
            <td>20</td>
        </tr>
        <tr>
            <td>李四</td>
            <td>40</td>
        </tr>
    </tbody>
    <tfoot>
        <tr>
            <td>2</td>
            <td>30</td>
        </tr>
    </tfoot>
</table>
</body>
</html>
```



## 单元格合并

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>26-单元格合并</title>
</head>
<body>
<!--
1.水平方向上的单元格合并
可以给td标签添加一个colspan属性, 来指定把某一个单元格当做多个单元格来看待(水平方向)
例如:
<td colspan="2"></td>
含义: 把当前单元格当做两个单元格来看待

注意点:
1.由于把某一个单元格当做了多个单元格来看到, 所以就会多出一些单元格, 所以需要删掉一些单元格才能正常显示
2.一定要记住单元格合并永远都是向后或者向下合并, 而不能向前或者向上合并

2.垂直方向上的单元格合并
可以给td标签设置一个rowspan属性, 来指定把某一个单元格当做多个单元格来看待(垂直方向)
例如:
<td rowspan="2"></td>
含义: 把当前单元格当做两个单元格来看待
-->
<table bgcolor="black" width="500px" height="300px" align="center">
    <tr bgcolor="white">
        <td rowspan="2"></td>
        <td></td>
        <td></td>
    </tr>
    <tr bgcolor="white">
        <!--<td></td>-->
        <td></td>
        <td></td>
    </tr>
    <tr bgcolor="white">
        <td colspan="2"></td>
        <td></td>
    </tr>
</table>

<!--<table bgcolor="black" width="500px" height="300px" align="center">-->
    <!--<tr bgcolor="white">-->
        <!--<td colspan="2"></td>-->
        <!--&lt;!&ndash;<td></td>&ndash;&gt;-->
        <!--<td></td>-->
    <!--</tr>-->
    <!--<tr bgcolor="white">-->
        <!--<td></td>-->
        <!--<td></td>-->
        <!--<td></td>-->
    <!--</tr>-->
    <!--<tr bgcolor="white">-->
        <!--<td></td>-->
        <!--<td colspan="2"></td>-->
        <!--&lt;!&ndash;<td></td>&ndash;&gt;-->
    <!--</tr>-->
<!--</table>-->
</body>
</html>
```



## 单元格合并练习

```html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>27-单元格合并练习</title>
</head>
<body>
<!--<table bgcolor="black" cellspacing="1px" width="500px" height="300px" align="center">-->
    <!--<tr bgcolor="white">-->
        <!--<td></td>-->
        <!--<td></td>-->
        <!--<td></td>-->
    <!--</tr>-->
    <!--<tr bgcolor="white">-->
        <!--<td colspan="3"></td>-->
        <!--&lt;!&ndash;<td></td>&ndash;&gt;-->
        <!--&lt;!&ndash;<td></td>&ndash;&gt;-->
    <!--</tr>-->
    <!--<tr bgcolor="white">-->
        <!--<td></td>-->
        <!--<td></td>-->
        <!--<td></td>-->
    <!--</tr>-->
    <!--<tr bgcolor="white">-->
        <!--<td></td>-->
        <!--<td></td>-->
        <!--<td></td>-->
    <!--</tr>-->
<!--</table>-->

<!--<table bgcolor="black" cellspacing="1px" width="500px" height="300px" align="center">-->
    <!--<tr bgcolor="white">-->
        <!--<td></td>-->
        <!--<td rowspan="4"></td>-->
        <!--<td></td>-->
    <!--</tr>-->
    <!--<tr bgcolor="white">-->
        <!--<td></td>-->
        <!--&lt;!&ndash;<td></td>&ndash;&gt;-->
        <!--<td></td>-->
    <!--</tr>-->
    <!--<tr bgcolor="white">-->
        <!--<td></td>-->
        <!--&lt;!&ndash;<td></td>&ndash;&gt;-->
        <!--<td></td>-->
    <!--</tr>-->
    <!--<tr bgcolor="white">-->
        <!--<td></td>-->
        <!--&lt;!&ndash;<td></td>&ndash;&gt;-->
        <!--<td></td>-->
    <!--</tr>-->
<!--</table>-->


<!--<table bgcolor="black" cellspacing="1px" width="500px" height="300px" align="center">-->
    <!--<tr bgcolor="white">-->
        <!--<td></td>-->
        <!--<td></td>-->
        <!--<td></td>-->
    <!--</tr>-->
    <!--<tr bgcolor="white">-->
        <!--<td colspan="2"></td>-->
        <!--&lt;!&ndash;<td></td>&ndash;&gt;-->
        <!--<td></td>-->
    <!--</tr>-->
    <!--<tr bgcolor="white">-->
        <!--<td></td>-->
        <!--<td rowspan="2"></td>-->
        <!--<td></td>-->
    <!--</tr>-->
    <!--<tr bgcolor="white">-->
        <!--<td></td>-->
        <!--&lt;!&ndash;<td></td>&ndash;&gt;-->
        <!--<td></td>-->
    <!--</tr>-->
<!--</table>-->

<!--
快速移动选中的代码, 上下移动
往上移动 Ctrl + shift + ↑(方向键上)
往下移动 Ctrl + shift + ↓(方向键下)

快速合并和展开代码(合并和展开的是某一个标签)
合并: Ctrl + -
展开: Ctrl + +

快速合并和展开代码(合并和展开选中的所有标签)
合并: Ctrl + shift + -
展开: Ctrl + shift + +

快速新启一行
shift + enter
-->
<table bgcolor="black" cellspacing="1px" width="500px" height="300px" align="center">
    <caption>单元格合并</caption>
    <tr bgcolor="white">
        <td></td>
        <td></td>
        <td></td>
    </tr>
    <tr bgcolor="white">
        <td></td>
        <td colspan="2" rowspan="2"></td>
        <!--<td></td>-->
    </tr>
    <tr bgcolor="white">
        <td></td>
        <!--<td colspan="2"></td>-->
        <!--<td></td>-->
    </tr>
    <tr bgcolor="white">
        <td></td>
        <td></td>
        <td></td>
    </tr>
</table>
</body>
</html>
```



## 表单标签

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>28-表单标签</title>
</head>
<body>
<!--
1.什么是表单?
表单就是专门用来收集用户信息的

2.什么是表单元素?
2.1什么是元素?
在HTML中 标签/标记/元素都是指HTML中的标签
例如: <a> a标签/a标记/a元素

表单元素其实还是HTML中的一些标签, 只不过这些标签比较特殊, 在浏览器中所有的表单标签都有特殊的外观和默认的功能

3.表单的格式:
<form>
    <表单元素>
</form>

4.常见的表单元素
input标签, input标签有一个type属性, 这个属性有很多类型的取值, 取值的不同就决定了input标签的功能和外观不同

5.注意:
表单元素一定要写在表单中
-->
<form>
    <!--明文输入框-->
    账号:<input type="text"><br>
    <!--暗文输入框-->
    密码:<input type="password"><br>
    <!--给输入框设置默认值-->
    账号:<input type="text" value="lnj"><br>
    密码:<input type="password" value="123"><br>

    <!--
    单选框
    注意点:
    1.默认情况下单选框不会互斥, 要想单选框互斥那么必须给每一个单选框标签都设置一个name属性, 然后name属性还必须设置相同的值
    2.要想让单选框默认选中某一个框子, 那么可以给input标签添加一个checked属性
    3.在HTML中如果属性的取值和属性的名称一样, 可以只写一个. 但是在XHTML中必须写上取值, 所以在企业开发中我们推荐大家不要省略取值
    -->
    性别:
    <input type="radio" name="xx" checked>男
    <input type="radio" name="xx">女
    <input type="radio" name="xx" >保密<br>

    <!--多选框-->
    爱好:
    <input type="checkbox">篮球
    <input type="checkbox">足球
    <input type="checkbox" checked="checked">棒球
    <input type="checkbox" checked="checked">足浴
</form>
</body>
</html>
```



## 表单标签2

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>29-表单标签2</title>
</head>
<body>
<form action="http://www.baidu.com">
    <!--明文输入框-->
    账号:<input type="text" name="aa"><br>
    <!--暗文输入框-->
    密码:<input type="password" name="bb"><br>
    <!--
    定义一个普通按钮
    作用:配合JS完成一些操作
    -->
    <input type="button" value="我是按钮" onclick="alert('lnj')">

    <!--
    定义一个图片按钮
    作用:配合JS完成一些操作
    -->
    <input type="image" src="images/register.jpg" onclick="alert('lnj')">

    <!--
    定义重置按钮
    作用:清空表单中的数据
    注意点:
    重置按钮有默认的按钮标题, 默认叫做 重置
    也可以通过value属性来修改默认标题
    -->
    <input type="reset" value="清空">

    <!--
    定义提交按钮
    作用:将表单中的数据提交到远程服务器
    注意点:
    要想把表单中的数据提交到远程服务器,必须满足两个条件
    1.告诉表单需要提交到哪个服务器
    可以通过form标签的action属性来告诉表单,需要提交到那个服务器
    2.告诉表单,表单中的哪些数据需要提交
    -->
    <input type="submit">

    <!--
    隐藏域
    作用: 用于悄悄咪咪的收集用户的一些数据, 隐藏域是不会出现在界面中的
    -->
    <input type="hidden" name="cc" value="it666">
</form>
</body>
</html>
```



## label标签

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>30-Label标签</title>
</head>
<body>
<!--
1.默认情况下文字和输入框是没有关联关系的, 也就是说点击文字输入框不会聚焦, 如果想点击文字时让对应的输入框聚焦, 那么就需要让文字和输入框进行绑定

2.要想让输入框和文字绑定在一起, 那么我们可以使用Label标签

3.绑定的格式:
3.1将文字利用label标签包裹起来
3.2给输入框添加一个id属性
3.3在label标签中通过for属性和输入框中的id进行绑定即可
  <label for="account">账号:</label><input type="text" id="account">
-->
<form action="">
    <label for="account">账号:</label><input type="text" id="account"><br>
    <label for="pwd">密码:</label><input type="password" id="pwd"><br>

    <hr>
    <label>
        账号:<input type="text">
    </label><br>
    <hr>

    <label for="def">账号:</label><input type="text" id="abc"><br>
    <label for="abc">密码:</label><input type="password" id="def"><br>
</form>
</body>
</html>
```



## datalist标签

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>31-Datalist标签</title>
</head>
<body>
<!--
1.datalist标签
作用: 给输入框绑定待选项

2.datalist格式:
<datalist>
    <option>待选项内容</option>
</datalist>

3.如何给输入框绑定待选列表
1.搞一个输入框
2.搞一个datalist列表
3.给datalist列表标签添加一个id
4.给输入框添加一个list属性,将datalist的id对应的值赋值给list属性即可
-->

请输入你的车型: <input type="text" list="cars">

<datalist id="cars">
    <option>奔驰</option>
    <option>宝马</option>
    <option>奥迪</option>
    <option>路虎</option>
    <option>宾利</option>
</datalist>

</body>
</html>
```



## 表单标签H5

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>32-表单标签-H5</title>
</head>
<body>
<form>
    <!--
    可以自动校验输入的内容是否符合邮箱的格式
    -->
    邮箱:<input type="email"><br>
    <!--
    可以自动校验输入的内容是否是URL地址
    -->
    域名:<input type="url"><br>
    <!--
    输入框中只能输入数字
    -->
    电话:<input type="number"><br>
    <!--
    可以通过日历来选择时间
    -->
    时间:<input type="date"><br>

    <!--
    可以通过取色板来选择颜色
    -->
    颜色: <input type="color"><br>

    <input type="submit">
</form>
</body>
</html>
```



## select和option标签

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>33-表单标签3</title>
    <style type="text/css">
        textarea{
            resize: none;
        }
    </style>
</head>
<body>
<!--
1.select标签
作用: 用于定义下拉列表

格式:
<select>
    <optgroup label="分组名称">
        <option>列表数据</option>
    </optgroup>
</select>

注意点:
1.下拉列表不能输入内容, 但是可以直接在列表中选择内容
2.可以通过给option标签添加一个selected属性来指定列表的默认值
3.可以通过给option标签包裹一层optgroup标签来给下拉列表中的数据分类

2.textarea标签
作用: 定义一个多行输入框

格式:
<textarea>
</textarea>

注意点:
1.默认情况下输入框可以无限换行
2.默认情况下输入框有自己的宽度和高度
3.可以通过cols和rows来指定输入框的宽度和高度, 但是虽然指定了列数和行数, 但是还是可以无限往下输入
4.默认情况下输入框是可以手动拉伸的
-->

<!--<select>-->
    <!--<option>北京</option>-->
    <!--<option>上海</option>-->
    <!--<option>广州</option>-->
    <!--<option>广西</option>-->
    <!--<option selected="selected">武汉</option>-->
<!--</select>-->

<select>
    <optgroup label="北京">
        <option>朝阳区</option>
        <option>昌平区</option>
        <option>通州区</option>
    </optgroup>
    <optgroup label="广州">
        <option>天河区</option>
        <option>越秀区</option>
        <option>黄浦区</option>
    </optgroup>
</select>

<hr>

<textarea cols="20" rows="5">
</textarea>
</body>
</html>
```



## 表单练习

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>10-表单练习</title>
</head>
<body>
<form action="http://www.520it.com">
    <fieldset>
        <legend>注册界面</legend>
        <p>
            账号: <input type="text" name="username">
        </p>
        <p>
            密码: <input type="password" name="pwd">
        </p>
        <p>
            性别:
            <input type="radio" name="gender" value="male">男
            <input type="radio" name="gender" value="female">女
            <input type="radio" name="gender" checked="checked" value="yao">保密
        </p>
        <p>
            爱好:
            <input type="checkbox" name="hobby" value="basketball">篮球
            <input type="checkbox" name="hobby" value="football">足球
            <input type="checkbox" name="hobby" checked="checked" value="crazy">足浴
        </p>
        <p>
            个人简介:
            <textarea cols="25" rows="5"></textarea>
        </p>
        <p>
            生日: <input type="date" name="birthday">
        </p>
        <p>
            邮箱: <input type="email" name="email">
        </p>
        <p>
            手机: <input type="number" maxlength="11" name="phone">
        </p>
        <p>
            <input type="submit" value="注册">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
            <input type="reset" value="清空">
        </p>
    </fieldset>
</form>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>34-表单练习</title>
</head>
<body>

<form action="http://www.baidu.com">
    <fieldset>
        <legend>注册界面</legend>
        <p>
            账号: <input type="text" name="account">
        </p>
        <p>
            密码: <input type="password" name="pwd">
        </p>
    <!--
    在单选框和多选框中,所有的项目的name必须一致
    在表单标签中,除了按钮标签以外的标签,都可以通过value来指定需要提交到服务器的数据
    -->
        <p>
            性别:
            <input type="radio" name="gender" value="male">男
            <input type="radio" name="gender" value="female">女
            <input type="radio" checked="checked" name="gender" value="yao">保密
        </p>
        <p>
            爱好:
            <input type="checkbox" name="sport" value="basketball">篮球
            <input type="checkbox" name="sport" value="football">足球
            <input type="checkbox" checked="checked" name="sport" value="crazy">足浴
        </p>
        <p>
            简介:
            <textarea cols="30" rows="10" name="desc"></textarea>
        </p>
        <p>
            生日:
            <input type="date" name="birthday">
        </p>
        <p>
            邮箱:
            <input type="email" name="email">
        </p>
        <p>
            电话:
            <input type="number" name="phone">
        </p>
        <p>
            <input type="submit" value="注册">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
            <input type="reset" value="清空">
        </p>
    <!--
    account=lnj
    &pwd=123
    &gender=on
    &sport=on
    &sport=on
    &desc=just+do+it
    &birthday=2018-03-26
    &email=123%40qq.com
    &phone=01234567890

account=lnj
&pwd=123
&gender=male
&sport=basketball
&sport=crazy
&desc=just+do+it
&birthday=2018-03-26
&email=123%40qq.com
&phone=01234567890
    -->
        </fieldset>
</form>
</body>
</html>
```



## video标签

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>35-video标签</title>
</head>
<body>
<!--
1.什么是video标签?
作用: 播放视频

格式:
<video src="">
</video>

video标签的属性
src: 用于告诉video标签需要播放的视频地址
autoplay: 用于告诉video标签是否需要自动播放视频
controls: 用于告诉video标签是否需要显示控制条
poster: 用于告诉video标签视频没有播放之前显示的占位图片
loop: 一般用于做广告视频, 用于告诉video标签视频播放完毕之后是否需要循环播放
preload: 预加载视频, 但是需要注意preload和autoplay相冲, 如果设置了autoplay属性, 那么preload属性就会失效
muted:静音
width/height: 和img标签中的一模一样
-->

<!--<video src="images/sb1.webm" autoplay="autoplay" controls="controls"></video>-->
<!--<video src="images/sb1.webm"  controls="controls" poster="images/NJ.jpg"></video>-->
<video src="images/sb1.webm"  autoplay="autoplay" loop="loop" muted="muted" width="800px"></video>
</body>
</html>
```



## video标签的第二种用法

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>36-video标签的第二种格式</title>
</head>
<body>
<!--
1.格式:
<video>
    <source src="" type=""></source>
    <source src="" type=""></source>
</video>

2.第二种格式存在的意义:
由于视频数据非常非常的重要, 所以五大浏览器厂商都不愿意支持别人的视频格式, 所以导致了没有一种视频格式是所有浏览器都支持的
这个时候W3C为了解决这个问题, 所以推出了第二个video标签的格式

video标签的第二种格式存在的意义就是为了解决浏览器适配问题
video 元素支持三种视频格式, 我们可以把这三种格式都通过source标签指定给video标签, 那么以后当浏览器播放视频时它就会从这三种中选择一种自己支持的格式来播放

3.注意点:
3.1当前通过video标签的第二种格式虽然能够指定所有浏览器都支持的视频格式, 但是想让所有浏览器都通过video标签播放视频还有一个前提条件, 就是浏览器必须支持HTML5标签, 否则同样无法播放
3.2在过去的一些浏览器是不支持HTML5标签的, 所以为了让过去的一些浏览器也能够通过video标签来播放视频, 那么我们以后可以通过一个JS的框架叫做html5media来实现
-->
<video autoplay="autoplay" controls="controls">
    <source src="images/sb1.webm" type="video/webm"></source>
    <source src="images/sb1.mp4" type="video/mp4"></source>
    <source src="images/sb1.ogg" type="video/ogg"></source>
</video>
</body>
</html>
```



## audio标签

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>37-audio标签</title>
</head>
<body>
<!--
1.什么是audio标签?
作用: 播放音频

格式:
<audio src="">
</audio>

<audio>
    <source src="" type="">
</audio>

注意点:
audio标签的使用和video标签的使用基本一样, video中能够使用的属性在audio标签中大部分都能够使用, 并且功能都一样
只不过有3个属性不能用, height/width/poster
-->

<!--<audio src="images/yinyue.mp3" autoplay="autoplay" controls="controls"></audio>-->

<audio autoplay="autoplay" controls="controls">
    <source src="images/yinyue.mp3" type="audio/mp3">
</audio>
</body>
</html>
```



## 详情和概要标签

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>38-详情和概要标签</title>
</head>
<body>
<!--
1.什么是详情和概要标签?
作用:利用summary标签来描述概要信息, 利用details标签来描述详情信息
默认情况下是折叠展示, 想看见详情必须点击

格式:
<details>
    <summary>概要信息</summary>
    详情信息
</details>
-->

<details>
    <summary>郑伊健</summary>
简介：郑伊健，1967年10月4日出生于中国香港，籍贯广东恩平，香港影视演员、流行男歌手。1988年参加新秀歌唱大赛加入无线电视，因拍摄“阳光柠檬茶”广告而入行，拜罗文为师。1991年...
</details>
</body>
</html>
```



## marquee标签

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>39-marquee标签</title>
    <style>
        marquee {
            width:200px;
            height:200px;
            background-color: skyblue;
        }
    </style>
</head>
<body>
<!--
1.什么是marquee标签?
作用: 跑马灯

格式:
<marquee>内容</marquee>

属性:
direction: 设置滚动方向 left/right/up/down
scrollamount: 设置滚动速度, 值越大就越快
loop: 设置滚动次数, 默认是-1, 也就是无限滚动
behavior: 设置滚动类型 slide滚动到边界就停止, alternate滚动到边界就弹回

注意点:
marquee标签不是W3C推荐的标签, 在W3C官方文档中也无法查询这个标签, 但是各大浏览器对这个标签的支持非常好
-->

<!--滚动方向-->
<marquee>随便写点内容</marquee>
<marquee direction="right">随便写点内容</marquee>
<marquee direction="up">随便写点内容</marquee>
<marquee direction="down">随便写点内容</marquee>
<hr>
<!--设置滚动速度-->
<marquee scrollamount="1">随便写点内容</marquee>
<marquee scrollamount="100">随便写点内容</marquee>
<hr>
<!--设置滚动次数-->
<marquee loop="1">随便写点内容</marquee>
<hr>
<!--设置滚动类型-->
<marquee behavior="slide">随便写点内容</marquee>
<marquee behavior="alternate">随便写点内容</marquee>

<marquee>
    <img src="images/NJ.jpg" width="50px">
</marquee>
</body>
</html>
```



## html中被废弃的标签

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>40-HTML中被废弃的标签</title>
    <style type="text/css">
        .one {
            font-weight: bold;
        }
        .two {
            text-decoration: underline;
        }
        .three {
            font-style: italic;
        }
        .four {
            text-decoration: line-through;
        }
    </style>
</head>
<body>
<!--
1.为什么HTML中有一部分标签会被废弃?
因为当前的HTML中的标签只有一个作用, 就是用来添加语义
而早期的HTML标签中有一部分标签是没有语义的,
有一部分标签是用来修改样式的
所以这部分标签就被淘汰了

<br> <hr> <font>
<b> <u> <i> <s>以上标签都是没有语义的,都是用来修改样式的
b(bold) 加粗文本, 没有任何语义的
u(underline) 给文本天津下划线, 没有任何语义的
i(italic) 将文本倾斜, 没有任何语义的
s(strikethourgh) 给文本添加删除线, 没有任何语义的

注意点:
以后在企业开发中, 不到万不得已一定不要使用这些被废弃掉的标签
如果一定要使用, 一般情况下都是用来作为CSS的钩子来使用

strong == b
ins == u
em == i
del == s

strong语义: 定义重要性强调的文字
ins语义(inseted): 定义插入的文字
em语义(emphasized): 定义强调的文字
del语义(deleted): 定义被删除的文字
-->

<b>我是文字</b>
<u>我是文字</u>
<i>我是文字</i>
<s>我是文字</s>
<hr>
<strong>我是文字</strong>
<ins>我是文字</ins>
<em>我是文字</em>
<del>我是文字</del>
<hr>
<p class="one">我是文字</p>
<p class="two">我是文字</p>
<p class="three">我是文字</p>
<p class="four">我是文字</p>
</body>
</html>
```



## 字符实体

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>41-字符实体</title>
</head>
<body>
<!--
1.在HTML中对空格/回车/tab不敏感, 会把多个空格/回车/tab当做一个空格来处理


2.什么是字符实体?
在HTML中有的字符是被HTML保留的, 有的HTML字符在HTML中是有特殊含义的, 是不能在浏览器中直接显示出来的, 那么这些东西要想显示出来就必须通过字符实体

&nbsp; 空格, 一个&nbsp;就是一个空格, 有多少个&nbsp;就有多少个空格
&lt; 小于符号 <
(less than)
&gt; 大于符号 >
(greater than)
&copy; 版权符号
-->

<p>我&nbsp;&nbsp;&nbsp;爱你</p>

<p>到此为止我们的HTML的基础标签就学习完毕了, 例如我们学习了&lt;h1&gt;标签, &lt;table&gt;标签, &lt;img&gt;标签....</p>

&copy;
</body>
</html>
```



# CSS

## css格式

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>02-CSS的格式</title>
    <style type="text/css">
        p{
            text-align: center;
            color: red;
        }
    </style>
</head>
<body>
<!--
1.格式:
<style type="text/css">
        标签名称{
            属性名称: 属性对应的值;
            ...
        }
</style>

2.注意点:
1.style标签必须写在head标签的开始标签和结束标签之间(也就是必须和title标签是兄弟关系)
2.style标签中的type属性其实可以不用写, 默认就是type="text/css"
3.设置样式时必须按照固定的格式来设置. key: value;
其中:不能省略, 分号大多数情况下也不能省略

4.CSS怎么学?
CSS的学习一共分为两大部分, 一个是CSS的属性, 另一个是CSS选择器. 也就是说着两部分学完CSS就没有别的东西了
-->
<h1>我是标题</h1>
<p>我是段落</p>
<p>我是段落</p>
<p>我是段落</p>
</body>
</html>
```



## 字体属性

```html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>03-文字属性</title>
    <style>
        p{
            font-style: italic;
            font-weight: bold;
            font-size: 10px;
            font-family:"楷体";
        }
    </style>
</head>
<body>
<!--
1.规定文字样式的属性
格式：font-style: italic;
取值：
normal ： 正常的， 默认就是正常的
italic :  倾斜的
快捷键：
fs font-style: italic;
fsn font-style: normal;

2.规定文字粗细的属性
格式： font-weight: bold;
单词取值:
bold 加粗
bolder  比加粗还要粗
lighter 细线， 默认就是细线
数字取值：
100-900之间整百的数字

快捷键
fw font-weight:;
fwb font-weight: bold;
fwbr  font-weight: bolder;

3.规定文字大小的属性
格式：font-size: 30px;
单位：px（像素 pixel）
注意点： 通过font-size设置大小一定要带单位， 也就是一定要写px
快捷键
fz font-size:;
fz30 font-size: 30px;

4.规定文字字体的属性
格式：font-family:"楷体";
注意点：
1.如果取值是中文， 需要用双引号或者单引号括起来
2.设置的字体必须是用户电脑里面已经安装的字体
快捷键
ff font-family:;

-->
<i>我是文字</i>
<b>我是文字</b>
<p>abc我是段落</p>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>04-字体属性补充</title>
    <style>
        p{
            /*
            被注释掉的内容
            */
            /*font-family:"乱七八糟的字体", "微软雅黑";*/
            /*font-family: Arial, "微软雅黑";*/
            font-family:"Microsoft YaHei";
        }
    </style>
</head>
<body>
<!--
1.如果设置的字体不存在, 那么系统会使用默认的字体来显示
默认使用宋体

2.如果设置的字体不存在, 而我们又不想用默认的字体来显示怎么办?
可以给字体设置备选方案
格式:font-family:"字体1", "备选方案1", ...;

3.如果想给中文和英文分别单独设置字体, 怎么办?
但凡是中文字体, 里面都包含了英文
但凡是英文字体, 里面都没有包含中文
也就是说中文字体可以处理英文, 而英文字体不能处理中文
**注意点: 如果想给界面中的英文单独设置字体, 那么英文的字体必须写在中文的前面

4.补充在企业开发中最常见的字体有以下几个
中文: 宋体/黑体/微软雅黑
英文: "Times New Roman"/Arial

还需要知道一点, 就是并不是名称是英文就一定是英文字体
因为中文字体其实都有自己的英文名称, 所以是不是中文字体主要看能不能处理中文
宋体 SimSun
黑体 SimHei
微软雅黑 Microsoft YaHei
-->
<p>abc我是段落</p>
</body>
</html>
```



## 文字属性的缩写

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>05-文字属性的缩写</title>
    <style>
        p{
            /*
            font-style: italic;
            font-weight: bold;
            font-size: 10px;
            font-family:"楷体";
            */
            font:bold italic 10px "楷体";
        }
    </style>
</head>
<body>
<!--
缩写格式:
font: style weight size family;
例如:
font:italic bold 10px "楷体";

注意点:
1.在这种缩写格式中有的属性值可以省略
sytle可以省略
weight可以省略
2.在这种缩写格式中style和weight的位置可以交换
3.在这种缩写格式中有的属性值是不可以省略的
size不能省略
family不能省略
4.size和family的位置是不能顺便乱放的
size一定要写在family的前面, 而且size和family必须写在所有属性的最后
-->
<p>abc我是段落</p>
</body>
</html>
```



## 文本属性

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>06-文本属性</title>
    <style>
        p{
            text-decoration: none;
            text-align: left;
            text-indent: 2em;

        }
        a{
            text-decoration: none;
        }
    </style>
</head>
<body>
<!--
1.文本装饰的属性
格式:text-decoration: underline;
取值:
underline 下划线
line-through 删除线
overline 上划线
none 什么都没有, 最常见的用途就是用于去掉超链接的下划线
快捷键:
td  text-decoration: none;
tdu text-decoration: underline;
tdl text-decoration: line-through;
tdo text-decoration: overline;

2.文本水平对齐的属性
格式: text-align: right;
取值:
left 左
right 右
center 中
快捷键
ta text-align: left;
tar text-align: right;
tac text-align: center;

3.文本缩进的属性
格式: text-indent: 2em;
取值:
2em, 其中em是单位, 一个em代表缩进一个文字的宽度
快捷键
ti text-indent:;
ti2e text-indent: 2em;
-->
<u>我是文字</u>
<s>我是文字</s>
<p>我是段落我是段落我是段落我是段落我段落是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是</p>
<a href="#">我是超链接</a>
</body>
</html>
```



## 颜色属性

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>07-颜色属性</title>
    <style>
        p{
            /*color: red;*/
            /*color: rgb(255,0,0);*/
            /*color: rgba(255,0,0,1);*/
            color: #FF0000;
            color: #F00;
            /*color: rgba(255,0,0,0.2);*/
            color: #ffee00;
            color: #fe0;
            color: #769abb;
        }
    </style>
</head>
<body>
<!--
1.在CSS中如何通过color属性来修改文字颜色
格式: color: 值;
取值:
1.1英文单词
一般情况下常见的颜色都有对应的英文单词, 但是英文单词能够表达的颜色是有限制的, 也就是说不是所有的颜色都能够通过英文单词来表达

1.2rgb
rgb其实就是三原色, 其中r(red 红色) g(green 绿色) b(blue 蓝色)
格式: rgb(0,0,0)
那么这个格式中的
第一个数字就是用来设置三原色的光源元件红色显示的亮度
第二个数字就是用来设置三原色的光源元件绿色显示的亮度
第三个数字就是用来设置三原色的光源元件蓝色显示的亮度
这其中的每一个数字它的取值是0-255之前, 0代表不发光, 255代表发光, 值越大就越亮

红色: rgb(255,0,0);
绿色: rgb(0,255,0);
蓝色: rgb(0,0,255);
黑色: rgb(0,0,0);
白色: rgb(255,255,255);

在前端开发中其实并不常用黑色
只要让红色/绿色/蓝色的值都一样就是灰色
而且如果这三个值越小那么就越偏黑色, 越大就越偏白色
例如: color: rgb(200,200,200);

1.3rgba
rgba中的rgb和前面讲解的一样, 只不过多了一个a
那么这个a呢代表透明度, 取值是0-1, 取值越小就越透明
例如: color: rgba(255,0,0,0.2);

1.4十六进制
在前端开发中通过十六进制来表示颜色, 其实本质就是RGB
十六进制中是通过每两位表示一个颜色
例如: #FFEE00 FF表示R EE表示G 00表示B

什么是十六进制?
十六进制和十进制一样都是一种计数的方式
在十进制中取值范围0-9, 逢十进一
在十六进制中取值范围0-F, 逢十六进一

9
a  10
b  11
c  12
d  13
e  14
f  15
10  16
11  17
12  18

十六进制和十进制转换的公式
用十六进制的第一位*16 + 十六进制的第二位 = 十进制
15 == 1*16 + 5 = 21
12 == 1*16 + 2 = 18
FF == F*16 + F == 15*16 + 15 == 240 + 15 = 255
00 == 0*16 + 0 = 0

1.5十六进制缩写
在CSS中只要十六进制的颜色每两位的值都是一样的, 那么就可以简写为一位
例如:
#FFEE00 == #FE0

注意点:
1.如果当前颜色对应的两位数字不一样, 那么就不能简写
#123456;

2.如果两位相同的数字不是属于同一个颜色的, 也不能简写
#122334

-->
<p>我是段落</p>
</body>
</html>
```



## 标签选择器

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>08-标签选择器</title>
    <style>
        p{
            color: red;
        }
        h1{
            color: blue;
        }
    </style>
</head>
<body>
<!--
1.什么是标签选择器?
作用: 根据指定的标签名称, 在当前界面中找到所有该名称的标签, 然后设置属性

格式:
标签名称{
    属性:值;
}

注意点:
1.标签选择器选中的是当前界面中所有的标签, 而不能单独选中某一个标签
2.标签选择器无论标签藏得多深都能选中
3.只要是HTML中的标签就可以作为标签选择器(h/a/img/ul/ol/dl/input....)
-->
<p>我是段落</p>
<p>我是段落</p>
<p>我是段落</p>
<p>我是段落</p>
<ul>
    <li>
        <ul>
            <li>
                <ul>
                    <li>
                        <p>我是段落</p>
                    </li>
                </ul>
            </li>
        </ul>
    </li>
</ul>
<h1>我是标题</h1>
</body>
</html>
```



## id选择器

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>09-id选择器</title>
    <style>
        #identity1{
            color: red;
        }
        #identity2{
            color: green;
        }
        #identity3{
            color: blue;
        }
        #identity4{
            color: yellow;
        }
    </style>
</head>
<body>
<!--
1.什么是id选择器?
作用: 根据指定的id名称找到对应的标签, 然后设置属性

格式:
#id名称{
    属性:值;
}

注意点:
1.每个HTML标签都有一个属性叫做id, 也就是说每个标签都可以设置id
2.在同一个界面中id的名称是不可以重复的
3.在编写id选择器时一定要在id名称前面加上#
4.id的名称是有一定的规范的
4.1id的名称只能由字母/数字/下划线
a-z 0-9 _
4.2id名称不能以数字开头
4.3id名称不能是HTML标签的名称
不能是a h1 img input ...
5.在企业开发中一般情况下如果仅仅是为了设置样式, 我们不会使用id ,因为在前端开发中id是留给js使用的
-->
<p id="identity1">迟到毁一生</p>
<p id="identity2">早退穷三代</p>
<p id="identity3">按时上下班</p>
<p id="identity4">必成高富帅</p>
</body>
</html>
```



## class选择器

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>10-类选择器</title>
    <style>
        .pp{
            color: red;
        }
        .ppp{
            color: green;
        }
        .pppp{
            color: blue;
        }
        .ppppp{
            color: yellow;
        }
        .para1{
            font-size: 100px;
        }
        .para2{
            font-style: italic;
        }
    </style>
</head>
<body>
<!--
1.什么是类选择器?
作用: 根据指定的类名称找到对应的标签, 然后设置属性

格式:
.类名{
    属性:值;
}

注意点:
1.每个HTML标签都有一个属性叫做class, 也就是说每个标签都可以设置类名
2.在同一个界面中class的名称是可以重复的
3.在编写class选择器时一定要在class名称前面加上.
4.类名的命名规范和id名称的命名规范一样
5.类名就是专门用来给CSS设置样式的
6.在HTML中每个标签可以同时绑定多个类名
格式:
<标签名称 class="类名1 类名2 ...">
错误的写法:
<p class="para1" class="para2">
-->
<p class="pp">迟到毁一生</p>
<p class="ppp">早退穷三代</p>
<p class="pppp">按时上下班</p>
<p class="ppppp">必成高富帅</p>

<p class="para1 para2">我是段落</p>
<p class="para1 para2">我是段落</p>

</body>
</html>
```



## 后代选择器

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>12-后代选择器</title>
    <style>
        /*
        div p{
            color: red;
        }
        */
        /*
        #identity p{
            color: red;
        }
        */
        /*
        .para p{
            color: blue;
        }
        */
        /*
        #identity #iii{
            color: skyblue;
        }
        */
        /*
        #identity .ccc{
            color: purple;
        }
        */
        div ul li p{
            color: red;
        }
    </style>
</head>
<body>
<!--
1.什么是后代选择器?
作用: 找到指定标签的所有特定的后代标签, 设置属性

格式:
标签名称1 标签名称2{
    属性:值;
}
先找到所有名称叫做"标签名称1"的标签, 然后再在这个标签下面去查找所有名称叫做"标签名称2"的标签, 然后在设置属性
div p{}

注意点:
1.后代选择器必须用空格隔开
2.后代不仅仅是儿子, 也包括孙子/重孙子, 只要最终是放到指定标签中的都是后代
3.后代选择器不仅仅可以使用标签名称, 还可以使用其它选择器
4.后代选择器可以通过空格一直延续下去
-->
<!--div ul li p-->
<p>我是段落</p>
<div id="identity" class="para">
    <p>我是段落</p>
    <p>我是段落</p>
    <ul>
        <li>
            <!--<p id="iii" class="ccc">我是段落</p>-->
            <p>我是段落</p>
        </li>
        <li>
            <p>我是段落</p>
        </li>
    </ul>
</div>
<p>我是段落</p>
</body>
</html>
```



## 子元素选择器

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>13-子元素选择器</title>
    <style>
        /*
        div>p{
            color: red;
        }
        */
        
        #identity>p{
            color: blue;
        }
        
        /*
        div>ul>li>p{
            color: purple;
        }
        */
    </style>
</head>
<body>
<!--
1.什么是子元素选择器?
作用: 找到指定标签中所有特定的直接子元素, 然后设置属性

格式:
标签名称1>标签名称2{
    属性:值;
}
先找到所有名称叫做"标签名称1"的标签, 然后在这个标签中查找所有直接子元素名称叫做"标签名称2"的元素

注意点:
1.子元素选择器只会查找儿子, 不会查找其他被嵌套的标签
2.子元素选择器之间需要用>符号连接, 并且不能有空格
3.子元素选择器不仅仅可以使用标签名称, 还可以使用其它选择器
4.子元素选择器可以通过>符号一直延续下去
-->
<!--div>ul>li>p-->
<p>我是段落</p>
<div id="identity">
    <p>我是段落</p>
    <p>我是段落</p>
    <ul>
        <li><p>我是段落</p></li>
    </ul>
</div>
<p>我是段落</p>
</body>
</html>
```



## 子代和后代选择器

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>14-后代选择器和子元素选择器</title>
</head>
<body>
<!--
1.后代选择器和子元素选择器之间的区别?
1.1
后代选择器使用空格作为连接符号
子元素选择器使用>作为连接符号
1.2
后代选择器会选中指定标签中, 所有的特定后代标签, 也就是会选中儿子/孙子..., 只要是被放到指定标签中的特定标签都会被选中
子元素选择器只会选中指定标签中, 所有的特定的直接标签, 也就是只会选中特定的儿子标签

2.后代选择器和子元素选择器之间的共同点
2.1
后代选择器和子元素选择器都可以使用标签名称/id名称/class名称来作为选择器
2.2
后代选择器和子元素选择器都可以通过各自的连接符号一直延续下去
选择器1>选择器2>选择器3>选择器4{}

3.在企业开发中如何选择
如果想选中指定标签中的所有特定的标签, 那么就使用后代选择器
如果只想选中指定标签中的所有特定儿子标签, 那么就使用子元素选择器

-->
</body>
</html>
```



## 交集选择器

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>15-交集选择器</title>
    <style>
        /*
        p.para1{
            color: red;
        }
        */
        .para1#identity{
            color: blue;
        }
    </style>
</head>
<body>
<!--
1.什么是交集选择器?
作用: 给所有选择器选中的标签中, 相交的那部分标签设置属性

格式:
选择器1选择器2{
    属性: 值;
}

注意点:
1.选择器和选择器之间没有任何的连接符号
2.选择器可以使用标签名称/id名称/class名称
3.交集选择器仅仅作为了解, 企业开发中用的并不多
-->

<p>我是段落</p>
<p>我是段落</p>
<p class="para1" id="identity">我是段落</p>
<p class="para1">我是段落</p>
<p>我是段落</p>
</body>
</html>
```



## 并集选择器

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>16-并集选择器</title>
    <style>
        /*
        .ht{
            color: red;
        }
        .para{
            color: red;
        }
        */
        .ht,.para{
            color: red;
        }
    </style>
</head>
<body>
<!--
1.什么是并集选择器?
作用: 给所有选择器选中的标签设置属性

格式:
选择器1,选择器2{
    属性:值;
}

注意点:
1.并集选择器必须使用,来连接
2.选择器可以使用标签名称/id名称/class名称
-->

<h1 class="ht">我是标题</h1>
<p class="para">我是段落</p>
<p>我是段落</p>
<p>我是段落</p>
</body>
</html>
```



## 兄弟选择器

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>17-兄弟选择器</title>
    <style>
        /*
        h1+p{
            color: red;
        }
        */
        h1~p{
            color: green;
        }
    </style>
</head>
<body>
<!--
1.相邻兄弟选择器 CSS2
作用: 给指定选择器后面紧跟的那个选择器选中的标签设置属性

格式:
选择器1+选择器2{
    属性:值;
}

注意点:
1.相邻兄弟选择器必须通过+连接
2.相邻兄弟选择器只能选中紧跟其后的那个标签, 不能选中被隔开的标签

2.通用兄弟选择器 CSS3
作用: 给指定选择器后面的所有选择器选中的所有标签设置属性

格式:
选择器1~选择器2{
    属性:值;
}

注意点:
1.通用兄弟选择器必须用~连接
2.通用兄弟选择器选中的是指定选择器后面某个选择器选中的所有标签, 无论有没有被隔开都可以选中
-->

<!--
<h1>我是标题</h1>
<a href="#">我是超链接</a>
<p>我是段落</p>
<p>我是段落</p>
<p>我是段落</p>
<h1>我是标题</h1>
<p>我是段落</p>
<p>我是段落</p>
<p>我是段落</p>
-->
<h1>我是标题</h1>
<a href="#">我是超链接</a>
<p>我是段落</p>
<p>我是段落</p>
<a href="#">我是超链接</a>
<p>我是段落</p>

<h1>我是标题</h1>
<p>我是段落</p>
<p>我是段落</p>
<p>我是段落</p>
</body>
</html>
```



## 序选择器

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>18-序选择器</title>
    <style>
        /*
        p:first-child{
            color: red;
        }
        */
        /*
        p:first-of-type{
            color: blue;
        }
        */
        /*
        p:last-child{
            color: red;
        }
        */
        /*
        p:last-of-type{
            color: blue;
        }
        */
        /*
        p:nth-child(3){
            color: red;
        }
        */
        /*
        p:nth-of-type(3){
            color: blue;
        }
        */
        /*
        p:nth-last-child(2){
            color: red;
        }
        */
        /*
        p:nth-last-of-type(2){
            color: red;
        }
        */
        /*
        p:only-child{
            color: purple;
        }
        */
        /*
        p:only-of-type {
            color: red;
        }
        */
        .para:only-of-type {
            color: red;
        }
    </style>
</head>
<body>
<!--
CSS3中新增的选择器最具代表性的就是序选择器
1.同级别的第几个
:first-child 选中同级别中的第一个标签
:last-child 选中同级别中的最后一个标签
:nth-child(n) 选中同级别中的第n个标签
:nth-last-child(n) 选中同级别中的倒数第n个标签
:only-child 选中父元素中唯一的标签
注意点: 不区分类型

2.同类型的第几个
:first-of-type 选中同级别中同类型的第一个标签
:last-of-type  选中同级别中同类型的最后一个标签
:nth-of-type(n) 选中同级别中同类型的第n个标签
:nth-last-of-type(n)  选中同级别中同类型的倒数第n个标签
:only-of-type 选中父元素中唯一类型的某个标签
-->
<!--
<h1>我是标题</h1>
<p>我是段落1</p>
<p>我是段落2</p>
<p>我是段落3</p>
<p>我是段落4</p>
<div>
    <p>我是段落5</p>
    <p>我是段落6</p>
    <p>我是段落7</p>
    <p>我是段落8</p>
</div>
-->
<p class="para">我是段落1</p>
<div>
    <p class="para">我是段落2</p>
    <p class="para">我是段落2</p>
    <h1>我是标题</h1>
</div>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>19-序选择器下</title>
    <style>
        /*
        p:nth-child(odd){
            color: red;
        }
        p:nth-child(even){
            color: blue;
        }
         */
        /*
        p:nth-of-type(odd){
            color: red;
        }
        p:nth-of-type(even){
            color: blue;
        }
        */
        /*
        p:nth-child(2n+0){
            color: red;
        }
        p:nth-child(2n+1){
            color: blue;
        }
        */
        p:nth-child(3n+0){
            color: red;
        }
    </style>
</head>
<body>
<!--
:nth-child(odd) 选中同级别中的所有奇数
:nth-child(even) 选中同级别中的所有偶数

:nth-child(xn+y)
x和y是用户自定义的, 而n是一个计数器, 从0开始递增

-->
<p>我是项目</p>
<p>我是项目</p>
<p>我是项目</p>
<p>我是项目</p>
<p>我是项目</p>
<p>我是项目</p>
<p>我是项目</p>
</body>
</html>
```



## 属性选择器

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>20-属性选择器上</title>
    <style>
        /*
        p[id]{
            color: red;
        }
        */
        p[class=cc]{
            color: blue;
        }
    </style>
</head>
<body>
<!--
1.什么是属性选择器?
作用: 根据指定的属性名称找到对应的标签, 然后设置属性

格式:
[attribute]
作用:根据指定的属性名称找到对应的标签, 然后设置属性

[attribute=value]
作用: 找到有指定属性, 并且属性的取值等于value的标签, 然后设置属性
最常见的应用场景, 就是用于区分input属性
input[type=password]{}
<input type="text" name="" id="">
<input type="password" name="" id="">
-->
<p id="identity1">我是段落1</p>
<p id="identity2" class="cc">我是段落2</p>
<p class="cc">我是段落3</p>
<p id="identity3" class="para">我是段落4</p>
<p>我是段落5</p>

</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>21-属性选择器下</title>
    <style>
        /*
        img[alt^=abc]{
            color: red;
        }
        */
        /*
        img[alt|=abc]{
            color: red;
        }
        img[alt$=abc]{
            color: blue;
        }
        */
        /*
        img[alt*=abc]{
            color: red;
        }
        */
        img[alt~=abc]{
            color: red;
        }
    </style>
</head>
<body>
<!--
1.属性的取值是以什么开头的
[attribute|=value] CSS2
[attribute^=value] CSS3
两者之间的区别:
CSS2中的只能找到value开头,并且value是被-和其它内容隔开的
CSS3中的只要是以value开头的都可以找到, 无论有没有被-隔开

2.属性的取值是以什么结尾的
[attribute$=value] CSS3

3.属性的取值是否包含某个特定的值得
[attribute~=value] CSS2
[attribute*=value] CSS3
两者之间的区别:
CSS2中的只能找到独立的单词, 也就是包含value,并且value是被空格隔开的
CSS3中的只要包含value就可以找到
-->

<!--
<img src="" alt="abcdef">
<img src="" alt="abc-www">
<img src="" alt="abc ppp">
<img src="" alt="defabc">
<img src="" alt="ppp abc">
<img src="" alt="www-abc">
<img src="" alt="qq">
<img src="" alt="yy">
-->

<img src="" alt="abcwwwmmm">
<img src="" alt="wwwmmmabc">
<img src="" alt="wwwabcmmm">
<img src="" alt="www-abc-mmm">
<img src="" alt="www abc mmm">
<img src="" alt="qq">

</body>
</html>
```



## 通配符选择器

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>22-通配符选择器</title>
    <style>
        /*
        .cc{
            color: red;
        }
        */
        *{
            color: red;
        }
    </style>
</head>
<body>
<!--
1.什么是通配符选择器?
作用: 给当前界面上所有的标签设置属性

格式:
*{
    属性:值;
}

注意点:
由于通配符选择器是设置界面上所有的标签的属性, 所以在设置之前会遍历所有的标签, 如果当前界面上的标签比较多, 那么性能就会比较差, 所以在企业开发中一般不会使用通配符选择器
-->
<!--
<h1 class="cc">我是标题</h1>
<p class="cc">我是段落</p>
<a href="#" class="cc">我是超链接</a>
-->
<h1>我是标题</h1>
<p>我是段落</p>
<a href="#">我是超链接</a>

</body>
</html>
```



## css继承性

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>24-CSS三大特性之继承性</title>
    <style>
        div{
            color: red;
            text-decoration: none;
            font-size: 30px;
        }
    </style>
</head>
<body>
<!--
1.什么是继承性?
作用: 给父元素设置一些属性, 子元素也可以使用, 这个我们就称之为继承性

注意点:
1.并不是所有的属性都可以继承, 只有以color/font-/text-/line-开头的属性才可以继承
2.在CSS的继承中不仅仅是儿子可以继承, 只要是后代都可以继承
3.继承性中的特殊性
3.1a标签的文字颜色和下划线是不能继承的
3.2h标签的文字大小是不能继承的

应用场景:
一般用于设置网页上的一些共性信息, 例如网页的文字颜色, 字体,文字大小等内容
body{}
-->

<div>
    <p>我是段落</p>
</div>

<div>
    <ul>
        <li>
            <p>我是段落</p>
        </li>
    </ul>
</div>

<div>
    <a href="#">我是超链接</a>
</div>

<div>
    <h1>我是大标题</h1>
</div>
</body>
</html>
```



## 层叠性

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>25-CSS三大特性之层叠性</title>
    <style>
        p{
            color: red;
        }
        .para{
            color: blue;
        }
    </style>
</head>
<body>
<!--
1.什么是层叠性?
作用: 层叠性就是CSS处理冲突的一种能力

注意点:
层叠性只有在多个选择器选中"同一个标签", 然后又设置了"相同的属性", 才会发生层叠性

CSS全称 Cascading StyleSheet
-->
<p id="identity" class="para">我是段落</p>
</body>
</html>
```



## 优先级

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>26-CSS三大特性之优先级</title>
    <style>
        /*
        li{
            color: blue;
        }
        ul{
            color: red;
        }
        */
        /*
        p{
            color: blue;
        }
        p{
            color: red;
        }
        */
        #identity{
            color: purple;
        }
        .para{
            color: pink;
        }
        p{
            color: green;
        }
        *{
            color: blue;
        }
        li{
            color: red;
        }
    </style>
</head>
<body>
<!--
1.什么是优先级?
作用:当多个选择器选中同一个标签, 并且给同一个标签设置相同的属性时, 如何层叠就由优先级来确定

2.优先级判断的三种方式
2.1间接选中就是指继承
如果是间接选中, 那么就是谁离目标标签比较近就听谁的
2.2相同选择器(直接选中)
如果都是直接选中, 并且都是同类型的选择器, 那么就是谁写在后面就听谁的
2.3不同选择器(直接选中)
如果都是直接选中, 并且不是相同类型的选择器, 那么就会按照选择器的优先级来层叠
id>类>标签>通配符>继承>浏览器默认
-->
<ul>
    <li>
        <p id="identity" class="para">我是段落</p>
    </li>
</ul>
</body>
</html>
```



## 优先级之important

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>27-优先级之important</title>
    <style>
        #identity{
            color: purple;
            font-size: 50px;
        }
        .para{
            color: pink ;
        }
        p{
            color: green;
        }
        *{
            color: blue !important;
            font-size:10px;
        }
        li{
            color: red ;
        }
    </style>
</head>
<body>
<!--
1.什么是!important
作用: 用于提升某个直接选中标签的选择器中的某个属性的优先级的, 可以将被指定的属性的优先级提升为最高

注意点:
1.!important只能用于直接选中, 不能用于间接选中
2.通配符选择器选中的标签也是直接选中的
3.!important只能提升被指定的属性的优先级, 其它的属性的优先级不会被提升
4.!important必须写在属性值得分号前面
5.!important前面的感叹号不能省略

-->
<ul>
    <li>
        <p id="identity" class="para">我是段落</p>
    </li>
</ul>
</body>
</html>
```



## 优先级中的权重问题

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>28-优先级之权重问题</title>
    <style>
        /*
        #identity1 .box2{
            color: red;
        }
        .box1 .box2{
            color: green;
        }
        div ul li p{
            color: blue;
        }
        */
        /*
        .box1 .box2{
            color: blue;
        }
        div .box2{
            color: green;
        }
        */
        /*
        #identity1 ul li p{
            color: red;
        }
        #identity1 ul p{
            color: green;
        }
        */
        /*
        .box1 li #identity2{
            color: blue;
        }

        #identity1 ul .box2{
            color: red;
        }
        */

        .box2{
            color: red;
        }
        li{
            color: blue;
        }
    </style>
</head>
<body>
<!--
1.什么是优先级的权重?
作用: 当多个选择器混合在一起使用时, 我们可以通过计算权重来判断谁的优先级最高

2.权重的计算规则
2.1首先先计算选择器中有多少个id, id多的选择器优先级最高
2.2如果id的个数一样, 那么再看类名的个数, 类名个数多的优先级最高
2.3如果类名的个数一样, 那么再看标签名称的个数, 标签名称个数多的优先级最高
2.4如果id个数一样, 类名个数也一样, 标签名称个数也一样, 那么就不会继续往下计算了, 那么此时谁写在后面听谁的
也就是说优先级如果一样, 那么谁写在后面听谁的

注意点:
1.只有选择器是直接选中标签的才需要计算权重, 否则一定会听直接选中的选择器的
-->

<div id="identity1" class="box1">
    <ul>
        <li>
            <p id="identity2" class="box2">我是段落</p>
        </li>
    </ul>
</div>
</body>
</html>
```



## span和div标签

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>29-div和span标签</title>
    <style>
        .header{
            width: 980px;
            height: 100px;
            background: red;
            margin: auto;
            margin-bottom: 10px;
        }
        .content{
            width: 980px;
            height: 500px;
            background: green;
            margin: auto;
            margin-bottom: 10px;
        }
        .footer{
            width: 980px;
            height: 100px;
            background: blue;
            margin: auto;
        }
        .logo{
            width: 200px;
            height: 50px;
            background: pink;
            float: left;
            margin: 20px;
        }
        .nav{
            width: 600px;
            height: 50px;
            background: yellow;
            float: right;
            margin: 20px;
        }
        .aside{
            width: 250px;
            height: 460px;
            background: purple;
            float: left;
            margin: 20px;
        }
        .article{
            width: 650px;
            height: 460px;
            background: deepskyblue;
            float: right;
            margin: 20px;
        }
        span{
            color: red;
        }
    </style>
</head>
<body>
<!--
1.什么是div?
作用: 一般用于配合css完成网页的基本布局

2.什么是span?
作用: 一般用于配合css修改网页中的一些局部信息

3.div和span有什么区别?
1.div会单独的占领一行,而span不会单独占领一行
2.div是一个容器级的标签, 而span是一个文本级的标签

4.容器级的标签和文本级的标签的区别?
容器级的标签中可以嵌套其它所有的标签
文本级的标签中只能嵌套文字/图片/超链接

容器级的标签
div h ul ol dl li dt dd ...

文本级的标签
span p buis strong em ins del ...

注意点:
哪些标签是文本级的哪些标签是容器级的, 我们不用刻意去记忆, 在企业开发中一般情况下要嵌套都是嵌套在div中, 或者按照组标签来嵌套
-->
<!--
<div class="header">
    <div class="logo"></div>
    <div class="nav"></div>
</div>
<div class="content">
    <div class="aside"></div>
    <div class="article"></div>
</div>
<div class="footer"></div>
-->

<!--
<p>努力到<span>无能为力</span>, 拼搏到<span>感动自己</span></p>
-->

<!--
<div>我是div</div>
<div>我是div</div>

<span>我是span</span>
<span>我是span</span>
-->

<!--
<div>
    <div>
        <ul>
            <li>
                <img src="images/picture.jpg" alt="">
            </li>
        </ul>
    </div>
</div>
-->

<!--
<p>我是段落
    <h1>我是标题</h1>
</p>
-->

<h1>我是标题
    <p>我是段落</p>
</h1>
</body>
</html>
```



## css元素的显示模式

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>30-CSS元素的显示模式</title>
    <style>
        div{
            background: red;
            width: 200px;
            height: 200px;
        }
        span{
            background: blue;
            width: 200px;
            height: 200px;
        }
        img{
            width: 50px;
            height: 50px;
        }
    </style>
</head>
<body>
<!--
在HTML中HTML将所有的标签分为两类, 分别是容器级和文本级
在CSS中CSS也将所有的标签分为两类, 分别是块级元素和行内元素

1.什么是块级元素, 什么是行内元素?
块级元素会独占一行
行内元素不会独占一行

容器级的标签
div h ul ol dl li dt dd ...
文本级的标签
span p buis stong em ins del ...

块级元素
p div h ul ol dl li dt dd
行内元素
span buis strong em ins del

2.块级元素和行内元素的区别?
2.1块级元素
独占一行
如果没有设置宽度, 那么默认和父元素一样宽
如果设置了宽高, 那么就按照设置的来显示

2.2行内元素
不会独占一行
如果没有设置宽度, 那么默认和内容一样宽
行内元素是不可以设置宽度和高度的

2.3行内块级元素
为了能够让元素既能够不独占一行, 又可以设置宽度和高度, 那么就出现了行内块级元素
-->

<div>我是div</div>
<p>我是段落</p>
<h1>我是标题</h1>
<hr>
<span>我是span</span>
<b>我是加粗</b>
<strong>我是强调</strong>
<hr>
<img src="images/picture.jpg" alt="">
<img src="images/picture.jpg" alt="">
</body>
</html>
```



## 背景颜色

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>33-背景颜色</title>
    <style>
        div{
            width: 500px;
            height: 500px;
        }
        .box1{
            background-color: red;
        }
        .box2{
            background-color: rgb(0,255,0);
        }
        .box3{
            background-color: rgba(0,0,255,0.7);
        }
        .box4{
            background-color: #0ff;
        }
    </style>
</head>
<body>
<!--
1.如何设置标签的背景颜色?
在CSS中有一个background-color:属性, 就是专门用来设置标签的背景颜色的

取值:
具体单词
rgb
rgba
十六进制

快捷键:
bc background-color: #fff;

-->
<div class="box1"></div>
<div class="box2"></div>
<div class="box3"></div>
<div class="box4"></div>
</body>
</html>
```



## 背景图片

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>34-背景图片</title>
    <style>
        div{
            width: 500px;
            height: 500px;
        }
        .box1{
            background-image: url(images/girl.jpg);
            /*background-image: url(http://img4.imgtn.bdimg.com/it/u=2278032206,4196312526&fm=21&gp=0.jpg);*/
        }
    </style>
</head>
<body>
<!--
1.如何设置背景图片?
在CSS中有一个叫做background-image: url();的属性, 就是专门用于设置背景图片的

快捷键:
bi background-image: url();

注意点:
1.图片的地址必须放在url()中, 图片的地址可以是本地的地址, 也可以是网络的地址
2.如果图片的大小没有标签的大小大, 那么会自动在水平和垂直方向平铺来填充
3.如果网页上出现了图片, 那么浏览器会再次发送请求获取图片
-->
<div class="box1"></div>
</body>
</html>
```



## 背景平铺属性

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>35-背景平铺属性</title>
    <style>
        /*
        div{
            width: 500px;
            height: 500px;
        }
        .box1{
            background-image: url(images/girl.jpg);
            background-repeat: repeat-y;
        }
        */
        /*
        body{
            background-image: url(images/bg2.jpg);
        }
        */
        div{
            width: 980px;
            height: 60px;
            background-image: url(images/1.png);
        }
    </style>
</head>
<body>
<!--
1.如何控制背景图片的平铺方式?
在CSS中有一个background-repeat属性, 就是专门用于控制背景图片的平铺方式的

取值:
repeat 默认, 在水平和垂直都需要平铺
no-repeat 在水平和垂直都不需要平铺
repeat-x 只在水平方向平铺
repeat-y 只在垂直方向平铺

快捷键
bgr background-repeat:

应用场景:
可以通过背景图片的平铺来降低图片的大小, 提升网页的访问速度
-->
<div class="box1"></div>
</body>
</html>
```



## 背景定位属性

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>36-背景定位属性</title>
    <style>
        div{
            /*width: 500px;*/
            height: 500px;
        }
        .box1{
            /*background-color: red;*/
            /*background-image: url(images/girl.jpg);*/
            /*background-repeat: no-repeat;*/
            /*background-position: left top;*/
            /*background-position: right top;*/
            /*background-position: right bottom;*/
            /*background-position: left bottom;*/
            /*background-position: center center;*/
            /*background-position: left center;*/
            /*background-position: center top;*/
            /*background-position: 100px 0;*/
            /*background-position: 100px 200px;*/
            background-position: -100px -100px;
            background-image: url(images/yxlm.jpg);
            /*background-position: center top;*/
        }
    </style>
</head>
<body>
<!--
1.如何控制背景图片的位置?
在CSS中有一个叫做background-position:属性, 就是专门用于控制背景图片的位置

2.格式:
background-position: 水平方向 垂直方向;

3.取值
2.1具体的方位名词
水平方向: left center right
垂直方向: top center bottom

2.2具体的像素
例如: background-position: 100px 200px;
记住一定要写单位, 也就是一定要写px
记住具体的像素是可以接收负数的

快捷键:
bp background-position: 0 0;

注意点:
同一个标签可以同时设置背景颜色和背景图片, 如果颜色和图片同时存在, 那么图片会覆盖颜色
-->

<div class="box1">
    <!--<img src="images/yxlm.jpg" alt="">-->
</div>
</body>
</html>
```



## 背景关联方式和背景缩写

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>37-背景缩写</title>
    <style>
        div{
            /*width: 500px;*/
            /*height: 500px;*/
            /*
            background-color: red;
            background-image: url(images/girl.jpg);
            background-repeat: no-repeat;
            background-position: left bottom;
            */
            /*background: red url(images/girl.jpg) no-repeat left bottom;*/
            /*background: red;*/
            /*background: url(images/girl.jpg);*/
        }

        body{
            background-image: url(images/girl.jpg);
            background-repeat: no-repeat;
            /*background-attachment: scroll;*/
            background-attachment:fixed;
        }
    </style>
</head>
<body>
<!--
1.背景属性缩写的格式
background: 背景颜色 背景图片 平铺方式 关联方式 定位方式;

快捷键:
bg+ background: #fff url() 0 0 no-repeat;

2.注意点：
background属性中， 任何一个属性都可以被省略

3.什么是背景关联方式？
默认情况下背景图片会随着滚动条的滚动而滚动， 如果不想让背景图片随着滚动条的滚动而滚动， 那么我们就可以修改背景图片和滚动条的关联方式

4.如何修改背景关联方式？
在CSS中有一个叫做background-attachment的属性， 这个属性就是专门用于修改关联方式的

格式
background-attachment：scroll;

取值：
scroll 默认值， 会随着滚动条的滚动而滚动
fixed 不会随着滚动条的滚动而滚动

快捷键:
ba background-attachment:;
-->

<div></div>
我是文字
</body>
</html>
```



## 背景图片和插入图片的区别

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>38-背景图片和插入图片区别</title>
    <style>
        div{
            /*width: 150px;*/
            /*height: 170px;*/
            width: 300px;
            height: 300px;
            background-color: red;
        }
        .box1{
            background-image: url(images/girl.jpg);
            background-repeat:no-repeat;
            background-position: right bottom;
        }
    </style>
</head>
<body>
<!--
1.背景图片和插入图片区别?
1.1
背景图片仅仅是一个装饰, 不会占用位置
插入图片会占用位置

1.2
背景图片有定位属性, 所以可以很方便的控制图片的位置
插入图片没有定位属性, 所有控制图片的位置不太方便

1.3
插入图片的语义比背景图片的语义要强, 所以在企业开发中如果你的图片想被搜索引擎收录, 那么推荐使用插入图片
-->
<div class="box1">我是文字</div>
<div class="box2">
    <img src="images/girl.jpg" alt="">
    我是文字
</div>
</body>
</html>
```



## 背景图片练习

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>39-背景图片练习</title>
    <style>
        .box1{
            width: 1024px;
            height: 768px;
            background-image: url(images/bybg.jpg);
        }
        .box2{
            /*background-color: red;*/
            width: 1024px;
            height: 768px;
            background-image: url(images/bybottom.png);
            background-position: center bottom;
            background-repeat:no-repeat;
            /*background-position: 100px 200px;*/
        }
    </style>
</head>
<body>
<div class="box1">
    <div class="box2"></div>
</div>
</body>
</html>
```



## css精灵图

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>40-CSS精灵图</title>
    <style>
        .box{
            width: 86px;
            height: 28px;
            background-image: url(images/weibo.png);
            background-position: -425px -200px;
        }
    </style>
</head>
<body>
<!--
1.什么是CSS精灵图
CSS精灵图是一种图像合成技术

2.CSS精灵图作用
可以减少请求的次数, 以及可以降低服务器处理压力

3.如何使用CSS精灵图
CSS的精灵图需要配合背景图片和背景定位来使用
-->
<div class="box"></div>
</body>
</html>
```



## 精灵图练习

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>41-CSS精灵图练习</title>
    <style>
        div{
            background-image: url(images/a-z.jpg);
            display: inline-block;
        }
        .box1{
            width: 110px;
            height: 143px;
            background-position: -560px -216px;
        }
        .box2{
            width: 127px;
            height: 140px;
            background-position: -834px -217px;
        }
        .box3{
            width: 124px;
            height: 144px;
            background-position: -264px -215px;
        }

    </style>
</head>
<body>
<div class="box1"></div>
<div class="box2"></div>
<div class="box3"></div>
</body>
</html>
```



## 边框属性

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>42-边框属性上</title>
    <style>
        .box{
            width: 100px;
            height: 100px;
            background-color: red;
            /*border: 5px solid blue;*/
            /*border: 5px solid;*/
            /*border: 5px blue;*/
            /*border: solid blue;*/

            border-top:5px solid blue;
            border-right:10px dashed green;
            border-bottom:15px dotted purple;
            border-left:20px double pink;
        }
    </style>
</head>
<body>
<!--
1.什么边框?
边框就是环绕在标签宽度和高度周围的线条

2.边框属性的格式
2.1连写(同时设置四条边的边框)
border: 边框的宽度 边框的样式 边框的颜色;

快捷键:
bd+ border: 1px solid #000;

注意点:
1.连写格式中颜色属性可以省略, 省略之后默认就是黑色
2.连写格式中样式不能省略, 省略之后就看不到边框了
3.连写格式中宽度可以省略, 省略之后还是可以看到边框

2.2连写(分别设置四条边的边框)
border-top: 边框的宽度 边框的样式 边框的颜色;
border-right: 边框的宽度 边框的样式 边框的颜色;
border-bottom: 边框的宽度 边框的样式 边框的颜色;
border-left: 边框的宽度 边框的样式 边框的颜色;

快捷键:
bt+ border-top: 1px solid #000;
br+
bb+
bl+
-->
<div class="box"></div>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>43-边框属性下</title>
    <style>
        .box{
            width: 500px;
            height: 500px;
            background-color: red;
            /*border-width: 5px 10px 15px 20px;*/
            /*border-style: solid dashed dotted double;*/
            /*border-color: blue green purple pink;*/
            /*border-color: blue green purple;*/
            /*border-color: blue green;*/
            /*border-color: blue;*/

            border-top-width: 5px;
            border-top-style: solid;
            border-top-color: blue;

            border-right-width: 10px;
            border-right-style: dashed;
            border-right-color: green;

            border-bottom-width: 15px;
            border-bottom-style: dotted;
            border-bottom-color: purple;

            border-left-width: 20px;
            border-left-style: double;
            border-left-color: pink;
        }
    </style>
</head>
<body>
<!--
2.3连写(分别设置四条边的边框)
border-width: 上 右 下 左;
border-style: 上 右 下 左;
border-color: 上 右 下 左;

注意点:
1.这三个属性的取值是按照顺时针来赋值, 也就是按照上右下左来赋值, 而不是按照日常生活中的上下左右
2.这三个属性的取值省略时的规律
2.1上 右 下 左 > 上 右 下 > 左边的取值和右边的一样
2.2上 右 下 左 > 上 右 > 左边的取值和右边的一样 下边的取值和上边一样
2.3上 右 下 左 > 上 > 右下左边取值和上边一样


3.非连写(方向+要素)
border-left-width: 20px;
border-left-style: double;
border-left-color: pink;
-->
<div class="box"></div>
</body>
</html>
```



## 内边距属性

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>45-内边距属性</title>
    <style>
        div{
            width: 98px;
            height: 90px;
            border: 1px solid #000;
            background-color: red;
        }
        .box1{
            padding-top: 20px;
        }
        .box2{
            padding-right:40px;
        }
        .box3{
            padding-bottom:80px;
        }
        .box4{
            padding-left:160px;
        }
        .box5{
            /*padding:20px 40px 80px 160px;*/
            /*padding:20px 40px 80px;*/
            /*padding:20px 40px;*/
            padding:20px;
        }
    </style>
</head>
<body>
<!--
1.什么是内边距?
边框和内容之间的距离就是内边距

2.格式
2.1非连写
padding-top: ;
padding-right: ;
padding-bottom: ;
padding-left: ;

2.2连写
padding: 上 右 下 左;

2.这三个属性的取值省略时的规律
2.1上 右 下 左 > 上 右 下 > 左边的取值和右边的一样
2.2上 右 下 左 > 上 右 > 左边的取值和右边的一样 下边的取值和上边一样
2.3上 右 下 左 > 上 > 右下左边取值和上边一样

注意点:
1.给标签设置内边距之后, 标签占有的宽度和高度会发生变化
2.给标签设置内边距之后, 内边距也会有背景颜色

-->
<div class="box1">我是文字我是文字我是文字我是文字我是文字我是文字我是文字</div>
<hr>
<div class="box2">我是文字我是文字我是文字我是文字我是文字我是文字我是文字</div>
<hr>
<div class="box3">我是文字我是文字我是文字我是文字我是文字我是文字我是文字</div>
<hr>
<div class="box4">我是文字我是文字我是文字我是文字我是文字我是文字我是文字</div>
<hr>
<div class="box5">我是文字我是文字我是文字我是文字我是文字我是文字我是文字</div>
</body>
</html>
```



## 外边距属性

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>46-外边距属性</title>
    <style>
        *{
            padding:0;
            margin:0;
        }
        span{
            display: inline-block;
            width: 100px;
            height: 100px;
            border: 1px solid #000;
            background-color: red;
        }
        div{
            height: 100px;
            border: 1px solid #000;
        }
        .box1{
            /*
            margin-top:20px;
            margin-right:40px;
            margin-bottom:80px;
            margin-left:160px;
            */
            /*margin:20px 40px 80px 160px;*/
            /*margin:20px 40px 80px;*/
            /*margin:20px 40px;*/
            margin:20px;
        }

    </style>
</head>
<body>
<!--
1.什么是外边距?
标签和标签之间的距离就是外边距

2.格式
2.1非连写
margin-top: ;
margin-right: ;
margin-bottom: ;
margin-left: ;

2.2连写
margin: 上 右 下 左;

2.这三个属性的取值省略时的规律
2.1上 右 下 左 > 上 右 下 > 左边的取值和右边的一样
2.2上 右 下 左 > 上 右 > 左边的取值和右边的一样 下边的取值和上边一样
2.3上 右 下 左 > 上 > 右下左边取值和上边一样

注意点:
外边距的那一部分是没有背景颜色的
-->
<span class="box1">我是span</span><span class="box2">我是span</span><span class="box3">我是span</span><div class="box4"></div>

</body>
</html>
```



## 垂直方向外边距的合并现象

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>47-外边距合并现象</title>
    <style>
        span{
            display: inline-block;
            width: 100px;
            height: 100px;
            border: 1px solid #000;
        }
        div{
            height: 100px;
            border: 1px solid #000;
        }
        .hezi1{
            margin-right:50px;
        }
        .hezi2{
            margin-left:100px;
        }
        .box1{
            margin-bottom:50px;
        }
        .box2{
            margin-top:100px;
        }
    </style>
</head>
<body>
<!--
在默认布局的垂直方向上, 默认情况下外边距是不会叠加的, 会出现合并现象, 谁的外边距比较大就听谁的
-->
<span class="hezi1">我是span</span><span class="hezi2">我是span</span>
<div class="box1">我是div</div>
<div class="box2">我是div</div>
</body>
</html>
```



## css盒子模型

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>48-CSS盒子模型</title>
    <style>
        span,a,b,strong{
            display: inline-block;
            width: 100px;
            height: 100px;
            border: 6px solid #000;
            padding: 20px;
            margin: 20px;
        }

    </style>
</head>
<body>
<!--
1.什么是CSS盒子模型?
CSS盒子模型仅仅是一个形象的比喻, HTML中所有的标签都是盒子

结论
1.在HTML中所有的标签都可以设置
宽度/高度  == 指定可以存放内容的区域
内边距  == 填充物
边框  == 手机盒子自己
外边距  == 盒子和盒子之间的间隙
-->
<span>我是span</span>
<a href="#">我是超链接</a>
<b>我是加粗</b>
<strong>我是强调</strong>

</body>
</html>
```



## 盒子模型的宽度和高度

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>49-盒子模型宽度和高度</title>
</head>
<body>
<!--
1.内容的宽度和高度
就是通过width/height属性设置的宽度和高度

2.元素的宽度和高度
宽度 = 左边框 + 左内边距 + width + 右内边距 + 右边框
高度 同理可证

3.元素空间的宽度和高度
宽度 = 左外边距 + 左边框 + 左内边距 + width + 右内边距 + 右边框 + 右外边距
高度 同理可证
-->
</body>
</html>
```



## 盒子模型宽高练习

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>50-盒子宽高练习</title>
    <style>
        /*判断是否是内容宽高为100的盒子
        只需要看width/height是否为100即可*/
        .box1{
            width:50px;
            height: 50px;
            padding: 25px;
            background-color: green;
        }
        .box2{
            width: 96px;
            height: 96px;
            border: 2px solid #000;
        }
        .box3{
            width: 100px;
            height: 100px;
            background-color: yellow;
        }
        /*判断是否是元素宽高为200的盒子
        只需要看 边框 + 内边距 + 内容宽度/内容高度 时候等于200即可*/
        .box4{
            width: 100px;
            height: 100px;
            margin: 50px;
            background-color: red;
        }
        .box5{
            /*25+25+150 = 200*/
            width: 150px;
            height: 150px;
            border: 25px solid #000;
        }
        .box6{
            /*25+25+150 = 200*/
            /*5+5+20+20+150=200*/
            width: 150px;
            height: 150px;
            /*padding: 25px;*/
            padding:20px;
            border: 5px solid #000;
            background-color: blue;
        }
        /*判断是否是元素空间宽高为300的盒子
        只需要看 外边距+边框+内边距+内容宽度/内容高度*/
        .box7{
            width: 300px;
            height: 300px;
            background-color: purple;
        }
        .box8{
            /*
            50+50+25+25+25+25+100
            100 +50 +50 + 100
            300
            */
            width: 100px;
            height: 100px;
            padding: 25px;
            border: 25px solid #000;
            margin: 50px;
        }
        /*现有如下盒子模型, 要求增加padding属性为25之后仍然保持元素宽高为200

        元素宽高 = 边框 + 内边距 + 内容宽高
                 = 0 + 0 + 200 = 200
                 = 0 + 25 + 25 + 200 = 250
                 = 0 + 25 + 25 + 150 = 200
         规律:
         1.增加了padding之后元素的宽高也会发生变化
         2.如果增加了padding之后还想保持元素的宽高, 那么就必须减去内容的宽高
        */
        .box9{
            width: 150px;
            height: 150px;
            padding: 25px;
            background-color: red;
        }
        /*现有如下盒子模型, 要求增加border属性为20之后仍然保持元素宽高为200
        元素宽高 = 边框 + 内边距 + 内容宽高
                 = 0 + 0 + 200 = 200
                 = 0 + 20 + 20 + 200 = 240
                 = 0 + 20 + 20 + 160 = 200
          规律:
         1.增加了border之后元素的宽高也会发生变化
         2.如果增加了border之后还想保持元素的宽高, 那么就必须减去内容的宽高
        */
        .box10{
            width: 160px;
            height: 160px;
            border: 20px solid #000;
            background-color: deepskyblue;
        }
    </style>
</head>
<body>
<!--<div class="box1">box1</div>-->
<!--<div class="box2">box2</div>-->
<!--<div class="box3">box3</div>-->
<!--<hr>-->
<!--<div class="box4">box4</div>-->
<!--<div class="box5">box5</div>-->
<!--<div class="box6">box6</div>-->
<!--<hr>-->
<!--<div class="box7">box7</div>-->
<!--<div class="box8">box8</div>-->
<!--<hr>-->
<div class="box9">box9</div>
<div class="box10">box10</div>
</body>
</html>
```



## box-sizing属性

```html
<!--<!DOCTYPE html>-->
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>51-盒子box-sizing属性</title>
    <style>
        .content{
            width: 300px;
            height: 300px;
            background-color: red;
        }
        .aside{
            width: 100px;
            height: 200px;
            background-color: green;
            float: left;
        }
        .article{
            /*
            1.CSS3中新增了一个box-sizing属性, 这个属性可以保证我们给盒子新增padding和border之后, 盒子元素的宽度和高度不变
            2.box-sizing属性是如何保证增加padding和border之后, 盒子元素的宽度和高度不变
            和我们前面学习的原理一样, 增加padding和border之后要想保证盒子元素的宽高不变, 那么就必须减去一部分内容的宽度和高度
            3.box-sizing取值
            3.1content-box
            元素的宽高 = 边框 + 内边距 + 内容宽高
            3.2border-box
            元素的宽高 = width/height的宽高
            */
            box-sizing: border-box;
            width: 200px;
            height: 200px;
            background-color: blue;
            float: right;
            border: 20px solid #000;
            padding: 20px;
        }
    </style>
</head>
<body>

<div class="content">
    <div class="aside"></div>
    <div class="article"></div>
</div>

</body>
</html>
```



## 盒子居中和内容居中

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>53-盒子居中和内容居中</title>
    <style>
        .father{
            width: 800px;
            height: 500px;
            background-color: red;
            /*text-align: center;*/
            margin:0 auto;
        }
        .son{
            width: 100px;
            height: 100px;
            background-color: blue;
        }
    </style>
</head>
<body>
<!--
1.text-align:center;和margin:0 auto;区别
text-align: center;作用
设置盒子中存储的文字/图片水平居中

margin:0 auto;作用
让盒子自己水平居中
-->
<div class="father">
    我是文字<br/>
    <img src="images/girl.jpg" alt="">
    <!--<div class="son"></div>-->
</div>
</body>
</html>
```



## 清空默认边距

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>54-清空默认边距</title>
    <style>
        /*
        *{
            margin: 0;
            padding: 0;
        }
        */
        body,div,dl,dt,dd,ul,ol,li,h1,h2,h3,h4,h5,h6,pre,code,form,fieldset,legend,input,textarea,p,blockquote,th,td{
            margin:0;padding:0
        }
        p{
            width: 610px;
            height: 110px;
            background-color: #cdcdcd;
            border: 1px solid #000000;
        }
    </style>
</head>
<body>
<!--
1.为什么要清空默认边距(外边距和内边距)
在企业开发中为了更好的控制盒子的宽高和计算盒子的宽高等等, 所以在企业开发中, 编写代码之前第一件事情就是清空默认的边距

2.如何清空默认的边距
格式
*{
            margin: 0;
            padding: 0;
}

3.注意点
通配符选择器会找到(遍历)当前界面中所有的标签, 所以性能不好
企业开发中可以从这个网址中拷贝
http://yui.yahooapis.com/3.18.1/build/cssreset/cssreset-min.css

-->
<p>葬爱:非主流文化的常用词，是当今网络流行术语.且流行于非主流杀马特之中。葬，即埋葬，爱，即爱情，翻译成外语就bury love</p>
</body>
</html>
```



## 行高和字号

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>54-清空默认边距</title>
    <style>
        body,div,dl,dt,dd,ul,ol,li,h1,h2,h3,h4,h5,h6,pre,code,form,fieldset,legend,input,textarea,p,blockquote,th,td{
            margin:0;padding:0
        }
        p{
            width: 610px;
            height: 110px;
            background-color: #cdcdcd;
            border: 1px solid #000000;
        }
        div{
            box-sizing: border-box;
            width: 100px;
            height: 80px;
            border: 1px solid #000;
            line-height: 20px;
            /*line-height: 80px;*/
            padding-top:20px;
            padding-bottom:20px;
        }
    </style>
</head>
<body>
<!--
1.什么是行高?
在CSS中所有的行都有自己的行高

注意点:
行高和盒子高不是同一个概念
行高指的是每行内容的高度
盒子高指的是元素的高度

规律:
1.文字在行高中默认是垂直居中的

2.在企业开发中我们经常将盒子的高度和行高设置为一样, 那么这样就可以保证一行文字在盒子的高度中是垂直居中的
简而言之就是: 要想一行文字在盒子中垂直居中, 那么只需要设置这行文字的"行高等于盒子的高"即可

3.在企业开发中如果一个盒子中有多行文字, 那么我们就不能使用设置行高等于盒子高来实现让文字垂直居中, 只能通过设置padding来让文字居中
-->

<!--<p>葬爱:非主流文化的常用词，是当今网络流行术语.且流行于非主流杀马特之中。葬，即埋葬，爱，即爱情，翻译成外语就bury love</p>-->
<!--<div>我是文字我是文字我是文字</div>-->
<!--<div>我是文字</div>-->
<div>我是文字我是文字我是文字</div>
</body>
</html>
```



## 还原字体和字号

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>54-清空默认边距</title>
    <style>
        body,div,dl,dt,dd,ul,ol,li,h1,h2,h3,h4,h5,h6,pre,code,form,fieldset,legend,input,textarea,p,blockquote,th,td{
            margin:0;padding:0
        }
        p{
            box-sizing: border-box;
            width: 610px;
            height: 110px;
            background-color: #cdcdcd;
            border: 1px solid #000000;
            font-family:"黑体";
            font-size: 20px;
            line-height: 40px;
            color: #67676d;
            padding-left: 10px;
            padding-right: 10px;
            padding-top: 10px;
        }
    </style>
</head>
<body>
<!--
注意点:
1.在企业开发中, 如果一个盒子中存储的是文字, 那么一般情况下我们会以盒子左边的内边距为基准, 不会以右边的内边距为基准, 因为这个右边的内边距有误差

2.右边内边距的误差从何而来? 因为右边如果放不下一个文字, 那么文字就会换行显示, 所以文字和内边距之间的距离就有了误差

3.顶部的内边距并不是边框到文字顶部的距离, 而是边框到行高顶部的距离
-->

<p>葬爱:非主流文化的常用词，是当今网络流行术语.且流行于非主流杀马特之中。葬，即埋葬，爱，即爱情，翻译成外语就bury love</p>

</body>
</html>
```



## 文章界面

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>57-文章界面</title>
    <style>
        body,div,dl,dt,dd,ul,ol,li,h1,h2,h3,h4,h5,h6,pre,code,form,fieldset,legend,input,textarea,p,blockquote,th,td{
            margin:0;padding:0
        }
        body{
            background-color: #efefef;
        }
        div{
            margin:0 auto;
            box-sizing: border-box;
            width: 372px;
            height: 232px;
            border: 1px solid #666;
            padding: 15px;
        }
        h1{
            font-family:"微软雅黑";
            font-size: 18px;
            border-bottom: 1px solid #666;
            padding-bottom: 10px;
        }
        span{
            font-size: 14px;
        }
        ul{
            list-style: none;
            margin-top: 10px;
        }
        ul li{
            line-height: 30px;
            border-bottom: 1px dashed #666;
            font-family:"微软雅黑";
            font-size: 12px;
            color: #242424;
            padding-left: 20px;
        }
    </style>
</head>
<body>
<!--
1.清空所有的边距
2.从外向内, 从上至下的编写网页
-->
<div>
    <h1>最新文章<span>/New Articles</span></h1>
    <ul>
        <li>问中国：经济新活力篇 治国理政 长征</li>
        <li>问中国:民生保障不是"清谈馆"国平 国庆节</li>
        <li>李克强不忘棚户区改造10日至12日将视察澳门
        </li>
        <li>王岐山监督中央部委五大抓手:关键少数要加码</li>
        <li>李南江老师免费公开H5+跨平台开发教学视频</li>
    </ul>
</div>
</body>
</html>
```



## 网页的布局方式

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>58-网页的布局方式</title>
    <style>
        /*
        div,h1,p{
            border: 1px solid #000;
        }
        span,strong,b{
            border: 1px solid #000;
        }
        */
        *{
            margin: 0;
            padding: 0;
        }
        .box1{
            width: 100px;
            height: 100px;
            background-color: red;
            /*display: inline-block;*/
            float: left;
        }
        .box2{
            width: 100px;
            height: 100px;
            background-color: blue;
            /*display: inline-block;*/
            /*margin-left:930px;*/
            float: right;
        }
    </style>
</head>
<body>
<!--
1.什么是网页的布局方式?
网页的布局方式其实就是指浏览器是如何对网页中的元素进行排版的

1.标准流(文档流/普通流)排版方式
1.1其实浏览器默认的排版方式就是标准流的排版方式
1.2在CSS中将元素分为三类, 分别是块级元素/行内元素/行内块级元素
1.3 在标准流中有两种排版方式, 一种是垂直排版, 一种是水平排版
垂直排版, 如果元素是块级元素, 那么就会垂直排版
水平排版, 如果元素是行内元素/行内块级元素, 那么就会水平排版

2.浮动流排版方式
2.1浮动流是一种"半脱离标准流"的排版方式
2.2浮动流只有一种排版方式, 就是水平排版. 它只能设置某个元素左对齐或者右对齐

注意点:
1.浮动流中没有居中对齐, 也就是没有center这个取值
2.在浮动流中是不可以使用margin: 0 auto;

特点:
1.在浮动流中是不区分块级元素/行内元素/行内块级元素的
无论是级元素/行内元素/行内块级元素都可以水平排版
2.在浮动流中无论是块级元素/行内元素/行内块级元素都可以设置宽高
3.综上所述, 浮动流中的元素和标准流中的行内块级元素很像

3.定位流排版方式
-->
<!--
<div>我是div</div>
<h1>我是标题</h1>
<p>我是段落</p>
-->
<!--
<span>我是span</span>
<strong>我是强调</strong>
<b>我是加粗</b>
-->
<span class="box1"></span>
<span class="box2"></span>
</body>
</html>
```



## 浮动元素的脱标

```html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>59-浮动元素的脱标</title>
    <style>
        .box1{
            float: left;
            width: 100px;
            height: 100px;
            background-color: red;
        }
        .box2{
            width: 150px;
            height: 150px;
            background-color: blue;
        }
    </style>
</head>
<body>
<!--
1.什么是浮动元素的脱标?
脱标: 脱离标准流
当某一个元素浮动之后, 那么这个元素看上去就像被从标准流中删除了一样, 这个就是浮动元素的脱标

2.浮动元素脱标之后会有什么影响?
如果前面一个元素浮动了, 而后面一个元素没有浮动 , 那么这个时候前面一个元就会盖住后面一个元素
-->
<div class="box1"></div>
<div class="box2"></div>
</body>
</html>
```



## 浮动元素的排序规则

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>60-浮动元素排序规则</title>
    <style>
        .box1{
            float: left;
            width: 50px;
            height: 50px;
            background-color: red;
        }
        .box2{
            width: 100px;
            height: 100px;
            background-color: pink;
        }
        .box3{
            float: left;
            width: 150px;
            height: 150px;
            background-color: yellow;
        }
        .box4{
            float: left;
            width: 200px;
            height: 200px;
            background-color: tomato;
        }
    </style>
</head>
<body>
<!--
1.浮动元素排序规则
1.1相同方向上的浮动元素, 先浮动的元素会显示在前面, 后浮动的元素会显示在后面
1.2不同方向上的浮动元素, 左浮动会找左浮动, 右浮动会找右浮动
1.3浮动元素浮动之后的位置, 由浮动元素浮动之前在标准流中的位置来确定
-->
<div class="box1">1</div>
<div class="box2">2</div>
<div class="box3">3</div>
<div class="box4">4</div>
</body>
</html>
```



## 浮动元素的贴靠现象

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>61-浮动元素贴靠现象</title>
    <style>
        .father{
            width: 400px;
            height: 400px;
            border: 1px solid #000;
        }
        .box1{
            float: left;
            width: 50px;
            height: 300px;
            background-color: red;
        }
        .box2{
            float: left;
            width: 50px;
            height: 100px;
            background-color: green;
        }
        .box3{
            float: left;
            width: 350px;
            height: 100px;
            background-color: blue;
        }
    </style>
</head>
<body>
<!--
1.什么是浮动元素贴靠现象?
1.如果父元素的宽度能够显示所有浮动元素, 那么浮动的元素会并排显示
2.如果父元素的宽度不能显示所有浮动元素, 那么会从最后一个元开始往前贴靠
3.如果贴靠了前面所有浮动元素之后都不能显示, 最终会贴靠到父元素的左边或者右边
-->
<div class="father">
    <div class="box1"></div>
    <div class="box2"></div>
    <div class="box3"></div>
</div>
</body>
</html>
```



## 浮动元素的自围现象

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>62-浮动元素字围现象</title>
    <style>
        div{
            float: left;
            width: 100px;
            height: 100px;
            /*background-color: red;*/
            border: 1px solid #000;
        }
        p{
            width: 500px;
            height: 500px;
            background-color: yellow;
        }
        img{
            float: left;
        }
    </style>
</head>
<body>
<!--
1.什么是浮动元素字围现象?
浮动元素不会挡住没有浮动元素中的文字, 没有浮动的文字会自动给浮动的元素让位置,这个就是浮动元素字围现象
-->

<!--<div></div>-->
<img src="images/girl.jpg" alt="">
<p>1999年-2002年范冰冰一共出演了《青春出动》、《小李飞刀》、《秦始皇》、《中关村风云》、《少年包青天2》《尘埃落定》等十七部电视剧[12]  ，以及《河东狮吼》等三部电影[13]  。
    2003年在由梁羽生小说改编的电视剧《萍踪侠影》中饰演女主角云蕾。同年在冯小刚执导的贺岁档电影《手机》中饰演女主角武月[14]  。
    2004年凭借电影《手机》获得第27届大众电影百花奖最佳女主角奖，同时《手机》也获得大众电影百花奖最佳故事片奖。[2]  9月，出演根据古龙小说改编的古装剧《小鱼儿与花无缺》，饰演女主角铁心兰[15]  。同年出演《大唐芙蓉园》中的杨玉环等五部电视剧，以及电影《千机变2》和《情癫大圣》[16]  。
    2005年发行首张个人专辑《刚刚开始》，这张处女大碟由圈内多位音乐人联袂制作，包含了多种风格迥异的音乐元素。[3]  同年主演由张之亮执导的古装片《墨攻》，饰演女主角逸悦。[17]
    2006年出演电视剧《封神榜之凤鸣岐山》，饰演女主角苏妲己。之后接连拍摄《苹果》、《导火线》、《心中有鬼》等六部电影[18]  。
    2007年2月，主演的电影《苹果》入围第57届柏林国际电影节主
    范冰冰
    范冰冰(23张)
    竞赛单元，导演李玉，女主演范冰冰，男主演佟大为共同出席本届电影节。[19]  6月，范冰冰与华谊约满，自组工作室，投资拍摄民国剧《胭脂雪》，并首次担当制片人，同时饰演女主角文玉禾[20]  。10月，凭借电影《苹果》获得第四届欧亚国际电影节最佳女演员奖。[21]  同年出演《合约情人》、《精舞门》、《新宿事件》等五部电影[22]  。同年凭借电影《心中有鬼》获得第44届台湾电影金马奖最佳女配角[4]  。</p>

</body>
</html>
```



## 浮动练习

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>63-浮动练习</title>
    <style>
        body,div,dl,dt,dd,ul,ol,li,h1,h2,h3,h4,h5,h6,pre,code,form,fieldset,legend,input,textarea,p,blockquote,th,td{
            margin:0;padding:0
        }
        .header{
            width: 980px;
            height: 100px;
            /*background-color: red;*/
            margin:0 auto;
        }
        .header .logo{
            width: 250px;
            height: 100px;
            background-color: pink;
            float: left;
        }
        .header .language{
            width: 150px;
            height: 50px;
            background-color: skyblue;
            float: right;
        }
        .header .nav{
            width: 650px;
            height: 50px;
            background-color: purple;
            float: right;
        }
        .content{
            width: 980px;
            height: 400px;
            /*background-color: green;*/
            margin:0 auto;
            margin-top: 10px;
        }
        .content .aside{
            width: 320px;
            height: 400px;
            background-color: yellow;
            float: left;
        }
        .content .article{
            width: 650px;
            height: 400px;
            /*background-color: pink;*/
            float: right;
        }
        .content .article .articleTop{
            width: 650px;
            height: 350px;
            /*background-color: blue;*/
        }
        .content .article .articleTop .articleTopLeft{
            width: 400px;
            height: 350px;
            /*background-color: red;*/
            float: left;
        }
        .content .article .articleTop .articleTopLeft .new1{
            width: 400px;
            height: 200px;
            background-color: deepskyblue;
        }
        .content .article .articleTop .articleTopLeft .new2{
            width: 400px;
            height: 140px;
            background-color: tomato;
            margin-top: 10px;
        }
        .content .article .articleTop .articleTopRight{
            width: 240px;
            height: 350px;
            background-color: green;
            float: right;
        }
        .content .article .articleBottom{
            width: 650px;
            height: 40px;
            background-color: darkgoldenrod;
            margin-top: 10px;
        }
        .footer{
            width: 980px;
            height: 40px;
            background-color: tomato;
            margin:0 auto;
            margin-top:10px;
        }
    </style>
</head>
<body>
<!--
1.企业开发中什么时候使用标准流什么时候使用浮动流?
垂直方向使用标准流, 水平方向使用浮动流

2.拿到一个很复杂的界面如何入手?
2.1从上至下布局
2.2从外向内布局
2.3水平方向可以先划分为一左一右再对左边或者右边进行进一步布局
-->
<div class="header">
    <div class="logo"></div>
    <div class="language"></div>
    <div class="nav"></div>
</div>
<div class="content">
    <div class="aside"></div>
    <div class="article">
        <div class="articleTop">
            <div class="articleTopLeft">
                <div class="new1"></div>
                <div class="new2"></div>
            </div>
            <div class="articleTopRight"></div>
        </div>
        <div class="articleBottom"></div>
    </div>
</div>
<div class="footer"></div>
</body>
</html>
```



## 浮动元素的高度

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>64-浮动元素高度问题</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            border: 1px solid #000;
        }
        p{
            float: left;
            width: 50px;
            height: 50px;
            background-color: red;
        }
    </style>
</head>
<body>
<!--
1.在标准流中内容的高度可以撑起父元素的高度
2.在浮动流中浮动的元素是不可以撑起父元素的高度的
-->
<div>
    <p></p>
</div>
</body>
</html>
```



## 清除浮动方式一

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>65-清除浮动方式一</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .box1{
            height: 20px;
            background-color: red;
        }
        .box2{
            background-color: green;
        }
        .box1 p{
            width: 100px;
            background-color: blue;
        }
        .box2 p{
            width: 100px;
            background-color: yellow;
        }
        p{
            float: left;
        }
    </style>
</head>
<body>
<!--
1.清除浮动的第一种方式
给前面一个父元素设置高度

注意点:
在企业开发中, 我们能不写高度就不写高度, 所以这种方式用得很少
-->
<div class="box1">
    <p>我是文字1</p>
    <p>我是文字1</p>
    <p>我是文字1</p>
</div>

<div class="box2">
    <p>我是文字2</p>
    <p>我是文字2</p>
    <p>我是文字2</p>
</div>
</body>
</html>
```



## 清除浮动方式二

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>66-清除浮动方式二</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        body{
            border: 1px solid #000;
        }
        .box1{
            background-color: red;
        }
        .box2{
            background-color: green;
            clear: both;
            margin-top: 28px;
        }
        .box1 p{
            width: 100px;
            background-color: blue;
        }
        .box2 p{
            width: 100px;
            background-color: yellow;
        }
        p{
            float: left;
        }
        /*
        .dd{
            width: 500px;
            height: 500px;
            background-color: red;
            border: 1px solid #000;
        }
        .ddd{
            width: 200px;
            height: 200px;
            background-color: blue;
            margin-top: 100px;
        }
        */
    </style>
</head>
<body>
<!--
1.清除浮动的第二种方式
给后面的盒子添加clear属性

clear属性取值:
none: 默认取值, 按照浮动元素的排序规则来排序(左浮动找左浮动, 右浮动找右浮动)
left: 不要找前面的左浮动元素
right: 不要找前面的右浮动元素
both: 不要找前面的左浮动元素和右浮动元素

注意点:
当我们给某个元素添加clear属性之后, 那么这个属性的margin属性就会失效
-->

<div class="box1">
    <p>我是文字1</p>
    <p>我是文字1</p>
    <p>我是文字1</p>
</div>

<div class="box2">
    <p>我是文字2</p>
    <p>我是文字2</p>
    <p>我是文字2</p>
</div>

<!--
<div class="dd">
    <div class="ddd"></div>
</div>
-->
</body>
</html>
```



## 清除浮动方式三

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>67-清除浮动方式三</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .box1{
            background-color: red;
            /*margin-bottom: 10px;*/
        }
        .box2{
            background-color: green;
            /*margin-top: 10px;*/
        }
        .box1 p{
            width: 100px;
            background-color: blue;
        }
        .box2 p{
            width: 100px;
            background-color: yellow;
        }
        p{
            float: left;
        }
        .wall{
            clear: both;
        }
        .h20{
            height: 20px;
            background-color: skyblue;
        }
    </style>
</head>
<body>
<!--
1.清除浮动的第三种方式
隔墙法

2.外墙法
2.1在两个盒子中间添加一个额外的块级元素
2.2给这个额外添加的块级元素设置clear: both;属性

注意点:
外墙法它可以让第二个盒子使用margin-top属性
外墙法不可以让第一个盒子使用margin-bottom属性

3.内墙法
3.1在第一个盒子中所有子元素最后添加一个额外的块级元素
3.2给这个额外添加的块级元素设置clear: both;属性

注意点:
内墙法它可以让第二个盒子使用margin-top属性
内墙法它可以让第一个盒子使用margin-bottom属性

4.外墙法和内墙法区别?
外墙法不能撑起第一个盒子的高度, 而内墙法可以撑起第一个盒子的高度

5.在企业开发中不常用隔墙法来清除浮动
-->
<div class="box1">
    <p>我是文字1</p>
    <p>我是文字1</p>
    <p>我是文字1</p>
    <div class="wall h20"></div>
</div>

<!--<div class="wall h20"></div>-->

<div class="box2">
    <p>我是文字2</p>
    <p>我是文字2</p>
    <p>我是文字2</p>
</div>
</body>
</html>
```



## 伪元素选择器

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>68-伪元素选择器</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 200px;
            height: 200px;
            background-color: red;
        }
        /*
        p{
            width: 50px;
            height: 50px;
            background-color: pink;
        }
        */

        div::before{
            content: "爱你";
            width: 50px;
            height: 50px;
            background-color: pink;
            display: block;
        }
        div::after{
            /*指定添加的子元素中存储的内容*/
            content: "么么哒";
            /*指定添加的子元素的宽度和高度*/
            width: 50px;
            /*height: 50px;*/
            /*内容是可以超出标签的范围的, 所以高度为0依然可以看见内容*/
            height:0;
            background-color: pink;
            /*指定添加的子元素的显示模式*/
            display: block;
            /*隐藏添加的子元素*/
            visibility: hidden;
        }

    </style>
</head>
<body>
<!--
1.什么是伪元素选择器?
伪元素选择器作用就是给指定标签的内容前面添加一个子元素或者给指定标签的内容后面添加一个子元素

2.格式:
标签名称::before{
    属性名称:值;
}
给指定标签的内容前面添加一个子元素

标签名称::after{
    属性名称:值;
}
给指定标签的内容后面添加一个子元素

-->
<div>
    <!--<p>爱你</p>-->
    我是文字
    <!--<p>么么哒</p>-->
</div>

</body>
</html>
```



## 清除浮动方式四

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>69-清除浮动方式四</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .box1{
            background-color: red;
            /*margin-bottom: 10px;*/
        }
        .box2{
            background-color: green;
            /*margin-top: 10px;*/
        }
        .box1 p{
            width: 100px;
            background-color: blue;
        }
        .box2 p{
            width: 100px;
            background-color: yellow;
        }
        p{
            float: left;
        }
        .box1::after{
            /*设置添加的子元素的内容为空*/
            content: "";
            /*设置添加的子元素为块级元素*/
            display: block;
            /*设置添加的子元素的高度为0*/
            height: 0;
            /*设置添加的子元素看不见*/
            visibility: hidden;
            /*给添加的子元素设置clear: both;*/
            clear: both;
        }
        .box1{
            /*兼容IE6*/
            *zoom:1;
        }
    </style>
</head>
<body>
<!--
1.清除浮动的第四种方式
利用伪元素选择器清除浮动
本质上就是内墙法, 只不过是直接通过CSS代码添加了内墙, 其它特性和内墙法都一样

注意点:
IE6中不支持这种方式, 为了兼容IE6必须给前面的盒子添加*zoom:1;属性
-->
<div class="box1">
    <p>我是文字1</p>
    <p>我是文字1</p>
    <p>我是文字1</p>
</div>

<div class="box2">
    <p>我是文字2</p>
    <p>我是文字2</p>
    <p>我是文字2</p>
</div>
</body>
</html>
```



## 清除浮动方式五

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>70-清除浮动方式五</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        /*
        div{
            width: 100px;
            height: 100px;
            background-color: red;
            overflow: hidden;
        }
        */
        /*
        .box1{
            background-color: red;
            overflow: hidden;
            *zoom:1;
        }
        .box2{
            background-color: green;
        }
        .box1 p{
            width: 100px;
            background-color: blue;
        }
        .box2 p{
            width: 100px;
            background-color: yellow;
        }
        p{
            float: left;
        }
        */
        .box1{
            width: 200px;
            height: 200px;
            background-color: red;
            /*border: 1px solid #000;*/
            overflow: hidden;
        }
        .box2{
            width: 100px;
            height: 100px;
            background-color: blue;
            margin-top: 20px;
        }
    </style>
</head>
<body>
<!--
1.overflow: hidden;作用
1.1可以将超出标签范围的内容裁剪掉
1.2清除浮动
1.3可以通过overflow: hidden;让里面的盒子设置margin-top之后, 外面的盒子不被顶下来
-->

<!--
<div>我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字</div>
-->

<!--
<div class="box1">
    <p>我是文字1</p>
    <p>我是文字1</p>
    <p>我是文字1</p>

</div>

<div class="box2">
    <p>我是文字2</p>
    <p>我是文字2</p>
    <p>我是文字2</p>
</div>
-->
<div class="box1">
    <div class="box2"></div>
</div>
</body>
</html>
```



## 相对定位

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>75-定位流</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 100px;
            height: 100px;
        }
        .box1{
            background-color: red;
        }
        .box2{
            background-color: green;
            position: relative;
            top: 20px;
            left: 20px;
            /*right: 20px;*/
            /*margin-bottom: 20px;*/
            margin-top: 20px;
        }
        .box3{
            background-color: blue;
        }
        span{
            position: relative;
            width: 100px;
            height: 100px;
            background-color: red;
        }
        input{
            width: 200px;
            height: 50px;
        }
        img{
            width: 100px;
            height: 50px;
            position: relative;
            top: 20px;
        }
    </style>
</head>
<body>
<!--
1.定位流分类
1.1相对定位
1.2绝对定位
1.3固定定位
1.4静态定位

2.什么是相对定位?
相对定位就是相对于自己以前在标准流中的位置来移动

3.相对定位注意点
3.1相对定位是不脱离标准流的, 会继续在标准流中占用一份空间
3.2在相对定位中同一个方向上的定位属性只能使用一个
3.3由于相对定位是不脱离标准流的, 所以在相对定位中是区分块级元素/行内元素/行内块级元素
3.4由于相对定位是不脱离标准流的, 并且相对定位的元素会占用标准流中的位置, 所以当给相对定位的元素设置margin/padding等属性的时会影响到标准流的布局

4.相对定位应用场景
4.1用于对元素进行微调
4.2配合后面学习的绝对定位来使用
-->

<!--
<div class="box1"></div>
<div class="box2"></div>
<div class="box3"></div>
-->
<span>我是span</span>


<input type="text" name="" id="">
<img src="images/vcode.jpg" alt="">
</body>
</html>
```



## 绝对定位

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>76-定位流-绝对定位</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 100px;
            height: 100px;
        }
        .box1{
            background-color: red;
        }
        .box2{
            background-color: green;
            position: absolute;
            /*float: left;*/
            left: 0;
            /*right: 0;*/
            /*top: 0;*/
            bottom: 0;
        }
        .box3{
            background-color: blue;
        }
        span{
            position: absolute;
            width: 100px;
            height: 100px;
            background-color: yellow;
        }
    </style>
</head>
<body>
<!--
1.什么是绝对定位?
绝对定位就是相对于body来定位

2.绝对定位注意点
2.1绝对定位的元素是脱离标准流的
2.2绝对定位的元素是不区分块级元素/行内元素/行内块级元素
-->
<div class="box1"></div>
<div class="box2"></div>
<div class="box3"></div>
<span>我是span</span>
<img src="./images/1.jpg" alt="">
</body>
</html>
```



## 绝对定位的参考点

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>77-绝对定位-参考点</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .box1{
            width: 300px;
            height: 300px;
            background-color: red;
            position: relative;
        }
        .box2{
            width: 200px;
            height: 200px;
            background-color: green;
            position: relative;
        }
        .box3{
            width: 100px;
            height: 100px;
            background-color: blue;
            position: absolute;
            left: 0;
            bottom: 0;
          }
    </style>
</head>
<body>
<!--
1.规律
1.默认情况下所有的绝对定位的元素, 无论有没有祖先元素, 都会以body作为参考点

2.如果一个绝对定位的元素有祖先元素, 并且祖先元素也是定位流, 那么这个绝对定位的元素就会以定位流的那个祖先元素作为参考点
2.1只要是这个绝对定位元素的祖先元素都可以
2.2指的定位流是指绝对定位/相对定位/固定定位
2.3定位流中只有静态定位不行

3.如果一个绝对定位的元素有祖先元素, 并且祖先元素也是定位流, 而且祖先元素中有多个元素都是定位流, 那么这个绝对定位的元素会以离它最近的那个定位流的祖先元素为参考点
-->
<div class="box1">
    <div class="box2">
        <div class="box3"></div>
    </div>
</div>
</body>
</html>
```



## 绝对定位的注意点

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>78-绝对定位-注意点</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        /*
        .box1{
            width: 100px;
            height: 100px;
            background-color: red;
            position: absolute;
            right: 0;
            bottom: 0;
        }
        .box2{
            width: 2000px;
            height: 100px;
            background-color: green;
        }
        .box3{
            width: 100px;
            height: 2000px;
            background-color: blue;
        }
        */
        .box1{
            width: 300px;
            height: 300px;
            background-color: red;
            border: 10px solid #000;
            padding: 30px;
            position: relative;
        }
        .box2{
            width: 100px;
            height: 100px;
            background-color: green;
            position: absolute;
            left: 0;
            top: 0;
        }
    </style>
</head>
<body>
<!--
1.如果一个绝对定位的元素是以body作为参考点, 那么其实是以网页首屏的宽度和高度作为参考点, 而不是以整个网页的宽度和高度作为参考点

2.一个绝对定位的元素会忽略祖先元素的padding
-->

<!--
<div class="box1"></div>
<div class="box2"></div>
<div class="box3"></div>
-->

<div class="box1">
    <div class="box2"></div>
</div>
</body>
</html>
```



## 子绝父相

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>79-绝对定位-子绝父相</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        ul{
            list-style: none;
            width: 800px;
            height: 50px;
            margin: 0 auto;
            margin-top: 100px;
            background-color: red;
        }
        ul li{
            float: left;
            width: 150px;
            line-height: 50px;
            text-align: center;
            background-color: #ccc;
        }
        ul li:nth-of-type(4){
            background-color: yellow;
            position: relative;
        }
        ul li img{
            /*
            相对定位弊端:
            相对定位不会脱离标准流, 会继续在标准流中占用一份空间, 所以不利于布局界面
            */
            /*
            position: relative;
            left: -42px;
            top: -18px;
            */
            /*
            绝对定位弊端:
            默认情况下绝对定位的元素会以body作为参考点, 所以会随着浏览器的宽度高度的变化而变化
            */
            /*
            position: absolute;
            left: 526px;
            top: 90px;
            */

            /*
            子绝父相
            子元素用绝对定位, 父元素用相对定位
            */
            position: absolute;
            /*left: 40px;*/
            left: 50%;
            margin-left: -12px;
            top: -10px;
        }
    </style>
</head>
<body>
<ul>
    <li>服装城</li>
    <li>美妆馆</li>
    <li>京东超市</li>
    <li>全球购
        <img src="images/hot.png" alt="">
    </li>
    <li>闪购</li>
    <li>团购</li>
    <li>拍卖</li>
    <li>金融</li>
</ul>
</body>
</html>
```



## 绝对定位实现水平居中

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>80-绝对定位-水平居中</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 300px;
            /*width: 50%;*/
            height: 50px;
            background-color: red;
            /*margin: 0 auto;*/
            position: absolute;
            left: 50%;
            margin-left: -150px;
        }
    </style>
</head>
<body>
<!--
1.如何让绝对定位的元素水平居中
只需要设置绝对定位元素的left:50%;
然后再设置绝对定位元素的 margin-left: -元素宽度的一半px;
-->
<div></div>
</body>
</html>
```



## 定位练习一

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>81-定位练习1</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 300px;
            height: 300px;
            border: 2px solid #ccc;
            margin: 0 auto;
            margin-top: 100px;
            position: relative;
        }
        div img{
            width: 300px;
            height: 200px;
        }
        div .hot{
            width: 45px;
            height: 44px;
            background: url("images/tuangou.png") no-repeat 0px -280px;
            /*display: inline-block;*/
            position: absolute;
            left: 0;
            top: 0;
        }
        div .price{
            width: 134px;
            height: 42px;
            background: url("images/tuangou.png") no-repeat 0px -362px;
            /*display: inline-block;*/
            position: absolute;
            left: -7px;
            top: 163px;
        }
    </style>
</head>
<body>
<div>
    <img src="images/meat.jpg" alt="">
    <span class="hot"></span>
    <span class="price"></span>
    <p>【2店通用】Love Taste爱味道牛排生活馆 双人套餐，提供免费WiFi</p>
</div>
</body>
</html>
```



## 定位练习二

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>82-定位练习2</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 520px;
            height: 280px;
            border: 2px solid gold;
            margin: 0 auto;
            margin-top: 100px;
            position: relative;
        }
        div span{
            /*margin-top: 10px;*/
            /*display: block;*/
            width: 40px;
            height: 80px;
            background-color: rgba(0,0,0,0.3);
            font-size: 50px;
            color: white;
            text-align: center;
            line-height: 80px;
        }
        div .leftArrow{
            position: absolute;
            left: 0px;
            top: 100px;
        }
        div .rightArrow{
            position: absolute;
            right: 0px;
            top: 100px;
        }
        ol{
            list-style: none;
            width: 200px;
            height: 40px;
            background-color: rgba(255,255,255,0.7);
            position: absolute;
            right: 10px;
            bottom: 10px;
        }
        ol li{
            width: 40px;
            /*height: 40px;*/
            line-height: 40px;
            text-align: center;
            border: 1px solid gold;
            box-sizing: border-box;
            float: left;
        }
    </style>
</head>
<body>
<div>
    <img src="images/ad.jpg" alt="">
    <span class="leftArrow">&lt;</span>
    <span class="rightArrow">&gt;</span>
    <ol>
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
        <li>5</li>
    </ol>
</div>
</body>
</html>
```



## 固定定位

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>83-定位流-固定定位</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        /*
        div{
            width: 200px;
            height: 2000px;
            border: 1px solid #000;
            background-image: url(images/girl.jpg);
            background-repeat: no-repeat;
            background-attachment: fixed;
        }
        */
        div{
            width: 100px;
            height: 100px;
        }
        .box1{
            background-color: red;
        }
        .box2{
            background-color: green;
            position: fixed;
        }
        .box3{
            background-color: blue;
        }
        /*
        span{
            width: 100px;
            height: 100px;
            background-color: yellow;
            position: fixed;
        }
        */
        .box4{
            height: 2000px;
            background-color: yellow;
        }
    </style>
</head>
<body>
<!--<div></div>-->

<!--
1.什么是固定定位?
固定定位和前面学习的背景关联方式很像, 背景定位可以让背景图片不随着滚动条的滚动而滚动, 而固定定位可以让某个盒子不随着滚动条的滚动而滚动

注意点:
1.固定定位的元素是脱离标准流的, 不会占用标准流中的空间
2.固定定位和绝对定位一样不区分行内/块级/行内块级
3.IE6不支持固定定位
-->
<div class="box1"></div>
<div class="box2"></div>
<div class="box3"></div>
<div class="box4"></div>
<!--<span>我是span</span>-->
</body>
</html>
```



## 定位流的z-index

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>85-定位流-z-index属性</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        /*
        div{
            width: 100px;
            height: 100px;
        }
        .box1{
            background-color: red;
            position: relative;
            left: 0;
            top: 0;
        }
        .box2{
            background-color: green;
            position: absolute;
            left: 50px;
            top: 50px;
        }
        .box3{
            background-color: blue;
            position: fixed;
            left: 100px;
            top: 100px;
        }
        */
        .father1{
            width: 200px;
            height: 200px;
            background-color: red;
            position: relative;
            z-index: 2;
        }
        .father2{
            width: 200px;
            height: 200px;
            background-color: green;
            position: relative;
            z-index: 3;
        }
        .son1{
            width: 100px;
            height: 100px;
            background-color: blue;
            position: absolute;
            left: 200px;
            top: 200px;
            z-index: 1;
        }
        .son2{
            width: 100px;
            height: 100px;
            background-color: yellow;
            position: absolute;
            left: 250px;
            top: 50px;
            z-index: 2;
        }
    </style>
</head>
<body>
<!--
1.什么是z-index属性?
默认情况下所有的元素都有一个默认的z-index属性, 取值是0.
z-index属性的作用是专门用于控制定位流元素的覆盖关系的

1.默认情况下定位流的元素会盖住标准流的元素
2.默认情况下定位流的元素后面编写的会盖住前面编写的
3.如果定位流的元素设置了z-index属性, 那么谁的z-index属性比较大, 谁就会显示在上面

注意点:
1.从父现象
1.1如果两个元素的父元素都没有设置z-index属性, 那么谁的z-index属性比较大谁就显示在上面
1.2如果两个元素的父元素设置了z-index属性, 那么子元素的z-index属性就会失效, 也就是说谁的父元素的z-index属性比较大谁就会显示在上面
-->
<!--
<div class="box1"></div>
<div class="box2"></div>
<div class="box3"></div>
-->
<div class="father1">
    <div class="son1"></div>
</div>
<div class="father2">
    <div class="son2"></div>
</div>
</body>
</html>
```



## a标签的伪类选择

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>86-a标签的伪类选择器</title>
    <style>
        /*
        a:link{
            color: tomato;
        }
        a:visited{
            color: green;
        }
        a:hover{
            color: orange;
        }
        a:active{
            color: pink;
        }
        */
        a{
            // 简写格式
            color: green;
        }
        /*a:link{*/
            /*color: green;*/
        /*}*/
        /*a:visited{*/
            /*color: green;*/
        /*}*/
        a:hover{
            color: orange;
        }
        a:active{
            color: pink;
        }
    </style>
</head>
<body>
<!--
1.通过我们的观察发现a标签存在一定的状态
1.1默认状态, 从未被访问过
1.2被访问过的状态
1.3鼠标长按状态
1.4鼠标悬停在a标签上状态

2.什么是a标签的伪类选择器?
a标签的伪类选择器是专门用来修改a标签不同状态的样式的

3.格式
:link 修改从未被访问过状态下的样式
:visited 修改被访问过的状态下的样式
:hover 修改鼠标悬停在a标签上状态下的样式
:active 修改鼠标长按状态下的样式

4.注意点
4.1a标签的伪类选择器可以单独出现也可以一起出现
4.2a标签的伪类选择器如果一起出现, 那么有严格的顺序要求
编写的顺序必须要个的遵守爱恨原则 love hate
4.3如果默认状态的样式和被访问过状态的样式一样, 那么可以缩写
-->
<a href="http://www.taobao.com">taobao</a>
<a href="http://www.jd.com">jd</a>
</body>
</html>
```



## 过渡模块

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>88-过渡模块</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 100px;
            height: 50px;
            background-color: red;
            /*告诉系统哪个属性需要执行过渡效果*/
            transition-property: width, background-color;
            /*告诉系统过渡效果持续的时长*/
            transition-duration: 5s, 5s;

            /*transition-property: background-color;*/
            /*transition-duration: 5s;*/
        }
        /*:hover这个伪类选择器除了可以用在a标签上, 还可以用在其它的任何标签上*/
        div:hover{
            width: 300px;
            background-color: blue;
        }
    </style>
</head>
<body>
<!--
1,过渡三要素
1.1必须要有属性发生变化
1.2必须告诉系统哪个属性需要执行过渡效果
1.3必须告诉系统过渡效果持续时长

2.注意点
当多个属性需要同时执行过渡效果时用逗号隔开即可
transition-property: width, background-color;
transition-duration: 5s, 5s;
-->
<div></div>
</body>
</html>
```



## 过渡模块的其他属性

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>89-过渡模块-其它属性</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div {
            width: 100px;
            height: 50px;
            background-color: red;
            transition-property: width;
            transition-duration: 5s;
            /*告诉系统延迟多少秒之后才开始过渡动画*/
            /*transition-delay: 2s;*/
        }
        div:hover{
            width: 300px;
        }
        ul{
            width: 800px;
            height: 500px;
            margin: 0 auto;
            background-color: pink;
            border: 1px solid #000;
        }
        ul li{
            list-style: none;
            width: 100px;
            height: 50px;
            margin-top: 50px;
            background-color: blue;
            transition-property: margin-left;
            transition-duration: 10s;
        }
        ul:hover li{
            margin-left: 700px;
        }
        ul li:nth-child(1){
            /*告诉系统过渡动画的运动的速度*/
            transition-timing-function: linear;
        }
        ul li:nth-child(2){
            transition-timing-function: ease;
        }
        ul li:nth-child(3){
            transition-timing-function: ease-in;
        }
        ul li:nth-child(4){
            transition-timing-function: ease-out;
        }
        ul li:nth-child(5){
            transition-timing-function: ease-in-out;
        }
    </style>
</head>
<body>
<!--<div></div>-->
<ul>
    <li>linear</li>
    <li>ease</li>
    <li>ease-in</li>
    <li>ease-out</li>
    <li>ease-in-out</li>
</ul>
</body>
</html>
```



## 过渡模块的连写

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>90-过渡模块-连写</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div {
            width: 100px;
            height: 50px;
            background-color: red;
            /*transition-property: width;*/
            /*transition-duration: 5s;*/
            /*transition: width 5s linear 0s,background-color 5s linear 0s;*/
            /*transition: background-color 5s linear 0s;*/
            /*transition: width 5s,background-color 5s,height 5s;*/
            transition: all 5s;
        }
        div:hover{
            width: 300px;
            height: 300px;
            background-color: blue;
        }
    </style>
</head>
<body>
<!--
1.过渡连写格式
transition: 过渡属性 过渡时长 运动速度 延迟时间;

2.过渡连写注意点
2.1和分开写一样, 如果想给多个属性添加过渡效果也是用逗号隔开即可
2.2连写的时可以省略后面的两个参数, 因为只要编写了前面的两个参数就已经满足了过渡的三要素
2.3如果多个属性运动的速度/延迟的时间/持续时间都一样, 那么可以简写为
transition:all 0s;
-->
<div></div>
</body>
</html>
```



## 过渡实现弹性效果

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>91-过渡模块-弹性效果</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            height: 100px;
            background-color: red;
            margin-top: 100px;
            text-align: center;
            line-height: 100px;
        }
        div span{
            font-size: 80px;
            /*transition-property: margin;*/
            /*transition-duration: 3s;*/
            transition: margin 3s;
        }
        div:hover span{
            margin: 0 20px;
        }
    </style>
</head>
<body>
<!--
1.编写过渡套路
1.1不要管过渡, 先编写基本界面
1.2修改我们认为需要修改的属性
1.3再回过头去给被修改属性的那个元素添加过渡即可
-->
<div>
    <span>江</span>
    <span>哥</span>
    <span>带</span>
    <span>你</span>
    <span>狂</span>
    <span>虐</span>
    <span>H</span>
    <span>5</span>
</div>
</body>
</html>
```



## 过渡实现手风琴效果

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>92-过渡模块-手风琴效果</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        ul{
            width: 960px;
            height: 300px;
            margin: 100px auto;
            border: 1px solid #000;
            overflow: hidden;
        }
        ul li{
            list-style: none;
            width: 160px;
            height: 300px;
            background-color: red;
            float: left;
            /*border: 1px solid #000;*/
            /*box-sizing: border-box;*/
            /*transition-property: width;*/
            /*transition-duration: 0.5s;*/
            transition: width 0.5s;
        }
        ul:hover li{
            width: 100px;
        }
        ul li:hover{
            width: 460px;
        }
    </style>
</head>
<body>
<ul>
    <li><img src="images/ad1.jpg" alt=""></li>
    <li><img src="images/ad2.jpg" alt=""></li>
    <li><img src="images/ad3.jpg" alt=""></li>
    <li><img src="images/ad4.jpg" alt=""></li>
    <li><img src="images/ad5.jpg" alt=""></li>
    <li><img src="images/ad6.jpg" alt=""></li>
</ul>
</body>
</html>
```



## 2D转换模块

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>93-2D转换模块</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        ul{
            width: 800px;
            height: 500px;
            border: 1px solid #000;
            margin: 0 auto;
        }
        ul li{
            list-style: none;
            width: 100px;
            height: 50px;
            background-color: red;
            margin: 0 auto;
            margin-top: 50px;
            text-align: center;
            line-height: 50px;
        }
        ul li:nth-child(2){
            /*其中deg是单位, 代表多少度*/
            transform: rotate(45deg);
        }
        ul li:nth-child(3){
            /*
            第一个参数:水平方向
            第二个参数:垂直方向
            */
            transform: translate(100px, 0px);
        }
        ul li:nth-child(4){
            /*
            第一个参数:水平方向
            第二个参数:垂直方向
            注意点:
            如果取值是1, 代表不变
            如果取值大于1, 代表需要放大
            如果取值小于1, 代表需要缩小
            如果水平和垂直缩放都一样, 那么可以简写为一个参数
            */
            /*transform: scale(0.5, 0.5);*/
            transform: scale(1.5);
        }
        ul li:nth-child(5){
            /*
            注意点:
            1.如果需要进行多个转换, 那么用空格隔开
            2.2D的转换模块会修改元素的坐标系, 所以旋转之后再平移就不是水平平移的
            */
            transform: rotate(45deg) translate(100px, 0px) scale(1.5, 1.5);
            /*transform: translate(100px, 0px);*/
        }
    </style>
</head>
<body>
<ul>
    <li>正常的</li>
    <li>旋转的</li>
    <li>平移的</li>
    <li>缩放的</li>
    <li>综合的</li>
</ul>
</body>
</html>
```



## 形变中心

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>94-2D转换模块-形变中心点</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        ul{
            width: 200px;
            height: 200px;
            border: 1px solid #000;
            margin: 100px auto;
            position: relative;
        }
        ul li{
            list-style: none;
            width: 200px;
            height: 200px;
            position: absolute;
            left: 0;
            top: 0;
            /*
            第一个参数:水平方向
            第二个参数:垂直方向
            注意点
            取值有三种形式
            具体像素
            百分比
            特殊关键字
            */
            /*transform-origin: 200px 0px;*/
            /*transform-origin: 50% 50%;*/
            /*transform-origin: 0% 0%;*/
            /*transform-origin: center center;*/
            transform-origin: left top;
        }
        ul li:nth-child(1){
            /*
            默认情况下所有的元素都是以自己的中心点作为参考来旋转的, 我们可以通过形变中心点属性来修改它的参考点
            */
            background-color: red;
            transform: rotate(30deg);
        }
        ul li:nth-child(2){
            background-color: green;
            transform: rotate(50deg);
        }
        ul li:nth-child(3){
            background-color: blue;
            transform: rotate(70deg);
        }
    </style>
</head>
<body>
<ul>
    <li></li>
    <li></li>
    <li></li>
</ul>
</body>
</html>
```



## 旋转轴向

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>95-2D转换模块-旋转轴向</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        ul{
            width: 800px;
            height: 500px;
            margin: 0 auto;

        }
        ul li{
            list-style: none;
            width: 200px;
            height: 200px;
            margin: 0 auto;
            margin-top: 50px;
            border: 1px solid #000;
            /*
            1.什么是透视
            近大远小
            2.注意点
            一定要注意, 透视属性必须添加到需要呈现近大远小效果的元素的父元素上面
            */
            perspective: 500px;
        }
        ul li img{
            width: 200px;
            height: 200px;
            /*perspective: 500px;*/
        }
        ul li:nth-child(1){
            /*默认情况下所有元素都是围绕Z轴进行旋转*/
            transform: rotateZ(45deg);
        }
        ul li:nth-child(2) img{
            transform: rotateX(45deg);
        }
        ul li:nth-child(3) img{
            /*
            总结:
            想围绕哪个轴旋转, 那么只需要在rotate后面加上哪个轴即可
            */
            transform: rotateY(45deg);
        }
    </style>
</head>
<body>
<ul>
    <li><img src="images/rotateZ.jpg" alt=""></li>
    <li><img src="images/rotateX.jpg" alt=""></li>
    <li><img src="images/rotateY.jpg" alt=""></li>
</ul>
</body>
</html>
```



## 转换模块的练习

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>96-2D转换模块-练习</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 310px;
            height: 438px;
            border: 1px solid #000;
            background-color: skyblue;
            margin: 100px auto;
            perspective: 500px;
        }
        div img{
            transform-origin: center bottom;
            transition: transform 1s;
        }
        div:hover img{
            transform: rotateX(80deg);
        }
    </style>
</head>
<body>
<div>
    <img src="images/pk.png" alt="">
</div>
</body>
</html>
```



## 相片墙案例

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>97-2D转换模块-相片墙</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        ul{
            height: 400px;
            border: 1px solid #000;
            background-color: skyblue;
            margin-top: 100px;
            text-align: center;
        }
        ul li{
            list-style: none;
            width: 150px;
            height: 200px;
            background-color: red;
            display: inline-block;
            margin-top: 100px;
            transition: all 1s;
            position: relative;
            box-shadow: 0 0 10px;
        }
        ul li:nth-child(1){
            transform: rotate(30deg);
        }
        ul li:nth-child(2){
            transform: rotate(-40deg);
        }
        ul li:nth-child(3){
            transform: rotate(10deg);
        }
        ul li:nth-child(4){
            transform: rotate(45deg);
        }
        ul li img{
            width: 150px;
            height: 200px;
            border: 5px solid #fff;
            box-sizing: border-box;
        }
        ul li:hover{
            /*transform: rotate(0deg);*/
            /*transform: none;*/
            transform: scale(1.5);
            z-index: 998;
        }
    </style>
</head>
<body>
<ul>
    <li><img src="images/1.jpg" alt=""></li>
    <li><img src="images/2.jpg" alt=""></li>
    <li><img src="images/3.jpg" alt=""></li>
    <li><img src="images/4.jpg" alt=""></li>
</ul>
</body>
</html>
```



## 盒子阴影和文字阴影

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>98-盒子阴影和文字阴影</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .box1{
            width: 200px;
            height: 200px;
            background-color: red;
            margin: 100px auto;
            text-align: center;
            line-height: 200px;
            /*box-shadow: 10px 10px 10px 10px skyblue;*/
            /*box-shadow: 10px 10px 10px 10px skyblue inset;*/
            box-shadow: 10px 10px 10px;
            color: yellow;
        }
        .box2{
            width: 200px;
            height: 200px;
            margin: 0 auto;
            background-color: pink;
            text-align: center;
            line-height: 200px;
            font-size: 40px;
            /*text-shadow: 10px 10px 10px black;*/
            text-shadow: 10px 10px 10px;
            color: purple;
        }
    </style>
</head>
<body>
<!--
1.如何给盒子添加阴影
box-shadow: 水平偏移 垂直偏移 模糊度 阴影扩展 阴影颜色 内外阴影;

2.注意点
2.1盒子的阴影分为内外阴影, 默认情况下就是外阴影
2.2快速添加阴影只需要编写三个参数即可
box-shadow: 水平偏移 垂直偏移 模糊度;
默认情况下阴影的颜色和盒子内容的颜色一致

3.如何给文字添加阴影
text-shadow: 水平偏移 垂直偏移 模糊度 阴影颜色 ;
-->
<div class="box1">我是盒子</div>
<div class="box2">我是盒子</div>
</body>
</html>
```



## 翻转菜单

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>99-翻转菜单-综合练习</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .nav{
            width: 400px;
            height: 40px;
            margin: 0 auto;
            margin-top: 100px;
            background-color: black;
        }
        .nav>li{
            list-style: none;
            float: left;
            width: 120px;
            height: 40px;
            background-color: green;
            margin-left: 10px;
            text-align: center;
            line-height: 40px;
            position: relative;
        }
        .sub{
            /*不显示元素*/
            /*display: none;*/
            width: 120px;
            position: absolute;
            left: 0;
            top: 40px;
        }
        .sub li{
            list-style: none;
            background-color: deeppink;
            transform: rotateY(180deg);
            transition: all 1s;
            opacity: 0;
        }
        /*
        .nav>li:hover .sub{
            display: block;
        }
        */
        .nav>li:hover .sub li{
            transform: none;
            opacity: 1;
        }
        .nav>li:hover .sub li:nth-child(1){
            transition-delay: 0ms;
        }
        .nav>li:hover .sub li:nth-child(2){
            transition-delay: 200ms;
        }
        .nav>li:hover .sub li:nth-child(3){
            transition-delay: 400ms;
        }
        .nav>li:hover .sub li:nth-child(4){
            transition-delay: 600ms;
        }
        .nav>li:hover .sub li:nth-child(5){
            transition-delay: 800ms;
        }

        .nav>li .sub li:nth-child(5){
            transition-delay: 0ms;
        }
        .nav>li .sub li:nth-child(4){
            transition-delay: 200ms;
        }
        .nav>li .sub li:nth-child(3){
            transition-delay: 400ms;
        }
        .nav>li .sub li:nth-child(2){
            transition-delay: 600ms;
        }
        .nav>li .sub li:nth-child(1){
            transition-delay: 800ms;
        }
        div{
            width: 400px;
            height: 300px;
            background-color: red;
            margin: 0 auto;
        }
    </style>
</head>
<body>
<ul class="nav">
    <li>一级菜单
        <ul class="sub">
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
        </ul>
    </li>
    <li>一级菜单
        <ul class="sub">
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
        </ul>
    </li>
    <li>一级菜单
        <ul class="sub">
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
        </ul>
    </li>
</ul>
<div>我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字</div>
</body>
</html>
```



## 动画模块

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>100-动画模块</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 100px;
            height: 50px;
            background-color: red;
            /*transition-property: margin-left;*/
            /*transition-duration: 3s;*/

            /*1.告诉系统需要执行哪个动画*/
            animation-name: lnj;
            /*3.告诉系统动画持续的时长*/
            animation-duration: 3s;
        }
        /*2.告诉系统我们需要自己创建一个名称叫做lnj的动画*/
        @keyframes lnj {
            from{
                margin-left: 0;
            }
            to{
                margin-left: 500px;
            }
        }

        /*div:hover{*/
            /*margin-left: 500px;*/
        /*}*/
    </style>
</head>
<body>
<!--
1.过渡和动画之间的异同
1.1不同点
过渡必须人为的触发才会执行动画
动画不需要人为的触发就可以执行动画

1.2相同点
过渡和动画都是用来给元素添加动画的
过渡和动画都是系统新增的一些属性
过渡和动画都需要满足三要素才会有动画效果
-->
<div></div>
</body>
</html>
```



## 动画模块的其他属性

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>101-动画模块-其它属性上</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div {
            width: 100px;
            height: 50px;
            background-color: red;
            animation-name: sport;
            animation-duration: 2s;
            /*告诉系统多少秒之后开始执行动画*/
            /*animation-delay: 2s;*/
            /*告诉系统动画执行的速度*/
            animation-timing-function: linear;
            /*告诉系统动画需要执行几次*/
            animation-iteration-count: 3;
            /*告诉系统是否需要执行往返动画
            取值:
            normal, 默认的取值, 执行完一次之后回到起点继续执行下一次
            alternate, 往返动画, 执行完一次之后往回执行下一次
            */
            animation-direction: alternate;
        }
        @keyframes sport {
            from{
                margin-left: 0;
            }
            to{
                margin-left: 500px;
            }
        }
        div:hover{
            /*
            告诉系统当前动画是否需要暂停
            取值:
            running: 执行动画
            paused: 暂停动画
            */
            animation-play-state: paused;
        }
    </style>
</head>
<body>
<div class="box1"></div>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>102-动画模块-其它属性下</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .box1 {
            width: 100px;
            height: 50px;
            background-color: red;
            position: absolute;
            left: 0;
            top: 0;
            animation-name: sport;
            animation-duration: 5s;
        }
        @keyframes sport {
            0%{
                left: 0;
                top: 0;
            }
            25%{
                left: 300px;
                top: 0;
            }
            50%{
                left: 300px;
                top: 300px;
            }
            75%{
                left: 0;
                top: 300px;
            }
            100%{
                left: 0;
                top: 0;
            }
        }

        .box2{
            width: 200px;
            height: 200px;
            background-color: blue;
            margin: 100px auto;
            animation-name: myRotate;
            animation-duration: 5s;
            animation-delay: 2s;
            /*
            通过我们的观察, 动画是有一定的状态的
            1.等待状态
            2.执行状态
            3.结束状态
            */
            /*
            animation-fill-mode作用:
            指定动画等待状态和结束状态的样式
            取值:
            none: 不做任何改变
            forwards: 让元素结束状态保持动画最后一帧的样式
            backwards: 让元素等待状态的时候显示动画第一帧的样式
            both: 让元素等待状态显示动画第一帧的样式, 让元素结束状态保持动画最后一帧的样式
            */
            /*animation-fill-mode: backwards;*/
            /*animation-fill-mode: forwards;*/
            animation-fill-mode: both;
        }
        @keyframes myRotate {
            0%{
                transform: rotate(10deg);
            }
            50%{
                transform: rotate(50deg);
            }
            100%{
                transform: rotate(70deg);
            }
        }
    </style>
</head>
<body>
<div class="box1"></div>
<div class="box2"></div>
</body>
</html>
```



## 动画简写

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>103-动画模块-连写</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div {
            width: 100px;
            height: 50px;
            background-color: red;
            /*animation: move 3s linear 2s 1 normal;*/
            animation: move 3s;
        }
        @keyframes move {
            from{
                margin-left: 0;
            }
            to{
                margin-left: 500px;
            }
        }
    </style>
</head>
<body>
<!--
1.动画模块连写格式
animation:动画名称 动画时长 动画运动速度 延迟时间 执行次数 往返动画;

2.动画模块连写格式的简写
animation:动画名称 动画时长;
-->
<div></div>
</body>
</html>
```



## 云层效果

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>104-动画模块-云层效果</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        ul{
            height: 400px;
            background-color: skyblue;
            margin-top: 100px;
            animation: change 5s linear 0s infinite alternate;
            position: relative;
            overflow: hidden;
        }
        ul li{
            list-style: none;
            width: 400%;
            height: 100%;
            position: absolute;
            left: 0;
            top: 0;
        }
        ul li:nth-child(1){
            background-image: url("images/cloud_one.png");
            animation: one 30s linear 0s infinite alternate;
        }
        ul li:nth-child(2){
            background-image: url("images/cloud_two.png");
            animation: two 30s linear 0s infinite alternate;
        }
        ul li:nth-child(3){
            background-image: url("images/cloud_three.png");
            animation: three 30s linear 0s infinite alternate;
        }
        @keyframes change {
            from{
                background-color: skyblue;
            }
            to{
                background-color: black;
            }
        }
        @keyframes one {
            from{
                margin-left: 0;
            }
            to{
                margin-left: -100%;
            }
        }
        @keyframes two {
            from{
                margin-left: 0;
            }
            to{
                margin-left: -200%;
            }
        }
        @keyframes three {
            from{
                margin-left: 0;
            }
            to{
                margin-left: -300%;
            }
        }
    </style>
</head>
<body>
<ul>
    <li></li>
    <li></li>
    <li></li>
</ul>
</body>
</html>
```



## 无线滚动

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>105-动画模块-无限滚动</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 600px;
            height: 188px;
            border: 1px solid #000;
            margin: 100px auto;
            overflow: hidden;
        }

        ul{
            width: 2000px;
            height: 188px;
            background-color: black;
            animation: move 10s linear 0s infinite normal;
        }
        ul li{
            float: left;
            list-style: none;
            width: 300px;
            height: 188px;
            background-color: red;
            border: 1px solid #000;
            box-sizing: border-box;
        }
        ul:hover{
            /*动画添加给谁, 就让谁停止*/
            animation-play-state: paused;
        }
        ul:hover li{
            opacity: 0.5;
        }
        ul li:hover{
            opacity: 1;
        }
        @keyframes move {
            from{
                margin-left: 0;
            }
            to{
                margin-left: -1200px;
            }
        }
    </style>
</head>
<body>
<div>
    <ul>
        <li><img src="images/banner1.jpg" alt=""></li>
        <li><img src="images/banner2.jpg" alt=""></li>
        <li><img src="images/banner3.jpg" alt=""></li>
        <li><img src="images/banner4.jpg" alt=""></li>
        <li><img src="images/banner1.jpg" alt=""></li>
        <li><img src="images/banner2.jpg" alt=""></li>
    </ul>
</div>
</body>
</html>
```



## 3D模块

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>106-3D转换模块</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .father{
            width: 200px;
            height: 200px;
            background-color: red;
            border: 1px solid #000;
            margin: 100px auto;
            perspective: 500px;
            /*
            1.什么是2D和3D
            2D就是一个平面, 只有宽度和高度, 没有厚度
            3D就是一个立体, 有宽度和高度, 还有厚度
            默认情况下所有的元素都是呈2D展现的
            2.如何让某个元素呈3D展现
            和透视一样, 想看到某个元素的3d效果, 只需要给他的父元素添加一个transform-style属性, 然后设置为preserve-3d即可
            */
            transform-style: preserve-3d;
            transform: rotateY(0deg);
        }
        .son{
            width: 100px;
            height: 100px;
            background-color: blue;
            border: 1px solid #000;
            margin: 0 auto;
            margin-top: 50px;
            transform: rotateY(45deg);
        }
    </style>
</head>
<body>
<div class="father">
    <div class="son"></div>
</div>
</body>
</html>
```



## 3D-正方体

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>108-3D转换模块-正方体终极</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        ul{
            width: 200px;
            height: 200px;
            border: 1px solid #000;
            box-sizing: border-box;
            margin: 100px auto;
            position: relative;
            transform: rotateY(0deg) rotateX(0deg);
            transform-style: preserve-3d;
        }
        ul li{
            list-style: none;
            width: 200px;
            height: 200px;
            font-size: 60px;
            text-align: center;
            line-height: 200px;
            position: absolute;
            left: 0;
            top: 0;
        }
        ul li:nth-child(1){
            background-color: red;
            transform: rotateX(90deg) translateZ(100px);
        }
        ul li:nth-child(2){
            background-color: green;
            transform: rotateX(180deg) translateZ(100px);
        }
        ul li:nth-child(3){
            background-color: blue;
            transform: rotateX(270deg) translateZ(100px);
        }
        ul li:nth-child(4){
            background-color: yellow;
            transform: rotateX(360deg) translateZ(100px);
        }
        ul li:nth-child(5){
            background-color: purple;
            transform: translateX(-100px) rotateY(90deg);
        }
        ul li:nth-child(6){
            background-color: pink;
            transform: translateX(100px) rotateY(90deg);
        }

    </style>
</head>
<body>
<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
    <li>6</li>
</ul>
</body>
</html>
```



## 长方体

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>109-3D转换模块-长方体</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        /*
        .father{
            width: 200px;
            height: 200px;
            border: 1px solid #000;
            margin: 100px auto;
        }
        .son{
            width: 200px;
            height: 200px;
            background-color: rgba(255,0,0,0.3);
            transform: scale(1.5, 1);
        }
        */
        ul{
            width: 200px;
            height: 200px;
            border: 1px solid #000;
            box-sizing: border-box;
            margin: 100px auto;
            position: relative;
            transform: rotateY(0deg) rotateX(0deg);
            transform-style: preserve-3d;
        }
        ul li{
            list-style: none;
            width: 200px;
            height: 200px;
            font-size: 60px;
            text-align: center;
            line-height: 200px;
            position: absolute;
            left: 0;
            top: 0;
        }
        ul li:nth-child(1){
            background-color: red;
            transform: rotateX(90deg) translateZ(100px) scale(2, 1);
        }
        ul li:nth-child(2){
            background-color: green;
            transform: rotateX(180deg) translateZ(100px) scale(2, 1);
        }
        ul li:nth-child(3){
            background-color: blue;
            transform: rotateX(270deg) translateZ(100px) scale(2, 1);
        }
        ul li:nth-child(4){
            background-color: yellow;
            transform: rotateX(360deg) translateZ(100px) scale(2, 1);
        }
        ul li:nth-child(5){
            background-color: purple;
            transform: translateX(-200px) rotateY(90deg);
        }
        ul li:nth-child(6){
            background-color: pink;
            transform: translateX(200px) rotateY(90deg);
        }
    </style>
</head>
<body>
<!--
<div class="father">
    <div class="son"></div>
</div>
-->
<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
    <li>6</li>
</ul>
</body>
</html>
```



## 3D模块的练习

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>110-3D转换模块-练习</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        body{
            /*想看到整个立方的近大远小效果, 就给ul的父元素添加透视*/
            perspective: 500px;
        }
        ul{
            width: 200px;
            height: 200px;
            box-sizing: border-box;
            margin: 100px auto;
            position: relative;
            transform: rotateY(0deg) rotateX(0deg);
            transform-style: preserve-3d;
            animation: sport 5s linear 0s infinite normal;
        }
        ul li{
            list-style: none;
            width: 200px;
            height: 200px;
            font-size: 60px;
            text-align: center;
            line-height: 200px;
            position: absolute;
            left: 0;
            top: 0;
        }
        ul li:nth-child(1){
            background-color: red;
            transform: rotateX(90deg) translateZ(100px) scale(2, 1);
        }
        ul li:nth-child(2){
            background-color: green;
            transform: rotateX(180deg) translateZ(100px) scale(2, 1);
        }
        ul li:nth-child(3){
            background-color: blue;
            transform: rotateX(270deg) translateZ(100px) scale(2, 1);
        }
        ul li:nth-child(4){
            background-color: yellow;
            transform: rotateX(360deg) translateZ(100px) scale(2, 1);
        }
        ul li:nth-child(5){
            background-color: purple;
            transform: translateX(-200px) rotateY(90deg);
        }
        ul li:nth-child(6){
            background-color: pink;
            transform: translateX(200px) rotateY(90deg);
        }
        ul li img{
            /*
            注意点:
            只要父元素被拉伸了,子元素也会被拉伸
            */
            width: 200px;
            height: 200px;
        }
        @keyframes sport {
            from{
                transform: rotateX(0deg);
            }
            to{
                transform: rotateX(360deg);
            }
        }
    </style>
</head>
<body>
<ul>
    <li><img src="images/banner1.jpg" alt=""></li>
    <li><img src="images/banner2.jpg" alt=""></li>
    <li><img src="images/banner3.jpg" alt=""></li>
    <li><img src="images/banner4.jpg" alt=""></li>
    <li></li>
    <li></li>
</ul>
</body>
</html>
```



## 3D播放器的效果

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>111-3D播放器下</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        body{
            background: url("images/jacky/bg.jpg") no-repeat;
            background-size:cover;
            overflow: hidden;
        }
        ul{
            width: 200px;
            height: 200px;
            /*background-color: red;*/
            position: absolute;
            bottom: 100px;
            left: 50%;
            margin-left:-100px;
            transform-style: preserve-3d;
            /*transform: rotateX(-10deg);*/
            animation: sport 6s linear 0s infinite normal;
        }
        ul li{
            list-style: none;
            width: 200px;
            height: 200px;
            font-size: 60px;
            text-align: center;
            line-height: 200px;
            position: absolute;
            left: 0;
            top: 0;
            background-color: black;
        }
        ul li:nth-child(1){
            transform: rotateY(60deg) translateZ(200px);
        }
        ul li:nth-child(2){
            transform: rotateY(120deg) translateZ(200px);
        }
        ul li:nth-child(3){
            transform: rotateY(180deg) translateZ(200px);
        }
        ul li:nth-child(4){
            transform: rotateY(240deg) translateZ(200px);
        }
        ul li:nth-child(5){
            transform: rotateY(300deg) translateZ(200px);
        }
        ul li:nth-child(6){
            transform: rotateY(360deg) translateZ(200px);
        }
        ul li img{
            width: 200px;
            height: 200px;
            border: 5px solid skyblue;
            box-sizing: border-box;
        }
        ul:hover{
            animation-play-state: paused;
        }
        ul:hover li img{
            opacity: 0.5;
        }
        ul li:hover img{
            opacity: 1;
        }
        @keyframes sport {
            from{
                /*
                注意点:
                1.动画中如果有和默认样式中同名的属性, 会覆盖默认样式中同名的属性
                2.在编写动画的时候, 固定不变的值写在前面, 需要变化的值写在后面
                */
                transform: rotateX(-10deg) rotateY(0deg);
            }
            to{
                transform: rotateX(-10deg) rotateY(360deg);
            }
        }
        .heart{
            width: 173px;
            height: 157px;
            position: absolute;
            left: 100px;
            bottom: 100px;
            animation: move 10s linear 0s infinite normal;
        }
        @keyframes move {
            0%{
                left: 100px;
                bottom: 100px;
                opacity: 1;
            }
            20%{
                left: 300px;
                bottom: 300px;
                opacity: 0;
            }
            40%{
                left: 500px;
                bottom: 500px;
                opacity: 1;
            }
            60%{
                left: 800px;
                bottom: 300px;
                opacity: 0;
            }
            80%{
                left: 1200px;
                bottom: 100px;
                opacity: 1;
            }
            100%{
                left: 800px;
                bottom: -200px;
            }
        }
    </style>
</head>
<body>
<ul>
    <li><img src="images/jacky/1.png" alt=""></li>
    <li><img src="images/jacky/2.jpg" alt=""></li>
    <li><img src="images/jacky/3.jpg" alt=""></li>
    <li><img src="images/jacky/4.gif" alt=""></li>
    <li><img src="images/jacky/5.jpg" alt=""></li>
    <li><img src="images/jacky/6.jpg" alt=""></li>
</ul>
<img src="images/jacky/xin.png" class="heart">
<audio src="images/jacky/江哥最爱的歌.mp3" autoplay="autoplay" loop="loop"></audio>
</body>
</html>
```



## 背景尺寸

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>112-背景尺寸属性</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        ul{
            width: 800px;
            height: 500px;
            margin: 0 auto;
        }
        ul li{
            list-style: none;
            float: left;
            width: 200px;
            height: 200px;
            margin: 30px 30px;
            border: 1px solid #000;
            text-align: center;
            line-height: 200px;
        }
        ul li:nth-child(1){
            background: url("images/dog.jpg") no-repeat;
        }
        ul li:nth-child(2){
            background: url("images/dog.jpg") no-repeat;
            /*
            第一个参数:宽度
            第二个参数:高度
            */
            background-size:200px 100px;
        }
        ul li:nth-child(3){
            background: url("images/dog.jpg") no-repeat;
            /*
            第一个参数:宽度
            第二个参数:高度
            */
            background-size:100% 80%;
        }
        ul li:nth-child(4){
            background: url("images/dog.jpg") no-repeat;
            /*
            第一个参数:宽度
            第二个参数:高度
            */
            background-size:auto 100px;
        }
        ul li:nth-child(5){
            background: url("images/dog.jpg") no-repeat;
            /*
            第一个参数:宽度
            第二个参数:高度
            */
            background-size:100px auto;
        }
        ul li:nth-child(6){
            background: url("images/dog.jpg") no-repeat;
            /*
            cover含义:
            1.告诉系统图片需要等比拉伸
            2.告诉系统图片需要拉伸到宽度和高度都填满元素
            */
            background-size:cover;
        }
        ul li:nth-child(7){
            background: url("images/dog.jpg") no-repeat;
            /*
            cover含义:
            1.告诉系统图片需要等比拉伸
            2.告诉系统图片需要拉伸到宽度或高度都填满元素
            */
            background-size:contain;
        }
    </style>
</head>
<body>
<!--
1.什么是背景尺寸属性
背景尺寸属性是CSS3中新增的一个属性, 专门用于设置背景图片大小
-->
<ul>
    <li>默认</li>
    <li>具体像素</li>
    <li>百分比</li>
    <li>宽度等比拉伸</li>
    <li>高度等比拉伸</li>
    <li>cover</li>
    <li>contain</li>
</ul>
</body>
</html>
```



## 背景图片定位区域

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>113-背景图片定位区域属性</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        ul li{
            list-style: none;
            float: left;
            width: 100px;
            height: 100px;
            text-align: center;
            line-height: 100px;
            border: 20px dashed #000;
            padding: 50px;
            margin-left: 20px;
            background: url("images/dog.jpg") no-repeat;
        }
        ul li:nth-child(2){
            /*
            告诉系统背景图片从什么区域开始显示,
            默认情况下就是从padding区域开始显示
            */
            background-origin: padding-box;
        }
        ul li:nth-child(3){
            background-origin:border-box;
        }
        ul li:nth-child(4){
            background-origin:content-box;
        }
    </style>
</head>
<body>
<ul>
    <li>默认</li>
    <li>padding</li>
    <li>border</li>
    <li>content</li>
</ul>
</body>
</html>
```



## 背景绘制区域

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>114-背景绘制区域属性</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        ul li{
            list-style: none;
            float: left;
            width: 100px;
            height: 100px;
            text-align: center;
            line-height: 100px;
            border: 20px dashed #000;
            padding: 50px;
            margin-left: 20px;
            background: red url("images/dog.jpg") no-repeat;
        }
        ul li:nth-child(2){
            /*
            背景绘制区域属性是专门用于指定从哪个区域开始绘制背景的, 默认情况下会从border区域开始绘制背景
            */
            background-clip: padding-box;
        }
        ul li:nth-child(3){
            background-clip: border-box;
        }
        ul li:nth-child(4){
            background-clip: content-box;
        }
    </style>
</head>
<body>
<ul>
    <li>默认</li>
    <li>padding</li>
    <li>border</li>
    <li>content</li>
</ul>
</body>
</html>
```



## 多重背景图片

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>115-多重背景图片</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 500px;
            height: 500px;
            border: 1px solid #000;
            margin: 0 auto;
            /*
            多张背景图片之间用逗号隔开即可
            注意点:
            先添加的背景图片会盖住后添加的背景图片
            建议在编写多重背景时拆开编写
            */
            /*background: url("images/animal1.png") no-repeat left top,url("images/animal2.png") no-repeat right top,url("images/animal3.png") no-repeat left bottom,url("images/animal4.png") no-repeat right bottom,url("images/animal5.png") no-repeat center center;*/
            background-image: url("images/animal1.png"),url("images/animal2.png"),url("images/animal3.png");
            background-repeat: no-repeat, no-repeat, no-repeat;
            background-position: left top, right top, left bottom;
        }
    </style>
</head>
<body>
<div></div>
</body>
</html>
```



## 多重背景图片练习

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>116-多重背景图片-练习</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 600px;
            height: 190px;
            border: 1px solid #000;
            margin: 100px auto;
            background-image: url("images/bg-plane.png"),url("images/bg-sun.png"), url("images/bg-clouds.png");
            background-repeat: no-repeat, no-repeat, no-repeat;
            background-size: 50px 50px, 50px 50px, auto auto;
            background-position: 50px 150px, 400px 50px, 0px 0px;
            animation: move 10s linear 0s infinite normal;
        }
        @keyframes move {
            from{
                background-position: 50px 150px, 400px 50px, 0px 0px;
            }
            to{
                background-position: 500px -150px, 400px 50px, -600px 0px;
            }
        }
    </style>
</head>
<body>
<div></div>
</body>
</html>
```



## css编写格式

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>117-CSS编写格式</title>

    <!--<link rel="stylesheet" href="117-abc.css">-->

    <style>
        @import "117-abc.css";
    </style>

    <style>
        div{
            color: blue;
        }
    </style>




</head>
<body>
<!--
1.行内样式
2.内嵌样式
3.外链样式  -- 企业开发用外链样式
4.导入样式


外链样式和导入样式区别:
共同点: 都是将CSS代码写到了一个单独的文件中
不同点:
外链样式, 在显示网页时, 会先加载CSS文件, 再显示页面
导入样式, 在显示网页时, 会先显示界面, 再加载CSS文件

外链样式是通过一个HTML标签引入CSS的
而导入样式是通过@import引入CSS的, 而@import是CSS2.1推出, 所以导入样式存在兼容问题


优先级问题
行内样式的优先级最高
其它写法谁写在后面就听谁的
-->
<!--<div style="color: red">我是div</div>-->
<div>我是div</div>

</body>
</html>
```



## 边框圆角

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>118-边框圆角基本概念</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .father{
            width: 200px;
            height: 200px;
            border: 1px solid #000;
            margin: 100px auto;
        }
        .father>.son{
            width: 200px;
            height: 200px;
            background-color: pink;
            /*
            边框圆角的作用:
            将原始的直角变为圆角
            格式:
            border-radius: 100px 0px 0px 0px;
            第一个参数: 指定左上角的半径
            按照左上/右上/右下/左下的顺序
            */
            /*border-radius: 0px 0px 0px 100px;*/
            /*如果省略一个参数, 会变成对角的值*/
            /*border-radius: 100px 80px 40px;*/
            /*如果省略两个参数, 会变成对角的值*/
            /*border-radius: 100px 80px;*/
            /*如果省略三个参数, 其它角会和它一样*/
            /*border-radius: 100px;*/
            /*如果指定的半径是当前元素宽高的一半, 那么就是一个圆形*/
            /*border-radius: 100px;*/
            border-radius: 50%;
        }
    </style>
</head>
<body>
<div class="father">
    <div class="son"></div>
</div>
</body>
</html>
```



## 单独指定圆角和椭圆

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">

    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .father{
            width: 200px;
            /*height: 200px;*/
            height: 100px;
            border: 1px solid #000;
            margin: 100px auto;
        }
        .father>.son {
            width: 200px;
            /*height: 200px;*/
            height: 100px;
            background-color: pink;
            /*单独指定某一个角的半径*/
            /*border-top-left-radius: 100px;*/
            /*border-top-right-radius: 100px;*/
            /*border-bottom-left-radius: 100px;*/
            /*border-bottom-right-radius: 100px;*/

            /*如果传递了两个参数, 那么第一个参数代表水平方向的半径, 第二个参数代表垂直方向的半径*/
            /*
            border-top-left-radius: 100px 50px;
            border-top-right-radius: 100px 50px;
            border-bottom-left-radius: 100px 50px;
            border-bottom-right-radius: 100px 50px;
            */

            /*border-top-left-radius: 50%;*/
            /*border-top-right-radius: 50%;*/
            /*border-bottom-left-radius: 50%;*/
            /*border-bottom-right-radius: 50%;*/

            /*
            斜杠之前的代表左上/右上/右下/左下的水平方向的半径
              斜杠之后的代表左上/右上/右下/左下的垂直方向的半径
              */
            /*border-radius: 100px 100px 100px 100px/50px 50px 50px 50px;*/
            border-radius: 100px/50px;
        }
        </style>
</head>
<body>
<div class="father">
    <div class="son"></div>
</div>
</body>
</html>
```



## 线性渐变

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>125-线性渐变</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 500px;
            height: 200px;
            border: 1px solid #000;
            margin: 50px auto;
            /*
            渐变不是一个新的属性, 而是一个取值
            默认情况下线性渐变是从上至下的渐变
            我们可以通过在颜色的前面添加to xxx修改渐变的方向
            to top
            to left
            to right

            */
            /*background-image: linear-gradient(to top right, red, blue);*/
            
            /*除了可以通过关键字控制渐变的方向以外, 还可以通过角度来控制渐变的方向, 角度是按照顺时针旋转*/
            /*background-image: linear-gradient(200deg, red, blue);*/


            /*30%以前不渐变, 从30%开始慢慢渐变到下一个颜色*/
            /*background-image: linear-gradient(red 30%, blue);*/
            /*background-image: linear-gradient(red 0%, red 30%,blue 30%, blue 60%, green 60%, green 100%);*/
            /*background-image: linear-gradient(to left ,red 0%, red 30%,blue 30%, blue 60%, green 60%, green 100%);*/
        }
    </style>
</head>
<body>
<div></div>
</body>
</html>
```



## 径向渐变

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>126-径向渐变</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 200px;
            height: 200px;
            margin: 100px auto;
            border: 1px solid #000;
            /*可以在颜色前面添加at和关键字来指定从什么位置开始渐变*/
            /*background-image: radial-gradient(at left top,red, blue);*/
            /*除了可以通过关键字指定从什么位置开始渐变以外, 还可以通过指定具体像素来指定从什么位置开始渐变*/
            /*background-image: radial-gradient(at 50px 50px,red, blue);*/

            /*也可以在颜色前面直接加上一个具体的像素来指定渐变的范围*/
            /*background-image: radial-gradient(150px ,red, blue);*/

            /*
            编写规范:
            如果既要指定扩散的范围, 又要指定起始位置, 那么把扩散的范围写在前面, 而起始位置写在后面
            */
            /*background-image: radial-gradient(50px at right bottom ,red, blue);*/
        }
    </style>
</head>
<body>

<div></div>
</body>
</html>
```



## 伸缩布局

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>127-伸缩布局</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .one{
            border: 1px solid #000;
            margin: 100px auto;
            list-style: none;
            overflow: hidden;
        }
        .one>li{
            float: left;
            width: 100px;
            height: 100px;
            background-color: pink;
            margin: 20px;
            text-align: center;
            line-height: 100px;
            font-size: 35px;
        }
        .two{
            border: 1px solid #000;
            margin: 0 auto;
            list-style: none;

            display: flex;
        }
        .two>li{
            width: 100px;
            height: 100px;
            background-color: pink;
            margin: 20px;
            text-align: center;
            line-height: 100px;
            font-size: 35px;
        }
    </style>
</head>
<body>
<ul class="one">
    <li>1</li>
    <li>2</li>
    <li>3</li>
</ul>

<ul class="two">
    <li>1</li>
    <li>2</li>
    <li>3</li>
</ul>
</body>
</html>
```



## 伸缩布局-主轴方向

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>128-伸缩布局-主轴方向</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        ul{
            border: 1px solid #000;
            margin: 100px auto;
            list-style: none;
            /*
            在伸缩布局中, 默认伸缩项是从左至右的排版的
            */
            display: flex;
            /*
            主轴的排版的方向默认就是row, 默认就是从左至右
            */
            /*flex-direction: row;*/
            /*
            修改主轴的排版方向为从右至左
            */
            /*flex-direction: row-reverse;*/
            /*
            告诉系统把主轴的方向改为垂直方向
            注意点:
            1.默认情况下主轴是水平方向的, 但是也可以修改为垂直方向.只要看到flex-direction: column/column-reverse就代表主轴被修改为了垂直方向
            2.如果将主轴修改为了垂直方向, 那么侧轴就会自动从垂直方向转换为水平方向
            */
            /*
            修改主轴的排版方向为从上至下
            */
            /*flex-direction: column;*/

            /*
            修改主轴的排版方向为从下至上
            */
            flex-direction: column-reverse;
        }
        ul>li{
            width: 100px;
            height: 100px;
            background-color: pink;
            text-align: center;
            line-height: 100px;
            font-size: 35px;
            margin: 20px;
        }
    </style>
</head>
<body>

<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
</ul>
</body>
</html>
```



## 伸缩布局-主轴对齐

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>128-伸缩布局-主轴方向</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        ul{
            width: 600px;
            height: 600px;
            border: 1px solid #000;
            margin: 100px auto;
            list-style: none;
            display: flex;
            /*
            justify-content:
            用于设置伸缩项主轴上的对齐方式
            如果设置为flex-start, 代表告诉系统伸缩项和主轴的起点对齐
            */
            /*justify-content: flex-start;*/
            /*justify-content: flex-end;*/
            /*居中对齐*/
            /*justify-content: center;*/
            /*两端对齐*/
            /*justify-content: space-between;*/
            /*环绕对齐*/
            justify-content: space-around;
        }
        ul>li{
            width: 100px;
            height: 100px;
            background-color: pink;
            text-align: center;
            line-height: 100px;
            font-size: 35px;
        }
        ul>li:nth-child(2){
            background-color: skyblue;
        }
        ul>li:nth-child(3){
            background-color: yellow;
        }
    </style>
</head>
<body>

<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
</ul>
</body>
</html>
```



## 伸缩布局-侧轴对齐

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>128-伸缩布局-主轴方向</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        ul{
            width: 600px;
            height: 600px;
            border: 1px solid #000;
            margin: 100px auto;
            list-style: none;
            display: flex;
            /*
            通过align-items可以修改侧轴的对齐方式
            默认情况下是以侧轴的起点对齐
            */
            /*align-items: flex-start;
            /*align-items: flex-end;*/
            /*align-items: center;*/
            /*
            注意点:
            和主轴不同的是, 侧轴没有两端对齐和环绕对齐
            */
            /*基线对齐*/
            /*align-items: baseline;*/
            /*拉伸对齐*/
            align-items: stretch;

        }
        ul>li{
            width: 100px;
            /*height: 100px;*/
            background-color: pink;
            text-align: center;
            line-height: 100px;
            font-size: 35px;
        }
        ul>li:nth-child(1){
            /*padding-top: 50px;*/
        }
        ul>li:nth-child(2){
            background-color: skyblue;
        }
        ul>li:nth-child(3){
            /*padding-top: 100px;*/
            background-color: yellow;
        }
    </style>
</head>
<body>

<ul>
    <li>j</li>
    <li>y</li>
    <li>z</li>
</ul>
</body>
</html>
```

## align-self

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>128-伸缩布局-主轴方向</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        ul{
            width: 600px;
            height: 600px;
            border: 1px solid #000;
            margin: 100px auto;
            list-style: none;
            display: flex;
        }
        ul>li{
            width: 100px;
            height: 100px;
            background-color: pink;
            text-align: center;
            line-height: 100px;
            font-size: 35px;
        }
        ul>li:nth-child(1){
            /*padding-top: 50px;*/
            /*
            如果在伸缩容器中通过 align-items 修改侧轴的对齐方式, 是修改所有伸缩项侧轴的对齐方式
            如果是在伸缩项中通过 align-self 修改侧轴的对齐方式, 是单独修改当前伸缩项侧轴的对齐方式
            align-self属性的取值和align-items一样
            */
            align-self: flex-end;
        }
        ul>li:nth-child(2){
            background-color: skyblue;
            align-self: center;
        }
        ul>li:nth-child(3){
            /*padding-top: 100px;*/
            background-color: yellow;
            align-self: flex-end;
        }
    </style>
</head>
<body>

<ul>
    <li>j</li>
    <li>y</li>
    <li>z</li>
</ul>
</body>
</html>
```



## 伸缩布局-主轴侧轴的方向问题

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>128-伸缩布局-主轴方向</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        ul{
            width: 600px;
            height: 600px;
            border: 1px solid #000;
            margin: 100px auto;
            list-style: none;
            display: flex;
            /*
            默认情况下主轴是水平方向, 侧轴是垂直方向
            默认情况下主轴的起点在伸缩容器的最左边, 主轴的终点在伸缩容器的最右边
             默认情况下侧轴的起点在伸缩容器的最顶部, 侧轴的终点在伸缩容器的最底部
             我们可以通过flex-direction属性修改主轴的方向
             如果flex-direction属性的取值是column/column-reverse
             就代表将主轴的方向修改为了垂直方向, 那么侧轴就会立刻变为水平方向
            */
            /*justify-content: space-around;*/
            /*flex-direction: column;*/
            /*align-items: center;*/
        }
        ul>li{
            width: 100px;
            height: 100px;
            background-color: pink;
            text-align: center;
            line-height: 100px;
            font-size: 35px;
        }
        ul>li:nth-child(1){

        }
        ul>li:nth-child(2){
            background-color: skyblue;
        }
        ul>li:nth-child(3){
            background-color: yellow;
        }
    </style>
</head>
<body>

<ul>
    <li>j</li>
    <li>y</li>
    <li>z</li>
</ul>
</body>
</html>
```



## 伸缩布局-设置换行

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>128-伸缩布局-主轴方向</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        ul{
            width: 400px;
            height: 600px;
            border: 1px solid #000;
            margin: 100px auto;
            list-style: none;
            display: flex;
            /*
            在伸缩布局中, 如果伸缩容器的宽度不够, 系统会自动压缩伸缩项的宽度, 保证所有的伸缩想都能放在伸缩容器中
            如果当伸缩容器宽度不够时, 不想让伸缩项被压缩, 也可以让系统换行flex-wrap: wrap;

            默认情况下如果伸缩容器的高度比换行之后所有伸缩项的高度还要高, 那么系统会自动将剩余空间平分之后添加给每一行
            */
            /*宽度不够也不换行, 默认取值*/
            /*flex-wrap: nowrap;*/
            flex-wrap: wrap;
            /*flex-wrap: wrap-reverse;*/
        }
        ul>li{
            width: 200px;
            height: 200px;
            background-color: pink;
            text-align: center;
            line-height: 200px;
            font-size: 35px;
        }
        ul>li:nth-child(1){

        }
        ul>li:nth-child(2){
            background-color: skyblue;
        }
        ul>li:nth-child(3){
            background-color: yellow;
        }
    </style>
</head>
<body>

<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
</ul>
</body>
</html>
```



## 伸缩布局-换行对齐模式

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>128-伸缩布局-主轴方向</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        ul{
            width: 400px;
            height: 600px;
            border: 1px solid #000;
            margin: 100px auto;
            list-style: none;
            display: flex;
            flex-wrap: wrap;
            /*
            如果伸缩容器中的伸缩项换行了, 那么我们就可以通过align-content来设置行与行之间的对齐方式
            */
            /*align-content: flex-start;*/
            /*align-content: flex-end;*/
            /*align-content: center;*/
            /*align-content: space-between;*/
            /*align-content: space-around;*/
            /*
            默认情况下换行就是就是拉伸对齐
            一定要注意: 在换行中的拉伸对齐是指, 所有行的高度总和要和伸缩容器的高度一样
            所以会将多余的空间平分之后添加给每一行
            */
            align-content: stretch;
        }
        ul>li{
            width: 200px;
            height: 200px;
            background-color: pink;
            text-align: center;
            line-height: 200px;
            font-size: 35px;
        }
        ul>li:nth-child(1){

        }
        ul>li:nth-child(2){
            background-color: skyblue;
        }
        ul>li:nth-child(3){
            background-color: yellow;
        }
    </style>
</head>
<body>

<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
</ul>
</body>
</html>
```



## 伸缩布局-伸缩项排序

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>128-伸缩布局-主轴方向</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        ul{
            width: 400px;
            height: 400px;
            border: 1px solid #000;
            margin: 100px auto;
            list-style: none;
            display: flex;
        }
        ul>li{
            width: 100px;
            height: 100px;
            background-color: pink;
            text-align: center;
            line-height: 100px;
            font-size: 35px;
        }
        ul>li:nth-child(1){
            /*
            如果想调整伸缩布局中伸缩项的顺序, 那么我们可以通过修改伸缩项的order属性来实现
            默认情况下order的取值是0
            如果我们设置了order属性的值, 那么系统就会按照设置的值从小到大的排序
            */
            order:-1;
        }
        ul>li:nth-child(2){
            background-color: skyblue;
            order: 2;
        }
        ul>li:nth-child(3){
            background-color: yellow;
            order: 1;
        }
    </style>
</head>
<body>

<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
</ul>
</body>
</html>
```



## 伸缩布局-伸缩项放大比

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>128-伸缩布局-主轴方向</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        ul{
            width: 400px;
            height: 400px;
            border: 1px solid #000;
            margin: 100px auto;
            list-style: none;
            display: flex;
        }
        ul>li{
            width: 100px;
            height: 100px;
            background-color: pink;
            text-align: center;
            line-height: 100px;
            font-size: 35px;
        }
        ul>li:nth-child(1){
            /*
            1.flex-grow作用:
            当所有伸缩项宽度的总和没有伸缩容器宽度大的时, 我们可以通过flex-grow让系统调整伸缩项的宽度, 以便于让所有伸缩项的宽度的总和等于伸缩容器的宽度

            2.flex-grow计算公式
            2.1计算剩余的空间
            伸缩容器宽度 - 所有伸缩项的宽度
            400 - 300 = 100
            2.2计算每份剩余空间的宽度
            剩余空间 / 需要的份数
            100 / 6 = 16.66
            2.3计算每个伸缩项最终的宽度
            伸缩项的宽度 + 需要的份数 * 每份的宽度

            注意点:
            flex-grow默认值是0
            */
            flex-grow: 1;
            /*100 + 1 * 16.66 = 116.66*/
        }
        ul>li:nth-child(2){
            background-color: skyblue;
            flex-grow: 2;
            /*100 + 2 * 16.66 = 133.3*/
        }
        ul>li:nth-child(3){
            background-color: yellow;
            flex-grow: 3;
            /*100 + 3 * 16.66 = 149.8*/
        }
    </style>
</head>
<body>

<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
</ul>
</body>
</html>
```



## 伸缩布局-伸缩项缩小比

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>128-伸缩布局-主轴方向</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        ul{
            width: 400px;
            height: 400px;
            border: 1px solid #000;
            margin: 100px auto;
            list-style: none;
            display: flex;
        }
        ul>li{
            width: 200px;
            height: 200px;
            background-color: pink;
            text-align: center;
            line-height: 200px;
            font-size: 35px;
        }
        ul>li:nth-child(1){
            /*
            1.flex-shrink作用:
              当所有伸缩项宽度的总和比伸缩容器宽度大的时, 我们可以通过flex-shrink让系统调整伸缩项的宽度, 以便于让所有伸缩项的宽度的总和等于伸缩容器的宽度

            2.计算每个伸缩项需要压缩的宽度
            2.1计算溢出的宽度
            伸缩容器的宽度 -  所有伸缩项的宽度总和
            400 - 600 = -200
            2.2计算总权重
            每个伸缩项需要的份数 * 每个伸缩项的宽度
            1 * 200 +  2 * 200 + 3 * 200 = 1200
            2.3计算每个伸缩项需要压缩的宽度
            溢出的宽度 * 需要的份数 * 每个伸缩项的宽度 / 总权重
            -200 * 1 * 200 / 1200 = -33.33

            注意点:
            flex-shrink: 默认值是1
            */
            flex-shrink: 1;
        }
        ul>li:nth-child(2){
            background-color: skyblue;
            flex-shrink: 2;
        }
        ul>li:nth-child(3){
            background-color: yellow;
            flex-shrink: 3;
        }
    </style>
</head>
<body>

<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
</ul>
</body>
</html>
```



## 伸缩布局-伸缩项自适应

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>128-伸缩布局-主轴方向</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        ul{
            width: 400px;
            height: 400px;
            border: 1px solid #000;
            margin: 100px auto;
            list-style: none;
            display: flex;
        }
        ul>li{
            width: 200px;
            height: 200px;
            background-color: pink;
            text-align: center;
            line-height: 200px;
            font-size: 35px;
        }
        ul>li:nth-child(1){
            /*
            如果有伸缩项没有设置flex-grow, 那么系统会保持原有的宽度
            而会将多余的宽度等分之后, 按照每个伸缩项需要的份数添加给它们
            如果想让某个伸缩项不缩小, 那么需要将它的flex-shrink设置为0
            */
            flex-shrink: 0;

        }
        ul>li:nth-child(2){
            background-color: skyblue;
            flex-grow: 1;
        }
        ul>li:nth-child(3){
            background-color: yellow;
            flex-grow: 4;
        }
    </style>
</head>
<body>

<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
</ul>
</body>
</html>
```



## 伸缩布局-伸缩项宽度

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>128-伸缩布局-主轴方向</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        ul{
            width: 800px;
            height: 400px;
            border: 1px solid #000;
            margin: 100px auto;
            list-style: none;
            display: flex;
        }
        ul>li{
            /*width: 200px;*/
            height: 200px;
            background-color: pink;
            text-align: center;
            line-height: 200px;
            font-size: 35px;
        }
        ul>li:nth-child(1){
            /*
            如果是伸缩布局, 除了可以通过元素的width属性来设置宽度以外, 还可以通过flex-basis属性来设置伸缩项的宽度

            注意点:
            1.width属性可以设置宽度/flex-basis也可以设置宽度
            那么如果两者同时存在, 系统会听flex-basis的
            2.flex-basis属性是专门提供给伸缩布局使用的
            3.如果同时通过这两个属性设置了宽度, 但是其中一个是auto, 那么系统会按照具体值来设置
            */
            /*flex-basis: 100px;*/
            /*width: 200px;*/
            /*width: auto;*/
            flex-basis: auto;
        }
        ul>li:nth-child(2){
            background-color: skyblue;
            flex-basis:200px;
        }
        ul>li:nth-child(3){
            background-color: yellow;
            flex-basis:300px;
        }
    </style>
</head>
<body>

<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
</ul>
</body>
</html>
```



## 伸缩布局-伸缩项属性简写

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>128-伸缩布局-主轴方向</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        ul{
            width: 800px;
            height: 400px;
            border: 1px solid #000;
            margin: 100px auto;
            list-style: none;
            display: flex;
        }
        ul>li{
            width: 200px;
            height: 200px;
            background-color: pink;
            text-align: center;
            line-height: 200px;
            font-size: 35px;
        }
        ul>li:nth-child(1){
            /*
            flex: flex-grow flex-shrink flex-basis;
            默认值:
            flex: 0 1 auto;

            注意点:
            在连写格式中, flex-shrink flex-basis是可以省略的
            */
            /*flex: 1 1 200px;*/
            flex: 1;
        }
        ul>li:nth-child(2){
            background-color: skyblue;
            /*flex: 1 2 200px;*/
            flex: 2;
        }
        ul>li:nth-child(3){
            background-color: yellow;
            /*flex: 1 3 200px;*/
            flex: 3;
        }
    </style>
</head>
<body>

<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
</ul>
</body>
</html>
```



## 媒体查询

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>143-媒体查询</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        /*
        需求:
        PC显示小牛(大于等于980)
        pad显示小猪(小于980, 并且大于等于620)
        phone显示小马(小于620)
        */

        /*如果浏览器的宽度大于等于980, 那么就执行后面{}中的代码*/
        @media(min-width: 980px) {
            body{
                background: url("images/animal1.png");
            }
        }
        /*如果浏览器的宽度大于等于620, 并且宽度小于980*/
        /*注意点: and的两边必须添加空格*/
        @media(min-width: 620px) and (max-width: 979px) {
            body{
                background: url("images/animal2.png");
            }
        }
        @media(max-width: 619px) {
            body{
                background: url("images/animal3.png");
            }
        }
    </style>
</head>
<body>

</body>
</html>
```



## 媒体查询-外链式

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>144-媒体查询-外链式</title>
    <link rel="stylesheet" href="144-style-pc.css" media="(min-width:980px)">
    <link rel="stylesheet" href="144-style-pad.css" media="(min-width:620px) and (max-width:979px)">
    <link rel="stylesheet" href="144-style-phone.css" media=" (max-width:619px)">

</head>
<body>

</body>
</html>
```



## grid布局

https://enen.me/wp-content/uploads/2021/11/grid.pdf

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .main {
            display: grid;
            width: 600px;
            height: 600px;
            border: 10px solid skyblue;
            grid-template-columns: 100px 100px 100px;
            /* 等价于 */
            grid-template-columns: repeat(3,100px);
            /* auto-fill的意思是如果一行能放下多少就放下多少，放不下的就换行*/
            grid-template-columns: repeat(auto-fill,100px);
            /* 为了方便表示比例关系，网格布局提供了fr关键字（fraction 的缩写，意为"片段"）*/
            grid-template-columns: repeat(3,1fr);
            /* minmax()，函数产生一个长度范围，表示长度就在这个范围之中，它接受两个参数，分别为最小值和最大值 */
            grid-template-columns: 1fr minmax(150px,1fr);
            /* auto，表示由浏览器自己决定长度 */
            grid-template-columns: 100px auto 100px;
            /* 网格线，可以用方括号定义网格线名称，方便以后的引用 */
            grid-template-columns: [c1] 100px [c2] 100px [c3] 100px [c4];
            /* 间隔 */
            row-gap: 20px;
            column-gap: 20px;
            gap: 20px 20px;
            /* 一个区域由单个或多个单元格组成，由你决定 (具体使用，需要在项目属性里面设置) */
            grid-template-areas: 'a b c'
                                 'd e f'
                                 'g h i';
            grid-template-areas: 'a a a'
                                 'b b b'
                                 'c c c';
            grid-template-areas: 'a . c'
                                 'd . f'
                                 'g . i';
            /* 划分网格以后，容器的子元素会按照顺序，自动放置在每一个网格。默认的放置顺序是“先行后列”，即先填满第一行，再开始放入第二行 */
            grid-auto-flow: row; 
            grid-auto-flow: column;
            grid-auto-flow: row dense;
            /* 容器属性 justify-items(水平方向) / align-items (垂直方向) */
            /*justify-items: start | end | center | stretch;*/
            /*align-items: start | end | center | stretch;*/
            /* place-items属性是align-items属性和justify-items属性的合并简写形式 */
            /* 容器属性 justify-content (水平方向) / align-content (垂直方向) */
            /*justify-content: start | end | center | stretch | space-around | space-between | space-evenly;*/
            /*align-content: start | end | center | stretch | space-around | space-between | space-evenly;*/
            
            /* grid-auto-columns / grid-auto-rows 用来设置多出来的项目宽和高*/
            grid-auto-rows: 50px;

            /* grid-column-start / grid-column-end / grid-row-start / grid-row-end一句话解释，用来指定item的具体位置, 根据在哪根网格线 */
            /* 列方向4根网格线，第一个项目占用了从第一根到第三根 */
            grid-column-start: 1;
            grid-column-end: 3;
            /* 简写 */
            grid-column: 1 / 3;
            grid-column-start: span 2;
            /* grid-area指定项目放在哪一个区域 配合grid-template-areas使用 */
            grid-area: b;

            /* grid-area属性还可用作grid-row-start、grid-column-start、grid-row-end、grid-column-end的合并简写形式，直接指定项目的位置
            grid-area: <row-start> / <column-start> / <row-end> / <column-end>; */
            grid-area: 1 / 1 / 3 / 3;

            /* justify-self / align-self / place-self  设置项目自己*/

            grid-template-rows: 100px 100px 100px 100px;

        }
        .item {
            background-color: red;
        }
    </style>
</head>
<body>
    <div class="mian">
        <div class="item item1"></div>
        <div class="item item2"></div>
        <div class="item item3"></div>
        <div class="item item4"></div>
        <div class="item item5"></div>
        <div class="item item6"></div>
        <div class="item item7"></div>
        <div class="item item8"></div>
        <div class="item item9"></div>
        <div class="item item10"></div>
    </div>
</body>
</html>
```



## 毛玻璃

```css
-webkit-backdrop-filter: blur(25px)
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        body {
            background: url("./images/bg.jpg");
        }
        div {
            width: 300px;
            height: 500px;
            background: transparent;
            margin: 0 auto;
            box-shadow: 0 25px 50px rgba(0, 0, 0, .2);
            backdrop-filter: blur(50px);
        }
    </style>
</head>
<body>
    <div></div>
</body>
</html>
```



## 垂直居中

```html
单行文字：
line-height和height设置一样
```

```css
多行文字:
div {
    background: blue;
    height: 210px;
    width: 170px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    text-align: center;
}
```

```css
grid布局:
div {
    display: grid;
    grid-template-columns: repeat(6,1fr);
    align-items: center;
    justify-content: center;
}
```

```css
绝对定位
```

```css
vertical-align加伪元素
div.search {
	display: inline-block;
	vertical-align: middle;
}
div.main::after {
	content: "",
	display: inline-block;
	background-color: gold;
	height: 100%;
	width: 0;
	vertical-align: middle;
}
```



## 水平居中

```html
1.行内元素
text-align: center;
2.块级元素
margin: 0 auto;
```



## 水平垂直居中

```css
1.绝对定位transform
2.绝对定位:left 0 right 0 bottom 0 top 0
3.flex
div {
	display: flex;
	align-items: center;
	justify-content: center;
}
4.伪元素和vertical-align
```

## 文字显示两行

```css
p {
	overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
}
```

## BFC 定义

> BFC(Block formatting context)直译为"块级格式化上下文"。它是一个独立的渲染区域，只有Block-level box参与， 它规定了内部的Block-level Box如何布局，并且与这个区域外部毫不相干。

在解释什么是BFC之前，我们需要先知道Box、Formatting Context的概念。

Box：css布局的基本单位
Box 是 CSS 布局的对象和基本单位， 直观点来说，就是一个页面是由很多个 Box 组成的。元素的类型和 display 属性，决定了这个 Box 的类型。 不同类型的 Box， 会参与不同的 Formatting Context（一个决定如何渲染文档的容器），因此Box内的元素会以不同的方式渲染。让我们看看有哪些盒子：

block-level box:display 属性为 block, list-item, table 的元素，会生成 block-level box。并且参与 block fomatting context；
inline-level box:display 属性为 inline, inline-block, inline-table 的元素，会生成 inline-level box。并且参与 inline formatting context；
run-in box: css3 中才有， 这儿先不讲了。
Formatting Context
Formatting context 是 W3C CSS2.1 规范中的一个概念。它是页面中的一块渲染区域，并且有一套渲染规则，它决定了其子元素将如何定位，以及和其他元素的关系和相互作用。最常见的 Formatting context 有 Block fomatting context (简称BFC)和 Inline formatting context (简称IFC)。

BFC是一个独立的布局环境，其中的元素布局是不受外界的影响，并且在一个BFC中，块盒与行盒（行盒由一行中所有的内联元素所组成）都会垂直的沿着其父元素的边框排列。

BFC的布局规则
内部的Box会在垂直方向，一个接一个地放置。

Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠。

每个盒子（块盒与行盒）的margin box的左边，与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。

BFC的区域不会与float box重叠。

BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。

计算BFC的高度时，浮动元素也参与计算。

如何创建BFC
1、float的值不是none。
2、position的值不是static或者relative。
3、display的值是inline-block、table-cell、flex、table-caption或者inline-flex
4、overflow的值不是visible

**BFC的作用**

1.利用BFC避免margin重叠。

2.自适应两栏布局(一个浮动，一个overflow:hidden)

3.清楚浮动。

**总结**
以上例子都体现了：

BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。

因为BFC内部的元素和外部的元素绝对不会互相影响，因此， 当BFC外部存在浮动时，它不应该影响BFC内部Box的布局，BFC会通过变窄，而不与浮动有重叠。同样的，当BFC内部有浮动时，为了不影响外部元素的布局，BFC计算高度时会包括浮动的高度。避免margin重叠也是这样的一个道理。

## inline和block的区别

display:block特点

1、独占一行，多个block元素另起一行，默认情况下，block元素宽度自动填满其父元素宽度

2、block元素可以设置width,height属性。块元素即使设置了宽度,仍然是独占一行。

3、block元素可以设置margin和padding属性。

display:inline特点

1、inline元素不会独占一行，多个相邻的行内元素会排列在同一行里，直到一行排列不下，才会新换一行，其宽度随元素的内容而变化

2、inline元素设置width,height属性无效。

3、inline元素的margin和padding属性，水平方向的padding-left, padding-right, margin-left, margin-right都产生边距效果；但竖直方向的padding-top, padding-bottom, margin-top, margin-bottom不会产生边距效果。

## 什么是文档流

文档流指的是文档中的[标签](https://so.csdn.net/so/search?q=标签&spm=1001.2101.3001.7020)在排列时所占用的位置。 将窗体自上而下分成一行行 ，并在每行中按从左至右的顺序排放标签，即为文档流。 也就是说在文档流中标签默认会紧贴到上一个标签的右边，如果右边不足以放下标签，标签则会另起一行，在新的一行中继 续从左至右摆放。

目前常见的会影响元素脱离文档流的css属性有：
①float浮动。
②position的absolute和fixed定位。
