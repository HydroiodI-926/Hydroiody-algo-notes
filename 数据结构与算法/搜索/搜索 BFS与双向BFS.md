## BFS
bfs就像是二叉树的层序遍历（由上到下，由左到右）从根节点开始先从左到右遍历完每层的所有元素再到下一层，如此类推；拓展到图，队列可以严格保证每当输出一个元素时，把子节点的所有元素按一定顺序排列储存，输出下一个元素时又会把下一个元素的子节点按顺序储存，且这些子节点一定排在上一个输出元素的子节点之后，这样，队列中的元素排列的层序的确定的，每个层中的元素排列也是按顺序的
**特点**：逐层扩散
## 模板
```cpp
vector<vector<int>> adj;
vector<bool> vis; // 用于标记节点防止重复标记导致mle

void bfs(int start,vector<vector<int>>& adj,vector<bool>& vis){
	queue<int> q;
	vis[start]=1; // 一定要入队列的时候就标记而不是等到出队列在标记
	q.push(start); 
	while(!q.empty()){
		int u=q.front();
		cout<<u<<" ";
		q.pop();
		for(int v:adj[u]){
			if(!vis[v]){
				vis[v]=1;
				q.push(v);
			}
		}
	}
}

void solve() {
    int n,m,s;
    cin>>n>>m>>s;
    adj.assign(n+1,{});
    vis.assign(n+1,0);
    for(int i=0;i<m;i++){
    	int u,v;
    	cin>>u>>v;
    	adj[u].push_back(v);
    	adj[v].push_back(u);
    }
    bfs(s,adj,vis);
}
```
## 剪枝设计
1. vis数组标记已经入队过，防止重复入队列，同时也是必要的剪枝手段。在一个节点入队列的时候就标记，而不是出队列的时候标记，比如一个节点如果入度较多的话就会重复入队，重复入队的



## 例题
### 迷宫最短路