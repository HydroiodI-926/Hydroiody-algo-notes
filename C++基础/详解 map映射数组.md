# 4. map（映射容器）

## 基本介绍
`map` 是 C++ 中的**键值对容器**，就像字典一样，通过**键**来快速查找**值**。

## 头文件
```cpp
#include <map>
using namespace std;
```

## 声明和初始化
```cpp
// 各种创建方式
map<string, int> m1;                    // 空map
map<string, int> m2 = {
    {"apple", 1},
    {"banana", 2},
    {"orange", 3}
};                                     // 直接初始化

map<int, string> m3;
map<pair<int, int>, bool> m4;          // 键可以是pair等类型
```

## 核心特性

### 优点：
- ✅ **快速查找**：基于红黑树，查找效率 O(log n)
- ✅ **自动排序**：按键的升序自动排列
- ✅ **键唯一性**：每个键只能出现一次
- ✅ **灵活键类型**：支持各种类型作为键

### 缺点：
- ❌ **内存开销**：比数组和vector大
- ❌ **插入删除稍慢**：需要维护树结构

## 常用操作详解

### 插入元素
```cpp
map<string, int> scores;

// 方法1：使用[]运算符（最常用）
scores["Alice"] = 95;
scores["Bob"] = 87;

// 方法2：使用insert函数
scores.insert({"Charlie", 92});
scores.insert(make_pair("David", 88));

// 方法3：使用emplace（C++11，效率高）
scores.emplace("Eve", 90);
```

### 访问元素
```cpp
map<string, int> scores = {{"Alice", 95}, {"Bob", 87}};

// 方法1：使用[]运算符
cout << scores["Alice"] << endl;  // 95

// 方法2：使用at（安全，会检查）
cout << scores.at("Bob") << endl; // 87

// 方法3：使用find（检查是否存在）
auto it = scores.find("Charlie");
if (it != scores.end()) {
    cout << it->second << endl;   // 找到，输出值
} else {
    cout << "未找到" << endl;
}
```
[[详解  箭头运算符]]
### 修改元素
```cpp
map<string, int> scores = {{"Alice", 95}};

scores["Alice"] = 98;                    // 直接修改
scores.at("Alice") = 99;                 // 使用at修改

// 如果键不存在，[]会创建新元素
scores["NewStudent"] = 85;              // 自动创建并赋值
```

### 删除元素
```cpp
map<string, int> scores = {
    {"Alice", 95}, {"Bob", 87}, {"Charlie", 92}
};

// 方法1：通过键删除
scores.erase("Bob");                    // 删除键为"Bob"的元素

// 方法2：通过迭代器删除
auto it = scores.find("Charlie");
if (it != scores.end()) {
    scores.erase(it);                   // 删除找到的元素
}

// 方法3：删除多个元素
scores.erase(scores.begin(), scores.end());  // 删除所有元素
// 或者
scores.clear();                         // 清空map
```

### 检查元素是否存在
```cpp
map<string, int> scores = {{"Alice", 95}};

// 方法1：使用find
if (scores.find("Alice") != scores.end()) {
    cout << "Alice存在" << endl;
}

// 方法2：使用count（返回0或1）
if (scores.count("Bob") > 0) {
    cout << "Bob存在" << endl;
} else {
    cout << "Bob不存在" << endl;
}

// 方法3：使用contains（C++20）
// if (scores.contains("Alice")) { ... }
```

## 遍历方法
```cpp
map<string, int> scores = {
    {"Alice", 95}, {"Bob", 87}, {"Charlie", 92}
};

// 方法1：迭代器遍历
for (auto it = scores.begin(); it != scores.end(); it++) {
    cout << it->first << ": " << it->second << endl;
}

// 方法2：范围for循环（推荐）
for (const auto& pair : scores) {
    cout << pair.first << ": " << pair.second << endl;
}

// 方法3：结构化绑定（C++17，最简洁）
for (const auto& [name, score] : scores) {
    cout << name << ": " << score << endl;
}
```

**输出：**
```
Alice: 95
Bob: 87  
Charlie: 92
```

## 容量和信息
```cpp
map<string, int> scores = {{"Alice", 95}, {"Bob", 87}};

cout << scores.size() << endl;      // 2（元素个数）
cout << scores.empty() << endl;     // false（是否为空）

// 获取边界
auto first = scores.begin();        // 指向最小键的迭代器
auto last = scores.end();           // 末尾迭代器
```

## 实际应用示例

### 示例1：单词计数器
```cpp
#include <iostream>
#include <map>
#include <string>
using namespace std;

int main() {
    map<string, int> wordCount;
    string word;
    
    cout << "请输入单词（输入end结束）:" << endl;
    while (cin >> word && word != "end") {
        wordCount[word]++;  // 自动计数
    }
    
    cout << "\n单词统计结果:" << endl;
    for (const auto& [w, count] : wordCount) {
        cout << w << ": " << count << "次" << endl;
    }
    
    return 0;
}
```

