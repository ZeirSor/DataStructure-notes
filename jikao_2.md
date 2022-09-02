## 第二次机考

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <windows.h>
#include <conio.h>
#include <iostream>
using namespace std;
#define MAX 50

typedef struct treeNode
{
	char data[MAX];
	struct treeNode* leftChild;
	struct treeNode* rightChild;
}treeNode, * treeNodePtr;

typedef struct queueNode
{
	treeNode* qData;
	struct queueNode* next;
}queueNode, * queueNodePtr;

typedef struct Queue
{
	queueNode* head;
	queueNode* rear;
}Queue, * QueuePtr;

void preCreate(treeNode** T)
{
	char str[MAX];
	gets_s(str);
	if (strcmp(str, "#") == 0) {
		*T == NULL;
	}
	else {
		*T = (treeNode*) malloc(sizeof(treeNode));
		(*T)->leftChild = NULL;
		(*T)->rightChild = NULL;
		strcpy((*T)->data, str);
		preCreate(&(*T)->leftChild);
		preCreate(&(*T)->rightChild);
	}
}

void preOrder(treeNode* T)
{
	if (T == NULL)
		return;
	else {
		printf("%s ", T->data);
		//puts(T->data);
		preOrder(T->leftChild);
		preOrder(T->rightChild);
	}
}

void inOrder(treeNode* T)
{
	if (T == NULL)
		return;
	else {
		preOrder(T->leftChild);
		//puts(T->data);
		printf("%s ", T->data);
		preOrder(T->rightChild);
	}
}

void postOrder(treeNode* T)
{
	if (T == NULL)
		return;
	else {
		preOrder(T->leftChild);
		preOrder(T->rightChild);
		//puts(T->data);
		printf("%s ", T->data);
	}
}

const char* dentPrint(treeNode* T, int level, const char* flag)
{
	int i, j;
	if (T) {
		for (i = 1; i < level; i++) {
			printf(" ");
		}
		printf("%s(%s)", T->data, flag);
		for (j = i; j < 20; j++) {
			printf("-");
		}
		cout << endl;
		dentPrint(T->leftChild, level + 4, "L");
		dentPrint(T->rightChild, level + 4, "R");
		return "PRINT SUCCESSFULLY!";
	}
	else return "THE TREE IS NULL!";
}

int sumNode(treeNode* T)
{
	if (T == NULL)
		return 0;
	else {
		int left = sumNode(T->leftChild);
		int right = sumNode(T->rightChild);
		return left + right + 1;
	}
}

int leaveCount(treeNode* T)
{
	if (!T)
		return 0;
	if (!(T->leftChild) && !(T->rightChild))
		return 1;
	else
		return leaveCount(T->leftChild) + leaveCount(T->rightChild);
}

int depthTree(treeNode* T)
{
	if (!T)
		return 0;
	else {
		int leftDepth = depthTree(T->leftChild);
		int rightDepth = depthTree(T->rightChild);
		if (leftDepth > rightDepth)
			return leftDepth + 1;
		else
			return rightDepth + 1;
	}
}

treeNode* searchTree(treeNode* T, const char* str)
{
	if (!T)
		return NULL;
	if (strcmp(T->data, str) == 0)
		return T;
	//dentPrint(T, 4, "root");
	searchTree(T->leftChild, str);
	searchTree(T->rightChild, str);
}

Queue* initQueue()
{
	Queue* Q = (Queue*) malloc(sizeof(Queue));
	Q->head = Q->rear = (queueNode*) malloc(sizeof(queueNode));
	Q->head->next = NULL;
	return Q;
}

