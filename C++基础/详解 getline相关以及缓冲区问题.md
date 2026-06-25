## 基本概念

### `getline(cin, s)` 是什么？
`getline(cin, s)` 是用于**读取整行输入**的函数，包括空格，直到遇到换行符。

### 与 `cin >>` 的区别
```cpp
string s;
cin >> s;        // 读取一个单词（遇到空格停止）
getline(cin, s); // 读取整行（包括空格）
```

## 实际对比演示

### 示例1：`cin >>` 的行为
```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string name;
    cout << "请输入你的全名: ";
    cin >> name;  // 只读取第一个单词
    
    cout << "你好, " << name << "!" << endl;
    return 0;
}
```

**运行结果：**
```
请输入你的全名: 张三 丰
你好, 张三!
```
❌ 只读取了"张三"，"丰"被留在缓冲区

### 示例2：`getline(cin, s)` 的行为
```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string name;
    cout << "请输入你的全名: ";
    getline(cin, name);  // 读取整行
    
    cout << "你好, " << name << "!" << endl;
    return 0;
}
```

**运行结果：**
```
请输入你的全名: 张三 丰
你好, 张三 丰!
```
✅ 正确读取了完整的"张三 丰"

## 输入缓冲区问题详解

### 什么是输入缓冲区？
输入缓冲区是内存中的一块区域，用来临时存储用户的输入数据。

**输入过程：**
```
键盘输入 → 输入缓冲区 → 程序读取
```

### 缓冲区问题的产生

#### 问题场景：混合使用 `cin >>` 和 `getline()`
```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    int age;
    string name;
    
    cout << "请输入年龄: ";
    cin >> age;           // 读取数字
    cout << "请输入姓名: ";
    getline(cin, name);   // 读取姓名
    
    cout << "年龄: " << age << endl;
    cout << "姓名: " << name << endl;
    
    return 0;
}
```

**运行结果：**
```
请输入年龄: 25
请输入姓名: 年龄: 25
姓名: 
```
❌ **问题：** `getline()` 没有等待输入，直接读取了换行符！

### 缓冲区问题的原因

**输入过程分解：**
1. 用户输入：`25` + `回车`
2. 缓冲区内容：`"25\n"`
3. `cin >> age` 读取 `25`，留下 `"\n"` 在缓冲区
4. `getline(cin, name)` 看到 `"\n"`，立即返回空字符串

**内存示意图：**
```
输入前缓冲区: [空]
用户输入25回车: [ '2' '5' '\n' ]
cin >> age 后: [ '\n' ] ← 换行符还在！
getline读取后: [空] ← 读取了换行符，得到空字符串
```

## 解决方案

### 方法1：使用 `cin.ignore()` 清空缓冲区
```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    int age;
    string name;
    
    cout << "请输入年龄: ";
    cin >> age;
    
    // 清空缓冲区中的换行符
    cin.ignore();
    
    cout << "请输入姓名: ";
    getline(cin, name);
    
    cout << "年龄: " << age << endl;
    cout << "姓名: " << name << endl;
    
    return 0;
}
```

### 方法2：更安全的 `cin.ignore()`
```cpp
// 清空缓冲区直到换行符（包括换行符）
cin.ignore(numeric_limits<streamsize>::max(), '\n');
```

**完整示例：**
```cpp
#include <iostream>
#include <string>
#include <limits>  // 需要这个头文件
using namespace std;

int main() {
    int age;
    string name;
    
    cout << "请输入年龄: ";
    cin >> age;
    
    // 清空缓冲区中的所有字符，直到遇到换行符
    cin.ignore(numeric_limits<streamsize>::max(), '\n');
    
    cout << "请输入姓名: ";
    getline(cin, name);
    
    cout << "年龄: " << age << endl;
    cout << "姓名: " << name << endl;
    
    return 0;
}
```

## `getline()` 的详细用法

### 基本语法
```cpp
getline(输入流, 字符串变量);
getline(输入流, 字符串变量, 分隔符);  // 可指定分隔符
```

### 指定分隔符
```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string data;
    
    cout << "请输入数据（用逗号分隔）: ";
    // 读取直到遇到逗号
    getline(cin, data, ',');
    
    cout << "读取的数据: " << data << endl;
    return 0;
}
```

