## 基本介绍
`deque` 是 C++ 中的**双端队列容器**，它允许在**头部和尾部**都进行高效的插入和删除操作，就像排队时可以从前端或后端加入或离开一样。

## 头文件
```cpp
#include <deque>
using namespace std;
```

## 声明和初始化
```cpp
// 各种创建方式
deque<int> dq1;                    // 空deque
deque<int> dq2(5);                 // 5个元素，都是0
deque<int> dq3(5, 10);             // 5个元素，都是10
deque<int> dq4 = {1, 2, 3, 4, 5};  // 直接初始化
deque<int> dq5(dq4);               // 拷贝构造
deque<int> dq6(dq4.begin(), dq4.begin() + 3); // 通过迭代器范围初始化
```

## 核心特性

### 优点：
- ✅ **双端操作**：头尾都能高效插入删除
- ✅ **随机访问**：支持下标访问，像vector一样
- ✅ **动态扩容**：自动管理内存
- ✅ **迭代器支持**：支持各种迭代器操作

### 缺点：
- ❌ **内存不连续**：比vector稍慢的随机访问
- ❌ **内存开销**：比vector稍大

## 常用操作详解

### 头部和尾部操作
```cpp
deque<int> dq;

// 尾部操作（类似vector）
dq.push_back(1);     // deque: [1]
dq.push_back(2);     // deque: [1, 2]
dq.push_back(3);     // deque: [1, 2, 3]

// 头部操作（deque特有）
dq.push_front(0);    // deque: [0, 1, 2, 3]
dq.push_front(-1);   // deque: [-1, 0, 1, 2, 3]

// 访问头尾元素
cout << dq.front() << endl;  // -1（头部元素）
cout << dq.back() << endl;   // 3（尾部元素）

// 删除头尾元素
dq.pop_front();      // 删除头部，deque: [0, 1, 2, 3]
dq.pop_back();       // 删除尾部，deque: [0, 1, 2]
```

### 随机访问
```cpp
deque<int> dq = {10, 20, 30, 40, 50};

// 像数组一样使用下标访问
cout << dq[0] << endl;    // 10
cout << dq[2] << endl;    // 30
dq[1] = 25;               // 修改元素：deque: [10, 25, 30, 40, 50]

// 使用at（安全，会检查边界）
cout << dq.at(3) << endl; // 40
```

### 插入和删除
```cpp
deque<int> dq = {1, 2, 3, 4, 5};

// 在指定位置插入
auto it = dq.begin() + 2;
dq.insert(it, 99);        // 在索引2处插入99：deque: [1, 2, 99, 3, 4, 5]

// 插入多个元素
dq.insert(dq.begin() + 1, 3, 88); // 在索引1处插入3个88

// 删除指定位置元素
dq.erase(dq.begin() + 1);         // 删除索引1处的元素

// 删除范围元素
dq.erase(dq.begin() + 2, dq.begin() + 4); // 删除索引2到4之间的元素
```

### 容量和信息
```cpp
deque<int> dq = {1, 2, 3, 4, 5};

cout << dq.size() << endl;     // 5（元素个数）
cout << dq.empty() << endl;    // false（是否为空）

// 调整大小
dq.resize(8);                  // 大小变为8，新元素为0
dq.resize(3);                  // 大小变为3，删除多余元素

// 清空deque
dq.clear();                    // 清空所有元素
```

## 遍历方法
```cpp
deque<int> dq = {1, 2, 3, 4, 5};

// 方法1：下标遍历
for (int i = 0; i < dq.size(); i++) {
    cout << dq[i] << " ";
}

// 方法2：范围for循环（推荐）
for (int value : dq) {
    cout << value << " ";
}

// 方法3：迭代器遍历
for (auto it = dq.begin(); it != dq.end(); it++) {
    cout << *it << " ";
}

// 方法4：反向迭代器
for (auto it = dq.rbegin(); it != dq.rend(); it++) {
    cout << *it << " ";  // 逆序输出
}
```

## 实际应用示例

