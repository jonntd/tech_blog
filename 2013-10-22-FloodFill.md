--- 
layout: post 
title: Flood fill 
categories: programming 
--- 

FloodFill simple implementation                        
----

Oct. 22, 2013 ����someone�ᵽFlood fill algorithm, ����������Ϥ������֣�����Googleһ�£���Ȼ��BFS������������Ҫ��ʵ��һ�¡�һֱ�ϣ��ϵ�������, Sunday, Nov. 4, ��Ū���������           

������ͼ����Windows > Paint����л��ģ�һ����ֵ���״��������Paint����е�fill���߿��԰������ֵ���״����ĳһ����ɫ���Ǽ����ֹ�ʵ�֣���ô����?                 
![Alt text](data/2013-10-20_Shape_to_fill.bmp "input shape")            

�����ǽ�����ֱ�������10000�Σ�20000��...�Ľ����

![Alt text](data/floodfill_out_s10000.bmp "output")            
![Alt text](data/floodfill_out_s20000.bmp "output")             
![Alt text](data/floodfill_out_s30000.bmp "output")            
![Alt text](data/floodfill_out_s80000.bmp "output")     

�ӳ���Ľ����������һ������֮���������������ֵ���״��������Ľ���У���10000�ε�20000����չ�������Ƿ����20000��30000����չ������һ�����أ���ʲô�йأ�(After some steps, the shape will be filled. From the result images aboved, the extended red region added during from 10000 to 20000 is the same as the region added during from 20000 to 30000? what is the reason behind that.)                    

New functions learned:            
std::stoi
std::to_string                       

Source code: https://github.com/renc/FloodFill         