<h4 id="QJIca">拓扑排序</h4>
<h5 id="eXKs6">什么是拓扑排序？</h5>
首先拓扑排序的定义如下

 拓扑排序是一种对有向无环图的顶点进行排序的方法。它的主要目的是产生一个顶点的线性序列，使得如果在图中存在一条从顶点 A 指向顶点 B 的边，则在排序结果中，顶点 A 出现在顶点 B 之前。

拓扑排序就是 遍历一个没有环的有向图中， 按照箭头的顺序(依赖关系)遍历节点。 

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486661868-a09ce913-92dc-4759-a5a4-38a33c9224f4.png)

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486662038-0ae1b4a3-1e3f-44ca-8879-739d12fdf553.png)

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486661879-c198e468-d9a9-4160-96f8-faecfc02a288.png)

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486662436-54edeeff-b02b-46e3-a18b-997232ad3f19.png)

在上面一个有向无环图中，必须先访问a，才可以访问b或c；必须先访问b和c，才可以访问d。

所以，图的拓扑排序为a，b，c，d 或者a，c，b，d

<h5 id="lQSvt">**如何实现拓扑排序？**</h5>
首先给出拓扑排序的算法，只有两步：

**第一步：**访问一个入度为0的节点

**第二步：**删除入度为0的节点以及从这个节点出去的所有边，继续执行1，直至图中没有节点。

以上面的图为例，a的入度为0，所以访问a，同时删除a和a的两个出边；此时，b和c节点的入度变为0。因为b和c的入度都为0，我们可以随便选择一个访问，假设访问的b，删除b节点和它 的出边。然后访问c节点，删除c节点和它 的出边，此时d节点入度为0，直接删除d节点，图中没有节点。算法执行完毕。所以一个拓扑排序是 a,b,c,d。

如何来实现呢？使用BFS进行拓扑排序。

**计算入度**：对于图中的每个顶点，计算它的入度（即有多少边指向该顶点）。	

**初始化队列**：创建一个队列，用于存储所有入度为 0 的顶点。这些顶点没有任何先决条件，可以立即处理。

**拓扑排序**：

    - 当队列不为空时，从队列中移除一个顶点，并将其添加到拓扑排序的结果中。
    - 遍历该顶点的所有邻接顶点，将这些邻接顶点的入度减 1（因为它们的一个依赖已经被移除了）。
    - 如果任何邻接顶点的入度变为 0，则将其添加到队列中。	

**检查是否存在拓扑排序**：如果排序结果中的顶点数量不等于图中的顶点总数，则说明图中存在环，因此不存在拓扑排序。

为什么可以使用BFS呢?

BFS 以层级的方式遍历，在处理当前层的顶点之前，所有的前置顶点（即入度已被减至 0 的顶点）已经被处理。这种层级性保证了拓扑排序的正确性。

伪代码：

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486662435-af5dbcf9-948b-400f-a1e7-f1054f2c8500.png)

<h4 id="nz1OY">拓扑排序的拓展</h4>
拓扑排序的一个典型应用是解决具有依赖关系的任务调度问题。例如，在编译器中对程序模块进行编译时，必须先编译依赖模块，这种依赖关系就可以通过拓扑排序来解决。在编译过程中，模块间的依赖关系可以被视为一个有向图，其中每个模块代表一个顶点，依赖关系表示为有向边。例如，如果模块 A 依赖于模块 B，则会有一条从 B 指向 A 的边。对于CSAPP的例子中，可以使用图来表示这个依赖关系：

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486662483-a8e87a1a-6526-4fce-805b-4443c108b5f2.png)

拓扑排序还有一个重要的用途是检测循环依赖。如果模块间的依赖关系形成了一个环，那么将无法进行有效的拓扑排序。这在编译过程中是一个关键的检查点，因为循环依赖通常是编程错误。

<h4 id="bHkH5">拓扑排序</h4>
<h5 id="DXAwd">[leetcode207. 课程表](https://leetcode.cn/problems/course-schedule/)</h5>
这个学期必须选修`numCourses`门课程，记为`0`到`numCourses-1`。在选修某些课程之前需要一些先修课程。 先修课程按数组`prerequisites`给出，其中 `prerequisites[i] = [a``<sub>i</sub>``, b``<sub>i</sub>``]` ，表示如果要学习课程`a``<sub>i</sub>`则**必须**先学习课程`b``<sub>i</sub>`。

例如，先修课程对 `[0, 1]` 表示：想要学习课程 `0` ，你需要先完成课程 `1` 。请判断是否可能完成所有课程的学习？如果可以，返回 `true` ；否则，返回 `false` 。

**输入：**numCourses = 2, prerequisites = [[1,0],[0,1]]

**输出：**false

**解释：**总共有 2 门课程。学习课程 1 之前，你需要先完成课程 0 ；并且学习课程 0 之前，你还应先完成课程 1 。这是不可能的。

```c
class Solution {
    public:
    boolcanFinish(int n, vector<vector<int>>& p){
        vector<int>d(n,0);
        vector<vector<int>>g(n);
        for(int i=0;i<p.size();i++){
            g[p[i][0]].push_back(p[i][1]);
            d[p[i][1]]++;
        }
        queue<int>q;
        int cnt=0;
        for(int i=0;i<n;i++)if(d[i]==0)q.push(i);
        while(!q.empty()){
            int t=q.front();
            q.pop();
            cnt++;
            for(auto ne:g[t]){
                d[ne]--;
                if(d[ne]==0)q.push(ne);
            }
        }
        return cnt==n;
    }
}；
```

<h5 id="yoA5P">[210. 课程表 II](https://leetcode.cn/problems/course-schedule-ii/)</h5>
**输出一个拓扑排序**

现在你总共有 `numCourses` 门课需要选，记为 `0` 到 `numCourses - 1`。给你一个数组 `prerequisites` ，其中 `prerequisites[i] = [a``<sub>i</sub>``, b``<sub>i</sub>``]` ，表示在选修课程 `a``<sub>i</sub>` 前 **必须** 先选修 `b``<sub>i</sub>`。如果想要学习课程`0`，需要先完成课程`1`，用一个匹配来表示:`[0,1]`。返回为了学完所有课程所安排的学习顺序。可能会有多个正确的顺序，只要返回 **任意一种** 就可以了。如果不可能完成所有课程，返回**一个空数组。**

**<u>输入：</u>**numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]] 

_**<u>输出</u>**_**：**[0,2,1,3]

_**<u>解释：</u>**_总共有 4 门课程。要学习课程 3，你应该先完成课程 1 和课程 2。并且课程 1 和课程 2 都应该排在课程 0 之后。因此一个正确的课程顺序是 [0,1,2,3]，另一个正确的排序是 [0,2,1,3]。

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486662592-f7e9e602-bbc8-4426-a062-881b3a84f7e0.png)

```c
class Solution {
    public:
    vector<int> findOrder(int n,vector<vector<int>>& p){
        vector<int>d(n,0);
        vector<vector<int>>g(n);
        for(int i=0;i<p.size();i++){
            g[p[i][1]].push_back(p[i][0]);
            d[p[i][0]]++;
        }
        queue<int>q;
        vector<int>res;
        for(int i=0;i<n;i++)if(d[i]==0)q.push(i);
        while(!q.empty()){
            int t = q.front(); q.pop();
            res.push_back(t);
            for(auto ne:g[t]){
                d[ne]--;
                if(d[ne]==0)q.push(ne);
            }
        }
        vector<int>ans;
        return res.size()==n?res:ans;
    }
};
```

