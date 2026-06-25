# C++ istringstream 详解

## 🎯 **istringstream 是什么？**

**istringstream** = input + string + stream（输入字符串流）
- 把**字符串当作输入流**来处理
- 可以像使用 `cin` 一样从字符串中读取数据

## 🔧 **基本用法**

### 头文件和创建
```cpp
#include <sstream>  // 必须包含的头文件
#include <string>
#include <iostream>
using namespace std;

int main() {
    string data = "Hello 123 3.14";
    istringstream iss(data);  // 创建 istringstream 对象
    
    return 0;
}
```

## 💡 **核心功能：字符串分割**

### 按空格分割字符串
```cpp
#include <sstream>
#include <string>
#include <iostream>
using namespace std;

int main() {
    string text = "Apple Banana Cherry Date";
    istringstream iss(text);
    
    string word;
    while (iss >> word) {  // 自动按空格分割！
        cout << word << endl;
    }
    
    return 0;
}
```
**输出：**
```
Apple
Banana
Cherry
Date
```

## 🛠️ **实际应用示例**

### 示例1：解析混合数据
```cpp
#include <sstream>
#include <string>
#include <iostream>
using namespace std;

int main() {
    string data = "John 25 3.75";
    istringstream iss(data);
    
    string name;
    int age;
    double gpa;
    
    iss >> name >> age >> gpa;
    
    cout << "姓名: " << name << endl;
    cout << "年龄: " << age << endl;
    cout << "GPA: " << gpa << endl;
    
    return 0;
}
```

### 示例2：处理 CSV 数据
```cpp
#include <sstream>
#include <string>
#include <iostream>
#include <vector>
using namespace std;

int main() {
    string csv = "Apple,1.2,10;Banana,0.8,15;Orange,1.5,8";
    
    // 第一层分割：按分号
    istringstream lineStream(csv);
    string item;
    
    while (getline(lineStream, item, ';')) {
        cout << "商品组: " << item << endl;
        
        // 第二层分割：按逗号
        istringstream itemStream(item);
        string field;
        while (getline(itemStream, field, ',')) {
            cout << "  - " << field << endl;
        }
    }
    
    return 0;
}
```

### 示例3：类型转换
```cpp
#include <sstream>
#include <string>
#include <iostream>
using namespace std;

int main() {
    string numbers = "10 20 30 40 50";
    istringstream iss(numbers);
    
    int sum = 0;
    int num;
    
    while (iss >> num) {
        sum += num;
        cout << "读取到: " << num << endl;
    }
    
    cout << "总和: " << sum << endl;
    
    return 0;
}
```

## 📋 **常用成员函数**

### `>>` 操作符 - 提取数据
```cpp
string text = "123 3.14 Hello";
istringstream iss(text);

int a;
double b;
string c;

iss >> a >> b >> c;  // 自动类型转换！
```

### `str()` - 获取/设置字符串
```cpp
istringstream iss("初始内容");
cout << iss.str() << endl;  // 获取当前字符串

iss.str("新的内容");        // 设置新字符串
```

### 状态检查函数
```cpp
istringstream iss("100 200");
int a, b, c;

iss >> a >> b;
cout << "eof: " << iss.eof() << endl;    // 是否到结尾
cout << "fail: " << iss.fail() << endl;  // 是否失败
cout << "good: " << iss.good() << endl;  // 是否正常

iss >> c;  // 尝试读取不存在的第三个数字
cout << "fail: " << iss.fail() << endl;  // 变为 true
```

## 🔄 **与 getline 配合使用**

### 处理复杂格式
```cpp
#include <sstream>
#include <string>
#include <iostream>
using namespace std;

int main() {
    string complexData = "姓名:John|年龄:25|城市:New York";
    
    istringstream iss(complexData);
    string pair;
    
    while (getline(iss, pair, '|')) {
        istringstream pairStream(pair);
        string key, value;
        
        getline(pairStream, key, ':');
        getline(pairStream, value);
        
        cout << key << " = " << value << endl;
    }
    
    return 0;
}
```

## ⚠️ **常见错误和注意事项**

### 错误1：重复使用不重置
```cpp
// ❌ 错误写法
istringstream iss("10 20");
int a, b, c;

iss >> a >> b;  // 正常读取
iss >> c;       // 失败！流已经到末尾

// ✅ 正确写法
iss.clear();           // 清除错误状态
iss.str("30 40 50");   // 设置新内容
iss >> a >> b >> c;    // 重新读取
```

### 错误2：类型不匹配
```cpp
string data = "Hello 123";
istringstream iss(data);

string text;
int number;

iss >> text;     // 成功：读取 "Hello"
iss >> number;   // 成功：读取 123

// 但如果数据是 "Hello World"
// iss >> number 会失败！
```

## 🎯 **实用技巧**

### 技巧1：安全的类型转换函数
```cpp
#include <sstream>
#include <string>
using namespace std;

template<typename T>
T stringTo(const string& str) {
    istringstream iss(str);
    T result;
    iss >> result;
    
    if (iss.fail()) {
        throw runtime_error("转换失败");
    }
    
    return result;
}

// 使用
int num = stringTo<int>("123");
double val = stringTo<double>("3.14");
```

### 技巧2：一次读取所有数字
```cpp
#include <sstream>
#include <string>
#include <vector>
#include <iostream>
using namespace std;

vector<int> extractNumbers(const string& text) {
    istringstream iss(text);
    vector<int> numbers;
    int num;
    
    while (iss >> num) {
        numbers.push_back(num);
    }
    
    return numbers;
}

int main() {
    string data = "这里有10个苹果和20个香蕉";
    auto nums = extractNumbers(data);
    
    for (int n : nums) {
        cout << n << " ";  // 输出: 10 20
    }
    
    return 0;
}
```

## 💡 **与其他方法的对比**

| 方法 | 优点 | 缺点 |
|------|------|------|
| **istringstream** | 类型安全、自动分割、C++标准 | 性能稍慢 |
| **strtok** | 性能高、C标准库 | 不安全、会修改原字符串 |
| **手动分割** | 完全控制、性能最好 | 代码复杂、容易出错 |

## 📋 **总结**

### istringstream 的核心价值：
1. **字符串分割**：自动按空格分割单词
2. **类型转换**：字符串 → 各种数据类型
3. **格式解析**：处理 CSV、配置文件等结构化文本
4. **安全可靠**：类型安全，不会内存越界

### 记住这个万能模式：
```cpp
string input = "你的数据";
istringstream iss(input);
数据类型 变量;

while (iss >> 变量) {
    // 处理每个数据
}
```

**一句话总结：istringstream 让字符串处理变得像文件/控制台输入一样简单！**