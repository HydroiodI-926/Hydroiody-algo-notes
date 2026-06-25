## 🎯 new 是什么？

`new` 是 C++ 中的**动态内存分配运算符**，用于在**堆(heap)**上分配内存。

## 💡 基本用法

### 1. 分配单个变量
```cpp
// 语法：指针 = new 类型;
int* ptr = new int;        // 分配一个int，未初始化
int* ptr2 = new int(42);   // 分配一个int并初始化为42
double* d = new double(3.14); // 分配double并初始化

// 使用
*ptr = 100;
cout << *ptr << endl;      // 输出: 100
cout << *ptr2 << endl;     // 输出: 42

// 必须手动释放！
delete ptr;
delete ptr2;
delete d;
```

### 2. 分配数组
```cpp
// 语法：指针 = new 类型[大小];
int* arr = new int[5];     // 分配5个int的数组

// 初始化数组
for (int i = 0; i < 5; i++) {
    arr[i] = i * 10;
}

// 使用
for (int i = 0; i < 5; i++) {
    cout << arr[i] << " "; // 输出: 0 10 20 30 40
}

// 必须用 delete[] 释放数组！
delete[] arr;
```

### 3. 分配对象
```cpp
class Student {
public:
    string name;
    int age;
    Student(string n, int a) : name(n), age(a) {}
};

// 分配对象并调用构造函数
Student* student = new Student("Alice", 20);

cout << student->name << " " << student->age << endl;

delete student;  // 会调用析构函数
```

## 🔄 new 与 malloc 的区别

| 特性 | new | malloc |
|------|-----|--------|
| **语言** | C++ 运算符 | C 库函数 |
| **返回值** | 正确类型的指针 | void* 需要强制转换 |
| **初始化** | 调用构造函数 | 不初始化 |
| **失败处理** | 抛出异常 | 返回 NULL |
| **大小计算** | 自动计算 | 手动计算 |
| **推荐使用** | ✅ C++中推荐 | ❌ C++中不推荐 |

```cpp
// new (推荐)
int* p1 = new int(100);

// malloc (不推荐在C++中使用)
int* p2 = (int*)malloc(sizeof(int));
*p2 = 100;

// 释放
delete p1;
free(p2);
```

## ⚠️ 重要规则：有 new 就要有 delete

### 内存泄漏示例
```cpp
// ❌ 错误：内存泄漏
void badFunction() {
    int* ptr = new int(100);
    // 忘记 delete ptr; ← 内存泄漏！
}

// ✅ 正确：配对使用
void goodFunction() {
    int* ptr = new int(100);
    // 使用 ptr...
    delete ptr;  // 释放内存
}
```

### 数组的特殊释放
```cpp
int* single = new int(10);
int* array = new int[100];

// 正确释放
delete single;    // 单个变量
delete[] array;   // 数组必须用 delete[]

// ❌ 错误！
// delete array;     // 未定义行为！
// delete[] single;  // 未定义行为！
```

## 🛠️ 实用技巧

### 1. 检查分配是否成功
```cpp
// 方法1：使用异常（默认）
try {
    int* huge = new int[1000000000000];
    delete[] huge;
} catch (const bad_alloc& e) {
    cout << "内存分配失败: " << e.what() << endl;
}

// 方法2：使用 nothrow
int* ptr = new(nothrow) int[1000];
if (ptr == nullptr) {
    cout << "内存分配失败" << endl;
} else {
    // 使用 ptr
    delete[] ptr;
}
```

### 2. 二维数组分配
```cpp
// 分配 3x4 的二维数组
int rows = 3, cols = 4;
int** matrix = new int*[rows];  // 分配行指针数组

for (int i = 0; i < rows; i++) {
    matrix[i] = new int[cols];  // 为每行分配列
}

// 初始化
for (int i = 0; i < rows; i++) {
    for (int j = 0; j < cols; j++) {
        matrix[i][j] = i * j;
    }
}

// 释放：先释放每一行，再释放行指针数组
for (int i = 0; i < rows; i++) {
    delete[] matrix[i];
}
delete[] matrix;
```

### 3. 与指针的配合使用
```cpp
#include <iostream>
using namespace std;

int main() {
    // 动态分配字符串
    char* str = new char[100];
    strcpy(str, "Hello, Dynamic Memory!");
    cout << str << endl;
    delete[] str;
    
    // 动态分配结构体
    struct Point {
        int x, y;
    };
    
    Point* point = new Point{10, 20};
    cout << "(" << point->x << ", " << point->y << ")" << endl;
    delete point;
    
    return 0;
}
```

## 🎯 现代 C++ 的更好选择

### 使用智能指针（推荐！）
```cpp
#include <memory>

// unique_ptr：独占所有权
unique_ptr<int> ptr1 = make_unique<int>(42);
unique_ptr<int[]> arr1 = make_unique<int[]>(100);

// shared_ptr：共享所有权  
shared_ptr<int> ptr2 = make_shared<int>(100);

// 不需要手动 delete！
// 自动管理内存释放
```

### 使用标准容器
```cpp
#include <vector>
#include <string>

// 代替动态数组
vector<int> numbers = {1, 2, 3, 4, 5};
numbers.push_back(6);  // 自动管理内存

// 代替动态字符串数组
vector<string> names = {"Alice", "Bob"};
names.emplace_back("Charlie");
```

## 📋 快速总结表

| 操作 | 语法 | 释放方式 |
|------|------|----------|
| 单个变量 | `new Type` | `delete ptr` |
| 单个变量(初始化) | `new Type(value)` | `delete ptr` |
| 数组 | `new Type[size]` | `delete[] ptr` |
| 不抛异常 | `new(nothrow) Type` | 同上 |
| 对象 | `new Class(args)` | `delete ptr` |

## ⚠️ 常见错误

```cpp
// 1. 内存泄漏
int* leak = new int(100);
// 忘记 delete leak;

// 2. 重复释放
int* p = new int(10);
delete p;
// delete p;  // ❌ 错误：重复释放

// 3. 使用已释放的内存
int* p = new int(20);
delete p;
// cout << *p;  // ❌ 错误：野指针

// 4. 错误的释放方式
int* arr = new int[10];
// delete arr;  // ❌ 应该用 delete[] arr;
```

## 💡 最佳实践

1. **尽量少用 new/delete** - 使用智能指针或标准容器
2. **new 和 delete 必须成对出现**
3. **数组用 delete[]，单个用 delete**
4. **分配后检查是否成功**
5. **释放后将指针设为 nullptr**

```cpp
int* ptr = new int(100);
// 使用...
delete ptr;
ptr = nullptr;  // 防止野指针
```

记住：**有 new 必有 delete**，这是 C++ 程序员的基本素养！