---
layout: post
title: Composite
categories: 
 - DP
tags:
 - DP
---

* content
{:toc}

> 设计模式系列文章参考`GeekBand-李建忠`老师课堂内容整理。如有意购买该课程请访问网易云课堂。

## 动机

1. 在软件构建过程中的某些情况下，客户代码过多地依赖于对象容器复杂的内部实现结构，对象容器内部实现结构（而非抽象接口）的变化将引起客户代码的频繁变化，带来了代码的维护性、扩展性等弊端。
2. 如何将`客户代码与复杂的对象容器结构`解耦？让对象容器自己来实现自己的复杂结构，从而使得客户代码就像处理简单对象一样来处理复杂的对象容器？




## 定义

	将对象组合成树形结构以表示`部分-整体`的层次结构。Composite使得用户对单个对象和组合对象的使用具有一致性。
	
## 伪码

```c
//抽象类
class Component
{
public:
	
	virtual ~Component(){}
	virtual void process() = 0; 
};

//树节点
class Composite : public Component
{
	string name;
	list<Component*> elements;
    
public:
	Composite(const string& s) : name(s) {}

	void Add(Component* element) 
	{
		elements.push_back(element);
	}
	void Remove(Component* element)
	{
		elements.remove(element);
	}
    
	void Process()
	{
		for (auto &e : elements)
			e->Process(); 		//多态调用
	}
};

//叶子节点
class Leaf : public Component
{
	string name;
	
public:
	
	Leaf(const string& s) : name(s) {}

	void Process() { /*...*/ }
};

//使用者
void Invoke(Component& c)
{
	c.Process();
}

int main()
{
	Composite root("root");
	Composite treeNode1("treeNode1");
	Composite treeNode2("treeNode2");
	Composite treeNode3("treeNode3");
	Composite treeNode4("treeNode4");
	Leaf leat1("left1");
	Leaf leat2("left2");

	root.Add(&treeNode1);
	treeNode1.Add(&treeNode2);
	treeNode2.Add(&leaf1);

	root.Add(&treeNode3);
	treeNode3.Add(&treeNode4);
	treeNode4.Add(&leaf2);
	
	
	//调用
	Invoke(root);
	Invoke(leaf2);
	Invoke(treeNode3);
}
```

## 要点

1. Composite模式采用树形结构来实现普遍存在的对象容器，从而将`一对多`的关系转化为`一对一`的关系，使得客户代码可以一致地（复用）处理对象和对象容器。
2. 将`客户代码与复杂的对象容器结构`解耦合是Composite的核心思想，解耦合之后，客户代码将与纯粹的抽象接口（而非对象容器的内部实现结构）发生依赖，从而更能`应对变化`。
3. Composite模式在具体实现中，可以让父对象中的子对象反向追溯；如果父对象有频繁的遍历需求，可使用缓存技巧来改善效率。