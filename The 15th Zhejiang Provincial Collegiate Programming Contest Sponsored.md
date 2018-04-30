### A Peak

#### 题意：  
**给出一串数字，然后判断是不是先增后减**

#### 思路：  
**两个变量交替，flag判断开始是否是上升，如果不是，那么就不成立，up=1代表现在是上升区段，反之，递减区段，ans记录段数**
```
#include <iostream>
#include<algorithm>

using namespace std;

int main() {
    int t, n;
    cin >> t;
    while (t--) {
        int flag = 0, up = 0, a, b, ans = 0;
        cin >> n;
        cin >> a >> b;
        if(a < b) flag = 1;
        if(flag) up = 1;
        a = b;
        for(int i = 3; i <= n; i++) {
            cin >> b;
            if(up && a > b) {
                up = 0;
                ans++;
            }
            if(!up && a < b) {
                flag = 0;
            }
            if(a == b) flag = 0;
            a = b;
        }
        if(flag && ans==1) cout << "Yes" << endl;
        else cout << "No" << endl;
    }
    return 0;
}
```

### B King of Karaoke

#### 题意：  
**两个数组，如果数组1加上一个K，使两数组相同数字个数最多**

#### 思路：  
**两数组部分元素的差值相同，那么这部分数字可以加上K相等，那么只需计算差值相同数的个数，用map存储**

```
#include<iostream>
#include<map>
#include<cstdio>
using namespace std;
const int maxn = 100000+10;
int a[maxn];
int main() {
    int t;
    scanf("%d" ,&t);
    while(t--) {
        int n, x, ans = 0;
        map<int ,int > mp;
        scanf("%d", &n);
        for(int i = 1; i <= n; i++) {
            scanf("%d", &a[i]);
        }
        for(int i = 1; i <= n; i++) {
            scanf("%d", &x);
            mp[a[i]-x] += 1;
            if(mp[a[i]-x] > ans) ans = mp[a[i]-x];
        }
        printf("%d\n", ans);
    }
    return 0;
}
```

### D Sequence Swapping

#### 题意：  
**每次交换一对'(' ')'，得到分数为括号对应的分数的乘积，最大多少分**

#### 思路：  
**dp，这道题要求只能左括号和右括号换，这就决定了每个左括号能到达的最右位置，如果这个括号可以被移动到第j个位置，那么他后面的左括号一定都在比他右的位置，也就是j+1位之后，设dp[i][j]是把第i个左括号移动到j右面（包括j）位中能得到的最大价值
转移方程为：  **

**dp[i][j]=max(dp[i+1][j+1]+移动到这里能得到的值，dp[i][j+1])(可以直接开始，因为dp[i+1][j+1]一开始是0所以无影响)而移动到这里所得到的值是（移动的这个左括号的值）\*（他交换的右括号的值的和），用前缀和表示前多少个右括号的和，在输入的时候记录它前面有几个右括号，他所在位减去他右边的左括号就是他右边的右括号的值数量。由此可知：右括号的值的和=前缀和（括号总数量-右边括号）-前缀和（左边的右括号）**

```
#include<bits/stdc++.h>

#define LL long long
using namespace std;
const int maxn = 1005;
LL dp[maxn][maxn], kuo[maxn], val[maxn], wei[maxn], zhi[maxn], qi[maxn];
char chuan[maxn];

int main() {
    LL a, k;
    while (~scanf("%lld", &a)) {
        while (a--) {
            scanf("%lld", &k);
            scanf("%s", chuan);
            LL ru = 1;
            kuo[0] = 0;
            LL ru1 = 1;
            for (LL i = 0; i < k; i++) {
                scanf("%lld", &val[i]);
                if (chuan[i] == ')') {
                    kuo[ru] = kuo[ru - 1] + val[i];
                    ru++;
                } else {
                    wei[ru1] = i;
                    zhi[ru1] = val[i];
                    qi[ru1] = ru - 1;
                    ru1++;
                }
            }
            ru1--;
            ru--;
            memset(dp, 0, sizeof(dp));
            LL jie = 0;
            for (LL i = ru1; i >= 1; i--) {
                LL zuo = ru1 - i;
                for (LL j = k - 1 - zuo; j >= wei[i]; j--) {
                    LL hou = k - 1 - j - zuo;
                    LL de = kuo[ru - hou] - kuo[qi[i]];
                    if (j == k - 1 - zuo)dp[i][j] = zhi[i] * de + dp[i + 1][j + 1];
                    else dp[i][j] = max(zhi[i] * de + dp[i + 1][j + 1], dp[i][j + 1]);
                    if (i == 1) {
                        if (dp[i][j] > jie)jie = dp[i][j];
                    }
                }
                for (LL j = wei[i] - 1; j >= wei[i - 1]; j--) {
                    dp[i][j] = dp[i][j + 1];
                    if (i == 1) {
                        if (dp[i][j] > jie)jie = dp[i][j];
                    }
                }
            }
            printf("%lld\n", jie);
        }
    }
    return 0;
}  
```
#### J CONTINUE...?

