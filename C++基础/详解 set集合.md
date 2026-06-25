## 基本介绍
`set` 是 C++ 中的**集合容器**，它存储**唯一**的元素，并且**自动排序**。
[[详解 set相关之unordered_set（无序集合）]]
[[详解 set对于输入字符串的处理]]

## 头文件
```cpp
#include <set>
using namespace std;
```

## 声明和初始化
```cpp
// 各种创建方式
set<int> s1;                    // 空set
set<int> s2 = {1, 2, 3, 4, 5};  // 直接初始化
set<int> s3(s2);                // 拷贝构造
set<int> s4(s2.begin(), s2.end()); // 通过迭代器范围初始化

// 自定义排序规则的set
set<int, greater<int>> s5;      // 降序排列
```

## 核心特性

### 优点：
- ✅ **自动去重**：每个元素只出现一次
- ✅ **自动排序**：默认升序排列
- ✅ **快速查找**：基于红黑树，查找效率 O(log n)
- ✅ **元素唯一**：无需手动检查重复

### 缺点：
- ❌ **不能直接修改元素**：因为修改可能破坏顺序
- ❌ **内存开销**：比vector和array大
- ❌ **插入删除稍慢**：需要维护树结构

## 常用操作详解

### 插入元素
```cpp
set<int> s;

// 方法1：使用insert
s.insert(3);
s.insert(1);
s.insert(4);
s.insert(1);  // 重复元素，不会被插入

// 方法2：使用emplace（C++11，效率高）
s.emplace(2);

// 方法3：插入多个元素
s.insert({5, 6, 7});

// 查看结果：自动排序且去重
for (int x : s) {
    cout << x << " ";  // 输出：1 2 3 4 5 6 7
}
```

### 删除元素
```cpp
set<int> s = {1, 2, 3, 4, 5, 6, 7};

// 方法1：通过值删除
s.erase(3);                    // 删除元素3

// 方法2：通过迭代器删除
auto it = s.find(4);
if (it != s.end()) {
    s.erase(it);               // 删除找到的元素
}

// 方法3：删除多个元素（通过迭代器范围）
s.erase(s.begin(), s.find(3)); // 删除从开始到3之前的元素

// 方法4：清空所有元素
s.clear();                     // 清空set
```

### 查找元素
```cpp
set<int> s = {1, 2, 3, 4, 5};

// 方法1：使用find（返回迭代器）
auto it = s.find(3);
if (it != s.end()) {
    cout << "找到元素: " << *it << endl;
} else {
    cout << "未找到元素" << endl;
}

// 方法2：使用count（返回0或1）
if (s.count(3) > 0) {
    cout << "元素存在" << endl;
} else {
    cout << "元素不存在" << endl;
}

// 方法3：使用contains（C++20）
// if (s.contains(3)) { ... }
```

### 遍历方法
```cpp
set<int> s = {3, 1, 4, 1, 5, 9, 2, 6};

// 方法1：范围for循环（推荐）
for (int value : s) {
    cout << value << " ";  // 输出：1 2 3 4 5 6 9（自动排序去重）
}

// 方法2：迭代器遍历
for (auto it = s.begin(); it != s.end(); it++) {
    cout << *it << " ";
}

// 方法3：反向遍历
for (auto it = s.rbegin(); it != s.rend(); it++) {
    cout << *it << " ";  // 逆序输出：9 6 5 4 3 2 1
}
```

## 容量和信息
```cpp
set<int> s = {1, 2, 3};

cout << s.size() << endl;      // 3（元素个数）
cout << s.empty() << endl;     // false（是否为空）

// 获取边界
auto first = s.begin();        // 指向最小元素的迭代器
auto last = s.end();           // 末尾迭代器
```

## 实际应用示例

### 示例1：去重排序
```cpp
#include <iostream>
#include <set>
#include <vector>
using namespace std;

int main() {
    vector<int> numbers = {5, 2, 8, 2, 5, 1, 8, 9, 3, 2};
    set<int> unique_sorted(numbers.begin(), numbers.end());
    
    cout << "去重排序后的结果: ";
    for (int num : unique_sorted) {
        cout << num << " ";  // 输出：1 2 3 5 8 9
    }
    cout << endl;
    
    return 0;
}
```

### 示例2：单词去重
```cpp
#include <iostream>
#include <set>
#include <string>
using namespace std;

int main() {
    set<string> words;
    string word;
    
    cout << "请输入单词（输入end结束）:" << endl;
    while (cin >> word && word != "end") {
        words.insert(word);
    }
    
    cout << "\n去重后的单词（按字典序）:" << endl;
    for (const string& w : words) {
        cout << w << endl;
    }
    
    return 0;
}
```

