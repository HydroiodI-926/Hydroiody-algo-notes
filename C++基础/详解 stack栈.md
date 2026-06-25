## 基本介绍
`stack` 是 C++ 中的**栈容器**，它遵循**后进先出（LIFO）**的原则，就像一叠盘子，最后放上去的盘子最先被取下来。

## 头文件
```cpp
#include <stack>
using namespace std;
```

## 声明和初始化
```cpp
// 创建栈
stack<int> s1;                    // 空栈
stack<string> s2;                 // 字符串栈

// 从其他容器初始化
vector<int> vec = {1, 2, 3, 4, 5};
stack<int> s3(deque<int>(vec.begin(), vec.end()));

// 注意：stack没有直接的列表初始化，需要逐个push
```

## 核心特性

### 优点：
- ✅ **后进先出**：保证元素按相反顺序处理
- ✅ **操作简单**：只有基本的栈操作
- ✅ **线程安全**：在单线程中操作安全

### 缺点：
- ❌ **功能有限**：不能随机访问，只能访问栈顶
- ❌ **不支持迭代器**：不能遍历栈
- ❌ **底层依赖**：默认基于deque实现

## 常用操作详解

### 添加元素（压栈）
```cpp
stack<int> s;

s.push(1);     // 栈: [1]（栈顶）
s.push(2);     // 栈: [1, 2]（栈顶）
s.push(3);     // 栈: [1, 2, 3]（栈顶）
s.push(4);     // 栈: [1, 2, 3, 4]（栈顶）

// 也可以使用emplace（C++11）
s.emplace(5);  // 栈: [1, 2, 3, 4, 5]（栈顶）
```

### 访问栈顶元素
```cpp
stack<int> s;
s.push(1);
s.push(2);
s.push(3);

cout << s.top() << endl;  // 3（栈顶元素，最后加入的）

// 注意：只是访问，不会删除元素
```

### 删除元素（出栈）
```cpp
stack<int> s;
s.push(1);
s.push(2);
s.push(3);

cout << "出栈: " << s.top() << endl;  // 3
s.pop();        // 删除栈顶元素，栈变为: [1, 2]（栈顶为2）

cout << "新栈顶: " << s.top() << endl;  // 2
s.pop();        // 删除栈顶元素，栈变为: [1]（栈顶为1）
```

### 容量和信息
```cpp
stack<int> s;
s.push(1);
s.push(2);
s.push(3);

cout << s.size() << endl;   // 3（元素个数）
cout << s.empty() << endl;  // false（是否为空）

// 清空栈
while (!s.empty()) {
    s.pop();
}
cout << s.empty() << endl;  // true
```

## 实际应用示例

### 示例1：函数调用栈模拟
```cpp
#include <iostream>
#include <stack>
#include <string>
using namespace std;

int main() {
    stack<string> callStack;
    
    // 模拟函数调用
    callStack.push("main");
    callStack.push("functionA");
    callStack.push("functionB");
    callStack.push("functionC");
    
    // 模拟函数返回（后进先出）
    cout << "函数返回顺序:" << endl;
    while (!callStack.empty()) {
        string currentFunction = callStack.top();
        cout << "返回: " << currentFunction << endl;
        callStack.pop();
    }
    
    return 0;
}
```

### 示例2：括号匹配检查
```cpp
#include <iostream>
#include <stack>
#include <string>
using namespace std;

bool checkParentheses(const string& expr) {
    stack<char> s;
    
    for (char c : expr) {
        if (c == '(' || c == '[' || c == '{') {
            s.push(c);
        } else if (c == ')' || c == ']' || c == '}') {
            if (s.empty()) {
                return false;  // 右括号多余
            }
            char top = s.top();
            s.pop();
            
            // 检查括号是否匹配
            if ((c == ')' && top != '(') ||
                (c == ']' && top != '[') ||
                (c == '}' && top != '{')) {
                return false;
            }
        }
    }
    
    return s.empty();  // 如果栈为空，则所有括号都匹配
}

int main() {
    string expr1 = "((a+b)*[c-d])";
    string expr2 = "([)]";
    
    cout << expr1 << " 括号匹配: " << (checkParentheses(expr1) ? "是" : "否") << endl;
    cout << expr2 << " 括号匹配: " << (checkParentheses(expr2) ? "是" : "否") << endl;
    
    return 0;
}
```

