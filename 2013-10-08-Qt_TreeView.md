{

title: "Tree view at Qt"

tags: ["qt"] 
} 

����
----
> Model/View/item�Ĺ�ϵ���е���Container/Algorithm/Iterator�Ĺ�ϵ��
> Tree view����Model/View Programming��һ���֡���Щ
> 

Index �����²㼶�ṹ�Ķ�ά���� 
----
QModelIndex()��ʾmodel�����ϲ��root item��index, QModelIndex().isValid() == 0. 

item֮ǰ�����²㼶��ϵ��index.parent()��ȡindex��parent. ��index.parent().isValid() == false����ʾ���item��parent����root item. ��Щitems����Ϊtop-level itmes. 

to retrieve data from a model:
for (int row = 0; row < model->rowCount(parentIndex); ++row)
{
	for (int col = 0; col < model->columnCount(parentIndex); ++col)
	{
		QString text = model->data(index, Qt::DisplayRole).toString();
        // Display the text in a widget.
	}
}
����tree view�е�ÿһ��row�����ǵ�column��Ŀ����һ������
ÿһ��index��role�����Ͷ�һ��? �뿴�Ƿ���QVariant CustomModel::data(const QModelIndex &, int role) const {} ò��һ��index�����и��ָ�����role������Է�ʲô������ok��ֻҪ������������и���Ӧ��role���غ��ʵ����ݡ���������ڴ���ǳ����ã��������CustomDelegate::paint()�о��Ǹ���dataֵ���ж���ô���ͻ�ʲô�ġ�
Question: QVariant QModelIndex::data(role); ���������model->data(index, role)���һ������? �����и�Сϸ��, model��ָ�����data()����override. 
��data(...)�����Ķ�Ӧ����һ������CustomModel::setData(...); �������һ���õĵط����࣬���ڸ�model��������insertRow()֮���ÿһ��index���벻ͬ������ʱ����õ���

Delegate �������ر��ͼ���ͽ�����Ҫ 
----
ǰ��: install a custom delegate for a view. 
����: render and editting. 

To render:
	Delegate::paint().
	
To provide editing facilities:
	QWidget *Delegate::createEditor(QWidget *, ..., const QModelIndex &);
	Ϊĳһ�� or ÿһ��index�ṩeditor? Can we create different editors depending on the model index supplied by the view ? ����ĳЩ���б�edit��ֻ�ǿհ���? 
	
	��Щnew QWidget objӦ���Ǳ�view��manager�ģ��������ܾ���view->delegate->createEditor for a given index.
	
	һ���������ĳ��item/index������һ��QWidget obj�Ժ��Ǹ�viewò�ƾͻ��Զ������index������QWidget obj�������������ǲ���Ҫ���ģ����ǿ��������������֮����ô�� (a) copy the model data into the editor, Delegate::setEditorData(QWidget *, const QModelIndex &) const, ���������߾���view�����ǵ�, index�� widget�Ѿ�һһ��Ӧ�õ�. (b) when user has finished editting the value in the widget, the view asks the delegate to store the edited value in the model by calling Delegate::setModelData(QWidget *, model, index) const; �����(a and b)������������editor <-> model/index.  
	
Question: in the delegate class, how to get its view ? try to get the QObject *parent() first and cast it to the QTreeView or custom view type.
	
	
Render
----
Scenior: һ�������Զ���CustomModel, CustomView, CustomeDelegate��֮��ſ����Լ�Ҫ��ô��Tree view�еĶ���. 

CustomView::paintEvent(QPaintEvent *)
	drawTree(...);
		drawRow(QPainter *, const option, const index); ��ʲô����?
			ò�ƿ���ѡ����ض���index (item)��һЩ����, Ȼ��QTreeView(...)��Ĭ�ϻ���
			drawBranches(QPainter *, const QRect &rect, const index) const;
			delegate->paint(...);
			
�ɼ�������һ��һ�еػ�������. �Ȼ�branch��֦��Ȼ����delegate->paint()�����ݡ�
D:\Qt\Qt5.1.0\5.1.0\Src\qtbase\src\widgets\itemviews\qtreeview.cpp for details.

darwBranches(..., const QRect &rect, ...)
	�ڷ�֦����һ�����������һ����һ�����ұ�������setIndentation(int)�������������Ķ��١���rect�������������һ�е�������ұ߼���ÿһ�ε��������á�����һ��top-level �Ǵ�root-item����һ�����á�����expand/collapse��event����ֻ�ڻ�branch�����������Ӧ�ġ�
	�������ˣ����������������level��һ���ģ��ɷ�ͬlevel�ò�ͬ��С��������? δ�⡣
example:
--level0
----level1
------level2
the indentation for this is 2 (--). 

�������level0ǰ�������ȥ��������QTreeView::setRootIsDecorated(false);



Read-Only access : (Qt::ItemIsEnabled | Qt::ItemIsSelectable)
Editable items: Qt::ItemIsEditable | (Qt::ItemIsEnabled | Qt::ItemIsSelectable) ����Read-Only access�����ϼӵ�. 
������һ��۲�Ƚ���Ҫ������flag���治����Qt::ItemIsEnabled, ��ô���item�����ϲ��ܸ�ɶ����, ���粻��editable�� 

Drag & Drop: ��һ��item drag����һ��item���棬��Щ�ǽ��ܣ���Щ�ǲ����ܵģ�Ϊʲô�أ�
Model::flags(const QModelIndex &) contain the Qt::ItemIsDragEnabled and Qt::ItemIsDropEnabled. ǰ�ᵱȻ���Ѿ����������ᵽ��(Qt::ItemIsEnabled | Qt::ItemIsSelectable),���Թ���Qt::ItemIsEnabledȥ�������item�Ͳ��ܱ�ѡ����, ò�ƾͲ���Ӧmouse event�ˡ�
��View���滹��һЩ����setDragDropMode(QAbstractItemView::DragDrop); setDragEnabled(true); setAcceptDrops(true); setDropIndicatorShown(true);
debugʱ�򿴵�����ĺ�������: mouse move -> mouseMoveEvent() -> ����LMB�϶�-> startDrag() -> dragEnterEvent() -> dragMoveEvent() -> drop the item -> dropEvent()


