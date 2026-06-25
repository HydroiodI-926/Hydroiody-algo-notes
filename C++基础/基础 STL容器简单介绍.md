## 什么是STL容器？
STL容器就像**各种不同的储物柜**，每种储物柜有自己独特的存放和取用方式。

## 常用STL容器分类

### 1. vector（动态数组）📚
```cpp
#include <vector>
using namespace std;

vector<int> v = {1, 2, 3};

// 像数组一样使用
v.push_back(4);     // 尾部添加：{1,2,3,4}
v.pop_back();       // 尾部删除：{1,2,3}
cout << v[0];       // 访问第一个元素：1
```

**特点：** 自动扩容的数组，访问快，尾部操作快
[[详解 vector动态数组]]

---

### 2. string（字符串）🔤
```cpp
#include <string>
using namespace std;

string s = "hello";
s += " world";      // "hello world"
cout << s[0];       // 'h'
cout << s.size();   // 11
```

**特点：** 专门存文本，功能丰富的字符串
[[详解 string字符串]]

---

### 3. map（字典）🗺️
```cpp
#include <map>
using namespace std;

map<string, int> scores;
scores["小明"] = 90;
scores["小红"] = 85;

cout << scores["小明"];  // 90
```

**特点：** 键值对存储，自动按键排序
[[详解 map映射数组]]

---

### 4. set（集合）🎯
```cpp
#include <set>
using namespace std;

set<int> numbers = {3, 1, 2};
numbers.insert(4);      // {1,2,3,4}
numbers.insert(2);      // 还是{1,2,3,4}（去重）

if(numbers.find(3) != numbers.end()) {
    cout << "找到了3";
}
```

**特点：** 自动去重，自动排序
[[详解 set集合]]

---

### 5. stack（栈）📚
```cpp
#include <stack>
using namespace std;

stack<int> s;
s.push(1);      // 压栈
s.push(2);
s.push(3);

cout << s.top();    // 3（查看栈顶）
s.pop();            // 弹出栈顶
cout << s.top();    // 2
```

**特点：** 后进先出（LIFO）
[[详解 stack栈]]

---

### 6. queue（队列）🚶‍♂️🚶‍♂️🚶‍♂️
```cpp
#include <queue>
using namespace std;

queue<int> q;
q.push(1);      // 入队
q.push(2);
q.push(3);

cout << q.front();  // 1（队首）
q.pop();            // 出队
cout << q.front();  // 2
```

**特点：** 先进先出（FIFO），像排队
[[详解 queue队列]]

---

### 7. deque（双端队列）🔁
```cpp
#include <deque>
using namespace std;

deque<int> dq = {2, 3};
dq.push_front(1);   // 头部添加：{1,2,3}
dq.push_back(4);    // 尾部添加：{1,2,3,4}

cout << dq.front(); // 1
cout << dq.back();  // 4
```

**特点：** 头尾都能快速插入删除
[[详解 deque双端队列]]

---

### 8. list（双向链表）🔗
```cpp
#include <list>
using namespace std;

list<int> lst = {1, 3};
lst.push_front(0);  // 头部添加
lst.push_back(4);   // 尾部添加

auto it = lst.begin();
it++;               // 移动到第二个元素
lst.insert(it, 2);  // 在第二个位置插入2
// 现在：{0,1,2,3,4}
```

**特点：** 任意位置插入删除快，但访问慢
[[详解 list双向链表]]

---

### 9. unordered_map / unordered_set（哈希版）⚡
```cpp
#include <unordered_map>
#include <unordered_set>

unordered_map<string, int> hashMap;  // 更快但不排序的map
unordered_set<int> hashSet;          // 更快但不排序的set
```

**特点：** 查找极快，但不保持顺序
[[详解 unordered_map or unordered_set]]

## 快速选择指南

| 你需要... | 选择这个容器 |
|-----------|-------------|
| 普通数组，经常访问 | `vector` |
| 键值对，要排序 | `map` |
| 键值对，要最快查找 | `unordered_map` |
| 去重，要排序 | `set` |
| 后进先出 | `stack` |
| 先进先出 | `queue` |
| 头尾都要操作 | `deque` |
| 频繁在中间插入删除 | `list` |
| 字符串操作 | `string` |

## 通用操作（大部分容器都有）

```cpp
vector<int> v = {1, 2, 3};

v.size();       // 元素个数
v.empty();      // 是否为空
v.clear();      // 清空所有元素

// 遍历（通用方法）
for(int i = 0; i < v.size(); i++) {
    cout << v[i] << " ";
}

// 或者用迭代器
for(auto it = v.begin(); it != v.end(); it++) {
    cout << *it << " ";
}

// 或者最简单（C++11）
for(int num : v) {
    cout << num << " ";
}
```
[[基础 比较器]]
## 算法竞赛常用组合

```cpp
#include <vector>
#include <map>
#include <set>
#include <algorithm>  // 排序等算法
#include <iostream>
using namespace std;

int main() {
    // 90%的情况用这三个就够了
    vector<int> arr;      // 动态数组
    map<string, int> dict; // 字典
    set<int> uniqueNums;  // 去重集合
    
    return 0;
}
```

**记住：** STL容器让编程更简单，不用自己造轮子！