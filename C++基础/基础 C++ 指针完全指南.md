
## 1. 指针基础概念

### 什么是指针？
指针是一个**变量**，它存储的是另一个变量的**内存地址**。

### 比喻理解
- **变量** = 房间（存放数据）
- **内存地址** = 门牌号  
- **指针** = 写着门牌号的纸条

## 2. 基本语法和操作符

### 声明指针
```cpp
数据类型* 指针变量名;
```
```cpp
int* ptr;        // 指向整数的指针
double* dPtr;    // 指向双精度浮点数的指针
char* cPtr;      // 指向字符的指针
```

### 关键操作符
```cpp
int num = 42;

&num;    // 取地址运算符 - 获取num的内存地址
*ptr;    // 解引用运算符 - 获取ptr指向的值
```

### 完整示例
```cpp
#include <iostream>
using namespace std;

int main() {
    int num = 100;
    int* ptr = &num;     // ptr保存num的地址
    
    cout << "变量num的值: " << num << endl;           // 100
    cout << "变量num的地址: " << &num << endl;        // 0x7ff...
    cout << "指针ptr的值: " << ptr << endl;           // 0x7ff... (与&num相同)
    cout << "指针ptr指向的值: " << *ptr << endl;      // 100
    
    *ptr = 200;          // 通过指针修改变量值
    cout << "修改后num的值: " << num << endl;         // 200
    
    return 0;
}
```

## 3. 指针的初始化

### 正确初始化方式
```cpp
int num = 10;

// 方式1：直接指向变量
int* ptr1 = &num;

// 方式2：先声明后赋值
int* ptr2;
ptr2 = &num;

// 方式3：初始化为空指针
int* ptr3 = nullptr;
```

### 危险操作（避免！）
```cpp
int* badPtr;        // ❌ 未初始化，指向随机地址
// *badPtr = 5;     // 可能导致程序崩溃

int* nullPtr = nullptr;
// *nullPtr = 5;    // ❌ 访问空指针，程序崩溃
```

## 4. 动态内存管理

### new 和 delete
```cpp
// 动态分配单个变量
int* ptr = new int;     // 分配一个int大小的内存
*ptr = 42;              // 在分配的内存中存储值
delete ptr;             // 释放内存

// 动态分配数组
int size = 10;
int* arr = new int[size];   // 分配10个int的数组

for(int i = 0; i < size; i++) {
    arr[i] = i * i;         // 像普通数组一样使用
}

delete[] arr;               // 释放数组内存
```
[[详解 new函数]]
### 二维动态数组
```cpp
int rows = 3, cols = 4;

// 分配行指针数组
int** grid = new int*[rows];

// 为每一行分配列数组
for(int i = 0; i < rows; i++) {
    grid[i] = new int[cols];
}

// 使用二维数组
grid[1][2] = 42;

// 释放内存（按分配顺序逆序）
for(int i = 0; i < rows; i++) {
    delete[] grid[i];
}
delete[] grid;
```

## 5. 指针与函数

### 指针作为函数参数
```cpp
// 传值（无法修改原变量）
void incrementByValue(int x) {
    x++;
}

// 传指针（可以修改原变量）
void incrementByPointer(int* x) {
    (*x)++;
}

// 传引用（更简洁的替代方案）
void incrementByReference(int& x) {
    x++;
}

int main() {
    int num = 10;
    
    incrementByValue(num);
    cout << num << endl;        // 10 (未改变)
    
    incrementByPointer(&num);
    cout << num << endl;        // 11 (已改变)
    
    incrementByReference(num);
    cout << num << endl;        // 12 (已改变)
    
    return 0;
}
```
[[详解 指针作为函数参数]]

### 返回指针的函数
```cpp
// 返回动态分配数组的函数
int* createArray(int size) {
    int* arr = new int[size];
    for(int i = 0; i < size; i++) {
        arr[i] = i + 1;
    }
    return arr;
}

int main() {
    int* myArray = createArray(5);
    
    for(int i = 0; i < 5; i++) {
        cout << myArray[i] << " ";  // 1 2 3 4 5
    }
    
    delete[] myArray;  // 记得释放内存！
    return 0;
}
```
[[详解 不释放内存会怎么样]]
## 6. 指针与数组