### 示例1：滑动窗口最大值
```cpp
#include <iostream>
#include <deque>
#include <vector>
using namespace std;

vector<int> maxSlidingWindow(vector<int>& nums, int k) {
    vector<int> result;
    deque<int> dq;  // 存储索引
    
    for (int i = 0; i < nums.size(); i++) {
        // 移除超出窗口范围的元素
        if (!dq.empty() && dq.front() == i - k) {
            dq.pop_front();
        }
        
        // 移除比当前元素小的元素
        while (!dq.empty() && nums[dq.back()] < nums[i]) {
            dq.pop_back();
        }
        
        // 添加当前元素索引
        dq.push_back(i);
        
        // 当窗口形成时，记录最大值
        if (i >= k - 1) {
            result.push_back(nums[dq.front()]);
        }
    }
    
    return result;
}

int main() {
    vector<int> nums = {1, 3, -1, -3, 5, 3, 6, 7};
    int k = 3;
    
    vector<int> result = maxSlidingWindow(nums, k);
    
    cout << "滑动窗口最大值: ";
    for (int val : result) {
        cout << val << " ";  // 输出: 3 3 5 5 6 7
    }
    cout << endl;
    
    return 0;
}
```

### 示例2：任务调度系统
```cpp
#include <iostream>
#include <deque>
#include <string>
using namespace std;

struct Task {
    string name;
    int priority;
    
    Task(string n, int p) : name(n), priority(p) {}
};

int main() {
    deque<Task> taskQueue;
    
    // 添加高优先级任务到头部
    taskQueue.push_front(Task("紧急修复", 1));
    taskQueue.push_front(Task("系统告警", 1));
    
    // 添加普通任务到尾部
    taskQueue.push_back(Task("日常备份", 3));
    taskQueue.push_back(Task("数据清理", 2));
    taskQueue.push_back(Task("生成报告", 3));
    
    // 处理任务
    cout << "任务处理顺序:" << endl;
    while (!taskQueue.empty()) {
        Task task = taskQueue.front();
        cout << "执行: " << task.name << " (优先级: " << task.priority << ")" << endl;
        taskQueue.pop_front();
    }
    
    return 0;
}
```

### 示例3：回文检查
```cpp
#include <iostream>
#include <deque>
#include <string>
#include <cctype>
using namespace std;

bool isPalindrome(const string& str) {
    deque<char> dq;
    
    // 将字符添加到deque中（忽略空格和标点）
    for (char c : str) {
        if (isalnum(c)) {
            dq.push_back(tolower(c));
        }
    }
    
    // 从两端比较字符
    while (dq.size() > 1) {
        if (dq.front() != dq.back()) {
            return false;
        }
        dq.pop_front();
        dq.pop_back();
    }
    
    return true;
}

int main() {
    string test1 = "A man a plan a canal Panama";
    string test2 = "race a car";
    
    cout << "\"" << test1 << "\" 是回文: " << (isPalindrome(test1) ? "是" : "否") << endl;
    cout << "\"" << test2 << "\" 是回文: " << (isPalindrome(test2) ? "是" : "否") << endl;
    
    return 0;
}
```

## 在算法竞赛中的应用

### 应用1：BFS with deque（0-1 BFS）
```cpp
#include <iostream>
#include <deque>
#include <vector>
using namespace std;

const int INF = 1e9;

int zeroOneBFS(vector<vector<pair<int, int>>>& graph, int start, int end) {
    int n = graph.size();
    vector<int> dist(n, INF);
    deque<int> dq;
    
    dist[start] = 0;
    dq.push_front(start);
    
    while (!dq.empty()) {
        int u = dq.front();
        dq.pop_front();
        
        for (auto& [v, weight] : graph[u]) {
            if (dist[v] > dist[u] + weight) {
                dist[v] = dist[u] + weight;
                
                if (weight == 0) {
                    dq.push_front(v);  // 权重0，加入前端
                } else {
                    dq.push_back(v);   // 权重1，加入后端
                }
            }
        }
    }
    
    return dist[end];
}

int main() {
    // 示例图：节点0->1(权重0), 0->2(权重1), 1->3(权重1), 2->3(权重0)
    int n = 4;
    vector<vector<pair<int, int>>> graph(n);
    graph[0].push_back({1, 0});
    graph[0].push_back({2, 1});
    graph[1].push_back({3, 1});
    graph[2].push_back({3, 0});
    
    int result = zeroOneBFS(graph, 0, 3);
    cout << "最短路径长度: " << result << endl;  // 输出: 1
    
    return 0;
}
```

