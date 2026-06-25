## 🔍 istringstream（输入字符串流）

### 是什么？
- **istringstream** = input + string + stream
- 把字符串当作输入流来处理的工具
- 包含在 `<sstream>` 头文件中

### 有什么用？
将字符串按照空格自动分割成多个部分

```cpp
#include <sstream>  // 必须包含这个头文件
#include <iostream>
#include <string>
using namespace std;

int main() {
    string text = "Apple 123 3.14 Hello";
    
    // 创建 istringstream 对象
    istringstream iss(text);
    
    string word;
    int number;
    double price;
    string greeting;
    
    // 像使用 cin 一样从字符串中读取数据
    iss >> word >> number >> price >> greeting;
    
    cout << word << endl;     // 输出: Apple
    cout << number << endl;   // 输出: 123
    cout << price << endl;    // 输出: 3.14
    cout << greeting << endl; // 输出: Hello
    
    return 0;
}
```

### 实际应用场景
```cpp
#include <iostream>
#include <string>
#include <sstream>
#include <vector>
using namespace std;

int main() {
    string line = "10 20 30 40 50";
    istringstream iss(line);  // 把字符串包装成输入流
    
    vector<int> numbers;
    int num;
    
    // 逐个读取数字
    while (iss >> num) {
        numbers.push_back(num);
    }
    
    cout << "读取到的数字: ";
    for (int n : numbers) {
        cout << n << " ";
    }
    // 输出: 读取到的数字: 10 20 30 40 50
    
    return 0;
}
```

## 🔄 continue（继续）

### 是什么？
- 循环控制关键字
- 跳过当前循环的剩余部分，直接开始下一次循环

### 怎么用？
```cpp
#include <iostream>
using namespace std;

int main() {
    for (int i = 1; i <= 5; i++) {
        if (i == 3) {
            continue;  // 跳过数字3的处理
        }
        cout << "处理数字: " << i << endl;
    }
    
    return 0;
}
```
**输出：**
```
处理数字: 1
处理数字: 2
处理数字: 4
处理数字: 5
```
注意：数字3被跳过了！

### 实际应用场景
```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string line;
    
    while (getline(cin, line)) {
        // 跳过空行
        if (line.empty()) {
            continue;  // 直接开始下一次循环，不处理空行
        }
        
        // 跳过注释行（以#开头）
        if (line[0] == '#') {
            continue;  // 跳过注释
        }
        
        // 处理有效行
        cout << "有效内容: " << line << endl;
    }
    
    return 0;
}
```

## 📝 iss（变量名）

### 是什么？
- 只是一个**变量名**，不是关键字
- 通常用来命名 `istringstream` 对象
- iss = Input String Stream 的缩写

### 命名习惯
```cpp
// 常见的命名方式
istringstream iss(line);     // 最常用
istringstream input(line);   // 也可以
istringstream stream(line);  // 也可以
istringstream ss(line);      // 也可以

// 本质上就是一个变量名，你可以随便起名
istringstream my_stream(line);  // 这样也行！
```

## 🎯 三者结合使用的完整示例

```cpp
#include <iostream>
#include <string>
#include <sstream>
#include <vector>
using namespace std;

int main() {
    string input;
    
    cout << "请输入多行数据 (空行结束):" << endl;
    
    while (getline(cin, input)) {
        // 使用 continue 跳过空行
        if (input.empty()) {
            continue;
        }
        
        // 使用 istringstream 解析每行数据
        istringstream iss(input);  // iss 是变量名
        
        vector<int> numbers;
        int num;
        
        // 读取该行中的所有数字
        while (iss >> num) {
            numbers.push_back(num);
        }
        
        // 跳过没有数字的行
        if (numbers.empty()) {
            continue;
        }
        
        // 处理有数字的行
        cout << "本行数字: ";
        for (int n : numbers) {
            cout << n << " ";
        }
        cout << endl;
    }
    
    return 0;
}
```

## 📋 快速总结表

| 关键字 | 类型 | 作用 | 示例 |
|--------|------|------|------|
| **istringstream** | 类 | 把字符串当输入流处理 | `istringstream iss("1 2 3");` |
| **continue** | 关键字 | 跳过当前循环的剩余部分 | `if(x==0) continue;` |
| **iss** | 变量名 | 通常用于命名 istringstream 对象 | `istringstream iss(line);` |

## 💡 记忆技巧

- **istringstream**：把字符串"变成"cin
- **continue**：循环中的"跳过"按钮
- **iss**：只是一个方便记忆的变量名

记住：`istringstream` + `continue` = 强大的文本处理组合！