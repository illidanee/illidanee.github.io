---
layout: post
title: ChainOfResposibility
categories: 
 - DP
tags:
 - DP
---

* content
{:toc}

> 设计模式系列文章参考`GeekBand-李建忠`老师课堂内容整理。如有意购买该课程请访问网易云课堂。

## 动机

1. 在软件构建过程中，一个请求可能被多个对象处理，但是每个请求在运行时只能有一个接收者，如果显示指定，将必不可少地带来请求者与接收者的紧耦合。
2. 如何使请求者不需要指定具体的接收者？让接收者自己在运行时决定来处理请求，从而使两者解耦合。




## 定义

	使多个对象都有机会处理请求，从而避免发送者和接收者之间的耦合关系。将这些对象连成一条链，并沿着这条链传递请求，直到有一个对象处理它为止。
	
## 伪码

```c
enum class RequestType
{
	REQ_HANDLER1,
	REQ_HANDLER2,
	REQ_HANDLER3
};

class Reqest
{
	string description;
	RequestType reqType;
    
public:
	
	Reqest(const string& desc, RequestType type) : description(desc), reqType(type) {}
	RequestType GetReqType() const { return reqType; }
	const string& GetDescription() const { return description; }
};

class ChainHandler
{
	ChainHandler *nextChain;
	void SendReqestToNextHandler(const Reqest & req)
	{
		if (nextChain != nullptr)
			nextChain->Handle(req);
	}
	
protected:
	
	virtual bool CanHandleRequest(const Reqest & req) = 0;
	virtual void ProcessRequest(const Reqest & req) = 0;

public:
	
	ChainHandler() { nextChain = nullptr; }
	void SetNextChain(ChainHandler *next) { nextChain = next; }

	void Handle(const Reqest & req)
	{
		if ( CanHandleRequest(req) )
			ProcessRequest(req);
		else
			SendReqestToNextHandler(req);
	}
};


class Handler1 : public ChainHandler
{
protected:
	
	bool CanHandleRequest(const Reqest& req) override
	{
		return req.GetReqType() == RequestType::REQ_HANDLER1;
	}
	void ProcessRequest(const Reqest& req) override
	{
		cout << "Handler1 is handle reqest: " << req.GetDescription() << endl;
	}
};
        
class Handler2 : public ChainHandler
{
protected:
	
	bool CanHandleRequest(const Reqest& req) override
	{
		return req.GetReqType() == RequestType::REQ_HANDLER2;
	}
	void ProcessRequest(const Reqest& req) override
	{
		cout << "Handler2 is handle reqest: " << req.GetDescription() << endl;
	}
};

class Handler3 : public ChainHandler
{
protected:
	
	bool CanHandleRequest(const Reqest& req) override
	{
		return req.GetReqType() == RequestType::REQ_HANDLER3;
	}
	void ProcessRequest(const Reqest& req) override
	{
		cout << "Handler3 is handle reqest: " << req.GetDescription() << endl;
	}
};

int main()
{
	Handler1 h1;
	Handler2 h2;
	Handler3 h3;
	h1.SetNextChain(&h2);
	h2.SetNextChain(&h3);

	Reqest req("process task ... ", RequestType::REQ_HANDLER3);
	h1.Handle(req);
	return 0;
}
```

## 总结

1. ChainOfResposibility模式的应用场合在于`一个请求可能有多个接收者，但是最后真正的接收者只有一个`，这个时候发送者和接收者的耦合可能出现`变化脆弱`的症状，职责链的目的就是将二者解耦合，从而更好地应多变化。
2. 应用了ChainOfResposibility模式后，对象的职责分派将更具灵活性。我们可以在运行时动态添加/修改请求的处理职责。
3. 如果请求传递到职责链的末端仍得不到处理，应该有一个合理的缺省机制。这也是每一个接收者的责任，而不是发送者的责任。