### 示例3：存在性检查
```cpp
#include <iostream>
#include <set>
using namespace std;

int main() {
    set<int> valid_ids = {1001, 1002, 1003, 1004, 1005};
    int user_id;
    
    cout << "请输入用户ID: ";
    cin >> user_id;
    
    if (valid_ids.find(user_id) != valid_ids.end()) {
        cout << "ID有效，允许访问" << endl;
    } else {
        cout << "ID无效，拒绝访问" << endl;
    }
    
    return 0;
}
```

## 在算法竞赛中的应用

### 应用1：统计不同数字的个数
```cpp
#include <iostream>
#include <set>
using namespace std;

int main() {
    int n;
    cin >> n;
    
    set<int> distinct;
    for (int i = 0; i < n; i++) {
        int num;
        cin >> num;
        distinct.insert(num);
    }
    
    cout << "不同数字的个数: " << distinct.size() << endl;
    return 0;
}
```

### 应用2：维护有序唯一序列
```cpp
// 动态维护一个有序且无重复的序列
#include <iostream>
#include <set>
using namespace std;

int main() {
    set<int> s;
    char op;
    int num;
    
    while (cin >> op >> num) {
        if (op == 'I') {
            s.insert(num);  // 插入元素
        } else if (op == 'D') {
            s.erase(num);   // 删除元素
        } else if (op == 'Q') {
            if (s.find(num) != s.end()) {
                cout << "存在" << endl;
            } else {
                cout << "不存在" << endl;
            }
        } else if (op == 'P') {
            // 打印当前所有元素
            for (int x : s) {
                cout << x << " ";
            }
            cout << endl;
        }
    }
    
    return 0;
}
```

### 应用3：寻找边界值
```cpp
#include <iostream>
#include <set>
using namespace std;

int main() {
    set<int> s = {10, 20, 30, 40, 50};
    
    // 查找第一个大于等于25的元素
    auto it_lower = s.lower_bound(25);
    if (it_lower != s.end()) {
        cout << "第一个大于等于25的元素: " << *it_lower << endl;  // 30
    }
    
    // 查找第一个大于35的元素
    auto it_upper = s.upper_bound(35);
    if (it_upper != s.end()) {
        cout << "第一个大于35的元素: " << *it_upper << endl;  // 40
    }
    
    return 0;
}
```

## 特殊用法和技巧

### 自定义排序规则
```cpp
#include <iostream>
#include <set>
#include <string>
using namespace std;

// 按字符串长度排序
struct CompareLength {
    bool operator()(const string& a, const string& b) const {
        return a.length() < b.length();
    }
};

int main() {
    set<string, CompareLength> words = {
        "apple", "banana", "cat", "dog", "elephant"
    };
    
    for (const string& w : words) {
        cout << w << " ";  // 按长度输出：cat dog apple banana elephant
    }
    cout << endl;
    
    return 0;
}
```

### 使用pair作为元素
```cpp
#include <iostream>
#include <set>
using namespace std;

int main() {
    set<pair<int, int>> points;
    
    points.insert({1, 2});
    points.insert({3, 4});
    points.insert({1, 2});  // 重复，不会被插入
    points.insert({2, 1});
    
    for (const auto& p : points) {
        cout << "(" << p.first << ", " << p.second << ") ";
    }
    // 输出：(1, 2) (2, 1) (3, 4) （按pair的默认排序：先first后second）
    cout << endl;
    
    return 0;
}
```

## 性能注意事项

### 时间复杂度
- **插入**：O(log n)
- **查找**：O(log n)  
- **删除**：O(log n)
- **遍历**：O(n)

### 空间复杂度
- O(n)，比vector和array大

## 与 unordered_set 的对比

| 特性 | set | unordered_set |
|------|-----|---------------|
| 底层实现 | 红黑树 | 哈希表 |
| 元素顺序 | 有序 | 无序 |
| 查找速度 | O(log n) | O(1)平均 |
| 内存使用 | 较少 | 较多 |
| 元素类型要求 | 需要<运算符 | 需要哈希函数 |

## 总结

`set` 是**有序唯一集合**，核心特点：
- 自动去重
- 自动排序
- 快速查找
- 元素不可直接修改

**适用场景：**
- 需要去重且排序的数据
- 需要快速查找存在性
- 需要维护有序唯一序列

**记忆口诀：**
> "set集合，元素唯一，自动排序，查找快速"