<h5 id="f21RF">[802. 找到最终的安全状态](https://leetcode.cn/problems/find-eventual-safe-states/)</h5>
有一个有 `n` 个节点的有向图，节点按 `0` 到 `n - 1` 编号。图由一个 **索引从 0 开始** 的 2D 整数数组 `graph`表示， `graph[i]`是与节点 `i` 相邻的节点的整数数组，这意味着从节点 `i` 到 `graph[i]`中的每个节点都有一条边。

如果一个节点没有连出的有向边，则该节点是 **终端节点** 。如果从该节点开始的所有可能路径都通向 **终端节点** ，则该节点为 **安全节点** 。

返回一个由图中所有 **安全节点** 组成的数组作为答案。答案数组中的元素应当按 **升序** 排列。

<h5 id="eOxji">[2115. 从给定原材料中找到所有可以做出的菜](https://leetcode.cn/problems/find-all-possible-recipes-from-given-supplies/)</h5>
有`n`道不同菜的信息。给你一个字符串数组`recipes`和一个二维字符串数组`ingredients`。第`i`道菜的名字为 `recipes[i]` ，如果你有它 **所有** 的原材料`ingredients[i]`，那么你可以**做出**这道菜。一道菜的原材料可能是 **另一道** 菜，也就是说 `ingredients[i]` 可能包含 `recipes` 中另一个字符串。同时给你一个字符串数组 `supplies` ，它包含你初始时拥有的所有原材料，每一种原材料你都有无限多。请你返回你可以做出的所有菜。你可以以 **任意顺序** 返回它们。注意两道菜在它们的原材料中可能互相包含。

**输入：**recipes = ["bread"], ingredients = [["yeast","flour"]], supplies = ["yeast","flour","corn"]

**输出：**["bread"]

**解释：**可以做出 "bread" ，因为我们有原材料 "yeast" 和 "flour" 。

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486662691-89fee5f9-d327-4966-9ac3-abbf9303632f.png)

```c
class Solution {
    public:
    vector<string> findAllRecipes(vector<string>& recipes, vector<vector<string>>& ingredients, vector<string>& supplies) {
        unordered_map<string,vector<string>>table; //建图
        unordered_map<string,int> indegree; //入度
        int n=recipes.size();
        for(int i=0;i<n;i++){
            for(int j=0;j<ingredients[i].size();j++){
                table[ingredients[i][j]].push_back(recipes[i]);
                indegree[recipes[i]]++;
            }
        }
        queue<string> q;
        for(auto sup:supplies){
            q.push(sup);
        }
        vector<string> res;
        while(!q.empty()){
            string  tp=q.front();
            q.pop();
            for(auto &nxt:table[tp]){
                if(--indegree[nxt]==0){
                    q.push(nxt);  //说明nxt可达.
                    res.push_back(nxt);
                }
            }
        }
        return res;
    }
};
```

<h5 id="wgT4c">[310. 最小高度树](https://leetcode.cn/problems/minimum-height-trees/)</h5>
找树的直径的中点（圆心）

树是一个无向图，其中任何两个顶点只通过一条路径连接。 换句话说，任何一个没有简单环路的连通图都是一棵树。给你一棵包含 `n` 个节点的树，标记为 `0` 到 `n - 1` 。给定数字 `n` 和一个有 `n - 1` 条无向边的 `edges` 列表（每一个边都是一对标签），其中 `edges[i] = [a``<sub>i</sub>``, b``<sub>i</sub>``]` 表示树中节点 `a``<sub>i</sub>` 和 `b``<sub>i</sub>` 之间存在一条无向边。可选择树中任何一个节点作为根。当选择节点 `x` 作为根节点时，设结果树的高度为 `h` 。在所有可能的树中，具有最小高度的树（即，`min(h)`）被称为 **最小高度树** 。

请你找到所有的 **最小高度树** 并按 **任意顺序** 返回它们的根节点标签列表。树的**高度** 是指根节点和叶子节点之间最长向下路径上边的数量。

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486662964-c0cacf42-39ca-4b2f-8286-f387ee6e1b90.png)

根节点在直径中间一个点或者两个点。也可以用拓扑排序，每次弹出所有度为1的点(叶子节点)，弹到最后剩一个点或者剩两个点的时候返回结果。

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486663032-ce4f385e-e3c2-43b0-ace4-a906afb24f4a.png)

无向图的拓扑排序

```c
class Solution {
    public:
    vector<int> findMinHeightTrees(int n, vector<vector<int>>& edges) {
        if (n == 1) {
            return {0};
        }
        vector<vector<int>> g(n);
        vector<int> degree(n);
        for (auto& e : edges) {
            int a = e[0], b = e[1];
            g[a].push_back(b);
            g[b].push_back(a);
            ++degree[a];
            ++degree[b];
        }
        queue<int> q;
        for (int i = 0; i < n; ++i) {
            if (degree[i] == 1) q.push(i);
        }
        vector<int> ans;
        int cnt = n;
        int step =0;
        while (cnt>2){
            int size = q.size();
            step++;
            for (int i=0; i<size; i++) {
                int a = q.front();
                q.pop();
                cnt--;
                for (int b : g[a]) {
                    if (--degree[b] == 1) {
                        q.push(b);
                    }
                }
            }
        }
        if(q.size()==2)
            cout<<step*2+1;
        else
            cout<<step*2;
        while(!q.empty()){
            ans.push_back(q.front());
            q.pop();
        }
        return ans;
    }
};
```

用step可以求树的直径（走的步数，如果queue中最后剩两个点，那么step*2+1；如果只剩一个点，那么step*2)。

for（ int i=0；i<size；i++）实际上是用于确保在处理当前层级的所有叶子节点（即度为1的节点）时，它们能够同时被处理，而不是一个接一个地处理。

如果不加这一行代码，算法会出现以下问题：

逐个处理叶子节点：没有这个循环控制时，queue<int> q 中的每个元素将被逐一处理。在一个while循环中仅处理了一个当前层级的叶子节点后，就立即开始处理由该操作产生的新叶子节点，而不是首先处理完当前层级的所有叶子节点。这会导致错误的结果。

逻辑不准确：正确的逻辑应该是每轮迭代中都移除当前层的所有叶子节点，并更新相关联节点的状态，然后基于更新后的状态进行下一轮迭代。如果只是简单地逐一处理队列中的元素而不考虑它们是否属于同一层级的叶子节点，那么你实际上改变了原始图的结构顺序，从而可能导致最终结果不是最小高度树的根节点。

