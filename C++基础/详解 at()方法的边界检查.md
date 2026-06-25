## 超出下标会发生什么？

```cpp
vector<int> v = {10, 20, 30};

// 使用 at() 方法
cout << v.at(3);  // ❌ 错误！下标超出范围
```

## 实际运行结果

### 情况1：程序崩溃并报错
```
terminate called after throwing an instance of 'std::out_of_range'
  what():  vector::_M_range_check: __n (which is 3) >= this->size() (which is 3)
Aborted (core dumped)
```

### 情况2：完整示例演示
```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> v = {10, 20, 30};
    
    try {
        cout << "尝试访问 v.at(3)..." << endl;
        cout << v.at(3) << endl;  // 这里会抛出异常
    } catch (const out_of_range& e) {
        cout << "捕获到异常: " << e.what() << endl;
        cout << "程序继续正常运行..." << endl;
    }
    
    cout << "程序结束" << endl;
    return 0;
}
```
[[详解 try-catch异常处理机制]]

**输出：**
```
尝试访问 v.at(3)...
捕获到异常: vector::_M_range_check: __n (which is 3) >= this->size() (which is 3)
程序继续正常运行...
程序结束
```

## 对比 `[]` 和 `at()` 的区别

```cpp
vector<int> v = {10, 20, 30};

// 使用 [] 运算符（不检查边界）
cout << v[3];  // ❌ 未定义行为！可能输出垃圾值或程序崩溃

// 使用 at() 方法（检查边界）  
cout << v.at(3); // ❌ 明确抛出异常，可以捕获处理
```

## 边界检查的详细说明

### `at()` 的安全机制：
1. **检查下标是否有效**：`if (index >= size())`
2. **如果无效**：抛出 `std::out_of_range` 异常
3. **如果有效**：返回对应元素

### `[]` 的不安全机制：
1. **直接访问内存**：不进行任何检查
2. **如果下标有效**：返回正确元素
3. **如果下标无效**：访问非法内存，导致：
   - 输出垃圾值
   - 程序崩溃
   - 修改了其他变量
   - 其他不可预测行为

## 实际应用建议

### 什么时候用 `at()`？
```cpp
// 当不确定下标是否安全时
int getSafeElement(const vector<int>& v, int index) {
    try {
        return v.at(index);
    } catch (const out_of_range& e) {
        return -1;  // 或者返回默认值
    }
}
```

### 什么时候用 `[]`？
```cpp
// 当确定下标安全时（比如在循环中）
for(int i = 0; i < v.size(); i++) {
    cout << v[i] << " ";  // ✅ 安全，因为i在范围内
}
```

### 安全访问的模式
```cpp
vector<int> v = {10, 20, 30};
int index = 5;

// 方法1：先检查再访问
if(index >= 0 && index < v.size()) {
    cout << v[index] << endl;  // 安全访问
} else {
    cout << "下标越界!" << endl;
}

// 方法2：使用at()并捕获异常
try {
    cout << v.at(index) << endl;
} catch (const out_of_range& e) {
    cout << "下标越界: " << e.what() << endl;
}
```

## 性能考虑

- **`[]`**：更快，没有检查开销
- **`at()`**：稍慢，有边界检查开销

在算法竞赛中，如果确定下标安全，通常用 `[]` 以获得更好性能。

## 总结

- **`at()`**：安全，会检查边界，抛出异常
- **`[]`**：快速，不检查边界，访问越界是未定义行为
- **建议**：在调试时用 `at()`，在确定安全时用 `[]`