# 循环链表与双向链表

**这回除了写一写链表知识外，还要在比较一下链表和顺序表哦！**

## 循环链表

### 定义

循环链表是另一种形式的链式存贮结构。它的特点是表中最后一个结点的指针域指向头结点，整个链表形成一个环

### 主要语句

判断空链表的条件是  
单链表中，判别条件为：`p!=NULL或p->next!=NULL;`  
循环链表中，判别条件为：`p!=L或p->next!=L;`

```
head==head->next;
rear==rear->next;
```

### 循环链表合并

假设A,B都是两个链表的尾指针  
下面只保留A的头结点，B的头结点舍弃掉

```
p = B->next->next;//存储B的首元结点
B->next = A->next;//B链表的尾部指向A链表的头部
a->next = p;//A的尾结点指向B的首元结点
```

## 双向链表

单链表查找后继节点时间复杂度为：0（1），而查找前驱节点为0（n）,  
为克服这个缺陷，有了双向链表。

### 定义

双向链表也叫双链表，是链表的一种，它的每个数据结点中都有两个指针，分别指向直接后继和直接前驱。所以，从双向链表中的任意一个结点开始，都可以很方便地访问它的前驱结点和后继结点。一般我们都构造双向循环链表。

### 主要语句

d-&gt;next-&gt;prior-&gt;next=d

### 存储结构

```
typedef struct DuLNode
{
    ElemTyple data;
    struct DuLNode *prior;
    struct DuLNode *next;
}DuLNode,*DuLinkList;
```

### 双向链表的插入

```
Status ListInsert_Dul(DuLinkList &L,int i,ElemType e)
{
    if(!(p=GetElem_Dul(L,i)))//在L中确定第i个元素的位置指针p
        return ERROE;
    s=new DuLNode;//生成新的节点
    s->data=e;
    s->prior=p->prior;//s->q(设q为p前面的节点)
    p->prior->next=s;//q->p
    s-next=p;//s->p
    p-prior=s;//p->s
    return OK;

}
```

### 双向链表的删除

```
Status ListDelte_Dul(DuLinkList &L,int i)
{
    if(!(p=GetElem_Dul(L,i)))
        return ERROR;
        //下面这两个可以交换位置
    p->prior->next=p->next;
    p->next->prior=p->prior;
    return OK;
}
```

## 时间复杂度的比较

| 顺序表 | 插入 | 取值 | 查找 | 删除 | 创建 |
| :---: | :---: | :---: | :---: | :--- | :--- |
| 顺序表 | o\(n\) | o\(1\) | o\(n\) | o\(n\) |  |
| 平均时间复杂度\(顺序表\) | n/2 |  | \(n+1\)/2 | \(n-1\)/2 |  |
| 单链表 | o\(n\)（找） | o\(n\) | o\(n\) | o\(n\)（找） | o\(n\)\(前插后插都是\) |

创建一个有序单链表·的时间复杂度为o\(n^2\)

单链表存储密度小于1



