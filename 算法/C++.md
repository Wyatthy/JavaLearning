

# 一些小函数

vector容器的大小：`a.size()`

char数组的实际大小：`strlen(a)`

string大小：`s.length();`

## 判断字符串  

```c++
#include<cctype>
int isalpha ( int c );			//判断是否为字母
int isdigit ( int c);			//判断是否为0-9数字
```

## atoi() 和 stoi()

相同点：
①都是C++的字符处理函数，把数字字符串转换成int输出
②头文件都是#include<cstring>

不同点：
①atoi()的参数是 const char* ,因此对于一个字符串str我们必须调用 c_str()的方法把这个string转换成 const char*类型的,而stoi()的参数是const string*,不需要转化为 const char*；
②stoi()会做范围检查，默认范围是在int的范围内的，如果超出范围的话则会runtime error！

```c++
cout<<atoi(s3);		//报错
cout<<atoi(s3.c_str());		//正确

// stoi example
#include <iostream>   // std::cout
#include <string>     // std::string, std::stoi

int main ()
{
  std::string str_dec = "2001, A Space Odyssey";
  std::string str_hex = "40c3";
  std::string str_bin = "-10010110001";
  std::string str_auto = "0x7f";

  std::string::size_type sz;   // alias of size_t

  int i_dec = std::stoi (str_dec,&sz);
  int i_hex = std::stoi (str_hex,nullptr,16);
  int i_bin = std::stoi (str_bin,nullptr,2);
  int i_auto = std::stoi (str_auto,nullptr,0);

  std::cout << str_dec << ": " << i_dec << " and [" << str_dec.substr(sz) << "]\n";
  std::cout << str_hex << ": " << i_hex << '\n';
  std::cout << str_bin << ": " << i_bin << '\n';
  std::cout << str_auto << ": " << i_auto << '\n';

  return 0;
}

/*输出：
2001，《太空漫游》：2001和[，《太空漫游》
40c3：16579
-10010110001：-1201
0x7f：127
*/
```



## to_string()

```c++
// to_string example
#include <iostream>   // std::cout
#include <string>     // std::string, std::to_string

int main ()
{
  std::string pi = "pi is " + std::to_string(3.1415926);
  std::string perfect = std::to_string(1+2+4+7+14) + " is a perfect number";
  std::cout << pi << '\n';
  std::cout << perfect << '\n';
  return 0;
}

/*输出：
pi is 3.141593
28 is a perfect number
*/
```

## lower_bound upper_bound

头文件： #include  <algorithm>

 