<h4 id="P4oM6">[1462. 课程表 IV](https://leetcode.cn/problems/course-schedule-iv/)</h4>
（bfs可达性）

你总共需要上 `numCourses` 门课，课程编号依次为 `0` 到 `numCourses-1` 。你会得到一个数组 `prerequisite` ，其中 `prerequisites[i] = [a``<sub>i</sub>``, b``<sub>i</sub>``]` 表示如果你想选 `b``<sub>i</sub>` 课程，你**必须**先选 `a``<sub>i</sub>`课程。有的课会有直接的先修课程,比如如果想上课程`1`,你必须先上课程 `0`,那么会以 `[0,1]` 数对的形式给出先修课程数对。

先决条件也可以是**间接**的。如果课程 `a` 是课程 `b` 的先决条件，课程 `b` 是课程 `c` 的先决条件，那么课程 `a` 就是课程 `c` 的先决条件。你也得到一个数组 `queries` ，其中 `queries[j] = [u``<sub>j</sub>``, v``<sub>j</sub>``]`。对于第 `j` 个查询，您应该回答课程 `u``<sub>j</sub>` 是否是课程 `v``<sub>j</sub>` 的先决条件。返回一个布尔数组`answer`，其中 `answer[j]` 是第 `j` 个查询的答案。

使用able[start][j]表示是否能从start走到j，bfs遍历start，在遍历的过程中更新able[start][j]。

每次遍历之前重置vis。

```c
class Solution {
    public:
    vector<vector<int>>g;
    vector<int>vis;
    void bfs(int start,vector<vector<bool>>&able){
        queue<int>q;
        q.push(start);
        while(!q.empty()){
            int t = q.front(); q.pop();
            vis[t]=true;
            for(auto e:g[t]){
                if(!vis[e]){
                    q.push(e);
                    able[start][e]=true;
                }
            }
        }
    }
    vector<bool> checkIfPrerequisite(int n, vector<vector<int>>& pre, vector<vector<int>>& q) {
        vector<vector<bool>>able(n,vector<bool>(n,false));
        for(int i=0;i<n;i++)
            able[i][i]=true;
        g.resize(n);
        vis.resize(n,false);
        for(auto v:pre){
            g[v[0]].push_back(v[1]);
        }
        for(int i = 0;i<n;i++){
            fill(vis.begin(),vis.end(),false);
            bfs(i,able);
        }
        vector<bool>res;
        for(auto qu:q){
            if(able[qu[0]][qu[1]])
                res.push_back(true);
            else 
                res.push_back(false);
        }
        return res;
    }
};
```

<h4 id="RLkCN">最短路模板</h4>
第一行正整数N,接下来N行字符串，’.’表示可以通过，’#’表示障碍，’S’表示起点（有且仅有一个）。’E’表示出口（有且仅有一个）表示从S到E最短路径的长度, 无法到达则输出 -1 

```c
#include<iostream>
#include<cstring>
#include<queue>
using namespace std;
int n;
int sx,sy;
int ex,ey;
char g[1001][1001];
bool vis[1001][1001];
int res=-1;
structnode{
    int x;
    int y;
    int step;
    node(int xx,int yy,int s){
        x=xx; y=yy; step=s;
    }
};
int dx[4]={0,1,0,-1};
int dy[4]={1,0,-1,0};
voidbfs(){
    queue<node>q;
    q.push(node(sx,sy,0));
    while(!q.empty()){
        node t=q.front();
        q.pop();
        int tx=t.x;
        int ty=t.y;
        int tstep=t.step;
        if(tx==ex&&ty==ey)res=tstep;
        vis[tx][ty]=true;
        for(int i=0;i<4;i++){
            int newx=(tx+dx[i]+n)%n;
            int newy=(ty+dy[i]+n)%n;
            if(!vis[newx][newy]&&g[newx][newy]!='#'){
                vis[newx][newy]=true;
                q.push(node(newx,newy,tstep+1));
            }

        }
    }
}
int main(){
    cin >> n;
    memset(vis,0,sizeof(vis));
    for(int i=0;i<n;i++){
        string s;
        cin>>s;
        for(int j=0;j<n;j++){
            g[i][j]=s[j];
            if(s[j]=='S'){
                sx=i;
                sy=j;
            }elseif(s[j]=='E'){
                ex=i;
                ey=j;
            }
        }
    }
    bfs();
    cout<<res;
    return0;
}
```

<h4 id="Rklb8">[**https://leetcode.cn/problems/bus-routes/**](https://leetcode.cn/problems/bus-routes/)</h4>
给你一个数组 `routes` ，表示一系列公交线路，其中每个 `routes[i]` 表示一条公交线路，第 `i` 辆公交车将会在上面循环行驶。例如,路线 `routes[0] = [1, 5, 7]` 表示第 `0` 辆公交车会一直按序列 `1 -> 5 -> 7 -> 1 -> 5 -> 7 -> 1 -> ...` 这样的车站路线行驶。现在从 `source` 车站出发(初始时不在公交车上)，要前往`target`车站。 期间仅可乘坐公交车。求出**最少乘坐的公交车数量**。如果不可能到达终点车站,返回`-1`。

**输入：**routes = [[1,2,7],[3,6,7]], source = 1, target = 6

**输出：**2

**解释：**最优策略是先乘坐第一辆公交车到达车站 7 , 然后换乘第二辆公交车到车站 6 。 

同一条线路上的距离为0。1到2的距离是0，1到7的距离是0，2到7的距离为0。

然后2到3的距离为1(通过7)，2到6的距离为1(通过7)。可以建图，遍历routes，记录距离为1的点。

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486663160-fa8ea005-bc47-40e2-962e-34e4aa416bfd.png)

```c
class Solution {
    public:
    int numBusesToDestination(vector<vector<int>>& routes, int source, int target) {
        //记录经过车站x的公交车编号
        unordered_map<int, vector<int>> stop_to_buses;
        for (int i = 0; i < routes.size(); i++) {
            for (int x : routes[i]) {
                stop_to_buses[x].push_back(i);
            }
        }
        //BFS
        unordered_map<int, int> dis;//
        dis[source]=0; queue<int> q;
        q.push(source);
        while (!q.empty()) {
            int x = q.front(); // 当前在车站 x
            q.pop();
            int dis_x = dis[x];
            for (int i : stop_to_buses[x]) {  // 遍历所有经过车站 x 的公交车 i
                for (int y : routes[i]) { // 遍历公交车 i 的路线
                    if (!dis.contains(y)) { // 没有访问过车站 y
                        dis[y] = dis_x + 1; // 从 x 站上车然后在 y 站下车
                        q.push(y);
                    }
                }
                routes[i].clear(); // 标记 routes[i] 遍历过（清空后路线就访问不到(不会第二次访问)）
            }
        }
        return dis.contains(target) ? dis[target] : -1;
    }
};
```

_**<u>有哪些公交车会经过车站 x？</u>**_

创建一个哈希表 stopToBuses，key 为车站编号，value 为经过该车站的公交车编号列表。遍历第 i 辆公交车的路线 routes[i]，对于车站 x=routes[i][j]，把公交车编号 i 加到 stopToBuses[x] 列表中。

_**<u>在BFS 中，如何保证每辆公交车的路线只遍历一次？</u>**_

可以创建一个 vis 数组。更简单的办法是，当公交车路线 routes[i] 遍历结束后，把 routes[i] 置为空。

_**<u>在 BFS 中，如何保证每个车站只入队一次？</u>**_

为了记录起点到每个站的最短路（最少乘坐的公交车数量），创建一个哈希表 dis，key 为车站编号，value 为起点到该车站的最短路。可以利用 dis 来知道车站 x 是否入队过：看 x 是否在 dis 中即可。

<h4 id="Y0Xx7">多源BFS问题 模板</h4>
题目描述：有多个起点，求所有点到不同起点的最短路径。

做法：把所有起点加入队列然后用bfs来做。

简单证明：

可以在所有起点前加一个虚拟起点，这个虚拟起点到所有起点的路径长度为0。

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486663432-635e2404-15c4-46a4-a514-637aeed6b236.png)

那么求所有点到起点的最短距离，就是求所有点到这个虚拟起点的最短距离。

由于虚拟起点到所有边到权值为0，所以可以把所有起点加到队列中去。

<h5 id="Jnnvr">[1162. As Far from Land as Possible](https://leetcode.cn/problems/as-far-from-land-as-possible/)</h5>
Given an n x n grid containing only values 0 and 1, where 0 represents water and 1 represents land, find a water cell such that its distance to the nearest land cell is maximized, and return the distance. If no land or water exists in the grid, return -1.

The distance used in this problem is the Manhattan distance: the distance between two cells (x0, y0) and (x1, y1) is |x0 - x1| + |y0 - y1|

题目描述：有多个起点，求所有点到不同起点的最短路径。

做法：把所有起点加入队列然后用bfs来做。

题意：求所有0中到所有1的最短距离的最大值。

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486663330-3c72f2f8-f2b5-4d19-86d3-bb3274347775.png)

```c
#define x first
#define y second
typedef pair<int,int> PII;
class Solution {
    public:
    int maxDistance(vector<vector<int>>& g){
        int n=g.size(),m=g[0].size(),INF=1e8;
        vector<vector<int>>dist(n,vector<int>(m,INF));
        queue<PII>q;
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                if(g[i][j]){
                    dist[i][j]=0;
                    q.push({i,j});
                }
            }
        }
        int dx[4]={-1,0,1,0};
        int dy[4]={0,1,0,-1};
        while(q.size()){
            auto t =q.front();
            q.pop();
            for(int i=0;i<4;i++){
                int x=t.x+dx[i],y=t.y+dy[i];
                if(x>=0&&y>=0&&x<n&&y<m&&dist[x][y]==INF){
                    dist[x][y]=dist[t.x][t.y]+1;
                    q.push({x,y});
                }
            }
        }
        int res=-1;
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                if(!g[i][j]){
                    res=max(res,dist[i][j]);
                }
            }
        }
        if(res==INF)res=-1;
        return res;
    } 
};
```

