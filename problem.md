# 第一次作业

# YY and Matrix

## ★题目描述

YY 有一个大小为 $n * m$ 的矩阵，现在要对矩阵进行q次操作，操作分为如下三种：

0 x y：交换矩阵的x、y两行。

1 x y：交换矩阵的x、y两列。

2 x y：求当前矩阵第x行第y列的元素。

## ★输入

第一行三个正整数n、m、q，表示矩阵大小和操作次数。

接下来n行，每行m个空格隔开的整数，表示矩阵的元素。

接下来q行，每行三个数op、x、y，表示上述操作中的一种。

对于80%的数据，1 <= n、m、q <= 1000。

对于100%的数据，1 <= n、m <= 1000,1 <= q <= 1000000,矩阵元素大小在int范围内且非负。

## ★输出

对于操作2，输出一个整数，表示对应的元素。

## ★输入样例

```in
  2 2 6
  1 2
  3 4
  0 1 2
  1 1 2
  2 1 1
  2 1 2
  2 2 1
  2 2 2  
```

## ★输出样例

```out
4
3
2
1
```

> 思路：
>
> 整行整列的变换实际上可以看作是对**矩阵行列编号的交换**O(1)。
>
> ​    做法：使用**两个一维向量**存储行列坐标
>
> 时间复杂度：O（q）

```c++
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;
const int maxn = 1005;

int n, m, q;
int a[maxn][maxn];
int row[maxn];
int col[maxn];

void init() {
	for (int i = 1; i < maxn; i++) {
		row[i] = i;
	}
	for (int j = 1; j < maxn; j++) {
		col[j] = j;
	}
}

int main() {
	
	init();
	
	cin >> n >> m >> q;
	for (int i = 1; i <= n; i++) {
		for (int j = 1; j <= m; j++) {
			cin >> a[i][j];
		}
	}
	
	while (q--) {
		int op, x, y;
		cin >> op >> x >> y;
		switch (op) {
			// switch row
			case 0:
				swap(row[x], row[y]);
				break;
			// switch col
			case 1:
				swap(col[x], col[y]);
				break;
			default:
				cout << a[row[x]][col[y]] << endl;
		}
	}
	
}
```



# YY and Rectangle

## ★题目描述

有 $n$ 张卡片，标记为 $1, 2, ..., n$ 。标记为 $i$ 的卡片上写有正整数 $a_i$ 。

YY 和 ZZ 分别从这 $n$ 张卡片中选择一张卡片取出，假设 YY 和 ZZ 选择的卡片上的数字分别为 $a$ 和 $b$ ，如果 $a*b$ 的矩形的面积大于或等于给定的阈值 $p$ ，则称这次选择得到的矩形是“Perfect Rectangle”。

现在，YY想知道，对于不同的阈值 $p$ ，有多少种的选择使得她们得到的矩形是“Perfect Rectangle”。

**注意**：两人不会选择同一张卡片。

## ★输入

第一行，包含一个正整数 $n$。$(2 \leq n \leq 10^5)$

第二行，包含 $n$ 个正整数 $a_i$，表示卡片上的数字。（$1 \leq a_i \leq 3 *10^5$）

第三行，包含一个正整数 $m$ ，表示询问的次数。（$1 \leq m \leq 10^5$）

接下来有 $m$ 行，每行一个正整数 $p$ ，表示待询问的阈值。（$1 \leq p \leq 3 * 10^5$）

## ★输出

输出包含 $m$ 行，每行一个整数，表示对于对应的阈值，使得她们得到的矩形是“Perfect Rectangle”的选择个数。

## ★输入样例

```in
2
3 3
2
9
10
```

## ★输出样例

```out
2
0
```

## ★样例解释

有 2 张卡片，标记为 1 的卡片上写有正整数 3，标记为 2 的卡片上写有正整数 3

有 2 个询问：

- 对于询问 1，阈值为 9，因此她们可以有 2 种选择：（1, 2）和（2, 1）。
- 对于询问 2，阈值为 10，因此她们可以有 0 种选择。

>思路：
>
>cnt[i]：记录每张牌上数字i的出现次数
>
>sum[i]：计算所有牌中,有多少张牌上的数字≥i
>
>对于每次询问的阈值q
>
>遍历计算有一个乘数＜根号q的情况
>
>计算两个乘数都大于等于根号q的情况
>
>ans *= 2
>
>•时间复杂度 O(m * sqrt(p))
>
>•（1≤m≤10^5）（1≤p≤3∗10^5）可行

```c++
#include <bits/stdc++.h>
using namespace std;

const int maxn = 3e5 + 5;

int n, m;
int cnt[maxn];
int sum[maxn];

int main() {
	
	cin >> n;
	for (int i = 0; i < n; i++) {
		int x;
		cin >> x;
		cnt[x]++;
	}
	for (int i = 3e5; i >= 1; i--) {
		sum[i] = cnt[i] + sum[i + 1];
	}
	
	cin >> m;
	while (m--) {
		long long ans = 0;
		double p;
		cin >> p;
		int i;
		for (i = 1; i < sqrt(p); i++) {
			ans += (long long)cnt[i] * sum[(int)ceil(p / i)];
		}
		ans += (long long)(1 + sum[i] - 1) * (sum[i] - 1) / 2;
		ans *= 2;
		cout << ans << endl;
	}
}

```



# YY and Array

## ★题目描述

YY 最近很头疼，明明学习数组学得头都大了，Z 老师还喜欢出一些丧心病狂的题目。今日份的配方如下：

Z 老师给了 YY 四个长度均为 $n$ （下标为 $1, 2, ..., n$）的数组：$a, b, c, d$ ，以及对应的两种数组操作：

- 操作1：对于所有的 $i \in \{1, 2, ..., n\}$ ，同时将所有的 $a_i$ 改为 $a_i \bigoplus b_i$ ，$\bigoplus$ 表示异或操作。
- 操作2：对于所有的 $i \in \{1, 2, ..., n\}$ ，同时将所有的 $a_i$ 改为 $a_{d_i}+t$ 。注意，数组 $d$ 是 1 到 $n$ 的一个排列。

现在，Z 老师希望 YY 连续进行 $m$ 次操作，使得最后得到的 $\sum_{i=1}^na_ic_i$ 最大，求该最大值。

YY 光是理解题目就已经耗掉所有精力了，帮帮 YY 吧。

## ★输入

第一行包含三个正整数 $n, m, t$ ，分别表示数组长度、操作次数、操作2的参数。（$1 \leq n, m \leq 25, \ 0 \leq t \leq 100$）

接下来四行，每行 $n$ 个整数，分别表示数组 $a, b, c, d$ ，数组 $d$ 是 1 到 $n$ 的一个排列。（$1 \leq a_i, b_i \leq 10^4, \ -10^4 \leq c_i \leq 10^4$）

## ★输出

输出包含一个整数 $s$，表示能得到的 $\sum_{i=1}^na_ic_i$ 的最大值。

## ★输入样例

```in
2 1 0
1 1
2 2
1 -1
2 1
```

## ★输出样例

```out
0
```

## ★样例解释

操作次数为 1，因此只有两种可能：

- 进行一次操作1，$a: \{1, 1\} \Rightarrow \{3, 3\}$，最后得到 $\sum_{i=1}^2a_ic_i=0$
- 进行一次操作2，$a: \{1, 1\} \Rightarrow \{1, 1\}$，最后得到 $\sum_{i=1}^2a_ic_i=0$

因此，最大值为 0。

## ★提示

异或操作的性质：对给定的数A，用同样的运算因子B作两次异或运算后仍得到A本身。即：

$$A \bigoplus B \bigoplus B = A$$



