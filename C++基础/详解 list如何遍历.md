# list 遍历访问详解

`list` 是 C++ STL 中的双向链表容器，提供了多种遍历方式。下面详细介绍各种遍历方法。

## 基本遍历方法

### 1. 范围 for 循环（推荐）
```cpp
#include <iostream>
#include <list>

using namespace std;

int main() {
    list<int> myList = {1, 2, 3, 4, 5};
    
    // 只读遍历
    cout << "元素: ";
    for (const auto& num : myList) {
        cout << num << " ";
    }
    cout << endl;
    
    // 可修改遍历
    list<int> numbers = {10, 20, 30};
    cout << "修改前: ";
    for (auto& num : numbers) {
        cout << num << " ";
        num *= 2; // 修改元素
    }
    cout << endl;
    
    cout << "修改后: ";
    for (const auto& num : numbers) {
        cout << num << " ";
    }
    cout << endl;
    
    return 0;
}
```

### 2. 迭代器遍历
```cpp
#include <iostream>
#include <list>

using namespace std;

int main() {
    list<string> fruits = {"apple", "banana", "orange", "grape"};
    
    // 正向迭代器
    cout << "正向遍历: ";
    for (auto it = fruits.begin(); it != fruits.end(); ++it) {
        cout << *it << " ";
    }
    cout << endl;
    
    // 常量迭代器（不能修改元素）
    cout << "常量迭代器: ";
    for (auto it = fruits.cbegin(); it != fruits.cend(); ++it) {
        cout << *it << " ";
        // *it = "new"; // 错误！不能修改
    }
    cout << endl;
    
    return 0;
}
```

### 3. 反向迭代器
```cpp
#include <iostream>
#include <list>

using namespace std;

int main() {
    list<int> numbers = {1, 2, 3, 4, 5};
    
    // 反向遍历
    cout << "反向遍历: ";
    for (auto it = numbers.rbegin(); it != numbers.rend(); ++it) {
        cout << *it << " ";
    }
    cout << endl;
    
    return 0;
}
```

## 高级遍历技巧

### 4. 使用 auto 关键字（C++11+）
```cpp
#include <iostream>
#include <list>

using namespace std;

int main() {
    list<double> prices = {19.99, 29.99, 9.99, 49.99};
    
    // 使用 auto 简化迭代器声明
    cout << "价格: ";
    for (auto it = prices.begin(); it != prices.end(); ++it) {
        cout << "$" << *it << " ";
    }
    cout << endl;
    
    return 0;
}
```

### 5. 基于索引的访问（不推荐，因为 list 不支持随机访问）
```cpp
#include <iostream>
#include <list>

using namespace std;

int main() {
    list<int> data = {10, 20, 30, 40, 50};
    
    // 注意：list 不支持 data[i] 这样的随机访问！
    // 下面的代码是错误的：
    // for (int i = 0; i < data.size(); i++) {
    //     cout << data[i] << " "; // 错误！
    // }
    
    // 正确的方式：使用迭代器
    cout << "正确遍历: ";
    int index = 0;
    for (auto it = data.begin(); it != data.end(); ++it, ++index) {
        cout << "data[" << index << "] = " << *it << " ";
    }
    cout << endl;
    
    return 0;
}
```

## 实际应用示例

### 示例1：遍历并处理元素
```cpp
#include <iostream>
#include <list>
#include <string>

using namespace std;

int main() {
    list<string> names = {"Alice", "Bob", "Charlie", "Diana"};
    
    // 遍历并处理每个元素
    cout << "姓名列表:" << endl;
    int count = 1;
    for (const auto& name : names) {
        cout << count << ". " << name << endl;
        count++;
    }
    
    // 修改特定条件的元素
    for (auto& name : names) {
        if (name.length() > 5) {
            name = "LongName";
        }
    }
    
    cout << "\n修改后:" << endl;
    for (const auto& name : names) {
        cout << name << " ";
    }
    cout << endl;
    
    return 0;
}
```

### 示例2：查找特定元素
```cpp
#include <iostream>
#include <list>
#include <algorithm>

using namespace std;

int main() {
    list<int> numbers = {15, 23, 7, 42, 18, 56, 31};
    int target = 42;
    
    // 使用迭代器查找
    auto it = find(numbers.begin(), numbers.end(), target);
    
    if (it != numbers.end()) {
        cout << "找到 " << target << " 在列表中" << endl;
        
        // 从找到的位置继续遍历
        cout << "从找到位置开始的剩余元素: ";
        for (; it != numbers.end(); ++it) {
            cout << *it << " ";
        }
        cout << endl;
    } else {
        cout << target << " 不在列表中" << endl;
    }
    
    return 0;
}
```