<h4 id="L0dB5">934. 最短的桥</h4>
[**https://leetcode.cn/problems/shortest-bridge/**](https://leetcode.cn/problems/shortest-bridge/)

给你一个大小为 `n x n` 的二元矩阵 `grid` ，其中 `1` 表示陆地，`0` 表示水域。

**岛** 是由四面相连的 `1` 形成的一个最大组，即不会与非组内的任何其他 `1` 相连。`grid` 中 **恰好存在两座岛**。你可以将任意数量的`0`变为`1`,以使两座岛连接起来，变成**一座岛** 。返回必须翻转的 `0` 的最小数目。

题意：给定01网格中含有两个包含1的连通分量，求连接这两个连通分量的最短路径是多少。思路：把其中一个连通分量全部加到队列中，然后用 多源BFS求解。用dfs把其中一个连通分量全部加到队列中，并把1变成0（网格类dfs）

```c
#include <vector>
#include <queue>
#include <cstring> // For memset
using namespace std;
class Solution {
    const int INF = 0x3f3f3f3f;
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
    vector<vector<int>> g, d;
    queue<pair<int, int>> q;
    //using pair instead of a custom Node class for simplicity
    public:
    int shortestBridge(vector<vector<int>>& grid) {
        g = grid;
        d.resize(g.size(), vector<int>(g[0].size(), 0x3f3f3f3f));
        for (int i = 0; i < g.size(); ++i) {
            for (int j = 0; j < g[0].size(); ++j) {
                if (g[i][j] == 1) {
                    dfs(i, j);
                    return bfs();
                }
            }
        }
        return -1;
    }
    private:
    void dfs(int x, int y) {
        g[x][y] = 2;
        d[x][y] = 0;
        q.push({x, y});
        for (int i = 0; i < 4; ++i) {
            int a = x + dx[i], b = y + dy[i];
            if (a < 0 || a >= g.size() || b < 0 || b >= g[0].size() || g[a][b] != 1) continue;
            dfs(a, b);
        }
    }
    int bfs() {
        while (!q.empty()) {
            auto t = q.front();
            q.pop();
            for (int i = 0; i < 4; ++i) {
                int x = t.first + dx[i], y = t.second + dy[i];
                if (x < 0 || x >= g.size() || y < 0 || y >= g[0].size() || d[x][y] <= d[t.first][t.second] + 1)
                    continue;
                d[x][y] = d[t.first][t.second] + 1;
                if (g[x][y] == 1) return d[x][y] - 1;
                q.push({x, y});
            }
        }
        return -1;
    }
};
```

<h4 id="tjepc">BFS输出最短路径</h4>
![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486663575-fe88b0e6-e4a9-4c62-8603-9bf1df07a740.png)

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486663592-1a2539ba-7929-45ae-96e2-f7cffe90ccb4.png)

核心思路：用pre数组存放当前选择的前一个选择（即cur变量），然后调用递归函数打印

例：输出迷宫最短路径坐标

0010

1000

n*m迷宫，0表示路，1表示墙，从左上角走到右下角，假设最短路径唯一，输出最短路径

用pre[n][m]保存当前位置的上一个位置，递归输出。

代码：

```c
#include<iostream>
#include<queue>
usingnamespace std;
struct node {
    int x, y;
    node(int _x, int _y) {
        x = _x;
        y = _y;
    }
    node() {
    };
};
int G[10][10];
bool vis[10][10];
node pre[10][10];
int n, m;
bool judge(int i, int j){
    if (vis[i][j] == true || G[i][j] == 1 || !(i >= 1 && i <= n && j >= 1 && j <= m))return false;
    return true;
}
void print(int x, int y){
    if (x == 1 && y == 1) {
        cout << "(" << x << "," << y << ")" << endl;
        return;
    }
    node temp = pre[x][y];
    print(temp.x, temp.y);
    cout << "(" << x << "," << y << ")" << endl;
    return;
}
void bfs(node temp){
    queue<node>q;
    q.push(temp);
    while (!q.empty()) {
        node cur = q.front();
        q.pop();
        int x = cur.x;
        int y = cur.y;
        vis[x][y] = true;
        if (x == n && y == m) {
            print(x, y);
            return;
        }

        if (judge(x + 1, y)) {
            pre[x + 1][y] = node(x, y);
            vis[x + 1][y] = true;
            q.push(node(x + 1, y));
        }
        if (judge(x - 1, y)) {
            pre[x - 1][y] = node(x, y);
            vis[x - 1][y] = true;
            q.push(node(x - 1, y));
        }
        if (judge(x, y - 1)) {
            pre[x][y - 1] = node(x, y);
            vis[x][y - 1] = true;
            q.push(node(x, y-1));
        }
        if (judge(x, y + 1)) {
            pre[x][y + 1] = node(x, y);
            vis[x][y + 1] = true;
            q.push(node(x, y + 1));
        }

    }
}
void input(){
    cin >> n >> m;
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            cin >> G[i][j];
        }
    }
}
int main(){
    input();
    node temp(1, 1);
    bfs(temp);
}
```

<h4 id="WItcs">抓住那头牛</h4>
农夫知道一头牛的位置，想要抓住它。农夫和牛都位于数轴上，农夫起始位于点 N，牛位于点 K。

农夫有两种移动方式：

1. 从 X 移动到 X−1 或 X+1，每次移动花费一分钟	
2. 从 X移动到 2∗X，每次移动花费一分钟

假设牛没有意识到农夫的行动，站在原地不动。农夫最少要花多少时间才能抓住牛？

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486663812-5b770a73-227c-49d6-9410-042d6cdb7587.png)

BFS最短路：每次可以走x-1,x+1,2*x

```c
#include <iostream>
#include <queue>
#include <cstring> // For memset

using namespace std;

int n, k;
int dist[100010]; // Assuming the maximum possible value is 100000

void bfs(int n) {
    dist[n] = 0;
    queue<int> q;
    q.push(n);
    while (!q.empty()) {
        int t = q.front();
        q.pop();
        if (t == k) break;
        if (t - 1 >= 0 && dist[t - 1] == -1) {
            q.push(t - 1);
            dist[t - 1] = dist[t] + 1;
        }
        if (t + 1 <= 100000 && dist[t + 1] == -1) {
            q.push(t + 1);
            dist[t + 1] = dist[t] + 1;
        }
        if (2 * t <= 100000 && dist[2 * t] == -1) {
            q.push(2 * t);
            dist[2 * t] = dist[t] + 1;
        }
    }
}

int main() {
    cin >> n >> k;
    memset(dist, -1, sizeof(dist)); // Initialize dist array with -1
    bfs(n);
    cout << dist[k];
    return 0;
}
```

<h4 id="g7OfL">LeetCode1091二进制矩阵中的最短路径 </h4>
宽度优先搜索求迷宫最短路模板：

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486664146-bede8816-dc38-4e11-86d6-01680297faca.png)

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486664256-5b8a950b-9d89-45ae-b978-36b98c827c0a.png)

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486664493-b3a4a8f6-6839-44d4-acec-9110276c4afa.png)

给你一个 n x n 的二进制矩阵 grid 中，返回矩阵中最短 畅通路径 的长度。如果不存在这样的路径，返回 -1。二进制矩阵中的 畅通路径 是一条从 左上角 单元格（即(0, 0)）到 右下角 单元格（即(n - 1, n - 1)的路径，该路径同时满足下述要求：路径途经的所有单元格都的值都是 0 。

路径中所有相邻的单元格应当在 8 个方向之一 上连通（即，相邻两单元之间彼此不同且共享一条边或者一个角）。畅通路径的长度 是该路径途经的单元格总数。

每次有8种选择，套用bfs模板

```c
#define x first
#define y second
typedef pair<int,int> PII;
class Solution {
    public:
    int shortestPathBinaryMatrix(vector<vector<int>>& g){
        if(g[0][0])return-1;
        int n=g.size();
        vector<vector<int>>dist(n,vector<int>(n,-1));
        dist[0][0]=1;
        queue<PII>q;
        q.push({0,0});
        int dx[]={-1,-1,-1,0,1,1,1,0};
        int dy[]={-1,0,1,1,1,0,-1,-1};
        while(q.size()){
            auto t=q.front();
            q.pop();
            for(int i=0;i<8;i++){
                int x=t.x+dx[i],y=t.y+dy[i];
                if(x>=0&&x<n&&y>=0&&y<n&&g[x][y]==0&&dist[x][y]==-1){
                    dist[x][y]=dist[t.x][t.y]+1;
                    q.push({x,y});
                }
            }
        }
        return dist[n-1][n-1];
    }
};
```

![](https://cdn.nlark.com/yuque/0/2025/gif/45533403/1745486664509-24fe674f-3ba7-43bd-8cd4-7620cedb03a2.gif)

<h4 id="N5Gpq">最优乘车 </h4>
BFS最短路

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486664593-8bb3341a-fe33-451f-bf05-01c79b0b0ee1.png)

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486664788-3932f056-8368-4294-8dbe-e62c4dd3e0df.png)

