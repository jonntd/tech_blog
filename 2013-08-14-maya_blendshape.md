--- 
layout: post 
title: Blendshape at maya 
categories: programming 
--- 

����
----
> ����Ӵ���һ��maya blendshape programming, ��΢��¼һ�¡�

BlendShape deformer basis (����)
----
��˵��һ���ص㣬��dragһ��cube or sphere�����ʱ�򣬻�����<transform node, shape node>����nodes, �����ϸ���뿴maya scene graph��DAG��

To create a simple blendshape example,  
Polygon mode, create a cube, we have <pCube1, pCubeShape1>;   
Select cube1, Shift+D, we have <pCube2, pCubeShape2>, move cube2 a little bit away from cube1.  
Select cube1, Shift+D, we have <pCube3, pCubeShape3>, move cube3 a little bit away from cube1.  
Select cube3 first, then shift+select cube2, then shift+select cube1, 
Animatioin mode, menu Create Deformer > BlendShape (Deformation order == Default), we have a blendshape deformer node, for example called blendShape1.   
cube1 is called source shape for this blenshape.   
cube2 and cube3 are as target shapes for this blendshape.

select cube1, menu Windows > Connections, we see the connections of these nodes.

![Alt text](2013-08-14_maya_blendshape_fig_maya_scene.PNG "maya scene")

**Maya/Fbx API Table.**   
| source | blendshape | channel | target |           
| ---- |     
| MObject | MFnBlendShapeDeformer | Target Weight slider | MObject |           
| FbxGeometry | FbxBlendShape | FbxBlendShapeChannel | FbxShape|       

���tableչʾ��blendshape deformer�������Ҫ��: source shape, target shapes, target weight (������blending weight).

�й�target weight slider��ע������:   

+ һ��blendshape�����ж��target weight slider, ÿһ��slider�п�����һ��or���target shapes, ȡ����In-Between off/on.  

+ ÿһ��target weight slider ��һ����ֵ��target weight (value) ��ʾtarget shapes��source��effect.  

+ ÿһ��target weight slider ��һ������target weight or target weight slider name or name of target weight. ���nameĬ��by default����transform node�����֣������������ӵ�����target weights�����ֱַ���pCube2 and pCube3. tricky����������ֿ��Ա��ĵ�, �ĳ�whatever u like, ��������ĳ�pCube2x. ������code������и������ˣ�����һ��target weight name, how to find out the target shape (or the transform node of the target shape) associated with this target weight name?

+ ÿһ��target weight slider ��Ӧ��fbx�����FbxBlendShapeChannel, ����maya/fbx exporter plugin (fbxmaya.mll)��channel��������blendShape1.nameOfTargetWeight.  

һЩ���Ե�MEL commands: 
```                  
	objExists pCube2   
	objExists |pCube2	
	objExists pCube2|pCubeShape2	
	objExists pCubeShape2	
	objExists |pCubeShape2		

	objExists blendShape1.pCube2x;   
	// Results: 1 //   
   
	blendShape -q -g blendShape1;
	// Result: pCubeShape1 //  the source shape's name, not transform node's name.
	blendShape -q -target blendShape1;   
	// Result: pCube2 pCube3 // the target shape ��s transform node��s name.
	blendShape -q -t blendShape1;
	// Result: pCube2 pCube3 // the flag -target is the same as -t
	blendShape �Cq �CweightCount blendShape1;   
	// Result: 2 //            ���ﷵ�ص�����Ӧ�þ��Ǹպ�����targets��Ŀ
    blendShape -q -w blendShape1;
	// Result: 0.5 0 //  ÿһ��target weight's value. 

	listAttr -sn blendShape1.weight[0];    
	// Result: pCube2x //  ��������Է��ʵ���Delete��baked��target. 
	listAttr -m blendShape1.w;  
	// Result: pCube2x pCube3 // names of target weight, not the name of target shape or transform node.
	// �����Ҳ�������Delete��baked��targets. 
	
	getAttr blendShape1.weight;
	// Result: 0.4 0 0.2 //����(s:pCube1, t:pCube2, pCube3, pCube4), Ȼ��ֱ�Ӱ�����Delete"��pCube3 (pCube3���baked����. 
	
	aliasAttr -q blendShape1;   
	// Result: pCube2x weight[0] pCube3 weight[1] // ��Ӧ��ϵ 
	aliasAttr -q blendShape1.pCube2x;   
	// Result: pCube2x //    
	aliasAttr smile blendShape1.weight[0];
	aliasAttr smile blendShape1.w[0]; // weight[0] == w[0] 
	// Result: 1 //  ��weight[0]����Ϊsmile, �����������pCube2x��. 
	aliasAttr -rm blendShape1.smile
	// ��ĳ�weight[x]������� ���������x����target/weight/slider��index. 
	
	setAttr blendShape1.pCube2x 0.5;  //�ɹ�, ��target weight slider��value.   
	setAttr "blendShape1.pCube2x" 0.8; //�ɹ�, ��target weight slider��value.   
	setAttr blendShape1.smile 0.6; // Succeed, 
	setAttr blendShape1.w[0] 0.7;  // Succeed, w[0] == weight[0] has been renamed as smile. 
	
	listConnections blendShape1.it; //һ����
	listConnections blendShape1.inputTarget;
	// Result: �г�����target shapes (������baked)
	
```    
��Script Editor������ help <command name>�᷵��help information.  ������C++ source����Ҳ��������Щmel commands, ��objExistsΪ��:
```
MString command = "objExists pCube1";
int result = 0; // false;
MStatus status = MGlobal::executeCommand(command, result);
```   
 