>思路：
>
>操作1是异或，两次异或之后还是本身，所以搜索的时候不会连续进行两次操作1，这样剪枝不会超时。
>在剩下搜索深度为偶数的时候更新节点；这是因为此时可以随意插入几对1操作，而搜索的时候不连续搜索1操作；
>
>时间复杂度：
>
>O(n * 2^(m + 1）)

### 代码

```c
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;
const int maxn = 30 + 5;

int n, m, t;
ll ans = INT_MIN;

vector <ll> a(maxn), b(maxn), c(maxn), d(maxn);

void dfs(int last,int deep){
	// if deep % 2 == 0, update ans
	if (deep % 2 == 0) {
		ll sum = 0;
		for (int i = 1; i <= n; i++) {
			sum += (ll) a[i] * c[i];
		}
		ans = max(ans, sum);
	}
	
	if (deep == 0) {
		return;
	}
	
	int tmp[30];
	for (int i = 1; i <= n; i++) {
		tmp[i] = a[i];
	}
	
	// if last is 1
	if (last == 1) {
		// op2, then last = 2
		for (int i = 1; i <= n; i++) {
			a[i] = tmp[d[i]] + t;
		}
		dfs(2, deep - 1);
	}
	else
	// if is first or last is 2
	if (last == 0 || last == 2){
		// op1, then last = 1
		for (int i = 1; i <= n; i++) {
			a[i] ^= b[i];
		}
		dfs(1, deep - 1);
		// op2, then last = 2
		for (int i = 1; i <= n; i++) {
			a[i] = tmp[d[i]] + t;
		}
		dfs(2, deep - 1);
	}
	
}


int main() {
	
	cin >> n >> m >> t;
	
	for (int i = 1; i <= n; i++) {
		cin >> a[i];
	}
	for (int i = 1; i <= n; i++) {
		cin >> b[i];
	}
	for (int i = 1; i <= n; i++) {
		cin >> c[i];
	}
	for (int i = 1; i <= n; i++) {
		cin >> d[i];
	}
	
	dfs(0, m);

	cout << ans << endl;
}
```

## 

# YY and Square

## ★题目描述

YY 想找出所有的 $<n, m>$ 正整数对使得 $n$ 行 $m$ 列的表格中恰好包含 $t$ 个正方形。

## ★输入

输入一行，包含一个正整数 $t$。$(1 \leq t \leq 10^{18})$

## ★输出

第一行输出一个整数 $z$，表示 $<n, m>$ 对的个数。

接下来包含 $z$ 行，每行两个正整数 $n, m$，以空格分隔，表示 $n$ 行 $m$ 列的表格中恰好包含 $t$ 个正方形。

注意：输出 $<n, m>$ 对时，以 $n$ 递增的顺序输出，$n$ 相同时以 $m$ 递增的顺序输出。

## ★输入样例

```in
8
```

## ★输出样例

```out
4
1 8
2 3
3 2
8 1
```

## ★样例解释

 1行8列、2行3列、3行2列、8行1列的表格均恰好包含8个正方形

下图以2行3列为例：

![](https://msk-picgo.oss-cn-shanghai.aliyuncs.com/img/20200828161854.png)



> 时间复杂度：O(t开方)

```c++
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;

ll t;
vector <vector<ll>> ans;

int main() {
	
	cin >> t;
	// i为行数
	// 以i为边长的正方形必有i*(i+1)*(2i+1)/6个小正方形满足条件
	for (ll i = 1; i * (i + 1) * (2 * i + 1) / 6 <= t; i++) {
		// 还需要多少个小正方形 
		ll remain = t - i * (i + 1) * (2 * i + 1) / 6;
		// 行数为i，每增加一列，增加(1+2+3++i)个小正方形，即i*(i+1)/2 
		ll increase = i * (i + 1) / 2;
		// 如果还需要的数量能整除一列增加的数量，那说明ok 
		if (remain % increase == 0) {
			// 列数=i+需要增加的列数 
			ll j = i + remain / increase;
			ans.push_back({i, j});
			if (i == j) break;
			ans.push_back({j, i});
		}
	}
	
	sort(ans.begin(), ans.end());
	cout << ans.size() << endl;
	for (int i = 0; i < ans.size(); i++) {
		cout << ans[i][0] << " " << ans[i][1] << endl;
	}
	
	return 0;
}
```

# YY and Sequence

## ★题目描述

"12345678910111213..."，YY 回味着今天 Z 老师教授的知识点——这样的数字序列的第 $k$ 位数字是什么。

YY 突发奇想，如果把所有的这种数字序列排放在一起，那么序列的第 $k$ 位数字是什么。

总而言之，题目是这样的：

给定一个数字序列 "1121231234..."，序列是由"1", "12", "123", "1234" ... 拼接而成的，第 $i$ 个拼接的子序列由数字 1 到 $i$ 组成。现在有 $q$ 个询问，每个询问包含一个正整数 $k$，YY 想知道上述的数字序列的第 $k$ 位数字是什么。

## ★输入

第一行，包含一个正整数 $q$ 。（$1 \leq q \leq 500$）

接下来 $q$ 行，每行一个正整数 $k$ 。（$1 \leq k \leq 10^9$）

## ★输出

输出包含 $q$ 行，每行一个正整数 $x$，表示对应询问的第 $k$ 位数字。($0 \leq x \leq 9$)

## ★输入样例

```in
5
1
2
3
4
5
```

## ★输出样例

```out
1
1
2
1
2
```

>思路：
>
>时间复杂度：
>
>O(n*len(n))

```c++
#include <bits/stdc++.h>
using namespace std;

const int maxn = 33000;

int q;
int len1[100000000];
int len2[100000000];
int put[100000000];

int cal(int x) {
	int ret = 0;
	while (x) {
		ret++;
		x /= 10;
	}
	return ret;
}

void init() {
	//1234567891011
	len1[0] = 0;
	for (int i = 1; i < maxn; i++) {
		len1[i] = len1[i - 1] + cal(i);
	}
	//112123123412345123456
	len2[0] = 0;
	for (int i = 1; i < maxn; i++) {
		len2[i] = len2[i - 1] + len1[i];
	}
	
	int tot = 1;
	stack <int> st;
	for (int i = 1; i < maxn; i++) {
		int num = i;
		while (num) {
			st.push(num % 10); 
			num /= 10;
		}
		while (!st.empty()) {
			put[tot++] = st.top();
			st.pop();
		}
	}
}

int main() {
	
	init();
	
	cin >> q;
	while (q--) {
		int k;
		cin >> k;
		int i = 1;
		while (len2[i] < k && i < maxn) {
			i++;
		}
		cout << put[k - len2[i - 1]] << endl;
	}	
} 
```



# 第二次作业

# YY and Lucky Number

## ★题目描述

YY 的幸运数字是......，是什么数字我也不知道，但是已知这个数字的十进制表示（不含前导零）中只包含不超过两个不同的数字。

给定一个数 $n$ ，请计算出不超过 $n$ 的所有正整数中，有可能是 YY 的幸运数字的个数。

## ★输入

输入一行，包含一个正整数 $n$。

对于 60% 的数据：

$1 \leq n \leq 10^5$

对于 100% 的数据：

$1 \leq n \leq 10^9$

## ★输出

输出一个整数，表示不超过 $n$ 的所有正整数中，有可能是 YY 的幸运数字的个数。

## ★输入样例

```in
103
```

## ★输出样例

```out
101
```

## ★样例解释

 不超过103的所有正整数中，只有102、103不可能是 YY 的幸运数字。

> 思路：
>
> 枚举前两位数字A、B
>
> 使用A和B，DFS搜索所有可能的答案
>
> 时间复杂度：与正确答案的个数有关

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

ll n, ans;

void dfs(int A, int B, ll now) {
	if (now > n) {
		return;
	}
	ans++;
	if (A != B) {
		dfs(A, B, now * 10 + A);
		dfs(A, B, now * 10 + B);
	}
	else {
		for (int i = 0; i <= 9; i++) {
			dfs(B, i, now * 10 + i);
		}
	}
}

int main () {
	
	cin >> n;
	if (n <= 9) {
		cout << n << endl;
		exit(0);
	}
	
	ans = 9;
	for (int i = 1; i <= 9; i++) {
		for (int j = 0; j <= 9; j++) {
			dfs(i, j, i * 10 + j);
		}
	}

	cout << ans << endl;
}

```



# YY and Triangle

### ★题目描述

YY 有点无聊。

YY 在纸上随意乱画。

### ★输入

一个正整数 $n$ ，表示三角形的规模。$(1 <= n <= 10)$

### ★输出

画出对应的三角形，注意最后一个有效字符后直接回车，且不要输出多余的空格。

规模为 1 的三角形如样例所示。

规模为 $n$ 的三角形由三个规模为 $n-1$ 的三角形拼接而成。

### ★样例输入1

```
1
```

### ★样例输出1

![avatar](data:img/jpg;base64,iVBORw0KGgoAAAANSUhEUgAAADQAAAA1CAIAAACBRl8ZAAACFElEQVRoBe2YIW/CQBSAywwkM7hKwh+onBtqFUsgKBRkiixZhoJMTLA/sIBCopaikGWqDjlJMrsEicOQMLcWGO8Kvfbd61vCyKGux929L+++S+5dZrVaGaf6uzhVsIBLw1F35xwzN3eqxe50vU2J/3F57y6p+ZHPo2Vu5g4WnVopt123UG7WR463kEch/kOBW0/HPbNVsfYh83aj/eF4830HU4MAt/Scvt0sFwSCXKnWWQzcmdDF0VSHm0+Go8eGnQ9Htyotszf+tTD8H/lLGc7XbdYuX+10g7i+eHbf8ViPhSJcoJsBRwHYDCMQzxtOOMVTgwt0s8SjINLxi6cEF+hWDx8Fkc7gFk8FbqPb0VEQ8ZjFw8PF6AZ8vOKh4WJ1AzpW8bBwSboBHqN4GX0ThrwqtbDbqrQo12ANR82kzpzOHDUD1HnaOZ05agao8/6tc+tpt1h10lQsKR8qYjIXUTyr7k/Khwo5nH+99NqxFQMCNd1DhRQu/FaD4IgekuraLoELqpm8rECNxpD1pri2R8NtqpnGtfhWI4ud3E+vFyPh8NVMMpo/glwvRsElF88oJhhEFe8YDlU8Q2BciybeERyyeMYxwSiSeIdwzLoBHUW8Azh23YCOIF4I7k90Azxl8VDPEZ+994cXCCK0sndP32+vQgc0s89fN7cmfBNaKDjCuixTQtvKsiLjIhqOmkydubPM3A9rNOQZ25nCzwAAAABJRU5ErkJggg==)

### ★样例输入2

```in
2
```

### ★样例输出2

![avatar](data:img/jpg;base64,iVBORw0KGgoAAAANSUhEUgAAAFQAAAB5CAIAAAAoO5F+AAAFBUlEQVR4Ae2dv2sUQRTHN1oYCELAIliF+AdcYRFsTOUV4oU0psphFQQxWCRaWMR/QC6FXJlKLoVEsLizukJImcIiYCGIkEquC0ggFoI7czGzczs/N+/NbHZfqt2Z3Xnzed/3NvBedjN1enqa1PXnWl3BGTfB11V9Up6Ur6EHKOxrKDpHJuVJ+Rp6gMK+hqLTA4/CPlDYH/dWFrYPzsbW0pOZp/2TQKbVZgIqf9TvjrZWl6bHG5lvra/t9YYj9bbCjAaDPzvY78xtLDcusGab7c3D3vD4YiD8QSj4k2Fvp7nems8QTi+tbo26/aPMUODDQPDHg9295+3mrEzXWN6Y6+z/fwrIcyHOwsCn6X602Vo8T3fBlSZ+c6c3jPXYCwHP0j0RjzrBniQs8Ye7g0iJHwCepXsj+6jL0kdNfHx4lu5r8qMuS59ETHx0eJ7uuUddFj9e4iPDG9Jd8EdLfFx4Y7oL+liJjwpvS3eBHyfxp6hRKSSo1RFq2JfdkwRfdoWw9kfKY3m27OuS8mVXCGt/pDyWZ8u+LilfdoWw9kfKg3pWakoVWfnsYHvhXjdEOR9cebkpVQSelTaSzvCwyL1+9wDD55pSfrs5vzotbTS6ffxmBiy8oilViD5QTRMUXt2UKoIfpqYJCa9rShWhD1LThIN3qlJ7+CFATRMM3rFK7UGPn/hQ8O5Vand89MQHgrc3pdyZxZXYiQ8CD53uAh838SHg4dNd0KMmPgA8RroLeszEx2tX/Xg38/2jgMgctW62Br8HmQFx2Lr96cPdW+Ic9wgPHnffIKsDhD3IPqIsQvBR3F4Co6R8CUSIsgVSPorbS2CUlC+BCFG2QMpHcXsJjJLyJRAhyhYKKi815FDfEmOdu5Xe+G0E6C5eMXi5IZdWW9DeEpN6QNBdvCLwuYYcq7bgvCWWVomGm5m/1oft4hWAl8QY5ypWmVWOMGYLtKbnD69uyGGUWVmEzU6+nANZ0/OG1zXkQCXh8cSLwu372fcQ2ThglHnCGyr0kJIwSH1RGCzK/OCNFXpASVJ2Uw8IKsq84PViMKkSwLfEDBHGDAFFmQ+8SQy2JbhnsTHCmCGYKHOHt4jBtgQliS3CmCWIxHeGt4rBdgQjiT3CmCWAxHeFdxGDbenykjhFGDN0+cSndhXzYx1/XMO+kr4h+ErK6gBFyjs4qZKXkPKVlNUBipR3cFIlLyHlKymrAxQpr3FStlOkucQy7NzIktpflkWV04U6WQblWfli4hN2SruGwbTg4NTISssXmY8kGhbUTrG6lvf7aHr4tHwhdYq0dk0TrOBgbWSx8oX0kUTTitq5Ap0sLfzlxeDbdKg0AkQYN+Vf19LAMzFynSKt040TtkojK5DlP5JoXFIz6V3XUsPzamW+U6Qxahk2K8KrlYqPJFoWVU47hJl0nxLevVopraU7MSjiXK3UrT0xbgsz+XIVvFvpWF7HeKZVxLEeblxcnjSHmXyt4v/YQIvBDaoVAY4wbskQZhPoin/iAy8Gt6lSBDzCuCVtmOXYc8pjiMGt5hRBiTBuSh1mefZJeBwxuN0JRZAijJtShZmCXYbHE4ObziqCFmHcUi7MVOhJ4tSu+tb5/OyN8v4bT179ef9WPfX654OHc8op/eDo68s7v9QfjFi9nuz/Vd75+MujF4vKGcugE7xljSs7rfo9f2VhfDdO8L4eq8r1pHxVlPTlIOV9PVaV62ut/D9xq5q3e3vCWwAAAABJRU5ErkJggg==)

> 思路：
>
> 规模为i的三角形（以下记作△i）到规模为i+1的三角形的变换过程，可以分解成四个步骤（如上）。
>
> 　　第一步： △i 向下复制，复制的行数为△i 的行数，即2^i；
>
> 　　第二步：复制的△i向右平移 2^(i+1) 的距离；
>
> 　　第三步：再次将△i 向下复制；
>
> 　　第四步： △i 向右平移2^i的距离。 
>
> 时间复杂度：O(3^(n-1))

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

int n;
vector <string> s(1 << 10);


void dfs(int n) {
	if (n == 1) return;
	dfs(n - 1);
	int step =  1 << (n - 1);
	// 生复制第一个n-1规模的三角形 
	for (int i = 2 * step - 1; i >= step; i--) {
		s[i] = s[i - step];
		for (int j = 1; j < 2 * step - i; j++) {
			s[i] += " ";
		}
		s[i] += s[i - step];
	}
	// 对原来的三角形做平移 
	string tmp;
	for (int i = step - 1; i >= 0; i--) {
		tmp = "";
		for (int j = 1; j <= step; j++) {
			tmp += " ";
		}
		s[i] = tmp + s[i];
	}
	
}

int main() {
	
	cin >> n;
	s[0] = " /\\";
	s[1] = "/__\\"; 
	
	dfs(n);
	
	for (int i = 0; i < 1 << (n ); i++) {
		cout << s[i] << endl;
	}
	
}
```



# YY and Pair

## ★题目描述



<img src="https://msk-picgo.oss-cn-shanghai.aliyuncs.com/img/20200929140714.jpg" style="zoom:50%;" />

YY 很享受一个人独处的时光。

但是偶尔也会觉得成双成对的也不赖。

现在，对于给定的一对正整数 $(x,y)$ ，可以进行若干次的如下操作：

1. 将 $(x, y)$ 修改为新的一对正整数 $(x + y, y)$
2. 将 $(x, y)$ 修改为新的一对正整数 $(x, y + x)$

YY 想知道，对于初始正整数对 $(1, 1)$ ，最少需要经过多少次操作使得最终得到的正整数对中包含至少一个正整数 $n$ 。



## ★输入

输入一行，包含一个正整数 $n$。

对于 60% 的数据：

$1 \leq n \leq 10^3$

对于 100% 的数据：

$1 \leq n \leq 10^6$

## ★输出

输出一个整数，表示最少需要的操作次数。

## ★输入样例

```in
7
```

## ★输出样例

```out
4
```

## ★样例解释

$n=7$ 时的一个最优解： $(1, 1) \rightarrow (1, 2) \rightarrow (3,2) \rightarrow (5,2) \rightarrow (7,2)$ ，需要四次操作。

>思路：
>
>令(i,j)表示根节点到达(i,j)的次数
>
>当i或者j一直比较大的时候不必一步步计算，直接一步到位到达i j
>
>j>i时 (i,j)=(i,j%i)+j/i; 即(2,5)=(2,5%2)+5/2=(2,1)+2 此时(2,1)是2>1 也就是i>j
>
>同理 i>j (i,j)=(i%j,j)+i/j;
>
>但是由于我们是不知道题目所给定的目标n的最优解出现在那个结点上，比如给定7，出现在(x,7)或是(7,x) 中具体是那个x，当然这两种情况是等价的，我们只要令其中一个i或者j为给定的目标值，然后遍历另一个数到n/2即可。然后取其中(i,n) 在i到n/2中的最小值就是我们的最优解。
>
>这个for循环是O（n/2） 里面的dfs函数是O(logn)所以
>
>最终时间复杂度是O(nlogn)

### 代码

```c
#include <bits/stdc++.h>
using namespace std;

int n;
int ans = INT_MAX ;


//dp[i][j]=dp[i][j%i]+j/i; j>i??? 
int dfs(int i,int j){
	if (i == 0 || j == 0) {
		return 1000009;
	}
	if (i == 1 && j == 1) {
		return 0;
	}
	if (i == 1) {
		return j - 1;
	}
	if (j == 1) {
		return i - 1;
	}
	
	if (i < j) {
		return dfs(i, j % i) + j / i;
	}
	else {
		return dfs(i % j, j) + i / j;
	}
}


int main(){
	cin>>n;
	
	for(int i = 0; i < n; i++){
		ans = min(ans, dfs(i + 1, n));
	}
	
	cout << ans << endl;
	
}
```

## 

# YY and Point

## ★题目描述

二维空间中散落着数不胜数的点，YY 可以将每个点的坐标 $(x_i, y_i)$ 修改为以下四种之一：

1. $(x_i, y_i)$
2. $(-x_i, y_i)$
3. $(x_i. -y_i)$
4. $(-x_i, -y_i)$

YY 想知道经过修改之后能得到的最小两点“Y式距离”是多少。

两点“Y式距离”定义为：若两点坐标分别为 $(x_1, y_1)$ 和 $(x_2, y_2)$ ，则两点“Y式距离”为 $D_Y = \sqrt{(x_1+x_2)^2+(y_1+y_2)^2}$

## ★输入

第一行，包含一个正整数 $n$。

接下来有 $n$ 行，每行两个整数 $x_i$ 和 $y_i$ ，表示点的坐标。

对于 60% 的数据：

$2 \leq n \leq 10^3, -10^6 \leq x_i, y_i \leq 10^6$

对于 100% 的数据：

$2 \leq n \leq 10^5, -10^6 \leq x_i, y_i \leq 10^6$

## ★输出

令得到的最小两点“Y式距离”为 $D_Y$ ，输出一个整数，表示 ${|D_Y|}^2$。

## ★输入样例

```in
4
2 4
-3 4
-5 -8
1 -7
```

## ★输出样例

```out
1
```

## ★样例解释

将点 $(2,4)$ 和点 $(-3, 4)$ 分别修改为 $(-2, 4)$ 和 $(3, -4)$ ，得到 $|D_Y|^2=((-2)+3)^2+(4+(-4))^2=1$

> 思路：
>
> 最接近点对问题
>
> 时间复杂度：
>
> ![image-20201212125925254](C:\Users\西瓜不甜\AppData\Roaming\Typora\typora-user-images\image-20201212125925254.png)





```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
#define INF 9223372036854775807LL

struct point {
	int x, y;
	point(int x, int y): x(x), y(y) {};
	bool operator <(point obj) {
		return y < obj.y;
	}
};
int n;
vector <point> input;

ll dis(point a, point b) {
	return pow(a.x - b.x, 2) + pow(a.y - b.y, 2);
}

ll solve(int l, int r) {
	if (l == r) {
		return INF;
	}
	
	int mid = (l + r) >> 1;
	ll d = min(solve(l, mid), solve(mid + 1, r));
	
	vector <point> v;
	for (int i = l; i <= r; i++) {
		if (dis(input[mid], input[i]) < d) {
			v.push_back(input[i]);
		}
	}

	for (int i = 0; i < v.size(); i++) {
		for (int j = i + 1; j < v.size(); j++) {
			d = min(d, dis(v[i], v[j]));
		}
	}
	
	return d;
}

int main() {
	cin >> n;
	for (int i = 0; i < n; i++) {
		int x, y;
		cin >> x >> y;
		input.push_back(point(abs(x), abs(y)));
	}
	sort(input.begin(), input.end());
	
	cout << solve(0, n - 1) << endl;
}
```



# YY and One

## ★题目描述

YY 很享受一个人独处的时光。

这可能也是 YY 对 1 这个数字情有独钟的原因。

现在，对于给定的一个正整数 $n$，YY 想把它表示为若干个加数之和，其中每个加数都是仅包含 1 的整数。YY 想知道这些加数最少可以包含多少个 1 。

例如：对于正整数 10，可以表示为：$10=11+(-1)$ ，包含 3 个 1 。

## ★输入

输入一行，包含一个正整数 $n$。

对于 60% 的数据：

$1 \leq n \leq 10^5$

对于 100% 的数据：

$1 \leq n \leq 10^{12}$

## ★输出

输出一个整数，表示所有加数包含 1 的个数的最小值。

## ★输入样例

```in
1232
```

## ★输出样例

```out
10
```

## ★样例解释

最优解为：$1232=1111+111+11+(-1)$ ，包含 10 个 1 。

>思路：
>
>①计算整数**n**的位数**len****=log10(n)+1**
>
>②分别计算与n同位和高一位的a数组的值与n的差值的绝对值
>
>③取绝对值较小的数n1，如果**n1!=0****，n=n1**，转1，否则，转4
>
>④输出结果，算法结束
>
>**最坏情况下**，n离同位和高一位的由1组成的数近似相等，每次消除最高位数需要6次运算，需要6*log10(n)次递归，每次递归计算当前数的位数需要log10(n)。因此，一共需要6*log(10)*log(10)次运算。
>
>
>
>时间复杂度O(n) = log10(n)*log10(n)。

### 代码

```c
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

ll a[13] = {1, 11, 111, 1111, 11111, 111111, 1111111, 11111111, 111111111, 1111111111, 11111111111, 111111111111, 1111111111111};
ll n;
ll num = 0;
void calNum(ll s) {
	if (s == 0) {
		return;
	}
	int len = (int)(log10(s * 1.0) + 1);
	ll x = abs(s - a[len - 1]);
	ll y = abs(s - a[len]);
	if (y < x) {
		num += len + 1;
		calNum(y);
	}
	else {
		num += len;
		calNum(x);
	}
}

int main() {
	cin >> n;
	calNum(n);
	cout << num << endl;
}
```

# 第三次作业

# YY and Card

## ★题目描述

YY 有 $n$ 张卡片，第 $i$ 张卡片上写有数字 $a_i$ 。

YY 可以修改卡片上数字的符号，如将 $5$ 修改为 $-5$ ，也可以保持卡片上的数字不变

YY 想知道一共有多少种方案，使得最终所有卡片上的数字之和为 $T$ 。

## ★输入

第一行，包含两个整数 $n$ 和 $T$。

第二行，包含 $n$ 个正整数，表示卡片上的数字 $a_1, a_2, ..., a_n$。

对于 60% 的数据：

$1 \leq n \leq 20, \ a_i > 0, \ \sum a_i \leq 1000, \ |T| \leq 1000$

对于 100% 的数据：

$1 \leq n \leq 1000, \ a_i > 0, \ \sum a_i \leq 1000, \ |T| \leq 1000$

## ★输出

若方案数为 $X$ ，输出 $X \ mod \ 1000000007$ 。

## ★输入样例

```in
5 3
1 1 1 1 1
```

## ★输出样例

```out
5
```

## ★样例解释

$-1+1+1+1+1 = 3$

$+1-1+1+1+1 = 3$

$+1+1-1+1+1 = 3$

$+1+1+1-1+1 = 3$

$+1+1+1+1-1 = 3$

>思路：
>
>对n个数里面的任意一个数要么是保持不变的，要么是取它的相反数。
>
>所以在最后求和得到T的情况下，肯定是一部分数为取+(也就是保持不变)，另外一部分取-(也就是取相反数)。
>
>所以我们将取+的数归在P集合，取-的数归在N集合。很显然，最终要对所有的数求和，可以得到以下式子。
>
>Sum(P) - Sum(N) = t   t就是我们给定的数
>
>Sum(P) + Sum(N) = sum  Sum是求和操作，sum是输入n个数的和。
>
>将两个式子相加消去Sum(N) 得：
>
>Sum(P) = (t + sum) / 2 令(t+sum)/2=target
>
>因此，题目简化为从n个数中选出一部分数，使其的和为target的方案数
>
>![image-20201212132935727](C:\Users\西瓜不甜\AppData\Roaming\Typora\typora-user-images\image-20201212132935727.png)
>
>![image-20201212133228345](C:\Users\西瓜不甜\AppData\Roaming\Typora\typora-user-images\image-20201212133228345.png)
>
>时间复杂度：O(ntarget) 

```c++
#include <bits/stdc++.h>
using namespace std;

const int maxn = 1005;
const int mod = 1e9 + 7;


int n, T;
int a[maxn];
int sum;
int dp[1000000];


int main() {
	
	cin >> n >> T;
	for (int i = 0; i < n; i++) {
		cin >> a[i];
		sum += a[i];
	}
	
	if (sum < T || (T + sum) % 2 != 0) {
		cout << 0 << endl;
		exit(0);
	}
	
	int target = (T + sum) / 2;
	dp[0] = 1;
	for (int i = 0; i < n; i++) {
		for (int j = target; j >= a[i]; j--) {
			dp[j] = (dp[j] + dp[j - a[i]]) % mod;
		}
	}
	
	cout << dp[target] << endl;
	
	
}
```

# 

# YY and Fibonacci

## ★题目描述

如果一个序列 $a_1, a_2, ..., a_m$ 满足：

- $m \geq 3$
- 对于所有 $i \geq 3$ ，都有 $a_i = a_{i-1} + a_{i-2}$

则称序列 $a_1, a_2, ..., a_m$ 具有 $Fibonacci$ 性。

现在给定一个严格递增的序列 $a_1, a_2, ..., a_n$ ，YY 想知道其中最长的具有 $Fibonacci$ 性的子序列的长度。

## ★输入

第一行，包含一个正整数 $n$。

第二行，包含 $n$ 个正整数，表示序列 $a_1, a_2, ..., a_n$。

对于 60% 的数据：

$3 \leq n \leq 100, 1 \leq a[1] < a[2] < ... < a[n] \leq 10^9$

对于 100% 的数据：

$3 \leq n \leq 3000, 1 \leq a[1] < a[2] < ... < a[n] \leq 10^9$

## ★输出

输出一个整数，表示最长的具有 $Fibonacci$ 性的子序列的长度。如果不存在这样的子序列，输出 $-1$ 。

## ★输入样例

```in
5
1 2 3 4 5
```

## ★输出样例

```out
4
```

## ★样例解释

最长的具有 $Fibonacci$ 性的子序列为：$1, 2, 3, 5$

## ★提示

可能会用到的数据结构：[c++ - unordered_map](http://www.cplusplus.com/reference/unordered_map/unordered_map/)

> 思路：
>
> dpij表示以A[ i ] [ j ] 为终点的最长斐波那契子序列的长度
>
> ![image-20201212135516309](C:\Users\西瓜不甜\AppData\Roaming\Typora\typora-user-images\image-20201212135516309.png)
>
> 时间复杂度:O(N^2)

```c++
#include <bits/stdc++.h>
#include <unordered_map> 

using namespace std;

const int maxn = 3000 + 5;

int n;
int nums[maxn];
int ans = -1;
unordered_map <int, int> mp;
vector <vector<int>> dp(maxn, vector<int>(maxn, 2));


int main() {
	
	cin >> n;
	for (int i = 0; i < n; i++) {
		cin >> nums[i];
		mp[nums[i]] = i;
	}
	
	for (int i = 0; i < n; i++) {
		for (int j = i + 1; j < n; j++) {
			int sum = nums[i] + nums[j];
			if (mp.count(sum)) {
				int k = mp[sum];
				dp[j][k] = max(dp[j][k], dp[i][j] + 1);
				ans = max(ans, dp[j][k]);
			}
		}
	}
	
	cout << ans << endl;
	
}
```



# YY and Inverse

## ★题目描述

逆序对的定义为：对于序列的第 $i$ 个和第 $j$ 个元素，如果满足 $i<j$ 且 $a_i>a_j$ ，则其为一个逆序对；否则不是。

YY 想知道 $1$ 到 $n$ 的全排列中，恰好拥有逆序对个数为 $t$ 的排列个数。

## ★输入

输入一行，包含两个正整数 $n$ 和 $t$ 。

对于 30% 的数据：

$2 \leq n \leq 8, 0 \leq t \leq 20$

对于80% 的数据：

$2 \leq n \leq 100, 0 \leq t \leq 100$

对于 100% 的数据：

$2 \leq n \leq 1000, 0 \leq t \leq 1000$

## ★输出

若恰好拥有逆序对个数为 $t$ 的排列个数为 $X$ ，输出 $X \ mod \ 1000000007$ 。

## ★输入样例

```in
3 1
```

## ★输出样例

```out
2
```

## ★样例解释

$n=3$ 时的全排列为：$[1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], [3,2,1]$

其中 $[1,3,2]$ 和 $[2,1,3]$ 有 $1$ 个逆序对。

> 思路：
>
> dp[i][j : 加入第i个数后，逆序对个数为j的方案
>
> <img src="C:\Users\西瓜不甜\AppData\Roaming\Typora\typora-user-images\image-20201212134311346.png" alt="image-20201212134311346" style="zoom: 50%;" />
>
> <img src="C:\Users\西瓜不甜\AppData\Roaming\Typora\typora-user-images\image-20201212134240169.png" alt="image-20201212134240169" style="zoom: 67%;" />
>
> 时间复杂度：O(nt)

```c++
#include <bits/stdc++.h>
using namespace std;

const int maxn = 1000 + 5;
const int mod = 1000000007;

int n, t;
vector <vector<long long> > dp(maxn, vector<long long>(maxn, 0));

int main() {
	
	cin >> n >> t;
	
	dp[1][0] = 1;
	
	for (int i = 2; i <= n; i++) {
		int sum = 0;
		for (int j = 0; j <= t; j++) {
			sum = (sum + dp[i - 1][j]) % mod;
			dp[i][j] = sum;
			if (i - 1 <= j) {
				sum = (sum - dp[i - 1][j - i + 1] + mod) % mod;
			}
		}
	}
	
	cout << dp[n][t] << endl;
}
 
```



# YY and Shop I

## ★题目描述

喜迎国庆中秋“双节”，周边的商店也迎来了促销。

本期活动中，共有 $n$ 种商品参与促销，编号分别为 $0, 1, ..., n-1$ ，第 $i$ 种商品的促销价为 $c_i$ ，原价为 $v_i$ ，限购数量为 $k_i$ 。但是事情并没有这么简单，店家会在活动当天宣布有一种商品退出促销，无法购买。

为了获取最大利益，YY 想提前计算好 $q$ 种情况，即在给定携带资金 $x_i$ 和无法购买第 $y_i$ 种商品的情况下，最多能够买走原价总和是多少的商品。注意：商品论件出售，不允许拆分。

## ★输入

第一行，包含一个正整数 $n$ 。

接下来有 $n$ 行，每行包含三个整数 $c_i$ ，$v_i$ 和 $k_i$。

接下来一行，包含一个正整数 $q$ 。

之后 $q$ 行，每行包含两个整数 $x_i$ 和 $y_i$ 。

对于 60% 的数据：

$1 \leq n\leq 50, \ 1 \leq q \leq 20$

$1 \leq c_i, v_i, k_i \leq 100$

$0 \leq x_i \leq 1000, 0 \leq y_i < n$

对于 100% 的数据：

$1 \leq n\leq 800, \ 1 \leq q \leq 50000$

$1 \leq c_i, v_i, k_i \leq 100$

$0 \leq x_i \leq 1000, 0 \leq y_i < n$

## ★输出

输出包含 $q$ 行，每行包含一个整数，表示对应的情况下 YY 能买走的最大原价总和。

## ★输入样例

```in
3
1 3 1
1 4 1
1 5 1
1
1 2
```

## ★输出样例

```out
4
```

## ★样例解释

资金为 1 且不能购买编号为 2 的商品，因此选择购买 1 件编号为 1 的商品，得到的原价总和为 4 。

## ★提示

有关背包问题的更多信息，可以参见：[背包九讲](http://sosd.space/index.php/s/UG1cdE8FKy9PxoS)

> 思路：
>
> 多重背包
>
> ![image-20201212140321331](C:\Users\西瓜不甜\AppData\Roaming\Typora\typora-user-images\image-20201212140321331.png)
>
> 商品    =   物品
>
> 促销价c   =   每件耗费的空间C
>
> 原价v    =   物品价值W
>
> 限购数量k  =   最多M件可用
>
> 携带资金x  =   容量V
>
> ![image-20201212140955721](C:\Users\西瓜不甜\AppData\Roaming\Typora\typora-user-images\image-20201212140955721.png)
>
> ![image-20201212141027856](C:\Users\西瓜不甜\AppData\Roaming\Typora\typora-user-images\image-20201212141027856.png)
>
> ![image-20201212141133072](C:\Users\西瓜不甜\AppData\Roaming\Typora\typora-user-images\image-20201212141133072.png)
>
> 时间复杂度：O(1000* 100 * 100)



```c++
#include <bits/stdc++.h>
using namespace std;

const int maxn = 1000 + 5;

int n, q;
int c[maxn], v[maxn], k[maxn];
int f[maxn][maxn];// 对于前i个商品花费j元可得到的最大原价值 
int g[maxn][maxn];// 后 

int main() {

	cin >> n;
	for (int i = 1; i <= n; i++) {
		cin >> c[i] >> v[i] >> k[i];
	}
	
	// 正序购买 
	// 对于商品i 
	for (int i = 1; i <= n; i++) {
		// 选购j件商品 
		for (int j = 0; j <= k[i]; j++) {
			// 买该商品花了t元 
			for (int cost = c[i] * j; cost <= 1000; cost++) {
				f[i][cost] = max(f[i][cost], f[i - 1][cost - c[i] * j] + v[i] * j);
			}
		}
	}
	// 逆序购买 
	for (int i = n; i >= 1; i--) {
		for (int j = 0; j <= k[i]; j++) {
			for (int cost = c[i] * j; cost <= 1000; cost++) {
				g[i][cost] = max(g[i][cost], g[i + 1][cost - c[i] * j] + v[i] * j);
			}
		}
	}
	
	
	int q;
	cin >> q;
	while (q--) {
		int ans = 0;
		int x, y;
		cin >> x >> y;
		y++;
		// 花费i 买前y-1个商品, 花费x-i买后y+1个商品 
		for (int cost = 0; cost <= x; cost++) {
			ans = max(ans, f[y - 1][cost] + g[y + 1][x - cost]);
		}
		cout << ans << endl;
	}
	
	
}
```



# YY and Shop II

## ★题目描述

喜迎国庆中秋“双节”，周边的商店也迎来了促销。YY 在本期促销活动中抽到了特等奖！

本期活动中，共有 $n$ 件商品参与促销，编号分别为 $1, 2, ..., n$ ，第 $i$ 件商品的价值为 $v_i$ ，YY 可以选择 $m$ 件拿走。但是，拿走商品有一定的限制，某些商品不能被直接拿走，如果想要拿走它，必须要先拿走它指定的另一件特定商品。

YY 想知道，最多能拿走总价值为多少的商品。

## ★输入

第一行，包含两个正整数 $n$ 和 $m$ 。

接下来有 $n$ 行，每行包含两个整数 $f_i$ 和 $v_i$。若 $f_i=0$ ，表示该商品可以直接拿走，否则，表示拿走该商品前需要先拿走第 $f_i$ 个商品。$v_i$ 表示该商品的价值。

对于 100% 的数据：

$1 \leq n, m \leq 300, \ 0 \leq f_i \leq n, \ 0 \leq v_i \leq 10^6$

数据保证商品的需求关系不存在环。

## ★输出

输出一个整数，表示能拿走的最大商品总价值。

## ★输入样例

```in
3 2
0 1
0 2
0 3
```

## ★输出样例

```out
5
```

## ★样例解释

3个商品都可以直接拿走，因此选择编号为 2 和 3 的商品，得到的总价值为 5。

## ★提示

有关背包问题的更多信息，可以参见：[背包九讲](http://sosd.space/index.php/s/UG1cdE8FKy9PxoS)

> 思路：
>
> 有依赖的背包
>
> •题目中0代表可以直接拿走，为此我们可以建立一个 虚拟节点0 号节点，将所有树的根节点都连向 0 号节点。0 号节点的价值为 0，并且允许拿走的数量量要自加一。
>
> 将森林拼接成一棵树之后方便我们遍历节点，然后我们就需要对这一颗树进行树上的动态规划了。
>
> ![image-20201212155228758](C:\Users\西瓜不甜\AppData\Roaming\Typora\typora-user-images\image-20201212155228758.png)
>
> 其中第二重循环上限是M+1,因为把0当作一个必选的点，这样可以直接从0访问到各节点
>
> 时间复杂度 ： O(n*m^2)



```c++
#include <bits/stdc++.h>
using namespace std;

const int maxn = 100005;

int n, m;
int dp[305][maxn];

struct Edge {
	int to, next;
};
vector <Edge> e(maxn);
int head[maxn];
int tot;

void add(int u, int v) {
	e[++tot].to = v;
	e[tot].next = head[u];
	head[u] = tot;
}

void dfs(int u) {
	int now = head[u];
	while (now) {
		int v = e[now].to;
		dfs(v);
		for (int i = m + 1; i >= 1; i--) {
			for (int j = 0; j < i; j++) {
				dp[u][i] = max(dp[u][i], dp[u][i - j] + dp[v][j] );
			}
		}
		now = e[now].next;
	}
}

int main() {
	
	cin >> n >> m;
	for (int i = 1; i <= n; i++) {
		int u, val;
		cin >> u >> val;
		dp[i][1] = val;
		add(u, i);
	}
	
	dfs(0);
	
	cout << dp[0][m + 1] << endl;
}
```





# 算法设计与分析 4.1 小明和果子

### ★题目描述

果园里有 $n$ 堆果子，每堆果子有 $a_i$ 个。小明要把所有果子合并成一堆。小明每次可以合并两堆果子，合并的代价是合并后果子的个数。问将所有果子合并成一堆的最小代价是多少。

### ★输入格式

第一行为一个正整数 $n$ ，表示有 $n$ 堆果子。

接下来一行有 $n$ 个正整数，用空格隔开，行末无空格，表示每堆果子的个数。

对于60%的数据，$1 \le n \le 10^3$。

对于100%的数据，$1 \le n \le 10^6$，$1 \le a_i \le 10^6$。

### ★输出格式

输出一个正整数，表示最小花费。

### ★样例输入

```wiki
3
1 2 4
```

### ★样例输出

```wiki
10
```

> 思路:
>
> 优先队列
>
> 时间复杂度：O（n * log n）

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef unsigned long long ull;

int n;
ull ans;
priority_queue <int, vector<int>, greater<int> > pq;

int main() {
	
	cin >> n;
	for (int i = 0; i < n; i++) {
		int x;
		cin >> x;
		pq.push(x);
	}
	
	while (pq.size() >= 2) {
		int a = pq.top();
		pq.pop();
		int b = pq.top();
		pq.pop();
		int sum = a + b;
		pq.push(sum);
		ans += sum;
	}
	
	cout << ans << endl;
}
```





# 算法设计与分析 4.2 小明卖股票

### ★题目描述

有 $n$ 天，第 $i$ 天小明会获得 $a_i$ 支股票。第 $i$ 天每支股票可以卖 $b_i$ 元，小明最多能卖 $c_i$ 支股票。请问 $n$ 天结束后小明最多能赚多少钱。

### ★输入格式

第一行为一个正整数 $n$ ，表示有 $n$ 天。

接下来 $n$ 行，每行有三个正整数 $a_i\ b_i\ c_i$ ，用空格隔开，行末无空格。

对于60%的数据，$1 \le n \le 10^3$。

对于100%的数据，$1 \le n \le 10^6$，$1 \le a_i,b_i,c_i \le 10^6$。

### ★输出格式
输出一个正整数，表示最多能赚多少钱。

### ★样例输入
```wiki
3
1 2 3
4 5 6
7 8 9
```
### ★样例输出
```wiki
87
```
### ★样例输出
```wiki
3
```
>思路：
>
>1、将当前手上的所有股票都卖出去，若当前手上的股票卖不完，就把剩下的股票数量加到后一天的获得的股票数量中
>
>2、若所获得的股票均已卖出，而当天能卖的股票额度未使用完，则判断当天股票能卖的金额和之前已经卖了的股票的最低金额进行比较，如果大于，则修改为在本次卖出
>
>时间复杂度：O(nlogn)

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

const int maxn = 1e6 + 5;

int n, m;
int a[maxn], b[maxn], c[maxn];
ll ans = 0;

struct Node {
	int val;
	int imax;
	Node (int val, int imax): val(val), imax(imax) {}
	bool operator <(Node obj) const{
		return val < obj.val;
	}
};

int main() {
	cin >> n;
	for (int i = 0; i < n; i++) {
		cin >> a[i] >> b[i] >> c[i];
	}
	
	priority_queue <Node> q;
	for (int i = n - 1; i >= 0; i--) {
		q.push(Node(b[i], c[i]));
		// num，第i天得到了多少支股票 
		int num = a[i];
		
		// 如果今天已经没有股票能卖或者队列已经空了 
		while (num && !q.empty()) {
			// 找到价格最高的选择 
			Node tmp = q.top();
			q.pop();
			// 如果可卖的大于现有的 
			if (tmp.imax > num) {
				ans += tmp.val * num;
				// 更新可卖数量 
				q.push(Node(tmp.val, tmp.imax - num));
				// 第i天无可卖股票 
				num = 0;
			}
			else {
				ans += tmp.val * tmp.imax;
				num -= tmp.imax;
			}
		}
	}
	
	cout << ans << endl;
	
}
```



# 算法设计与分析 4.3 小明借书
### ★题目描述

图书馆里有 $n$ 本书，借第 $i$ 本书要付 $a_i$ 元押金，$b_i$ 元租金，还书的时候会退押金，但是租金不会退。小明有 $s$ 元，他想看尽量多的书，请问他最多能看几本？

PS：小明不会借两本相同的书。

### ★输入格式

第一行为两个正整数 $n\ s$ ，用空格隔开，行末无空格，表示图书馆有 $n$ 本书，小明有 $s$ 元。

接下来 $n$ 行，每行有两个正整数 $a_i\ b_i$ ，用空格隔开，行末无空格。

对于30%的数据，$1 \le n \le 20$。

对于100%的数据，$1 \le n \le 10^6$，$1 \le a_i,b_i \le 10^6$，$1 \le s \le 2 \times 10^8$。

### ★输出格式
输出一个正整数，表示最多能看几本书。

### ★样例输入
```wiki
3 10
1 2
3 4
5 6
```
### ★样例输出
```wiki
2
```
> 思路：
>
> 1、将书本按押金大小排序，当押金相等时，租金小的在前
>
> 2、按顺序借书，即借即还
>
> 3、当目前剩下的钱不够再借下一本书，如果目前借过的书中，最贵的租金大于下一本书的租金，就不借租金最贵的那本，改借下一本书。改借不改变能看的数的数量，但剩余的金额变多了
>
> 时间复杂度：
>
> O(nlogn)

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

const int maxn = 1e6 + 5;

struct Book {
	int deposit, rent;
	Book (int deposit, int rent): deposit(deposit), rent(rent) {}
	bool operator < (Book obj) const {
		return rent < obj.rent;
	}
};

bool cmp(Book x, Book y) {
	if (x.deposit == y.deposit) {
		return x.rent > y.rent;
	}
	return x.deposit > y.deposit;
}


int n, s;
vector <Book> books;
priority_queue <Book> pq;

int main() {
	
	cin >> n >> s;
	for (int i = 0; i < n; i++) {
		int deposit, rent;
		cin >> deposit >> rent;
		if (deposit + rent <= s) {
			books.push_back(Book(deposit, rent));
		}
	}
	
	sort(books.begin(), books.end(), cmp);
	
	bool flag = false;
	int bor;
	for (int i = 0; i < books.size(); i++) {
		// 每打算借一本书时，s先加上最后借到的书的租金 
		if (flag) {
			s += bor;
		}
		// 用剩余金额s与这本书的总金额作比较，如果能借，就加入队列 
		if (books[i].deposit + books[i].rent <= s) {
			s -= books[i].deposit + books[i].rent;
			pq.push(books[i]);
			bor = books[i].deposit;
			flag = true;
		}
		else
		// 如果不能借， 和队列头的书作比较，如果这本书的租金比队头书租金少，那说明我们能花更少钱借到更多书
		//  把队头的书换成现在的书 
		if (pq.top().rent > books[i].rent 
			&& pq.top().rent + s >= books[i].deposit + books[i].rent) {
				s += pq.top().rent;
				pq.pop();
				s -= books[i].deposit + books[i].rent;
				pq.push(books[i]);
				bor = books[i].deposit;
				flag = true;
		}
		else {
			flag = false;
		}
	}
	
	cout << pq.size() << endl;
	
}
```



# 算法设计与分析 4.4 小明上课

### ★题目描述

有 $n$ 节课，每节课开始和结束的时间分别是 $a_i\ b_i$ 。小明无法选两节有冲突的课。如果两节课的时间有交集（端点相交也算），这两节课就有冲突。请问小明最多能选几门课。

### ★输入格式

第一行为一个正整数 $n$ ，表示有 $n$ 节课。

接下来 $n$ 行，每行有两个正整数 $a_i\ b_i$ ，用空格隔开，行末无空格。

对于60%的数据，$1 \le n \le 10^3$。

对于100%的数据，$1 \le n \le 10^6$，$1 \le a_i \le b_i \le 10^6$。

### ★输出格式
输出一个正整数，表示最多能选几门课。

### ★样例输入
```wiki
3
1 2
3 4
5 6
```

> 时间复杂度: O(nlogn)

```c++
#include <bits/stdc++.h>
using namespace std;

struct Class {
	int st,ed;
	
	Class (int st, int ed): st(st), ed(ed) {}
	
	bool operator <(struct Class obj) const{
		return ed < obj.ed;
	}
}; 

int n;
int ans = 0;
vector <Class> Classes;

int main() {
	
	cin >> n;
	for (int i = 0; i < n; i++) {
		int st, ed;
		cin >> st >> ed;
		Classes.emplace_back(Class(st, ed));
	}
	
	sort(Classes.begin(), Classes.end());
	
	int ed = -1;
	for (auto Class : Classes) {
		if (Class.st > ed) {
			ed = Class.ed;
			ans++;
		}
	}
	
	cout << ans << endl;
}
```



# 算法设计与分析 4.5 小明和 Johnson's rules
### ★题目描述

有 $n$ 个病人，两位医生（我们叫他们小易和小胜）。每个病人都要找两位医生看病，有的病人要先找小易看病，再找小胜看病；有的病人要先找小胜看病，再找小易看病。第 $i$ 个病人会找小易看 $a_i$ 分钟，找小胜看 $b_i$ 分钟。每个医生每次只能看一位病人，每位病人每次也只能找一个医生，两位医生同时上班，同时下班，并且他们会看完所有病人再下班。请问他们最早下班时间。

### ★输入格式

第一行为一个正整数 $n$ ，表示有 $n$ 个病人。

接下来 $n$ 行，每行有两个正整数 $a_i\ b_i\ c_i$ ，用空格隔开，行末无空格，表示第 $i$ 个病人会找小易看 $a_i$ 分钟，找小胜看 $b_i$ 分钟。

如果 $c_i = 0$，则病人会先找小易看病。

如果 $c_i = 1$，则病人会先找小胜看病。

对于60%的数据，$c_i = 0$。

对于100%的数据，$1 \le n \le 10^6$，$1 \le a_i, b_i \le 10^6$，$0 \le c_i \le 1$。

### ★输出格式
输出一个正整数，表示最早下班时间。

### ★样例输入
```wiki
3
1 2 0
3 4 0
5 6 1
```
### ★样例输出
```wiki
12
```
>1.题目里两个医生可以同时开始工作，并都有要优先看病的病人。相当于两条流水线可以同时开始工作，同时都有要优先进行的任务。
>
>2.首先对两个医生的病人都进行Johnson’s rules排序，目的是让两个医生的工作都尽量饱和。然后将排序结果存进两个队列中。
>
>3.接着按序诊断病人。每看诊完一个“一手病人”，就将该病人加入另一个医生的看诊队列的队尾。如果看诊的是“二手病人”，就直接出队。
>
>用pair<int,int>表示病人，用两个deque<pair<int, int> >表示看诊病人队列。首先用两个pair数组初步记录病人情况，并进行johnson’s rules规则的排序。然后将数组结果存入deque。
>
>
>
>当两个队列不为空时，有五种情况。
>
>第一种指q1为空，q2不空时。总时间T加上q2队首病人的first，如果该病人的second不为0，表示该病人是“一手病人”，就将该病人的second值赋值给first,second为0，加入q1队列的队尾。
>
>第二种指q2为空，q1不为空时。同上处理。
>
>第三、四种指两队都不为空，队首病人的first不等时。判断first更小的病人是哪种病人，分类处理。而first更大的病人需将first减去更小的first重新插入到原队列队首。
>
>第五种，如果first相等时，就用第一种情况的方法同时处理两个队首病人。



```c++
#include<bits/stdc++.h>
using namespace std;
const int N = 1e6 + 7;
vector<pair<int, int> > v[2];
deque<pair<int, int> > pa[2];
int cmp(pair<int, int> a, pair<int, int> b) {
	int aa = a.first - a.second, bb = b.first - b.second;
	if(aa <= 0 && bb <= 0)return a.first < b.first;
	else if(aa > 0 && bb > 0)return a.second > b.second;
	else return aa < bb;
}
int main() {
	int n; scanf("%d", &n);
	for(int i = 0; i < n; i++) {
		int a, b, c; scanf("%d%d%d", &a, &b, &c);
		if(c)swap(a, b);
		v[c].push_back(make_pair(a, b));
	}
	for(int i = 0; i < 2; i++)sort(v[i].begin(), v[i].end(), cmp);
	for(int i = 0; i < 2; i++)for(int j = 0; j < v[i].size(); j++)pa[i].push_back(v[i][j]);
	long long ans = 0;
	while(!pa[0].empty() || !pa[1].empty()) {
		if(pa[0].empty()) {
			pair<int, int> t = pa[1].front(); pa[1].pop_front();
			ans += t.first; t.first = 0;
			if(t.second) {
				swap(t.first, t.second);
				pa[0].push_back(t);
			}
		}
		else if(pa[1].empty()) {
			pair<int, int> t = pa[0].front(); pa[0].pop_front();
			ans += t.first; t.first = 0;
			if(t.second) {
				swap(t.first, t.second);
				pa[1].push_back(t);
			}
		}
		else if(pa[0].front().first < pa[1].front().first) {
			pair<int, int> t = pa[0].front(); pa[0].pop_front();
			int cost = t.first;
			ans += cost; t.first = 0;
			if(t.second) {
				swap(t.first, t.second);
				pa[1].push_back(t);
			}
			t = pa[1].front(); pa[1].pop_front();
			t.first -= cost; pa[1].push_front(t);
		}
		else if(pa[0].front().first > pa[1].front().first) {
			pair<int, int> t = pa[1].front(); pa[1].pop_front();
			int cost = t.first;
			ans += cost; t.first = 0;
			if(t.second) {
				swap(t.first, t.second);
				pa[0].push_back(t);
			}
			t = pa[0].front(); pa[0].pop_front();
			t.first -= cost; pa[0].push_front(t);
		}
		else {
			pair<int, int> t = pa[1].front(); pa[1].pop_front();
			int cost = t.first;
			ans += cost; t.first = 0;
			if(t.second) {
				swap(t.first, t.second);
				pa[0].push_back(t);
			}
			t = pa[0].front(); pa[0].pop_front();
			t.first = 0;
			if(t.second) {
				swap(t.first, t.second);
				pa[1].push_back(t);
			}
		}
	}
	printf("%lld\n", ans);
	return 0;
}
```



# 算法设计与分析 5.1 小明爱数数
### ★题目描述

给定一个自然数 $N$ ，找出一个 $M$ ，使得 $M > 0$ 且 $M$ 是 $N$ 的倍数，并且 $M$ 的十进制表示只包含 $0$ 或  $1$ 。求符合条件的最小的 $M$ 。

### ★输入格式

第一行为一个正整数 $N$ 。

对于100%的数据，$1 \le N \le 10^6$。

### ★输出格式

输出符合条件的最小的 $M$ 。

### ★样例输入
```wiki
4
```
### ★样例输出
```wiki
100
```
>思路：
>
>运用队列存储余数，逐一判断，知道满足条件，每次在M后面添加一位数a(这个数不是0就是1)，使M扩大为10*M+a。*
>
>*即：原数为M，扩大后的数为M1=10*M+a，将其对mod取模，
>
>则：mod_M=M%mod 、mod_M1=(10*M+a)%mod = (10*M%mod+a)%mod = (10*mod_M+a)%mod由此我们可以知道，
>
>当M满足条件时，则：mod_M1=0
>
>时间复杂度：O(n)

```c++
#include <bits/stdc++.h>
using namespace std;

const int maxn = 1e6 + 5;

int a[maxn], b[maxn], c[maxn];
int n;

int before[maxn]; //存放余数为i是的上一次余数
int worth[maxn];  //当余数为i时，worth为1 

void Prin(int i) {
	if (before[i] != -1) {
		Prin(before[i]);
	}
	cout << worth[i];
}

void bfs(int n) {
	queue <int> q;
	q.push(1);
	before[1] = -1;
	worth[1] = 1;
	
	while (!q.empty()) {
		int x = q.front();
		q.pop();
		
		int x0 = (x * 10) % n;
		if (!before[x0]) {
			before[x0] = x;
			worth[x0] = 0;
			q.push(x0);
		}
		if (x0 == 0) {
			Prin(x0);
			break;
		}
		
		int x1 = (x * 10 + 1) % n;
		if (!before[x1]) {
			before[x1] = x;
			worth[x1] = 1;
			q.push(x1);
		}
		if (x1 == 0) {
			Prin(x1);
			break;
		}
	}
}

int main() {
	cin >> n;
	if (n == 1) {
		cout << "1" << endl;
		return 0;  
	}
	
	bfs(n);
	
}
```



# 算法设计与分析 5.2 小明和序列
### ★题目描述

给你一个长度为 $n$ 的数列，此数列共有 $2^n- 1$ 个非空子序列，把每个子序列的元素和列出来共有 $2^n- 1$ 个数字，求当中第 $k$ 小的数。 

### ★输入格式

第一行为一个正整数 $n$。

接下来一行，有 $n$ 个正整数 $a_i$ ，用空格隔开，行末无空格。

对于30%的数据，$1 \le n \le 20$。

对于100%的数据，$1 \le n \le 10^4$，$1 \le k \le min(10^6, 2 ^ n - 1)$，$1 \le a_i \le 10^8$。

### ★输出格式

输出一个数，表示第 $k$ 小的数。

### ★样例输入
```wiki
5 30
4 2 1 16 8
```
### ★样例输出
```wiki
30
```
> 思路：
>
> 1、用树的左右结点表示，选不选这个数，结点的值表示当前的序列和，叶节点就是所有的子序列的和
>
> 2、用优先队列存储当前前k小序列和，当队列长度小于k时，直接加入叶子节点值，当队列长度等于k时，比较当前叶子结点值和优先队列队首的值，若小于，则将队首出队，当前叶子结点值加入优先队列。
>
> 3、剪枝：比较当前序列和和优先队列队首的值，若大于等于则剪枝
>
> 4、可先将序列按从大到小排序，让序列和小的尽可能先出现
>
> 时间复杂度:O(2^n)

```c++
#include <bits/stdc++.h>
using namespace std;

const int maxn = 10005;

int n, k;
int a[maxn];
long long ans;

struct Node {
	int idx;
	long long val;
	
	Node(int idx, long long val): idx(idx), val(val) {}
	
	bool operator <(Node obj) const {
		return val > obj.val;
	}
};

int main() {
	
	cin >> n >> k;
	
	for (int i = 0; i < n; i++) {
		cin >> a[i];
	}
	sort(a, a + n);
	
	priority_queue <Node> pq;
	pq.push(Node(0, a[0]));
	while (!pq.empty() && k--) {
		auto now = pq.top();
		pq.pop();
		ans = now.val;
		if (now.idx < n - 1) {
			pq.push(Node(now.idx + 1, now.val + a[now.idx + 1]));
			pq.push(Node(now.idx + 1, now.val + a[now.idx + 1] - a[now.idx]));
		}
	}
	
	cout << ans << endl;
	
}
```



# 算法设计与分析 5.3 小明坐地铁

### ★题目描述

一维数轴上有 $n$ 个地铁站，假设第 $i$ 个地铁站的坐标在 $x_i$ 。小明从第 $a_0$ 站出发，坐到第 $a_1$ 站，再从第 $a_1$ 站出发，坐到第 $a_2$ 站，...，再从第 $a_{m - 1}$ 站出发，坐到第 $a_m$ 站。现在你不知道 $a_i$ 具体的值，只知道 $d_i = |x_{a_{i - 1}} - x_{a_i}|$。请问 $n$ 最小可能是多少？

### ★输入格式

第一行为一个正整数 $m$。

接下来一行，有 $m$ 个正整数 $d_i$ ，用空格隔开，行末无空格。

对于30%的数据，$1 \le m \le 20$。

对于100%的数据，$1 \le m \le 25$，$1 \le d_i \le 10^5$。

### ★输出格式

输出一个数，表示 $n$ 最小可能是多少。

### ★样例输入

```wiki
2
3 3
```

### ★样例输出

```wiki
2
```

>思路：
>
>1、用树的左右结点表示，下一个站点在上一个站点的左边还是右边，结点的值表示站点所在的位置，假设a0站点在位置0
>
>2、通过判断每条到叶子结点的路径上有多少个值不同的点，来得到站点最小可能是多少
>
>3、采用计数数组来存储站点出现的次数，当值从0->1时，站点数+1；当值从1->0时，站点数-1
>
>4、到叶子结点时，比较当前站点数与当前最小站点数
>
>5、剪枝：当前的站点数若已经大于当前最小站点数，则剪枝
>
>时间复杂度：O(2^n)

```c++
#include <bits/stdc++.h>
using namespace std;

int m;

long long a[30];
int ans = 26;
long long imax;

bool mp[6000000];
 
void dfs(int idx, long long now,  int count) {
	if (count > ans ) {
		return;
	}
	if (idx == m + 1) {
		ans = min(ans, count);
		return;
	}

	if (mp[now + a[idx]]) {
		dfs(idx + 1, now + a[idx], count);
	}
	if (mp[now - a[idx]]) {
		dfs(idx + 1, now - a[idx],  count);
	}
	if (!mp[now + a[idx]]) {
		mp[now + a[idx]] = true;
		dfs(idx + 1, now + a[idx], count + 1);
		mp[now + a[idx]] = false;
	}
	if (!mp[now - a[idx]]) {
		mp[now - a[idx]] = true;
		dfs(idx + 1, now - a[idx],  count + 1);
		mp[now - a[idx]] = false;
	}
}



int main() {
	cin >> m;
	for (int i = 1 ; i <= m; i++) {
		cin >> a[i];
		imax += a[i];
	}
	imax +=  2500005;
	
	mp[2500005] = true;
	dfs(1, 2500005, 1);
	
	cout << ans << endl;
}
```



# 算法设计与分析 5.4 小明和最小点覆盖

### ★题目描述

给定 $n$ 个点 $m$ 条边的无向图，无重边无自环，至少要选几个点才能使得所有边所连接的两个点至少都有一个点被选到？若所需点数超过 $\lceil log_2n \rceil$ ，则输出 $-1$。

### ★输入格式

第一行为两个正整数 $n$，$m$ ，用空格隔开，行末无空格。

接下来 $m$ 行，每行有两个正整数 $u$，$v$ ，用空格隔开，行末无空格，表示边 $(u, v)$ 。

对于30%的数据，$1 \le n \le 16$。

对于100%的数据，$1 \le n \le 512$，$1 \le m \le \frac{n(n - 1)}{2}$。

### ★输出格式

输出一个数，表示所需的最少点数。若所需点数超过 $\lceil log_2n \rceil$ ，则输出 $-1$。

### ★样例输入
```wiki
3 2
1 2
1 3
```
### ★样例输出
```wiki
1
```
>思路：
>
>枚举边
>
>时间复杂度：O(n^2 * log(n))
>
>每个叶子节点到树根最多有⌈log_2⁡n ⌉+1个红色节点（包括根节点）
>
>叶子节点最多有2^(⌈log_2⁡n ⌉" " +1)个
>
>由边数超过(n-1)⌈log_2⁡n ⌉就可以直接输出-1 ，得每个叶子节点到树根最多有(n-1)⌈log_2⁡n ⌉  个节点
>
>因此，时间复杂度为O(2^(⌈log_2⁡N ⌉ +1) × (n-1)⌈log_2⁡n ⌉)=O(n^2 log⁡n)

```c++
#include <bits/stdc++.h>
using namespace std;

const int maxn = 1000;

int n, m;
bool vis[maxn];
int ans = INT_MAX;

struct Edge {
	int u, v;
};
Edge e[maxn * maxn];

void dfs(int i, int cnt) {
	if (cnt >= ans) {
		return;
	}
	if (i == m) {
		ans = cnt;
		return; 
	}
	
	if (vis[e[i].u] || vis[e[i].v]) {
		dfs(i + 1, cnt);
	}
	else {
		vis[e[i].u] = true;
		dfs(i + 1, cnt + 1);
		vis[e[i].u] = false;
		
		vis[e[i].v] = true;
		dfs(i + 1, cnt + 1);
		vis[e[i].v] = false;
	}
}


int main() {
	
	cin >> n >> m;
	
	for (int i = 0; i < m; i++) {
		cin >> e[i].u >> e[i].v;
	}
	
	dfs(0, 0);

	int minPoint = log(n - 1) / log(2) + 1;
	if (ans > minPoint) {
		cout << -1 << endl;
	}
	else {
		cout << ans << endl;
	}
	
}
```



# 算法设计与分析 5.5 小明和第K小带权匹配
### ★题目描述

给你 $2n$ 个点，$m$ 条边的二分图，二分图的两边各有 $n$ 个点，每条边只会从左边的点连向右边的点，没有重边，求权重和第 $k$ 小的匹配的值。

注：一个匹配是指给定图的一个边的子集（空集是任意集合的子集），其中任意两条边都没有公共的顶点。

### ★输入格式

第一行为两个正整数 $n$，$m$，$k$ ，用空格隔开，行末无空格。

接下来 $m$ 行，每行有三个正整数 $u$，$v$，$w$ ，用空格隔开，行末无空格，表示边 $(u, v)$，权值为$w$ 。

对于30%的数据，$1 \le m \le 20$。

对于100%的数据，$1 \le n \le 20$，$1 \le m \le 100$，$1 \le k \le 10^5$，$1 \le u, v \le n$，$1 \le w \le 1000$。

### ★输出格式

输出一个数，表示权重和第 $k$ 小的匹配的值。无解输出 $-1$ 。

### ★样例输入
```wiki
2 4 7
1 1 1
1 2 2
2 1 3
2 2 4
```
### ★样例输出
```wiki
5
```

> 思路：枚举加剪枝，维护长度大小为 k 的优先队列。
>
> 时间复杂度：......

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

const int maxn = 25;
priority_queue <ll, vector <ll>, less<ll> > pq;

int n, m, k;
int e[maxn][maxn];
bool vis[maxn];

void dfs(int idx, ll sum) {
	
	if (pq.size() == k && sum >= pq.top()) {
		return;
	}
	
	if (idx == n + 1) {
		pq.push(sum);
		if (pq.size() > k) {
			pq.pop();
		}
		return;
	} 
	
	dfs(idx + 1, sum);
	
	for (int i = 1; i <= n; i++) {
		if (e[idx][i] != 0 && !vis[i]) {
			vis[i] = true;
			dfs(idx + 1, sum + e[idx][i]);
			vis[i] = false;
		}
	}
}

int main() {
	
	cin >> n >> m >> k;
	
	for (int i = 1; i <= m; i++) {
		int u, v, w;
		cin >> u >> v >> w;
		e[u][v] = w;
	}
	
	dfs(1, 0);
	
	if (pq.size() != k) {
		cout << -1 << endl;
	}
	else {
		cout << pq.top() << endl;
	}
	
}
```

