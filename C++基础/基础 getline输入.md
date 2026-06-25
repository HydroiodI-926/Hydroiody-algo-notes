## 📖 基本介绍

`getline` 是 C++ 中用于读取整行文本的函数，包含在 `<string>` 头文件中。

## 🎯 两种主要形式

### 1. 标准 getline（最常用）
```cpp
#include <string>
istream& getline(istream& is, string& str);
```

### 2. 带分隔符的 getline
```cpp
istream& getline(istream& is, string& str, char delim);
```

## 💡 基本用法

### 从控制台读取一行
```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string line;
    
    cout << "请输入一行文本: ";
    getline(cin, line);
    
    cout << "你输入的是: " << line << endl;
    cout << "长度: " << line.length() << endl;
    
    return 0;
}
```

### 读取多行直到EOF
```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string line;
    int count = 0;
    
    cout << "请输入多行文本 (Ctrl+D/Ctrl+Z 结束):" << endl;
    
    while (getline(cin, line)) {
        count++;
        cout << "第 " << count << " 行: " << line << endl;
    }
    
    cout << "总共读取了 " << count << " 行" << endl;
    return 0;
}
```

## 🔧 带分隔符的用法

### 使用自定义分隔符
```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string data;
    
    cout << "请输入数据（以分号结束）: ";
    getline(cin, data, ';');  // 读取直到遇到分号
    
    cout << "读取的数据: " << data << endl;
    
    return 0;
}
```

### 解析CSV数据
```cpp
#include <iostream>
#include <string>
#include <sstream>
using namespace std;

int main() {
    string csvLine = "Apple,1.2,10;Banana,0.8,15";
    string item;
    
    istringstream lineStream(csvLine);
    
    while (getline(lineStream, item, ';')) {
        cout << "商品组: " << item << endl;
        
        // 进一步分割每个商品组
        istringstream itemStream(item);
        string field;
        while (getline(itemStream, field, ',')) {
            cout << " 字段: " << field << endl;
        }
    }
    
    return 0;
}
```

## ⚠️ 常见问题与解决方案

### 问题1：cin 和 getline 混用
```cpp
// ❌ 错误示例
int age;
string name;

cout << "请输入年龄: ";
cin >> age;

cout << "请输入姓名: ";
getline(cin, name);  // 这里会读到换行符，直接跳过！

// ✅ 正确解决方法
int age;
string name;

cout << "请输入年龄: ";
cin >> age;

cin.ignore();  // 忽略换行符

cout << "请输入姓名: ";
getline(cin, name);  // 现在能正常读取

cout << "年龄: " << age << ", 姓名: " << name << endl;
```

### 问题2：处理空行
```cpp
string line;
while (getline(cin, line)) {
    if (line.empty()) {
        cout << "检测到空行，跳过" << endl;
        continue;
    }
    // 处理非空行
    cout << "内容: " << line << endl;
}
```

## 🛠️ 实用技巧

### 1. 读取文件内容
```cpp
#include <iostream>
#include <fstream>
#include <string>
using namespace std;

int main() {
    ifstream file("data.txt");
    string line;
    
    if (file.is_open()) {
        while (getline(file, line)) {
            cout << line << endl;
        }
        file.close();
    }
    
    return 0;
}
```

### 2. 逐行处理并统计
```cpp
#include <iostream>
#include <string>
#include <sstream>
#include <vector>
using namespace std;

int main() {
    string line;
    vector<vector<int>> allNumbers;
    
    cout << "请输入多行数字，每行多个数字:" << endl;
    
    while (getline(cin, line)) {
        if (line.empty()) break;
        
        vector<int> numbers;
        istringstream iss(line);
        int num;
        
        while (iss >> num) {
            numbers.push_back(num);
        }
        
        if (!numbers.empty()) {
            allNumbers.push_back(numbers);
        }
    }
    
    // 输出统计结果
    for (size_t i = 0; i < allNumbers.size(); i++) {
        cout << "第 " << i + 1 << " 行: ";
        for (int num : allNumbers[i]) {
            cout << num << " ";
        }
        cout << endl;
    }
    
    return 0;
}
```

### 3. 安全的输入循环
```cpp
#include <iostream>
#include <string>
#include <limits>
using namespace std;

int main() {
    string input;
    bool valid = false;
    
    do {
        cout << "请选择 (y/n): ";
        getline(cin, input);
        
        if (input == "y" || input == "Y") {
            cout << "你选择了是" << endl;
            valid = true;
        } else if (input == "n" || input == "N") {
            cout << "你选择了否" << endl;
            valid = true;
        } else {
            cout << "无效输入，请重新输入" << endl;
        }
    } while (!valid);
    
    return 0;
}
```

## 📊 getline vs cin >>

| 特性 | `getline` | `cin >>` |
|------|-----------|----------|
| **读取内容** | 整行文本（包括空格） | 单个单词（遇到空格停止） |
| **包含空格** | ✅ 是 | ❌ 否 |
| **去除换行符** | ✅ 是 | ❌ 否（留在缓冲区） |
| **常用场景** | 读取句子、文件行 | 读取单个数据 |

## 🎯 最佳实践

1. **读取文本时优先使用 getline**
2. **混用 cin >> 和 getline 时记得 `cin.ignore()`**
3. **处理文件时用 `while(getline(file, line))` 模式**
4. **检查空行避免处理无效数据**
5. **结合 stringstream 解析复杂格式**

## 💡 记住这个万能模式

```cpp
#include <iostream>
#include <string>
#include <sstream>
using namespace std;

string line;
while (getline(cin, line)) {
    if (line.empty()) continue;  // 跳过空行
    
    istringstream iss(line);     // 创建字符串流
    string word;
    while (iss >> word) {        // 分割单词
        // 处理每个单词
        cout << word << " ";
    }
    cout << endl;
}
```
[[详解 continue、istringstream、iss]]