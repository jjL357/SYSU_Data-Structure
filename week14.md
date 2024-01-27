# Lab Week14 Implementation of Dijkstra Algorithm Using min-Heap
## һ��ʵ������
1. Represent a digraph with an Adjacency List
2. Determine the data structures for the process of Dijkstra algorithm using min-Heap
3. Design a C program implementing the Dijkstra algorithm on the digraph using min-Heap
## ʵ���ص�
1. ע���ʼ���ڵ�ʱ��nextָ�븳ֵΪNULL��visited��ֵ 0 ��ʾδ���ʣ���ͼ�ĵ�һ����������Ϊ��ʼ���㣬distance��ʼ��Ϊ0
2. ���õϽ�˹�����㷨��Ѱ�����·����������С�ѣ������нڵ������С���У��Ƚ�ÿ������dsitance,Դ���distance=0��visited=0,ֱ����Դ�����ڵĽڵ��distanceΪֱ�����ӱߵ�Ȩ�أ� while ѭ��ÿ�ν� distance��С��δ�����ʹ��ĵĽ��ȡ��m�������־Ϊ�ѷ��ʣ�����ɨ��ÿһ�������ý��ıߣ�����ߵ���һ�������distance > �ߵ�Ȩ�� + �ö��㵽Դ��ľ���distance��������һ�������distance = weight + �ö��㵽Դ��ľ��룬�������丸�׽ڵ㡣
3. ������С���ҳ�distance��С��δ�����ʵĽڵ㡣
4. ÿ�θı�ڵ��distanceʱ����С���нڵ���Ϣ�п��ܸı䣬����ÿ��ѭ����Ҫ������С�ѡ�
## ���˼·�������� 
1. �����ߡ��ڵ��ڽӱ�ͼ�Ľṹ��
```c
typedef struct Edge{
    int start;
    int end;
    int length; 
    struct Edge*next; 
}Edge;
```
�߰����������յ�����꣬�Լ��ߵ�Ȩֵ
```c
typedef struct Vertex 
{    
    int visited;
    int number;
    int index;
    int distance;
    struct Vertex*parent;
    Edge e;
}VertexNode,AdjList[size];
```
�������˽ڵ�����ƣ��ڵ���±꣬�Ƿ񱻷��ʵı��visited����Դ��ľ���distance�����Ұ����˹���������ڵ�Ϊ���ı�
```c
typedef struct
{   
    int nums_vertex;
    int nums_edges;
    AdjList adjList;
}Graph;
```
ͼ�������ڽӱ�ͱߺͶ��������
2. ������С�Ѻ���С�ѵ�push��pop�͵�����С�ѵ���ز���
```c
typedef struct
{   
  int n;
  VertexNode *heap_Vertex[size];  
}Min_heap;
void initial_heap(Min_heap*min_heap){
    min_heap->n=0;
}
void adjust_min_heap(Min_heap* min_heap,int start,int end){
for(int l=start*2+1;l<=end;l=l*2+1){
    int min_id=l;
    if(l+1<=end && min_heap->heap_Vertex[l]->distance  >min_heap->heap_Vertex[l+1]->distance){
        min_id=l+1;
    }
    if(min_heap->heap_Vertex[min_id]->distance < min_heap->heap_Vertex[start]->distance){
        VertexNode*temp=min_heap->heap_Vertex[min_id];
        min_heap->heap_Vertex[min_id]=min_heap->heap_Vertex[start];
        min_heap->heap_Vertex[start]=temp;
        start=min_id;
    }
    else break;
    l=min_id;
}  
}

VertexNode*front(Min_heap*min_heap){
    if(!empty(min_heap))return min_heap->heap_Vertex[0];
}
void struct_min_heap(Min_heap*min_heap,VertexNode*v,int heap_size){
min_heap->heap_Vertex[heap_size]=v;
heap_size++;
int start=heap_size-1;
for(int i=(start-1)/2;i>=0;i=(start-1)/2){
    if(min_heap->heap_Vertex[start]->distance >=min_heap->heap_Vertex[i]->distance)break;
    else {
        VertexNode*temp=min_heap->heap_Vertex[i];
        min_heap->heap_Vertex[i]=min_heap->heap_Vertex[start];
        min_heap->heap_Vertex[start]=temp;
        start=i;
    }
    if(i==0)break;
}
}
void push(Min_heap*min_heap,VertexNode*v){
struct_min_heap(min_heap,v,min_heap->n);
min_heap->n++;
}
void pop(Min_heap*min_heap){
VertexNode*temp=min_heap->heap_Vertex[0];
min_heap->heap_Vertex[0]=min_heap->heap_Vertex[min_heap->n-1];
adjust_min_heap(min_heap,0,min_heap->n-1);   
min_heap->n--; 
}
int empty(Min_heap*min_heap){
    return min_heap->n==0;
}
```
3. ����ͼ(����ÿ���������� name����ʼ���ڽӱ�j�ڵ�ĳ�ʼ������nextָ�븳ֵΪNULL��visited��ֵ 0 ��ʾδ���ʣ�distance��ʼ��infinite. �ٽ���edge�����ڶ����ڽӱ��nextָ�롣)
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
}```
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
    printf("Please input the number of each vertex\n");
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
    }
}
```
4. ��ӡ���ͼ���ڽӱ�
```c
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
```
5. �Ͻ�˹�����㷨Ѱ�����·��(������С�ѣ������нڵ������С���У��Ƚ�ÿ������dsitance,Դ���distance=0��visited=0,ֱ����Դ�����ڵĽڵ��distanceΪֱ�����ӱߵ�Ȩ�أ� while ѭ��ÿ�ν� distance��С��δ�����ʹ��ĵĽ��ȡ��m�������־Ϊ�ѷ��ʣ�����ɨ��ÿһ�������ý��ıߣ�����ߵ���һ�������distance > �ߵ�Ȩ�� + �ö��㵽Դ��ľ���distance��������һ�������distance = weight + �ö��㵽Դ��ľ��룬�������丸�׽ڵ�,ÿ�ζ�Ҫ���µ�����С�ѡ�  )
```c
void Dijkstra(Graph*g,int s){
    Min_heap m;
    initial_heap(&m);
    int n=g->nums_vertex,i;
    g->adjList[s].distance=0;
    for(i=1;i<=n;i++)push(&m,&g->adjList[i]);
    for(i=1;i<n;i++){
    VertexNode*temp=front(&m);
    while(temp->visited==1){
        pop(&m);
        temp=front(&m);
    }
    temp->visited=1;
    Edge*cur=temp->e.next;
    while(cur){
        if(cur->length+temp->distance < g->adjList[cur->end].distance){
            g->adjList[cur->end].distance= cur->length+temp->distance;
            g->adjList[cur->end].parent=temp;
        }
        cur=cur->next;
    }
    for(int j=m.n-1;j>=0;j--){
    adjust_min_heap(&m,j,m.n-1);
    }
    }
}
```
6. ��ӡ���Դ�㵽�������̾�������·��
```c
void print_distance(Graph*g,int s){
for(int i=1;i<=g->nums_vertex;i++){
    printf("The distance from %d to %d is : %d\n",g->adjList[s].number,g->adjList[i].number,g->adjList[i].distance);
    printf("The road from %d to %d :  ",g->adjList[s].number,g->adjList[i].number);
    VertexNode*temp=&g->adjList[i];
    int m=0;
    int r[size];
    while(temp){
        r[m++]=temp->number;
        temp=temp->parent;
    }
    for(int j=m-1;j>=0;j--){
    printf("%d",r[j]);
    if(j!=0)printf("->");
    }
    printf("\n");
}
}
```
## ��������
1.����һ
Please input the numbers of Vertexes and edges
7 10
Please input the number of each vertex
1 2 3 4 5 6 7
Please input edges(the indexes of vertexes and the length of edges):
1 3 2
1 2 5
2 4 1
3 4 6
3 6 8
2 5 6
4 6 2
4 5 1
6 7 3
5 7 7
1:3->2
2:4->5
3:4->6
4:6->5
5:7
6:7
7:
Please input the index of source vertex:
1
The distance from 1 to 1 is : 0
The road from 1 to 1 :  1
The distance from 1 to 2 is : 5
The road from 1 to 2 :  1->2
The distance from 1 to 3 is : 2
The road from 1 to 3 :  1->3
The distance from 1 to 4 is : 6
The road from 1 to 4 :  1->2->4
The distance from 1 to 5 is : 7
The road from 1 to 5 :  1->2->4->5
The distance from 1 to 6 is : 8
The road from 1 to 6 :  1->2->4->6
The distance from 1 to 7 is : 11
The road from 1 to 7 :  1->2->4->6->7
2. ������
   Please input the numbers of Vertexes and edges
