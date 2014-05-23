[categories] programming, 

����
---- 

# Using STL Vectors
�ڴ�����std::vector�������õ���? ��Ϊ����������, ֻ����������������ok��. 
������!=�õú�, ���������˽����Ƿ��㹻�����ú���...���������Ķ��ο�����ʱ��ıʼ�. 

Prefer vector over list 
---- 
�Ƚ� std::vector �� std::list, ����һ���ض���, vector���ڴ���������(contiguous buffers), �����Ա���avoid�ڴ涯̬����memory allocations����ɢ�ķ���scattered access.  ��Щ��������. 

���﷢ɢһ�£���ǰ����Qt 4.8��ʱ��(�Ժ�Qt 5�����б�)������QVector, QList, QLinkedList�����ǵ�����Ҳ����˼������QVector��std::vector����, QLinkedList��std::list���ƣ���QList��indices�������ģ�����ָ����������ǲ�������. ������google.

����Ϊʲôvector�ļ����ڴ����ͱ�����ɢ���ʶ��ܳ�Ϊ���ƣ�����Ӧ�����и�������ɵģ����ܸ��ڴ�����йأ��һ�������

Key issues of vector
---- 
����һ��֪ʶ����, vector.size() != vector.capacity(), һ����ʾ�Ѿ�װ�˶���Ԫ��, ��һ����ʾ��ǰ�Ѿ�����Ŀռ����װ���ٸ�Ԫ�أ��е㲻ͬŶ����ô��push_back()��ʱ��size and copacity������ô���ӵ���? 
```
#include <vector> 
#include <iostream>

int main()
{ 
	std::vector<int> arr; 
	for (int i = 0; i < 15; ++i)
	{
		arr.push_back( i ); 
		std::cout << "size = " << arr.size() << ", capacity = " << arr.capacity() << std::endl;
	}
		
	std::cout << "arr.capacity(): " << arr.capacity() << std::endl; 
	arr.swap( arr ); 
	std::cout << "arr.capacity(): " << arr.capacity() << std::endl;
	std::vector<int>(arr).swap( arr ); 
	std::cout << "arr.capacity(): " << arr.capacity() << std::endl;

	std::getchar();
	return 1; 
};
// the output, from visual studio 2012: 
size = 1, capacity = 1
size = 2, capacity = 2
size = 3, capacity = 3
size = 4, capacity = 4
size = 5, capacity = 6
size = 6, capacity = 6
size = 7, capacity = 9
size = 8, capacity = 9
size = 9, capacity = 9
size = 10, capacity = 13
size = 11, capacity = 13
size = 12, capacity = 13
size = 13, capacity = 13
size = 14, capacity = 19
size = 15, capacity = 19
arr.capacity(): 19
arr.capacity(): 19
arr.capacity(): 15
``` 
����std::vector���ܻ�Ԥ�ȷ����������ڴ�(Ӧ�ø�compiler��ʵ���й�?)��Ϊʲô�������Ĳ�����? ������Ԫ��ʱ������Ҫallocation�Ĵ���, ���ǵ�Ԫ�ؼ���reduce��ʱ��, ��������ݾ͸�����. 
��Ϊ��Ԥ�������ݣ����Կ��ܾͻ����over-allocation��������.  ���Ǿ͵�swap������,  �ͷŶ�����ڴ�, ��Сcapacity.  ����swap�ļ���ԭ���и����ֵ�"shrink-to-fit".  ������Ĵ��������У�һ��ʼ�һ��ô���swap����.

��������ӻ���һ������˼�ĵط�����Ҫ������Ԫ��, û�ж���ռ���, vector��һ���ӷ����һ��Ŀռ�, ��ʱ��capacity > size, �������Ƕ������?  ����Ϊ�Ƿ�һ�������������ȥ���ǣ��ǵ����Ƕ����أ�
����������15̫С��, ���������ɣ��Ұ����ĳ�150֮�󣬽����: 
``` 
... 
size = 138, capacity = 141
size = 139, capacity = 141
size = 140, capacity = 141
size = 141, capacity = 141
size = 142, capacity = 211
size = 143, capacity = 211
size = 144, capacity = 211
size = 145, capacity = 211
size = 146, capacity = 211
size = 147, capacity = 211
size = 148, capacity = 211
size = 149, capacity = 211
size = 150, capacity = 211
``` 
211 - 141 = 70����141��һ��! ȥ��������������, ��Ȼ:-)  
``` 
	function push_back (new value)��
		if capacity == size
			allocate more memory( size * 0.5f ); 
			// capacity is bigger than size now;
		append the new value;
		++ size;			
``` 

��һ�������ǣ�vectorҪ������������contiguous�ģ��ǵ���Ҫallocate more memory��ʱ�򣬵�ǰ���ݿ��β����ȴ��һ�����㹻����������Ҫ���? ��ʱ��ͻ��������һ���ط��¿���һ������, ���㹻����ڴ�������Ҫ�󣬰Ѿɵط���ֵ�����µط�ȥ�ᣬ��ָ���ֵ��pointer��ʧЧ�˰�? ���Ǽ�����int��Ϊindex, ����ЩindexӦ�û�����Ч�ģ�ֵ֮����Ⱥ����û�з����ı�. 

``` 
��������⻰����������index����Ч�����⣬������ȥ����Mudbox��sculpt layer groupʱ��������undo���⡣

-> layer 1 -> layer 2 -> layer 3 -> layer 4;
to delete layer 2: 
	need to record its location before deleting; 
	
undo to insert back the layer 2:
	use the recorded location to insert; 
	
����ô��¼layer 2��location��? �м�������ġ���pointer or int index�أ�
- pointer�Ļ�, �����¼location = after layer 1. �Ǽ���delete layer 2֮�� ��delete layer 1, undo to insert back layer 1, undo again to insert back layer 2��ʱ��, ��delete���ֱ�new��layer 1��pointer����һ����
Pointerִ�е��ڴ�ط��ǲ�һ���İɡ������һ�����int index����ʶĳ��layer. 

����, ��indexֻ���ܱ�ʶ����ĳ��layer, ���ǻ���Ҫ��ʾ�������layer��ǰ���أ����������layer�ĺ���. ���������, ����delete��layer�ǵ�һ��or���һ����	
``` 


References: 
---- 
http://upcoder.com/series/1/vectors-and-vector-based-containers/ ����Thomas Young �ǲ��뿪��PathEngine��. 
http://www.codeproject.com/Articles/340797/Number-crunching-Why-you-should-never-ever-EVER-us 
Thank you for sharing your experiences and insights. 