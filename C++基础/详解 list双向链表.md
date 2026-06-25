## 基本介绍
`list` 是 C++ 中的**双向链表容器**，它由节点组成，每个节点包含数据以及指向前一个节点和后一个节点的指针。

## 头文件
```cpp
#include <list>
using namespace std;
```

## 声明和初始化
```cpp
// 各种创建方式
list<int> l1;                    // 空list
list<int> l2(5);                 // 5个元素，都是0
list<int> l3(5, 10);             // 5个元素，都是10
list<int> l4 = {1, 2, 3, 4, 5};  // 直接初始化
list<int> l5(l4);                // 拷贝构造
list<int> l6(l4.begin(), l4.end()); // 通过迭代器范围初始化
```

## 核心特性

### 优点：
- ✅ **任意位置插入删除**：O(1) 时间复杂度
- ✅ **动态大小**：自动管理内存
- ✅ **不移动元素**：插入删除不会导致其他元素移动

### 缺点：
- ❌ **不支持随机访问**：不能像数组一样通过下标访问
- ❌ **内存开销大**：每个元素需要额外两个指针空间
- ❌ **缓存不友好**：节点在内存中不连续

## 常用操作详解

### 添加元素
```cpp
list<int> l;

// 尾部添加
l.push_back(1);     // list: [1]
l.push_back(2);     // list: [1, 2]

// 头部添加
l.push_front(0);    // list: [0, 1, 2]

// 在指定位置插入
auto it = l.begin();
it++;               // 指向第一个元素（1）
l.insert(it, 99);   // 在1之前插入99: [0, 99, 1, 2]

// 插入多个元素
l.insert(it, 3, 88); // 在1之前插入3个88: [0, 99, 88, 88, 88, 1, 2]
```

### 删除元素
```cpp
list<int> l = {0, 1, 2, 3, 4, 5};

// 删除头部元素
l.pop_front();      // 删除0: [1, 2, 3, 4, 5]

// 删除尾部元素
l.pop_back();       // 删除5: [1, 2, 3, 4]

// 删除指定位置元素
auto it = l.begin();
it++;               // 指向2
l.erase(it);        // 删除2: [1, 3, 4]

// 删除多个元素
it = l.begin();
it++;               // 指向3
auto it2 = l.end();
l.erase(it, it2);   // 删除从3到末尾: [1]

// 删除所有元素
l.clear();          // 清空list
```

### 访问元素
```cpp
list<int> l = {1, 2, 3, 4, 5};

// 访问头尾元素
cout << l.front() << endl;  // 1（头部元素）
cout << l.back() << endl;   // 5（尾部元素）

// 注意：list不支持随机访问，不能使用[]和at()
// cout << l[2] << endl;   // ❌ 错误
```
[[详解 list如何遍历]]
### 遍历方法
```cpp
list<int> l = {1, 2, 3, 4, 5};

// 方法1：范围for循环（推荐）
for (int value : l) {
    cout << value << " ";
}

// 方法2：迭代器遍历
for (auto it = l.begin(); it != l.end(); it++) {
    cout << *it << " ";
}

// 方法3：反向迭代器
for (auto it = l.rbegin(); it != l.rend(); it++) {
    cout << *it << " ";  // 逆序输出
}
```

## 容量和信息
```cpp
list<int> l = {1, 2, 3, 4, 5};

cout << l.size() << endl;     // 5（元素个数）
cout << l.empty() << endl;    // false（是否为空）

// 调整大小
l.resize(8);                  // 大小变为8，新元素为0: [1,2,3,4,5,0,0,0]
l.resize(3);                  // 大小变为3: [1,2,3]
```

## 特殊操作（list特有）

### 排序
```cpp
list<int> l = {3, 1, 4, 2, 5};

l.sort();  // 排序: [1, 2, 3, 4, 5]

// 自定义排序规则
l.sort(greater<int>());  // 降序排序: [5, 4, 3, 2, 1]
```

### 反转
```cpp
list<int> l = {1, 2, 3, 4, 5};

l.reverse();  // 反转: [5, 4, 3, 2, 1]
```

### 合并两个有序list
```cpp
list<int> l1 = {1, 3, 5};
list<int> l2 = {2, 4, 6};

l1.merge(l2);  // 合并后l1: [1,2,3,4,5,6], l2为空

// 注意：两个list必须都是有序的（升序）
```

### 拼接（splice）
```cpp
list<int> l1 = {1, 2, 3};
list<int> l2 = {4, 5, 6};

// 将l2的全部元素拼接到l1的末尾
l1.splice(l1.end(), l2);  // l1: [1,2,3,4,5,6], l2为空

// 拼接单个元素
l2 = {7, 8, 9};
auto it = l2.begin();
l1.splice(l1.begin(), l2, it);  // 将l2的第一个元素（7）拼接到l1开头
// l1: [7,1,2,3,4,5,6], l2: [8,9]

// 拼接一个范围内的元素
l2 = {10, 11, 12};
it = l2.begin();
it++;  // 指向11
l1.splice(l1.end(), l2, l2.begin(), it);  // 将l2中[begin, it)范围的元素（10）拼接到l1末尾
// l1: [7,1,2,3,4,5,6,10], l2: [11,12]
```

### 去重
```cpp
list<int> l = {1, 2, 2, 3, 3, 3, 4, 4, 4, 4};

l.unique();  // 去除连续重复元素: [1,2,3,4]

// 自定义去重规则（例如去除相邻且绝对值差小于1的元素）
l = {1, 2, 4, 3, 3, 2, 1};
l.sort();    // 先排序: [1,1,2,2,3,3,4]
l.unique();  // 去重: [1,2,3,4]
```

## 实际应用示例

