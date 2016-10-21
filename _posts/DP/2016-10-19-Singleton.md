---
layout: post
title: Singleton
categories: 
 - DP
tags:
 - DP
---

* content
{:toc}

> 设计模式系列文章参考`GeekBand-李建忠`老师课堂内容整理。如有意购买该课程请访问网易云课堂。

## 动机

1. 在软件系统中，经常有这样一些特殊的类，必须保证他们在系统中只存在一个实例，才能确保它们的逻辑正确性、以及良好的效率。
2. 如何绕过常规的构造器，提供一种机制来保证一个类只有一个实例？
3. 这应该是类的设计者的责任，而不是使用者的责任。





## 定义

	保证一个类只有一个实例，并提供一个该实例的全局访问点。

## 伪码

### 线程非安全版本

```c
class Singleton
{
public:
	static Singleton* getInstance();
    
private:
	static Singleton* m_instance;

	Singleton() {};
	Singleton(const Singleton& other) {};
};

Singleton* Singleton::m_instance = nullptr;

Singleton* Singleton::getInstance()
{
	if (m_instance == nullptr) 
	{
		m_instance = new Singleton();
	}
	return m_instance;
}
```

### 线程安全版本,但锁的代价过高

```c
class Singleton
{
public:
	static Singleton* getInstance();
    
private:
	static Singleton* m_instance;

	Singleton() {};
	Singleton(const Singleton& other) {};
};

Singleton* Singleton::m_instance = nullptr;

Singleton* Singleton::getInstance()
{
	Lock lock;
	if (m_instance == nullptr)
	{
		m_instance = new Singleton();
	}
	return m_instance;
}
```

### 线程安全版本,双检查锁,但由于内存读写reorder不安全

```c
class Singleton
{
public:
	static Singleton* getInstance();
    
private:
	static Singleton* m_instance;

	Singleton() {};
	Singleton(const Singleton& other) {};
};

Singleton* Singleton::m_instance = nullptr;

Singleton* Singleton::getInstance() 
{   
	if(m_instance==nullptr)
	{
		Lock lock;
		if (m_instance == nullptr) 
		{
			m_instance = new Singleton();
		}
	}
	return m_instance;
}
```

### C++11安全实现

```c
class Singleton
{
public:
	static Singleton* getInstance();
    
private:
	static Singleton* m_instance;

	Singleton();
	Singleton(const Singleton& other);
};

Singleton* Singleton::m_instance = nullptr;

std::atomic<Singleton*> Singleton::m_instance;
std::mutex Singleton::m_mutex;

Singleton* Singleton::getInstance()
{
	Singleton* tmp = m_instance.load(std::memory_order_relaxed);
	std::atomic_thread_fence(std::memory_order_acquire);//获取内存fence
	if (tmp == nullptr)
	{
		std::lock_guard<std::mutex> lock(m_mutex);
		tmp = m_instance.load(std::memory_order_relaxed);
		if (tmp == nullptr)
		{
			tmp = new Singleton;
			std::atomic_thread_fence(std::memory_order_release);//释放内存fence
			m_instance.store(tmp, std::memory_order_relaxed);
		}
	}
	return tmp;
}
```

## 总结

1. Singleton模式中的实例构造器可以设置为protected以允许子类派生。
2. Singleton模式一般不要支持拷贝构造函数和Clone接口，因为这有可能导致多个对象实例，与Singleton模式的初衷违背。
3. 如何实现多线程环境下安全的Singleton？注意对双检查锁的正确实现。

