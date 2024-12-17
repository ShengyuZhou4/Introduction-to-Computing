# Assignment #D: 十全十美 

Updated 1254 GMT+8 Dec 17, 2024

2024 fall, Complied by 周晟昱 工学院



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 02692: 假币问题

brute force, http://cs101.openjudge.cn/practice/02692

思路：

依次考虑每个银币为假币以及比真币重或轻的可能性，检验其是否与称量结果一致，若一致则直接输出。

代码：

```c++
#include <iostream>

int main()
{
    int n;
    std::cin >> n;
    std::string a[3], b[3], c[3];
    while (n--)
    {
        for (int i = 0; i < 3; i++)
            std::cin >> a[i] >> b[i] >> c[i];
        bool flag;
        // counterfeit coin is light
        for (char counterfeit_coin = 'A'; counterfeit_coin <= 'L'; ++counterfeit_coin)
        {
            flag = true;
            for (int af, bf, i = 0; i < 3; ++i)
            {
                af = a[i].find(counterfeit_coin);
                bf = b[i].find(counterfeit_coin);
                if (c[i] == "even")
                    flag = flag && af == std::string::npos && bf == std::string::npos;
                else if (c[i] == "up")
                    flag = flag && af == std::string::npos && bf != std::string::npos;
                else
                    flag = flag && af != std::string::npos && bf == std::string::npos;
                if (!flag)
                    break;
            }
            if (flag)
            {
                std::cout << counterfeit_coin << " is the counterfeit coin and it is light." << std::endl;
                break;
            }
        }
        if (flag)
            continue;
        // counterfeit coin is heavy
        for (char counterfeit_coin = 'A'; counterfeit_coin <= 'L'; ++counterfeit_coin)
        {
            flag = true;
            for (int af, bf, i = 0; i < 3; ++i)
            {
                af = a[i].find(counterfeit_coin);
                bf = b[i].find(counterfeit_coin);
                if (c[i] == "even")
                    flag = flag && af == std::string::npos && bf == std::string::npos;
                else if (c[i] == "up")
                    flag = flag && af != std::string::npos && bf == std::string::npos;
                else
                    flag = flag && af == std::string::npos && bf != std::string::npos;
                if (!flag)
                    break;
            }
            if (flag)
            {
                std::cout << counterfeit_coin << " is the counterfeit coin and it is heavy." << std::endl;
                break;
            }
        }
    }
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>



![](C:\Users\sheng\Downloads\assignmentD.assets\屏幕截图 2024-12-17 145624.png)

### 01088: 滑雪

dp, dfs similar, http://cs101.openjudge.cn/practice/01088

思路：

记忆化搜索。记录每个点出发的合法路径的最大长度，用DFS寻找所有可能路径取最大值即可。

代码：

```c++
#include <climits>
#include <iostream>
#include <vector>

void dfs(int x, int y, const std::vector<std::vector<int>> &h, std::vector<std::vector<int>> &max_length)
{
    if (max_length[x][y])
        return;
    max_length[x][y] = 1;
    if (h[x][y] > h[x - 1][y])
    {
        dfs(x - 1, y, h, max_length);
        max_length[x][y] = std::max(max_length[x][y], max_length[x - 1][y] + 1);
    }
    if (h[x][y] > h[x + 1][y])
    {
        dfs(x + 1, y, h, max_length);
        max_length[x][y] = std::max(max_length[x][y], max_length[x + 1][y] + 1);
    }
    if (h[x][y] > h[x][y - 1])
    {
        dfs(x, y - 1, h, max_length);
        max_length[x][y] = std::max(max_length[x][y], max_length[x][y - 1] + 1);
    }
    if (h[x][y] > h[x][y + 1])
    {
        dfs(x, y + 1, h, max_length);
        max_length[x][y] = std::max(max_length[x][y], max_length[x][y + 1] + 1);
    }
}
int main()
{
    int r, c, ans = 0;
    std::cin >> r >> c;
    std::vector<std::vector<int>> h(r + 2, std::vector<int>(c + 2, INT_MAX)), max_length(r + 2, std::vector<int>(c + 2, 0));
    for (int i = 1; i <= r; ++i)
        for (int j = 1; j <= c; ++j)
            std::cin >> h[i][j];
    for (int i = 1; i <= r; ++i)
        for (int j = 1; j <= c; ++j)
            dfs(i, j, h, max_length);
    for (int i = 1; i <= r; ++i)
        for (int j = 1; j <= c; ++j)
            ans = std::max(ans, max_length[i][j]);
    std::cout << ans << std::endl;
    return 0;
}
```



代码运行截图 ==（至少包含有"Accepted"）==



![](C:\Users\sheng\Downloads\assignmentD.assets\屏幕截图 2024-12-17 151043.png)

### 25572: 螃蟹采蘑菇

bfs, dfs, http://cs101.openjudge.cn/practice/25572/

思路：

用搜索遍历螃蟹能够达到的位置即可。原来使用DFS，由于在本机运行会爆栈，遂改为BFS实现。

代码：

```c++
#include <algorithm>
#include <iostream>
#include <queue>
#include <utility>
#include <vector>