### 示例3：条件遍历
```cpp
#include <iostream>
#include <list>

using namespace std;

int main() {
    list<int> scores = {85, 92, 78, 96, 88, 72, 95, 60};
    
    // 只遍历及格分数（>= 60）
    cout << "及格分数: ";
    for (const auto& score : scores) {
        if (score >= 60) {
            cout << score << " ";
        }
    }
    cout << endl;
    
    // 使用迭代器进行条件处理
    cout << "优秀分数(>=90): ";
    for (auto it = scores.begin(); it != scores.end(); ++it) {
        if (*it >= 90) {
            cout << *it << " ";
            *it = 100; // 将优秀分数改为满分
        }
    }
    cout << endl;
    
    cout << "修改后的所有分数: ";
    for (const auto& score : scores) {
        cout << score << " ";
    }
    cout << endl;
    
    return 0;
}
```

## 性能考虑和最佳实践

### 遍历性能
```cpp
#include <iostream>
#include <list>
#include <vector>
#include <chrono>

using namespace std;
using namespace std::chrono;

int main() {
    // 创建大数据集
    list<int> bigList;
    vector<int> bigVector;
    
    for (int i = 0; i < 1000000; ++i) {
        bigList.push_back(i);
        bigVector.push_back(i);
    }
    
    // 测试 list 遍历性能
    auto start = high_resolution_clock::now();
    
    long long sum1 = 0;
    for (const auto& num : bigList) {
        sum1 += num;
    }
    
    auto end = high_resolution_clock::now();
    auto duration1 = duration_cast<milliseconds>(end - start);
    cout << "List 遍历时间: " << duration1.count() << "ms" << endl;
    
    // 测试 vector 遍历性能
    start = high_resolution_clock::now();
    
    long long sum2 = 0;
    for (const auto& num : bigVector) {
        sum2 += num;
    }
    
    end = high_resolution_clock::now();
    auto duration2 = duration_cast<milliseconds>(end - start);
    cout << "Vector 遍历时间: " << duration2.count() << "ms" << endl;
    
    return 0;
}
```

### 遍历时的修改操作
```cpp
#include <iostream>
#include <list>

using namespace std;

int main() {
    list<int> data = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    
    // 安全的元素删除（使用迭代器）
    cout << "删除前的数据: ";
    for (const auto& num : data) {
        cout << num << " ";
    }
    cout << endl;
    
    // 删除所有偶数
    for (auto it = data.begin(); it != data.end(); ) {
        if (*it % 2 == 0) {
            it = data.erase(it); // erase 返回下一个有效迭代器
        } else {
            ++it;
        }
    }
    
    cout << "删除偶数后的数据: ";
    for (const auto& num : data) {
        cout << num << " ";
    }
    cout << endl;
    
    return 0;
}
```

## 遍历方法总结

| 方法 | 语法 | 特点 | 适用场景 |
|------|------|------|----------|
| **范围 for 循环** | `for (const auto& x : list)` | 简洁、易读、安全 | 大多数情况，推荐使用 |
| **正向迭代器** | `for (auto it = begin(); it != end(); ++it)` | 灵活，可修改元素 | 需要修改元素或复杂逻辑 |
| **常量迭代器** | `for (auto it = cbegin(); it != cend(); ++it)` | 只读，安全 | 不需要修改元素的遍历 |
| **反向迭代器** | `for (auto it = rbegin(); it != rend(); ++it)` | 反向遍历 | 需要从后向前遍历 |

## 重要注意事项

1. **不要使用随机访问**：`list` 不支持 `[]` 运算符，必须使用迭代器
2. **迭代器失效**：在修改链表结构（插入、删除）时，相关迭代器可能失效
3. **性能考虑**：对于单纯遍历，`vector` 通常比 `list` 更快
4. **内存局部性**：`list` 元素在内存中不连续，缓存性能较差

## 最佳实践建议

1. **优先使用范围 for 循环**：代码简洁，不易出错
2. **需要修改时使用迭代器**：可以精确控制修改位置
3. **避免在遍历中修改结构**：如需删除，使用 `it = list.erase(it)` 模式
4. **考虑使用 const 引用**：`for (const auto& elem : list)` 避免不必要的拷贝

掌握这些遍历方法，你就能高效地使用 `list` 容器了！