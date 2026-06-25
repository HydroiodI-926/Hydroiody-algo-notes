# 1. vector（动态数组）

## 基本介绍
`vector` 就像是**可以自动变长的智能数组**。你不用担心它的大小，它会根据需要自己扩容。

## 头文件
```cpp
#include <vector>
using namespace std;
```

## 声明和初始化
```cpp
// 各种创建方式
vector<int> v1;                    // 空vector
vector<int> v2(5);                 // 5个元素，都是0
vector<int> v3(5, 10);             // 5个元素，都是10
vector<int> v4 = {1, 2, 3, 4, 5};  // 直接初始化
vector<int> v5(v4);                // 拷贝v4
```

## 核心特性

### 优点：
- ✅ **自动扩容**：不用手动管理大小
- ✅ **快速随机访问**：像数组一样用 `v[i]` 访问
- ✅ **尾部操作高效**：在末尾添加/删除很快

### 缺点：
- ❌ **中间插入删除慢**：需要移动后面所有元素
- ❌ **扩容时有开销**：扩容需要拷贝所有元素到新内存

## 常用操作详解

### 添加元素
```cpp
vector<int> v = {1, 2, 3};

v.push_back(4);        // 尾部添加：{1,2,3,4}
v.insert(v.begin(), 0); // 开头插入：{0,1,2,3,4}
v.insert(v.begin() + 2, 9); // 在索引2处插入：{0,1,9,2,3,4}
```

### 删除元素
```cpp
vector<int> v = {1, 2, 3, 4, 5};

v.pop_back();          // 删除尾部：{1,2,3,4}
v.erase(v.begin());    // 删除开头：{2,3,4}
v.erase(v.begin() + 1); // 删除索引1：{2,4}
v.clear();             // 清空所有：{}
```

### 访问元素
```cpp
vector<int> v = {10, 20, 30};

cout << v[0];          // 10（不检查边界，快）
cout << v.at(1);       // 20（检查边界，安全）
cout << v.front();     // 10（第一个元素）
cout << v.back();      // 30（最后一个元素）
```

[[详解 at()方法的边界检查]]

### 容量管理
```cpp
vector<int> v;

cout << v.size();      // 当前元素个数
cout << v.capacity();  // 当前容量（>=size）
cout << v.empty();     // 是否为空

v.reserve(100);        // 预分配100个元素的空间
v.resize(5);           // 调整大小为5，多删少补0
```

[[详解 .capacity()]]

## 遍历方法
```cpp
vector<int> v = {1, 2, 3, 4, 5};

// 方法1：下标遍历（最常用）
for(int i = 0; i < v.size(); i++) {
    cout << v[i] << " ";
}

// 方法2：范围for循环（C++11，简洁）
for(int num : v) {
    cout << num << " ";
}

// 方法3：迭代器（灵活）
for(auto it = v.begin(); it != v.end(); it++) {
    cout << *it << " ";
}
```

## 实际应用示例
```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    // 创建存储学生分数的vector
    vector<int> scores;
    
    // 添加分数
    scores.push_back(85);
    scores.push_back(92);
    scores.push_back(78);
    
    // 计算平均分
    int sum = 0;
    for(int score : scores) {
        sum += score;
    }
    double average = (double)sum / scores.size();
    
    cout << "平均分: " << average << endl;
    cout << "共 " << scores.size() << " 个分数" << endl;
    
    return 0;
}
```

## 内存机制理解
```cpp
vector<int> v;
// 初始：容量=0，大小=0

v.push_back(1);
// 第一次扩容：容量=1，大小=1

v.push_back(2); 
// 第二次扩容：容量=2，大小=2

v.push_back(3);
// 第三次扩容：容量=4，大小=3（容量翻倍）

v.push_back(4);
// 容量=4，大小=4（刚好够用）

v.push_back(5);
// 第四次扩容：容量=8，大小=5
```

## 算法竞赛技巧
```cpp
// 1. 预分配空间避免频繁扩容
vector<int> v;
v.reserve(1000000);  // 知道大概大小时预先分配

// 2. 快速清空（不释放内存）
v.clear();
// 或者
vector<int>().swap(v);  // 彻底释放内存

// 3. 二维vector
vector<vector<int>> matrix(3, vector<int>(4, 0));
// 创建3行4列的矩阵，初始化为0
```

## 总结
`vector` 是**最常用**的容器，可以替代大多数数组场景。记住它的核心特点：
- 自动管理大小
- 尾部操作快
- 支持随机访问
- 中间操作慢