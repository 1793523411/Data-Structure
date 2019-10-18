---
title: 括号匹配
date: 2019-10-15 20:07:46
tags:
- 数据结构
- 栈
---
## 括号匹配

```c++
#include<iostream>
using namespace std;

#define OK 1
#define ERROR 0
#define OVERFLOW -2
typedef int Status;
typedef struct SNode{
	int data;
	struct SNode *next;
}SNode,*LinkStack;
Status InitStack(LinkStack &S)
{
	S = NULL;
	return OK;
}
bool StackEmpty(LinkStack S)
{
	if(!S)
		return true;
	return false;
}

Status Push(LinkStack &S,char e)
{
	SNode *p = new SNode;
	if(!p)
	{
		return OVERFLOW;
	}
	p->data = e;
	p->next = S;
	S = p;
	return OK;
}
Status Pop(LinkStack &S,char &e)
{
	SNode *p;
	if(!S)
		return ERROR;
	e = S->data;
	p = S;
	S = S->next;
	delete p;
	return OK;
}

Status GetTop(LinkStack S){
	if(S!=NULL)
		return S->data;
}

Status Matching(){
	SNode *S;  
	int flag;
	InitStack(S);
	flag=1;
	char ch;
	char x;
	cin>>ch;
	while(ch!='#'&&flag){
		switch(ch){
			case '[':
			case '(':
				Push(S,ch);
				break;
			case ')':
				if(!StackEmpty(S)&&GetTop(S)=='(')
					Pop(S,x);
				else flag=0;
				break;
			case ']':
				if(!StackEmpty(S)&&GetTop(S)=='[')
					Pop(S,x);
				else flag=0;
				break;
		}
		cin>>ch;
	}
	if(StackEmpty(S)&&flag) { cout<<"匹配成功 "; return true;}
	else {cout<<"匹配失败"; return false;}
} 

int main(){
	Matching();
	cout<<Matching;
	return 0;
}
```