bool bfs(int xx, int yy, bool horizontal, std::vector<std::vector<bool>> &visited, const std::vector<std::vector<bool>> &wall, const std::pair<int, int> &mushroom)
{
    visited[xx][yy] = true;
    std::queue<std::pair<int, int>> q;
    q.push({xx, yy});
    if (horizontal)
        while (!q.empty())
        {
            auto [x, y] = q.front();
            q.pop();
            if (std::make_pair(x, y) == mushroom || std::make_pair(x, y + 1) == mushroom)
                return true;
            if (!visited[x][y + 1] && !wall[x][y + 2])
                visited[x][y + 1] = true, q.push({x, y + 1});
            if (!visited[x][y - 1] && !wall[x][y - 1])
                visited[x][y - 1] = true, q.push({x, y - 1});
            if (!visited[x - 1][y] && !wall[x - 1][y] && !wall[x - 1][y + 1])
                visited[x - 1][y] = true, q.push({x - 1, y});
            if (!visited[x + 1][y] && !wall[x + 1][y] && !wall[x + 1][y + 1])
                visited[x + 1][y] = true, q.push({x + 1, y});
        }
    else
    {
        while (!q.empty())
        {
            auto [x, y] = q.front();
            q.pop();
            if (std::make_pair(x, y) == mushroom || std::make_pair(x + 1, y) == mushroom)
                return true;
            if (!visited[x - 1][y] && !wall[x - 1][y])
                visited[x - 1][y] = true, q.push({x - 1, y});
            if (!visited[x + 1][y] && !wall[x + 2][y])
                visited[x + 1][y] = true, q.push({x + 1, y});
            if (!visited[x][y - 1] && !wall[x][y - 1] && !wall[x + 1][y - 1])
                visited[x][y - 1] = true, q.push({x, y - 1});
            if (!visited[x][y + 1] && !wall[x][y + 1] && !wall[x + 1][y + 1])
                visited[x][y + 1] = true, q.push({x, y + 1});
        }
    }
    return false;
}
int main()
{
    int n, cnt = 0;
    bool ans;
    std::pair<int, int> mushroom, crab[2];
    std::cin >> n;
    std::vector<std::vector<bool>> wall(n + 2, std::vector<bool>(n + 2, true)), visited(n + 2, std::vector<bool>(n + 2, false));
    for (int t, i = 1; i <= n; ++i)
        for (int j = 1; j <= n; ++j)
        {
            std::cin >> t;
            wall[i][j] = (t == 1);
            if (t == 9)
                mushroom = {i, j};
            if (t == 5)
                crab[cnt++] = {i, j};
        }
    if (crab[0].first == crab[1].first)
        ans = bfs(crab[0].first, std::min(crab[0].second, crab[1].second), true, visited, wall, mushroom);
    else
        ans = bfs(std::min(crab[0].first, crab[1].first), crab[0].second, false, visited, wall, mushroom);
    std::cout << (ans ? "yes" : "no") << std::endl;
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>



![](C:\Users\sheng\Downloads\assignmentD.assets\屏幕截图 2024-12-17 155855.png)

### 27373: 最大整数

dp, http://cs101.openjudge.cn/practice/27373/

思路：

类似0/1背包，以最终获得的数字长度为维度递推即可。其中的大小判断实现较为复杂。

代码：

```python
m = int(input())
n = int(input())
num_list = list(input().split())
dp = [[] for _ in range(m + 1)]
for num in num_list:
    for i in range(m, len(num) - 1, -1):
        if i - len(num) == 0 and ''.join(dp[i]) < num:
            dp[i] = [num]
        elif len(dp[i - len(num)]) != 0:
            temp = dp[i - len(num)][:]
            temp.append(num)
            for j in range(len(temp) - 1, 0, -1):
                if temp[j] + temp[j - 1] > temp[j - 1] + temp[j]:
                    temp[j], temp[j - 1] = temp[j - 1], temp[j]
                else:
                    break
            if ''.join(dp[i]) < ''.join(temp):
                dp[i] = temp
for i in range(m, 0, -1):
    if len(dp[i]) != 0:
        print(''.join(dp[i]))
        break
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>



![](C:\Users\sheng\Downloads\assignmentD.assets\屏幕截图 2024-12-17 172525.png)

### 02811: 熄灯问题

brute force, http://cs101.openjudge.cn/practice/02811

思路：

由于确定了第一行的开关按法后所有开关的状态就已经确定了，所以只需要枚举第一行的操作方案后判断能否关闭所有灯即可。使用位运算进行了枚举。

代码：

```c++
#include <array>
#include <iostream>

int main()
{
    std::array<std::array<bool, 6>, 5> light, button;
    for (int t, i = 0; i < 5; ++i)
        for (int j = 0; j < 6; ++j)
        {
            std::cin >> t;
            light[i][j] = t;
        }
    for (int t = 0; t < 64; ++t)
    {
        std::array<std::array<bool, 6>, 5> state = light;
        for (int i = 0; i < 6; ++i)
            if (t & (1 << i))
            {
                button[0][i] = true;
                if (i != 0)
                    state[0][i - 1] = !state[0][i - 1];
                if (i != 5)
                    state[0][i + 1] = !state[0][i + 1];
                state[1][i] = !state[1][i];
                state[0][i] = !state[0][i];
            }
            else
                button[0][i] = false;
        for (int i = 1; i < 5; ++i)
            for (int j = 0; j < 6; ++j)
            {
                if (!state[i - 1][j])
                {
                    button[i][j] = false;
                    continue;
                }
                button[i][j] = true;
                if (j != 0)
                    state[i][j - 1] = !state[i][j - 1];
                if (j != 5)
                    state[i][j + 1] = !state[i][j + 1];
                state[i][j] = !state[i][j];
                state[i - 1][j] = !state[i - 1][j];
                if(i != 4)
                    state[i + 1][j] = !state[i + 1][j];
            }
        if (state[4] == std::array<bool, 6>{false, false, false, false, false, false})
        {
            for (int i = 0; i < 5; ++i)
                for (int j = 0; j < 6; ++j)
                    std::cout << button[i][j] << (j == 5 ? '\n' : ' ');
            return 0;
        }
    }
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>



![](C:\Users\sheng\Downloads\assignmentD.assets\屏幕截图 2024-12-17 160839.png)

### 08210: 河中跳房子

binary search, greedy, http://cs101.openjudge.cn/practice/08210/

思路：

采用二分答案，对于每个给定的间隔，用贪心找出最少需要移除的石头个数，判断其与上限的大小关系，以此改变查找范围，最终即可找出答案。

代码：

```c++
#include <iostream>
#include <vector>
int check(const std::vector<int> &d, const int &gap, const int &l)
{
    int ans = 0, current = 0;
    for (int i = 0; i < d.size(); ++i)
        if (d[i] - current >= gap)
            current = d[i];
        else
            ++ans;
    if (l - current < gap)
        ++ans;
    return ans;
}
int main()
{
    int l, n, m;
    std::cin >> l >> n >> m;
    std::vector<int> d(n);
    for (auto &i : d)
        std::cin >> i;
    int left = 0, right = l, mid;
    while (left < right)
    {
        mid = (left + right + 1) / 2;
        if (check(d, mid, l) <= m)
            left = mid;
        else
            right = mid - 1;
    }
    std::cout << left << std::endl;
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>



![](C:\Users\sheng\Downloads\assignmentD.assets\屏幕截图 2024-12-17 162514.png)

## 2. 学习总结和收获

最大整数这道题确实能体现Python在代码量上的优势，很复杂的功能实现起来相当简洁，不过不知道是不是数据太小的问题，感觉时间复杂度很高的代码也通过了。不过一开始的代码中dp数组的初始化用的是`dp = [[]] * (m + 1)`，也通过了，但是在Python命令行里试出来这是浅拷贝，会造成列表中所有元素相同。然而我把程序里所有的dp的第一个下标删除后程序并不能正确运行，不知道是什么原理。





