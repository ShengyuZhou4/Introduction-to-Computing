# Assignment #B: Dec Mock Exam大雪前一天

Updated 1649 GMT+8 Dec 5, 2024

2024 fall, Complied by 周晟昱 工学院



**说明：**

1）⽉考： AC3 。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### E22548: 机智的股民老张

http://cs101.openjudge.cn/practice/22548/

思路：

处理出每个日期之后股价的最大值，计算最大差值即可。

代码：

```python
a = list(map(int, input().split()))
max_after = [0] * len(a)
max_after[-1] = a[-1]
for i in range(len(a) - 2, -1, -1):
    if a[i] > max_after[i + 1]:
        max_after[i] = a[i]
    else:
        max_after[i] = max_after[i + 1]
ans = 0
for i in range(0, len(a)):
    ans = max(ans, max_after[i] - a[i])
print(ans)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>



![](C:\Users\sheng\Downloads\assignmentB.assets\屏幕截图 2024-12-06 144947.png)

### M28701: 炸鸡排

greedy, http://cs101.openjudge.cn/practice/28701/

思路：

显然答案不会超过$\frac{\sum_{i=1}^{n}t_i}{k}$，但如果有鸡排炸熟所需时间大于此时间则无法达成此结果，此时可以让此鸡排一直放在锅中，原问题转化为$n,k $均减一的问题。根据并不显然的推导，如果没有这样的鸡排则一定可以达成最大时间。

代码：

```c++
#include <iomanip>
#include <ios>
#include <iostream>
#include <queue>
int main()
{
    int n, k;
    double sum = 0.0, ti;
    std::cin >> n >> k;
    std::priority_queue<double> t;
    for (int i = 0; i < n; i++)
    {
        std::cin >> ti;
        sum += ti;
        t.push(ti);
    }
    while (k != 1 && t.top() > sum / k)
    {
        sum -= t.top();
        t.pop();
        --k;
    }
    std::cout << std::fixed << std::setprecision(3) << sum / k << std::endl;
    return 0;
}
```



代码运行截图 ==（至少包含有"Accepted"）==



![](C:\Users\sheng\Downloads\assignmentB.assets\屏幕截图 2024-12-06 145816.png)

### M20744: 土豪购物

dp, http://cs101.openjudge.cn/practice/20744/

思路：

设$dp_{0,i}$表示在未放回商品且以第$i$件商品为所获得的最后一件商品时的能获得的最大价值，$dp_{1,i}$表示在已放回商品且以第$i$件商品为所获得的最后一件商品时的能获得的最大价值，有$dp_{0,i}=\max\{dp_{0,i-1},0\}+price_i,dp_{1,i}=\max\{dp_{1,i-1},dp_{0,i-2}\}+price_i$。据此递推计算即可。

代码：

```python
prices = list(map(int, input().split(',')))
if max(prices) <= 0 or len(prices) == 1:
    print(max(prices))
else:
    dp = [[0] * len(prices), [0] * len(prices)]
    dp[0][0] = prices[0]
    dp[1][0] = -1000000000
    dp[0][1] = max(0, prices[0]) + prices[1]
    dp[1][1] = max(prices[0], prices[1])
    for i in range(2, len(prices)):
        dp[1][i] = max(dp[1][i - 1], dp[0][i - 2]) + prices[i]
        dp[0][i] = max(dp[0][i - 1], 0) + prices[i]
    print(max(max(dp[0]), max(dp[1])))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>



![](C:\Users\sheng\Downloads\assignmentB.assets\屏幕截图 2024-12-06 150422.png)

### T25561: 2022决战双十一

brute force, dfs, http://cs101.openjudge.cn/practice/25561/

思路：

枚举每件商品购买的店铺，计算购买方案对应的价格，取最小值即可。

代码：

