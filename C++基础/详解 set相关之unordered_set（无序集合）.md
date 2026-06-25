# unordered_set 详解

`unordered_set` 是 C++ STL 中的一个关联容器，它基于哈希表实现，提供了快速的查找、插入和删除操作。

## 基本特性

### 核心特点
- **无序存储**：元素不按特定顺序存储
- **唯一性**：容器内所有元素都是唯一的
- **基于哈希表**：使用哈希函数实现快速访问
- **平均 O(1) 时间复杂度**：查找、插入、删除操作的平均时间复杂度

### 头文件
```cpp
#include <unordered_set>
```

## 基本用法

### 初始化
```cpp
#include <iostream>
#include <unordered_set>
#include <vector>

using namespace std;

int main() {
    // 各种初始化方式
    unordered_set<int> us1;                    // 空集合
    unordered_set<int> us2 = {1, 2, 3, 4};     // 初始化列表
    unordered_set<int> us3(us2);               // 拷贝构造
    
    vector<int> vec = {5, 6, 7, 8};
    unordered_set<int> us4(vec.begin(), vec.end()); // 迭代器范围构造
    
    return 0;
}
```

## 常用操作详解

### 1. 插入元素
```cpp
unordered_set<string> fruits;

// 方法1: insert()
fruits.insert("apple");
fruits.insert("banana");

// 方法2: emplace() - 更高效，原地构造
fruits.emplace("orange");
fruits.emplace("grape");

// 插入多个元素
fruits.insert({"cherry", "mango"});

// 检查插入结果
auto result = fruits.insert("apple"); // 重复插入
cout << "插入成功: " << result.second << endl; // 输出 0 (false)
cout << "元素: " << *(result.first) << endl;   // 输出 apple
```

### 2. 查找元素
```cpp
unordered_set<int> numbers = {10, 20, 30, 40, 50};

// 方法1: find() - 返回迭代器
auto it = numbers.find(30);
if (it != numbers.end()) {
    cout << "找到: " << *it << endl; // 输出 30
} else {
    cout << "未找到" << endl;
}

// 方法2: count() - 返回元素个数 (0 或 1)
if (numbers.count(25)) {
    cout << "25 存在" << endl;
} else {
    cout << "25 不存在" << endl; // 输出这个
}

// 方法3: contains() - C++20 (返回 bool)
// if (numbers.contains(40)) {
//     cout << "40 存在" << endl;
// }
```

### 3. 删除元素
```cpp
unordered_set<string> words = {"hello", "world", "cpp", "stl", "unordered_set"};

// 方法1: erase(值) - 返回删除的元素个数
size_t count = words.erase("cpp");
cout << "删除了 " << count << " 个元素" << endl;

// 方法2: erase(迭代器)
auto it = words.find("stl");
if (it != words.end()) {
    words.erase(it);
}

// 方法3: erase(迭代器范围)
words.erase(words.begin(), next(words.begin(), 2));

// 方法4: clear() - 清空所有
// words.clear();
```

### 4. 遍历元素
```cpp
unordered_set<int> nums = {1, 3, 5, 7, 9, 2, 4, 6, 8};

// 方法1: 范围for循环 (推荐)
cout << "所有元素: ";
for (const auto& num : nums) {
    cout << num << " ";
}
cout << endl;

// 方法2: 迭代器
cout << "使用迭代器: ";
for (auto it = nums.begin(); it != nums.end(); ++it) {
    cout << *it << " ";
}
cout << endl;
```

## 容量相关操作

```cpp
unordered_set<double> values = {1.1, 2.2, 3.3, 4.4};

cout << "大小: " << values.size() << endl;
cout << "是否为空: " << values.empty() << endl;

// 预留空间以提高性能
values.reserve(100); // 预留100个元素的空间
cout << "桶数量: " << values.bucket_count() << endl;

// 重新哈希，调整桶的数量
values.rehash(50);
```

## 哈希相关操作

```cpp
unordered_set<string> colors = {"red", "green", "blue", "yellow"};

// 查看哈希相关信息
cout << "桶数量: " << colors.bucket_count() << endl;
cout << "最大桶数量: " << colors.max_bucket_count() << endl;
cout << "负载因子: " << colors.load_factor() << endl;
cout << "最大负载因子: " << colors.max_load_factor() << endl;

// 遍历所有桶
for (size_t i = 0; i < colors.bucket_count(); ++i) {
    cout << "桶 " << i << " 有 " << colors.bucket_size(i) << " 个元素" << endl;
}

// 查找元素的桶索引
string color = "red";
cout << color << " 在桶 " << colors.bucket(color) << endl;
```

