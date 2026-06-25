## set 和 unordered_set 的区别

### 1. set（有序集合）- **会自动排序**
[[变式对比 使用vector实现对字符串智能处理和长度排序]]

```cpp
#include <iostream>
#include <set>
#include <string>

using namespace std;

int main() {
    set<string> ordered_set = {"banana", "apple", "cherry", "date"};
    
    cout << "set（有序）中的元素: ";
    for (const auto& fruit : ordered_set) {
        cout << fruit << " ";
    }
    cout << endl;
    
    return 0;
}
```

**输出：**
```
set（有序）中的元素: apple banana cherry date
```

**特点：**
- 元素**自动按字典序排序**
- 基于红黑树实现
- 插入/查找时间复杂度：O(log n)
- 元素是有序的

### 2. unordered_set（无序集合）- **不会自动排序**

```cpp
#include <iostream>
#include <unordered_set>
#include <string>

using namespace std;

int main() {
    unordered_set<string> unordered_set = {"banana", "apple", "cherry", "date"};
    
    cout << "unordered_set（无序）中的元素: ";
    for (const auto& fruit : unordered_set) {
        cout << fruit << " ";
    }
    cout << endl;
    
    return 0;
}
```

**可能的输出：**
```
unordered_set（无序）中的元素: date cherry apple banana
```
*注意：输出顺序是不确定的，每次运行可能不同*

**特点：**
- 元素**不保证顺序**
- 基于哈希表实现
- 插入/查找平均时间复杂度：O(1)
- 元素是无序的

---

## 更详细的字符串排序示例

### 示例1：基本字符串排序
```cpp
#include <iostream>
#include <set>
#include <unordered_set>
#include <vector>

using namespace std;

int main() {
    vector<string> words = {"zebra", "apple", "banana", "cat", "apple", "banana"};
    
    set<string> ordered_set(words.begin(), words.end());
    unordered_set<string> unordered_set(words.begin(), words.end());
    
    cout << "原始数据: ";
    for (const auto& word : words) cout << word << " ";
    cout << endl;
    
    cout << "set（有序）: ";
    for (const auto& word : ordered_set) cout << word << " ";
    cout << endl;
    
    cout << "unordered_set（无序）: ";
    for (const auto& word : unordered_set) cout << word << " ";
    cout << endl;
    
    return 0;
}
```

**输出：**
```
原始数据: zebra apple banana cat apple banana 
set（有序）: apple banana cat zebra 
unordered_set（无序）: cat zebra banana apple 
```

### 示例2：包含大小写的字符串排序
```cpp
#include <iostream>
#include <set>
#include <string>

using namespace std;

int main() {
    set<string> case_sensitive = {"Apple", "apple", "Banana", "banana", "Cat"};
    
    cout << "区分大小写的排序: ";
    for (const auto& word : case_sensitive) {
        cout << word << " ";
    }
    cout << endl;
    
    return 0;
}
```

**输出：**
```
区分大小写的排序: Apple Banana Cat apple banana
```
*注意：大写字母在ASCII码中排在小写字母前面*

---

## 自定义排序规则

你还可以为 `set` 提供自定义的比较函数来改变排序规则：

### 示例3：按字符串长度排序
```cpp
#include <iostream>
#include <set>
#include <string>

using namespace std;

// 自定义比较函数：按长度排序，长度相同则按字典序
struct LengthCompare {
    bool operator()(const string& a, const string& b) const {
        if (a.length() == b.length()) {
            return a < b; // 长度相同时按字典序
        }
        return a.length() < b.length(); // 按长度升序
    }
};

int main() {
    set<string, LengthCompare> length_ordered_set = {
        "apple", "banana", "cat", "dog", "elephant", "fox"
    };
    
    cout << "按长度排序: ";
    for (const auto& word : length_ordered_set) {
        cout << word << "(" << word.length() << ") ";
    }
    cout << endl;
    
    return 0;
}
```

**输出：**
```
按长度排序: cat(3) dog(3) fox(3) apple(5) banana(6) elephant(7)
```
[[详解 按字符串长度排序代码详解]]
### 示例4：不区分大小写排序
```cpp
#include <iostream>
#include <set>
#include <string>
#include <algorithm>

using namespace std;

// 不区分大小写的比较
struct CaseInsensitiveCompare {
    bool operator()(const string& a, const string& b) const {
        string a_lower = a;
        string b_lower = b;
        transform(a.begin(), a.end(), a_lower.begin(), ::tolower);
        transform(b.begin(), b.end(), b_lower.begin(), ::tolower);
        return a_lower < b_lower;
    }
};

int main() {
    set<string, CaseInsensitiveCompare> case_insensitive_set = {
        "Apple", "banana", "apple", "Banana", "CAT", "cat"
    };
    
    cout << "不区分大小写排序: ";
    for (const auto& word : case_insensitive_set) {
        cout << word << " ";
    }
    cout << endl;
    
    return 0;
}
```

**输出：**
```
不区分大小写排序: Apple banana CAT
```

---

## 总结与选择建议

| 特性 | `set` | `unordered_set` |
|------|-------|-----------------|
| **排序** | ✅ **自动排序**（字典序） | ❌ 不排序 |
| **实现** | 红黑树 | 哈希表 |
| **时间复杂度** | O(log n) | 平均 O(1) |
| **内存使用** | 较少 | 较多（哈希表开销） |
| **使用场景** | 需要有序数据、范围查询 | 只需要快速查找、不关心顺序 |

### 如何选择？

- **需要自动排序** → 用 `set`
- **只需要快速查找，不关心顺序** → 用 `unordered_set`
- **数据量很大，追求极致性能** → 优先考虑 `unordered_set`
- **需要范围查询（如找所有在 "apple" 和 "cherry" 之间的元素）** → 用 `set`

在算法竞赛中，如果题目不要求顺序，通常推荐使用 `unordered_set`，因为它更快。但如果需要有序输出或者进行范围操作，就必须使用 `set`。