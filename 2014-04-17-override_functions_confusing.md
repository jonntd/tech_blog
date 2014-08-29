--- 
layout: post 
title: function override 
categories: programming 
--- 

# �ĸ�override�ĺ������ǰ�? 

���·����ı������ڿ�����ʱ���Ӹ��ൽ����ü��㰡�����ϲ�(���������෽����Ϊ��)�ᶨ��һЩvirtual function, ���²��override ĳ���ϲ��virtual function������������������ϲ��function(may or may not be virtual)��ȥ����һ��virtual fuction��ʱ�򣬵�������һ���е�ʵ�ְ汾�������������أ�

show me the code: 
������ֻ�Ƕ�������, ManipBase and ManipMove, ����vritual functions interfaceA() and interfaceB(); Ϊʲô����Щ����? maya api�в�����manipulator��, base��ʾ����, move��ʾ�����ƶ������manipulator. 
����ȿ���һ�����Test1��Ȼ����΢���˸Ķ����õ�Test2 and Test3. 
```
// code 
#include <stdio.h>

namespace Test1 {
class ManipBase
{
public:								   
	virtual void interfaceA()
	{
		printf("ManipBase::interfaceA.\n");
		interfaceB();
	}
	virtual void interfaceB()
	{
		printf("ManipBase::interfaceB.\n");
	}
};

class ManipMove : public ManipBase
{									
public:								   
	virtual void interfaceA()
	{
		printf("ManipMove::interfaceA.\n");
		interfaceB();
	}
	virtual void interfaceB()
	{
		printf("ManipMove::interfaceB.\n");
	}
};

void test()
{
	ManipBase *pManip = new ManipMove;
	pManip->interfaceA();
}					  
// the output result: 
//ManipMove::interfaceA.
//ManipMove::interfaceB.
}; // namespace

namespace Test2 {
class ManipBase
{
public:								   
	virtual void interfaceA()
	{
		printf("ManipBase::interfaceA.\n");
		interfaceB();
	}
	virtual void interfaceB()
	{
		printf("ManipBase::interfaceB.\n");
	}
};

class ManipMove : public ManipBase
{									
public:								   
	virtual void interfaceA()
	{
		printf("ManipMove::interfaceA.\n");
		interfaceB();
	}
};						 

void test()
{
	ManipBase *pManip = new ManipMove;
	pManip->interfaceA();
}					  
// the output result: 
//ManipMove::interfaceA.
//ManipBase::interfaceB.
}; // namespace

namespace Test3 {
class ManipBase
{
public:								   
	virtual void interfaceA()
	{
		printf("ManipBase::interfaceA.\n");
		interfaceB();
	}
	virtual void interfaceB()
	{
		printf("ManipBase::interfaceB.\n");
	}
};

class ManipMove : public ManipBase
{									
public:					
	virtual void interfaceB()
	{
		printf("ManipMove::interfaceB.\n");
	}
};

void test()
{
	ManipBase *pManip = new ManipMove;
	pManip->interfaceA();
}
// the output result: 
//ManipBase::interfaceA.
//ManipMove::interfaceB.
}; // namespace
					  
int main()
{				  
	Test1::test();
	Test2::test();
	Test3::test();
	
	return 1;
}
```
�����
�ҵĸо���virtual function�ĵ��û��"��ײ�"��ʼ�ң��ҵ���ִ�У��Ҳ��������ϲ���. 

�����"��ײ�"����˼����������������ManipBase *pManip = new ManipMove;����ײ����ManipMove;
�����ټ���һ��ManipBase <- ManipMove <- ManipSuperFastMove :-) 
case 1: ManipBase *pManip = new ManipMove;����ײ㻹��ManipMove;
case 2: ManipBase *pManip = new ManipSuperFastMove;����ײ����ManipSuperFastMove;

