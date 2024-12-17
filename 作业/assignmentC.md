# Assignment #C: 五味杂陈 

Updated 1148 GMT+8 Dec 10, 2024

2024 fall, Complied by 周晟昱 工学院



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 1115. 取石子游戏

dfs, https://www.acwing.com/problem/content/description/1117/

思路：

按照提示递归判断即可。

代码：

```c++
#include <iostream>
#include <utility>
bool win(int a, int b)
{
    if (b == 0)
        return false;
    return (a >= 2 * b ? true : !win(b, a - b));
}
int main()
{
    int a, b;
    while (std::cin >> a >> b, a + b)
    {
        if (a < b)
            std::swap(a, b);
        std::cout << (win(a, b) ? "win" : "lose") << std::endl;
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>



![](C:\Users\sheng\Downloads\assignmentC.assets\屏幕截图 2024-12-10 204133.png)

### 25570: 洋葱

Matrices, http://cs101.openjudge.cn/practice/25570

思路：

直接计算每层数的和取最大值即可。

代码：

```c++
#include <algorithm>
#include <iostream>
#include <vector>

inline int calc_sum(int x1, int y1, int x2, int y2, std::vector<std::vector<int>> &onion)
{
    if (x1 == x2)
        return onion[x1][y1];
    int sum = 0;
    for (int i = x1; i < x2; ++i)
        sum += onion[i][y1];
    for (int i = y1; i < y2; ++i)
        sum += onion[x2][i];
    for (int i = x2; i > x1; --i)
        sum += onion[i][y2];
    for (int i = y2; i > y1; --i)
        sum += onion[x1][i];
    return sum;
}
int main()
{
    int n, ans = 0;
    std::cin >> n;
    std::vector<std::vector<int>> onion(n, std::vector<int>(n));
    for (int i = 0; i < n; ++i)
        for (int j = 0; j < n; ++j)
            std::cin >> onion[i][j];
    for (int k = 0; k <= n - k - 1; ++k)
        ans = std::max(ans, calc_sum(k, k, n - k - 1, n - k - 1, onion));
    std::cout << ans << std::endl;
    return 0;
}
```



代码运行截图 ==（至少包含有"Accepted"）==



![](C:\Users\sheng\Downloads\assignmentC.assets\屏幕截图 2024-12-10 204240.png)

### 1526C1. Potions(Easy Version)

greedy, dp, data structures, brute force, *1500, https://codeforces.com/problemset/problem/1526/C1

思路：

采用贪心策略，喝下所有的药，并将所有恢复生命值为负数的药水存入大根堆，若某时刻生命值为负数，则“反悔”，即不喝之前扣除生命最多的药水，如此操作直至无法反悔或到达最后。

代码：

```c++
#include <iostream>
#include <queue>

