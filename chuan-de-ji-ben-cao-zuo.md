---
title: 串的结构与匹配算法
date: 2019-10-19 21:20:46
tags:
- 数据结构
- 串
---
## 串的存储结构
**顺序存储**
### 定长顺序存储
串长也为结构体的一部分
<!-- more -->
```c++
#define MAXLEN//串的最大长度
typedef struct{
    char ch[MAXLEN+1];//存储串的一维数组
    int length;//当前长度
}SString;
```
### 堆式顺序存储结构
```c++
typedef struct{
    char *ch;//若是非空串，按串的长度分配存储区，否则串为空
    int length;//当前长度
}HString;
```
**链式存储**
### 链式存储结构
特点：
* 存储密度小
* 方便插入删除
```c++
#define CHUNKSIZE 80
typedef struct Chunk{
    char ch[CHUNKSIZE];
    struct Chunk *next;
}Chunk;
typedef struct{
    Chunk *head,*tail;//出头指针外，可附设一个尾指针，指示链表的最后一个节点，并给出链表长度。如此定义的串存储结构称为块链结构
    int length;
}
```
## 串的模式匹配算法
### BF算法
```c++
int Index_BF(SString S,SString T,int pos){
    i=pos; j=1;//pos为指定从字符串的哪一个位置开始查找
    while(i<=S.length && j<=T.length){
        if(S.ch[i]==T.ch[i]){
            ++i;++j;
        }else{
            i=i-j+2;j=1;//这里j是从一开始的，决定了i=i-j+2
        }
    if(j>T.length) return i-T.length;
    else return 0;
    }
}
```
分析：设子串m,主串n
* 最好情况下，每趟不匹配的字符都发生在第一个
 * 前i-1趟中对子串:i-1 + m 次
 * 对主串：由1到n-m+1
平均此次数为:(m+n)/2;
* 最坏情况下，每趟不匹配的字符都发生在最后一个
 * 前i-1趟：（i-1）*m次,第i趟：m次
 * 对主串：由1到n-m+1
平均次数为：m*(n-m+2)/2;
**由于每次不匹配时都要回溯，造成算法时间复杂度较高**
### KMP算法
```c++
int Index_KMP(SString S,SString T,int pos){
    i=pos;j=1;
    while(i<=S.length && j<=T.length){
        if(j==0||S.ch[i]==T.ch[j]){
            ++i;++j;
        }else{
            j=next[j];//模式串向右移动
        }
    }
    if(j>T.length ) return i-T.Length;
    else return 0;
}
```
### 计算next函数值
next函数的值取决于模式串本身，而和相匹配的主串无关

```c++
void get_next(SString T,int next[]){
    i=1;next[1]=0;j=0;
    while(i<T.length){
        if(j==T.ch[i]==T.ch[j]){
            ++i;++j;next[i]=j;
        }else{
            j=next[j];
        }
    }
}
```
![](http://pz56qt9o7.bkt.clouddn.com/9.gif)  
### 计算next函数修正值
你不上面next的某些缺陷具体看图
```c++
void get_nextval(SString T,int nextval[]){
    i=1;nextval[1]=0;j=0;
    while(i<T.length){
        if(j==0||T.ch[i]==T.ch[j]){
            ++i;++j;
            if(T.ch[i]!=T.ch[j]) nextval[i]=j;
            else nextval[i]=nextval[j];
        }else{
            j=nextval[j];
        }
    }
}
```
![](http://pz56qt9o7.bkt.clouddn.com/10.gif)  