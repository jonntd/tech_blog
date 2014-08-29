--- 
layout: post 
title: Note of Maya programming 
categories: programming computer graphics  
--- 

����
---- 
ѧmayaʱ����Щ�¸���Ҫ��¼һ�£��������ÿ�. 


Command 
---- 
��������cmd���﷨, �������������������һЩflags. [ MPxCommand::newSyntax() ]
�����﷨, �ھ���ִ����������ʱ��, ���ȵ�Ȼ���Ƿֽ�����, �����Ƿ��������Щ����. [doIt, parseArgs, argument list, argument data]

��regular command and interactive command (tool command)����, �ֱ��ӦMPxCommand and MPxToolCommand. 


Context 
---- 
�������select tool, paint select tool, move tool...����polygon/drag a cube�ȣ���ʵÿһ������һ��context������context�����tool������һ����˼��
mel �� "currentCtx"ѯ�ʵ�ǰcontext���͵�object. 
grabUVContext // �������context command to create an object of this context;
// Result, grabUVContext1; // object��������grabUVContext1;
setToolTo grabUVContext1; // switch from current ctx to this new context object;

context�漰�ķ�Χ�ܹ�:
+ context command, Ҳ������plugin����ע��registerһ��contextʱ����ʵ��һ��ctx command. �����Ǹ�grabUVContext��ʵ��һ��cmd��������һ�������cmd����, ctx cmd; -q Ҳ����query��Ҳ����Ϊһ��cmd����. 
+ context and interactive command, ����moveTool;         
+ context and manipulator; ���Ӻܶ�.               
              

UI   
----  
2014/7 ��ΪҪ��maya�����ĳ��shelf�ϼ�button, ������Ҫ�˽�һ��mel������ô��ui. 
������һ��window, ������layout, �����Ǹ��ָ�����layout,  layout֮������и��ӹ�ϵ��Ȼ����button. 
��ǰ֪����button��:
shelfButton �������ڽ�����Polygon������Ե���˾ͽ�ģ�͵�����;
toolButton ���ǽ�����ߵ�select move rotate scale��. 
![Alt text](data/2014-06-25-UndoRedoStack.png "output")
UI���� window/tabLayout/shelfLayout/shelfButton; ��setParent .. ���Իص���һ��layout. 

��ô��ôadd custom shelf or shelf button ��?

����1. �ҵ�maya��װ·��\scripts\startup\Ŀ¼, ��������ܶ�shelf_xxx.mel, ����shelf_Polygons.mel, shelf_Surfaces.mel, ...��Щ����maya����ʱ����ص�, Ҳ���������ڽ����Ͽ�����.
��ô���ǾͿ��Գ�������·������������Լ�д��shelf_yyy.mel�ˡ�
���������ԡ�MAYA_SHELF_PATH�� environment variable?

����2. 

References: 
+ mel command document: window, tabLayout, shelfLayout, shelfButton. 