二分查找的函数有 3 个： [
](https://www.cnblogs.com/cunyusup/p/8438749.html)

lower_bound(起始地址，结束地址，要查找的数值) 返回的是数值 **第一个** 出现的位置。

upper_bound(起始地址，结束地址，要查找的数值) 返回的是 第一个大于待查找数值 出现的位置。

binary_search(起始地址，结束地址，要查找的数值)  返回的是是否存在这么一个数，是一个**bool值**。

注意：使用二分查找的前提是数组有序。

 

1 函数lower_bound()  参考：[有关lower_bound()函数的使用](https://www.cnblogs.com/is-Tina/p/7294067.html)

 

功能：函数lower_bound()在first和last中的前闭后开区间进行二分查找，返回**大于或等于val的第一个元素位置**。如果所有元素都小于val，则返回last的位置.

注意：如果所有元素都小于val，则返回last的位置，且last的位置是**越界**的！！

 

2 函数upper_bound()

 

功能：函数upper_bound()返回的在前闭后开区间查找的关键字的上界，返回**大于val**的第一个元素位置

注意：返回查找元素的最后一个可安插位置，也就是“元素值>查找值”的第一个元素的位置。同样，如果val大于数组中全部元素，返回的是last。(注意：数组下标越界)

 

**PS**：

​    lower_bound(val):返回容器中第一个值【大于或等于】val的元素的iterator位置。

​    upper_bound(val): 返回容器中第一个值【大于】val的元素的iterator位置。

例子：

```
void main()
{
    vector<int> t;
    t.push_back(1);
    t.push_back(2);
    t.push_back(3);
    t.push_back(4);
    t.push_back(6);
    t.push_back(7);
    t.push_back(8);

    
    int low=lower_bound(t.begin(),t.end(),5)-t.begin();
    int upp=upper_bound(t.begin(),t.end(),5)-t.begin();
    cout<<low<<endl;
    cout<<upp<<endl;
	return 0;
}
```

# 输入输出

## 格式化输出

### printf格式输出数字，位数不够前面补0

```c++
int a = 4;
printf("%03d",a);
/*输出：
004
*/
//也可以用 * 代替位数，在后面的参数列表中用变量控制输出位数；
int a = 4;
int n = 3;
printf("%0*d",n,a);
输出：004
```

printf格式输出数字，保留小数点

```c++
float p=1f;
printf("%.2f",p);
/*输出：
1.00
*/
```



# 体排序

```c++
#include<bits/stdc++.h>
using namespace std;
typedef struct{
	string name,id;
	int score;
}stu; 

bool cmp(stu &a,stu &b)            //当return的是ture时，a先输出，所以是升序
{
     return a.score < b.score;
}
int main(){
	stu li[n];
    //li的初始化
	sort(li,li+n,cmp);
	return 0;
}
```

## 容器

### Unordered Set

Unordered sets are containers that store unique elements in no particular order, and which allow for fast retrieval of individual elements based on their value.

# Dijkstra

```c++
Dijkstra() {
  初始化;
  for(循环n次) {
    u = 使dis[u]最小的还未被访问的顶点的编号;
    记u为确定值;
    for(从u出发能到达的所有顶点v){
      if(v未被访问 && 以u为中介点使s到顶点v的最短距离更优)
        优化dis[v];
    }
  }
}
```

```c++
//邻接矩阵
int n, e[maxv][maxv];
int dis[maxv], pre[maxv];// pre用来标注当前结点的前一个结点
bool vis[maxv] = {false};
void Dijkstra(int s) {
  fill(dis, dis + maxv, inf);
  dis[s] = 0;
  for(int i = 0; i < n; i++) pre[i] = i; //初始状态设每个点的前驱为自身
  for(int i = 0; i < n; i++) {
    int u = -1, minn = inf;
    for(int j = 0; j < n; j++) {
      if(visit[j] == false && dis[j] < minn) {
        u = j;
        minn = dis[j];
      }
    }
    if(u == -1) return;
    visit[u] = true;
    for(int v = 0; v < n; v++) {
      if(visit[v] == false && e[u][v] != inf && dis[u] + e[u][v] < dis[v]) {
        dis[v] = dis[u] + e[u][v];
        pre[v] = u; // pre用来标注当前结点的前一个结点
      }
    }
  }
}
```

```c++
//邻接表
struct node {
  int v, dis;
}
vector<node> e[maxv];
int n;
int dis[maxv], pre[maxv];// pre用来标注当前结点的前一个结点
bool vis[maxv] = {false};
for(int i = 0; i < n; i++) pre[i] = i; //初始状态设每个点的前驱为自身
void Dijkstra(int s) {
  fill(dis, dis + maxv, inf);
  dis[s] = 0;
  for(int i = 0; i < n; i++) {
    int u = -1, minn = inf;
    for(int j = 0; j < n; j++) {
      if(visit[j] == false && dis[j] < minn) {
        u = j;
        minn = dis[j];
      }
    }
    if(u == -1) return ;
    visit[u] = true;
    for(int j = 0; j < e[u].size(); j++) {
      int v = e[u][j].v;
      if(visit[v] == false && dis[u] + e[u][j].dis < dis[v]) {
        dis[v] = dis[u] + e[u][j].dis;
        pre[v] = u;
      }
    }
  }
}
```

## DFS（深度优先搜索）：不撞南墙不回头

## BFS（深度优先搜索）：发散性寻找（分身寻找）

#### 以经典例题：迷宫问题为例

**画个迷宫1表示墙，0表示路。**
![在这里插入图片描述](C++STL.assets/d7aac5d5bc37470eb77a3c1d91da6ba6-1.jpg)

### DFS思想

从起点开始，沿着一条路一直走到底，若是发现不能到达目标解，那就返回到上一个节点，而后从另外一条路开始走到底（即尽可能往深处走）svg

### BFS思想

从起点开始，逐层寻找（发散性寻找）（即往四周走）xml

##### DFS优势：消耗内存少 （容易时间超限）

##### BFS优势：消耗时间少 （容易内存超限）

##### DFS适合题目类型：给定初始状态跟目标状态，要求判断从初始状态到目标状态是否有解。

##### BFS适合题目类型：给定初始状态跟目标状态，要求求出从初始状态到目标状态的最优解。

例如：上述的迷宫问题。
如果问：从迷宫左上角到迷宫右下角是否存在路径长度为9的路径，则用DFS。
如果问：从迷宫左上角到迷宫右下角的最短路径是多少，则用BFS。

# 未解决的问题

双蛋问答

https://blog.csdn.net/u010833547/article/details/105027015

https://blog.csdn.net/jacicson1987/article/details/104935989

```C++

#include<stdio.h>
#define Max(a,b) (a>b?a:b)
#define Min(a,b) (a<b?a:b)
int dp[1005][50];
int main(int argc, char* argv[])
{
	int n,m;
	scanf("%d%d",&n,&m);
	for (int i=1;i<=n;i++)
    {
        dp[i][1]=i;
    }
    for (int cnt=2;cnt<=m;cnt++)
    {
        for (int ind=1;ind<=n;ind++)
        {
            dp[ind][cnt]=1+dp[ind-1][cnt];
            for (int k=2;k<=ind;k++)
                dp[ind][cnt]=Min(dp[ind][cnt],1+Max(dp[k-1][cnt-1],dp[ind-k][cnt]));
        }
    }
    printf("%d\n",dp[n][m]);
	return 0;
}
```

