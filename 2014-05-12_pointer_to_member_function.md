[categories] programming,

function pointer��֪����, ��
``` 
int max( int a, int b) { ... };
 
int (*pFtor)( int, int ) = &max;
int iResult = pFtor(1, 2); 

or

typedef int(*FtorType)(int, int); 
FtorType pFtor; 
pFtor = &max;
int iResult = pFtor(1, 2);
``` 

���ǵ������������´���ʱ���ǿ־���һ��: 
``` 
class Tool {
	int doA(ArgumentType &);
	int doB(ArgumentType &); 
}; 
typedef int(Tool::*MemberFunctionPtr)(ArgumentType &);


Tool toolObj;
MemberFunctionPtr pFtorA = &Tool::doA; 
MemberFunctionPtr pFtorB = &Tool::doB; 
 
int iResultA = (toolObj.*pFtorA)(argumentList); 
int iResultB = (toolObj.*pFtorB)(argumentList); 

or
Tool *pToolObj;
int iResultA = (pToolObj->*pFtorA)(argumentList); 

``` 

Google��һ�£�ԭ����pointer to member function. һ��pointer��ָ��һ��obsolete address, �������pointer to member function, ����ָ��һ����class�����ƫ�Ƶ�ַoffset���ѡ�����һ������(֪���������)��+���offset, �Ƶ���������ĳ������? 

�����ӵ������Ҳ�Ǹ����õ����������Tool������derived class;
```  
class ToolX {
	// override doA and doB functions;
};
class ToolY {
	// override doA and doB functions;
};

ToolX toolXObj;
ToolY toolYObj; 
MemberFunctionPtr pFtorA = &Tool::doA; 
MemberFunctionPtr pFtorB = &Tool::doB; 
 
int iResultXA = (toolObjX.*pFtorA)(argumentList);  // ? �ǵ�����member func from base class or derived class��? 
int iResultYA = (toolObjY.*pFtorA)(argumentList);  // ?

```   

OK����֪�����������ļ����Ժ���ô, ���ּ�����ʲô����? 
Function pointerò�ƺܶ�����¶���������Ϊ�ص�����callback function, Ŀ�������. ����pointer to member functionҲ������ô�á�
``` 
typedef int(Tool::*MemberFunctionPtr)(ArgumentType &);

class Tool {
	
	ToolCallback *buildToolClassback() {
		ToolCallback *callback(*this, &Tool::doA);
		return callback;
	}

	int doA(ArgumentType &);
	int doB(ArgumentType &); 
}; 

class ToolCallback {
	ToolCallback( Tool &_toolObj, MemberFunctionPtr _p) 
	: toolObj(_toolObj), pFtor(_p) {}
	
	int do(ArgumentType) { (toolObj.*pFtor)( argument ); }

	Tool &toolObj; 				// ���1: an object;
	MemberFunctionPtr pFtor;	// ���2: on offset; 
}; 

// �������ǾͿ��԰�ToolCallback *callback�ڴ������洫����ȥ, 
// ���������ִ�����������, �Ǿ�
callback->do(argument); 
``` 