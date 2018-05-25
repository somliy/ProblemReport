##  Codeforces Round \#479 (Div. 3)

---
### A. Wrong Subtraction
**题意：给出一个数字n，然后进行k次操作，每次在n的个位上减1，如果个位是0的话，去掉这一位，继续操作，输出最后结果**  

**思路：模拟减1操作，直接用个位数字与k比较，如果n大，那么输出n-k，否则需要判断是否能把个位去掉**
```
#include<iostream>
#include<cstdio>
using namespace std;
typedef long long ll;

int main() {
	ll n, k;
	int ans = -1;
	scanf("%lld %lld", &n, &k);
	while(k > 0) {
		int x = n % 10;
		if(x > k) {
			ans = n - k;
			k = 0;
		}else {
			if(x == k) {
				ans = n - x;
				k = 0;
			}else {
				k = k - x - 1;
				n /= 10;
				ans = n;
			}
		}
	}
	printf("%d\n", ans);
	return 0;
}

```
### B. Two-gram
**题意：给出一字符串，找出其中出现最多的两个相邻的字母**    

**思路：用string枚举所有的可能，循环判断次数**
```
#include<iostream>
#include<cstdio>
#include<cstring>
#include<map>
using namespace std;
typedef long long ll;

map<string, int> mp;

int main() {
	int n, maxx = -1;
	cin >> n;
	string s, ans, p;
	cin >> s;
	for(int i = 0; i < n-1; i++) {
		p = s.substr(i,2);
		mp[p]++;
		if(mp[p] > maxx) {
			maxx = mp[p];
			ans = p;
		}
	}
	cout << ans << endl;
	return 0;
}

```

### C. Less or Equal
**题意：给出n，k和一组数字（无大小顺序），从n个数中找出k个数字，输出一个x，x是大于等于k个数字中最大的，如果没有输出-1**    

**思路：首先排序，因为要找k个数字，查看第k个数字是否跟k+1一样大，如果一样大就找不出k个（至少k+1个）  
还有一个问题，当k为0的情况，因为$1<=a_{i}<=10^{9}$是从1开始的，当k=0时，要找出0个，如果排完续后$a_{1}=1的话，就不可能找出x，输出-1
否则输出1**
```
#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<map>
using namespace std;
typedef long long ll;
const ll maxn = 1e6+5;
ll a[maxn];

int main() {
	int n, m;
	scanf("%d %d", &n, &m);
	for(int i = 0; i < n; i++) {
		scanf("%I64d", &a[i]);
	}
	sort(a,a+n);
	if(n == m) {
		printf("%I64d", a[m-1]);
		return 0;
	}
	if(m == 0) {
		if(a[0] == 1) printf("-1\n");
		else printf("1\n");
	}else {
		if(a[m] == a[m-1]) printf("-1\n");
		else printf("%I64d\n", a[m-1]);
	}

	return 0;
}

```
### D. Divide by three, multiply by two
**题意：给出一组数字，按照要求排序，规则是后面的数只能由前面的数÷3或者×2得来，题目一定可解**    

**思路：根据一个排序规则排序即可，根据每个数字对3能取余的次数，从大到小排序，如果一样的话，再根据剩下数字的从小到大排序      原因：要把能把3除尽的大的数字尽可能放前面，使他们变小，除了能÷3的数字，就是偶数，需要从小到大排，因为×会变大**
```
//
// Created by 486 on 2018/5/24.
//

#include<iostream>
#include<vector>
#include<cstring>
#include<algorithm>

#define INIT ios::sync_with_stdio(false)
using namespace std;
typedef long long ll;
const int maxn = 105;
ll a[maxn];
vector<ll> v;

bool cmp(ll a, ll b) {
    int numa = 0, numb = 0;
    while (a % 3 == 0) {
        a /= 3;
        numa++;
    }
    while (b % 3 == 0) {
        b /= 3;
        numb++;
    }
    if (numb != numa) return numa > numb;
    return a < b;
}

int main() {
    INIT;
    int n;
    ll x;
    cin >> n;
    for (int i = 0; i < n; i++) {
        cin >> x;
        v.push_back(x);
    }
    sort(v.begin(), v.end(), cmp);
    for (int i = 0; i < n - 1; i++) {
        cout << v[i] << " ";
    }
    cout << v[n - 1] << endl;
    return 0;
}
```
### E. Cyclic Components
**题意：给出一个图，找出图中有多少个环，都是单环，没有多余的边**  

**思路：利用链表存储，存储两个方向，根据一个点开始搜索，变搜索边标记，如果每一个点可以连接另外两个点（一个进一个出），那么是符合要求的，如果不是，要么不是环，要么是一个复杂环**
```
#include<iostream>
#include<algorithm>
#include<cstring>
#include <map>

using namespace std;
const int maxn = 2e5+10;
vector <int> v[maxn];
bool vis[maxn];
int flag;


void dfs(int x) {
    vis[x] = true;
    if(v[x].size() != 2) flag = 1;
    for(int i : v[x]) {
        if(!vis[i])
            dfs(i);
    }
}

int main() {
    int n, m, ans = 0;
    cin >> n >> m;
    for(int i = 1; i <= m; i++) {
        int x, y;
        cin >> x >> y;
        v[x].push_back(y);
        v[y].push_back(x);
    }
    memset(vis, false, sizeof(vis));
    for(int i = 1; i <= n; i++) {
        flag = 0;
        if(!vis[i]) {
            dfs(i);
            if(!flag) ans++;
        }
    }
    cout << ans << endl;
    return 0;
}



```
### F. Consecutive Subsequence
**题意：给出一组数字，输出其中一组数的坐标，要求是找出最长的上升序列，每次递增1**  

**思路：用map记录一个数字前面连续数字的个数，中间记录最长的长度，和坐标，最后根据最后的数字跟长度可以推算出开始的数字，循环输出即可**
```
#include<iostream>
#include<algorithm>
#include<cstring>
#include <map>

using namespace std;

typedef long long ll;
const int maxn = 2e5+10;
int a[maxn];
map<int ,int > mp;


int main() {
    int n, ans, len = -1;
    cin >> n;
    for(int i = 1; i <= n; i++) {
        cin >> a[i];
        int x = a[i];
        mp[x] = mp[x-1] + 1;
        if(len < mp[x]) {
            len = mp[x];
            ans = x;
        }
    }
    cout << len << endl;
    int startNum = ans - len + 1;
    for(int i = 1; i <= n; i++) {
        if(a[i] == startNum) {
            cout << i << " ";
            startNum++;
        }
    }
    cout << endl;
    return 0;
}

```