8 15
Please input the number of each vertex
1 2 3 4 5 6 7 8
Please input edges(the indexes of vertexes and the length of edges):
1 2 28
1 5 1
1 3 2
2 6 10
2 4 9
3 6 24
3 8 27
4 5 5
4 7 8
4 8 7
5 6 26
5 2 8
6 7 7
6 8 1
7 8 8
1:2->5->3
2:6->4
3:6->8
4:5->7->8
5:6->2
6:7->8
7:8
8:
Please input the index of source vertex:
1
The distance from 1 to 1 is : 0
The road from 1 to 1 :  1
The distance from 1 to 2 is : 9
The road from 1 to 2 :  1->5->2
The distance from 1 to 3 is : 2
The road from 1 to 3 :  1->3
The distance from 1 to 4 is : 18
The road from 1 to 4 :  1->5->2->4
The distance from 1 to 5 is : 1
The road from 1 to 5 :  1->5
The distance from 1 to 6 is : 19
The road from 1 to 6 :  1->5->2->6
The distance from 1 to 7 is : 26
The road from 1 to 7 :  1->5->2->4->7
The distance from 1 to 8 is : 20
The road from 1 to 8 :  1->5->2->6->8

## ������� 
����ͼ�Ķ�����Ϊ n ,����Ϊ m  
1. ���ʼ����ͼ��ʱ�򣬽��ߺ͵���ϵ������ʱ�临�Ӷ�ΪO(m+n) ��  
2. Dijkstra�㷨����������С�ѵ�Ѱ�ң�ÿ��ѭ��Ѱ���������ǽ����е����е�����������ÿ�������ߵ�ʱ�临�Ӷ�Ϊ O(logn)��ÿ�α����߼���distanceʱ�临�Ӷ�Ϊ O(m),ÿ�θı�ڵ��distanceʱ����С���нڵ���Ϣ�п��ܸı䣬����ÿ��ѭ����Ҫ������С��,ʱ�临�Ӷ�Ϊ O(nlogn)��
3. Dijkstra�㷨�У�Ҫ��n-1���������������Ҫѭ��n-1�Ρ�
�������������������ʱ�临�Ӷ�Ϊ O(n^nlogn)�� 

## �ܽ�������  
1. �ܽ᣺�Ƚ�Prim�㷨��Dijkstra�㷨
Ŀ�ģ�Prim�㷨���������ͨ������Ȩͼ����С�����������⣬�� Dijkstra �㷨�������Դ�㵽Ŀ�������·�����⡣
���̣�Prim �㷨��Ѱ���Ѿ����ʵĵ��δ���ʵĵ��Ȩ����С�ıߣ���prim�㷨��Ѱ��δ������distance��С�ĵ㡣
1. ���ʣ��Ͻ���˹��ÿ�δӶ����ó���С��distance�Ľڵ��������µĽڵ��distance�����ʱ��������distance���ˣ�����ֻ�Ƕѵ�push pop������O��logn���Ĳ�����Ҫ�����������������Ǿ�ҪO(nlogn)���ǻ�����ֱ��ÿ�α�����������С�ģ�ֻҪO(n) ���Ƿ�ʹ�ö�ֱ�ӱ�������죿




