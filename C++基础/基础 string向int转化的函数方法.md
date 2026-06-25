# 字符串转整数简单概括

## 🎯 **推荐方法：`std::stoi`**
```cpp
#include <string>
string str = "123";
int num = stoi(str);  // 最简单、最安全
```

## 🔧 **其他方法：**
- **`std::istringstream`** - 灵活，适合复杂格式
- **`std::from_chars`** - C++17，性能最好  
- **`atoi`** - C风格，不推荐（无法检测错误）

## ⚠️ **重要提醒：**
```cpp
// 一定要错误处理！
try {
    int num = stoi(str);
} catch (...) {
    // 处理转换失败
}
```

## 💡 **一句话总结：**
**用 `stoi()`，加 try-catch，搞定！**