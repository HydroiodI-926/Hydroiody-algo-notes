# C++ 输入结束判断小笔记

### 1. 最常用方法（推荐）
```cpp
int num;
while (cin >> num) {
    // 处理 num
}
// 自动检测EOF或输入错误
```

### 2. do-while 版本
```cpp
int num;
do {
    cin >> num;
    if (cin) {  // 检查输入是否成功
        // 处理 num
    }
} while (cin);  // 输入成功时继续
```

### 3. 显式EOF检测
```cpp
int num;
while (true) {
    cin >> num;
    if (cin.eof()) break;  // 检测到EOF
    // 处理 num
}
```

### 4. 综合检测（最安全）
```cpp
int num;
while (true) {
    cin >> num;
    if (cin.eof()) break;     // 文件结束
    if (cin.fail()) break;    // 输入错误
    // 处理 num
}
```
## 🔧 实用工具函数

### 安全读取数字
```cpp
bool safeRead(int& num) {
    cin >> num;
    if (cin.eof()) return false;
    if (cin.fail()) {
        cin.clear();
        cin.ignore(1000, '\n');
        return false;
    }
    return true;
}

// 使用
while (safeRead(num)) {
    // 处理 num
}
```

### 读取整个序列
```cpp
vector<int> nums;
int num;
while (cin >> num) {
    nums.push_back(num);
}
```

## 📝 使用场景

### 算法竞赛模式
```cpp
// 多组测试数据
while (cin >> a >> b) {
    cout << a + b << endl;
}

// 第一行指定数量
cin >> n;
for (int i = 0; i < n; i++) {
    cin >> data[i];
}
```

### 交互式程序
```cpp
cout << "输入数字 (Ctrl+D结束): ";
int num;
while (cin >> num) {
    cout << "收到: " << num << endl;
    cout << "继续输入: ";
}
```

## ⚠️ 重要提醒

1. **EOF快捷键**：
   - Windows: `Ctrl + Z` + `Enter`
   - Linux/Mac: `Ctrl + D`

2. **输入错误处理**：
   ```cpp
   if (cin.fail()) {
       cin.clear();                    // 清除错误状态
       cin.ignore(1000, '\n');        // 清空缓冲区
   }
   ```

3. **性能优化**（算法竞赛）：
   ```cpp
   ios::sync_with_stdio(false);
   cin.tie(nullptr);
   ```

## 🎯 核心要点

- **用 `while(cin >> var)`** 而不是 `while(cin >> EOF)`
- **cin本身可作为布尔条件**，成功为true，失败为false
- **EOF是检测出来的**，不是读取出来的
- **do-while要先读后判断**，注意第一次可能就遇到EOF

记住这些模式，就能正确处理各种输入结束的情况！