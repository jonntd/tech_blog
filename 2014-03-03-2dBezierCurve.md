Title: 2d bezier curve - simple implement             
Author: canjiang ren                
Date: 2014-03                
Tag: geometry             
                       

2d bezier curve - simple implementation                        
----
��������һ������nurb curve�����⣬������knots�������󷨵����⡣�Ƕ���̫�ѣ��������˵��bezier curve�׶��ܶ࣬һֱ��ʵ��һ�������������Ǿ����˴ˡ�

��ε�������:
+ bezier curve, 2d, degree 3 (which means cubic, order = 4, 4 control points); 
+ ��polyline����4�����Ƶ�����߻�����. 
+ ��polyline��curve��������ʵ����ֻ��curve��һ��approximation. ��������curve��200��С�߶�����ʾ, �͵�sample 201����. �����Ҫevaluation������һ��t [0, 1], ���Ӧcurve�ϵ�����position (x, y). �������õ���de Casteljau��������evaluation.
+ ��key 0/1/2/3 to selete the corresponding control point����Up/Down/Left/Right��move the control points, update the curve realtime. 

��ȥwindows/event managent�ĸ����������⣬������˼����ôʵ��de Casteljau:
<code>
Vector2d evaluateCubicBezier_deCasteljau(const std::vector<Vector2d> &aCPs, float t)
{
	// this function is very interesting. everytime the line sections reduce, recursively, 
	// when it redecued to one, we do a linear interopolation of it and get the result.
    // 
	
	
	if (!aCPs.size()) 
	{
		assert(false);
		return Vector2d();
	}
	if (aCPs.size() == 1)
		return aCPs[0]; 
	
	std::vector<Vector2d> a;
	for (int i = 0; i < aCPs.size() - 1; ++i)
		a.push_back( aCPs[i] + (aCPs[i+1] - aCPs[i]) * t );
	assert(a.size() == aCPs.size() - 1);
	return evaluateCubicBezier_deCasteljau(a, t); 
} 
</code>

������һЩ��Ȥ�Ľ��:
![Alt text](data/2014-03-2dBezierCurve_out_0.png "output") 
                

Source code: https://github.com/renc/PhoenixCore/blob/master/examples/08curves2dmain.cpp 