![](https://cdn.nlark.com/yuque/0/2025/gif/45533403/1745486664833-58730308-bf37-4d87-82fb-8c1101495599.gif)

编辑

思路:

同一条线路前面的站可以到后面的站，把一个巴士站点能到的所有的点都用权值为1的边连起来建图。

这样从1到N的边数-1就是换乘次数。

用BFS最短路来写。

```c
#include <iostream>
#include <queue>
#include <vector>
#include <string>
#include <cstring> // For memset
using namespace std;
const int N = 1000;
int g[N][N];
int dist[N];
int n, m;
void bfs() {
    memset(dist, 0x3f, sizeof(dist)); 
    // Initialize dist array with a large value (0x3f3f3f3f)
    queue<int> q;
    dist[1] = 0;
    q.push(1);
    while (!q.empty()) {
        int t = q.front();
        q.pop();
        for (int i = 1; i <= n; ++i) {
            if (g[t][i] == 1 && t != i && dist[i] > dist[t] + 1) {
                q.push(i);
                dist[i] = dist[t] + 1;
            }
        }
    }
}

int main() {
    cin >> m >> n;
    string line;
    getline(cin, line); // Consume the leftover newline character from the previous input

    for (int i = 0; i < m; ++i) {
        getline(cin, line);
        stringstream ss(line);
        vector<int> nodes;
        int num;
        while (ss >> num) {
            nodes.push_back(num);
        }
        for (int j = 0; j < nodes.size(); ++j) {
            for (int k = j + 1; k < nodes.size(); ++k) {
                g[nodes[j]][nodes[k]] = 1;
            }
        }
    }

    bfs();
    if (dist[n] > 0x3f3f3f3f / 2) cout << "NO" << endl;
    else cout << dist[n] - 1 << endl;

    return 0;
}
```

![](https://cdn.nlark.com/yuque/0/2025/gif/45533403/1745486665011-3430eae3-5985-4c9a-9f2f-dd871355c967.gif)

<h4 id="oOwLs">LeetCode1091二进制矩阵中的最短路径</h4>
给你一个 n x n 的二进制矩阵 grid 中，返回矩阵中最短 畅通路径 的长度。如果不存在这样的路径，返回 -1 。

二进制矩阵中的 畅通路径 是一条从 左上角 单元格（即(0, 0)）到 右下角 单元格（即(n - 1, n - 1)）的路径，该路径同时满足下述要求：

路径途经的所有单元格都的值都是 0。路径中所有相邻的单元格应当在 8 个方向之一 上连通（即，相邻两单元之间彼此不同且共享一条边或者一个角）。畅通路径的长度 是该路径途经的单元格总数。

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486665189-ad346c10-acbd-49e4-b904-2136f824b6b8.png)

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486665316-3f65c2fc-78b4-45ed-bc5d-92f0c8a55b60.png)

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486665323-ac9caefa-4290-4f4d-977f-4bebea954748.png)

```c
#define x first 
#define y second
typedef pair<int,int> PII;
class Solution {
    public:
    intshortestPathBinaryMatrix(vector<vector<int>>& g){
        if(g[0][0])return-1;
        int n=g.size();
        vector<vector<int>>dist(n,vector<int>(n,-1));
        dist[0][0]=1;
        queue<PII>q;
        q.push({0,0});
        int dx[]={-1,-1,-1,0,1,1,1,0};
        int dy[]={-1,0,1,1,1,0,-1,-1};
        while(q.size()){
            auto t=q.front();
            q.pop();
            for(int i=0;i<8;i++){
                int x=t.x+dx[i],y=t.y+dy[i];
                if(x>=0&&x<n&&y>=0&&y<n&&g[x][y]==0&&dist[x][y]==-1){
                    dist[x][y]=dist[t.x][t.y]+1;
                    q.push({x,y});
                }
            }
        }
        return dist[n-1][n-1];
    }
};
```

<h4 id="z2Rzn">**数组模拟队列bfs**</h4>
```c
struc tnode{
    state;
    node(_state){
        state = _state;
    }
}queue[MAX_SIZE];
int head=0;
int tail=0;
//入队
node t=node(state);
queue[tail++]=t;
//出队
node t=queue[head++];
//判不空while(head<tail){}
```

bfs求最小操作数

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486665409-4675735f-1fe5-4372-9c9f-3b6f5ec1d7b0.png)

<h4 id="XGPgu">**野人与传教士渡河：**</h4>
N名传教士和N个野蛮人同在一个小河渡口，渡口上只有一条可容M人的小船。

问题的目标是要用这条小船把这2*N个人全部渡到对岸去，条件是在渡河的过程中，河两岸随时都保持传教士人数不少于野蛮人的人数。否则野蛮人会把处于少数的传教士吃掉。如果顺利渡河，返回最小渡河次数，否则返回-1。

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486665489-436c508d-d7c6-48d1-b11a-deffdc963ee8.png)

(1)河两岸都需要保持这个条件。

(2)渡船的时候也需要保持这个条件。

(3)假设从右岸为终点）可以从右岸载人渡到左岸，也算一次。

为了防止重复渡河，需要有个状态判断是否重复。

未完待续。

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486665744-f6f6e5fe-4b20-493d-8e7a-86aa46d8217a.png)

<h4 id="tzTVs">TANK: </h4>
N*M的矩阵，有坦克A和B，终点C和D 坦克A需要走到C,坦克B需要走到D。 每经过一秒，坦克可以选择周围的8个方向任意移动一步，也可以选择原地不动。 但是两个坦克不能相邻(周围8个方向)，也不能去有墙的地方 问，最少需要多少秒，才能使两个坦克都达到目的地。

状态:

x1,y1,x2,y2。O(N*N)。

优化：首先判断起点和终点是否在一个集合中(dfs)（O(N*N)），如果不在的话，就直接返回。

```c
#include<iostream>#include<cstring>usingnamespace std;
bool visit[9][9][9][9];
int sx1,sy1,sx2,sy2,ex1,ey1,ex2,ey2;
int res;
int g[9][9];
structnode{
    int x1;int y1;int x2;int y2;
    int step;
}queue[100000];
int dx[9] = {0,-1,-1,-1,0,1,1,1,0};
int dy[9] = {-1,-1,0,1,1,1,0,-1,0};
int head=0;
int tail=0;
intbfs(){
    int x1,x2,y1,y2;
    queue[tail].x1=sx1;
    queue[tail].y1=sy1;
    queue[tail].x2=sx2;
    queue[tail].y2=sy2;
    queue[tail++].step=0;
    visit[sx1][sy1][sx2][sy2]=1;
    while(head<tail)
    {
        node t = queue[head++];
        if(t.x1==ex1&&t.y1==ey1&&t.x2==ex2&&t.y2==ey2){
            res=t.step;
            break;
        }
        for(int i=0;i<9;i++){
            int t1;
            x1=t.x1+dx[i];
            y1=t.y1+dy[i];
            //遇到障碍,跳过if(g[x1][y1]==-1) continue;
            t1 = g[x1][y1];
            g[x1][y1]=8;
            for(int j=0;j<9;j++){
                int t2;
                x2 =t.x2+dx[j];
                y2 =t.y2+dy[j];
                if(g[x2][y2]==-1)continue;
                //如果访问过，就跳过if(visit[x1][y1][x2][y2]==1) continue;
                //如果两个坦克不相邻(8个方向)if((x1-x2)*(x1-x2)+(y1-y2)*(y1-y2)>2){
                t2=g[x2][y2];
                g[x2][y2]=9;
                visit[x1][y1][x2][y2]=1;
                queue[tail].x1=x1;
                queue[tail].y1=y1;
                queue[tail].x2=x2;
                queue[tail].y2=y2;
                queue[tail++].step=t.step+1;
                g[x2][y2]=t2;
            }
        }
        g[x1][y1]=t1;
    }

}
return res;
}
```

<h4 id="OKBmq">LeetCode 994 腐烂的橘子</h4>
![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486666071-5efe375e-cac0-4202-aadc-e0806e3dfc76.png)

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486665988-ab49f425-67a6-4552-b271-54c8af54cd2f.png)

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486666101-c609b5e8-fe73-4bf0-ae32-91b63b8f332e.png)

