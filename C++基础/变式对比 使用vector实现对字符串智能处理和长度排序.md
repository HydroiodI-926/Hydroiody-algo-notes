```cpp
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>

using namespace std;

// 自定义排序函数
bool lengthCompare(const string& a, const string& b) {
    if (a.length() == b.length()) {
        // 长度相同时保持原顺序（不排序）
        return false; // 返回 false 表示不改变相对顺序
    }
    return a.length() < b.length(); // 按长度升序
}

int main() {
    vector<string> words = {
        "apple", "banana", "cat", "dog", "elephant", "fox"
    };
    
    // 使用稳定排序保持相等元素的相对顺序
    stable_sort(words.begin(), words.end(), lengthCompare);
    
    cout << "按长度排序（长度相同保持原顺序）: ";
    for (const auto& word : words) {
        cout << word << "(" << word.length() << ") ";
    }
    cout << endl;
    
    return 0;
}
```
