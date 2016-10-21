---
layout: post
title: FactoryMethod
categories: 
 - DP
tags:
 - DP
---

* content
{:toc}

> 设计模式系列文章参考`GeekBand-李建忠`老师课堂内容整理。如有意购买该课程请访问网易云课堂。

## 动机

1. 在软件系统中，经常面临着创建对象的工作。由于需求的变化，需要创建的对象的具体类型经常变化。
2. 如何应对这种变化？如何绕过常规的对象创建方法（new）,提供一种`封装机制`来避免客户程序和这种`具体对象创建工作`的紧耦合？




## 定义

	定义一个用于创建对象的接口，让子类决定实例化哪一个类。FactoryMethod使得一个类的实例化延迟(目的：解耦，手段：虚函数)到子类。

## 伪码

```c
//抽象类
class ISplitter
{
public:
	virtual void split() = 0;
};

//具体类
class BinarySplitter : public ISplitter { };

class TxtSplitter: public ISplitter { };

class PictureSplitter: public ISplitter { };

class VideoSplitter: public ISplitter { };

//抽象工厂类
class SplitterFactory
{
public:
	virtual ISplitter* CreateSplitter() = 0;
};

//具体工厂类
class BinarySplitterFactory: public SplitterFactory
{
public:
	virtual ISplitter* CreateSplitter()
	{
		return new BinarySplitter();
	}
};

class TxtSplitterFactory: public SplitterFactory
{
public:
	virtual ISplitter* CreateSplitter()
	{
		return new TxtSplitter();
	}
};

class PictureSplitterFactory: public SplitterFactory
{
public:
	virtual ISplitter* CreateSplitter()
	{
		return new PictureSplitter();
	}
};

class VideoSplitterFactory: public SplitterFactory
{
public:
	virtual ISplitter* CreateSplitter()
	{
		return new VideoSplitter();
	}
};

class MainForm : public Form
{
	SplitterFactory*  factory;//工厂

public:
	MainForm(SplitterFactory*  factory)
	{
		this->factory=factory;
	}
    
	void Button1_Click()
	{
		ISplitter * splitter = factory->CreateSplitter(); 	//多态new
		splitter->split();
	}
};
```

## 要点

1. Factory Method模式用于隔离类对象的使用者和具体类型之间的耦合关系。面对一个经常变化的具体类型，紧耦合关系(new)会导致软件的脆弱。
2. Factory Method模式通过面向对象的手法，将所要创建的具体对象工作延迟到子类，从而实现一种扩展(而非更改)的策略，较好的解决了这种紧耦合关系。
3. Factory Method模式解决`单个对象`的需求变化。缺点在于要求创建方法和参数相同。