```c
class Solution {
    public:
    struct node{
        int x;
        int y;
        int min;
        node(){};
        node(int x,int y,int min):x(x),y(y),min(min){};
    }queue[100000];
    int head=0;
    int tail=0;
    bool vis[11][11];
    int bfs(vector<vector<int>>&grid){
        int res=0;
        int n=grid.size();
        int m=grid[0].size();
        int cnt=0;
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                if(grid[i][j]==2){
                    node t=node(i,j,0);
                    queue[tail++]=t;
                }
                if(grid[i][j]==1)cnt++;
            }
        }
        int dx[4]={0,1,0,-1};
        int dy[4]={1,0,-1,0};
        while(head<tail){
            int size=tail-head;
            for(int k=0;k<size;k++){
                node t=queue[head++];
                int x=t.x;
                int y=t.y;
                vis[x][y]=true;
                int minute=t.min;
                res=max(res,minute);
                for(int i=0;i<4;i++){
                    int nx=x+dx[i];
                    int ny=y+dy[i];
                    if(!(nx>=0&&nx<n&&ny>=0&&ny<m))continue;
                    if(grid[nx][ny]!=1)continue;
                    if(vis[nx][ny])continue;
                    vis[nx][ny]=true;
                    grid[nx][ny]=2;
                    cnt--;
                    node nt=node(nx,ny,minute+1);
                    queue[tail++]=nt;
                }
            }
        }
        if(cnt!=0)return-1;
        return res; 
    }
    int orangesRotting(vector<vector<int>>& grid){
        memset(vis,0,sizeof(vis));
        returnbfs(grid);
    }
};
```

<h4 id="xtw3K">LeetCode1926 迷宫中的离入口最近的出口</h4>
![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486666313-47905f02-9851-4dfa-b2c9-5f1bfca16731.png)

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486666519-bb7d2787-39a1-4e33-ada7-38534e1b78c6.png)

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486666829-fbffd1d9-5667-4465-965e-aaf4276f2ac2.png)

```c
class Solution {
    private:
    struct node{
        int x;
        int y;
        int step;
        node(){};
        node(int _x,int _y,int _step):x(_x),y(_y),step(_step){};
    }queue[100001];
    bool vis[101][101];
    intbfs(vector<vector<char>>&g,vector<int>&st){
        int n=g.size();
        int m=g[0].size();
        int head=0;
        int tail=0;
        //初始化队列 for(int i=0;i<n;i++){
        if(g[i][0]=='.'&&(!(st[0]==i&&st[1]==0))){
            node t=node(i,0,0);
            queue[tail++]=t;
            cout<<i<<" "<<0<<endl;

        }
        if(g[i][m-1]=='.'&&!(i==st[0]&&m-1==st[1])){
            node t=node(i,m-1,0);
            queue[tail++]=t;
        }
    }
    for(int i=0;i<m;i++){
        if(g[0][i]=='.'&&!(0==st[0]&&i==st[1])){
            node t=node(0,i,0);
            queue[tail++]=t;
        }
        if(g[n-1][i]=='.'&&!(n-1==st[0]&&i==st[1])){
            node t=node(n-1,i,0);
            queue[tail++]=t;
        }
    }
    int dx[4]={1,0,-1,0};
    int dy[4]={0,-1,0,1};
    while(head<tail){
        int size=tail-head;
        for(int i=0;i<size;i++){
            node t = queue[head++];
            vis[t.x][t.y]=true;
            if(t.x==st[0]&&t.y==st[1]){
                return t.step;
            }
            for(int k=0;k<4;k++){
                int nx=t.x+dx[k];
                int ny=t.y+dy[k];
                if(!(nx>=0&&nx<n&&ny>=0&&ny<m))continue;
                if(vis[nx][ny])continue;
                if(g[nx][ny]=='+')continue;
                vis[nx][ny]=true;
                node nt=node(nx,ny,t.step+1);
                queue[tail++]=nt;
            }
        }
    }
    return -1;
}
public:
int nearestExit(vector<vector<char>>& maze, vector<int>& entrance){
    memset(vis,0,sizeof(vis));
    returnbfs(maze,entrance);
}
};
```

<h4 id="ZZJ5s">LeetCode 934最短的桥</h4>
![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486666722-e672b9d8-b888-47a9-9de9-8e9d2acb9033.png)

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486666805-79b48379-e5a3-457a-9071-22588da8eae4.png)

```c
bool vis[101][101];
int n,m;
vector<vector<int>>g;
int bfs(){
    queue<vector<int>>q;
    for(int i=0;i<n;i++)
        for(int j=0;j<m;j++)
            if(g[i][j]==2)
                vector<int>t={i,j,0};
    q.push(t);

    int dx[4]={1,0,-1,0};
    int dy[4]={0,1,0,-1};
    while(!q.empty()){
        int size=q.size();
        for(int i=0;i<size;i++){
            vector<int>t=q.front();
            q.pop();
            vis[t[0]][t[1]]=true;
            if(g[t[0]][t[1]]==1)return t[2];
            for(int k=0;k<4;k++){
                int nx=t[0]+dx[k];
                int ny=t[1]+dy[k];
                if(!(nx>=0&&nx<n&&ny>=0&&ny<m))continue;
                if(vis[nx][ny])continue;
                vector<int>nt={nx,ny,t[2]+1};
                q.push(nt);
            }
        }
    }
    return-1;
}
void dfs(int x,int y,int flag){
    if(!(x>=0&&x<n&&y>=0&&y<m))return;
    if(g[x][y]!=1)return;
    if(flag==1)g[x][y]=2;
    dfs(x+1,y,flag);
    dfs(x-1,y,flag);
    dfs(x,y-1,flag);
    dfs(x,y+1,flag);
}
int shortestBridge(vector<vector<int>>& grid){
    g=grid;
    n=g.size();
    m=g[0].size();
    int cnt=0;
    for(int i=0;i<n;i++){
        for(int j=0;j<m;j++){
            if(grid[i][j]==2||g[i][j]==0)continue;
            if(cnt==0){
                dfs(i,j,1);
                cnt++;
            }
        }
    }
    return bfs()-1;
}
```

<h4 id="UPJqy">LeetCode 1293网格中的最短路径</h4>
bfs加一维。

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486666899-3b4a8192-2405-45ab-937c-d35f94d494a0.png)

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486667079-4705f8c8-4690-47ee-a5ff-3356b2986e42.png)

因为一个点可能被周围4个点更新，如果使用vis数组，只更新了一次就停止更新，这样会错失其他3个点的更新。

如果是普通的bfs求最短路，可以使用vis数组，因为如果一个点被访问到了，那么它的距离就是最短的，之后可以不用被更新。但是多了一维之后，第一次访问到的不一定是最短的，所以需要一直更新。

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486667286-ac8ae64d-831f-4b2e-addc-7648dfcac925.png)

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486667458-35912a2b-86eb-4a22-b877-119959709784.png)

```c
class Solution {
    struct node{
        int x;
        int y;
        int r;
        node(){};
        node(int _x,int _y,int _r):x(_x),y(_y),r(_r){};
    }queue[100000];
    public:
    int shortestPath(vector<vector<int>>& g, int k){
        int n=g.size();int m=g[0].size();
        int head=0; int tail=0;
        int dis[41][41][41*41];
        memset(dis,0x3f3f3f3f,sizeof(dis));
        node t=node(0,0,0);
        queue[tail++]=t;
        dis[0][0][0]=0;
        int dx[4]={1,0,-1,0};
        int dy[4]={0,-1,0,1};
        while(head<tail){
            node t=queue[head++];
            int distance = dis[t.x][t.y][t.r];
            if(t.x==n-1&&t.y==m-1)return distance;
            for(int i=0;i<4;i++){
                int nx=t.x+dx[i];
                int ny=t.y+dy[i];
                if(!(nx>=0&&nx<n&&ny>=0&&ny<m))continue;
                int r=t.r+g[nx][ny];
                if(r>k)continue;
                if(distance+1>=dis[nx][ny][r])continue;
                dis[nx][ny][r]=distance+1;
                node nt = node(nx,ny,r);
                queue[tail++] = nt;
            }
        }
        return -1;    
    }
};
```

<h4 id="tg1X4">LeetCode 1210 穿过迷宫的最少次数</h4>
![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486667585-361c5081-df19-4bf7-90ca-1096763c5f39.png)

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486667561-a11c5bcf-d47a-4abd-a286-c2a606bb26ec.png)

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486667610-24a12873-7673-41d0-bc35-ec3af086ff3f.png)

