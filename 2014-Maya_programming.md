[categories] computer graphics, programming,

����
---- 
ѧmayaʱ����Щ�¸���Ҫ��¼һ�£��������ÿ�. 


Command 
---- 
��������cmd���﷨, �������������������һЩflags. [ MPxCommand::newSyntax() ]
�����﷨, �ھ���ִ����������ʱ��, ���ȵ�Ȼ���Ƿֽ�����, �����Ƿ��������Щ����. [doIt, parseArgs, argument list, argument data]

Context 
---- 
�������select tool, paint select tool, move tool...����polygon/drag a cube�ȣ���ʵÿһ������һ��context������context�����tool������һ����˼��
mel �� "currentCtx"ѯ�ʵ�ǰcontext���͵�object. 
texGrabContext // �������context command to create an object of this context;
// Result, texGrabContext1; // object��������texGrabContext1;
setToolTo texGrabContext1; // switch from current ctx to this new context object;

 