updated about getting the target shapes from the blendshape deformer node.
![Alt text](2013-08-14_maya_blendshape_fig_maya_scene_Weight_InBetween.PNG "What is Target Weight? Like slots") 

updated 2013/10/11. blendShape�����Բ�����deformed result = source * (1 - target_weight) + target * target_weight;
source and target����˵shape mesh, Ҳ�����Ǳ��blendshape deformation node�Ľ����

udated 2013/10/12. ����һ��ʹ��objExists mel command��bug. ����Face + FaceShape���沿��transform + shape nodes, ����DAG�е�path��:
|man|head_group|Face|FaceShape 
��ô��Script Editor����ִ��:
objExists |Face
�Ľ����ʲô��? false, �Ҳ���|Face ? �����в���������Face���node����? ����ִ��:
objExists Face
�����true�� ԭ�������Ҳ���Face, �����Ҳ���|Face. ������ʲô����? |Face��ʾ���face node��ֱ����root����. �����Ҿ�����objExists����Ǳ��|Ϊ��. ��maya scene graph�����������ᵽ|.


MFnBlendShapeDeformer::addTarget(base shape, channel/target weight index, target shape, weight value); 
�ɷ���add��ͬ��targets�أ�����ÿ��add target֮ǰ���жϼ����Ѿ��еû�����removeTarget��?


Target weight�ϵ�connection (MPlug��Ӧ��)
---- 
��һ���򵥵����ӡ���һ��blendShape1(cube1 as source, cube2 and cube3 and cube4 as target shapes, rename the weight to be weight1 and weight2)��Ȼ��select cube3 and shift select cube1, remove target from blendshape (Ŀ������ʾplug ��existing weight index list���ܲ���ȣ���Ϊǰ��plug���Ǹ���ɾ����target֮ǰռ��λ�û���) 
blendshape1.weight1 ��һ��plug�����֣���ʵ���weight1Ҳͬʱ�Ƕ�Ӧtarget slider�����֡������ᵽ������listAttr and aliasAttr������ʲôplugs�͸����ǵ�����. ����������ô���obtain��Щsliders/weights:
```
	// mel commands
	blendShape -q -wc blendShapeName; // return the weight count (including the baked&deleted target);
	blendShape -q -t blendShapeName; // retrun the target shpae 's name (no baked&deleted target(
	listAttr -m blendShapeName.w; // return all the names of weights, including the removed or baked&deleted one.
	aliasAttr -q blendShapeName; // return all the weights' names and associated weight[i] 
	
	// c++ code
	unsigned int iWeightCount = MFnBlendShapeDeformer::numWeights(); 
	MFnBlendShapeDeformer::weightIndexList( MIntArray &indexList );
	assert( iWeightCount == indexList.length() );
	
	MPlug plugs = blendShapeNode.findPlug("weight", &status); 
	assert( iWeightCount == plugs.numElements() ); // one weight <-> one "weight" plug
	// �����һ�������� ��������pCube3��remove target�� or ֱ�Ӱ�����Delete����. 
	
	for (unsigned int i = 0; i < iWeightCount; ++i)
	{
		MPlug weightPlug = plugs[i]; // we can get the slider/weight name.
		
		// indexList[] is only a small set of the indices of a very large weight array[],
		// some items of the big weight array[] are not used, some items are
		// deleted and invalid if some targets are removed from weight.
		const int iWeightIndexOfWeightArray = indexList[i]; 
		// this iWeightIndexOfWeightArray can be used to get the weight value and targets.
		...
		
		// btw, one weightPlug / slider / weight <--> one FbxBlendShapeChannel
		// the target shapes of this channel become some objects of FbxShape.
	} 
	
```


