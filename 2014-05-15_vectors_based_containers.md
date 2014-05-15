[categories] programming, 

����
---- 
> �ڴ�����std::vector�������õ���? ��Ϊ����������, ֻ����������������ok��. 
> ������!=�õú�, ���������˽����Ƿ��㹻�����ú���...���������Ķ��ο�����ʱ��ıʼ�. 

# Using STL Vectors

Prefer vector over list 
---- 
�Ƚ� std::vector �� std::list, ����һ���ض���, vector���ڴ���������(contiguous buffers), �����Ա���avoid�ڴ涯̬����memory allocations����ɢ�ķ���scattered access.  ��Щ��������. 

���﷢ɢһ�£���ǰ����Qt 4.8��ʱ��(�Ժ�Qt 5�����б�)������QVector, QList, QLinkedList�����ǵ�����Ҳ����˼������QVector��std::vector����, QLinkedList��std::list���ƣ���QList��indices�������ģ�����ָ����������ǲ�������. ������google.

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
// the output: 
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






References: 
---- 
http://upcoder.com/series/1/vectors-and-vector-based-containers/ ����Thomas Young �ǲ��뿪��PathEngine��. 
http://www.codeproject.com/Articles/340797/Number-crunching-Why-you-should-never-ever-EVER-us 
Thank you for sharing your experiences and insights. 