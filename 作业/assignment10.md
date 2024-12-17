# Assignment #10: dp & bfs

Updated 2 GMT+8 Nov 25, 2024

2024 fall, Complied by 周晟昱 工学院



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### LuoguP1255 数楼梯

dp, bfs, https://www.luogu.com.cn/problem/P1255

思路：

设$dp_i$为上$i$级楼梯的方案数，显然有$dp_1=1,dp_2=2,dp_i=dp_{i-2}+dp_{i-1}(i\ge 3)$，按此递推计算即可。

代码：

```python
dp = [1, 1]
for _ in range(1, int(input())):
    dp.append(dp[-1] + dp[-2])
print(dp[-1])
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>



![](C:\Users\sheng\Downloads\assignment10.assets\屏幕截图 2024-11-26 152754.png)

### 27528: 跳台阶

dp, http://cs101.openjudge.cn/practice/27528/

思路：

设$dp_i$为上$i$级台阶的方案数，则有$dp_0=1,dp_i=\sum_{j=0}^{i-1}dp_j(i\ge1)$，据此计算即可。

代码：

```python
dp = [0] * (int(input()) + 1)
dp[0] = 1
for i in range(1, len(dp)):
    dp[i] = sum(dp[:i])
print(dp[-1])
```



代码运行截图 ==（至少包含有"Accepted"）==



![](C:\Users\sheng\Downloads\assignment10.assets\屏幕截图 2024-11-26 153303.png)

### 474D. Flowers

dp, https://codeforces.com/problemset/problem/474/D

思路：

设$dp_i$为吃$i$朵花的方案数，则有$dp_0=1,dp_i=\begin{cases}dp_{i-1},i<k\\dp_{i-1}+dp_{i-k},i\ge k\end{cases}$，据此计算即可。可以用前缀和加快区间和查询速度。

代码：

```c++
#include <array>
#include <iostream>