### 示例3：表达式求值（简化版）
```cpp
#include <iostream>
#include <stack>
#include <string>
#include <cctype>
using namespace std;

int evaluatePostfix(const string& expr) {
    stack<int> s;
    
    for (char c : expr) {
        if (isdigit(c)) {
            s.push(c - '0');  // 字符转数字
        } else {
            int b = s.top(); s.pop();
            int a = s.top(); s.pop();
            
            switch (c) {
                case '+': s.push(a + b); break;
                case '-': s.push(a - b); break;
                case '*': s.push(a * b); break;
                case '/': s.push(a / b); break;
            }
        }
    }
    
    return s.top();
}

int main() {
    string postfix = "23+45-*";  // 表示 (2+3)*(4-5)
    cout << "后缀表达式 " << postfix << " 的结果是: " << evaluatePostfix(postfix) << endl;
    return 0;
}
```

## 在算法竞赛中的应用

### 应用1：深度优先搜索（DFS）模拟
```cpp
#include <iostream>
#include <stack>
#include <vector>
using namespace std;

void DFS(vector<vector<int>>& graph, int start) {
    int n = graph.size();
    vector<bool> visited(n, false);
    stack<int> s;
    
    s.push(start);
    visited[start] = true;
    
    while (!s.empty()) {
        int node = s.top();
        s.pop();
        cout << "访问节点: " << node << endl;
        
        // 注意：为了保持与递归DFS相同的顺序，我们从右往左压栈
        for (auto it = graph[node].rbegin(); it != graph[node].rend(); it++) {
            int neighbor = *it;
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                s.push(neighbor);
            }
        }
    }
}

int main() {
    // 示例图
    vector<vector<int>> graph = {
        {1, 2},
        {0, 3, 4},
        {0, 5},
        {1},
        {1, 5},
        {2, 4}
    };
    
    DFS(graph, 0);
    return 0;
}
```

### 应用2：浏览器历史记录模拟
```cpp
#include <iostream>
#include <stack>
#include <string>
using namespace std;

int main() {
    stack<string> backStack, forwardStack;
    string currentPage = "首页";
    
    // 模拟浏览网页
    backStack.push(currentPage);
    
    // 访问新页面
    currentPage = "新闻页";
    backStack.push(currentPage);
    cout << "当前页面: " << currentPage << endl;
    
    currentPage = "体育新闻";
    backStack.push(currentPage);
    cout << "当前页面: " << currentPage << endl;
    
    // 模拟点击后退
    if (backStack.size() > 1) {
        forwardStack.push(backStack.top());
        backStack.pop();
        currentPage = backStack.top();
        cout << "后退到: " << currentPage << endl;
    }
    
    // 模拟点击前进
    if (!forwardStack.empty()) {
        backStack.push(forwardStack.top());
        currentPage = forwardStack.top();
        forwardStack.pop();
        cout << "前进到: " << currentPage << endl;
    }
    
    return 0;
}
```

## 特殊用法和技巧

### 使用其他容器作为底层
```cpp
#include <iostream>
#include <stack>
#include <vector>
using namespace std;

// 使用vector作为stack的底层容器
stack<int, vector<int>> s;

// 这样可以在需要时使用vector的特性，但stack的接口不变
s.push(1);
s.push(2);
cout << s.top() << endl;  // 2
```

### 栈交换
```cpp
stack<int> s1, s2;
s1.push(1);
s1.push(2);
s2.push(3);

s1.swap(s2);  // 交换两个栈的内容

cout << s1.top() << endl;  // 3
cout << s2.top() << endl;  // 2
```

## 性能注意事项

### 时间复杂度
- **push**：O(1)
- **pop**：O(1)  
- **top**：O(1)
- **size/empty**：O(1)

### 空间复杂度
- O(n)

## 常见错误

### 错误1：空栈访问
```cpp
stack<int> s;
// cout << s.top() << endl;  // ❌ 未定义行为，栈为空
// s.pop();                  // ❌ 未定义行为，栈为空

// ✅ 正确做法
if (!s.empty()) {
    cout << s.top() << endl;
    s.pop();
}
```

### 错误2：误解LIFO原则
```cpp
stack<int> s;
s.push(1);
s.push(2);
s.push(3);

// 有些人错误地认为可以访问非栈顶元素
// ❌ 错误：试图访问栈中间元素
// ✅ 正确：只能访问栈顶元素s.top()
```

## 总结

`stack` 是**后进先出栈**，核心特点：
- 元素后进先出（LIFO）
- 只能在栈顶添加和删除
- 不支持随机访问和遍历
- 操作简单高效

**适用场景：**
- 需要按相反顺序处理任务
- 深度优先搜索（DFS）
- 括号匹配、表达式求值
- 函数调用栈、浏览器历史记录

**记忆口诀：**
> "stack栈，后进先出，压栈弹栈，顶顶顶"
