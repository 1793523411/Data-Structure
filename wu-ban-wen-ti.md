---
title: 队列
date: 2019-10-19 15:20:46
tags:
- 数据结构
- 队列
---
## 舞伴问题（读文件与输入）
运用到队列的知识，包括初始化，入队，出队，取取顶元素，结构体数组；
### 读文件版
读文件函数的运用，将文件里的内容一条条的读入到结构体数组中，队列先进先出
**读文件**
```c++
    #include<fstream>
    #include<string>

    fstream file;
	file.open("DancePartner.txt");
	if (!file) {
		cout << "错误！未找到文件！\n\n" << endl;
		exit( ERROR);
	}
	while (!file.eof()) {
		file >> dancer[i].name >> dancer[i].sex;
		i++;
	}
```
**完整代码**
```c++
#include<iostream>
#include<fstream>
#include<string>
#define MAXQSIZE 100//队列可能达到的最大长度
#define OK 1
#define ERROR 0
#define OVERFLOW -2
using namespace std;

typedef struct {
	char name[20]; //姓名
	char sex; //性别，'F'表示女性，'M'表示男性
} Person;

//- - - - - 队列的顺序存储结构- - - - - 
typedef struct {
	Person *base; //队列中数据元素类型为Person
	int front; //头指针
	int rear; //尾指针
} SqQueue;

SqQueue Mdancers, Fdancers; //分别存放男士和女士入队者队列

int InitQueue(SqQueue &Q) {//构造一个空队列Q
	Q.base = new Person[MAXQSIZE]; //为队列分配一个最大容量为MAXSIZE的数组空间
	if (!Q.base)
		exit( OVERFLOW); //存储分配失败
	Q.front = Q.rear = 0; //头指针和尾指针置为零，队列为空
	return OK;
}

int EnQueue(SqQueue &Q, Person e) {//插入元素e为Q的新的队尾元素
	if ((Q.rear + 1) % MAXQSIZE == Q.front) //尾指针在循环意义上加1后等于头指针，表明队满
		return ERROR;
	Q.base[Q.rear] = e; //新元素插入队尾
	Q.rear = (Q.rear + 1) % MAXQSIZE; //队尾指针加1
	return OK;
}

int QueueEmpty(SqQueue &Q) {
	if (Q.front == Q.rear) //队空
		return OK;
	else
		return ERROR;
}

int DeQueue(SqQueue &Q, Person &e)//删除Q的队头元素，用e返回其值
{
	if (Q.front == Q.rear)
		return ERROR; //队空
	e = Q.base[Q.front]; //保存队头元素
	Q.front = (Q.front + 1) % MAXQSIZE; //队头指针加1
	return OK;
}

Person GetHead(SqQueue Q) {//返回Q的队头元素，不修改队头指针
	if (Q.front != Q.rear) //队列非空
		return Q.base[Q.front]; //返回队头元素的值，队头指针不变
}

//算法3.23　舞伴问题
void DancePartner(Person dancer[], int num) {//结构数组dancer中存放跳舞的男女，num是跳舞的人数。
	InitQueue(Mdancers); //男士队列初始化
	InitQueue(Fdancers); //女士队列初始化
	Person p;
	for (int i = 0; i < num; i++) //依次将跳舞者根据其性别入队
	{
		p = dancer[i];
		if (p.sex == 'F')
			EnQueue(Fdancers, p); //插入女队
		else
			EnQueue(Mdancers, p); //插入男队
	}
	cout << "The dancing partners are:" << endl;
	while (!QueueEmpty(Fdancers) && !QueueEmpty(Mdancers)) {//依次输出男女舞伴的姓名
		DeQueue(Fdancers, p); //女士出队
		cout << p.name << "  "; //输出出队女士姓名
		DeQueue(Mdancers, p); //男士出队
		cout << p.name << endl; //输出出队男士姓名
	}
	if (!QueueEmpty(Fdancers)) { //女士队列非空，输出队头女士的姓名
		p = GetHead(Fdancers); //取女士队头
		cout << "The first man to get a partner is: " << endl;
		cout << p.name << endl;
	} else if (!QueueEmpty(Mdancers)) { //男士队列非空，输出队头男士的姓名
		p = GetHead(Mdancers); //取男士队头
		cout << "The first woman to get a partner is: " << p.name << endl;
	}
}

int main(){
	Person dancer[MAXQSIZE];
	int i = 0;
	fstream file;
	file.open("DancePartner.txt");
	if (!file) {
		cout << "错误！未找到文件！\n\n" << endl;
		exit( ERROR);
	}
	while (!file.eof()) {
		file >> dancer[i].name >> dancer[i].sex;
		i++;
	}
	DancePartner(dancer, i + 1);
	return 0;
}
```
### 输入人数版
调用的函数和前面的一样，只是主函数不一样，以不同的方式传入数据，
前者是从文件中读取，而后者是自己输入获取，
其运算结果都一样；
```c++
#include<iostream>
using namespace std;
#define MAXQSIZE 100
#define QueueSize 20
#define OK 1
#define ERROR 0
#define OVERFLOW 0
 
typedef char QElemType;
typedef int Status;
//typedef char SElemType;

typedef struct
{
    char name[QueueSize];
    char sex;
}person;

 
typedef struct
{
    person *dancer;
    person *base;       //存储空间的基地址
    int front;             //头指针
    int rear;              //尾指针 
}SqQueue; 

Status InitQueue(SqQueue &Q)
{//构造一个空队列Q
    Q.base=new person[MAXQSIZE];    //为队列分配一个最大容量为MAXQSIZE的数组空间
    if(!Q.base) exit(OVERFLOW);        //存储分配失败
    Q.front=Q.rear=0;                  //头指针和尾指针为零，队列为空
    return OK;     
 }

 
 Status EnQueue(SqQueue &Q,person e)
 {//插入元素e为Q的新的队尾元素
     if((Q.rear+1)%MAXQSIZE==Q.front)   //尾指针在循环意义上加1后等于头指针，表明队满
        return ERROR;
    Q.base[Q.rear]=e;                //新元素插入队尾
    Q.rear=(Q.rear+1)%MAXQSIZE;       //队尾指针加1
    return OK; 
 }
 
 int QueueEmpty(SqQueue &Q)
{
    if (Q.front==Q.rear)   return OK;
    else return ERROR; 
}
 
 Status DeQueue(SqQueue &Q,person &e)
 {//删除Q的队头元素，用e返回其值
     if(Q.front==Q.rear) return ERROR;   //队空
    e=Q.base[Q.front];                  //保存队头元素
    Q.front=(Q.front+1)%MAXQSIZE;       //队头指针加1 
    return OK; 
     
 }

person GetHead(SqQueue Q)
{//返回Q的队列元素，不修改队头指针
    if(Q.front!=Q.rear)              //队列非空
        return Q.base[Q.front];      //返回队头元素的值，队头指针不变 
 } 

void DancePartner(person dancer[],int num)
{//结构数组dancer中存放跳舞的男女，num是跳舞的人数 
    person p;
    int i;
    SqQueue Mdancers,Fdancers;
    InitQueue(Mdancers);     //男士队列初始化 
    InitQueue(Fdancers);     //女士队列初始化 
    for (i=0;i<num;i++)      //根据性别依次将跳舞的人插入相应队列 
    {
        p=dancer[i];
        if (p.sex=='F')  EnQueue(Fdancers,p);    //插入男队
        else EnQueue(Mdancers,p);               //插入女队    
     } 
    cout<<"The dancing partner are:\n";
    while(!QueueEmpty(Fdancers)&&!QueueEmpty(Mdancers)) 
    {//依次输出男女舞伴的姓名 
        DeQueue(Fdancers,p);     //女士出队
        cout<<p.name<<" ";      //输出出队女士姓名
        DeQueue(Mdancers,p);    //男士出队
        cout<<p.name<<endl;     //输出出队男士姓名 
             
    }
    if (!QueueEmpty(Fdancers))      //女士队非空，输出队头女士的姓名 
    {
        p=GetHead(Fdancers);     // 取女队的头
		cout<<"The first woman to get a partner is:"<<p.name<<endl;  
    }
    else if (!QueueEmpty(Mdancers))    //男士队非空，输出男士队头的姓名
    {
        p=GetHead(Mdancers);     // 取男队的头
        cout<<"The first man to get a partner is:"<<p.name<<endl;
    }
     
 } 
 
int main()
 {
     int i,j;
     person dancer[QueueSize];
     cout<<"请输入跳舞的人数：";
     cin>>j;
     while(j<=0)
     {
         cout<<"输入错误，请重新输入跳舞的人数："; 
         cin>>j;
     }
    for(i=1;i<=j;i++)
    {
        cout<<"请输入第"<<i<<"舞者的名字:"<<endl;
        cin>>dancer[i-1].name;
        cout<<"请输入第"<<i<<"个人的性别(F/M):"<<endl;
        cin>>dancer[i-1].sex;
        while(dancer[i-1].sex!='F'&&dancer[i-1].sex!='M')
        {
            cout<<"*******输入错误，请重新输入：\n";
            cout<<dancer[i-1].sex;
            cout<<"请输入第"<<i<<"个人的性别(F/M):"<<endl;
            cin>>dancer[i-1].sex;
            break;
        }
    }
    DancePartner(dancer,j);
     
      
 }
```
这次代码有点长，好像前面的也是耶(*^▽^*)；