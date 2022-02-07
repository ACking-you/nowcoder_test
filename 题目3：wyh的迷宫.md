[toc]

## 题目三：wyh的迷宫

[题目链接](https://ac.nowcoder.com/acm/contest/26908/1003)

### 解题分析

很快想到dfs或者bfs解法，从起点开始dfs或bfs即可。然而这次也是情况和第一题类似。。

我bfs能过，而提前阻断优化后的dfs则通不过了。。。就离谱。

**解题具体思路：**

建图思路：由于题目有多组数据，所以我们用vector数组实现变长的随时变化处理，可以重复利用同一片空间，节省空间复杂度。我把障碍物标记为1，终点标记为2，其他标记为0。

搜索思路：可以用bfs或者dfs(需要剪枝，我不知为何没剪枝成功)

### 解题代码

#### dfs(只能过一例，未剪枝成功)

```cpp
#include<bits/stdc++.h>
 
using namespace std;

const int dx[4]{-1,1,0,0};
const int dy[4]{0,0,-1,1};
vector<int> mmp[505];
bool check[505][505]{0};
int t,n,m;
bool f = false;
inline bool isValid(int x,int y){
    return x>=0&&x<n&&y>=0&&y<m;
}
void dfs(int x,int y){
    if(mmp[x][y]==2){
        f = true;
        return ;
    }
    for (int i = 0; i < 4; ++i) {
        int c_x = x+dx[i];
        int c_y = y+dy[i];
        if(f)//TODO 如果已经搜索到了就提前阻断防止继续搜索
            return;
        if(isValid(c_x,c_y)&&!check[c_x][c_y]&&mmp[c_x][c_y]!=1){
            check[c_x][c_y] = 1;
            dfs(c_x,c_y);
            check[c_x][c_y] = 0;
        }
    }
}
int main(){
    ios::sync_with_stdio(false);
    cin>>t;
    char tmp[505];
    int x,y;
    while (t--){
        cin>>n>>m;
        for (int i = 0; i < n; ++i) {
            cin>>tmp;
            mmp[i].resize(m);
            for (int j = 0; j < m; ++j) {
                if(tmp[j]=='s'){
                    mmp[i][j] = 0;
                    x = i;y = j;
                }else if(tmp[j]=='x'){
                    mmp[i][j] = 1;//1代表障碍物
                }else if(tmp[j]=='t'){
                    mmp[i][j] = 2;//2代表终点
                } //其余情况默认值0即可
                else{
                    mmp[i][j] = 0;
                }
            }
        }
        dfs(x,y);
        if(f)cout<<"YES"<<'\n';
        else cout<<"NO"<<'\n';
        f = false;
    }
    return 0;
}
```



#### bfs(正统解法)

> 效率还可，继续优化的话，可以双向bfs
>
> ![](https://img-blog.csdnimg.cn/94d788caad5a43e4abd54c5ac9b772e6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQysrKysrKysrKysrKysrKysrKys=,size_20,color_FFFFFF,t_70,g_se,x_16)

```cpp
#include<bits/stdc++.h>
 
using namespace std;

const int dx[4]{-1,1,0,0};
const int dy[4]{0,0,-1,1};	//用于上下左右移动位置
vector<int> mmp[505];		//用于存图
int t,n,m;
//TODO 判断是否越界
inline bool isValid(int x,int y){
    return x>=0&&x<n&&y>=0&&y<m;
}
//TODO bfs搜索过程
bool bfs(int x,int y){
    queue<pair<int,int>>Q;
    bool check[505][505]{0};//记录已经走过的点，防止走回头路
    Q.push({x,y});
    check[x][y] = 1;
    while (!Q.empty()){
        auto& t = Q.front();Q.pop();
        if(mmp[t.first][t.second]==2)
            return true;
        for (int i = 0; i < 4; ++i) {
            int tx = t.first+dx[i];
            int ty = t.second+dy[i];
            if(isValid(tx,ty)&&!check[tx][ty]&&mmp[tx][ty]!=1){
                check[tx][ty] = 1;
                Q.push({tx,ty});
            }
        }
    }
    return false;
}
int main(){
    ios::sync_with_stdio(false);
    cin>>t;
    char tmp[505];//TODO 用于记录每一行的字符
    int x,y;
    while (t--){
        cin>>n>>m;
        for (int i = 0; i < n; ++i) {
            cin>>tmp;
            mmp[i].resize(m);
            for (int j = 0; j < m; ++j) {
                if(tmp[j]=='s'){
                    mmp[i][j] = 0;
                    x = i;y = j;
                }else if(tmp[j]=='x'){
                    mmp[i][j] = 1;//1代表障碍物
                }else if(tmp[j]=='t'){
                    mmp[i][j] = 2;//2代表终点
                } //其余情况默认值0即可
                else{
                    mmp[i][j] = 0;
                }
            }
        }
        //TODO 根据bfs搜索的答案进行打印
        bool f = bfs(x,y);
        if(f)cout<<"YES"<<'\n';
        else cout<<"NO"<<'\n';
    }
    return 0;
}
```