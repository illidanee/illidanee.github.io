---
layout: post
title: Decorator
categories: 
 - DP
tags:
 - DP
---

* content
{:toc}

> 设计模式系列文章参考`GeekBand-李建忠`老师课堂内容整理。如有意购买该课程请访问网易云课堂。

## 动机

1. 在某些情况下我们可能会`过度地使用继承来扩展对象的功能`，由于继承为类型引入的静态特质，使得这种扩展方式缺乏灵活性，并且随着子类的增多（扩展功能的增多），各种子类的组合（扩展功能）会导致更多的子类的膨胀。
2. 如何使`对象功能的扩展`能够根据需要来动态的实现？同时避免`扩展功能的增多`带来的子类膨胀问题？从而使得任何`功能扩展变化`所导致的影响降为最低？




## 定义

	动态（组合）地给一个对象增加一些额外的职责。就增加功能而言，Decorator模式比生成子类（继承）更为灵活（消除重复代码&减少子类个数）。

## 伪码

```c
//稳定
class Stream
{
public：
	virtual char Read(int number) = 0;
	virtual void Seek(int position) = 0;
	virtual void Write(char data) = 0;
};

class FileStream : public Stream
{
public:
	virtual char Read(int number) { }
	virtual void Seek(int position) { }
	virtual void Write(char data) { }
};

class NetworkStream : public Stream
{
public:
	virtual char Read(int number) { }
	virtual void Seek(int position) { }
	virtual void Write(char data) { }
};

class MemoryStream : public Stream
{
public:
	virtual char Read(int number) { }
	virtual void Seek(int position) { }
	virtual void Write(char data) { }
};

//扩展
class DecoratorStream : public Stream
{
protected:
	Stream* stream;
    
	DecoratorStream(Stream * stm)
		: stream(stm)
	{
	}
};

class CryptoStream : public DecoratorStream 
{
public:
	CryptoStream(Stream* stm)
		: DecoratorStream(stm)
	{
	}

	virtual char Read(int number)
	{
		//额外的加密操作...
		stream->Read(number);
		//额外的加密操作...
	}
	virtual void Seek(int position)
	{
		//额外的加密操作...
		stream->Seek(position);
		//额外的加密操作...
	}
	virtual void Write(byte data)
	{
		//额外的加密操作...
		stream->Write(data);
		//额外的加密操作...
	}
};

class BufferedStream : public DecoratorStream
{
	Stream* stream;
    
public:
	BufferedStream(Stream* stm)
		: DecoratorStream(stm)
	{
	}
};

//稳定
void Process()
{
	//运行时装配
	FileStream* s1 = new FileStream();
	CryptoStream* s2 = new CryptoStream(s1);
	BufferedStream* s3 = new BufferedStream(s1);
	BufferedStream* s4 = new BufferedStream(s2);
}
```

## 总结

1. 通过采用组合而非继承的手法，Decorator模式实现了在运行时动态扩展对象的能力，而且可以根据需要扩展多个功能。避免了使用继承带来的`灵活性差`和`多子类衍生问题`。
2. Decorator类在接口上表现为is-a Component的继承关系，即Decorator类继承了Component类所具有的接口。但在实现上又表现为has-a Component的组合关系，即Decorator类又使用了另外一个Component类。
3. Decorator模式的目的并非解决`多子类衍生的多继承`问题，Decorator模式应用的要点在于解决`主体类在多个方向上的扩展功能`-是为`装饰`的含义。

