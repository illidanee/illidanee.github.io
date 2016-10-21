---
layout: post
title: AbstractFactory
categories: 
 - DP
tags:
 - DP
---

* content
{:toc}

> 设计模式系列文章参考`GeekBand-李建忠`老师课堂内容整理。如有意购买该课程请访问网易云课堂。

## 动机

1. 在软件系统中，经常面临着`一系列相互依赖的对象`的创建工作；同时，由于需求的变化，往往存在更多系列对象的创建工作。
2. 如何应对这种变化？如何绕过常规的对象创建方法(new),提供一种`封装机制`来避免客户程序和这种`多系列具体对象创建工作`的紧耦合？




## 定义

	提供一个接口，让该接口负责创建一些列`相关或者相互依赖的对象`，无需指定他们具体的类。

## 伪码

```c
//数据库访问有关的基类
class IDBConnection { };
class IDBCommand { };
class IDataReader { };

class IDBFactory
{
public:
	virtual IDBConnection* CreateDBConnection() = 0;
	virtual IDBCommand* CreateDBCommand() = 0;
	virtual IDataReader* CreateDataReader() = 0;
};

//支持SQLServer
class SqlConnection: public IDBConnection { };
class SqlCommand: public IDBCommand { };
class SqlDataReader: public IDataReader { };

class SqlDBFactory:public IDBFactory
{
public:
	virtual IDBConnection* CreateDBConnection() { };
	virtual IDBCommand* CreateDBCommand() { };
	virtual IDataReader* CreateDataReader() { };
};

//支持Oracle
class OracleConnection: public IDBConnection { };
class OracleCommand: public IDBCommand { };
class OracleDataReader: public IDataReader { };

class OracleDBFactory:public IDBFactory
{
public:
	virtual IDBConnection* CreateDBConnection() { };
	virtual IDBCommand* CreateDBCommand() { };
	virtual IDataReader* CreateDataReader() { };
};

class EmployeeDAO
{
	IDBFactory* dbFactory;

public:
	void Query()
	{
		IDBConnection* connection = dbFactory->CreateDBConnection();
		IDBCommand* command = dbFactory->CreateDBCommand();
		IDBDataReader* reader = command->ExecuteReader();
	}
};
```

## 要点

1. 如果没有应对`多系列对象构建`的需求变化，则没有必要使用Abtract Factory模式，这时候使用工厂模式完全可以。
2. `系列对象`是指在某一特定系列下的对象之间有相互依赖、或作用的关系。不同系列的对象之间不能相互依赖。
3. Abstract Factory模式主要在于应对`新系列`的需求变动。其缺点在于难以应对`新对象`的需求变动。