#### 题意：  
**给出一个01串，0代表女生，1代表男生，从左往右每人拿着宝石，并且递增，从1开始**

#### 思路：  
**判断人数，如果宝石总数是奇数，那么一定不行，然后判断人的个数，如果是偶数可以一一配对，如果是奇数，有一个只能是0，x的组合，根据奇偶分配组，分配一半，另一半相对应分配**

```
#include <algorithm>
#include <cstdio>
#include <iostream>
using namespace std;
const int maxn = 100010;
int ans[maxn], a[maxn];
char now[maxn];
int main() {
    int T, n, boy,girl, i;
    int N;
    while(~scanf("%d", &T)) {
        while(T--){
            scanf("%d", &n);
            scanf("%s", now+1);
            for(int i = 1; i <= n; i++){
                a[i] = now[i] - '0';
            }
            if((((n+1)*n)/2) % 2 == 1){
                printf("-1\n");
                continue;
            }
            if(n % 2 == 1){
                i = 0, N = n;
            } else{
                i = 1, N = n+1;
            }
            for(; i <= N/2; i++){
                if(i % 2 == 0){
                    boy = 3;
                    girl = 1;
                }
                else{
                    boy = 4;
                    girl = 2;
                }
                if(a[i] == 0) ans[i] = girl;
                else ans[i] = boy;
                if(a[N-i] == 0) ans[N-i] = girl;
                else ans[N-i] = boy;
            }
            for(int i=1; i<=n; i++)
                printf("%d", ans[i]);
            printf("\n");
        }
    }
    return 0;
}
```

### K	Mahjong Sorting

#### 题意：

#### 思路：

```
#include <algorithm>
#include <iostream>

using namespace std;
const int maxn = 100010;
int a[maxn * 3];
int t;
int n, m;

int main() {
    cin >> t;
    while (t--) {
        cin >> n >> m;
        a[n] = 3 * m + 1;
        int index = -1;
        for (int i = 0; i < n; i++) {
            char c;
            int k;
            cin >> c;
            if (c == 'C') {
                cin >> k;
                a[i] = k;
            } else if (c == 'B') {
                cin >> k;
                a[i] = m + k;
            } else if (c == 'D') {
                cin >> k;
                a[i] = m * 2 + k;
            } else {
                index = i;
            }
        }
        if (n == 1) {
            cout << 3 * m << endl;
        } else {
            if (index == -1) {
                if (a[0] > a[1]) {
                    cout << 1 << endl;
                } else {
                    cout << 3 * m - n + 1 << endl;
                }
            } else {
                if (index == 0) {
                    cout << a[1] - 1 << endl;
                } else {
                    if (a[0] > a[1] && index != 1) {
                        cout << 1 << endl;
                    } else {
                        int sum = a[index + 1] - a[index - 1] - 1;
                        if (index == 1) {
                            sum++;
                        }
                        cout << sum << endl;
                    }
                }
            }
        }
    }
    return 0;
}
```

### L	Doki Doki Literature Club

#### 题意：
**给出几个词语，还有分数，选出部分单词，并根据公式，计算出分数，要求分数最高**

#### 思路：  
**分数越高应该越早输出，那么根据分数排序，如果分数一样如何选择单词呢，从样例里可以看出根据字典序输出的，那么只需要根根据字典序排序排序输出，并计算分数即可**

```
#include <cstdio>
#include<string>
#include<algorithm>
#include <iostream>

using namespace std;
const int maxn = 100 + 10;
typedef long long ll;

struct Node {
    string s;
    ll v;
} node[maxn];

bool cmd(Node a, Node b) {
    if (a.v == b.v) return a.s.compare(b.s);
    else return a.v > b.v;
}

int main() {
    int t;
    while (cin >> t) {
        while (t--) {
            int n, m;
            cin >> n >> m;
            for (int i = 0; i < n; i++) {
                cin >> node[i].s >> node[i].v;
            }
            sort(node, node + n, cmd);
            ll ans = 0;
            for (int i = 0; i < m; i++) {
                ans += (m - i) * node[i].v;
            }
            cout << ans << " ";
            for (int i = 0; i < m - 1; i++) {
                cout << node[i].s << " ";
            }
            cout << node[m - 1].s << endl;
        }
    }
    return 0;
}
```


### M	Lucky 7

#### 题意：  
**给出n， b和一组数，如果在n个数中有一个加上b可以被7整除，就输出yes**

#### 思路： 

**暴力**

```
#include <cstdio>

using namespace std;

int main() {
    int t;
    while (~scanf("%d", &t)) {
        while (t--) {
            int n, b;
            scanf("%d%d", &n, &b);
            int x, flag = 0;
            for (int i = 0; i < n; i++) {
                scanf("%d", &x);
                if ((x + b) % 7 == 0) flag = 1;
            }
            if (flag == 1) printf("Yes\n");
            else printf("No\n");
        }
    }
    return 0;
}
```