void enQueue(Queue* Q, treeNode* t)
{
	queueNode* s = (queueNode*) malloc(sizeof(queueNode));
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

treeNode* deQueue(Queue* Q)
{
	if (isEmpty(Q))
		return NULL;
	queueNode* s = Q->head->next;
	Q->head->next = s->next;

	if (Q->rear == s)
		Q->rear = Q->head;

	return s->qData;
}

int queueLength(Queue* Q)
{
	int len = 0;
	queueNode* p = Q->head;
	while (p != Q->rear) {
		len++;
		p = p->next;
	}
	return len;
}

void levelTraverse(treeNode* T)
{
	Queue* Q = initQueue();
	enQueue(Q, T);
	while (!isEmpty(Q)) {
		treeNode* de = deQueue(Q);
		printf("%s ", de->data);
		if (de->leftChild)
			enQueue(Q, de->leftChild);
		if (de->rightChild)
			enQueue(Q, de->rightChild);
	}
}

int widthTree(treeNode* T)
{
	Queue* Q = initQueue();
	int len = 0;
	int width = 0;
	enQueue(Q, T);
	while (!isEmpty(Q)) {
		len = queueLength(Q);
		if (len > width)
			width = len;
		for (int i = 0; i < len; i++) {
			treeNode* de = deQueue(Q);
			if (de->leftChild)
				enQueue(Q, de->leftChild);
			if (de->rightChild)
				enQueue(Q, de->rightChild);
		}
	}
	return width;
}

const char* delTree(treeNode** T)
{
	if ((*T)->leftChild) {
		delTree(&(*T)->leftChild);
	}
	if ( ! ( (*T)->leftChild && (*T)->leftChild ) ) {
		free(*T);
		*T = NULL;
		return "DELETE SUCCESSFULLY!";
	}
	if ((*T)->leftChild) {
		delTree(&(*T)->leftChild);
	}
}

int main(int argc, char* argv[])
{
	treeNode* T ;
	cout << "请基于前序创建二叉树" << endl;
	preCreate(&T);		//前序创建
	system("cls");
	int fun;
	while (1) {
		printf("\n\n");
		printf("\t\t\t************************************************************\n");
		printf("\t\t\t*************************** 菜单 ***************************\n");
		printf("\t\t\t	| 01 |	前序创建二叉树\n");
		printf("\t\t\t	| 02 |	前序遍历\n");
		printf("\t\t\t	| 03 |	中序遍历\n");
		printf("\t\t\t	| 04 |	后序遍历\n");
		printf("\t\t\t	| 05 |	层次遍历\n");
		printf("\t\t\t	| 06 |	凹入输出\n");
		printf("\t\t\t	| 07 |	总结点数\n");
		printf("\t\t\t	| 08 |	叶子结点数\n");
		printf("\t\t\t	| 09 |	二叉树的深度\n");
		printf("\t\t\t	| 10 |	二叉树的宽度\n");
		printf("\t\t\t	| 11 |	给定值查找\n");
		printf("\t\t\t	| 12 |	删除整个二叉树\n");
		printf("\t\t\t	| 0  |	退出程序\n");
		printf("\t\t\t************************************************************\n");
		printf("\t\t\t************************************************************\n");
		cin >> fun;
		switch (fun) {
		case 0:
			return 0;
		case 1:
			system("cls");
			preCreate(&T);		//前序创建
			break;
		case 2:
			system("cls");
			preOrder(T);		//前序遍历
			printf("\n");
			break;
		case 3:
			system("cls");
			inOrder(T);			//中序遍历
			printf("\n");
			break;
		case 4:
			system("cls");
			postOrder(T);		//后序遍历
			printf("\n");
			break;
		case 5:
			system("cls");
			levelTraverse(T);	//层次遍历
			printf("\n");
			break;
		case 6:
			system("cls");
			cout << dentPrint(T, 4, "Root");	//凹入输出
			printf("\n");
			break;
		case 7:
			system("cls");
			cout << "总节点数为：" << sumNode(T);
			printf("\n");
			break;
		case 8:
			system("cls");
			cout << "叶子节点数为：" << leaveCount(T); cout << endl;
			printf("\n");
			break;
		case 9:
			system("cls");
			cout << "二叉树的深度为：" << depthTree(T) << endl;
			printf("\n");
			break;
		case 10:
			system("cls");
			cout << "二叉树的宽度为：" << widthTree(T) << endl;
			printf("\n");
			break;
		case 11:
			system("cls");
			char find[MAX];
			cout << "请输入查找值：" << endl;
			cin >> find;
			if (searchTree(T, find)) {
				cout << "SUCCESS!" << endl;
				dentPrint(searchTree(T, find), 4, "root");
			}
			else cout << "NULL！" << endl;
			printf("\n");
			break;
		case 12:
			system("cls");
			cout << delTree(&T) << endl;
			cout << dentPrint(T, 4, "root") << endl;
			printf("\n");
			break;
		default:
			system("cls");
			printf("输入错误！\n");
			break;
		}
		printf("请按任意键继续！");
		getchar();
		getchar();
		system("cls");
	}

	//cout << searchTree(T, "凹入")->data << endl;
	//searchTree(T, "叶子");
	//searchTree(T, "凹入");
	//searchTree(T, "fault");

	return 0;
}
```

```c++
/*
*		测试题目：组织结构模拟
*	具体内容：
*		基本信息包括：机构名
*		功能：
*			1） 初始化创建一个包含 n 个部门名称的孩子兄弟链表存储结构
*			2） 凹入表法输出整个树状结构（树的先序遍历） 
*			3） 求组织图的深度（即树的深度） 
*			4） 将所有没有下级部门的部门名输出（即树的叶子结点） 
*			5） 按部门名进行查找，并输出该部门的下一层所有部门
*			6） 在部门名的下一层中，插入一个新的机构
*			7） 删除整个组织机构（即删除孩子兄弟链表）
*/

#define _CRT_SECURE_NO_WARNINGS
#include<iostream>
#include<cstdio>
#include<cstdlib>
#include<cstring>
#include<cmath>
#include<windows.h>


using namespace std;

#define MAX 50

typedef struct csTree
{
	char data[MAX];
	struct csTree* firstChild;
	struct csTree* nextChild;
}csTree, * csTreePtr;

typedef struct queueNode
{
	csTree* qData;
	struct queueNode* next;
}queueNode, * queueNodePtr;

typedef struct Queue
{
	queueNode* head;
	queueNode* rear;
}Queue, * QueuePtr;

Queue* initQueue()
{
	Queue* q = (QueuePtr)malloc(sizeof(Queue));
	q->head = q->rear = (queueNode*)malloc(sizeof(queueNode));
	q->head ->next =  NULL;
	return q;
}

void enQueue(Queue* q, csTree* t)
{
	queueNode* s = (queueNode*) malloc(sizeof(queueNode));
	s->qData = t;
	s->next = NULL;
	q->rear->next = s;
	q->rear = s;
	return ;
}

int isEmpty(Queue* q)
{
	if (q->head == q->rear)
		return 1;
	else return 0;
}

csTree* deQueue(Queue* q)
{
	if (isEmpty(q))
		return NULL;
	queueNode* de = q->head->next;
	q->head->next = de->next;

	if (q->rear == de)
		q->rear = q->head;

	return de->qData;
}

int queueLength(Queue* q)
{
	int len = 0;
	queueNode* s = q->head;
	while (s != q->rear) {
		len++;
		s = s->next;
	}
	return len;
}

csTree* readEdgeCreate()
{
	csTree* T = (csTree*) malloc(sizeof(csTree));

	Queue* q = initQueue();

	char fa[MAX];
	char ch[MAX];
	cin >> fa >> ch;
	strcpy(T->data, ch);
	if (strcmp(fa, "#") == 0)
	T->firstChild = NULL;
	T->nextChild = NULL;
	enQueue(q, T);

	csTree* last = (csTree*) malloc(sizeof(csTree));

	while (strcmp(ch, "#") != 0) {

		cin >> fa >> ch;
		csTree* s = (csTree*) malloc(sizeof(csTree));
		strcpy(s->data, ch);
		s->firstChild = NULL;
		s->nextChild = NULL;

		enQueue(q, s);

		csTree* head = q->head->next->qData;
		while (strcmp(head->data, fa) != 0) {
			deQueue(q);
			head = q->head->next->qData;
		}

		if ( !(head->firstChild) ){
			head->firstChild = s;
			last = s;
		}
		else {
			last->nextChild = s;
			last = s;
		}
	}
	return T;
}

const char* dentPrint(csTree* T, int level)
{
	int i, j;
	if (T) {
		for ( i = 0; i < level; i++) {
			cout << " ";
		}
		cout << T->data << endl;
		dentPrint(T->firstChild, level + 8);
		dentPrint(T->nextChild, level);
		return "SUCCESS";
	}
	return "NULL";
}

int widthTree(csTree* T)
{
	int d1;
	int d2;
	if (!T)
		return 0;
	else {
		d1 = widthTree(T->firstChild);
		d2 = widthTree(T->nextChild);
		return d1 + 1 > d2 ? d1 + 1 : d2;
	}
}

void printLeaf(csTree* T)
{
	if (T->firstChild == NULL)
		cout << T->data << endl;
	if (T->firstChild)
		printLeaf(T->firstChild);
	if (T->nextChild)
		printLeaf(T->nextChild);
}

void searchTreeAndPrint(csTree* T, const char* find)
{
	if (!T)
		return;
	if (strcmp(T->data, find) == 0) {
		dentPrint(T->firstChild, 8);
		return;
	}
	searchTreeAndPrint(T->firstChild, find);
	searchTreeAndPrint(T->nextChild, find);
}

void insertNode(csTree* T, const char* newNode)
{
	csTree* s = (csTree*) malloc(sizeof(csTree));
	strcpy(s->data, newNode);
	s->firstChild = s->nextChild = NULL;

	if (T->firstChild == NULL)
		T->firstChild = s;
	else {
		csTree* p = T->firstChild;
		csTree* pNext = p->nextChild;
		while (pNext) {
			p = pNext;
			pNext = pNext->nextChild;
		}
		p->nextChild = s;
	}
	return;
}

void findInsertNode(csTree* T, const char* fa, const char* ch)
{
	if (!T)
		return;
	if (strcmp(T->data, fa) == 0) {
		return insertNode(T, ch);
	}
	else if (T->firstChild)
		findInsertNode(T->firstChild, fa, ch);
	findInsertNode(T->nextChild, fa, ch);
}

const char* delTree(csTree** T)
{
	if ((*T)->firstChild)
		delTree(&(*T)->firstChild);
	if (!((*T)->firstChild && (*T)->nextChild)) {
		free(*T);
		(*T) = NULL;
		return "SUCCESSFULLY DELETE!";
	}
	if ((*T)->nextChild)
		delTree(&(*T)->nextChild);
}

int main(int argc, char* argv[])
{
	csTree* T = readEdgeCreate();
	dentPrint(T, 8);
	cout << widthTree(T) << endl;
	printLeaf(T);
	searchTreeAndPrint(T, "控计");
	findInsertNode(T, "控计", "智能");
	dentPrint(T, 8);
	cout << delTree(&T) << endl;
	cout << dentPrint(T, 8) << endl;
	return 0;
}
```

```c++
#define _CRT_SECURE_NO_WARNINGS
#include<iostream>
#include<cstdio>
#include<cstdlib>
#include<cstring>
#include<cmath>
#include<windows.h>

using namespace std;

typedef struct treeNode
{
	int data;
	struct treeNode* leftChild;
	struct treeNode* rightChild;
}treeNode, * treeNodePtr;

typedef struct queueNode
{
	treeNode* qData;
	struct queueNode* next;
}queueNode, * queueNodePtr;

typedef struct Queue
{
	queueNode* head;
	queueNode* rear;
}Queue, * QueuePtr;

void preCreate(treeNode** T)
{
	int data;
	cin >> data;
	if (data == -1) {
		*T == NULL;
	}
	else {
		*T = (treeNode*) malloc(sizeof(treeNode));
		(*T)->leftChild = NULL;
		(*T)->rightChild = NULL;
		(*T)->data = data;
		preCreate(&(*T)->leftChild);
		preCreate(&(*T)->rightChild);
	}
}

void preOrder(treeNode* T)
{
	if (T == NULL)
		return;
	else {
		printf("%d ", T->data);
		//puts(T->data);
		preOrder(T->leftChild);
		preOrder(T->rightChild);
	}
}

void inOrder(treeNode* T)
{
	if (T == NULL)
		return;
	else {
		preOrder(T->leftChild);
		//puts(T->data);
		printf("%d ", T->data);
		preOrder(T->rightChild);
	}
}

void postOrder(treeNode* T)
{
	if (T == NULL)
		return;
	else {
		preOrder(T->leftChild);
		preOrder(T->rightChild);
		//puts(T->data);
		printf("%d ", T->data);
	}
}

const char* dentPrint(treeNode* T, int level, const char* flag)
{
	int i, j;
	if (T) {
		for (i = 1; i < level; i++) {
			printf(" ");
		}
		printf("%d(%s)", T->data, flag);

		cout << endl;
		dentPrint(T->leftChild, level + 4, "L");
		dentPrint(T->rightChild, level + 4, "R");
		return "PRINT SUCCESSFULLY!";
	}
	else return "THE TREE IS NULL!";
}

int sumNode(treeNode* T)
{
	if (T == NULL)
		return 0;
	else {
		int left = sumNode(T->leftChild);
		int right = sumNode(T->rightChild);
		return left + right + 1;
	}
}

int leaveCount(treeNode* T)
{
	if (!T)
		return 0;
	if (!(T->leftChild) && !(T->rightChild))
		return 1;
	else
		return leaveCount(T->leftChild) + leaveCount(T->rightChild);
}

int depthTree(treeNode* T)
{
	if (!T)
		return 0;
	else {
		int leftDepth = depthTree(T->leftChild);
		int rightDepth = depthTree(T->rightChild);
		if (leftDepth > rightDepth)
			return leftDepth + 1;
		else
			return rightDepth + 1;
	}
}

treeNode* searchTree(treeNode* T, int find)
{
	treeNode* left;
	treeNode* right;
	if (!T)
		return NULL;
	else {
		if (T->data == find)
			return T;
		left = searchTree(T->leftChild, find);
		right = searchTree(T->rightChild, find);
		return left ? left : right;
	}
}

Queue* initQueue()
{
	Queue* Q = (Queue*) malloc(sizeof(Queue));
	Q->head = Q->rear = (queueNode*) malloc(sizeof(queueNode));
	Q->head->next = NULL;
	return Q;
}

void enQueue(Queue* Q, treeNode* t)
{
	queueNode* s = (queueNode*) malloc(sizeof(queueNode));
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

treeNode* deQueue(Queue* Q)
{
	if (isEmpty(Q))
		return NULL;
	queueNode* s = Q->head->next;
	Q->head->next = s->next;

	if (Q->rear == s)
		Q->rear = Q->head;

	return s->qData;
}

int queueLength(Queue* Q)
{
	int len = 0;
	queueNode* p = Q->head;
	while (p != Q->rear) {
		len++;
		p = p->next;
	}
	return len;
}

void levelTraverse(treeNode* T)
{
	Queue* Q = initQueue();
	enQueue(Q, T);
	while (!isEmpty(Q)) {
		treeNode* de = deQueue(Q);
		printf("%d ", de->data);
		if (de->leftChild)
			enQueue(Q, de->leftChild);
		if (de->rightChild)
			enQueue(Q, de->rightChild);
	}
}

int widthTree(treeNode* T)
{
	Queue* Q = initQueue();
	int len = 0;
	int width = 0;
	enQueue(Q, T);
	while (!isEmpty(Q)) {
		len = queueLength(Q);
		if (len > width)
			width = len;
		for (int i = 0; i < len; i++) {
			treeNode* de = deQueue(Q);
			if (de->leftChild)
				enQueue(Q, de->leftChild);
			if (de->rightChild)
				enQueue(Q, de->rightChild);
		}
	}
	return width;
}

const char* delTree(treeNode** T)
{
	if ((*T)->leftChild) {
		delTree(&(*T)->leftChild);
	}
	if (!((*T)->leftChild && (*T)->leftChild)) {
		free(*T);
		*T = NULL;
		return "DELETE SUCCESSFULLY!";
	}
	if ((*T)->leftChild) {
		delTree(&(*T)->leftChild);
	}
}

int findMax(treeNode* T,int max)
{
	if (T) {
		max = max > T->data ? max: T->data;
		max = findMax(T->leftChild, max);
		max = findMax(T->rightChild, max);
	}
	return max;
}

int findMin(treeNode* T, int min)
{
	if (T) {
		min = min < T->data ? min : T->data;
		min = findMin(T->leftChild, min);
		min = findMin(T->rightChild, min);
	}
	return min;
}

typedef struct stackNode
{
	treeNode* data;
	struct stackNode* next;
}stackNode, * stackNodePtr;

typedef struct stack
{
	stackNode* head;
	stackNode* rear;
}stack, * stackPtr;

stack* initStack()
{
	stack* S = (stack*) malloc(sizeof(stack));
	S->head = (stackNode*)malloc(sizeof(stackNode));
	S->head->next = NULL;
	return S;
}

void push(stack* S, treeNode* e)
{
	stackNode* s = (stackNode*) malloc(sizeof(stackNode));
	s->data = e;
	s->next = NULL;
	s->next = S->head->next;
	S->head->next = s;
	return;
}

int isEmpty_stack(stack* S) 
{
	if (S->head->next == NULL)
		return 1;
	else return 0;
}

void pop(stack* S)
{
	if (isEmpty_stack(S))
		return;
	stackNode* s = S->head->next;
	S->head->next = s->next;

	free(s);
}

void printStack(stack* S)
{
	stackNode* n = S->head->next;
	while (n->next) {
		cout << n->data->data << " <- ";
		n = n->next;
	}
	cout << n->data->data << endl;
}

void pathToLeaf(treeNode* T, stack* S)
{
	if (T) {
		push(S, T);
		if ( (T->leftChild == NULL) && (T->rightChild == NULL) ) {
			printStack(S);
		}
		else {
			pathToLeaf(T->leftChild, S);
			pathToLeaf(T->rightChild, S);
		}
		pop(S);
	}
}

void printPathToLeaf(treeNode* T)
{
	stack* S = initStack();
	pathToLeaf(T, S);
}

void path_StartToEnd(treeNode* T, treeNode* end, stack* S)
{
	if (T) {
		push(S, T);
		if (T == end) {
			printStack(S);
		}
		else {
			path_StartToEnd(T->leftChild, end, S);
			path_StartToEnd(T->rightChild, end, S);
		}
		pop(S);
	}
}

void printPath_StartToEnd(treeNode* T)
{
	cout << "请依次输入起点和终点：" << endl;
	int s;	cin >> s;
	treeNode* start = searchTree(T, s);
	int e;	cin >> e;
	treeNode* end = searchTree(T, e);

	stack* S = initStack();
	path_StartToEnd(start, end, S);
}

void printFind(treeNode* T)
{
	int find;
	treeNode* p;
	cout << "请输入查找值：" << endl;
	cin >> find;
	if (p = searchTree(T, find)) {
		cout << "SUCCESS!" << endl;
		cout << "以该值为根的二叉树为：" << endl;
		dentPrint(p, 4, "root");
	}
	else cout << "NULL！" << endl;
}

void findOdd(treeNode* T)
{
	if (T) {
		if (T->data % 2 == 1)
			cout << T->data << " ";
		findOdd(T->leftChild);
		findOdd(T->rightChild);
	}
}

void findEven(treeNode* T)
{
	if (T) {
		if (T->data % 2 == 0)
			cout << T->data << " ";
		findEven(T->leftChild);
		findEven(T->rightChild);
	}
}

void AllPath(treeNode* T, int path[], int index)
{
	int i;
	if (T) {
		path[index] = T->data;
		if ((T->leftChild == NULL) && (T->rightChild == NULL)) {
			for (i = 0; i < index; i++) {
				cout << path[i] << "->";
			}
			cout << path[index] << endl;
		}
		else {
			AllPath(T->leftChild, path, index + 1);
			AllPath(T->rightChild, path, index + 1);
		}
	}
}

void printAllPath(treeNode* T)
{
	int* path = (int*) malloc(sizeof(int) * depthTree(T));
	AllPath(T, path, 0);
}


void path_StartToEnd2(treeNode* T, treeNode* end, int path[], int index)
{
	int i;
	if (T) {
		path[index] = T->data;
		if (T == end) {
			for (i = 0; i < index; i++) {
				cout << path[i] << "->";
			}
			cout << path[index] << endl;
		}
		else {
			path_StartToEnd2(T->leftChild, end, path, index + 1);
			path_StartToEnd2(T->rightChild, end, path, index + 1);
		}
	}
}

void printPath_StartToEnd2(treeNode* T)
{
	cout << "请依次输入起点和终点：" << endl;
	int s;	cin >> s;
	treeNode* start = searchTree(T, s);
	int e;	cin >> e;
	treeNode* end = searchTree(T, e);

	int* path = (int*) malloc(sizeof(int) * depthTree(start));
	path_StartToEnd2(start, end, path, 0);
}

void swapSubtree(treeNode** T)
{
	cout << "请输入父节点值" << endl;
	int fv;		cin >> fv;
	treeNode* index =  searchTree(*T, fv);
	treeNode* temp = index->leftChild;
	index->leftChild = index->rightChild;
	index->rightChild = temp;
	cout << "交换后二叉树为" << endl;
	dentPrint(*T, 4, "root");
}

int main(int argc, char* argv[])
{
	treeNode* T;
	preCreate(&T);
	dentPrint(T, 0, "root");
	cout << "前序遍历：";	preOrder(T);		cout << endl;
	cout << "中序遍历：";	inOrder(T);			cout << endl;
	cout << "后序遍历：";	postOrder(T);		cout << endl;
	cout << "层次遍历：";	levelTraverse(T);	cout << endl;
	cout << "叶子节点数为：" << leaveCount(T) << endl;
	cout << "总节点数为：" << sumNode(T) << endl;
	cout << "二叉树的深度为：" << depthTree(T) << endl;
	cout << "二叉树的宽度为：" << widthTree(T) << endl;
	//cout << searchTree(T, 91);
	cout << "二叉树上的结点最大值为：" << findMax(T, T->data) << endl;
	cout << "二叉树上的结点最小值为：" << findMin(T, T->data) << endl;
	cout << "二叉树上所有奇数结点为："; findOdd(T); cout << endl;
	cout << "二叉树上所有偶数结点为："; findEven(T); cout << endl;
	printFind(T);		//查找给定值
	printPathToLeaf(T);		//输出根到子树的所有路径
	printPath_StartToEnd(T);	//输出一颗子树上任意两点间的路径

	printAllPath(T);	//输出根到子树的所有路径(算法二)
	printPath_StartToEnd2(T);	//输出一颗子树上任意两点间的路径(算法二)

	swapSubtree(&T);

	cout << delTree(&T);	//删除整棵树
	cout << dentPrint(T, 0, "root");

	return 0;
}
```
