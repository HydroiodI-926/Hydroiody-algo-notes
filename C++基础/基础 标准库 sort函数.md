# C++ sort 函数详解

## 🎯 **基本用法**
```cpp
#include <algorithm>
#include <vector>
using namespace std;

vector<int> nums = {5, 2, 8, 1, 9};

// 默认升序排序
sort(nums.begin(), nums.end());
// 结果：1, 2, 5, 8, 9
```

## 🔧 **三种排序方式**

### 1. 默认排序（升序）
```cpp
sort(arr.begin(), arr.end());
```

### 2. 使用 greater（降序）
```cpp
#include <functional>
sort(arr.begin(), arr.end(), greater<int>());
// 结果：9, 8, 5, 2, 1
```

### 3. 自定义比较函数
```cpp
// 按字符串长度排序
bool compareLength(const string& a, const string& b) {
    return a.length() < b.length();
}

vector<string> words = {"apple", "cat", "banana"};
sort(words.begin(), words.end(), compareLength);
// 结果：cat, apple, banana
```

## 💡 **Lambda 表达式（推荐）**
```cpp
// 降序排序
sort(nums.begin(), nums.end(), [](int a, int b) {
    return a > b;
});

// 按字符串长度排序
sort(words.begin(), words.end(), [](const string& a, const string& b) {
    return a.length() < b.length();
});
```

## 🛠️ **实际示例**

### 对自定义结构体排序
```cpp
struct Student {
    string name;
    int score;
};

vector<Student> students = {{"Alice", 85}, {"Bob", 92}, {"Charlie", 78}};

// 按分数降序排序
sort(students.begin(), students.end(), [](const Student& a, const Student& b) {
    return a.score > b.score;
});
```

### 多条件排序
```cpp
// 先按分数降序，分数相同按名字升序
sort(students.begin(), students.end(), [](const Student& a, const Student& b) {
    if (a.score == b.score) {
        return a.name < b.name;
    }
    return a.score > b.score;
});
```

## 📋 **重要特性**

- **时间复杂度**：O(N log N)
- **修改原数组**：直接修改，不返回新数组
- **适用范围**：vector, array, deque 等随机访问容器
- **稳定性**：**不稳定排序**（相等元素可能改变相对顺序）

## ⚠️ **稳定排序版本**
```cpp
#include <algorithm>
// 保持相等元素的相对顺序
stable_sort(arr.begin(), arr.end());
```

## 💡 **一句话总结：**
**`sort(开始, 结束, 比较函数)` - 快速排序任何序列！**