## 函数原型
```c++
template <class BidirectionalIterator>
bool next_permutation(BidirectionalIterator first, BidirectionalIterator last);

// 带比较函数的版本
template <class BidirectionalIterator, class Compare>
bool next_permutation(BidirectionalIterator first, BidirectionalIterator last, Compare comp);
```
## 基本用法
### 1. 生成全排列

```cpp

#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

int main() {
    vector<int> nums = {1, 2, 3};
    
    // 生成所有排列
    do {
        for (int num : nums) {
            cout << num << " ";
        }
        cout << endl;
    } while (next_permutation(nums.begin(), nums.end()));
    
    return 0;
}

```

**输出：**

```text
1 2 3
1 3 2
2 1 3
2 3 1
3 1 2
3 2 1
```
## 重要特性

### 1. 返回值

- **true**：成功生成了下一个排列
    
- **false**：当前已经是最后一个排列，序列被重置为第一个排列
    

### 2. 字典序

按字典序生成下一个排列：

```cpp
vector<int> nums = {1, 3, 2};
next_permutation(nums.begin(), nums.end());
// nums 变为 {2, 1, 3}
```

### 3. 需要先排序

为了获得所有排列，通常需要先对序列排序：

```cpp
vector<int> nums = {3, 1, 2};
sort(nums.begin(), nums.end());  // 先排序：{1, 2, 3}

do {
    // 处理排列
} while (next_permutation(nums.begin(), nums.end()));
```