int main()
{
    long long n, a, hp = 0, ans = 0;
    std::cin >> n;
    std::priority_queue<long long> q;
    while (n--)
    {
        std::cin >> a;
        if (a < 0)
            q.push(-a);
        hp += a;
        ++ans;
        while (hp < 0)
        {
            hp += q.top();
            q.pop();
            --ans;
        }
    }
    std::cout << ans << std::endl;
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>



![](C:\Users\sheng\Downloads\assignmentC.assets\屏幕截图 2024-12-10 211401.png)

### 22067: 快速堆猪

辅助栈，http://cs101.openjudge.cn/practice/22067/

思路：

用栈和multiset维护当前状态，即可实现查询。

代码：

```c++
#include <iostream>
#include <set>
#include <stack>
#include <string>
int main()
{
    std::string input;
    std::stack<int> stk;
    std::multiset<int> st;
    while (std::getline(std::cin, input))
    {
        switch (input[1])
        {
            case 'u':
            {
                int num = std::stoi(input.substr(5));
                stk.push(num);
                st.insert(num);
                break;
            }
            case 'o':
            {
                if (!stk.empty())
                {
                    st.erase(st.find(stk.top()));
                    stk.pop();
                }
                break;
            }
            case 'i':
            {
                if (!stk.empty())
                    std::cout << (*st.begin()) << std::endl;
                break;
            }
        }
    }
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>



![](C:\Users\sheng\Downloads\assignmentC.assets\屏幕截图 2024-12-10 211454.png)

### 20106: 走山路

Dijkstra, http://cs101.openjudge.cn/practice/20106/

思路：

运用优先队列优化的Dijkstra算法计算最短路即可。

代码：

```c++
#include <climits>
#include <cstdlib>
#include <functional>
#include <iostream>
#include <queue>
#include <string>
#include <utility>
#include <vector>

inline int getnum(const int &x, const int &y, const int &n)
{
    return (x - 1) * n + y - 1;
}
inline std::pair<int, int> getpos(const int &num, const int &n)
{
    return std::make_pair(num / n + 1, num % n + 1);
}
int main()
{
    int m, n, p;
    std::cin >> m >> n >> p;
    std::vector<std::vector<int>> map(m + 2, std::vector<int>(n + 2, -1));
    std::string input;
    for (int i = 1; i <= m; ++i)
        for (int j = 1; j <= n; ++j)
        {
            std::cin >> input;
            if(input[0] != '#')
                map[i][j] = std::stoi(input);
        }
    while (p--)
    {
        int x1, y1, x2, y2;
        std::cin >> x1 >> y1 >> x2 >> y2;
        ++x1, ++y1, ++x2, ++y2;
        if (map[x1][y1] == -1 || map[x2][y2] == -1)
        {
            std::cout << "NO" << std::endl;
            continue;
        }
        std::vector<int> dis(m * n, INT_MAX);
        dis[getnum(x1, y1, n)] = 0;
        std::priority_queue<std::pair<int, int>, std::vector<std::pair<int, int>>, std::greater<std::pair<int, int>>> q;
        q.push(std::make_pair(0, getnum(x1, y1, n)));
        while (!q.empty())
        {
            auto [distant, num] = q.top();
            q.pop();
            if (distant > dis[num])
                continue;
            auto &&[x, y] = getpos(num, n);
            if (x == x2 && y == y2)
                break;
            if (map[x - 1][y] != -1)
                if (dis[getnum(x - 1, y, n)] > dis[num] + std::abs(map[x - 1][y] - map[x][y]))
                {
                    dis[getnum(x - 1, y, n)] = dis[num] + std::abs(map[x - 1][y] - map[x][y]);
                    q.push(std::make_pair(dis[getnum(x - 1, y, n)], getnum(x - 1, y, n)));
                }
            if (map[x + 1][y] != -1)
                if (dis[getnum(x + 1, y, n)] > dis[num] + std::abs(map[x + 1][y] - map[x][y]))
                {
                    dis[getnum(x + 1, y, n)] = dis[num] + std::abs(map[x + 1][y] - map[x][y]);
                    q.push(std::make_pair(dis[getnum(x + 1, y, n)], getnum(x + 1, y, n)));
                }
            if (map[x][y - 1] != -1)
                if (dis[getnum(x, y - 1, n)] > dis[num] + std::abs(map[x][y - 1] - map[x][y]))
                {
                    dis[getnum(x, y - 1, n)] = dis[num] + std::abs(map[x][y - 1] - map[x][y]);
                    q.push(std::make_pair(dis[getnum(x, y - 1, n)], getnum(x, y - 1, n)));
                }
            if (map[x][y + 1] != -1)
                if (dis[getnum(x, y + 1, n)] > dis[num] + std::abs(map[x][y + 1] - map[x][y]))
                {
                    dis[getnum(x, y + 1, n)] = dis[num] + std::abs(map[x][y + 1] - map[x][y]);
                    q.push(std::make_pair(dis[getnum(x, y + 1, n)], getnum(x, y + 1, n)));
                }
        }
        if (dis[getnum(x2, y2, n)] == INT_MAX)
            std::cout << "NO" << std::endl;
        else
            std::cout << dis[getnum(x2, y2, n)] << std::endl;
    }
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>



![](C:\Users\sheng\Downloads\assignmentC.assets\屏幕截图 2024-12-10 211617.png)

### 04129: 变换的迷宫

bfs, http://cs101.openjudge.cn/practice/04129/

思路：

BFS，但需要注意时间对周期的模也是状态的一部分。

代码：

```c++
#include <iostream>
#include <set>
#include <queue>
#include <utility>
#include <vector>
int main()
{
    int t;
    std::cin >> t;
    while (t--)
    {
        int r, c, k, x1, y1, x2, y2;
        bool unable_to_reach = true;
        std::cin >> r >> c >> k;
        std::vector<std::vector<bool>> is_rock(r, std::vector<bool>(c, false));
        char ch;
        for (int i = 0; i < r; ++i)
            for (int j = 0; j < c; ++j)
            {
                std::cin >> ch;
                is_rock[i][j] = ch == '#';
                if (ch == 'S')
                    x1 = i, y1 = j;
                if (ch == 'E')
                    x2 = i, y2 = j;
            }
        std::set<std::pair<int, std::pair<int, int>>> least_time;
        std::queue<std::pair<int, std::pair<int, int>>> q;
        q.push({0, {x1, y1}});
        least_time.insert({0, {x1, y1}});
        while (!q.empty())
        {
            auto [t, p] = q.front();
            q.pop();
            if (p.first == x2 && p.second == y2)
            {
                std::cout << t << std::endl;
                unable_to_reach = false;
                break;
            }
            ++t;
            if (p.first + 1 < r && (!is_rock[p.first + 1][p.second] || t % k == 0) && least_time.find({t % k, {p.first + 1, p.second}}) == least_time.end())
                q.push({t, {p.first + 1, p.second}}), least_time.insert({t % k, {p.first + 1, p.second}});
            if (p.first - 1 >= 0 && (!is_rock[p.first - 1][p.second] || t % k == 0) && least_time.find({t % k, {p.first - 1, p.second}}) == least_time.end())
                q.push({t, {p.first - 1, p.second}}), least_time.insert({t % k, {p.first - 1, p.second}});
            if (p.second + 1 < c && (!is_rock[p.first][p.second + 1] || t % k == 0) && least_time.find({t % k, {p.first, p.second + 1}}) == least_time.end())
                q.push({t, {p.first, p.second + 1}}), least_time.insert({t % k, {p.first, p.second + 1}});
            if (p.second - 1 >= 0 && (!is_rock[p.first][p.second - 1] || t % k == 0) && least_time.find({t % k, {p.first, p.second - 1}}) == least_time.end())
                q.push({t, {p.first, p.second - 1}}), least_time.insert({t % k, {p.first, p.second - 1}});
        }
        if (unable_to_reach)
            std::cout << "Oop!" << std::endl;
    }
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>



![](C:\Users\sheng\Downloads\assignmentC.assets\屏幕截图 2024-12-10 211930.png)

## 2. 学习总结和收获

搜索题实现起来需要极高的专注度，否则调试会花费大量的时间。贪心题……再接再厉吧，尽量能提升一下自己的思维灵活性。

最近要开始复习理论知识，准备迎接期末考试。





