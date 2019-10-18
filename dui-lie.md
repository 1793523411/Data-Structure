## 对列
### 队列的顺序结构存储
下标移动
<!-- more -->
```c++
#define MAXQSIZE 100
typedef struct{
    QElemType *base;
    int front;
    int rear;
}SqQueue
```
### 队列的初始化
```c++
Status InitQueue(SqQueue &Q){
    Q.base=new QElemType[MAXQSIZE];
    if(!Q.base) exit(OVERFLOW);
    Q.front=Q.rear=0;
    return OK;
}
```

### 求队列的长度
```c++
int QueueLength(SqQueue Q){
    return(Q.rear-Q.front+MAXQSIZE)%MAXQSIZE;
    //考虑到循环对列中队头队尾的两种情况
}
```
### 顺序对列入队
```c++
Status EnQueue(SqQueue &Q,QElemType e){
    if((Q.rear+1)%MAXQSIZE==Q.front))
    return ERROR;
    Q.base[Q.rear]=e;
    Q.rear=(Q.rear+1)%MAXQSIZE;
    return OK;
}
```
### 出队
```c++
Status DeQueue (SqQueue &Q,QElemType &e){
    if(Q.front==Q.rear){
        return ERROR;
    }
    e=Q.base[Q.front];
    Q.front=(Q.front+1)%MAXQSIZE;
    return OK;
}
```
### 去队头元素
```c++
SElemType GetHead(SqQueue Q){
    if(Q.front!=Q.rear){//队列非空
        return Q.base[Q.front];
    }
}
```
## 链队
###　链队的结构
```c++
typedef struct QNode{
    QElemType data;
    struct QNode *next;
}QNode,*QueuePtr
stypedef struct{
    QueuePtr front;
    QueuePtr rear;
}LinkQueue;
```
### 初始化
```c++
Status InitQueue(LinkQueue &Q){
    Q.front=Q.rear=new QNode;
    Q.front->next=NULL;
    return OK;
}
```
### 入队
```c++
Status EnQueue(LinkQueue &Q,QElemType e){
    p=new QNode;
    p->data=e;
    p->next=NULL;
    Q.rear->next=p;
    Q.rear=p;
    return OK;
}
```
### 出队
**需考虑最后元素被删后队尾指针会不会丢失**
```c++
Status DeQueue(LinkQueue &Q,QElemType &e){
    if(Q.front==Q.rear) return ERROR;
    p=Q.front->next;
    e=p->data;
    Q.front->next=p->next;
    if(Q.rear==p) Q.rear=Q.front;
    delete p;
    return OK;
}
```
### 取队头元素
```c++
SElemType GetHead(LinkQueue Q){
    if(Q.front!=Q.rear){
        return Q.front->next->data;
    }
}
```

### 循环对列
解决假溢出,用于循环对列：`Q.rear=(Q.rear+1)%MAXQSIZE;`
判断队满与队空：
#### 方法一：少用一个元素空间
队空的条件：`Q.front==Q.rear`
队满的条件：`(Q.rear+1)%MAXQSIZE==Q.front`
#### 方法二：设置一个标志
数组Q[]存放元素，设置标志tag==0，tag==1来区别
**初始化**
```c++
SeQueue QueueInit(SeQueue Q){
    Q.front=Q.rear=0;
    Q.tag=0;
    return Q;
}
```
**入队**
```c++
SeQueue QueueIn(SeQueue Q,int e){
    if((Q.tag==1)&&(Q.rear==Q.front)) cout<<"队已满"<<endl;
    else{
        Q.rear=(Q.rear+1)%m;
        Q.data[Q.rear]=e;
        if(Q.tag==0) Q.tag=1;//对列已不空
    }
    return Q;
}
```
**出队**
```c++
ElemType QueueOut(SeQueue Q){
    if(Q.tag=0){
        cout<<"对为空"<<endl;
        exit(0);
    }else{
        Q.front=(Q.front+1)%m;
        e=Q.data[Q.front];
        if(Q.rear==Q.front){//队为空
            Q.tag=0;
        }
    }
    return(e);
}
```
