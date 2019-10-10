# 初始化

_接着上一篇：链表的基本操作（这两篇应该是同一篇的）_
```
LNode *L,*p,*Lb,*Lc;
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
最后就是L->Y->p->q
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
注：创建一个有序单链表，时间复杂度为0(n）：因为
一些快速的排序算法，不能用，只能用直接插入等o(n^2) 级的排序算法来实现排序。因为是有序单链表那么每次插入到链表尾结点，那么每次插入都要从头扫到尾，然后1+2+3+... m = O(m^2)这样。


```
Status insert_L(LinkList &L,int i){
	LNode *p,*s;
	int j=0,k=0;
	p = L->next;
	if(i<=p->data){
		s=new LNode;							//生成新结点s
	s->data=i;								//将结点s的数据域置为e
	s->next=L->next;						//将结点s插入L中
	L->next=s;
	}
	while(p){
		if(p->data>i) break;
		p = p->next;
		j++;
	}
	p = L->next;
	while(p && k<j-1){p=p->next;++k;}
	if(!p||k>j-1)	return ERROR;			//i大于表长+1或者小于1
	s=new LNode;							//生成新结点s
	s->data=i;								//将结点s的数据域置为e
	s->next=p->next;						//将结点s插入L中
	p->next=s;
	return OK;
} 
```

