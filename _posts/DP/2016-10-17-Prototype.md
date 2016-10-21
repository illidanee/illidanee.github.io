---
layout: post
title: Prototype
categories: 
 - DP
tags:
 - DP
---

* content
{:toc}

> 设计模式系列文章参考`GeekBand-李建忠`老师课堂内容整理。如有意购买该课程请访问网易云课堂。

## 动机

1. 在软件系统中，经常面临着`某些结构复杂的对象`的创建工作；由于需求的变化，这些对象经常面临着剧烈的变化，但是他们却拥有比较稳定一致的接口。
2. 如何应对这种变化？如何向`客户程序(使用这些对象的程序)`隔离出`这些易变对象`，从而使得`依赖这些易变对象的客户程序`不随着需求改变而改变？




## 定义

	使用原型实例指定创建对象的种类，然后通过拷贝这些原型来创建新的对象。
	
## 伪码

```c
//抽象类
class ISplitter
{
public:
	virtual ISplitter* clone() = 0; 	//通过克隆自己来创建对象
};

//具体类
class BinarySplitter : public ISplitter
{
public:
	virtual ISplitter* clone()
	{
		return new BinarySplitter(*this);
	}
};

class TxtSplitter: public ISplitter
{
public:
	virtual ISplitter* clone()
	{
		return new TxtSplitter(*this);
	}
};

class PictureSplitter: public ISplitter
{
public:
	virtual ISplitter* clone()
	{
		return new PictureSplitter(*this);
	}
};

class VideoSplitter: public ISplitter
{
public:
	virtual ISplitter* clone()
	{
		return new VideoSplitter(*this);
	}
};

class MainForm : public Form
{
	ISplitter*  prototype;					//原型对象

public:
    
	MainForm(ISplitter*  prototype)
	{
		this->prototype = prototype;
	}

	void Button1_Click()
	{
		ISplitter * splitter = prototype->clone(); 		//克隆原型
		splitter->split();
	}
};
```

## 要点

1. Prototype模式同样用于隔离类对象的使用者和具体类型(易变类)之间的耦合关系，他们同样要求这些`易变类`拥有`稳定的接口`。
2. Prototype模式对于`如何创建易变类的实体对象`采用`原型克隆`的方法来做，它使得我们可以非常灵活地动态创建`拥有某些稳定接口`的新对象-所需工作仅仅是注册一个新类的对象(即原型)，然后在任何需要的地方Clone。
3. Prototype模式中的Clone方法可以利用某些框架中的序列化来实现深拷贝。