```c
class Solution {
    structnode{
        int x;
        int y;
        int d;
        node(){};
        node(int _x,int _y,int _d):x(_x),y(_y),d(_d){};
    }queue[100001];
    public:
    int dis[101][101][2];
    intminimumMoves(vector<vector<int>>& g){
        memset(dis,0x3f3f3f3f,sizeof(dis));
        int n=g.size();
        int head=0;
        int tail=0;
        node t=node(0,1,0);
        dis[0][1][0]=0;
        dis[0][0][0]=0;
        queue[tail++]=t;
        while(head<tail){
            node t=queue[head++];
            int distance=dis[t.x][t.y][t.d];
            if(t.x==n-1&&t.y==n-1&&t.d==0)return distance;
            int x=t.x;
            int y=t.y;
            int d=t.d;
            int nx,ny,nd;
            if(y+1<n&&g[x][y+1]==0&&d==0){
                nx=x;ny=y+1;nd=0;
                if(dis[nx][ny][nd]>distance+1){
                    dis[nx][ny][nd]=distance+1;
                    node nt=node(nx,ny,nd);
                    queue[tail++]=nt;
                }
            }
            if(y+1<n&&x-1>=0&&g[x][y+1]==0&&g[x-1][y+1]==0&&d==1){
                nx=x;ny=y+1;nd=1;
                if(dis[nx][ny][nd]>distance+1){
                    dis[nx][ny][nd]=distance+1;
                    node nt=node(nx,ny,nd);
                    queue[tail++]=nt;
                }
            }
            if(d==0&&x+1<n&&y-1>=0&&g[x+1][y]==0&&g[x+1][y-1]==0){
                nx=x+1;ny=y;nd=0;
                if(dis[nx][ny][nd]>distance+1){
                    dis[nx][ny][nd]=distance+1;
                    node nt=node(nx,ny,nd);
                    queue[tail++]=nt;
                }
            }
            if(x+1<n&&g[x+1][y]==0&&d==1){
                nx=x+1;ny=y;nd=1;
                if(dis[nx][ny][nd]>distance+1){
                    dis[nx][ny][nd]=distance+1;
                    node nt=node(nx,ny,nd);
                    queue[tail++]=nt;
                }
            }
            if(x+1<n&&y-1>=0&&g[x+1][y]==0&&g[x+1][y-1]==0&&d==0){
                nx=x+1;ny=y-1;nd=1;
                if(dis[nx][ny][nd]>distance+1){
                    dis[nx][ny][nd]=distance+1;
                    node nt=node(nx,ny,nd);
                    queue[tail++]=nt;
                }
            }
            if(x-1>=0&&y+1<n&&g[x][y+1]==0&&g[x-1][y+1]==0&&d==1){
                nx=x-1;ny=y+1;nd=0;
                if(dis[nx][ny][nd]>distance+1){
                    dis[nx][ny][nd]=distance+1;
                    node nt=node(nx,ny,nd);
                    queue[tail++]=nt;
                }
            }
        }
        return-1;
    }
};
```

