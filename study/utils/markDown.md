安装配置
-----
[Sublime Text3 下载安装](http://www.sublimetext.com/3)
	
   `注册码
    ----- BEGIN LICENSE -----
	sgbteam
	Single User License
	EA7E-1153259
	8891CBB9 F1513E4F 1A3405C1 A865D53F
	115F202E 7B91AB2D 0D2A40ED 352B269B
	76E84F0B CD69BFC7 59F2DFEF E267328F
	215652A3 E88F9D8F 4C38E3BA 5B2DAAE4
	969624E7 DC9CD4D5 717FB40C 1B9738CF
	20B3C4F1 E917B5B3 87C38D9C ACCE7DD8
	5F7EF854 86B9743C FADC04AA FB0DA5C0
	F913BE58 42FEA319 F954EFDD AE881E0B
	------ END LICENSE ------`

> **添加插件**

   *点击  View > Show Console  ，输入以下代码执行，然后重启编辑器*

  `import urllib.request,os,hashlib; h = '6f4c264a24d933ce70df5dedcf1dcaee' + 'ebe013ee18cced0ef93d5f746d80ef60'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)`

MarkDown 介绍
----------
> __Markdown 的目标是实现「易读易写」。__
	
   *参考[Markdown 文档说明](https://www.appinn.com/markdown/basic.html)*

	可读性，无论如何，都是最重要的。一份使用 Markdown 格式撰写的文件应该可以直接以纯文本发布，并且看起来不会像是
	由许多标签或是格式指令所构成。Markdown 语法受到一些既有 text-to-HTML 格式的影响，包括 Setext、atx、
	Textile、reStructuredText、Grutatext 和 EtText，而最大灵感来源其实是纯文本电子邮件的格式。

	总之， Markdown 的语法全由一些符号所组成，这些符号经过精挑细选，其作用一目了然。比如：在文字两旁加上星号，
	看起来就像*强调*。Markdown 的列表看起来，嗯，就是列表。Markdown 的区块引用看起
	来就真的像是引用一段文字，就像你曾在电子邮件中见过的那样。

标题
-------
> __Markdown 支持两种标题的语法，类 `Setext` 和类 `atx` 形式。__

	类Setext形式是用底线的形式，利用 =（最高阶标题）和 - （第二阶标题），任何数量的 = 和 - 都可以有效果，例如：
		
		This is an H1
		============
		This is an H2
		----------

	类 Atx 形式则是在行首插入 1 到 6 个 # ，对应到标题 1 到 6 阶，例如：
		
		# 这是 H1
		## 这是 H2
		###### 这是 H6

区块引用 Blockquotes
-------

> __Markdown 标记区块引用是使用类似 email 中用 > 的引用方式。如果你还熟悉在 email 信件中的引言部分，你就知道怎么在 Markdown 文件中建立一个区块引用，那会看起来像是你自己先断好行，然后在每行的最前面加上 > ：__


	> This is the first level of quoting.
	>
	> > This is nested blockquote.
	>
	> Back to the first level.

> __引用的区块内也可以使用其他的 Markdown 语法，包括标题、列表、代码区块等：__

	> ## 这是一个标题。
	> 
	> 1.   这是第一行列表项。
	> 2.   这是第二行列表项。
	> 
	> 给出一些例子代码：
	> 
	>     return shell_exec("echo $input | $markdown_script");

列表
----

> __Markdown 支持有序列表和无序列表。__
	
	无序列表使用星号、加号或是减号作为列表标记

	*   Red  	+   Red  	-   Red
	*   Green	+   Green	-   Green
	*   Blue 	+   Blue 	-   Blue


	有序列表则使用数字接着一个英文句点：

	1.  Bird
	2.  McHale
	3.  Parish

链接
----

> __Markdown 支持两种形式的链接语法： 行内式和参考式两种形式,不管是哪一种，链接文字都是用 [方括号] 来标记。__
	
	要建立一个行内式的链接，只要在方块括号后面紧接着圆括号并插入网址链接即可，如果你还想要加上链接的 title 文字，
	只要在网址后面，用双引号把 title 文字包起来即可，例如：

	This is [an example](http://example.com/ "Title") inline link.

	[This link](http://example.net/) has no title attribute.

	会产生：
	<p>This is <a href="http://example.com/" title="Title">
	an example</a> inline link.</p>

	<p><a href="http://example.net/">This link</a> has no
	title attribute.</p>

强调
----

>	__Markdown 使用星号（*）和底线（_）作为标记强调字词的符号，被 * 或 _ 包围的字词会被转成用 `<em>` 标签包围，用两个 * 或 _ 包起来的话，则会被转成 `<strong>`，__
	例如：

	*single asterisks*

	_single underscores_

	**double asterisks**

	__double underscores__

	会转成：

	<em>single asterisks</em>

	<em>single underscores</em>

	<strong>double asterisks</strong>

	<strong>double underscores</strong>

代码
----

> __如果要标记一小段行内代码，你可以用反引号把它包起来（`），例如__

	Use the `printf()` function.

	会产生：

	<p>Use the <code>printf()</code> function.</p>

图片
----

> __很明显地，要在纯文字应用中设计一个「自然」的语法来插入图片是有一定难度的。Markdown 使用一种和链接很相似的语法来标记图片，同样也允许两种样式： 行内式和参考式。__

   __行内式__的图片语法看起来像是：

	一个惊叹号 !
	接着一个方括号，里面放上图片的替代文字
	接着一个普通括号，里面放上图片的网址，最后还可以用引号包住并加上 选择性的 'title' 文字。

	![Alt text](/path/to/img.jpg)

	![Alt text](/path/to/img.jpg "Optional title")


反斜杠
----

> __Markdown 可以利用反斜杠来插入一些在语法中有其它意义的符号，例如：如果你想要用星号加在文字旁边的方式来做出强调效果（但不用 `<em>` 标签），你可以在星号的前面加上反斜杠：__

	\*literal asterisks\*

	Markdown 支持以下这些符号前面加上反斜杠来帮助插入普通的符号：

	\   反斜线
	`   反引号
	*   星号
	_   底线
	{}  花括号
	[]  方括号
	()  括弧
	#   井字号
	+   加号
	-   减号
	.   英文句点
	!   惊叹号


表格
-----

**代码**
```
| 一个普通标题 | 一个普通标题 | 一个普通标题 |
| ------ | ------ | ------ |
| 短文本 | 中等文本 | 稍微长一点的文本 |
| 稍微长一点的文本 | 短文本 | 中等文本 |
```

**效果如下**

| 一个普通标题 | 一个普通标题 | 一个普通标题 |
| ------ | ------ | ------ |
| 短文本 | 中等文本 | 稍微长一点的文本 |
| 稍微长一点的文本 | 短文本 | 中等文本 |

**语法说明**

	1）|、-、:之间的多余空格会被忽略，不影响布局。
	2）默认标题栏居中对齐，内容居左对齐。
	3）-:表示内容和标题栏居右对齐，:-表示内容和标题栏居左对齐，:-:表示内容和标题栏居中对齐。
	4）内容和|之间的多余空格会被忽略，每行第一个|和最后一个|可以省略，-的数量至少有一个。