**运行结果：**
```
请输入数据（用逗号分隔）: 苹果,香蕉,橙子
读取的数据: 苹果
```

### 读取多行
```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string line;
    int lineCount = 0;
    
    cout << "请输入多行文本（空行结束）:" << endl;
    
    while (true) {
        getline(cin, line);
        if (line.empty()) {  // 空行结束
            break;
        }
        lineCount++;
        cout << "第" << lineCount << "行: " << line << endl;
    }
    
    cout << "共输入 " << lineCount << " 行" << endl;
    return 0;
}
```

## 实际应用技巧

### 技巧1：安全的混合输入
```cpp
#include <iostream>
#include <string>
#include <limits>
using namespace std;

void safeInput() {
    int number;
    string text;
    
    // 读取数字
    cout << "请输入数字: ";
    cin >> number;
    cin.ignore(numeric_limits<streamsize>::max(), '\n');
    
    // 读取字符串
    cout << "请输入文本: ";
    getline(cin, text);
    
    cout << "数字: " << number << endl;
    cout << "文本: " << text << endl;
}
```

### 技巧2：处理文件读取
```cpp
#include <iostream>
#include <fstream>
#include <string>
using namespace std;

void readFile() {
    ifstream file("data.txt");
    string line;
    
    if (file.is_open()) {
        while (getline(file, line)) {  // 读取文件的每一行
            cout << line << endl;
        }
        file.close();
    }
}
```

### 技巧3：字符串分割
```cpp
#include <iostream>
#include <string>
#include <sstream>
using namespace std;

void splitString() {
    string line;
    cout << "请输入多个单词: ";
    getline(cin, line);
    
    stringstream ss(line);
    string word;
    
    cout << "分割后的单词:" << endl;
    while (ss >> word) {
        cout << "- " << word << endl;
    }
}
```

## 在算法竞赛中的应用

### 读取未知数量的行
```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

int main() {
    vector<string> lines;
    string line;
    
    // 读取直到文件结束（竞赛中常用）
    while (getline(cin, line)) {
        lines.push_back(line);
    }
    
    // 处理数据...
    for (const string& l : lines) {
        cout << l << endl;
    }
    
    return 0;
}
```

### 处理带空格的测试数据
```cpp
// 输入格式：第一行数字n，后面n行字符串
#include <iostream>
#include <string>
#include <vector>
using namespace std;

int main() {
    int n;
    cin >> n;
    cin.ignore();  // 重要！清除换行符
    
    vector<string> strings;
    for (int i = 0; i < n; i++) {
        string s;
        getline(cin, s);  // 读取可能包含空格的字符串
        strings.push_back(s);
    }
    
    // 处理数据...
    return 0;
}
```

## 常见错误和解决方法

### 错误1：忘记清空缓冲区
```cpp
int num;
string str;

cin >> num;
// ❌ 忘记 cin.ignore();
getline(cin, str);  // 会读取换行符
```

### 错误2：错误的位置清空
```cpp
int a, b;
string s;

cin >> a;
cin.ignore();  // ✅ 正确位置
cin >> b;
cin.ignore();  // ✅ 正确位置  
getline(cin, s);
```

### 错误3：过度清空
```cpp
// 如果后面还要用cin >>，不要过度清空
int a, b;
string s;

cin >> a;
// ❌ 如果用户输入 "123 456"，这会清空456
cin.ignore(numeric_limits<streamsize>::max(), '\n');  
cin >> b;  // 需要用户重新输入
```

## 总结

### `getline(cin, s)` 的核心要点：
1. **读取整行**，包括空格
2. **自动丢弃**换行符
3. **需要处理**缓冲区中的残留字符

### 缓冲区管理黄金法则：
```cpp
// 在 cin >> 后使用 getline 前，一定要清空缓冲区
cin >> variable;
cin.ignore(numeric_limits<streamsize>::max(), '\n');
getline(cin, stringVariable);
```

### 记忆口诀：
> "cin>> 后，getline 前，ignore 一下保平安"

现在你理解 `getline()` 和输入缓冲区的问题了吗？如果清楚了，我们可以继续学习其他内容！