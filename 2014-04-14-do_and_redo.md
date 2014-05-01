
[categories] programming


��ѧmaya/MPxCommand, ��һ��������Ҫ֧��undo/redo��ʱ���û������Ĵ��������: 
- do sth -> undo it, or 
- do sth -> undo it -> redo it;
��ʵ�ֵ�ʱ��ᷢ��do sth �� redo sth�Ĵ������һ�������ǵ�Ȼ��дһ������do and redo���õĺ����ͺ��ˣ���������ظ��

maya/devkit��������ʹ�õķ�����:
```
//code  
virtual bool isUndoable() const { return true or false; }
virtual MStatus doIt( const MArglist & )
{
	// parse the argument list;
	// do some setting;
	return redoIt(); 
}
virtual MStatus redoIt() { do the actually command operation here }; 
virtual MStatus undoIt() {}; 
``` 
���û���������Ctrl+Z����undo, �־�����Ҫredo��ȥ, ��ô Shift+Z��redo��ʱ�����operation��setting��ʵ��������֮ǰ��doItʱ���setting������doIt()��redoIt()����һ�������ѡ�

���do/redo��������ķֹ����⣬����ǰ��Mudbox project�о�����������ʱ��д���е���:
```
// code
enum Status { DO, UNDO, REDO } status; 
virtual bool CustomOp::ExecuteAndInvert( void )
{
    if (status == DO) {}
	else if (status == UNDO) {}
	else (status == REDO) {} 
};
// for more details, check the mudbox sdk operation.h;
```
