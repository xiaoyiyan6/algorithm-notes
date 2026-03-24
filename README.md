# 数据结构【二叉树】

## 一.树的存储

### 1.已知父子关系（结构体数组）
//const int MAXN = 1005;
//struct Node
//{
//	int l,r;//左右孩子的编号
//	int val;//节点本值
//} tree[MAXN];
//
//int main()
//{
//	int n;
//	cin >> n;
//	for (int i = 0; i < n; i++)
//	{
//		cin >> tree[i].val >> tree[i].l >> tree[i].r;
//	}
//	return 0;
//}

### 2.无向边（邻接表）

//const int MAXN = 100005;
//vector<int> node[MAXN];
//
//int main()
//{
//	int n;
//	cin >> n;
//	for (int i = 0; i < n; i++)
//	{
//		int u, v;//有边的两个节点编号
//		cin >> u >> v;
//		node[u].push_back(v);//我加上你
//		node[v].push_back(u);//你加上我，就构成连接啦
//	}
//	return 0;
//}

## 二。树的遍历

### 1.DFS

//const int MAXN = 10005;
//vector<int> node[MAXN];
//bool flag[MAXN];
//
//void dfs(int u)
//{
//	flag[u] = true;//每历遍一个新的就标记一下
//	cout << u ;//前序输出
//	for (int v : node[u])
//	{
//		if (!flag[v])//没被标记过（就是没有遍历过）
//		{
//			dfs(v);
//		}
//	}
//}//走迷宫沿着一条路一直走，走到头了才回到起点走下一条路

### 3.BFS

/const int MAXN = 10005;
//vector<int> node[MAXN];
//bool flag[MAXN];
//
//void bfs(int start)
//{
//	queue<int> q;
//	q.push(start);
//	flag[start] = true;
//	while (!q.empty())
//	{
//		int u = q.front(); q.pop();
//		cout << u;
//		for (int v : node[u])
//		{
//			if (!flag[v])
//			{
//				flag[v] = true;
//				q.push(v);//用的是队列，先进先出，孩子是压在后面，都是每一层的取完了才轮到下一层
//			}    
//		}
//	}
//}

### 3.前中后遍历（递归）

//#include <iostream>
//using namespace std;
//struct TreeNode
//{
//	int val;
//	TreeNode* left;
//	TreeNode* right;
//	TreeNode(int x):val(x),left(nullptr),right(nullptr){}
//};
//void preorder(TreeNode* head)//前序
//{
//	if (head == nullptr)return;
//	cout << head->val << " " ;
//	preorder(head->left);
//	preorder(head->right);
//}
//void inorder(TreeNode* head)//中序
//{
//	if (head == nullptr)return;
//	
//	preorder(head->left);
//	cout << head->val << " ";
//	preorder(head->right);
//}
//void posorder(TreeNode* head)//后序
//{
//	if (head == nullptr)return;
//	
//	preorder(head->left);
//	preorder(head->right);
//	cout << head->val << " ";
//}
//void deleteTree(TreeNode* head)
//{
//	if (head == nullptr)return;
//	deleteTree(head->left);
//	deleteTree(head->right);
//	delete head;
//}
//int main()
//{
//	TreeNode* head = new TreeNode(1);
//	head->left = new TreeNode(2);
//	head->right = new TreeNode(3);
//	head->left->left = new TreeNode(4);
//	head->left->right = new TreeNode(5);
//	head->right->left = new TreeNode(7);
//	preorder(head);
//	cout << endl;
//	inorder(head);
//	cout << endl;
//	posorder(head);
//	cout << endl;
//	return 0;
//}

## 三。树的属性

### 1.深度
//const int MAXN = 100005;
//vector<int> node[MAXN];
//int depth[MAXN];
//int maxdepth = 0;
//
//void dfs(int u, int fa)//这里是默认先进来的是father
//{
//	depth[u] = depth[fa] + 1;
//	maxdepth = max(maxdepth, depth[u]);//如果不要这一行的话，就有可能先走了最长的那一支，然后又走了短的，就没有把最长的记下来
//	for (int v : node[u])
//	{
//		if (v != fa)dfs(v, u);
//	}
//}
//
//int main()
//{
//	int n;
//	cin >> n;
//	for (int i = 0; i < n; i++)
//	{
//		int u, v;
//		cin >> u >> v;
//		node[u].push_back(v);
//		node[v].push_back(u);
//	}
//
//	depth[0] = 0;
//	dfs(1,0);
//	cout << "深度：" << maxdepth << endl;
//	return 0;
//}

### 2.宽度
const int MAXN = 100005;
vector<int> node[MAXN];
int cntdepth[MAXN];//记录每一层的节点数
int maxdepth = 0;

void bfs(int start)
{
	queue<int> q;
	q.push(start);
	vector<int> depth(MAXN, 0);
	depth[start] = 1;
	cntdepth[1]++;
	maxdepth = 1;//这堆都是在初始化
	while (!q.empty())
	{
		int u = q.front(); q.pop();//永远是一层先pop完了才轮到下一层
		for (int v : node[u])
		{
			if (depth[v] == 0)
			{
				depth[v] = depth[u] + 1;
				cntdepth[depth[v]]++;//每遍历一个孩子就+1
				maxdepth = max(maxdepth, depth[v]);
				q.push(v);
			}
		}
	}
}
int main()
{
	int n;
	cin >> n;
	for (int i = 0; i < n; i++)
	{
		int u, v;
		cin >> u >> v;
		node[u].push_back(v);
		node[v].push_back(u);
	}
	bfs(1);
	int width = 0;
	for (int i = 0; i <= maxdepth; i++)
	{
		width = max(width, cntdepth[i]);
	}
	cout << width;
	return 0;
}

