# 字符串匹配算法及另外的操作
## 字符串匹配+统计比较次数
```c++
/***字符串匹配算法***/
#include<cstring>
#include<iostream>
using namespace std;

#define OK 1
#define ERROR 0
#define OVERFLOW -2
typedef int Status;
#define MAXSTRLEN 255   		//用户可在255以内定义最长串长
typedef struct {		//0号单元存放串的长度
 char ch[MAXSTRLEN+1];
 int length;
  }SString;
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

//填写4.1　BF算法 并增加计算比较次数 
int Index_BF(SString S, SString T, int pos)
{
	//返回模式T在主串S中第pos个字符之后第s一次出现的位置。若不存在，则返回值为0
	//其中，T非空，1≤pos≤StrLength(S)
	int i=pos; int j=1;
	int count1=0;
	while(i<=S.length&&j<=T.length){
		count1++;
//		cout<<"比较次数为:"<<count1<<endl;
		if(S.ch[i]==T.ch[j]){
			++i;++j;
		}else{
			i=i-j+2;j=1;
		}
		
	}
	cout<<"比较次数为:"<<count1<<endl;
	if(j>T.length)
			return i-T.length;
		else return 0;
	

}//Index
//算法4.3　计算next函数值
void get_next(SString T, int next[])
{ //求模式串T的next函数值并存入数组next
	int i = 1, j = 0;
	next[1] = 0;
	while (i < T.length)
		if (j == 0 || T.ch[i] == T.ch[j])
		{
			++i;
			++j;
			next[i] = j;
		}
		else
			j = next[j];
}//get_next

//算法4.2　KMP算法,增加计算比较次数
int Index_KMP(SString S, SString T, int pos, int next[])
{ 	// 利用模式串T的next函数求T在主串S中第pos个字符之后的位置的KMP算法
	//其中，T非空，1≤pos≤StrLength(S)
	int i = pos, j = 1;
	int count2=0;
	while (i <= S.length && j <= T.length)
	{	
		count2++;
		//cout<<"比较次数为:"<<count2<<endl;
	if (j == 0 || S.ch[i] == T.ch[j]) // 继续比较后继字
		{
			++i;
			++j;
		}
		else
			j = next[j]; }// 模式串向右移动
	cout<<"比较次数为:"<<count2<<endl;
	if (j > T.length) // 匹配成功
		return i - T.length;
	else
		return 0;
	 
}//Index_KMP
int main()
{
	SString S;
	StrAssign(S,"bbabababababba") ;
	SString T;
	StrAssign(T,"babb") ;
	
	cout<<"调用BF算法:"<<endl; 
	cout<<"主串和子串在第"<<Index_BF(S,T,1)<<"个字符处首次匹配\n";
	int *p = new int[T.length+1]; // 生成T的next数组
	get_next(T,p);
	cout<<"调用KMP算法:"<<endl;
	cout<<"主串和子串在第"<<Index_KMP(S,T,1,p)<<"个字符处首次匹配\n";
	return 0;
}

```
## 统计匹配字符串+统计子串出现次数
```c++
/***字符串匹配算法***/
#include<cstring>
#include<iostream>
using namespace std;

#define OK 1
#define ERROR 0
#define OVERFLOW -2
typedef int Status;
#define MAXSTRLEN 255   		//用户可在255以内定义最长串长
typedef struct {		//0号单元存放串的长度
 char ch[MAXSTRLEN+1];
 int length;
  }SString;
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

//填写4.1　BF算法 并增加计算比较次数 
int Count_BF(SString S, SString T, int pos)
{
	//返回模式T在主串S中第pos个字符之后第s一次出现的位置。若不存在，则返回值为0
	//其中，T非空，1≤pos≤StrLength(S)
	int count=0;
	int i=pos;
	int j=1;
	for(i=pos;i<=S.length;){
		//cout<<i;
	while(i<=S.length&&j<=T.length){

		if(S.ch[i]==T.ch[j]){
			++i;++j;
		}else{
			i=i-j+2;j=1;
		}
		
	}
	if(j>T.length){
		pos = i-T.length;
		i=i-j+2;
		j=1;
//		cout<<pos<<"&"<<i<<endl;
		count++;
		continue;
	}
}
	return count;
	

}//Index
//算法4.3　计算next函数值
void get_next(SString T, int next[])
{ //求模式串T的next函数值并存入数组next
	int i = 1, j = 0;
	next[1] = 0;
	while (i < T.length)
		if (j == 0 || T.ch[i] == T.ch[j])
		{
			++i;
			++j;
			next[i] = j;
		}
		else
			j = next[j];
}//get_next

//算法4.2　KMP算法,增加计算比较次数
int Index_KMP(SString S, SString T, int pos, int next[])
{ 	// 利用模式串T的next函数求T在主串S中第pos个字符之后的位置的KMP算法
	//其中，T非空，1≤pos≤StrLength(S)
	int i = pos, j = 1,count=0;
	for(i=pos;i<=S.length;){
		while (i <= S.length && j <= T.length)
	{	
	if (j == 0 || S.ch[i] == T.ch[j]) // 继续比较后继字
		{
			++i;
			++j;
		}
		else
			j = next[j]; }// 模式串向右移动

	if (j > T.length) // 匹配成功
		pos = i - T.length;
		i=i+1;
		j=1;
		count++;
	}

	return count;	
		
	 
}//Index_KMP
int main()
{
	SString S;
	StrAssign(S,"bbababbbababababbababb");
			//bba babb bababa babb a babb 3次 
	SString T;
	StrAssign(T,"babb") ;
	
	cout<<"调用BF算法:"<<endl; 
	cout<<"子串在主串中出现"<<Count_BF(S,T,1)<<"次\n";
	int *p = new int[T.length+1]; // 生成T的next数组
	get_next(T,p);
	cout<<"调用KMP算法:"<<endl;
	cout<<"子串在主串中出现"<<Index_KMP(S,T,1,p)<<"次\n";
	return 0;
}

```
