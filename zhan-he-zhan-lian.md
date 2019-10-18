---
title: 栈和链栈
date: 2019-10-15 20:07:46
tags:
- 数据结构
- 栈
---
```c++
#define OK 1
#define ERROR 0
#define OVERFLOW -2
typedef int Status;		//Status 是函数返回值类型，其值是函数结果状态代码。
typedef int ElemType;	 //ElemType 为可定义的数据类型，此设为int类型
```
## 顺序栈
### 顺序栈的存储结构
```c++
#define MAXSIZE 100
typedef struct{
    SElemType *base;
    SElemType *top;
    int stacksize;//栈的最大容量
}SqStack;
```
### 顺序栈的初始化
```c++
Status InitStack(SqStack &S){
    //构造一个空栈
    S.base=new SElemType[MAXSIZE];
    if(!S.base) exit(OVERFLOW);
    S.top=S.base;
    S.stacksize=MAXSIZE;
    return OK;
}
```
### 入栈
```c++
Status Push(SqStack &S,SElemType e){
    if(S.top-S.base===S.stacksize) return ERROR;
    *S.top++=e;//先赋制再加
    return OK;
}
```
### 出栈
```c++
Status Pop(Sqstack &S,SElemType &e){
    if(S.top==S.base) return ERROR;
    e=*--S.top;//先减后赋值
    return OK;
}
```
### 取栈顶元素
```c++
SElemType GetTop(SqStack S){
    if(S.top!=S.base){
        return *(S.top-1);
    }
}
```
## 链栈
### 结构
```c++
typedef struct StackNode{
    ELemType data;
    struct StackNode *next;
}StackNode,*LinkStack;
```
### 初始化
```c++
Status InitStack(LinkList &S){
    //构造一个空栈，栈顶指针置空
    S=NULL;
    return OK;
}
```
### 入栈
```c++
Status Push(LinkListStack &S,SElemType e){
    p=new StackNode;
    p->data=e;//将新节点数据域置为e
    p->next=S;//将新节点插入栈顶
    S=p;//修改指针指向栈顶
    return OK;
}
```
### 出栈
```c++
Status Pop(LinkStack &S,SElemType &e){
    if(S==Null) return ERROR;
    e=S->next;
    p=S;//用p临时保存栈顶元素，以备释放
    S=S->next;
    delete p;
    return OK;
}
```
### 取栈顶元素
```c++
SElemType GetTop(LinkList S){
    if(S!=NULL){
        return S->data;
    }
}