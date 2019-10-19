# 表达式求值
## 中缀表达式
<!-- more -->
```c++
#include<iostream>
using namespace std;

#define OK 1
#define ERROR 0
#define OVERFLOW -2
const char oper[7] = { '+', '-', '*', '/', '(', ')', '#' };
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

Status GetTop(LinkStack &S){
	if(!S)
	return ERROR;
		return S->data;
}
bool In(char ch) {//判断ch是否为运算符
	for (int i = 0; i < 7; i++) {
		if (ch == oper[i]) {
			return true;
		}
	}
	return false;
}
char Precede(char theta1, char theta2) {//判断运算符优先级
	if ((theta1 == '(' && theta2 == ')') || (theta1 == '#' && theta2 == '#')) {
		return '=';
	} else if (theta1 == '(' || theta1 == '#' || theta2 == '(' || (theta1
			== '+' || theta1 == '-') && (theta2 == '*' || theta2 == '/')) {
		return '<';
	} else
		return '>';
}
char Operate(char first, char theta, char second) {//计算两数运算结果
	switch (theta) {
	case '+':
		return (first - '0') + (second - '0') + 48;
	case '-':
		return (first - '0') - (second - '0') + 48;
	case '*':
		return (first - '0') * (second - '0') + 48;
	case '/':
		return (first - '0') / (second - '0') + 48;
	}
	return 0;
}

void EvaluateExpression(){
	SNode *OPND,*OPTR;
	char ch,theta,x,a,b;
//	int a,b;
	InitStack(OPND);
	InitStack(OPTR);
	Push(OPTR,'#');
	cin>>ch;
	while(ch!='#'||GetTop(OPTR)!='#'){
//		cout<<GetTop(OPND);
		if(!In(ch)){
			Push(OPND,ch);
			cin>>ch;
		}else
			switch(Precede(GetTop(OPTR),ch)){
				case '<':
					Push(OPTR,ch);
					cin>>ch;
					break;
				case '>':
					Pop(OPTR,theta);
					Pop(OPND,b);Pop(OPND,a);
					Push(OPND,Operate(a,theta,b));
					break;
				case '=':
					Pop(OPTR,x);cin>>ch;
					break; 
			}
	}
		cout<<GetTop(OPND)-48;
//		return GetTop(OPND);
}
int main(){
	EvaluateExpression();
	return 0;
} 
```

## 后缀表达式
### 链栈
```c++
#include<iostream>
using namespace std;

#define OK 1
#define ERROR 0
#define OVERFLOW -2
const char oper[7] = { '+', '-', '*', '/', '$' };
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

Status Push(LinkStack &S,float e)
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
float Pop(LinkStack &S)
{
	SNode *p;
	float e;
	if(!S)
		return ERROR;
	e = S->data;
	p = S;
	S = S->next;
	delete p;
	return e;
}

Status GetTop(LinkStack &S){
	if(S!=NULL)
		return S->data;
}

float EvaluateExpression(){
	SNode *OPND;
	InitStack(OPND);
	float num=0.0,x1,x2,scale;
	char x;
	cin>>x;
	while(x!='$'){
		while((x>='0'&&x<='9')||x=='.'){
			if(x==' '||x=='+'||x=='-'||x=='*'||x=='/') break;
				if(x!='.'){
					num=num*10+(float)(x-'0');
					cin>>x;
				}else{
					scale=10.0;cin>>x;
					while(x>='0'&&x<='9'){
						num=num+(float)(x-'0')/scale;
						scale= scale*10;
						cin>>x;
					}
				}
				Push(OPND,num);
				num=0.0;
		}
		
		
		switch(x){
							
				case ' ':break;
				case '+':Push(OPND,Pop(OPND)+Pop(OPND));break;
				case '-':x1=Pop(OPND);
							x2=Pop(OPND);
							Push(OPND,x2-x1);break;
				case '*': Push(OPND,Pop(OPND)*Pop(OPND));break;
				case '/':x1=Pop(OPND);x2=Pop(OPND);Push(OPND,x2/x1);break;
				defaule: ;
		}
		cin>>x;
	}
	cout<<"后缀表达式的值为"<<Pop(OPND); 
} 
int main(){
	
	EvaluateExpression();
	return 0;
} 
```
### 顺序栈
```c++
#include<iostream>
using namespace std;
typedef struct{
	float *top;
	float *base;
}SqStack;

void init(SqStack &S){
	S.base=new float[30]; 
	S.top=S.base;
}
void Push(SqStack &S,float e){
	*S.top++=e;
}
float Pop(SqStack &S){
	float e;
	e=*--S.top;
	return e;
}
float GetTop(SqStack S){
	return *(S.top-1);
}

float expr(){
	SqStack OPND;
	init(OPND);
	float num=0.0,x1,x2,scale;
	char x;
	cin>>x;
	while(x!='$'){
		while((x>='0'&&x<='9')||x=='.'){
			if(x==' '||x=='+'||x=='-'||x=='*'||x=='/') break;
				if(x!='.'){
					num=num*10+(float)(x-'0');
					cin>>x;
				}else{
					scale=10.0;cin>>x;
					while(x>='0'&&x<='9'){
						num=num+(float)(x-'0')/scale;
						scale= scale*10;
						cin>>x;
					}
				}
				Push(OPND,num);
				num=0.0;
		}
		
		
		switch(x){
							
				case ' ':break;
				case '+':Push(OPND,Pop(OPND)+Pop(OPND));break;
				case '-':x1=Pop(OPND);
							x2=Pop(OPND);
							Push(OPND,x2-x1);break;
				case '*': Push(OPND,Pop(OPND)*Pop(OPND));break;
				case '/':x1=Pop(OPND);x2=Pop(OPND);Push(OPND,x2/x1);break;
				defaule: ;
		}
		cin>>x;
	}
	cout<<"后缀表达式的值为"<<Pop(OPND); 
} 
int main(){
	expr();
	return 0;
} 
```