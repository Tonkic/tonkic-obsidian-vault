
```C++
#include <vector>
//初始化；
vector<int> a; 
//迭代器迭代一次读两个元素，使用.fisrt .second读取
vector<pair<int, int>>; 
// 定义一个 3x4 的二维数组，初始值为 0
int rows = 3, cols = 4;
vector<vector<int>> arr(rows, vector<int>(cols, 0))
//排序
std::sort(v.begin(), v.end());
例 vector<int> v;//返回最值迭代器，需要取内容
最大值：int maxValue = *max_element(v.begin(),v.end()); 
最小值：int minValue = *min_element(v.begin(),v.end());
//"第二种遍历方式，迭代器访问"
	for (vector<Point>::iterator iter = m_testPoint.begin(); iter != m_testPoint.end(); iter++)
	{
		cout << (*iter).x << "	" << (*iter).y << endl;
	}


# include <unordered_set>
//初始化,a是set，只存键，b是map，键值对
unordered_set<std::string> a;
unordered_map<std::string, int> b;
// 插入元素
a.insert("apple");
// 删除元素
a.erase("cherry");
// 查找元素
a.find("banana");
// 遍历 unordered_set
for (const auto& i : a) {
	std::cout << i << std::endl;
}


#include <map>
//底层是红黑树实现
std::map<key_type, value_type> myMap;
myMap[key] = value;
for (std::map<key_type, value_type>::iterator it = myMap.begin(); it != myMap.end(); ++it) {
    std::cout << it->first << " => " << it->second << std::endl;
}
if (myMap.find(key) != myMap.end()) {
    // 键是否存在
}
```
