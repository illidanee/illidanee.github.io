---
layout: post
title: TemplateMethod
categories: 
 - DP
tags:
 - DP
---

* content
{:toc}

> 设计模式系列文章参考`GeekBand-李建忠`老师课堂内容整理。如有意购买该课程请访问网易云课堂。

## 动机

1. 在软件构建过程中，对于某一项任务，它常常有`稳定的整体操作结构`，但各个`子步骤却有很多改变`的需求，或者由于固有的原因（比如框架和应用程序之间的关系）而`无法和任务的整体结构同时实现`。
2. 如何在`确定稳定操作结构`的前提下，来灵活应对各个`子步骤的变化`或`晚期实现`需求？




## 定义

	定义一个操作中的算法的骨架（稳定），而将一些步骤（变化）延迟到子类中。TemplateMethod使得子类可以不改变（复用）一个算法的结构即可重定义（override重写）该算法的某些特定步骤。

## 伪码

```c
//稳定
class Library
{
public:
	void Run()
	{
		Step1();
		if ( Step2() ) 
		{
			Step3(); 
		}
		for (int i = 0; i < 4; i++){
			Step4();			
		}
		Step5();
	}
	
protected:
	void Step1() { }
	void Step3() { }
	void Step5() { }

	virtual bool Step2() = 0;
	virtual void Step4() = 0;
};

//扩展
class Application : public Library 
{
protected:
	virtual bool Step2() { }
	virtual void Step4() { }
};

//稳定
int main()
{
	Library* pLib=new Application();
	pLib->Run();

	delete pLib;
}
```

## 总结

1. TemplateMethod模式是一种非常基础性的设计模式，在面向对象系统中有着大量的应用。它用最简洁的机制（虚函数的多态性）为很多应用程序框架提供了灵活的扩展点，是代码复用方面的基本实现结构。
2. 除了可以灵活应对子步骤的变化外，`不要调用我，让我来调用你。`的反向控制结构是TemplateMethod的典型应用。
3. 在具体实现方面，被TemplateMethod调用的虚方法可以具有实现，也可以没有任何实现（抽象方法、纯虚方法），但一般推荐将他们为protected方法。

