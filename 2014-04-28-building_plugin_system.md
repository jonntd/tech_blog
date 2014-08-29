--- 
layout: post 
title: How to build a plugin system 
categories: programming 
--- 

�ܶ�������ṩ��API��customer, ����дplugin����չԭ���ĳ���. 

����mudbox��maya. ��mudbox��API������һ�����ʵ��
- �Զ���operation, support undo and redo;
- �Զ���ĳ��brush (operation); 
- �̳�ʵ��ĳ���࣬����ԭ�е��Ǹ�. ����map extraction? 
��ǰ�Ӵ�����maya api���֪࣬��plugin����ʵ��context and manipulator.

���������, һ���򵥵�plugin system����ô��ƺ�ʵ�ֵ���? 

case 1: 
Reference: Using Dynamic Link Libraries (DLL) to Create Plug-Ins, by Jeremiah van Oosten. 
http://3dgep.com/?p=1759 

ϵͳ����������, ���ǳ�������(1)�����е�.exe, (2)ĳ�����Ա����ص�plugin, ���е��¿��ܲ����ײ����(3)��ǰ���������õģ������ӹ�ϵ��dll. ���ߵĽṹ������: 
   
  ![Alt text](data/how_plugin_system_work.PNG "plugin system example1")
�ɼ�CommonDLL����app.exe�����ã�Ҳ����������plugin��ʹ��.

��ʵ����������questions: 
- ��ôexportһ��dll A�������Ϣ�����棬�ñ��dll B�ܹ�ʹ��? 
- C++�����ʱ�򣬻��漰һ��managling name������. ��ʱ���Ҫ�ص�extern "C"

����õ���source code: https://github.com/renc/plugins_system_example1/

case 2: 
Reference: Nuclex Plugin Architecture
�������CommonDLL���ӽṹһ�������˿���plugin version. 

�����ο�references: 
Qt plugin; 
Building an Engine Plugin System, by Niklas Frykholm, from bitsquid. http://www.altdevblogaday.com/ 



