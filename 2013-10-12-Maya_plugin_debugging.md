--- 
layout: post 
title: How to debug maya custom plugin 
categories: programming 
--- 

����
----
> �漰maya plugin������ʱ����ôdebug��? 

Attach to maya process ... ���� 
----
step 1. maya plugin manager (Window -> Settings/Preferences -> Plug-in Manager)��load your custom plugin. This plugin need to be built by the same compiler version of maya, Ҳ����Ҫ. 
step 2. at your visual studio project of that custom plugin, Tools -> Attach to Process ..., choose maya.exe you will see visual studio processing some source files....�������������. 

Debug
---- 
+ MGlobal::displayInfo(MString("..")); �����Script Editor. 

+ MStatus status; �ⶫ���Ǻܶຯ���ķ���ֵ���ǵ��ж��Ƿ�MS::kSuccess. ��MString MStatus::errorString() const����������������ķ���״̬. 

+ 