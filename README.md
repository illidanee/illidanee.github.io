> Windowsʹ��Jekyll�����Blog����������Github��



## ����

* [Git](https://git-scm.com/)
* [Ruby](http://rubyinstaller.org/downloads/)
* [Rubydevkit](http://rubyinstaller.org/downloads/)
* [Jekyll](http://jekyllrb.com/)
* [Jekyll����](https://github.com/Gaohaoyang/gaohaoyang.github.io)

## ��װ����

> ��װGit

ֱ�Ӱ�װ��

> ��װRuby

ֱ�Ӱ�װ��ѡ��Ŀ¼C:\Ruby22��

> ��װRubyDevkit

��ѹRubyDevkit��Ŀ¼C:\DevKit����CMD�����Ŀ¼ִ��

	ruby dk.rb init
	
�������������config.yml���򿪲��༭�����Ruby�İ�װĿ¼���£�


	---
	- C:\Ruby22


Ȼ��ִ��

	ruby dk.rb install
	
> ��װJekyll

	gem install jekyll

> Clone Jekyll����

	git clone https://github.com/Gaohaoyang/gaohaoyang.github.io
	
> ����Jekyll

��CMD��������Ŀ¼����ִ��

	jekyll server

## ����

### ��װjekyll����1

�������£�

	C:\DevKit>gem install jekyll
	ERROR:  Could not find a valid gem 'jekyll' (>= 0), here is why:
			  Unable to download data from https://gems.ruby-china.org/ - SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B: certificate verify failed (https://gems.ruby-china.org/specs.4.8.gz)

�����

	1. ��װ֤�飺�ڵ�ַhttp://curl.haxx.se/ca/cacert.pem����cacert.pem�ļ���Ŀ¼C:\Ruby22\bin��Ȼ����ӻ���������

		SSL_CERT_FILE = C:\Ruby22\bin\cacert.pem

	2. ����gem sources

		gem sources --add https://gems.ruby-china.org/ --remove https://rubygems.org/
			  
### ��װjekyll����2

�������£�

	ERROR:  Error installing jekyll:
			invalid gem: package is corrupt, exception while verifying: undefined method `size' for nil:NilClass (NoMethodError) in C:/Ruby22-x64/lib/ruby/gems/2.2.0/cache/jekyll-3.3.0.gem

�����

	ɾ���ļ�C:/Ruby22-x64/lib/ruby/gems/2.2.0/cache/jekyll-3.3.0.gem�����°�װ��
	
### ����Jekyll����1

�������£�

	C:\Users\Administrator\Desktop\gaohaoyang.github.io>jekyll server Configuration file: C:/Users/Administrator/Desktop/gaohaoyang.github.io/_config.yml
	Configuration file: C:/Users/Administrator/Desktop/gaohaoyang.github.io/_config.yml
	  Dependency Error: Yikes! It looks like you don't have jekyll-paginate or one of its dependencies installed. In order to use Jekyll as currently configured, you'll need to install this gem. The full error message from Ruby is: 'cannot load such file -- jekyll-paginate' If you run into trouble, you can find helpful resources at http://jekyllrb.com/help/!jekyll 3.3.0 | Error:  jekyll-paginate
	
�����

	gem install jekyll-paginate
	

### ����Jekyll����2

�������£�

	--watch arg is unsupported on Windows.If you are on Windows Bash, please see: https://github.com/Microsoft/BashOnWindows/issues/216
	
�����
	
��װJekyll3.2.1�汾��

	gem uninstall jekyll
	gem install jekyll 3.2.1
	
## �ο�


* [Jekyll ���̬����](https://gaohaoyang.github.io/2015/02/15/create-my-blog-with-jekyll/)
* [Jekyll��github�Ϲ�����ѵ�WebӦ��](http://blog.fens.me/jekyll-bootstarp-github/)
* [Download a cacert.pem for RailsInstaller](https://gist.github.com/fnichol/867550)����Ҫ����ǽ��
* [RubyGems ����- Ruby China](https://gems.ruby-china.org/)
* [liquid�÷��ʼ�](http://blog.csdn.net/dont27/article/details/38097581)
