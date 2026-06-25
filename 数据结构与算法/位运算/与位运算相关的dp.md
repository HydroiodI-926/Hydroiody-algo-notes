# [P4310 绝世好题 - 洛谷](https://www.luogu.com.cn/problem/P4310)
自己的解 超时 
```cpp
int n;

void solve() {
	cin>>n;
	vector<int> a(n+1);
	vector<int> dp(n+1,0);
	for(int i=1;i<=n;i++){
		cin>>a[i];
	}
	int ans=0;
	for(int i=1;i<=n;i++){
		if(a[i]!=0){
			dp[i]=1;
			for(int j=1;j<i;j++){
				if((a[j]&a[i])!=0){
					dp[i]=max(dp[i],dp[j]+1);
				}
			}
			ans=max(ans,dp[i]);
		} 
	}
	cout<<ans;
}
```