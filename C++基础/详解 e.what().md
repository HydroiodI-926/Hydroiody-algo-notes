# `e.what()` 小笔记

## 📌 基本概念
- `e.what()` 是**异常对象的方法**
- 返回**错误描述信息**（字符串）
- 所有标准异常类都有这个方法

## 🎯 语法格式
```cpp
const char* error_message = e.what();
```

## 📝 使用示例
```cpp
try {
    vector<int> v = {1, 2, 3};
    cout << v.at(10);  // 越界访问
} catch (const out_of_range& e) {
    cout << "错误: " << e.what() << endl;
}
```

**输出示例：**
```
错误: vector::_M_range_check: __n (which is 10) >= this->size() (which is 3)
```

## 🔧 主要用途
1. **调试信息** - 显示具体错误原因
2. **日志记录** - 保存错误详情
3. **用户提示** - 友好的错误消息

## ⚠️ 注意事项
- 返回 `const char*`，**不能修改**
- 只在 `catch` 块内使用是安全的
- 不要长期保存返回的指针

## 🎨 自定义异常
```cpp
class MyException : public exception {
    string msg;
public:
    MyException(const string& s) : msg(s) {}
    const char* what() const noexcept override {
        return msg.c_str();
    }
};
```

## 📊 常见异常类型
| 异常类型 | 含义 | `.what()` 内容 |
|---------|------|---------------|
| `out_of_range` | 下标越界 | 越界的具体信息 |
| `invalid_argument` | 无效参数 | 参数错误描述 |
| `runtime_error` | 运行时错误 | 自定义错误消息 |

## 💡 记忆口诀
> "异常对象点what，错误信息告诉你"

## 🚀 快速参考
```cpp
// 标准用法
catch (const exception& e) {
    cout << e.what() << endl;
}

// 获取并保存
const char* msg = e.what();
string error_str = "问题: " + string(msg);
```

---

**一句话总结：** `e.what()` 就是异常的"说话"方法，告诉你到底出了什么问题！