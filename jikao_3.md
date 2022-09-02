```c++
//	7月11日上午9点考试：考场及考试安排请助教发群里
//	考试范围：（图，顶点数据类型：字符串）
//		1. 图（或网）的邻接矩阵创建及输出
//		2. 求图中顶点的度
//		3. 图中增加顶点或者边
//		4. 图中删除顶点或者边
//		5. 图的深度遍历（或广度遍历）
//		6. 图或网的邻接表的创建和输出

#include <iostream>
#include <cstdio>
#include <string>
#include <algorithm>
#include <iso646.h>

using namespace std;

const int MAXSIZE = 20;

typedef struct MGraph
{
	string vertex[MAXSIZE];
	int Matrix[MAXSIZE][MAXSIZE];
	int vertexNum, arcsNum;
}MGraph, * MGraphPtr;

MGraph* create_MGraph()
{
	MGraph* G = new MGraph;

	printf("请输入顶点数和边数:");
	cin >> G->vertexNum >> G->arcsNum;

	getchar();
	for (int i = 0; i < G->vertexNum; i++) {
		printf("请输入第%d个顶点的值: ", i + 1);
		getline(cin, G->vertex[i]);
	} cout << endl;

	printf("请输入边不存在的标志：");
	int flag;	cin >> flag;

	for (int i = 0; i < G->vertexNum; i++)
		for (int j = 0; j < G->vertexNum; j++)
			if (i == j) G->Matrix[i][j] = 0;
			else G->Matrix[i][j] = flag;

	for (int k = 0; k < G->arcsNum; k++)	
	{
		int i, j, w;
		printf("请输入第%d条边的弧尾、弧头、序号和权值: ", k + 1);
		cin >> i >> j >> w;
		G->Matrix[i][j] = w;
		G->Matrix[j][i] = w;
	}
	return G;
}

void show_MGraph(MGraph* G)
{
	cout << "\t" ;
	for (int i = 0; i < G->vertexNum; i++) cout << G->vertex[i] << "\t"; cout << endl;

	for (int i = 0; i < G->vertexNum; i++) {
		cout << G->vertex[i] << "\t";
		for (int j = 0; j < G->vertexNum; j++) cout << G->Matrix[i][j] << "\t";
		cout << endl;
	}
}

int* degree(MGraph* G)
{
	int* d = new int[MAXSIZE];
	memset(d, 0, sizeof(int) * MAXSIZE);
	for (int i = 0; i < G->vertexNum; i++) {
		for (int j = 0; j < G->vertexNum; j++) {
			if (G->Matrix[i][j] != 52725 and i != j) d[i] ++;
		}
	}
	return d;
}

int positionInGraph(MGraph* G, string find)
{
	for (int i = 0; i < G->vertexNum; i++) 
		if (G->vertex[i] == find) return i;
	return -1;
}

void deleteVertex(MGraph** G)
{
	string del;
	cin >> del;
	int d = positionInGraph(*G, del);
	for (int i = 0; i < (*G)->vertexNum; i++) {
		(*G)->Matrix[i][d] = 52725;
		(*G)->Matrix[d][i] = 52725;
	}
}

void deleteArc(MGraph** G)
{
	string del_start, del_end;
	cin >> del_start >> del_end;
	int d1 = positionInGraph(*G, del_start);
	int d2 = positionInGraph(*G, del_end);
	(*G)->Matrix[d1][d2] = 52725;
	(*G)->Matrix[d2][d1] = 52725;
	(*G)->arcsNum--;
}

void addVertex(MGraph** G)
{
	string add;
	cin >> add;
	int newPos = (*G)->vertexNum;
	(*G)->vertex[newPos] = add;
	(*G)->vertexNum++;
	for (int i = 0; i < (*G)->vertexNum; i++) {
		(*G)->Matrix[i][newPos] = 52725;
		(*G)->Matrix[newPos][i] = 52725;
	}
	(*G)->vertexNum++;
}

void addArc(MGraph** G)
{
	string add_start, add_end;
	int w;
	cin >> add_start >> add_end >> w;
	int d1 = positionInGraph(*G, add_start);
	int d2 = positionInGraph(*G, add_end);
	(*G)->Matrix[d1][d2] = w;
	(*G)->Matrix[d2][d1] = w;
	(*G)->arcsNum++;
}

int visited[MAXSIZE] = { 0 };

void dfs(MGraph* G, int start, int visited[])
{
	cout << G->vertex[start] << ' ';
	visited[start] = 1;
	for (int i = 0; i < G->vertexNum; i++) {
		if (G->Matrix[start][i]!=52725 and G->Matrix[start][i]!=0 and visited[i] == 0) {
			dfs(G, i, visited);
		}
	}
}

void dfs_MGraph(MGraph* G)
{
	string s;
	cin >> s;
	memset(visited, 0, sizeof visited);
	int start = positionInGraph(G, s);
	dfs(G, start, visited);
	for (int i = 0; i < G->vertexNum; i++) {
		if (visited[i] != 1)
			dfs(G, i, visited);
	}
}

void dfs_path(MGraph* G, int start, int visited[])
{
	//cout << G->vertex[start] << ' ';
	visited[start] = 1;
	for (int i = 0; i < G->vertexNum; i++) {
		if (G->Matrix[start][i] != 52725 and G->Matrix[start][i] != 0 and visited[i] == 0) {
			dfs_path(G, i, visited);
		}
	}
}

void dfs_ifPathExist(MGraph* G)
{
	string s, e;
	cin >> s >> e;
	memset(visited, 0, sizeof visited);
	int start = positionInGraph(G, s);
	int end = positionInGraph(G, e);
	dfs(G, start, visited);
	if (visited[end] == 1) cout << "EXIST!" << endl;
	else cout << "NOT EXIST!" << endl;
}

typedef struct queueNode
{
	string qData;
	struct queueNode* next;
}queueNode, * queueNodePtr;

typedef struct Queue
{
	queueNode* head;
	queueNode* rear;
}Queue, * QueuePtr;

Queue* initQueue()
{
	Queue* Q = new Queue;
	Q->head = Q->rear = new queueNode;
	Q->head->next = NULL;
	return Q;
}

void enQueue(Queue* Q, int t)
{
	queueNode* s = new queueNode;
	s->qData = t;
	s->next == NULL;
	Q->rear->next = s;
	Q->rear = s;
	return;
}

int isEmpty(Queue* Q)
{
	if (Q->head == Q->rear)
		return 1;
	else return 0;
}

string deQueue(Queue* Q)
{
	if (isEmpty(Q))
		return NULL;
	queueNode* s = Q->head->next;
	Q->head->next = s->next;

	if (Q->rear == s)
		Q->rear = Q->head;

	return s->qData;
}

void bfs(MGraph* G, int start, int visited[])
{
	int i, j;		
	Queue* Q = initQueue();
	for (i = 0; i < G->vertexNum; i++) {
		if (visited[i] == 0)	
		{
			visited[i] = 1;		
			cout << G->vertex[i];
			enQueue(Q, i);		
			while (!isEmpty(Q)) {
				i = positionInGraph(G, deQueue(Q));	
				for (j = 0; j < G->vertexNum; j++) {
					if (G->Matrix[i][j] == 1 && visited[j] == 0) {
						visited[j] = 1;
						cout << G->vertex[j];
						enQueue(Q, j);
					}
				}
			}
		}
	}
}

void bfs_MGraph(MGraph* G)
{
	string s;
	cin >> s;
	memset(visited, 0, sizeof visited);
	int start = positionInGraph(G, s);
	bfs(G, start, visited);
	for (int i = 0; i < G->vertexNum; i++) {
		if (visited[i] != 1)
			bfs(G, i, visited);
	}
}

//邻接表存储代码
typedef struct arcNode		//边链表
{
	int vex_num;
	int weight;
	arcNode* next;
}arcNode;

typedef struct vexNode		//顶点表数据类型
{
	string vex;
	arcNode* First;
}vexNode;

typedef struct ALGraph		//邻接表数据类型
{
	vexNode List[MAXSIZE];
	int vexNum;
	int	arcsNum;
}ALGraph;

ALGraph* create_ALGraph()
{
	ALGraph* G = new ALGraph;
	cout << "请输入顶点数和边数：";
	cin >> G->vexNum >> G->arcsNum;

	getchar();
	cout << "请依次输入各顶点的值：" << endl;
	for (int v = 0; v < G->vexNum; v++) {
		getline(cin, G->List[v].vex);
		G->List[v].First = NULL;
	}

	cout << "请依次输入弧头序号、弧尾序号、权值：" << endl;
	int start, end, w;
	for (int k = 0; k < G->arcsNum; k++) {
		cin >> start >> end >> w;
		arcNode* s = new arcNode;
		s->vex_num = end;
		s->weight = w;
		s->next = G->List[start].First;
		G->List[start].First = s;
		
		arcNode* t = new arcNode;
		t->vex_num = start;
		t->weight = w;
		t->next = G->List[end].First;
		G->List[end].First = t;
	}
	return G;
}

void show_ALGraph(ALGraph* GL)
{
	printf("\n图的邻接表为：\n");
	for (int i = 0; i < GL->vexNum; i++) {
		if (GL->List[i].First) {
			printf("\t____\n");
			cout << '\t' << GL->List[i].vex << "-->";
			arcNode* s = GL->List[i].First;
			while (s) {
				arcNode* p = s->next;
				cout << GL->List[s->vex_num].vex << '(' << s->weight << ')';
				if (p) printf("-->");
				s = s->next;
			}
			printf("\n");
			printf("\t￣￣\n");
		}
	}
}

int posInALGraph(ALGraph* G, string find)
{
	for (int i = 0; i < G->vexNum; i++)
		if (G->List[i].vex == find) return i;
	return -1;
}

void dfs_alg(ALGraph* GL, int* visited, int index)
{
	cout << GL->List[index].vex << ' ';
	visited[index] = 1;

	arcNode* p = GL->List[index].First;
	while (p) {
		if (visited[p->vex_num] == 0)
			dfs_alg(GL, visited, p->vex_num);
		p = p->next;
	}
}

void dfs_ALGraph(ALGraph* GL)
{
	string s;
	cin >> s;
	memset(visited, 0, sizeof visited);
	int start = posInALGraph(GL, s);
	dfs_alg(GL, visited, start);
	for (int i = 0; i < GL->vexNum; i++) {
		if (visited[i] != 1)
			dfs_alg(GL, visited, i);
	}
	cout << endl;
}

void bfs_alg(ALGraph* GL, int* visited, int index)
{
	Queue* Q = initQueue();
	printf("%c\t", GL->List[index].vex);
	visited[index] = 1;
	enQueue(Q, index);
	while (!isEmpty(Q)) {
		int i =  posInALGraph(GL, deQueue(Q));
		arcNode* p = GL->List[index].First;
		while (p) {
			if (visited[p->vex_num] == 0) {
				printf("%c\t", GL->List[p->vex_num].vex);
				visited[p->vex_num] = 1;
				enQueue(Q, p->vex_num);
			}
			p = p->next;
		}
	}
}

void bfs_ALGraph(ALGraph* GL)
{
	string s;
	cin >> s;
	memset(visited, 0, sizeof visited);
	int start = posInALGraph(GL, s);
	dfs_alg(GL, visited, start);
	for (int i = 0; i < GL->vexNum; i++) {
		if (visited[i] != 1)
			bfs_alg(GL, visited, i);
	}
	cout << endl;
}

ALGraph* turn_MgToALg(MGraph* G)
{
	ALGraph* GL = new ALGraph;
	GL->vexNum = G->vertexNum;
	GL->arcsNum = G->arcsNum;

	for (int i = 0; i < GL->vexNum; i++) {
		GL->List[i].vex = G->vertex[i];
		GL->List[i].First = NULL;
	}

	int start, end, w;

	for (int i = 0; i < GL->vexNum; i++) {
		for (int j = 0; j < GL->vexNum; j++) {
			if (G->Matrix[i][j] != 52725 and G->Matrix[i][j] != 0 ) {
				start = i, end = j, w = G->Matrix[i][j];
				arcNode* s = new arcNode;
				s->vex_num = end;
				s->weight = w;
				s->next = GL->List[start].First;
				GL->List[start].First = s;
			}
		}
	}
	return GL;
}

int main()
{
	MGraph* G = create_MGraph();
	show_MGraph(G);
	//deleteVertex(&G);
	//show_MGraph(G);
	//deleteArc(&G);
	//show_MGraph(G);
	//addVertex(&G);
	//addArc(&G);
	//show_MGraph(G);
	//dfs_MGraph(G);
	//int* deg = degree(G);
	//for (int i = 0; i < G->vertexNum; i++) {
	//	cout << deg[i] << ' ';
	//}
	//bfs_MGraph(G);
	ALGraph* GL = turn_MgToALg(G);
	show_ALGraph(GL);
	//dfs_ifPathExist(G);
	return 0;
}

```