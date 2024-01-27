# Lab Week 11
## ʵ�����ݣ�Implementation of Topological Sorting
Refer to the example of the digraph and data structures in Lecture 21, Slide 42-65.
(1) Represent a digraph with an Adjacency List
(2) Determine the data structures for the process of improved topological sorting
(3) Design a C program implementing the improved topological sorting on the digraph

## ʵ���ص�
1. �ö�����������Ϊ0�ĵ㣬ÿ�ν����е�ͷȡ�����������ӵĶ�����������1�Ĳ�����������Ϊ0������������д�š�
2. ���ڽӱ����͵����֮��Ĺ�ϵ���ȱ���һ�Σ������Ϊ0�ĵ��������У����������ö��жԵ���в�����
3. ������Ž��������У����Ըö��������Ϊ-1��ʾ�ýڵ��Ѿ������ʹ���������С�
4. ���ýṹ�幹���ڽӱ�ʱ���������Ķ��㱣�棬��������������������
5. ��������ʱ�����Ϊ0�Ķ��������У������������Ķ�����ȼ�1���ж�������Ƿ������������𰸣��ظ��˲�����
## ���˼·��������
1. ��ƶ���
```c
typedef struct queue{
alist *nums[100];
int head;
int tail;
}queue;
void init_queue(queue*q){
    q->head=0;
    q->tail=0;
}
void push(queue*q, alist*val){
    q->nums[q->tail++]=val;
}
void pop(queue*q){
    q->head++;
}
int empty(queue*q){
    if(q->head==q->tail)return 1;
    return 0;
}
alist* front(queue*q){
    
    return q->nums[q->head];
}
```
2. ����ڽӱ�,�ڽӱ��е�out�������Ϊ������Ķ���ĵ�ַ��������������������
```c
typedef struct AdjacencyList{
int val;
int insize;
int outsize;
struct AdjacencyList**out;
}alist;
void init_alist(alist*l,int val){
    l->insize=0;
    l->outsize=0;
    l->val=val;
    l->out=(alist**)malloc(sizeof(alist*)*size);
}
```
3. ����ͼ��������֮��Ĺ�ϵ
```c
int find(int x,alist*a,int n){
    for(int i=0;i<n;i++){
        if(a[i].val==x)return i;
    }
}
void construct_graph(int x,alist*a,alist*nums,int n){
    a->out[a->outsize++]=&nums[find(x,nums,n)];
    nums[find(x,nums,n)].insize++;
}
```
4. �����Ϊ0�Ķ�����в���������������У�ͬʱ���õݹ������Ϊ������ӵĶ��������������
```c
void topo(alist*a,int*nums,int *s){                 
nums[(*s)++]=a->val;
a->insize-=1;
for(int i=0;i<a->outsize;i++){
    (*(a->out[i])).insize-=1;
    if((*(a->out[i])).insize==0)topo(&(*a->out[i]),nums,s);
}
}
```
## ����ʵ��
```c
#include<stdio.h>
#include<malloc.h>
#define size 100
typedef struct AdjacencyList{
int val;
int insize;
int outsize;
struct AdjacencyList**out;
}alist;
typedef struct queue{
alist *nums[100];
int head;
int tail;
}queue;
void init_queue(queue*q){
    q->head=0;
    q->tail=0;
}
void push(queue*q, alist*val){
    q->nums[q->tail++]=val;
}
void pop(queue*q){
    q->head++;
}
int empty(queue*q){
    if(q->head==q->tail)return 1;
    return 0;
}
alist* front(queue*q){
    
    return q->nums[q->head];
}
void init_alist(alist*l,int val){
    l->insize=0;
    l->outsize=0;
    l->val=val;
    l->out=(alist**)malloc(sizeof(alist*)*size);
}
int find(int x,alist*a,int n){
    for(int i=0;i<n;i++){
        if(a[i].val==x)return i;
    }
}
void construct_graph(int x,alist*a,alist*nums,int n){
    a->out[a->outsize++]=&nums[find(x,nums,n)];
    nums[find(x,nums,n)].insize++;
}
void topo(alist*a,int*nums,int *s){                 
nums[(*s)++]=a->val;
a->insize-=1;
for(int i=0;i<a->outsize;i++){
    (*(a->out[i])).insize-=1;
    if((*(a->out[i])).insize==0)topo(&(*a->out[i]),nums,s);
}
}
int main(){
    int ans[size];
    alist v[size];
    int vsize=0;
    queue q;
    printf("Please input the number of vertices:\n");
    int n;
    scanf("%d",&n);
    printf("Please input the vertices:\n");
    for(int i=0;i<n;i++){
    int x;
    scanf("%d",&x);    
    init_alist(&v[i],x);    
    }
    printf("Please input the relationship of each vertices and input -1 to quit:\n");
    for(int i=0;i<n;i++){
        printf("The %d to:\n",v[i].val);
        while(1){
        int x;
        scanf("%d",&x);
        if(x==-1)break ;
        construct_graph(x,&v[i],v,n);
        }
    }
     for(int i=0;i<n;i++){
      if(v[i].insize==0){
        v[i].insize-=1;
        push(&q,&v[i]);
      }
    }
    int s=0;
    while(!empty(&q)){
        alist *temp=front(&q);
        pop(&q);
        if(temp->insize==-1)topo(temp,ans,&s);
    }
    for(int i=0;i<n;i++)printf("%d ",ans[i]);
    for(int i=0;i<n;i++)free(v[i].out);
    return 0;
}
```
## ��������
1.**����һ** 
Please input the number of vertices:
6
Please input the vertices:
1 2 3 4 5 6
Please input the relationship of each vertices and input -1 to quit:
The 1 to:
2
3 4 -1
The 2 to:
-1
The 3 to:
2 5
-1
The 4 to:
5
-1
The 5 to:
-1
The 6 to:
4 5
-1
1 3 2 6 4 5 
2.**������** 
Please input the number of vertices:
5
Please input the vertices:        
1 2 3 4 5
Please input the relationship of each vertices and input -1 to quit:
The 1 to:
2
4
-1
The 2 to:
3
4
-1
The 3 to:
5
-1
The 4 to:
5
3
-1
The 5 to:
-1
1 2 4 3 5 
## �������
�趥��ĸ���Ϊn,�ߵĸ���Ϊm��
����ͼ�������ڽӱ�Ĺ��̵�ʱ�临�Ӷ�ΪO(m*n)����Ϊ������ϵ���������ÿ����ʱҪ�Զ�����б����ҵ���Ӧ�������˵㡣
������������Ҫ��ÿ�������ÿ���߽��в�����������������ĵ�ʱ�临�Ӷ�ΪO(n+m)��