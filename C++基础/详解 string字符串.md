## 基本介绍
`string` 是 C++ 中专门用于处理文本的容器，它把字符数组包装成了更易用的类。

## 头文件
```cpp
#include <string>
using namespace std;
```

## 声明和初始化
```cpp
// 各种创建方式
string s1;                     // 空字符串
string s2 = "hello";           // 用C字符串初始化
string s3("world");            // 直接初始化
string s4(5, 'a');             // 5个'a'："aaaaa"
string s5(s2);                 // 拷贝构造
string s6 = s2 + " " + s3;     // 连接字符串："hello world"
```

## 核心特性

### 优点：
- ✅ **自动内存管理**：不用关心字符串长度
- ✅ **丰富的操作**：拼接、查找、替换等
- ✅ **安全**：避免缓冲区溢出
- ✅ **兼容C风格**：可以转换为 `const char*`

### 缺点：
- ❌ **性能开销**：比原始字符数组稍慢

## 常用操作详解
[[基础 string向int转化的函数方法]]
### 字符串连接
```cpp
string s1 = "Hello";
string s2 = "World";

string s3 = s1 + " " + s2;    // "Hello World"
s1 += " C++";                 // s1变为"Hello C++"

// 也可以用 append()
s1.append(" Programming");    // "Hello C++ Programming"
```

### 访问字符
```cpp
string s = "Hello";

cout << s[0];          // 'H'（不检查边界）
cout << s.at(1);       // 'e'（检查边界，安全）
cout << s.front();     // 'H'（第一个字符）
cout << s.back();      // 'o'（最后一个字符）

// 修改字符
s[0] = 'h';            // "hello"
s.at(1) = 'E';         // "hEllo"
```

### 字符串信息
```cpp
string s = "Hello";

cout << s.length();    // 5
cout << s.size();      // 5（与length()相同）
cout << s.empty();     // 是否为空
cout << s.capacity();  // 当前容量
```

### 字符串比较
```cpp
string s1 = "apple";
string s2 = "banana";

if(s1 == s2) { ... }           // 相等比较
if(s1 < s2)  { ... }           // 字典序比较
if(s1.compare(s2) < 0) { ... } // 同样比较，返回负数表示s1小
```

### 子串操作
```cpp
string s = "Hello World";

string sub1 = s.substr(6);        // 从索引6开始到结尾："World"
string sub2 = s.substr(0, 5);     // 从索引0开始，长度为5："Hello"
string sub3 = s.substr(6, 5);     // 从索引6开始，长度为5："World"
```

### 查找操作
```cpp
string s = "Hello World";

size_t pos = s.find("World");     // 查找子串，返回位置6
if(pos != string::npos) {
    cout << "找到 at " << pos;    // 输出：找到 at 6
}

pos = s.find('o');                // 查找字符，返回4
pos = s.find('o', 5);             // 从索引5开始找'o'，返回7

// 其他查找方法
pos = s.rfind('o');               // 从后往前找，返回7
pos = s.find_first_of("aeiou");   // 找第一个元音，返回1('e')
pos = s.find_last_of("aeiou");    // 找最后一个元音，返回7('o')
```
**查找失败 → 返回 `string::npos`**
 
### 替换操作
```cpp
string s = "Hello World";

s.replace(6, 5, "C++");          // 从索引6开始，替换5个字符为"C++"
cout << s;                        // "Hello C++"

s.replace(0, 5, "Hi");           // 从索引0开始，替换5个字符为"Hi"
cout << s;                        // "Hi C++"
```

### 插入和删除
```cpp
string s = "Hello";

s.insert(5, " World");           // 在索引5处插入 -> "Hello World"
s.erase(5, 6);                   // 从索引5开始删除6个字符 -> "Hello"
s.push_back('!');                // 在末尾添加字符 -> "Hello!"
s.pop_back();                    // 删除末尾字符 -> "Hello"

// 清空字符串
s.clear();                       // 清空所有内容
```

## 输入输出

### 读取字符串
```cpp
string s;

cin >> s;                        // 读取一个单词（遇到空格停止）
getline(cin, s);                 // 读取整行（包括空格）

// 注意：cin >> 后如果要getline，需要先清空缓冲区
int num;
string name;

cin >> num;
cin.ignore();                    // 忽略换行符
getline(cin, name);              // 现在可以正确读取
```
[[详解 getline相关以及缓冲区问题]]

### 输出字符串
```cpp
string s = "Hello";

cout << s << endl;               // 直接输出
printf("%s\n", s.c_str());       // 用C风格输出
```

## 与C风格字符串转换

### string → C字符串
```cpp
string s = "Hello";
const char* cstr = s.c_str();    // 获取C风格字符串
cout << cstr;                    // "Hello"
```

### C字符串 → string
```cpp
const char* cstr = "World";
string s(cstr);                  // 直接构造
string s2 = cstr;               // 赋值
```

## 遍历方法
```cpp
string s = "Hello";

// 方法1：下标遍历
for(int i = 0; i < s.size(); i++) {
    cout << s[i] << " ";
}

// 方法2：范围for循环（C++11）
for(char c : s) {
    cout << c << " ";
}

// 方法3：迭代器
for(auto it = s.begin(); it != s.end(); it++) {
    cout << *it << " ";
}
```

## 实际应用示例
```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    // 用户名验证
    string username;
    cout << "请输入用户名: ";
    getline(cin, username);
    
    // 检查用户名长度
    if(username.length() < 3) {
        cout << "用户名太短！" << endl;
        return 1;
    }
    
    // 检查是否包含空格
    if(username.find(' ') != string::npos) {
        cout << "用户名不能包含空格！" << endl;
        return 1;
    }
    
    cout << "欢迎, " << username << "!" << endl;
    
    // 字符串处理示例
    string message = "  Hello World!  ";
    
    // 去除前后空格（简单版）
    size_t start = message.find_first_not_of(" ");
    size_t end = message.find_last_not_of(" ");
    string trimmed = message.substr(start, end - start + 1);
    
    cout << "原字符串: '" << message << "'" << endl;
    cout << "处理后: '" << trimmed << "'" << endl;
    
    return 0;
}
```

## 算法竞赛技巧
```cpp
// 1. 快速读取整行
string line;
getline(cin, line);

// 2. 字符串分割（简单版）
string data = "apple,banana,orange";
size_t pos = 0;
while((pos = data.find(',')) != string::npos) {
    string token = data.substr(0, pos);
    cout << token << endl;
    data.erase(0, pos + 1);
}
cout << data << endl;  // 最后一个

// 3. 字符串转数字
string num_str = "12345";
int num = stoi(num_str);        // 字符串转整数
double d = stod("3.14");        // 字符串转浮点数

// 4. 数字转字符串
int num = 42;
string str = to_string(num);    // 整数转字符串

// 5. 大小写转换
string s = "Hello";
for(char& c : s) {
    c = toupper(c);             // "HELLO"
}
```

## 内存机制
```cpp
string s;
// string会自动管理内存，类似vector
// 容量会根据需要自动增长

s = "hi";                      // 容量可能为2
s += " this is a long string"; // 容量自动扩容
cout << s.capacity();          // 查看当前容量
s.shrink_to_fit();             // 缩减到合适大小
```

## 总结
`string` 是处理文本的最佳选择，比C风格字符串安全方便很多。记住它的核心特点：
- 自动内存管理
- 丰富的字符串操作
- 支持类似vector的操作
- 与C风格字符串兼容