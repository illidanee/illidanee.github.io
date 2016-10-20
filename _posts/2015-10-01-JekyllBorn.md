---
layout: post
title:  JekyllBorn
categories: 
- Born
tags: 
- Born
---

* content
{:toc}

> Windows使用Jekyll搭建个人Blog，并发布到Github。




## 工具

* [Git](https://git-scm.com/)
* [Ruby](http://rubyinstaller.org/downloads/)
* [Rubydevkit](http://rubyinstaller.org/downloads/)
* [Jekyll](http://jekyllrb.com/)
* [Jekyll主题](https://github.com/Gaohaoyang/gaohaoyang.github.io)

## 安装配置

> 安装Git

直接安装。

> 安装Ruby

直接安装。选择目录C:\Ruby22。

> 安装RubyDevkit

解压RubyDevkit到目录C:\DevKit。打开CMD进入该目录执行

`ruby dk.rb init`
	
上面的命令生成config.yml，打开并编辑，添加Ruby的安装目录如下：

`
---
- C:\Ruby22
`

然后执行

	ruby dk.rb install
	
> 安装Jekyll

	gem install jekyll

> Clone Jekyll主题

	git clone https://github.com/Gaohaoyang/gaohaoyang.github.io
	
> 运行Jekyll

打开CMD进入主题目录，并执行

	jekyll server

## 问题

### 安装jekyll错误1

错误如下：

	C:\DevKit>gem install jekyll
	ERROR:  Could not find a valid gem 'jekyll' (>= 0), here is why:
			  Unable to download data from https://gems.ruby-china.org/ - SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B: certificate verify failed (https://gems.ruby-china.org/specs.4.8.gz)

解决：

	1. 安装证书：在地址http://curl.haxx.se/ca/cacert.pem下载cacert.pem文件到目录C:\Ruby22\bin。然后添加环境变量。

		SSL_CERT_FILE = C:\Ruby22\bin\cacert.pem

	2. 更新gem sources

		gem sources --add https://gems.ruby-china.org/ --remove https://rubygems.org/
			  
### 安装jekyll错误2

错误如下：

	ERROR:  Error installing jekyll:
			invalid gem: package is corrupt, exception while verifying: undefined method `size' for nil:NilClass (NoMethodError) in C:/Ruby22-x64/lib/ruby/gems/2.2.0/cache/jekyll-3.3.0.gem

解决：

	删除文件C:/Ruby22-x64/lib/ruby/gems/2.2.0/cache/jekyll-3.3.0.gem，重新安装。
	
### 运行Jekyll错误1

错误如下：

	C:\Users\Administrator\Desktop\gaohaoyang.github.io>jekyll server Configuration file: C:/Users/Administrator/Desktop/gaohaoyang.github.io/_config.yml
	Configuration file: C:/Users/Administrator/Desktop/gaohaoyang.github.io/_config.yml
	  Dependency Error: Yikes! It looks like you don't have jekyll-paginate or one of its dependencies installed. In order to use Jekyll as currently configured, you'll need to install this gem. The full error message from Ruby is: 'cannot load such file -- jekyll-paginate' If you run into trouble, you can find helpful resources at http://jekyllrb.com/help/!jekyll 3.3.0 | Error:  jekyll-paginate
	
解决：

	gem install jekyll-paginate
	

### 运行Jekyll错误2

错误如下：

	--watch arg is unsupported on Windows.If you are on Windows Bash, please see: https://github.com/Microsoft/BashOnWindows/issues/216
	
解决：
	
安装Jekyll3.2.1版本：

	gem uninstall jekyll
	gem install jekyll 3.2.1
	
## 参考


* [Jekyll 搭建静态博客](https://gaohaoyang.github.io/2015/02/15/create-my-blog-with-jekyll/)
* [Jekyll在github上构建免费的Web应用](http://blog.fens.me/jekyll-bootstarp-github/)
* [Download a cacert.pem for RailsInstaller](https://gist.github.com/fnichol/867550)
* [RubyGems 镜像- Ruby China](https://gems.ruby-china.org/)
* [liquid用法笔记](http://blog.csdn.net/dont27/article/details/38097581)

