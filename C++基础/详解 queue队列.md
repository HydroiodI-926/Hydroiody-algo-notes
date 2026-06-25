## 基本介绍
`queue` 是 C++ 中的**队列容器**，它遵循**先进先出（FIFO）**的原则，就像现实生活中的排队一样。

## 头文件
```cpp
#include <queue>
using namespace std;
```

## 声明和初始化
```cpp
// 创建队列
queue<int> q1;                    // 空队列
queue<string> q2;                 // 字符串队列

// 从其他容器初始化
vector<int> vec = {1, 2, 3, 4, 5};
queue<int> q3(deque<int>(vec.begin(), vec.end()));

// 注意：queue没有直接的列表初始化，需要逐个push
```

## 核心特性

### 优点：
- ✅ **先进先出**：保证元素按顺序处理
- ✅ **操作简单**：只有基本的队列操作
- ✅ **线程安全**：在单线程中操作安全

### 缺点：
- ❌ **功能有限**：不能随机访问，只能访问头部和尾部
- ❌ **不支持迭代器**：不能遍历队列
- ❌ **底层依赖**：默认基于deque实现

## 常用操作详解

### 添加元素（入队）
```cpp
queue<int> q;

q.push(1);     // 队列: [1]
q.push(2);     // 队列: [1, 2]
q.push(3);     // 队列: [1, 2, 3]
q.push(4);     // 队列: [1, 2, 3, 4]

// 也可以使用emplace（C++11）
q.emplace(5);  // 队列: [1, 2, 3, 4, 5]
```

### 访问元素
```cpp
queue<int> q;
q.push(1);
q.push(2);
q.push(3);

cout << q.front() << endl;  // 1（队首元素，最早加入的）
cout << q.back() << endl;   // 3（队尾元素，最后加入的）

// 注意：只是访问，不会删除元素
```

### 删除元素（出队）
```cpp
queue<int> q;
q.push(1);
q.push(2);
q.push(3);

cout << "出队: " << q.front() << endl;  // 1
q.pop();        // 删除队首元素，队列变为: [2, 3]

cout << "新队首: " << q.front() << endl;  // 2
q.pop();        // 删除队首元素，队列变为: [3]
```

### 容量和信息
```cpp
queue<int> q;
q.push(1);
q.push(2);
q.push(3);

cout << q.size() << endl;   // 3（元素个数）
cout << q.empty() << endl;  // false（是否为空）

// 清空队列
while (!q.empty()) {
    q.pop();
}
cout << q.empty() << endl;  // true
```

## 实际应用示例

### 示例1：简单的任务处理系统
```cpp
#include <iostream>
#include <queue>
#include <string>
using namespace std;

int main() {
    queue<string> tasks;
    
    // 添加任务
    tasks.push("任务1：清理数据");
    tasks.push("任务2：生成报告");
    tasks.push("任务3：备份文件");
    tasks.push("任务4：发送邮件");
    
    // 处理任务
    cout << "开始处理任务..." << endl;
    int taskCount = 0;
    
    while (!tasks.empty()) {
        string currentTask = tasks.front();
        cout << "正在执行: " << currentTask << endl;
        tasks.pop();
        taskCount++;
        
        // 模拟任务处理时间
        // 这里可以添加实际的任务处理代码
    }
    
    cout << "所有任务完成！共处理了 " << taskCount << " 个任务" << endl;
    return 0;
}
```

### 示例2：广度优先搜索（BFS）模拟
```cpp
#include <iostream>
#include <queue>
#include <vector>
using namespace std;

int main() {
    // 模拟图的邻接表
    vector<vector<int>> graph = {
        {1, 2},     // 节点0的邻居
        {0, 3, 4},  // 节点1的邻居
        {0, 5},     // 节点2的邻居
        {1},        // 节点3的邻居
        {1, 5},     // 节点4的邻居
        {2, 4}      // 节点5的邻居
    };
    
    vector<bool> visited(6, false);
    queue<int> q;
    
    // 从节点0开始BFS
    q.push(0);
    visited[0] = true;
    
    cout << "BFS遍历顺序: ";
    while (!q.empty()) {
        int current = q.front();
        q.pop();
        cout << current << " ";
        
        // 遍历当前节点的所有邻居
        for (int neighbor : graph[current]) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                q.push(neighbor);
            }
        }
    }
    cout << endl;
    
    return 0;
}
```

### 示例3：消息队列系统
```cpp
#include <iostream>
#include <queue>
#include <string>
using namespace std;

struct Message {
    string content;
    int priority;
    
    Message(string c, int p) : content(c), priority(p) {}
};

int main() {
    queue<Message> messageQueue;
    
    // 添加消息到队列
    messageQueue.push(Message("用户登录", 1));
    messageQueue.push(Message("数据更新", 2));
    messageQueue.push(Message("系统通知", 3));
    messageQueue.push(Message("错误报告", 1));
    
    // 处理消息
    cout << "消息处理开始..." << endl;
    while (!messageQueue.empty()) {
        Message msg = messageQueue.front();
        cout << "处理消息: " << msg.content 
             << " (优先级: " << msg.priority << ")" << endl;
        messageQueue.pop();
    }
    
    cout << "所有消息处理完成！" << endl;
    return 0;
}
```

## 在算法竞赛中的应用

### 应用1：BFS算法模板
```cpp
#include <iostream>
#include <queue>
#include <vector>
using namespace std;

void BFS(vector<vector<int>>& graph, int start) {
    int n = graph.size();
    vector<bool> visited(n, false);
    queue<int> q;
    
    q.push(start);
    visited[start] = true;
    
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        cout << "访问节点: " << node << endl;
        
        for (int neighbor : graph[node]) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                q.push(neighbor);
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
    
    BFS(graph, 0);
    return 0;
}
```