### 3.每个节点的子树大小

const int MAXN = 100005;
vector<int> node[MAXN];
int sz[MAXN];
void dfs(int u, int fa)
{
	sz[u] = 1;
	for (int v : node[u])
	{
		if (v != fa)
		{
			dfs(v, u);
			sz[u] += sz[v];
		}
	}
}

### 4.树的直径（最远两个节点之间的距离）

const int MAXN = 100005;
vector<int> node[MAXN];

pair<int, int> bfs(int start)
{
	vector<int> dist(MAXN, -1);
	queue<int> q;
	q.push(start);
	dist[start] = 0;
	int farnode = start;
	while (!q.empty())
	{
		int u = q.front(); q.pop();
		for (int v : node[u])
		{
			if (dist[v] == -1)
			{
				dist[v] = dist[u] + 1;
				q.push(v);
				if (dist[v] > dist[farnode])farnode = v;
			}
		}
	}
	return { farnode,dist[farnode] };
}

int main()
{
	int n;
	cin >> n;
	for (int i = 1; i < n; i++)
	{
		int u, v;
		cin >> u >> v;
		node[u].push_back(v);
		node[v].push_back(u);
	}
	int p = bfs(1).first;//从root出发找到最远点
	int diameter = bfs(p).second;//又从最远点出发找到第二个最远点
	return 0;
}

### 5.树的重心（去掉它之后会有几个连通块，重心的最大连通块在所有最大连通块中是最小的）

const MAXN = 100005;
vector<int> node[MAXN];
int n,sz[MAXN];//n是总的节点数，sz是记录当前节点的子节点数
int ans_node, ans_size = 1e9;//前者是记录重心，后者是最大连通块的大小
void dfs(int u, int fa)
{
	sz[u] = 1;
	int max_part = 0;
	for (int v : node[u])
	{
		if (v != fa)
		{
			dfs(v, u);
			sz[u] += sz[v];
			max_part = max(max_part, sz[v]);//一个节点有两个脚（两个孩子），这里就是在比较两个子树的连通块的大小
		}
	}
	max_part = max(max_part, n - sz[u]);//这里是在比较父向连通块与子向连通块的大小（选出当前节点的最大连通块）
	if (max_part < ans_size)
	{
		ans_size = max_part;
		ans_node = u;//记录该节点，就是重心
	}
}

### 6.两节点的最近公共祖先
const int MAXN = 100005;
vector<int> node[MAXN];
int parent[MAXN], depth[MAXN];

void dfs(int u, int fa)
{
	parent[u] = fa;
	depth[u] = depth[fa] + 1;//先把自己处理完
	for (int v : node[u])
	{
		if (v != fa)dfs(v, u);//才去遍历孩子，递归孩子
	}
}

int lca(int u, int v)//这里uv是节点编号呀
{
	while (depth[u] > depth[v]) u = parent[u];//如果不同层就要先把他们弄同层
	while (depth[v] > depth[u]) v = parent[v];
	while (u != v)
	{
		u = parent[u];//然后不断往上拔，直到相遇
		v = parent[v];
	}
	return u;
}


### 7.树形dp（树的最大独立集（不能选相邻的）

const int MAXN = 100005;
vector<int> node[MAXN];
int dp0[MAXN], dp1[MAXN];//前者是不选的权值，后者是选的权值
int val[MAXN];

void dfs(int u, int fa)
{
	dp1[u] = val[u];//选的话就加上自己的权值
	dp0[u] = 0;//不选的话就初始值为零
	for (int v : node[u])
	{
		if (v != fa)
		{
			dfs(v, u);//后序遍历，先处理完孩子，再处理父亲
			dp1[u] += dp0[v];//父亲选了就不能选孩子
			dp0[u] += max(dp0[v], dp1[v]);//父亲没选孩子就选或者不选
		}
	}
}

### 8.换根（所有节点到它的距离和换成到它的孩子的距离和）

const int MAXN = 100005;
vector<int> node[MAXN];
long long sz[MAXN], f[MAXN];//前者是子节点数，后者是所有节点到它的距离和
int n;
void dfs1(int u, int fa)
{
	sz[u] = 1;
	for (int v : node[u])
	{
		if (v != fa)
		{
			dfs1(v, u);
			sz[u] += sz[v];
			f[1] += sz[v];
		}
	}
}
void dfs2(int u, int fa)
{
	for (int v : node[u])
	{
		if (v != fa)
		{
			f[v] = f[u] + n - 2 * sz[v];
			dfs2(v, u);
		}
	}
}
int main()
{
	cin >> n;
	for (int i = 0; i < n; i++)
	{
		int u, v;
		cin >> u >> v;
		node[u].push_back(v);
		node[v].push_back(u);
	}
	dfs1(1, 0);
	dfs2(1, 0);
	for (int i = 0; i <= n; i++)
	{
		cout << "以" << i << "为根的总距离：" << f[i] << endl;
	}
	return 0;
}