```python
n, m = map(int, input().split())
ans = [1000000000]
price = [[-1] * m for _ in range(n)]
store_sum = [0] * m
ticket = [[] for _ in range(m)]
for i in range(n):
    sss = input()
    for ss in sss.split():
        st, pr = map(int, ss.split(':'))
        price[i][st - 1] = pr
for i in range(m):
    sss = input()
    for ss in sss.split():
        ticket[i].append(tuple(map(int, ss.split('-'))))
        ticket[i].sort(key=lambda x: -x[1])
        
def dfs(goods_cnt, sum, ans, store_sum, price, ticket):
    if goods_cnt == n:
        t = 0
        for i in range(m):
            for lim, dis in ticket[i]:
                if store_sum[i] >= lim:
                    t -= dis
                    break
        t += sum
        t -= 50 * (sum // 300)
        ans[0] = min(ans[0], t)
    else:
        for i in range(m):
            if price[goods_cnt][i] != -1:
                store_sum[i] += price[goods_cnt][i]
                dfs(goods_cnt + 1, sum + price[goods_cnt][i], ans, store_sum, price, ticket)
                store_sum[i] -= price[goods_cnt][i]

dfs(0, 0, ans, store_sum, price, ticket)
print(ans[0])

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>



![](C:\Users\sheng\Downloads\assignmentB.assets\屏幕截图 2024-12-06 154041.png)

### T20741: 两座孤岛最短距离

dfs, bfs, http://cs101.openjudge.cn/practice/20741/

思路：

先通过一次DFS遍历第一座孤岛，存储其所有点的坐标，随后使之“沉没”。在扫描到陆地（必属于第二座孤岛）后，计算其与之前存储的所有点间最短路径的长度，取最小值即可。

代码：

```c++
#include <algorithm>
#include <cmath>
#include <iostream>
#include <utility>
#include <vector>

int n, ans = 1000000000;
std::vector<std::vector<int>> mp;
std::vector<std::pair<int, int>> land1;

void build_island(const int &x, const int &y)
{
    if (mp[x][y] == 0)
        return;
    land1.push_back(std::make_pair(x, y));
    mp[x][y] = 0;
    if (x - 1 >= 0)
        build_island(x - 1, y);
    if (x + 1 < n)
        build_island(x + 1, y);
    if (y - 1 >= 0)
        build_island(x, y - 1);
    if (y + 1 < n)
        build_island(x, y + 1);
}

int main()
{
    char c;
    std::cin >> n;
    mp.resize(n, std::vector<int>(n, 0));
    for (int i = 0; i < n; ++i)
        for (int j = 0; j < n; ++j)
        {
            std::cin >> c;
            mp[i][j] = c - '0';
        }
    bool flag = true;
    for (int i = 0; i < n; ++i)
        for (int j = 0; j < n; ++j)
            if (mp[i][j])
            {
                if (flag)
                {
                    build_island(i, j);
                    flag = false;
                }
                else
                    for (auto &[a, b] : land1)
                        ans = std::min(ans, std::abs(i - a) + std::abs(j - b) - 1);
            }
    std::cout << ans << std::endl;
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>



![](C:\Users\sheng\Downloads\assignmentB.assets\屏幕截图 2024-12-06 151024.png)

### T28776: 国王游戏

greedy, http://cs101.openjudge.cn/practice/28776

思路：

经过推导可以发现，只要将所有大臣按照左右手数字的乘积从小到大排序，就能满足题意。

代码：

```python
n = int(input())
a0, b0 = map(int, input().split())
ls = [tuple(map(int, input().split())) for _ in range(n)]
ls.sort(key=lambda x: x[0] * x[1])
ans = 0
for a, b in ls:
    ans = max(a0 // b, ans)
    a0 *= a
print(ans)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>



![](C:\Users\sheng\Downloads\assignmentB.assets\屏幕截图 2024-12-06 170030.png)

## 2. 学习总结和收获

这次考试确实写不完……其实本来可以AC4的，但是双十一的输入用C++实在太难处理，浪费了大量时间后才开始用Python重新写，结果就来不及了……贪心策略也想不出来……





