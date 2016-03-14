---
title: hexo那些踩过的坑
date: 2016-03-13 11:07:15
tags: [learning]
---
在几经波折，经历了初试的磨砺，和过了N月之后的事故(删掉了本地环境，还没有备份)，我终于决定将所踩过的坑记录下来，便于查阅

** yilia主题语言类别问题 **
	 这个本来自认为很顺理成章的问题，觉得只要修改公共配置_config.xml中的language选项，并修改为zh-CN就万事大吉了，但其实这里需要和主题中language文件夹下的某个语言文件的名字相同。比如我用的中文简体，则需要设置为zh-Hans即可。

** 删除已生成的文章 **
	
	delete db.json
	hexo clean
	hexo g

** 备份备份备份，重要的事情要说三遍 **
	搭博客，前前后后共搭了3次，一次是因为自己搞的太乱了，总是有问题，然后解决无果之后删掉重来一遍，但时隔无多，记得比较清楚。第三次就没这么幸运了，已经写好的文章，修改后的主题，统统都没有了，所以一时烦躁。但是也知道这次才发现，原来我是需要备份，好吧，如果你要笑我，就尽情的笑吧。真的是被自己蠢死了。经过几许折腾，终于重新搭了起来。这次我要备份。
	最简单的就是用github备份了，因为有.gitignore啊。
	提交步骤:

	(1) git init
	(2) git add .(当前文件夹下的全部文件)
	(3) git commit -m "say what you like"
	(4) git remote add origin 'https://github.com/shuyulxf/shuyu_blog_bak.git/
	(5) git push -u origin master


可能会踩到的坑：
在第4步的时候可能会报fatal: remote origin already exists.这样的错误。只需要执行如下命令就可以了

	git remote rm origin

还可能会出现fatal: repository https://github.com/shuyulxf/shuyu_blog_bak.git/ not found这样的问题，其实很弱智的啦，我确犯了。一定要现在github上创建该仓库哦。


** 附上一份安装过程吧 ** 

	(1)安装nodejs
	(2)安装hexo  npm install hexo-cli -g
	(3)hexo init 
	(4)npm install hexo-deployer-git --save

此时环境以及全部搭完。常见修改：

	(1)配置自己喜欢的主题
	下载，修改_config.xml中的theme参数
	(2)启动本地服务  hexo s 
	有时候4000端口可能起不来，因为被占用或者其它原因，这时可以修改_config.xml中的port参数，或者用hexo server -p 5000
	(3)生产public静态文件  hexo g
	(4)部署 hexo d
	先配置仓库信息_config.xml中deploy 选项，也需要先配置git的email和name，会提示的。



	
	