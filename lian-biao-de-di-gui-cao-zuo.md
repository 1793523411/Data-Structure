---
title: 链表的递归
date: 2019-10-15 20:07:46
tags:
- 数据结构
- 线性表
---
采用分治法进行递归算法一般形式为：
```c++
void p(参数列表){
    if(递归结束条件成立) 可直接求解//递归终止的条件
    else p(较小的参数)//递归步骤
}
```
## 遍历输出各个节点
```c++
void TraverseList(LinkList p){
    if(p==NULL) return;
    else{
        cout<<p-data<<endl;
        TRaverseList(p->next);
    }
}
```
可化简为：
```c++
void TraverseList(LinkList p){
    if(p)
    {
        cout<<p-data<<endl;
        TraverseList(p->next);
    }
}
```
## Hanoi塔问题
```c++
//对搬动进行计数
int m=0;
void move(char A,int n,char C){
    cout<<++m<<","<<n<<","<<"A"<<","<<C<<endl;
}
//递归算法
void Hanoi(int n,char A,char B,char C){
    if(n==1) move(A,1,C);
    else{
        Hanoi(n-1,A,C,B);
        move(A,n,C);
        Hanoi(n-1,B,A,C);
    }
}
```
## 求最大值
```c++
int Max(LinkList L){
	if(!L->next){
		return L->data;
	}else{
		int max=Max(L->next);
		return L->data>=max?L->data:max;
	}
}
```

## 求节点个数
```c++
int GetLength(LinkList p){
    if(!p->next)
    return 1;
    else{
        return Getlength(p->next)+1;
    }
}

```
## 求平均值
```c++
double GetAverage(LinkList p,int n){
    if(!p->next){
        return p->data;
    }else{
        double ave=GetAverage(p->next,n-1)；
        return (ave*(n-1)+p->next/n);
    }
}

```