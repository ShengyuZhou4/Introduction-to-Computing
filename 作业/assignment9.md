# Assignment #9: dfs, bfs, & dp

Updated 2107 GMT+8 Nov 19, 2024

2024 fall, Complied by 周晟昱 工学院



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 18160: 最大连通域面积

dfs similar, http://cs101.openjudge.cn/practice/18160

思路：

遍历地图，在遇到尚未被访问的连通块时用DFS统计其大小，取所有结果的最大值即可。

代码：

```c++
#include <algorithm>
#include <array>
#include <iostream>
#include <vector>

int dfs(int x, int y, std::vector<std::vector<bool>> &map, std::vector<std::vector<bool>> &visited)
{
    static const std::array<int, 3> mov{-1, 0, 1};
    visited[x][y] = true;
    int cnt = 1;
    for (int i = 0; i < 3; ++i)
        for (int j = 0; j < 3; ++j)
            if (map[x + mov[i]][y + mov[j]] && !visited[x + mov[i]][y + mov[j]])
                cnt += dfs(x + mov[i], y + mov[j], map, visited);
    return cnt;
}
int main()
{
    int t;
    std::cin >> t;
    while (t--)
    {
        int n, m, ans = 0;
        char c;
        std::cin >> n >> m;
        std::vector<std::vector<bool>> map(n + 2, std::vector<bool>(m + 2, false)), visited(n + 2, std::vector<bool>(m + 2, false));
        for (int i = 1; i <= n; ++i)
            for (int j = 1; j <= m; ++j)
            {
                std::cin >> c;
                map[i][j] = (c == 'W');
            }
        for (int i = 1; i <= n; ++i)
            for (int j = 1; j <= m; ++j)
                if (map[i][j] && !visited[i][j])
                    ans = std::max(ans, dfs(i, j, map, visited));
        std::cout << ans << std::endl;
    }
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>



![](C:\Users\sheng\Downloads\assignment9.assets\屏幕截图 2024-11-20 090623.png)

### 19930: 寻宝

bfs, http://cs101.openjudge.cn/practice/19930

思路：

从起点用BFS不断搜索即可。

代码：

```c++
#include <array>
#include <iostream>
#include <queue>
#include <utility>
#include <vector>

enum class type
{
    normal,
    treasure,
    trap
};
int bfs(std::vector<std::vector<type>> &map)
{
    std::queue<std::pair<int, int>> q;
    std::vector<std::vector<int>> dis(map.size(), std::vector<int>(map[0].size(), -1));
    q.push(std::make_pair(1, 1));
    dis[1][1] = 0;
    std::array<int, 4> mov_x = {0, 0, 1, -1}, mov_y = {1, -1, 0, 0};
    while (!q.empty())
    {
        auto [x, y] = q.front();
        q.pop();
        if (map[x][y] == type::treasure)
            return dis[x][y];
        for (int i = 0; i < 4; ++i)
        {
            int nx = x + mov_x[i], ny = y + mov_y[i];
            if (map[nx][ny] == type::trap || dis[nx][ny] != -1)
                continue;
            q.push(std::make_pair(nx, ny));
            dis[nx][ny] = dis[x][y] + 1;
        }
    }
    return -1;
}
int main()
{
    int m, n, t;
    std::cin >> m >> n;
    std::vector<std::vector<type>> map(m + 2, std::vector<type>(n + 2, type::trap));
    for (int i = 1; i <= m; ++i)
        for (int j = 1; j <= n; ++j)
        {
            std::cin >> t;
            map[i][j] = static_cast<type>(t);
        }
    int ans = bfs(map);
    if (ans == -1)
        std::cout << "NO" << std::endl;
    else
        std::cout << ans << std::endl;
    return 0;
}
```



代码运行截图 ==（至少包含有"Accepted"）==



![](C:\Users\sheng\Downloads\assignment9.assets\屏幕截图 2024-11-20 110927.png)

### 04123: 马走日

dfs, http://cs101.openjudge.cn/practice/04123

思路：

用DFS搜索，若已经无法移动则检查是否已经完成一次遍历，若是则找到一种方案，答案加一。

代码：

```c++
#include <array>
#include <iostream>
#include <vector>

