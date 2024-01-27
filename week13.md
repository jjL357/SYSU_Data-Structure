# Lab Week13
## ʵ������
Implementation of PRIM Algorithm Using min-Heap
Refer to the example of the digraph and data structures in Lecture 
23, Slide 9-34.
(1) Represent a digraph with an Adjacency List
(2) Determine the data structures for the process of PRIM algorithm using min-Heap
(3) Design a C program implementing the PRIM algorithm on the digraph using min-Heap

## ʵ���ص�
1. ʹ����С�Ѱ�������Ѱ����̱ߣ�ÿ�δӶѶ�����̱ߣ�����Ķ˵����δ�����ʵĽڵ���������������Ѱ�ҵıߣ�����pop������Ѱ�ҡ�
2. ÿ��ѭ��ȡ������Ҫ��ıߣ����������У�Ȼ�����δ�����ʽ�㣬�������־Ϊ�ѷ��ʣ�������ɨ��ÿһ�������ý��ıߣ���ÿһ����push����С����.
3. VertexNode����visited��־��visited=1ʱΪ�Ѿ����ʲ����뵽���У�Ϊ0��δ�����ʣ�������ֹ���Ƕ�ͬһ���ڵ��η��ʡ�
4. ���öѵ���ά��������ÿ�μ����µı߶�Ҫ������С�ѡ�

