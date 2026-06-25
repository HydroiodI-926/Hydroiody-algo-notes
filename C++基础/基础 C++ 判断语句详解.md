## 基本概念
判断语句让程序能够根据条件决定执行哪部分代码，就像生活中的"如果...那么..."。

## 头文件
```cpp
#include <iostream>
using namespace std;
```

## 1. if 语句（最基本的判断）

### 基本语法
```cpp
if (条件) {
    // 条件为真时执行的代码
}
```

### 简单示例
```cpp
int score = 85;

if (score >= 60) {
    cout << "及格了！" << endl;
}
```

## 2. if-else 语句（二选一）

### 基本语法
```cpp
if (条件) {
    // 条件为真时执行
} else {
    // 条件为假时执行
}
```

### 示例
```cpp
int age = 16;

if (age >= 18) {
    cout << "成年人" << endl;
} else {
    cout << "未成年人" << endl;
}
```

## 3. if-else if-else 语句（多条件判断）

### 基本语法
```cpp
if (条件1) {
    // 条件1为真时执行
} else if (条件2) {
    // 条件2为真时执行
} else if (条件3) {
    // 条件3为真时执行
} else {
    // 所有条件都为假时执行
}
```

### 成绩等级示例
```cpp
int score = 85;

if (score >= 90) {
    cout << "优秀" << endl;
} else if (score >= 80) {
    cout << "良好" << endl;
} else if (score >= 70) {
    cout << "中等" << endl;
} else if (score >= 60) {
    cout << "及格" << endl;
} else {
    cout << "不及格" << endl;
}
// 输出：良好
```

## 4. 比较运算符

### 常用比较运算符
```cpp
int a = 5, b = 3;

a == b    // 等于：false
a != b    // 不等于：true  
a > b     // 大于：true
a < b     // 小于：false
a >= b    // 大于等于：true
a <= b    // 小于等于：false
```

### 字符串比较
```cpp
string s1 = "apple";
string s2 = "banana";

if (s1 == s2) {
    cout << "相同" << endl;
} else if (s1 < s2) {
    cout << "apple在banana前面" << endl;  // 字典序比较
}
```

## 5. 逻辑运算符

### 与(AND)、或(OR)、非(NOT)
```cpp
int age = 20;
bool isStudent = true;

// 与(&&)：两个条件都为真
if (age >= 18 && isStudent) {
    cout << "成年学生" << endl;
}

// 或(||)：至少一个条件为真
if (age < 12 || age > 65) {
    cout << "特殊人群" << endl;
}

// 非(!)：取反
if (!isStudent) {
    cout << "不是学生" << endl;
}
```

### 复杂逻辑示例
```cpp
int score = 85;
bool attended = true;

if (score >= 60 && attended) {
    cout << "通过考试" << endl;
} else if (score >= 60 && !attended) {
    cout << "成绩合格但缺勤" << endl;
} else {
    cout << "未通过" << endl;
}
```

## 6. 嵌套if语句

### 基本语法
```cpp
if (条件1) {
    if (条件2) {
        // 两个条件都为真时执行
    }
}
```

### 实际示例
```cpp
int age = 20;
bool hasID = true;

if (age >= 18) {
    if (hasID) {
        cout << "允许进入" << endl;
    } else {
        cout << "需要出示身份证" << endl;
    }
} else {
    cout << "未成年人禁止进入" << endl;
}
```

## 7. switch 语句（多分支选择）

### 基本语法
```cpp
switch (表达式) {
    case 值1:
        // 代码
        break;
    case 值2:
        // 代码
        break;
    default:
        // 默认代码
}
```

### 示例
```cpp
int day = 3;

switch (day) {
    case 1:
        cout << "星期一" << endl;
        break;
    case 2:
        cout << "星期二" << endl;
        break;
    case 3:
        cout << "星期三" << endl;  // 执行这个
        break;
    case 4:
        cout << "星期四" << endl;
        break;
    case 5:
        cout << "星期五" << endl;
        break;
    default:
        cout << "周末" << endl;
}
```

## 8. 三元运算符（简洁的判断）

### 基本语法
```cpp
条件 ? 值1 : 值2
```