### 应用2：层序遍历二叉树
```cpp
#include <iostream>
#include <queue>
using namespace std;

// 二叉树节点定义
struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

void levelOrder(TreeNode* root) {
    if (root == nullptr) return;
    
    queue<TreeNode*> q;
    q.push(root);
    
    cout << "层序遍历: ";
    while (!q.empty()) {
        TreeNode* current = q.front();
        q.pop();
        cout << current->val << " ";
        
        if (current->left != nullptr) {
            q.push(current->left);
        }
        if (current->right != nullptr) {
            q.push(current->right);
        }
    }
    cout << endl;
}

int main() {
    // 构建示例二叉树:
    //       1
    //      / \
    //     2   3
    //    / \   \
    //   4   5   6
    
    TreeNode* root = new TreeNode(1);
    root->left = new TreeNode(2);
    root->right = new TreeNode(3);
    root->left->left = new TreeNode(4);
    root->left->right = new TreeNode(5);
    root->right->right = new TreeNode(6);
    
    levelOrder(root);
    
    // 输出: 层序遍历: 1 2 3 4 5 6
    
    return 0;
}
```

### 应用3：模拟排队系统
```cpp
#include <iostream>
#include <queue>
#include <string>
#include <ctime>
#include <cstdlib>
using namespace std;

struct Customer {
    string name;
    int arrivalTime;
    
    Customer(string n, int t) : name(n), arrivalTime(t) {}
};

int main() {
    queue<Customer> bankQueue;
    int currentTime = 0;
    
    // 模拟顾客到达
    srand(time(0));
    for (int i = 1; i <= 5; i++) {
        string customerName = "顾客" + to_string(i);
        bankQueue.push(Customer(customerName, currentTime));
        cout << "时间 " << currentTime << ": " << customerName << " 进入队列" << endl;
        currentTime += rand() % 3 + 1; // 随机间隔1-3分钟
    }
    
    // 模拟服务过程
    cout << "\n开始服务..." << endl;
    int serviceTime = 0;
    while (!bankQueue.empty()) {
        Customer current = bankQueue.front();
        bankQueue.pop();
        
        int waitTime = serviceTime - current.arrivalTime;
        cout << "时间 " << serviceTime << ": 服务 " << current.name 
             << " (等待了 " << waitTime << " 分钟)" << endl;
        
        serviceTime += 2; // 每个顾客服务2分钟
    }
    
    cout << "所有顾客服务完成！" << endl;
    return 0;
}
```

## 特殊用法和技巧

### 使用其他容器作为底层
```cpp
#include <iostream>
#include <queue>
#include <list>
using namespace std;

// 使用list作为queue的底层容器
queue<int, list<int>> q;

// 这样可以在需要时使用list的特性，但queue的接口不变
q.push(1);
q.push(2);
cout << q.front() << endl;  // 1
```

### 队列交换
```cpp
queue<int> q1, q2;
q1.push(1);
q1.push(2);
q2.push(3);

q1.swap(q2);  // 交换两个队列的内容

cout << q1.front() << endl;  // 3
cout << q2.front() << endl;  // 1
```

### 自定义队列元素
```cpp
#include <iostream>
#include <queue>
using namespace std;

struct Task {
    string name;
    int priority;
    
    // 重载<运算符用于优先级比较（如果需要优先级队列）
    bool operator<(const Task& other) const {
        return priority < other.priority;
    }
};

int main() {
    queue<Task> taskQueue;
    
    taskQueue.push({"数据分析", 2});
    taskQueue.push({"系统备份", 1});
    taskQueue.push({"用户反馈", 3});
    
    while (!taskQueue.empty()) {
        Task task = taskQueue.front();
        cout << "执行任务: " << task.name << " (优先级: " << task.priority << ")" << endl;
        taskQueue.pop();
    }
    
    return 0;
}
```

## 性能注意事项

### 时间复杂度
- **push**：O(1)
- **pop**：O(1)  
- **front/back**：O(1)
- **size/empty**：O(1)

### 空间复杂度
- O(n)

## 与 deque 和 stack 的对比

| 特性 | queue | deque | stack |
|------|-------|-------|-------|
| 数据结构和原则 | 队列，FIFO | 双端队列，两端操作 | 栈，LIFO |
| 主要操作 | push, pop, front, back | push_front, push_back, pop_front, pop_back | push, pop, top |
| 随机访问 | 不支持 | 支持 | 不支持 |
| 迭代器 | 不支持 | 支持 | 不支持 |

## 常见错误

### 错误1：空队列访问
```cpp
queue<int> q;
// cout << q.front() << endl;  // ❌ 未定义行为，队列为空
// q.pop();                    // ❌ 未定义行为，队列为空

// ✅ 正确做法
if (!q.empty()) {
    cout << q.front() << endl;
    q.pop();
}
```

### 错误2：误解FIFO原则
```cpp
queue<int> q;
q.push(1);
q.push(2);
q.push(3);

// 有些人错误地认为back是下一个要处理的元素
// ❌ 错误理解：认为q.back()是下一个
// ✅ 正确：q.front()才是下一个要处理的元素
```

## 总结

`queue` 是**先进先出队列**，核心特点：
- 元素先进先出（FIFO）
- 只能在队尾添加，队首删除
- 不支持随机访问和遍历
- 操作简单高效

**适用场景：**
- 需要按顺序处理任务
- 广度优先搜索（BFS）
- 消息队列等生产者-消费者模型
- 模拟排队系统

**记忆口诀：**
> "queue队列，先进先出，队尾添加，队首删除"