int main()
{
    int t, k, a, b;
    std::cin >> t >> k;
    std::array<int, 100001> dp{1};
    for (int i = 1; i < dp.size(); ++i)
        dp[i] = (dp[i - 1] + (i >= k ? dp[i - k] : 0)) % 1000000007;
    for (int i = 1; i < dp.size(); ++i)
        dp[i] = (dp[i] + dp[i - 1]) % 1000000007;
    while (t--)
    {
        std::cin >> a >> b;
        std::cout << (dp[b] - dp[a - 1] < 0 ? dp[b] - dp[a - 1] + 1000000007 : dp[b] - dp[a - 1]) << std::endl;
    }
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>



![](C:\Users\sheng\Downloads\assignment10.assets\屏幕截图 2024-11-26 154005.png)

### LeetCode5.最长回文子串

dp, two pointers, string, https://leetcode.cn/problems/longest-palindromic-substring/

思路：

枚举所有可能的字串，取最长者返回。

代码：

```c++
class Solution {
public:
    string longestPalindrome(string s) {
        int pos1 = 0, len1 = 0, pos2 = 0, len2 = 0;
        for(int j, i = 0; i < s.size(); ++i)
        {
            for(j = 0; i - j >= 0 && i + j < s.size(); ++j)
                if(s[i - j] != s[i + j])
                    break;
            if(len1 < j - 1)
            {
                len1 = j - 1;
                pos1 = i;
            }
        }
        for(int j, i = 0; i + 1 < s.size(); ++i)
        {
            for(j = 0; i - j >= 0 && i + j + 1 < s.size(); ++j)
                if(s[i - j] != s[i + j + 1])
                    break;
            if(len2 < j)
            {
                len2 = j;
                pos2 = i;
            }
        }
        if(2 * len1 + 1 > 2 * len2)
            return s.substr(pos1 - len1, 2 * len1 + 1);
        else
            return s.substr(pos2 - len2 + 1, 2 * len2);
    }
};
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>



![](C:\Users\sheng\Downloads\assignment10.assets\屏幕截图 2024-11-26 160955.png)



### 12029: 水淹七军

bfs, dfs, http://cs101.openjudge.cn/practice/12029/

思路：

用搜索模拟水流即可。

（自己的代码永远Runtime Error，所以用了题解的大部分内容，只有dfs函数用自己的思路重写了。这题的输入输出平等地卡掉了我的C++和Python代码😡）

代码：

```python
import sys
sys.setrecursionlimit(300000)
input = sys.stdin.read
def is_valid(x, y, m, n):
    return 0 <= x < m and 0 <= y < n
def dfs(x, y, water_height_value, m, n, h, water_height):
    if (not is_valid(x, y, m, n)) or water_height[x][y] >= water_height_value or h[x][y] > water_height_value:
        return
    water_height[x][y] = water_height_value
    for dx, dy in [(0, 1), (0, -1), (1, 0), (-1, 0)]:
        dfs(x + dx, y + dy, water_height_value, m, n, h, water_height)
data = input().split()
idx = 0
k = int(data[idx])
idx += 1
results = []
for _ in range(k):
    m, n = map(int, data[idx:idx + 2])
    idx += 2
    h = []
    for i in range(m):
        h.append(list(map(int, data[idx:idx + n])))
        idx += n
    water_height = [[0] * n for _ in range(m)]
    i, j = map(int, data[idx:idx + 2])
    idx += 2
    i, j = i - 1, j - 1
    p = int(data[idx])
    idx += 1
    for _ in range(p):
        x, y = map(int, data[idx:idx + 2])
        idx += 2
        x, y = x - 1, y - 1
        if h[x][y] <= h[i][j]:
            continue
        dfs(x, y, h[x][y], m, n, h, water_height)
    results.append("Yes" if water_height[i][j] > 0 else "No")
sys.stdout.write("\n".join(results) + "\n")
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>



![](C:\Users\sheng\Downloads\assignment10.assets\屏幕截图 2024-11-26 175248.png)

### 02802: 小游戏

bfs, http://cs101.openjudge.cn/practice/02802/

思路：

用BFS从起点不断搜索即可。

代码：

```c++
#include <cstdio>
#include <iostream>
#include <map>
#include <queue>
#include <utility>
#include <vector>

int main()
{
    int w, h, cnt = 0;
    while (true)
    {
        std::cin >> w >> h;
        if (w == 0 && h == 0)
            break;
        std::cout << "Board #" << ++cnt << ':' << std::endl;
        std::vector<std::vector<bool>> map(w + 2, std::vector<bool>(h + 2, false));
        for (int i = 1; i <= h; ++i)
        {
            std::getchar();
            for (int j = 1; j <= w; ++j)
                map[j][i] = std::getchar() == 'X';
        }
        int x1, y1, x2, y2, cntt = 0;
        while (true)
        {
            std::cin >> x1 >> y1 >> x2 >> y2;
            if (x1 == 0 && y1 == 0 && x2 == 0 && y2 == 0)
                break;
            std::cout << "Pair " << ++cntt << ": ";
            std::map<std::pair<int, int>, int> m;
            std::queue<std::pair<int, std::pair<int, int>>> q;
            q.push(std::make_pair(0, std::make_pair(x1, y1)));
            m[std::make_pair(x1, y1)] = 0;
            bool flag = false;
            while (!q.empty())
            {
                const auto [dis, pos] = q.front();
                q.pop();
                for (int i = 1; pos.first + i <= w + 1; ++i)
                {
                    if (pos.first + i == x2 && pos.second == y2)
                    {
                        std::cout << dis + 1 << " segments." << std::endl;
                        flag = true;
                        break;
                    }
                    if (map[pos.first + i][pos.second] || m.find(std::make_pair(pos.first + i, pos.second)) != m.end())
                        break;
                    m[std::make_pair(pos.first + i, pos.second)] = dis + 1;
                    q.push(std::make_pair(dis + 1, std::make_pair(pos.first + i, pos.second)));
                }
                if (flag)
                    break;
                for (int i = 1; pos.first - i >= 0; ++i)
                {
                    if (pos.first - i == x2 && pos.second == y2)
                    {
                        std::cout << dis + 1 << " segments." << std::endl;
                        flag = true;
                        break;
                    }
                    if (map[pos.first - i][pos.second] || m.find(std::make_pair(pos.first - i, pos.second)) != m.end())
                        break;
                    m[std::make_pair(pos.first - i, pos.second)] = dis + 1;
                    q.push(std::make_pair(dis + 1, std::make_pair(pos.first - i, pos.second)));
                }
                if (flag)
                    break;
                for (int i = 1; pos.second + i <= h + 1; ++i)
                {
                    if (pos.first == x2 && pos.second + i == y2)
                    {
                        std::cout << dis + 1 << " segments." << std::endl;
                        flag = true;
                        break;
                    }
                    if (map[pos.first][pos.second + i] || m.find(std::make_pair(pos.first, pos.second + i)) != m.end())
                        break;
                    m[std::make_pair(pos.first, pos.second + i)] = dis + 1;
                    q.push(std::make_pair(dis + 1, std::make_pair(pos.first, pos.second + i)));
                }
                if (flag)
                    break;
                for (int i = 1; pos.second - i >= 0; ++i)
                {
                    if (pos.first == x2 && pos.second - i == y2)
                    {
                        std::cout << dis + 1 << " segments." << std::endl;
                        flag = true;
                        break;
                    }
                    if (map[pos.first][pos.second - i] || m.find(std::make_pair(pos.first, pos.second - i)) != m.end())
                        break;
                    m[std::make_pair(pos.first, pos.second - i)] = dis + 1;
                    q.push(std::make_pair(dis + 1, std::make_pair(pos.first, pos.second - i)));
                }
                if (flag)
                    break;
            }
            if (!flag)
                std::cout << "impossible." << std::endl;
        }
        std::cout << std::endl;
    }
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>



![](C:\Users\sheng\Downloads\assignment10.assets\屏幕截图 2024-11-26 211359.png)

## 2. 学习总结和收获

代码长的搜索题目写起来确实耗费大量精力……尤其是作业后两道题调试各种稀奇古怪的bug花了一个晚上。以及一开始没看到最后一道题的“每组数据后输出一个空行”导致对着Presentation Error看了半天……