### 示例2：学生成绩管理系统
```cpp
#include <iostream>
#include <map>
#include <string>
using namespace std;

int main() {
    map<string, int> studentScores;
    
    // 添加学生成绩
    studentScores["张三"] = 85;
    studentScores["李四"] = 92;
    studentScores["王五"] = 78;
    studentScores["赵六"] = 96;
    
    // 查询成绩
    string name;
    cout << "请输入要查询的学生姓名: ";
    cin >> name;
    
    auto it = studentScores.find(name);
    if (it != studentScores.end()) {
        cout << name << "的成绩是: " << it->second << endl;
    } else {
        cout << "未找到学生: " << name << endl;
    }
    
    // 显示所有学生成绩（按姓名排序）
    cout << "\n所有学生成绩:" << endl;
    for (const auto& [student, score] : studentScores) {
        cout << student << ": " << score << endl;
    }
    
    return 0;
}
```

### 示例3：投票系统
```cpp
#include <iostream>
#include <map>
#include <string>
using namespace std;

int main() {
    map<string, int> votes;
    string candidate;
    
    cout << "投票系统（输入end结束投票）:" << endl;
    while (true) {
        cout << "请投票给: ";
        cin >> candidate;
        
        if (candidate == "end") {
            break;
        }
        
        votes[candidate]++;
        cout << candidate << " 获得一票，当前票数: " << votes[candidate] << endl;
    }
    
    // 显示投票结果
    cout << "\n最终投票结果:" << endl;
    for (const auto& [candidate, voteCount] : votes) {
        cout << candidate << ": " << voteCount << "票" << endl;
    }
    
    return 0;
}
```

## 在算法竞赛中的应用

### 应用1：统计频率
```cpp
// 统计数组中每个数字出现的次数
#include <iostream>
#include <map>
#include <vector>
using namespace std;

int main() {
    vector<int> nums = {1, 2, 3, 2, 1, 3, 3, 4, 5, 4, 3};
    map<int, int> freq;
    
    for (int num : nums) {
        freq[num]++;
    }
    
    for (const auto& [num, count] : freq) {
        cout << num << " 出现了 " << count << " 次" << endl;
    }
    
    return 0;
}
```

### 应用2：字符串映射
```cpp
// 将字符串映射到索引
#include <iostream>
#include <map>
#include <string>
#include <vector>
using namespace std;

int main() {
    vector<string> names = {"Alice", "Bob", "Charlie", "Alice", "Bob"};
    map<string, int> nameToIndex;
    int nextIndex = 1;
    
    for (const string& name : names) {
        if (nameToIndex.find(name) == nameToIndex.end()) {
            nameToIndex[name] = nextIndex++;
        }
        cout << name << " -> " << nameToIndex[name] << endl;
    }
    
    return 0;
}
```

### 应用3：坐标计数
```cpp
// 统计二维坐标点的出现次数
#include <iostream>
#include <map>
using namespace std;

int main() {
    map<pair<int, int>, int> pointCount;
    
    // 模拟一些坐标点
    vector<pair<int, int>> points = {
        {1, 2}, {3, 4}, {1, 2}, {5, 6}, {3, 4}, {1, 2}
    };
    
    for (const auto& point : points) {
        pointCount[point]++;
    }
    
    for (const auto& [point, count] : pointCount) {
        cout << "(" << point.first << ", " << point.second 
             << ") 出现了 " << count << " 次" << endl;
    }
    
    return 0;
}
```

## 特殊用法和技巧

### 默认值处理
```cpp
map<string, int> scores;

// 访问不存在的键会创建并赋默认值（0）
cout << scores["Unknown"] << endl;  // 输出0，并创建新元素

// 避免意外创建，先用find检查
if (scores.find("AnotherUnknown") != scores.end()) {
    cout << scores["AnotherUnknown"] << endl;
}
```

### 获取最小/最大键
```cpp
map<string, int> scores = {
    {"Charlie", 92}, {"Alice", 95}, {"Bob", 87}
};

if (!scores.empty()) {
    auto minKey = scores.begin()->first;    // 最小键："Alice"
    auto maxKey = scores.rbegin()->first;   // 最大键："Charlie"
    
    cout << "最小键: " << minKey << endl;
    cout << "最大键: " << maxKey << endl;
}
```

### 自定义比较函数
```cpp
// 按键的降序排列
#include <iostream>
#include <map>
#include <string>
using namespace std;

struct CompareDesc {
    bool operator()(const string& a, const string& b) const {
        return a > b;  // 降序
    }
};

int main() {
    map<string, int, CompareDesc> scores = {
        {"Alice", 95}, {"Bob", 87}, {"Charlie", 92}
    };
    
    for (const auto& [name, score] : scores) {
        cout << name << ": " << score << endl;
    }
    
    return 0;
}
```

**输出：**
```
Charlie: 92
Bob: 87
Alice: 95
```

## 性能注意事项

### 时间复杂度
- **插入**：O(log n)
- **查找**：O(log n)  
- **删除**：O(log n)
- **遍历**：O(n)

### 空间复杂度
- O(n)，比vector和array大

## 与 unordered_map 的对比

| 特性 | map | unordered_map |
|------|-----|---------------|
| 底层实现 | 红黑树 | 哈希表 |
| 元素顺序 | 有序 | 无序 |
| 查找速度 | O(log n) | O(1)平均 |
| 内存使用 | 较少 | 较多 |
| 键类型要求 | 需要<运算符 | 需要哈希函数 |

## 总结

`map` 是**键值对字典**，核心特点：
- 快速查找（O(log n)）
- 自动按键排序
- 键唯一
- 支持各种类型作为键

**适用场景：**
- 需要按键快速查找
- 需要有序遍历
- 键值对关系数据

**记忆口诀：**
> "map像字典，键找值方便，自动排好序，查找很快速"
