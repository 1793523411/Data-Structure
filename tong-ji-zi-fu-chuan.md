# 统计字符串的相关操作
## 统计字符出现次数
```c++
/*统计串S中字符的种类和个数*/
#include<cstring>
#include<iostream>
using namespace std;


#define OK 1
#define ERROR 0
#define OVERFLOW -2
typedef int Status;
#define MAXSTRLEN 255   		//用户可在255以内定义最长串长
//typedef char SString[MAXSTRLEN+1];		//0号单元存放串的长度

typedef struct{   
   char ch[MAXSTRLEN+1];				//若是非空串，则按串长分配存储区，否则ch为NULL   
   int length;				//串长度   
}SString; 

typedef struct {
     char ch;//存放出现的字符 
     int num;//出现的个数     
 } mytype;
Status StrAssign(SString &T, char *chars) { //生成一个其值等于chars的串T
	int i;
	if (strlen(chars) > MAXSTRLEN)
		return ERROR;
	else {
		T.length = strlen(chars);
		for (i = 1; i <= T.length; i++)
			T.ch[i] = *(chars + i - 1);
		return OK;
	}
}
void StrAnalyze(SString S)//统计串S中字符的种类和个数
{char c;
  mytype T[20]; //用结构数组T存储统计结果
 int i,j;
 for (i=0;i<=20;i++) {T[i].ch='\0',T[i].num=0;};
  for(i=1;i<=S.length;i++)
  {
    c=S.ch[i];j=0;
    while(T[j].ch&&T[j].ch!=c) j++; //查找当前字符c是否已记录过
    if(T[j].ch) T[j].num++;
    else {T[j].ch=c;T[j].num++;}
  }//for 
  for(j=0;T[j].ch;j++)
   cout<<T[j].ch<<":"<<T[j].num<<endl;
}//StrAnalyze

int  main()
{SString S;
char *T="sasdddas1111as";
StrAssign(S,T);
StrAnalyze(S);
return 0;
}



```
## 统计字符出现次数的另一种情况
```c++
/*统计串S中字符的种类和个数*/
#include<cstring>
#include<iostream>
using namespace std;

void count()//统计输入字符串中数字字符和字母字符的个数。
{ int i,num[36];
  char ch;
  for (i=0;i<36;i++) num[i]=0;
  while ((ch=getchar())!='#')
  {
  if('0'<=ch&&ch<='9') 
     {i=int(ch)-48;num[i]++;}
   else if('A'<=ch&&ch<='Z')
      {i=ch-65+10;num[i]++;}}
  for(i=0;i<10;i++)
	  cout<<"数字"<<i<<"的个数"<<num[i]<<endl;
  for(i=10;i<36;i++)   
    cout<<"字母"<<char(i+55)<<"的个数"<<num[i]<<endl;
}
int main()
{cout<<"请输入字符串:('#'结束)"; 
count();
}




```