<h4 id="hakxi">[3243. 新增道路查询后的最短距离 I](https://leetcode.cn/problems/shortest-distance-after-road-addition-queries-i/)</h4>
给你一个整数 `n` 和一个二维整数数组`queries`.有 `n` 个城市，编号从 `0` 到 `n - 1`。初始时，每个城市 `i` 都有一条**单向**道路通往城市`i + 1(0 <= i < n - 1)`。`queries[i] = [u``<sub>i</sub>``, v``<sub>i</sub>``]`表示新建一条从城市 `u``<sub>i</sub>` 到城市 `v``<sub>i</sub>` 的**单向**道路。每次查询后，你需要找到从城市 `0` 到城市 `n - 1` 的**最短路径**的**长度。**返回一个数组 `answer`，对于范围 `[0, queries.length - 1]` 中的每个 `i`，`answer[i]` 是处理完**前**`i + 1` 个查询后，从城市 `0` 到城市 `n - 1` 的最短路径的_长度。_

思路：建立单向邻接表。然后每次用bfs求最短路。

```c
class Solution {
    public:
    // 定义全局变量
    vector<vector<int>> g;    // 图的邻接表表示
    vector<bool> vis;          // 访问标记数组
    vector<vector<int>> qs;   // 查询列表

    // BFS函数，计算从0到n-1节点的最短距离
    int bfs() {
        queue<int> q;
        q.push(0);
        int step = 0;
        while (!q.empty()) {
            step++;
            int size = q.size();
            for (int i = 0; i<size ; i++) { // 遍历当前层的所有节点
                int t = q.front();
                q.pop();
                for (int e : g[t]) {
                    if(e == g.size()-1) return step;
                    if (!vis[e]){
                        vis[e] = true;
                        q.push(e);
                    }
                }
            }
        }
        return -1; // 如果无法到达
    }
    // g
    // 主函数，初始化全局变量，并对每个查询调用bfs函数
    vector<int> shortestDistanceAfterQueries(int n, vector<vector<int>>& queries) {
        // 初始化全局变量
        g.assign(n, vector<int>());
        for (int i = 0; i < n - 1; ++i) {
            g[i].push_back(i + 1);
        }
        vis.assign(n, -1);
        qs = queries;
        vector<int> ans(queries.size());
        for (int i = 0; i < queries.size(); ++i) {
            auto& q  = queries[i];
            g[q[0]].push_back(q[1]);
            fill(vis.begin(),vis.end(),false);
            ans[i] = bfs();
        }
        return ans;
    }
};
```

<h4 id="Tvv83">[1311. 获取你好友已观看的视频](https://leetcode.cn/problems/get-watched-videos-by-your-friends/)</h4>
有 `n` 个人，每个人都有一个 `0` 到 `n-1` 的唯一 id 。

给你数组 `watchedVideos` 和 `friends` ，其中 `watchedVideos[i]` 和 `friends[i]` 分别表示 `id = i` 的人观看过的视频列表和他的好友列表。

Level 1 的视频包含所有你好友观看过的视频，level 2 的视频包含所有你好友的好友观看过的视频，以此类推。一般的，Level 为 k 的视频包含所有从你出发，最短距离为 k 的好友观看过的视频。

给定你的 `id` 和一个 `level` 值，请你找出所有指定 `level` 的视频，并将它们按观看频率升序返回。如果有频率相同的视频，请将它们按字母顺序从小到大排列。

```c
class Solution{
    public:
    vector<string> watchedVideosByFriends(
        vector<vector<string>>& watchedVideos, 
        vector<vector<int>>& friends,
        int id,
        int level){
        int n = friends.size();
        vector<bool> visited(n, false); // 访问标记数组
        queue<int> q;
        q.push(id);
        visited[id] = true;
        int currentLevel = 0;
        // BFS寻找指定level的好友
        while (!q.empty() && currentLevel < level) {
            int size = q.size();
            for (int i = 0; i < size; ++i) {
                int currFriend = q.front(); q.pop();
                for (int friendId : friends[currFriend]) {
                    if (!visited[friendId]) {
                        visited[friendId] = true;
                        q.push(friendId);
                    }
                }
            }
            if(currentLevel == level - 1) break; // 达到目标层级，停止BFS
            currentLevel++;
        }
        // 统计对应level好友观看过的视频
        unordered_map<string, int> videoCount;
        while (!q.empty()) {
            int friendId = q.front(); q.pop();
            for (const string& video : watchedVideos[friendId]) {
                videoCount[video]++;
            }
        }
        // 将结果转换为vector并排序(unordered_map无序的)
        vector<pair<int, string>> countAndVideo;
        for (auto& [video, count] : videoCount) {
            countAndVideo.emplace_back(count, video);
        }
        sort(countAndVideo.begin(), countAndVideo.end(), [](const pair<int, string>& a, const pair<int, string>& b) {
            return a.first == b.first ? a.second < b.second : a.first < b.first;
        });
        vector<string> result;
        for (auto& [_, video] : countAndVideo) {
            result.push_back(video);
        }
        return result;
    }
};
```

<h4 id="dUMEm">[1129. 颜色交替的最短路径](https://leetcode.cn/problems/shortest-path-with-alternating-colors/)</h4>
（使用dist数组表示最短路径）

给定一个整数`n`，即有向图中的节点数，其中节点标记为`0`到`n - 1`。图中的每条边为红色或者蓝色，并且可能存在自环或平行边。给定两个数`redEdges`和`blueEdges`。

`redEdges[i] = [a``<sub>i，</sub>``b``<sub>i</sub>``]` 表示图中存在一条从节点 `a``<sub>i</sub>` 到节点 `b``<sub>i</sub>` 的红色有向边。`blueEdges[j] = [u``<sub>j</sub>``, v``<sub>j</sub>``]` 表示图中存在一条从节点 `u``<sub>j</sub>` 到节点 `v``<sub>j</sub>` 的蓝色有向边。

返回长度为 `n` 的数组 `answer`，其中 `answer[X]` 是从节点 `0` 到节点 `X` 的红色边和蓝色边交替出现的最短路径的长度。如果不存在这样的路径，那么 `answer[x] = -1`。

**输入：**n = 3，red_edges=[[0,1],[1,2]], blue_edges=[]

**输出：**[0,1,-1]

一个点有两个状态（0表示红边，1表示蓝边）邻接表需要多一维来记录状态。起始点可以是红色也可以蓝色，所以dist[0][0]=0,dist[0][1]=0。

（这样的图转换后是等价的）

并且遍历邻接点的时候，需要判断和当前节点颜色不同。

```c
#define x first
#define y second
class Solution {
    public:
    vector<int> shortestAlternatingPaths(int n, vector<vector<int>>& redEdges, vector<vector<int>>& blueEdges) {
        const int INF = 1e8;
        vector<vector<pair<int, int>>> g(n);
        for (auto& p: redEdges) g[p[0]].push_back({p[1], 0});
        for (auto& p: blueEdges) g[p[0]].push_back({p[1], 1});
        vector<vector<int>> dist(n, vector<int>(2, INF));
        dist[0][0] = dist[0][1] = 0;
        queue<pair<int, int>> q;
        q.push({0, 0}), q.push({0, 1});
        while (q.size()) {
            auto t = q.front();
            q.pop();
            for (auto& p: g[t.x]) {
                if (t.y != p.y && dist[p.x][p.y] > dist[t.x][t.y] + 1) {
                    dist[p.x][p.y] = dist[t.x][t.y] + 1;
                    q.push(p);
                }
            }
        }
        vector<int> res;
        for (int i = 0; i < n; i ++ ) {
            res.push_back(min(dist[i][0], dist[i][1]));
            if (res[i] == INF) res[i] = -1;
        }
        return res;
    }
};
```

dist(n,vector<int>(2, INF)):创建一个大小为n的向量，n表示图中的节点数量。

dist将包含n个元素，每个元素对应图中的一个节点。对于dist中的每个元素，都初始化为一个大小为2的向量，并且这两个值都被初始化为INF。INF通常是一个非常大的整数值，用来表示“无穷大”，或者说两个节点之间目前还没有找到路径。第一个值表示从某个节点出发通过红边可达的最短路径长度，第二个值则表示通过蓝边可达的最短路径长度。

<h4 id="Y8ZRO">[2039. 网络空闲的时刻](https://leetcode.cn/problems/the-time-when-the-network-becomes-idle/)</h4>
（以后有机会补充）

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486667817-6b5582a9-179b-4fe1-9986-207381c52b3b.png)

<h4 id="w9xCj">[2608. 图中的最短环](https://leetcode.cn/problems/shortest-cycle-in-a-graph/)</h4>
<h4 id="AGYNq">[**https://leetcode.cn/problems/shortest-cycle-in-a-graph/**](https://leetcode.cn/problems/shortest-cycle-in-a-graph/)</h4>
现有一个含`n`个顶点的**双向**图，每个顶点按从`0`到`n - 1`标记。图中的边由二维整数数组`edges`表示，其中`edges[i] = [u``<sub>i</sub>``, v``<sub>i</sub>``]`表示顶点`u``<sub>i</sub>`和`v``<sub>i</sub>`之间存在一条边。每对顶点最多通过一条边连接，并且不存在与自身相连的顶点。返回图中 **最短** 环的长度。如果不存在环，则返回`-1`**。环**是指以同一节点开始和结束，并且路径中的每条边仅使用一次。

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486667991-22b53b3f-8d6b-4c40-8387-428e911ab21a.png)

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486668145-c1729405-df55-4f10-b4c1-01366a3205c5.png)

为什么说发现一个已经入队的点，就说明有环？

**答:**说明到同一个点有两条不同的路径，这两条路径组成了一个环。

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486668205-35b9376d-7f30-4883-844d-d734db263223.png)

```c
class Solution {
    private:
    vector<vector<int>> g; // 图的邻接表表示
    //BFS函数，用于从start点开始寻找最短环
    int bfs(int start) {
        int dis[g.size()]; // dis[i] 表示从 start 到 i 的最短路长度
        memset(dis, -1, sizeof(dis));
        dis[start] = 0;
        queue<pair<int, int>> q; // 存储当前节点和它的父节点
        q.emplace(start, -1);
        int ans = INT_MAX;
        while (!q.empty()) {
            auto [x, fa] = q.front();
            q.pop();
            for (int y: g[x]) {
                if (dis[y]<0){ // 第一次遇到
                    dis[y]=dis[x]+1;
                    q.emplace(y, x);
                } else if (y != fa) { // 第二次遇到
                    ans = min(ans, dis[x] + dis[y] + 1);
                }
            }
        }
        return ans;
    }
    public:
    //初始化图并计算最短环
    int findShortestCycle(int n, vector<vector<int>>& edges) {
        g.resize(n); //调整图的大小
        for (auto &e: edges) {
            int x = e[0], y = e[1];
            g[x].push_back(y);
            g[y].push_back(x); //建图
        }   
        int ans = INT_MAX;
        for (int i = 0; i < n; ++i) // 枚举每个起点跑 BFS
            ans = min(ans, bfs(i));
        return ans < INT_MAX ? ans : -1;
    }
};
```

<h5 id="NIduz">[2385. 感染二叉树需要的总时间](https://leetcode.cn/problems/amount-of-time-for-binary-tree-to-be-infected/)</h5>
给你一棵二叉树的根节点`root`，二叉树中节点的值 **互不相同。**另给你一个整数`start`。在第`0`分钟，**感染**将会从值为`start`的节点开始爆发。每分钟如果节点满足以下全部条件就会被感染：节点此前还没有感染。节点与一个已感染节点相邻，返回感染整棵树需要的分钟数。

![](https://cdn.nlark.com/yuque/0/2025/png/45533403/1745486668216-0eb61755-260e-44e1-8a10-856ce2268107.png)

```c
class Solution {
    public:
    int amountOfTime(TreeNode* root, int start) {
        unordered_map<int, vector<int>> graph;//构建图
        buildGraph(root, nullptr, graph);//使用BFS计算感染整棵树所需的时间
        return calculateInfectionTime(graph, start);
    }
    private:
    void buildGraph(TreeNode* node, TreeNode* parent, unordered_map<int, vector<int>>& graph) {
        if (node) {
            //如果当前节点不是根节点，则连接父节点和子节点
            if(parent){
                graph[node->val].push_back(parent->val);
                graph[parent->val].push_back(node->val);
            }
            //连接左右子节点
            if (node->left){
                graph[node->val].push_back(node->left->val);
                graph[node->left->val].push_back(node->val);
            }
            if (node->right) {
                graph[node->val].push_back(node->right->val);
                graph[node->right->val].push_back(node->val);
            }
            // 递归构建左右子树的图
            buildGraph(node->left, node, graph);
            buildGraph(node->right, node, graph);
        }
    }
    int calculateInfectionTime(unordered_map<int, vector<int>>& graph, int start) {
        queue<vector<int>> q;
        q.push({start, 0});
        unordered_set<int> visited;
        visited.insert(start);
        int time = 0;
        while (!q.empty()) {
            auto arr = q.front();
            q.pop();
            int nodeVal = arr[0];
            time = arr[1];
            for (int childVal : graph[nodeVal]) {
                if (!visited.count(childVal)) {
                    q.push({childVal, time + 1});
                    visited.insert(childVal);
                }
            }
        }
        return time;
    }
};
```