## 实际应用示例

### 示例1：查找重复元素
```cpp
#include <iostream>
#include <unordered_set>
#include <vector>

using namespace std;

vector<int> findDuplicates(const vector<int>& nums) {
    unordered_set<int> seen;
    vector<int> duplicates;
    
    for (int num : nums) {
        if (seen.count(num)) {
            duplicates.push_back(num);
        } else {
            seen.insert(num);
        }
    }
    
    return duplicates;
}

int main() {
    vector<int> numbers = {1, 2, 3, 2, 1, 4, 5, 4};
    vector<int> duplicates = findDuplicates(numbers);
    
    cout << "重复元素: ";
    for (int num : duplicates) {
        cout << num << " ";
    }
    cout << endl;
    
    return 0;
}
```

### 示例2：两个数组的交集
```cpp
#include <iostream>
#include <unordered_set>
#include <vector>

using namespace std;

vector<int> intersection(const vector<int>& nums1, const vector<int>& nums2) {
    unordered_set<int> set1(nums1.begin(), nums1.end());
    unordered_set<int> result;
    
    for (int num : nums2) {
        if (set1.count(num)) {
            result.insert(num);
        }
    }
    
    return vector<int>(result.begin(), result.end());
}

int main() {
    vector<int> nums1 = {1, 2, 2, 1};
    vector<int> nums2 = {2, 2};
    
    vector<int> result = intersection(nums1, nums2);
    
    cout << "交集: ";
    for (int num : result) {
        cout << num << " ";
    }
    cout << endl;
    
    return 0;
}
```

### 示例3：字符串唯一字符检查
```cpp
#include <iostream>
#include <unordered_set>
#include <string>

using namespace std;

bool hasAllUniqueChars(const string& str) {
    unordered_set<char> charSet;
    
    for (char c : str) {
        if (charSet.count(c)) {
            return false;
        }
        charSet.insert(c);
    }
    
    return true;
}

int main() {
    string test1 = "abcdefg";
    string test2 = "hello";
    
    cout << test1 << " 所有字符唯一: " << hasAllUniqueChars(test1) << endl;
    cout << test2 << " 所有字符唯一: " << hasAllUniqueChars(test2) << endl;
    
    return 0;
}
```

## 性能优化技巧

### 1. 预分配空间
```cpp
// 如果知道大概的元素数量，预先分配空间可以提高性能
unordered_set<int> bigSet;
bigSet.reserve(10000); // 预分配空间

for (int i = 0; i < 10000; ++i) {
    bigSet.insert(i);
}
```

### 2. 调整负载因子
```cpp
unordered_set<int> mySet;

// 设置最大负载因子
mySet.max_load_factor(0.5); // 当负载因子超过0.5时重新哈希

// 强制重新哈希
mySet.rehash(1000);
```

## 与 set 的比较

| 特性 | unordered_set | set |
|------|---------------|-----|
| **底层实现** | 哈希表 | 红黑树 |
| **元素顺序** | 无序 | 有序 |
| **查找性能** | 平均 O(1)，最坏 O(n) | O(log n) |
| **插入性能** | 平均 O(1)，最坏 O(n) | O(log n) |
| **内存占用** | 通常更多 | 通常较少 |
| **使用场景** | 需要快速查找，不关心顺序 | 需要有序数据或范围查询 |

## 使用场景总结

使用 `unordered_set` 当：
- 需要快速查找元素是否存在
- 不关心元素的顺序
- 需要保证元素的唯一性
- 频繁进行插入和删除操作

不使用 `unordered_set` 当：
- 需要元素按顺序存储
- 需要进行范围查询
- 内存非常紧张
- 需要稳定的性能（避免最坏情况的 O(n)）

## 注意事项

1. **哈希冲突**：在最坏情况下，所有元素可能哈希到同一个桶，导致性能下降
2. **迭代器失效**：在重新哈希后，所有迭代器都会失效
3. **自定义类型**：如果存储自定义类型，需要提供哈希函数和相等比较函数
4. **内存开销**：哈希表通常比平衡二叉树占用更多内存