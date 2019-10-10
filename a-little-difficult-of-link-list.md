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