### 示例1：学生名单管理
```cpp
#include <iostream>
#include <list>
#include <string>
using namespace std;

struct Student {
    string name;
    int score;
    
    Student(string n, int s) : name(n), score(s) {}
};

int main() {
    list<Student> students;
    
    // 添加学生
    students.push_back(Student("Alice", 85));
    students.push_back(Student("Bob", 92));
    students.push_back(Student("Charlie", 78));
    
    // 在头部添加一个学生
    students.push_front(Student("David", 88));
    
    // 遍历输出
    cout << "学生名单:" << endl;
    for (const Student& s : students) {
        cout << s.name << ": " << s.score << endl;
    }
    
    return 0;
}
```

### 示例2：任务列表（频繁插入删除）
```cpp
#include <iostream>
#include <list>
#include <string>
using namespace std;

int main() {
    list<string> tasks;
    
    // 添加任务
    tasks.push_back("写报告");
    tasks.push_back("开会");
    tasks.push_back("调试代码");
    
    // 在第二个任务前插入紧急任务
    auto it = tasks.begin();
    it++;  // 指向第二个任务
    tasks.insert(it, "紧急修复");
    
    // 删除第一个任务
    tasks.pop_front();
    
    // 遍历输出
    cout << "当前任务:" << endl;
    for (const string& task : tasks) {
        cout << "- " << task << endl;
    }
    
    return 0;
}
```

## 在算法竞赛中的应用

### 应用1：约瑟夫问题
```cpp
#include <iostream>
#include <list>
using namespace std;

int josephus(int n, int k) {
    list<int> people;
    for (int i = 1; i <= n; i++) {
        people.push_back(i);
    }
    
    auto it = people.begin();
    while (people.size() > 1) {
        for (int count = 1; count < k; count++) {
            it++;
            if (it == people.end()) {
                it = people.begin();
            }
        }
        
        it = people.erase(it);
        if (it == people.end()) {
            it = people.begin();
        }
    }
    
    return people.front();
}

int main() {
    int n = 7, k = 3;
    cout << "约瑟夫问题: n=" << n << ", k=" << k << " 生存者: " << josephus(n, k) << endl;
    return 0;
}
```

### 应用2：大数据量的频繁插入删除
```cpp
// 当需要在一个序列中频繁插入删除，且不需要随机访问时，list比vector和deque更高效
#include <iostream>
#include <list>
#include <vector>
#include <chrono>
using namespace std;

void testPerformance() {
    const int N = 10000;
    
    // 测试vector在中间插入
    vector<int> vec;
    auto start = chrono::high_resolution_clock::now();
    for (int i = 0; i < N; i++) {
        // 在中间插入，vector需要移动后面所有元素
        vec.insert(vec.begin() + vec.size() / 2, i);
    }
    auto end = chrono::high_resolution_clock::now();
    cout << "Vector 中间插入时间: " 
         << chrono::duration_cast<chrono::milliseconds>(end - start).count() 
         << " ms" << endl;
    
    // 测试list在中间插入
    list<int> lst;
    start = chrono::high_resolution_clock::now();
    for (int i = 0; i < N; i++) {
        auto it = lst.begin();
        advance(it, lst.size() / 2);  // 移动到中间位置
        lst.insert(it, i);
    }
    end = chrono::high_resolution_clock::now();
    cout << "List 中间插入时间: " 
         << chrono::duration_cast<chrono::milliseconds>(end - start).count() 
         << " ms" << endl;
}

int main() {
    testPerformance();
    return 0;
}
```

## 性能注意事项

### 时间复杂度
- **任意位置插入删除**：O(1)（已知位置）
- **查找元素**：O(n)
- **排序**：O(n log n)
- **访问头尾元素**：O(1)

### 空间复杂度
- O(n)，每个元素需要两个指针的额外空间

## 与vector和deque的对比

| 特性 | list | vector | deque |
|------|------|--------|-------|
| 内存布局 | 不连续 | 连续 | 分段连续 |
| 随机访问 | 不支持 | O(1) | O(1) |
| 头部插入删除 | O(1) | O(n) | O(1) |
| 尾部插入删除 | O(1) | O(1) | O(1) |
| 中间插入删除 | O(1) | O(n) | O(n) |
| 迭代器类型 | 双向 | 随机 | 随机 |
| 缓存友好性 | 差 | 好 | 中等 |

## 常见错误

### 错误1：无效的迭代器操作
```cpp
list<int> l = {1, 2, 3};
auto it = l.begin();

// 错误：list迭代器不支持随机访问
// it = it + 2;  // ❌ 错误

// 正确：使用advance或多次自增
advance(it, 2);  // ✅ 正确
// 或者
it++; it++;      // ✅ 正确
```

### 错误2：在遍历时删除元素
```cpp
list<int> l = {1, 2, 3, 4, 5};

// 错误：在删除元素后继续使用无效的迭代器
for (auto it = l.begin(); it != l.end(); it++) {
    if (*it == 3) {
        l.erase(it);  // ❌ 错误，it失效后还继续自增
    }
}

// 正确：使用返回值更新迭代器
for (auto it = l.begin(); it != l.end(); ) {
    if (*it == 3) {
        it = l.erase(it);  // ✅ 正确，erase返回下一个迭代器
    } else {
        it++;
    }
}
```

## 总结

`list` 是**双向链表**，核心特点：
- 任意位置快速插入删除
- 不支持随机访问
- 内存不连续，缓存不友好
- 有特殊的排序、合并、去重操作

**适用场景：**
- 需要频繁在任意位置插入删除
- 不需要随机访问
- 需要特殊的链表操作（排序、合并、去重等）

**记忆口诀：**
> "list链表，插入删除快，访问比较慢，特殊操作多"