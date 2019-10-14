今天实验课又写了几串代码
## 首先是用栈实现进制转换


```
/***链栈实现数制的转换***/
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
Status Push(LinkStack &S,int e)
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
Status Pop(LinkStack &S,int &e)
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

//算法3.8　数制的转换(链栈实现)
void conversion (int N) {
   //对于输入的任意一个非负十进制数，打印输出与其等值的八进制数
  //请填写完整
	SNode *S;
	int e;
	InitStack(S);
	while(N){
		Push(S,N%8);
		N=N/8;
	}
	while(!StackEmpty(S)){
		Pop(S,e);
		cout<<e;
	}

}
int main()
{
	int N;
	cin>>N;
	conversion(N);
	return 0;
}

```

分析一下：
* 用push入栈，入栈存放的是余数，这里存放数据的方式是链栈。
* 定义一个int类型的数来接受pop出栈的数，并cin
* 入栈循环，结束条件：传入的数不为0，即余数整数位不为0；
* 出栈循环，结束条件：栈为空，定义了一个函数来判断栈是否为空

## 第二个实验
第二个是链表求最值，还要输出节点的数据，这个感觉不太像栈的内容，这里要求用递归来实现节点输出


```
/***依次输出链表中的各个结点***/
/*添加求最大值*/
#include<iostream>
using namespace std;

#define OK 1
#define ERROR 0
#define OVERFLOW -2
typedef int Status;
typedef int ElemType;
typedef struct LNode {
	ElemType data; //结点的数据域
	LNode *next; //结点的指针域
} LNode, *LinkList;//头指针

//后插法创建链表算法
void CreateList_L(LinkList &L, int n) {
	L = new LNode;
	L->next = NULL;
	LNode *p, *r;
	r = L;
	for (int i = 0; i < n; i++) {
		p = new LNode;
		cin >> p->data;
		p->next = NULL;
		r->next = p;
		r = p;
	}
}

//算法3.9　遍历输出链表中各个结点的递归算法
void TraverseList(LinkList p) {
	if (p == NULL)
		return; //递归终止
	else {
		cout << p->data << endl; //输出当前结点的数据域
		TraverseList(p->next); //p指向后继指点继续递归
	}
}

int MaxList_L(LinkList L){
	LNode *p;
	int max;
	p=L->next;
	max=p->data;
	p=p->next;
	while(p){
		if(p->data>max){
			max=p->data;
		}
//		cout<<max<<"\n";
		p=p->next;
	}
	//cout<<max;
	return max;
}
int main() {
	int n;
	LinkList L;
	//InitList(L);
	cout << "请输入元素个数：" << endl;
	cin >> n;
	cout << "请输入链表元素(以空格隔开，按回车结束)：" << endl;
	CreateList_L(L, n); //后插法创建链表算法
	cout << "链表中的元素依次为" << endl;
	TraverseList(L->next);
	/*求最大值*/
	cout<<"the max number is:\n";
	cout<<MaxList_L(L);
	cout << endl;
	return 0;
}
```
后插法创建了链表
开始遇到了一些问题：当输入两位数时，输出全是一位数，求最值也始终求不对，上课时有点疑惑，逻辑代码明明是正确的
下课来到自己电脑面前，深吸一口气一边说话一边看着代码的首部，我去，`typedef char ElemType;`,怪不得，数的全是字符，能对才怪，老师这是故意的吧
幸好我发现了，解决了，真开心

递归调用！！！
一：


```
int func(int num)
{
    if(num<=8)
        return num;
    return num%8+10*func(num/8);
}
```
二：


```
void change (int n)
{
int i=n%8;
if(i)
{
change(n/8);
printf("%d",i);
}
}
```