һ�������������ͨ���϶���slider����blendshape��Ч������Ӧ��mel and c++ code: 
```
	setAttr "blendshape1.weight1" 0.5

	weightPlug.setDouble( 0.5 ); // MPlug weightPlug; weightPlug.name() == "blendshape1.wieght1"; 
```

Ҳ���԰ѱ��ֵconnect�����target weight����, ����ͨ������animation����locator�ȿ���target weight��ֵ�ı仯��ÿһ��connection����source plug and destination plug��ɵ�. �������blendShape1����������һ��:
��һ��plane1, ��Window > Node Editor ��plane1��translateX����blendShape1.weight1, ��plane1��translateY����blendShape1.weight2, UI��������sliders����ɻ�ɫ��, DG������blendShape1���node����״Ҳ�ӳ����α�ɵ�����. 
![Alt text](2013-08-14_maya_blendshape_fig_BS_with_connnection_DG_NodeEditor.PNG "Connections at DG and NodeEditor")
����Ĺ�����ʵ���ǽ���������connections. ����һ��connection�� (pPlane1.translateX, blendshape1.weight1)������pPlane1.translateX��Ϊsource plug, blendShape1.weight1��Ϊdestination plug. 
����Ĵ�����ʾ��ô���һ��blendShape��plug, �Լ���Щplug������connections: 
```
	// given MFnDependencyNode blendShapeNode, and MStatus status;
	MPlug plugs = blendShapeNode.findPlug("weight", &status); 
	for (unsigned int i = 0; i < plugs.numElements(); ++i)
	{
		MPlug weightPlug = plugs[i]; // as destination plug for connection.
		MPlugArray inPlug; // as source plug for connection.
		weightPlug.connectedTo(inPlug, true, false);
		for (unsigned int i = 0; i < inPlug.length(); ++i)
			MGlobal::displayInfo(inPlug[i].name() + " + " + weightPlug.name()); // for debug.	
	}
	
	// mel command below: 
	listAttr -m blendShape1.w;
	// for example returns weight1 weight2 ... 
	connectionInfo -sourceFromDestination blendShape1.weight1;
	// for example returns pPlane1.translateX, the source of the connection (string, empty if null).
	connectionInfo -destinationFromSource pPlane1.translateX;
	// for example returns blendShape1.weight1 
	
```
 
�������target weight��Ӧ��ģ��target shape��removeTarget��, ��Щconnection�����𣿲����ˣ������Ǹ�����side effect�� �������°�ģ��addTarget��blendShape1���������°���Щ�Ͽ���connection���������أ�����֮ǰkey�Ķ����ȶ�ʧЧ��/
```
	// target shape is removed from this blendshape for some reason.
	// target shape is re-added into the blendshape. we want to 
	// restore those connections before. how to do this?
	MString commandString = "connectAttr " + inPlug[i].name() + " " + weightPlug.name();
	MGlobal::executeCommand(commandString, true); // true: display the result. 
```
����Ĵ�����������connectAttr���mel command, ò��MDGModifier::connect(,)Ҳ�����ƵĹ��ܣ������ϲ���ʱ���������һ���Ƿŵ�ĳ��custom command class�����õģ����һ�Ҫ�Զ���undo/redo�ĺ������һ�û�Թ�����ȶ��ԣ�ֱ����mel��û����Щ���ġ�����UI�ϵĲ������ҵĸо���������Ӧ�Ĳ��������õ��µ�mel commands (Script Editors������ʾ��Щ������ʷ), mel command�ٵ��µ�Ȼ��������C++ ����XX��ʵ�ֵġ�
UI operation -> mel command -> c++ XX function (not exposed to user); 
������һЩmaya c++ api������Ҳ��ʵ��ĳmel�����������£��Ͼ�����api���õĻ����ܰ�XX������ģ��һ�Σ����ǿ�����Щbugs. �����ǲ��������ȽϺ���: 
mel command�еģ���MGlobal::executeCommand(...); mel commandʵ������������ or ʵ�ֲ��˵�(���������Զ����ĳЩ����?) ����maya api����. 
 
 
һ��target shape���û�ֱ�Ӱ�"Delete" button ɾ�������target shape����״���Զ�Ĭ�ϱ�bake��blendshape���棬Ҳ������Ȼscene��û�����shape�ˣ���������Ӱ�컹�Ǳ��������blendshape���棬��Ӧ��weight���Ǵ��ڵġ��������ˣ�������ɾ�����û��target shape��weight��ô���أ�
���� blendShape1(source: pCube1; targets: pCube2, pCube3, pCube4). ��pCube3 "Delete" button ɾ����Ҫ��"pCube3" weightȥ��. google "deleteBlendshapeByIndex" http://www.creativecrash.com/forums/rigging-character-setup/topics/delete-one-blendshape-target 