## ���˼·��������
1. �����ڽӱ��ͼ��EdgeNode����ߵ���Ϣ����¼�յ�����ꡢ�������걣���ڰ����ߵ�VertexNode�С�Task�ṹ�屣���������Ϣ�������������������ʱ��͹ؼ�ʱ��͸�������VertexNode����EdgeNode��Task�Ͷ������Ⱥ����꣬graph�������������ڽӡ�VertexNode��visited=1ʱΪ�Ѿ����ʲ����뵽���У�Ϊ0��δ�����ʣ�������ֹ���Ƕ�ͬһ���ڵ��η��ʡ�
```c
typedef struct Edge{
    int start;
    int end;
    int length; 
    struct Edge*next; 
}Edge;
typedef struct Vertex 
{    
    int visited;
    int number;
    int index;
    int distance;
    struct Vertex*parent;
    Edge e;
}VertexNode,AdjList[size];
typedef struct
{   
    int nums_vertex;
    int nums_edges;
    AdjList adjList;
}Graph;
```
2.�ڽӱ�ĳ�ʼ��,��parent��nextָ�븳ֵΪNULL��visited��ֵ0��ʾδ����.�ڹ���ͼ������ߣ������²������ҵ��ߵ������������ڽӱ��λ�ã��½��߽��edge�����ڶ����ڽӱ��nextָ�롣  
```c   
void initial_vertex(VertexNode*v,int name,int index){
    v->distance=infinite;
    v->index=index;
    v->number=name;
    v->parent=NULL;
    v->visited=0;
    v->e.start=index;
    v->e.length=0;
    v->e.next=NULL;
    v->e.end=index;
}
void initial_edges(Edge*e,int start,int end,int length){
Edge*pre=e;
Edge*cur=e->next;
while(cur){
    pre=pre->next;
    cur=cur->next;
}
cur=(Edge*)malloc(sizeof(Edge));
cur->start=start;
cur->end=end;
cur->length=length;
cur->next=NULL;
pre->next=cur;
}
void initial_graph(Graph *g,int n,int m){
    g->nums_edges=m;
    g->nums_vertex=n;
    printf("Please input the number of each vertex:\n");
    for(int i=1;i<=n;i++){
    int name;
    scanf("%d",&name);
    initial_vertex(&g->adjList[i],name,i);
    }
    printf("Please input edges(the indexes of vertexes and the length of edges):\n");
    for(int i=1;i<=m;i++){
    int start,end,length;
    scanf("%d%d%d",&start,&end,&length);
    initial_edges(&g->adjList[start].e,start,end,length);
    initial_edges(&g->adjList[end].e,end,start,length);
    }
}
```
3. �ѵĹ�������ʼ����push��pop�͵���ά���Ȳ�����������ÿ��Ԫ��Ϊ�ߵĵ�ַ���Ƚϵĸ����Ǳߵ�Ȩ�أ�
```c
typedef struct
{   
  int n;
  Edge *heap_edge[size];  
}Min_heap;
void initial_heap(Min_heap*min_heap){
    min_heap->n=0;
}
void print_heap(Min_heap*heap){
printf("The heap: \n");
for(int i=0;i<heap->n;i++)printf("%d->%d:%d",heap->heap_edge[i]->start,heap->heap_edge[i]->end,heap->heap_edge[i]->length);
printf("\n");
}
void adjust_min_heap(Min_heap* min_heap,int start,int end){
for(int l=start*2+1;l<=end;l=l*2+1){
    int min_id=l;
    if(l+1<=end && min_heap->heap_edge[l]->length  >min_heap->heap_edge[l+1]->length){
        min_id=l+1;
    }
    if(min_heap->heap_edge[min_id]->length<min_heap->heap_edge[start]->length){
        Edge*temp=min_heap->heap_edge[min_id];
        min_heap->heap_edge[min_id]=min_heap->heap_edge[start];
        min_heap->heap_edge[start]=temp;
        start=min_id;
    }
    else break;
    l=min_id;
}  
}
void struct_min_heap(Min_heap*min_heap,Edge*e,int heap_size){
min_heap->heap_edge[heap_size]=e;
heap_size++;
int start=heap_size-1;
for(int i=(start-1)/2;i>=0;i=(start-1)/2){
    if(min_heap->heap_edge[start]->length >=min_heap->heap_edge[i]->length)break;
    else {
        Edge*temp=min_heap->heap_edge[i];
        min_heap->heap_edge[i]=min_heap->heap_edge[start];
        min_heap->heap_edge[start]=temp;
        start=i;
    }
    if(i==0)break;
}
}
void push(Min_heap*min_heap,Edge*e){
struct_min_heap(min_heap,e,min_heap->n);
min_heap->n++;
}
void pop(Min_heap*min_heap){
Edge*temp=min_heap->heap_edge[0];
min_heap->heap_edge[0]=min_heap->heap_edge[min_heap->n-1];
adjust_min_heap(min_heap,0,min_heap->n-1);   
min_heap->n--; 
}
```
4. Prim�㷨������һ����������У�ʹ��һ����С���Ż������Ѿ����ʹ��Ľڵ�ı߼����������ÿ��ѭ������ÿ�δӶѶ�����̱ߣ�����Ķ˵����δ�����ʵĽڵ���������������Ѱ�ҵıߣ�����pop������Ѱ�ҡ�ÿ��ѭ��ȡ������Ҫ��ıߣ����������У�Ȼ�����δ�����ʽ�㣬�������־Ϊ�ѷ��ʣ�������ɨ��ÿһ�������ý��ıߣ���ÿһ����push����С����.�������丸�׽ڵ㡣  
```c
void prim(Graph*g,VertexNode**v,Edge**e){
    Min_heap min_heap;
    initial_heap(&min_heap);
    int Vn=0,En=0;
    g->adjList[1].visited=1;
    Edge*temp=g->adjList[1].e.next;
    while(temp){
        push(&min_heap,temp);
        temp=temp->next;
    }
    v[Vn++]=&g->adjList[1];
    while(Vn<g->nums_vertex){
    int target=min_heap.heap_edge[0]->end;
    if(g->adjList[target].visited==0){
    v[Vn++]=&g->adjList[target];
    g->adjList[target].visited=1;
    g->adjList[target].parent=&g->adjList[min_heap.heap_edge[0]->start];
    e[En++]=min_heap.heap_edge[0];
    pop(&min_heap);
    Edge*temp2=g->adjList[target].e.next;
    while(temp2){
        push(&min_heap,temp2);
        temp2=temp2->next;
    }
    }
    else{
    pop(&min_heap);
    }
    }
}
```
5. ��ӡ��С�������ıߺͶ��㣬�������С��������Ȩֵ��
```c
void print_spanning_tree(VertexNode**v,Edge**e,int n,int*sum){
printf("The spanning tree:");
for(int i=0;i<n-1;i++){
    (*sum)+=e[i]->length;
    printf("%d->%d ",e[i]->start,e[i]->end);
}
printf("\n");
printf("The minimum weight is %d\n",(*sum));
}

```
## ����ʵ��
```c
#include<stdio.h>
#include<malloc.h>
#include<string.h>
#include<limits.h>
#define size 100
#define infinite INT_MAX
typedef struct Edge{
    int start;
    int end;
    int length; 
    struct Edge*next; 
}Edge;
typedef struct Vertex 
{    
    int visited;
    int number;
    int index;
    int distance;
    struct Vertex*parent;
    Edge e;
}VertexNode,AdjList[size];
typedef struct
{   
    int nums_vertex;
    int nums_edges;
    AdjList adjList;
}Graph;
typedef struct
{   
  int n;
  Edge *heap_edge[size];  
}Min_heap;
void initial_vertex(VertexNode*v,int name,int index){
    v->distance=infinite;
    v->index=index;
    v->number=name;
    v->parent=NULL;
    v->visited=0;
    v->e.start=index;
    v->e.length=0;
    v->e.next=NULL;
    v->e.end=index;
}
void initial_edges(Edge*e,int start,int end,int length){
Edge*pre=e;
Edge*cur=e->next;
while(cur){
    pre=pre->next;
    cur=cur->next;
}
cur=(Edge*)malloc(sizeof(Edge));
cur->start=start;
cur->end=end;
cur->length=length;
cur->next=NULL;
pre->next=cur;
}
void initial_graph(Graph *g,int n,int m){
    g->nums_edges=m;
    g->nums_vertex=n;
    printf("Please input the number of each vertex:\n");
    for(int i=1;i<=n;i++){
    int name;
    scanf("%d",&name);
    initial_vertex(&g->adjList[i],name,i);
    }
    printf("Please input edges(the indexes of vertexes and the length of edges):\n");
    for(int i=1;i<=m;i++){
    int start,end,length;
    scanf("%d%d%d",&start,&end,&length);
    initial_edges(&g->adjList[start].e,start,end,length);
    initial_edges(&g->adjList[end].e,end,start,length);
    }
}
void print_graph(Graph*g){
    for(int i=1;i<=g->nums_vertex;i++){
        printf("%d:",g->adjList[i].number);
        Edge*temp=g->adjList[i].e.next;
        while(temp){
            printf("%d",temp->end);
            if(temp->next)printf("->");
            temp=temp->next;
        }
        printf("\n");
    }
}
void initial_heap(Min_heap*min_heap){
    min_heap->n=0;
}
void print_heap(Min_heap*heap){
printf("The heap: \n");
for(int i=0;i<heap->n;i++)printf("%d->%d:%d",heap->heap_edge[i]->start,heap->heap_edge[i]->end,heap->heap_edge[i]->length);
printf("\n");
}
void adjust_min_heap(Min_heap* min_heap,int start,int end){
for(int l=start*2+1;l<=end;l=l*2+1){
    int min_id=l;
    if(l+1<=end && min_heap->heap_edge[l]->length  >min_heap->heap_edge[l+1]->length){
        min_id=l+1;
    }
    if(min_heap->heap_edge[min_id]->length<min_heap->heap_edge[start]->length){
        Edge*temp=min_heap->heap_edge[min_id];
        min_heap->heap_edge[min_id]=min_heap->heap_edge[start];
        min_heap->heap_edge[start]=temp;
        start=min_id;
    }
    else break;
    l=min_id;
}  
}
void struct_min_heap(Min_heap*min_heap,Edge*e,int heap_size){
min_heap->heap_edge[heap_size]=e;
heap_size++;
int start=heap_size-1;
for(int i=(start-1)/2;i>=0;i=(start-1)/2){
    if(min_heap->heap_edge[start]->length >=min_heap->heap_edge[i]->length)break;
    else {
        Edge*temp=min_heap->heap_edge[i];
        min_heap->heap_edge[i]=min_heap->heap_edge[start];
        min_heap->heap_edge[start]=temp;
        start=i;
    }
    if(i==0)break;
}
}
void push(Min_heap*min_heap,Edge*e){
struct_min_heap(min_heap,e,min_heap->n);
min_heap->n++;
}
void pop(Min_heap*min_heap){
Edge*temp=min_heap->heap_edge[0];
min_heap->heap_edge[0]=min_heap->heap_edge[min_heap->n-1];
adjust_min_heap(min_heap,0,min_heap->n-1);   
min_heap->n--; 
}
void prim(Graph*g,VertexNode**v,Edge**e){
    Min_heap min_heap;
    initial_heap(&min_heap);
    int Vn=0,En=0;
    g->adjList[1].visited=1;
    Edge*temp=g->adjList[1].e.next;
    while(temp){
        push(&min_heap,temp);
        temp=temp->next;
    }
    v[Vn++]=&g->adjList[1];
    while(Vn<g->nums_vertex){
    int target=min_heap.heap_edge[0]->end;
    if(g->adjList[target].visited==0){
    v[Vn++]=&g->adjList[target];
    g->adjList[target].visited=1;
    g->adjList[target].parent=&g->adjList[min_heap.heap_edge[0]->start];
    e[En++]=min_heap.heap_edge[0];
    pop(&min_heap);
    Edge*temp2=g->adjList[target].e.next;
    while(temp2){
        push(&min_heap,temp2);
        temp2=temp2->next;
    }
    }
    else{
    pop(&min_heap);
    }
    }
}
void print_spanning_tree(VertexNode**v,Edge**e,int n,int*sum){
printf("The spanning tree:");
for(int i=0;i<n-1;i++){
    (*sum)+=e[i]->length;
    printf("%d->%d ",e[i]->start,e[i]->end);
}
printf("\n");
printf("The minimum weight is %d\n",(*sum));
}
int main(){
Graph g;
int n,m;
int sum=0;
printf("Please input the numbers of Vertexes and edges\n");
scanf("%d%d",&n,&m);
initial_graph(&g,n,m);
VertexNode**V=(VertexNode**)malloc(sizeof(VertexNode*)*n);
Edge**E=(Edge**)malloc(sizeof(Edge*)*m);
prim(&g,V,E);
print_spanning_tree(V,E,n,&sum);
return 0;
}
```
## ��������
1.**����һ** 
Please input the numbers of Vertexes and edges
8 15
Please input the number of each vertex:
1 2 3 4 5 6 7 8
Please input edges(the indexes of vertexes and the length of edges):
1 2 28 
1 3 2
1 5 1
2 4 9
2 5 8
2 6 10
3 6 24
3 8 27
4 5 5
4 7 8
4 8 7
5 6 26
6 7 8 
6 8 1
7 8 7
The spanning tree:1->5 1->3 5->4 4->8 8->6 8->7 5->2 
The minimum weight is 31
2.**������** 
Please input the numbers of Vertexes and edges
9 20
Please input the number of each vertex:
1 2 3 4 5 6 7 8 9
Please input edges(the indexes of vertexes and the length of edges):
1 2 4
1 4 1
1 5 8
2 3 2
2 4 2
2 5 6
2 6 1
3 5 2
3 6 5
4 5 11
4 7 9
4 8 8
5 6 3
5 7 4
5 8 1
5 9 5
6 8 7
6 9 8
7 8 6
8 9 3
The spanning tree:1->4 4->2 2->6 2->3 3->5 5->8 8->9 5->7
The minimum weight is 16
## �������  
����ͼ�Ķ�����Ϊ n ,����Ϊ m  
1. ���ʼ����ͼ��ʱ�򣬽��ߺ͵���ϵ������ʱ�临�Ӷ�ΪO(m+n) ��  
2. Prim�㷨����������С�ѵ��Ż���ÿ��ѭ��Ѱ���������ǽ����е����б����������ڲ�������С���Ż�������ÿ�������ߵ�ʱ�临�Ӷ�Ϊ O(logm)��ÿ�ζѵ�ά���͵���Ҳ��Ҫ O(logm)��
3. Prim�㷨�У�Ҫ��n-1���������صı߽���������Ҫѭ��n-1�Ρ�
�������������������ʱ�临�Ӷ�Ϊ O(nlogm)��  