### 应用2：维护滑动窗口
```cpp
#include <iostream>
#include <deque>
#include <vector>
using namespace std;

vector<int> slidingWindowMinimum(vector<int>& nums, int k) {
    vector<int> result;
    deque<int> dq;  // 存储索引
    
    for (int i = 0; i < nums.size(); i++) {
        // 移除超出窗口范围的元素
        if (!dq.empty() && dq.front() == i - k) {
            dq.pop_front();
        }
        
        // 维护deque单调递增
        while (!dq.empty() && nums[dq.back()] > nums[i]) {
            dq.pop_back();
        }
        
        // 添加当前元素索引
        dq.push_back(i);
        
        // 当窗口形成时，记录最小值
        if (i >= k - 1) {
            result.push_back(nums[dq.front()]);
        }
    }
    
    return result;
}

int main() {
    vector<int> nums = {4, 2, 1, 5, 3, 6, 2, 8};
    int k = 3;
    
    vector<int> result = slidingWindowMinimum(nums, k);
    
    cout << "滑动窗口最小值: ";
    for (int val : result) {
        cout << val << " ";  // 输出: 1 1 1 3 2 2
    }
    cout << endl;
    
    return 0;
}
```

## 特殊用法和技巧

### 性能优化
```cpp
// 如果知道大概大小，可以预分配（但deque是分段连续的，效果不如vector明显）
deque<int> dq;
// 没有reserve方法，但可以预先插入元素
dq.resize(1000);  // 预分配1000个元素的空间
```

### 与vector的性能对比
```cpp
#include <iostream>
#include <deque>
#include <vector>
#include <chrono>
using namespace std;

void testPerformance() {
    const int N = 100000;
    
    // 测试vector头部插入
    vector<int> vec;
    auto start = chrono::high_resolution_clock::now();
    for (int i = 0; i < N; i++) {
        vec.insert(vec.begin(), i);  // vector头部插入很慢
    }
    auto end = chrono::high_resolution_clock::now();
    cout << "Vector 头部插入时间: " 
         << chrono::duration_cast<chrono::milliseconds>(end - start).count() 
         << " ms" << endl;
    
    // 测试deque头部插入
    deque<int> dq;
    start = chrono::high_resolution_clock::now();
    for (int i = 0; i < N; i++) {
        dq.push_front(i);  // deque头部插入很快
    }
    end = chrono::high_resolution_clock::now();
    cout << "Deque 头部插入时间: " 
         << chrono::duration_cast<chrono::milliseconds>(end - start).count() 
         << " ms" << endl;
}

int main() {
    testPerformance();
    return 0;
}
```

## 性能注意事项

### 时间复杂度
- **push_front/pop_front**：O(1)
- **push_back/pop_back**：O(1)
- **随机访问**：O(1)
- **中间插入删除**：O(n)

### 空间复杂度
- O(n)，比vector稍大

## 与vector和list的对比

| 特性 | deque | vector | list |
|------|-------|--------|------|
| 头部插入删除 | O(1) | O(n) | O(1) |
| 尾部插入删除 | O(1) | O(1) | O(1) |
| 中间插入删除 | O(n) | O(n) | O(1) |
| 随机访问 | O(1) | O(1) | O(n) |
| 内存布局 | 分段连续 | 完全连续 | 完全不连续 |

## 常见错误

### 错误1：迭代器失效
```cpp
deque<int> dq = {1, 2, 3, 4, 5};
auto it = dq.begin() + 2;

// 在中间插入可能使迭代器失效
dq.insert(dq.begin() + 1, 99);
// ❌ it可能已经失效
// cout << *it << endl;  // 未定义行为

// ✅ 正确：重新获取迭代器
it = dq.begin() + 3;
cout << *it << endl;  // 安全
```

### 错误2：边界检查
```cpp
deque<int> dq = {1, 2, 3};

// ❌ 可能越界
// cout << dq[5] << endl;  // 未定义行为

// ✅ 使用at进行安全访问
try {
    cout << dq.at(5) << endl;  // 抛出异常
} catch (const out_of_range& e) {
    cout << "下标越界" << endl;
}
```

## 总结

`deque` 是**双端队列**，核心特点：
- 头部和尾部都能高效插入删除
- 支持随机访问
- 内存分段连续
- 功能介于vector和list之间

**适用场景：**
- 需要频繁在头部和尾部操作
- 需要随机访问
- 滑动窗口类问题
- 0-1 BFS算法

**记忆口诀：**
> "deque双端队，头尾都能进和退，随机访问也支持，功能强大真给力"
