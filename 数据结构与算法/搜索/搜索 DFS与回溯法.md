
# 区分剪枝和过滤
## 过滤
本质上是所有可能走过一遍，然后对结果进行检测，排除掉不需要的或者重复的内容
## 剪枝
假设测试数据量很庞大，所有可能都走一遍会超时，这时候就不能只通过过滤解决问题，需要人为预测哪些可能不能走，减少计算量
## 例题
### DFS寻找所有子序列
### DFS实现全组合（指数型枚举）o(2^n)
[B3622 枚举子集（递归实现指数型枚举） - 洛谷](https://www.luogu.com.cn/problem/B3622#ide)
```cpp
int n;
string s="NY";
void dfs(string t,int idx){
	if(idx==n){
		cout<<t<<"\n";
		return;
	}
	else{
		for(char &c:s){
			t[idx]=c;
			dfs(t,idx+1);
		}
	}
}

void solve(){
	cin>>n;
	string res(n,0);
}
```
[P10448 组合型枚举 - 洛谷](https://www.luogu.com.cn/problem/P10448#ide)
```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int MAXN=1e6;

int n,k;
vector<bool> vis;
vector<int> orders;
void dfs(int idx,int pre){
	if(idx==k){
		for(int v:orders){
			cout<<v<<" ";
		}
		cout<<'\n';
		return;
	}
	for(int i=pre+1;i<=n;i++){
		if(!vis[i]){
			vis[i]=true;
			orders[idx]=i;
			dfs(idx+1,i);
			vis[i]=false;
		}
	}
}

void solve(){
	cin>>n>>k;
	vis.resize(n+1,0);
	orders.assign(k,0);
	dfs(0,0);
}

```
### DFS实现全排列（排列型枚举）o(n\*n!)
[B3623 枚举排列（递归实现排列型枚举） - 洛谷](https://www.luogu.com.cn/problem/B3623)
```cpp
int n,k;
vector<bool> vis;
vector<int> orders;
void dfs(int idx){
	if(idx==k){
		for(int v:orders){
			cout<<v<<" ";
		}
		cout<<'\n';
		return;
	}
	for(int i=1;i<=n;i++){
		if(!vis[i]){
			vis[i]=true;
			orders[idx]=i;
			dfs(idx+1);
			vis[i]=false;
		}
	}
}

void solve(){
	cin>>n>>k;
	vis.resize(n+1,0);
	orders.assign(k,0);
	dfs(0);
}
```