### 示例
```cpp
int a = 5, b = 3;
int max = (a > b) ? a : b;  // 如果a>b则max=a，否则max=b

string result = (score >= 60) ? "及格" : "不及格";
```
[[详解 三元运算符]]
## 9. 常见判断模式

### 范围判断
```cpp
int num = 25;

if (num >= 1 && num <= 100) {
    cout << "在1-100范围内" << endl;
}
```

### 空值判断
```cpp
string name = "";

if (name.empty()) {
    cout << "姓名为空" << endl;
}

vector<int> v;
if (v.empty()) {
    cout << "vector为空" << endl;
}
```

### 存在性判断
```cpp
vector<int> numbers = {1, 2, 3, 4, 5};
int target = 3;

bool found = false;
for (int num : numbers) {
    if (num == target) {
        found = true;
        break;
    }
}

if (found) {
    cout << "找到了" << endl;
}
```

## 10. 在算法竞赛中的实用技巧

### 快速判断奇偶
```cpp
int n = 5;
if (n % 2 == 0) {
    cout << "偶数" << endl;
} else {
    cout << "奇数" << endl;
}

// 或者用位运算（更快）
if (n & 1) {  // 奇数
    cout << "奇数" << endl;
} else {      // 偶数
    cout << "偶数" << endl;
}
```

### 边界判断
```cpp
// 数组下标边界检查
vector<int> arr = {1, 2, 3, 4, 5};
int index = 3;

if (index >= 0 && index < arr.size()) {
    cout << arr[index] << endl;  // 安全访问
} else {
    cout << "下标越界" << endl;
}
```

### 多条件简化
```cpp
// 用变量存储复杂条件，提高可读性
bool isValidInput = (x >= 0) && (x <= 100) && (y >= 0) && (y <= 100);
bool canMove = (isValidInput) && (grid[x][y] != WALL);

if (canMove) {
    // 移动逻辑
}
```

## 11. 常见错误和注意事项

### 错误1：赋值(=) vs 比较(==)
```cpp
int a = 5;

// ❌ 错误：把比较写成了赋值
if (a = 10) {  // 这会把a赋值为10，然后判断10是否为真（总是真）
    cout << "a是10" << endl;
}

// ✅ 正确：使用比较运算符
if (a == 10) {
    cout << "a是10" << endl;
}
```

### 错误2：忘记break
```cpp
int day = 1;

switch (day) {
    case 1:
        cout << "星期一" << endl;
        // ❌ 忘记break，会继续执行case2
    case 2:
        cout << "星期二" << endl;
        break;
}
// 输出：星期一 星期二（错误！）
```

### 错误3：浮点数比较
```cpp
double a = 0.1 + 0.2;

// ❌ 错误：直接比较浮点数
if (a == 0.3) {
    cout << "相等" << endl;  // 可能不会执行！
}

// ✅ 正确：使用容差比较
if (abs(a - 0.3) < 1e-9) {
    cout << "相等" << endl;
}
```

## 12. 完整示例程序

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    // 学生成绩评估系统
    int score;
    string name;
    
    cout << "请输入学生姓名: ";
    cin >> name;
    cout << "请输入成绩(0-100): ";
    cin >> score;
    
    // 输入验证
    if (score < 0 || score > 100) {
        cout << "成绩无效！" << endl;
        return 1;
    }
    
    // 成绩评级
    string grade;
    if (score >= 90) {
        grade = "A";
    } else if (score >= 80) {
        grade = "B";
    } else if (score >= 70) {
        grade = "C";
    } else if (score >= 60) {
        grade = "D";
    } else {
        grade = "F";
    }
    
    // 额外评价
    string comment;
    if (score == 100) {
        comment = "完美！";
    } else if (score >= 90) {
        comment = "优秀！";
    } else if (score >= 60) {
        comment = "继续努力！";
    } else {
        comment = "需要加油！";
    }
    
    cout << name << "的成绩是: " << score << endl;
    cout << "等级: " << grade << endl;
    cout << "评价: " << comment << endl;
    
    return 0;
}
```

## 总结

判断语句是编程的基础，记住几个要点：
- **if**：基本判断
- **if-else**：二选一  
- **if-else if-else**：多条件判断
- **switch**：多分支选择（适合固定值）
- **三元运算符**：简洁的二选一

**编程思维：** 把复杂问题分解成多个简单的判断条件！