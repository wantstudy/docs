_一天一个小命令_


*Linux 命令大全 [参考文档](http://man.linuxde.net/)*

## 文件操作

###  ls 查看文件夹下的内容

`ls <commond> <fileName/path>
   -a 查看当前目录下所有文件,包括隐藏文件
   -l 显示文件详细信息,等同于 ll`

  ![Alt ll](../../_media/linux/ll.png)

###  jar 对jar/war文件进行操作

 *用法:* `jar {ctxui}[vfmn0PMe] [jar-file] [manifest-file] [entry-point] [-C dir] files ...`

*选项：*  

	   -c 创建新文档

	   -t  列出档案目录

	   -x  从档案中提取指定的 (或所有) 文件

       -u  更新现有档案

       -v  在标准输出中生成详细输出

       -f  指定档案文件名

       -e  为捆绑到可执行 jar 文件的独立应用程序指定应用程序入口点

       -C  更改为指定的目录并包含以下文件`

*示例 1:*  将两个类文件归档到一个名为 classes.jar 的档案中:

       jar cvf classes.jar Foo.class Bar.class

*示例 2:*  使用现有的清单文件 'mymanifest' 并将 foo/ 目录中的所有文件归档到 'classes.jar' 中: 

       jar cvfm classes.jar mymanifest -C foo/ .


*示例 3:*  替换jar/war包内容

	jar tvf vedio.war | grep MediaController  查找文件所在目录

 	jar xvf vedio.war /WEB-INF/classes/com/cygps/controller/MediaController 解压指定文件

 	cp MediaController.class WEB-INF/classes/com/ivmscy/controller/  复制文件到解压后的目录

 	jar uvf vedio.war WEB-INF/classes/com/ivmscy/controller/  添加文件到war包
