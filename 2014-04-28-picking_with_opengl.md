--- 
layout: post 
title: Picking with OpenGL 
categories: computer graphics  
--- 

> If you have choices, choose the best. If you have no choice, do the best. �����ѡ���Ǿ�ѡ����õģ����û��ѡ���Ǿ�Ŭ��������á�

# Picking with OpenGL

��3d����ṩ������ʱ��������ѡ����ĳ��vertex����ĳ��face��Ȼ��ȥ���������ѡ�еĶ���������maya����ܶ๤��(maya�����context==tool?)������pick the manipulator, drag the manipulator, to update the attributes of the node (the selection list). ��һ������Ҫѡ��mouse���µĶ�����������ô��������?

��ʵһ��ʼ���뵽�ķ���ray intersection�ķ�����
approach 0. check the intersection between a ray and object.
���ǿ���ò�����������ַ��������:
approach 1. use the select mode in opengl;
approach 2. color coding/picking: render objects in different color, pick a color, map to the object. 

## Approach 0. ray intersection. 
Reference example: cinder, version 0.8.5, samples/Picking3D. 
(���cinder��������һ����˼�ط�������mouse event����Ӧrespond��ģ��simulate maya��. ������MayaCamUI.h����, ����mouse event���ж�����panning/tumbling/zooming�ĸ�ģʽ��, Ȼ�����perspective camera��position����Ϣ)

������һ��ray��mesh��triangle faces������. 
  Ray = point + vector direction(end point - start point);
  Triangle face = (point0, point1, point2); 
Question: ��ô���ray? �õ�����������world space, camera space, or model's object space ? 
                                           
## Approach 1. using selection buffer 
http://content.gpwiki.org/index.php/OpenGL:Tutorials:Picking



## Approach 2. color picking

�����approach 1��ʲô������? 
http://www.opengl.org/wiki/Common_Mistakes#Selection_and_Picking_and_Feedback_Mode 
ò��ִ����CPU�ˣ���ĳЩʵ�������ܲ���performance is lousy(�µ���:-). ����, ��modern opengl�б�deprecated and even removed�ˣ�ȡ����֮������approach 2�����color picking����. ����������: A modern OpenGL program should do color picking (render each object with some unique color and glReadPixels to find out what object your mouse was on) or do the picking with some 3rd party mathematics library (�һ��������math��������ָray-intersection����).

TODO...
