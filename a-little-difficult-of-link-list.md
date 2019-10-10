# 初始化

_接着上一篇：链表的基本操作（这两篇应该是同一篇的）_

```
LNode *L,*p,*Lb,*Lc，*B,*C;
        case 9:
            NI_L(L); 
            cout<<"链表的逆值";
            p=L->next;
            while(p)
            {
                cout<<p->data<<" ";
                p=p->next;
            }
            cout<<endl;
            break;
        case 10:
             cout<<"在创建一个表";
             InitList_L(Lb);
             CreateList_L(Lb,10);
             cout<<"成功创建链表!\n\n";
            p=L->next;
            while(p)
            {
                cout<<p->data<<" ";
                p=p->next;
            }
            cout<<endl;
             Merge_L(L,Lb,Lc);
             cout<<"合并后的列表为\n\n";
              p=Lc->next;
            while(p)
            {
                cout<<p->data<<" ";
                p=p->next;
            }
            cout<<endl;
            break;
          case 11:
            int i;
            cout<<"插入的数为："; 
            cin>>i;
            insert_L(L,i);
            cout<<"插入后的链表为\n";
            p=L->next;
            while(p)
            {
                cout<<p->data<<" ";
                p=p->next;
            }
            cout<<"\n"<<endl;
            break;
            case 12:
            InitList_L(B);
            InitList_L(C);
            Resolve_L(L,B,C);
            cout<<"大于零的链表为\n" ;
             p=B->next;
            while(p)
            {
                cout<<p->data<<" ";
                p=p->next;
            }
            cout<<"\n"<<endl;
            cout<<"小于零的链表为\n" ; 
             p=C->next;
            while(p)
            {
                cout<<p->data<<" ";
                p=p->next;
            }
            cout<<"\n"<<endl;
            break;
```

## 原地逆置

```
void NI_L(LinkList&L){
    LNode *q ,*p;
    p = L->next;
    L->next = NULL;
    while(p){
        q = p->next;
        p->next = L->next;
        L->next = p;
        p = q;
    }

}
```

这个我思考了好久，发现只有自己画图多画画才能理解其中的道理！  
第一次循环  
![](/assets/5.gif)  
这第一次循环看不出啥  
看第二次循环

![](/assets/6.gif)  
最后就是L-&gt;Y-&gt;p-&gt;q  
循环操作，就可以达到逆值。

## 有序链表的合并

```
void Merge_L(LinkList &L,LinkList &Lb,LinkList &Lc){
    LNode *pa,*pb,*pc,*q;
    pa = L->next;
    pb = Lb->next;
    Lc = L;
    pc = Lc; 
    while(pa&&pb){
        if(pa->data<pb->data){
            pc->next=pa;
            pc=pa;
            pa=pa->next;
        }else if(pa->data>pb->data){
            pc->next=pb;//将a所指结点链接到pc所指结点之后 
            pc=pb;//pc指向pa 
            pb=pb->next;//pa指向下一节点 
        }else{
        pc->next=pa;
        pc=pa;
        pa=pa->next; 
        q=pb->next;
        delete pb;
        pb = q;
    }
  } 
    pc->next=pa?pa:pb;
    delete Lb; 
}
```

注意相等的情况，pc指向pa是，删除和pa相同的pb，并让pb也指向下一个

## 有序表中插入数据插入和还保持并有序

注：创建一个有序单链表，时间复杂度为0\(n^2）：因为  
一些快速的排序算法，不能用，只能用直接插入等o\(n^2\) 级的排序算法来实现排序。因为是有序单链表那么每次插入到链表尾结点，那么每次插入都要从头扫到尾，然后1+2+3+... n = O\(n^2\)这样。

```
Status insert_L(LinkList &L,int i){
    LNode *p,*s;
    int j=0,k=0;
    p = L->next;
    if(i<=p->data){
        s=new LNode;                            //生成新结点s
    s->data=i;                                //将结点s的数据域置为e
    s->next=L->next;                        //将结点s插入L中
    L->next=s;
    }
    while(p){
        if(p->data>i) break;
        p = p->next;
        j++;
    }
    p = L->next;
    while(p && k<j-1){p=p->next;++k;}
    if(!p||k>j-1)    return ERROR;            //i大于表长+1或者小于1
    s=new LNode;                            //生成新结点s
    s->data=i;                                //将结点s的数据域置为e
    s->next=p->next;                        //将结点s插入L中
    p->next=s;
    return OK;
}
```

## 将链表分解

将链表分解为两个具有相同结构的链表，A和B,其中，A链表中存放大于0的数，B中存放小于0的数    （差不多题目叙述就是这个意思，今天懒得打字，嘻嘻嘻）  
key：在创建两个链表，if判断，然后加入到表中

```
void Resolve_L(LinkList &L,LinkList &B,LinkList &C){
    LNode *r,*p;
//也可一个链表还用原来的头结点，另一个用新分配的（不过，在这里我似乎不知道怎么用，因为这一个程序功能好多！）
//    B=L;
//    B->next = NULL;
//    C=new LNode;
//    C->next=NULL;
    p=L->next;
    while(p!=NULL){
        r=p->next;
        if(p->data<0){
            p->next=B->next;
            B->next=p;
        }else{
            p->next=C->next;
            C->next=p;
        }
        p=r;
    }
}
```

## 最后再写一个：顺序有序表的合并

**写完这个就去吃饭**

```
void MergeList_L(SqList LA,SqList LB,SqList &LC){
    LC.length=LA.length+LB.length;//求新表的长度
    LC.elem=new ElemType[LC.length];//分配一个新的数组
    pc=LC.elem;
    pa=LA.elem; pb=LB.elem;
    pa_last=LA.elem+LA,length-1;//指向最后一个元素
    pa_last=LB.elem+LB,length-1;
    while((pa<=pa_last)&&pb<=pb_last){//LA,LB均未达到末尾
    if(*pa<=*pb) *pc++ = *pa++;
    else *pc++ = *pb++;
        }
        //下面两个只会执行一个
        while(pa<=pa_last) *pc++ = *pa++;
    while(pb<=pb_last) *pc++ = *pb++;
}
```

最后的这段代码只是为了比较一下顺序表和链表的合并，跟主函数没关系。❥\(^\_-\)