### 数组名的本质
```cpp
int arr[5] = {1, 2, 3, 4, 5};

// arr本质上是指向数组第一个元素的指针
cout << arr << endl;        // 数组首地址
cout << &arr[0] << endl;    // 与arr相同
cout << *arr << endl;       // 1 (第一个元素的值)

// 指针算术
cout << *(arr + 1) << endl; // 2 (第二个元素)
cout << *(arr + 2) << endl; // 3 (第三个元素)
```

### 遍历数组的指针方式
```cpp
int arr[5] = {10, 20, 30, 40, 50};

// 方式1：使用下标
for(int i = 0; i < 5; i++) {
    cout << arr[i] << " ";
}

// 方式2：使用指针算术
for(int i = 0; i < 5; i++) {
    cout << *(arr + i) << " ";
}

// 方式3：使用指针变量
int* ptr = arr;
for(int i = 0; i < 5; i++) {
    cout << *ptr << " ";
    ptr++;  // 移动到下一个元素
}
```
[[详解 遍历数组的指针方式]]
## 7. 特殊指针类型

### void 指针
可以指向任何数据类型，但需要类型转换才能使用
```cpp
int num = 42;
double pi = 3.14;

void* vPtr;

vPtr = &num;        // 指向int
cout << *(int*)vPtr << endl;  // 需要强制类型转换

vPtr = &pi;         // 指向double  
cout << *(double*)vPtr << endl;
```

### const 指针
```cpp
int num = 10;
const int* ptr1 = &num;     // 指向常量的指针：不能通过ptr1修改值
// *ptr1 = 20;              // ❌ 错误

int* const ptr2 = &num;     // 常量指针：ptr2不能指向其他地址
// ptr2 = &otherNum;        // ❌ 错误

const int* const ptr3 = &num; // 指向常量的常量指针：都不能修改
```

## 8. 常见错误和调试技巧

### 常见错误
```cpp
// 1. 空指针解引用
int* ptr = nullptr;
// *ptr = 5;  // ❌ 运行时错误

// 2. 野指针
int* wildPtr;
// *wildPtr = 5;  // ❌ 未定义行为

// 3. 内存泄漏
void leakMemory() {
    int* ptr = new int[100];
    // 忘记 delete[] ptr;
}

// 4. 重复释放
int* ptr = new int;
delete ptr;
// delete ptr;  // ❌ 重复释放
```

### 调试技巧
```cpp
#include <iostream>
using namespace std;

// 检查指针有效性
void safePrint(int* ptr) {
    if(ptr != nullptr) {
        cout << *ptr << endl;
    } else {
        cout << "指针为空!" << endl;
    }
}

// 使用断言调试
#include <cassert>
void processArray(int* arr, int size) {
    assert(arr != nullptr);     // 如果arr为空，程序终止
    assert(size > 0);          // 如果size<=0，程序终止
    
    // 处理数组...
}
```

## 9. 在算法竞赛中的应用

### 动态数据结构
```cpp
// 动态数组（根据输入调整大小）
int main() {
    int n;
    cin >> n;
    
    int* arr = new int[n];
    for(int i = 0; i < n; i++) {
        cin >> arr[i];
    }
    
    // 处理数据...
    
    delete[] arr;
    return 0;
}

// 链表节点
struct ListNode {
    int val;
    ListNode* next;
    ListNode(int x) : val(x), next(nullptr) {}
};
```

### 性能优化
```cpp
// 传递大对象时使用指针/引用避免拷贝
struct BigData {
    int data[1000000];  // 很大的数据
};

void processByValue(BigData data) {}    // ❌ 拷贝整个数组，性能差
void processByPointer(BigData* data) {} // ✅ 只传递指针，高效
void processByReference(BigData& data) {} // ✅ 同样高效
```

## 10. 记忆口诀

- `&` → **取地址**（问前台要房间号）
- `*` → **解引用**（用钥匙开门拿东西）
- `类型*` → 声明指针类型
- `new` → 申请内存
- `delete` → 释放内存

## 总结

指针是C++中强大但需要小心使用的工具。掌握指针对于算法竞赛和底层编程至关重要。关键是要理解指针的本质是内存地址，并养成良好的内存管理习惯。

**黄金法则**：每个 `new` 都必须对应一个 `delete`，每个 `new[]` 都必须对应一个 `delete[]`！