# 栈与队列
<!-- TOC -->

- [栈与队列](#栈与队列)
	- [1. 栈(stack)Last In First Out](#1-栈stacklast-in-first-out)
		- [1.1. 栈的顺序存储](#11-栈的顺序存储)
			- [1.1.1. 结构代码](#111-结构代码)
			- [1.1.2. 初始化操作 InitSqStack(SqStack *S, int max)](#112-初始化操作-initsqstacksqstack-s-int-max)
			- [1.1.3. 判断栈空 int IsStackEmpty(SqStack S)](#113-判断栈空-int-isstackemptysqstack-s)
			- [1.1.4. 获取栈顶元素 int GetStackTop(SqStack S, int *e)](#114-获取栈顶元素-int-getstacktopsqstack-s-int-e)
			- [1.1.5. 求长度 int SqStackLength(SqStack S)](#115-求长度-int-sqstacklengthsqstack-s)
			- [1.1.6. 进栈操作 int PushSqStack(SqStack *S, int e)](#116-进栈操作-int-pushsqstacksqstack-s-int-e)
			- [1.1.7. 出栈操作 int PopSqStack(SqStack *S, int *e)](#117-出栈操作-int-popsqstacksqstack-s-int-e)
			- [1.1.8. 遍历操作 int TraverSqStack(Stack S)](#118-遍历操作-int-traversqstackstack-s)
			- [1.1.9. 创建顺序栈 int CreateSqStack(SqStack *S, int max)](#119-创建顺序栈-int-createsqstacksqstack-s-int-max)
		- [1.2. 栈的链式存取](#12-栈的链式存取)
			- [1.2.1. 结构代码](#121-结构代码)
			- [1.2.2. 初始化 int InitLinkStack(LinkStack *S)](#122-初始化-int-initlinkstacklinkstack-s)
			- [1.2.3. 判断栈空 int IsLinkStackEmpty(LinkStack S)](#123-判断栈空-int-islinkstackemptylinkstack-s)
			- [1.2.4. 获取栈顶元素 int GetLinkStackTop(LinkStack S, int *e)](#124-获取栈顶元素-int-getlinkstacktoplinkstack-s-int-e)
			- [1.2.5. 求长度 int LinkStackLength(LinkStack S)](#125-求长度-int-linkstacklengthlinkstack-s)
			- [1.2.6. 进栈操作 int PushLinkStack(LinkStack *S)](#126-进栈操作-int-pushlinkstacklinkstack-s)
			- [1.2.7. 出栈操作 int PopLinkStack(LinkStack *S， int *e)](#127-出栈操作-int-poplinkstacklinkstack-s-int-e)
			- [1.2.8. 遍历操作 int TraversLinkStack(LinkStack S)](#128-遍历操作-int-traverslinkstacklinkstack-s)
			- [1.2.9. 创建链栈 int CreateLinkStack(LinkStack S)](#129-创建链栈-int-createlinkstacklinkstack-s)
	- [2. 队列(Queue)First In First Out](#2-队列queuefirst-in-first-out)
		- [2.1. 链队列](#21-链队列)
			- [2.1.1. 链队列的结构代码](#211-链队列的结构代码)
			- [2.1.2. 链队列的初始化 int InitQueue(LinkQueue *Q)](#212-链队列的初始化-int-initqueuelinkqueue-q)
			- [2.1.3. 链队列入队操作 int EnQueue(LinkQueue *Q)](#213-链队列入队操作-int-enqueuelinkqueue-q)
			- [2.1.4. 链队列出队 int DeQueue(LinkQueue *Q, QElemType &e)](#214-链队列出队-int-dequeuelinkqueue-q-qelemtype-e)
			- [2.1.5. 获取链队列对头元素 int GetQueueHead(LinkQueue Q, QElemType &e)](#215-获取链队列对头元素-int-getqueueheadlinkqueue-q-qelemtype-e)
			- [2.1.6. 链队列的销毁 int DestroyQueue(LinkQueue *Q)](#216-链队列的销毁-int-destroyqueuelinkqueue-q)
		- [2.2. 顺序队列/循环队列](#22-顺序队列循环队列)
			- [2.2.1. 结构代码](#221-结构代码)
			- [2.2.2. 初始化 int InitQueue(SqQueue *Q)](#222-初始化-int-initqueuesqqueue-q)
			- [2.2.3. 求长度 int QueueLength(SqQueue Q)](#223-求长度-int-queuelengthsqqueue-q)
			- [2.2.4. 入队 int EnQueue(SqQueue *Q, QElemType e)](#224-入队-int-enqueuesqqueue-q-qelemtype-e)
			- [2.2.5. 出队 int DeQueue(SqQueue *Q, QElemType *e)](#225-出队-int-dequeuesqqueue-q-qelemtype-e)
			- [2.2.6. 取对头元素 SElemType GetHead(SqQueue Q)](#226-取对头元素-selemtype-getheadsqqueue-q)

<!-- /TOC -->
## 1. 栈(stack)Last In First Out

$$
栈\begin{cases}
    栈是限定仅在表尾进行插入和删除操作的线性表。\\
    栈顶：予许插入删除一段\\
    栈底：另一端\\
    LIFO结构\\
\end{cases}
$$

### 1.1. 栈的顺序存储

#### 1.1.1. 结构代码

<center>Method 1</center>

```c++
#define MAX 100
typedef struct stack
{
	SElemType data[MAX];
	int top;				//栈顶位置
	int StackSize;			//栈容量
}SqStack;
```

<center>Method 2</center>

```c++
typedef struct stack
{
	SElemType *data;		//指针
	int top;				//栈顶位置
	int StackSize;			//栈容量
}SqStack;
```

#### 1.1.2. 初始化操作 InitSqStack(SqStack *S, int max)

```c++
InitSqStack(SqStack *S, int max)
{
	S->data = (SElemType*)malloc(SElemType);
	if(S->data == NULL){
		printf("空间申请失败！");
		exit(0);
	}
	S->top = -1;
	S->StackSize = max;
	return 1;
}
```

#### 1.1.3. 判断栈空 int IsStackEmpty(SqStack S)

```c++
int IsSqStackEmpty(SqStack S)
{
	if(S.top == -1)
		return 1;
	else 
		return 0;
}
```

#### 1.1.4. 获取栈顶元素 int GetStackTop(SqStack S, int *e)

```c++
int GetSqStackTop(SqStack S, int *e)
{
	if(S.top == -1)
		return 0;
	*e = S.data[S.top];
		return 1;
}
```

#### 1.1.5. 求长度 int SqStackLength(SqStack S)

```c++
int StackLength(SqStack S)
{
	if(S.top == -1)
		return 0;
	return S.top+1;
}
```

#### 1.1.6. 进栈操作 int PushSqStack(SqStack *S, int e)

```c++
int PushSqStack(SqStack *S, int e)
{
	if(S->StackSize == S->top+1)
		return 0;
	S->data[++S->top] = e;
	return 1;
}
```

#### 1.1.7. 出栈操作 int PopSqStack(SqStack *S, int *e)

```c++
int PopSqStack(SqStack *S, int *e)
{
	if(S->top == -1)
		return 0;
	*e = S->data[S->top--];
		return 1;
}
```

#### 1.1.8. 遍历操作 int TraverSqStack(Stack S)

```c++
int TraverSqStack(Stack S)
{
	int k;
	if(s.top == -1){
		printf("栈空！");
		return 0;
	}
	for(k = S.top; k >= 0; k--)
		printf("%-5d",S.data.data[k]);
	printf("/n");
}
```

#### 1.1.9. 创建顺序栈 int CreateSqStack(SqStack *S, int max)

```c++
int CreateSqStack(SqStack *S, int max)
{
	int x, YN;
	InitSqStack(S, max);
	do{
		printf("请输入进栈数据：");
			scanf("%d", &x);
		printf("继续吗？yes = 1;no = 0");
			scanf("%d", &YN);
	}while(YN == 1);
}
```

### 1.2. 栈的链式存取

链栈**不需要头节点！**

#### 1.2.1. 结构代码

```c++
typedef struct StackNode
{
	SElemType data;
	struct StackNode *next;
}SNode,*LinkStack;
```

#### 1.2.2. 初始化 int InitLinkStack(LinkStack *S)

```c++
int InitLinkStack(LinkStack *S)
{
	*S = NULL;					//只有一句，一般不用单独写函数
}
```

#### 1.2.3. 判断栈空 int IsLinkStackEmpty(LinkStack S)

```c++
int IsLinkStackEmpty(LinkStack S)
{
	if(S == NULL)
		return 1;
	else
		return 0;
}
```

#### 1.2.4. 获取栈顶元素 int GetLinkStackTop(LinkStack S, int *e)

```c++
int GetLinkStackTop(LinkStack S, int *e)
{
	if(S == NULL)
		return 0;
	*e = S->data;
		return 1
}
```

#### 1.2.5. 求长度 int LinkStackLength(LinkStack S)

```c++
int LinkStackLength(LinkStack S)
{
	int j = 0;
	LinkStack p = S;
	while (p) {
		p = p->next;
		j++;
	}
	return j;
}
```

#### 1.2.6. 进栈操作 int PushLinkStack(LinkStack *S)

其实就是链表的头插法。

```c++
int PushLinkStack(LinkStack *S)
{
	LinkStack p = (LinkStack)malloc(SNode);
	if (p == NULL)
		return 0;
	p->data = e;
	p->next = *S;
	*S = p;
	return 1;
}
```

#### 1.2.7. 出栈操作 int PopLinkStack(LinkStack *S， int *e)

```c++
int PopLinkStack(LinkStack *S, int *e)
{
	LinkStack p = *S;
	if(p == NULL)
		return 0;
	*S = p->next;
	*e = p->data;
	free(p);
	return 1;
}
```

#### 1.2.8. 遍历操作 int TraversLinkStack(LinkStack S)

```c++
int TraversLinkStack(LinkStack S)
{
	if(S == NULL){
		printf("栈空！");
		return 0;
	}
	while(S){
		printf("%-5d",S->data);
		S = S->next;
	}
	printf("\n");
	return 1;
}
```

#### 1.2.9. 创建链栈 int CreateLinkStack(LinkStack S)

```c++
int CreateLinkStack(LinkStack S)
{
	int x,YN;
	InitLinkStack(&S);
	do{
		printf("请输入链栈数据：");
		scanf("%d",&x);
		PushLinkStack(S,x);
		printf("继续吗？YES = 1; NO = 0.");
		scanf("%d",&YN);
	}while(YN == 1);
	return 1;
}
```

## 2. 队列(Queue)First In First Out

$$
队列
\begin{cases}
队头删除\\
队尾插入\\
FIFO结构\\
\end{cases}
$$

### 2.1. 链队列

#### 2.1.1. 链队列的结构代码

```c++
//链队列的结点及指针类型
typedef struct QNode
{
	QElemType data;
	struct QNode *next;
}QNode, *QueuePtr;

//链队列类型
typedef struct
{
	QueuePtr front;
	QueuePtr rear;
}LinkQueue;
```

#### 2.1.2. 链队列的初始化 int InitQueue(LinkQueue *Q)

```c++
int InitQueue(LinkQueue* Q)
{
	Q->front = Q->rear = (QueuePtr)malloc(sizeof(QNode));
	if (!Q->front)
		exit(0);
	Q->front->next = NULL;
	return 1;
}
```

#### 2.1.3. 链队列入队操作 int EnQueue(LinkQueue *Q)

```c++
int EnQueue(LinkQueue *Q, QElemType e)
{
    QueuePtr p = (QueuePtr)malloc(sizeof(QNode));
    if (!p)
        exit(0);
    p->data = e;
    p->next = NULL;
    Q->rear->next = p;
    Q->rear = p;
    return 1;
}
```

#### 2.1.4. 链队列出队 int DeQueue(LinkQueue *Q, QElemType &e)

```c++
int DeQueue(LinkQueue* Q)
{
    if (Q->front == Q->rear)
        return 0;
    QueuePtr p = Q->front->next;
    Q->front->next = p->next;

    if (Q->rear == p)
        Q->rear = Q->front;

    free(p);
    return 1;
}
```

#### 2.1.5. 获取链队列对头元素 int GetQueueHead(LinkQueue Q, QElemType &e)

```c++
int GetQueueHead(LinkQueue Q, QElemType &e)
{
	if(Q.front == Q.rear)
		return 0;
	e = Q.front->next->data;
	return 1;
}
```

#### 2.1.6. 链队列的销毁 int DestroyQueue(LinkQueue *Q)

```c++
int DestroyQueue(LinkQueue *Q)
{
	while(Q->front){
		p = Q->front->next;
		free(Q->front);
		Q.front = p;
	}
	return 1;
}
```

### 2.2. 顺序队列/循环队列

#### 2.2.1. 结构代码

```c++
#define MAXQSIZE 100
Typedef struct{
	QElemTyoe *base;
	int front;			//头指针
	int rear;			//尾指针
}SqQueue;
```

#### 2.2.2. 初始化 int InitQueue(SqQueue *Q)

```c++
int InitQueue(SqQueue *Q)
{
	Q.base = new QElemType[MAXSIZE];
	if(!Q.base)
		exit(0);
	Q.front = Q.rear = 0;
	return 1;
}
```

#### 2.2.3. 求长度 int QueueLength(SqQueue Q)

```c++
int QueueLength(SqQueue Q)
{
	return (Q.rear-Q.front+MAXQSIZE)%MAXQSIZE;
}
```

#### 2.2.4. 入队 int EnQueue(SqQueue *Q, QElemType e)

```c++
int EnQueue(SqQueue *Q, QElemType e)
{
	if((Q->rear+1)%MAXQSIZE == Q->front)
		return 0;
	Q->data[Q->rear] = e;
	Q->rear = (Q->rear + 1)%MAXQSIZE;
	return 1;
}
```

#### 2.2.5. 出队 int DeQueue(SqQueue *Q, QElemType *e)

```c++
int DeQueue(SqQueue *Q, QElemType *e)
{
	if(Q->front  == Q->rear)
		return 0;
	*e = Q->data[Q->front];
	Q->front = (Q->front + 1)%MAXQSIZE;
	return 1;
}
```

#### 2.2.6. 取对头元素 SElemType GetHead(SqQueue Q)

```c++
SElemType GetHead(SqQueue Q)
{
	if(Q->front+1 != Q->rear)
		return Q.data[Q.front];
}
```