int dfs(int x, int y, std::vector<std::vector<bool>> &visited, int step)
{
    static const std::array<int, 8> dx = {2, 2, -2, -2, 1, 1, -1, -1}, dy = {1, -1, 1, -1, 2, -2, 2, -2};
    bool flag = true;
    int cnt = 0;
    visited[x][y] = true;
    for (int i = 0; i < 8; ++i)
    {
        int nx = x + dx[i], ny = y + dy[i];
        if (nx >= 0 && nx < visited.size() && ny >= 0 && ny < visited[0].size() && !visited[nx][ny])
            flag = false, cnt += dfs(nx, ny, visited, step + 1);
    }
    visited[x][y] = false;
    if (flag)
        return step == visited.size() * visited[0].size() ? 1 : 0;
    return cnt;
}
int main()
{
    int t;
    std::cin >> t;
    while (t--)
    {
        int n, m, x, y;
        std::cin >> n >> m >> x >> y;
        std::vector<std::vector<bool>> visited(n, std::vector<bool>(m, false));
        std::cout << dfs(x, y, visited, 1) << std::endl;
    }
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>



![](C:\Users\sheng\Downloads\assignment9.assets\屏幕截图 2024-11-20 112103.png)

### sy316: 矩阵最大权值路径

dfs, https://sunnywhy.com/sfbj/8/1/316

思路：

用DFS搜索所有可能路径，储存权值和最大的路径。

代码：

```c++
#include <climits>
#include <iostream>
#include <utility>
#include <vector>

int max_sum = INT_MIN;
std::vector<std::pair<int, int>> max_route;
void dfs(int current_sum, int i, int j, std::vector<std::vector<int>> &matrix, std::vector<std::vector<bool>> &visited, std::vector<std::pair<int, int>> &route)
{
    current_sum += matrix[i][j];
    if (i == matrix.size() - 1 && j == matrix[0].size() - 1)
    {
        if (current_sum <= max_sum)
            return;
        max_sum = current_sum;
        route.push_back({i + 1, j + 1});
        max_route = route;
        route.pop_back();
        return;
    }
    visited[i][j] = true;
    route.push_back({i + 1, j + 1});
    if (i - 1 >= 0 && j >= 0 && i - 1 < matrix.size() && j < matrix[0].size() && !visited[i - 1][j])
        dfs(current_sum, i - 1, j, matrix, visited, route);
    if (i + 1 >= 0 && j >= 0 && i + 1 < matrix.size() && j < matrix[0].size() && !visited[i + 1][j])
        dfs(current_sum, i + 1, j, matrix, visited, route);
    if (i >= 0 && j - 1 >= 0 && i < matrix.size() && j - 1 < matrix[0].size() && !visited[i][j - 1])
        dfs(current_sum, i, j - 1, matrix, visited, route);
    if (i >= 0 && j + 1 >= 0 && i < matrix.size() && j + 1 < matrix[0].size() && !visited[i][j + 1])
        dfs(current_sum, i, j + 1, matrix, visited, route);
    visited[i][j] = false;
    route.pop_back();
    return;
}
int main()
{
    int n, m;
    std::cin >> n >> m;
    std::vector<std::vector<int>> matrix(n, std::vector<int>(m));
    std::vector<std::vector<bool>> visited(n, std::vector<bool>(m, false));
    for (int i = 0; i < n; ++i)
        for (int j = 0; j < m; ++j)
            std::cin >> matrix[i][j];
    std::vector<std::pair<int, int>> route;
    dfs(0, 0, 0, matrix, visited, route);
    for (auto &[x, y] : max_route)
        std::cout << x << ' ' << y << std::endl;
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>



![](C:\Users\sheng\Downloads\assignment9.assets\屏幕截图 2024-11-20 114407.png)



### LeetCode62.不同路径

dp, https://leetcode.cn/problems/unique-paths/

思路：

显然总步数为$m+n-2$，只要在其中选取$n-1$步向下即可，故可直接计算组合数。

代码：

```c++
class Solution {
public:
    int uniquePaths(int m, int n) {
        long long ans = 1;
        for (int i = 1; i < m; ++i)
            ans = ans * (n + i - 1) / i;
        return ans;
    }
};
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>



![](C:\Users\sheng\Downloads\assignment9.assets\屏幕截图 2024-11-20 120118.png)

### sy358: 受到祝福的平方

dfs, dp, https://sunnywhy.com/sfbj/8/3/539

思路：

从前向后搜索分割方案，判断其是否符合题意。

代码：

```python
#include <cmath>
#include <iostream>
#include <string>

bool check(std::string s)
{
    int t = std::stoi(s);
    int sq = std::sqrt(t);
    return sq * sq == t && t != 0;
}
bool blessed(const std::string &s, int pos)
{
    if (pos == s.size())
        return true;
    for (int i = pos; i < s.size(); ++i)
        if (check(s.substr(pos, i - pos + 1)) && blessed(s, i + 1))
            return true;
    return false;
}
int main()
{
    std::string a;
    std::cin >> a;
    std::cout << (blessed(a, 0) ? "Yes" : "No") << std::endl;
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>



![](C:\Users\sheng\Downloads\assignment9.assets\屏幕截图 2024-11-20 152049.png)

## 2. 学习总结和收获

大部分搜索的题目思维难度较低，但容易出现难以发现的错误，在写程序和检查时要更加仔细。比如在寻宝上卡了很久，最后发现原来的程序无法正确处理出发点有宝藏的情况。





