# Lab Week 10. 
## ʵ�����ݣ�Array Implementation of min-Heaps
A complete min-heap is a min-heap with the structure of a 
complete binary tree which can be stored using an array.
(1) Determine the data structures for a complete min-heap
(2) Construct a complete min-heap of 20 (or more) random numbers 
by PUSH the numbers into the min-heap one by one
(3) POP the numbers out of the min-heap
## ʵ���ص�
ʵ��Ҫ��  
1. ÿ����һ�����������push������뵽��С���е����һλ������С���������Ԫ����������������丸�׽���ֵ���бȽϵ����� 
2. push������ÿ����һ������������С���������Ԫ�����Ƚ�����ڶ�������ײ㵱�������Ԫ���������������е����һλ��Ȼ�����丸�׽ڵ��ֵ���бȽϣ������ڸ��׽ڵ㣬����������丸�׽ڵ��ֵ���н�������С�ڵ�����ֹͣ��ֱ�����Ϊ���ڵ㡣
3. pop�����н��������ĸ��ڵ�Ӷ�����ɾ�������պ󽫶����������һ�����ȡ�����ŵ��������(������������һλ�ŵ���һλ��Ȼ����������ֵ���бȽϣ���ֵ����������������ֵ��С����������,�����ٴ�������������һ����������ֵ��С������ֵ������Ȼ����������������ظ��˲�����ֱ����Ϊ���һλ��    
4. ���׽ڵ������������±��ϵ:��-->��:i--->����:2*i+1,�Һ���:2*i+2;��-->��:i--->(i-1)/2��
## ���˼·��������
1. push������ÿ����һ������������С���������Ԫ�����Ƚ�����ڶ�������ײ㵱�������Ԫ���������������е����һλ��Ȼ�����丸�׽ڵ��ֵ���бȽϣ������ڸ��׽ڵ㣬����������丸�׽ڵ��ֵ���н�������С�ڵ�����ֹͣ��ֱ�����Ϊ���ڵ㡣
```c
void struct_min_heap(int*min_heap,int val,int heap_size){
min_heap[heap_size]=val;
heap_size++;
int start=heap_size-1;
for(int i=(start-1)/2;i>=0;i=(start-1)/2){
    if(min_heap[start]>=min_heap[i])break;
    else {
        int temp=min_heap[start];
        min_heap[start]=min_heap[i];
        min_heap[i]=temp;
        start=i;
    }
    if(i==0)break;
}
}
void min_heap_sort(int*min_heap,int heap_size){
   int end=heap_size-1;
   for(int i=(end-1)/2;i>=0;i--){
    adjust_min_heap(min_heap,i,heap_size-1);
    
   } 
}
void push(int*min_heap,int val,int *heap_size){
struct_min_heap(min_heap,val,*heap_size);
(*heap_size)++;
}
```
2. pop�����н��������ĸ��ڵ�Ӷ�����ɾ�������պ󽫶����������һ�����ȡ�����ŵ��������(������������һλ�ŵ���һλ��Ȼ����������ֵ���бȽϣ���ֵ����������������ֵ��С����������,�����ٴ�������������һ����������ֵ��С������ֵ������Ȼ����������������ظ��˲�����ֱ����Ϊ���һλ��
```c
void adjust_min_heap(int* min_heap,int start,int end){
for(int l=start*2+1;l<=end;l=l*2+1){
    int min_id=l;
    if(l+1<=end&&min_heap[l]>min_heap[l+1]){
        
        min_id=l+1;
    } 
    if(min_heap[min_id]<min_heap[start]){
        int temp=min_heap[start];
        min_heap[start]=min_heap[min_id];
        min_heap[min_id]=temp;
        start=min_id;
    }
    else break;
    l=min_id;
}
void pop(int*min_heap,int *heap_size){
min_heap[0]=min_heap[*heap_size-1];
adjust_min_heap(min_heap,0,*heap_size-1);   
(*heap_size)--; 
}
```  
## ��������
```c
#include<stdio.h>
#include<stdlib.h>
#include<time.h>
#include<math.h>
#include<string.h>
#define size 100
void print_heap(int*heap,int heap_size){
printf("The heap: \n");
for(int i=0;i<heap_size;i++)printf("%d ",heap[i]);
printf("\n");
}
void adjust_min_heap(int* min_heap,int start,int end){
for(int l=start*2+1;l<=end;l=l*2+1){
    int min_id=l;
    if(l+1<=end&&min_heap[l]>min_heap[l+1]){
        
        min_id=l+1;
    }   
    if(min_heap[min_id]<min_heap[start]){
        int temp=min_heap[start];
        min_heap[start]=min_heap[min_id];
        min_heap[min_id]=temp;
        start=min_id;
    }
    else break;
    l=min_id;
}  
}
void struct_min_heap(int*min_heap,int val,int heap_size){
min_heap[heap_size]=val;
heap_size++;
int start=heap_size-1;
for(int i=(start-1)/2;i>=0;i=(start-1)/2){
    if(min_heap[start]>=min_heap[i])break;
    else {
        int temp=min_heap[start];
        min_heap[start]=min_heap[i];
        min_heap[i]=temp;
        start=i;
    }
    if(i==0)break;
}
}
void min_heap_sort(int*min_heap,int heap_size){
   int end=heap_size-1;
   for(int i=(end-1)/2;i>=0;i--){
    adjust_min_heap(min_heap,i,heap_size-1);
    
   } 
}
void push(int*min_heap,int val,int *heap_size){
struct_min_heap(min_heap,val,*heap_size);
(*heap_size)++;
}
void pop(int*min_heap,int *heap_size){
min_heap[0]=min_heap[*heap_size-1];
adjust_min_heap(min_heap,0,*heap_size-1);   
(*heap_size)--; 
}
int main(){
    int min_heap[size];
	srand((unsigned)time(NULL));
    int heap_size=0;
    for(int i=0;i<20;i++){
    int x=rand()%100;
    printf("Push:%d\n",x);
    push(min_heap,x,&heap_size);
    print_heap(min_heap,heap_size);
    }
    for(int i=0;i<20;i++){
    printf("Pop:%d\n",min_heap[0]);
    pop(min_heap,&heap_size);
    print_heap(min_heap,heap_size);
    }    
	return 0;
} 
```
## ��������
1. 
**���**��
Push:2
The heap: 2 
Push:43
The heap: 2 43 
Push:67
The heap: 2 43 67 
Push:19
The heap: 2 19 67 43 
Push:68
The heap: 2 19 67 43 68 
Push:20
The heap: 2 19 20 43 68 67 
Push:87
The heap: 2 19 20 43 68 67 87 
Push:16
The heap: 2 16 20 19 68 67 87 43 
Push:23
The heap: 2 16 20 19 68 67 87 43 23 
Push:83
The heap: 2 16 20 19 68 67 87 43 23 83 
Push:92
The heap: 2 16 20 19 68 67 87 43 23 83 92 
Push:37
The heap: 2 16 20 19 68 37 87 43 23 83 92 67 
Push:53
The heap: 2 16 20 19 68 37 87 43 23 83 92 67 53 
Push:32
The heap: 2 16 20 19 68 37 32 43 23 83 92 67 53 87 
Push:12
The heap: 2 16 12 19 68 37 20 43 23 83 92 67 53 87 32 
Push:74
The heap: 2 16 12 19 68 37 20 43 23 83 92 67 53 87 32 74 
Push:33
The heap: 2 16 12 19 68 37 20 33 23 83 92 67 53 87 32 74 43 
Push:69
The heap: 2 16 12 19 68 37 20 33 23 83 92 67 53 87 32 74 43 69 
Push:68
The heap: 2 16 12 19 68 37 20 33 23 83 92 67 53 87 32 74 43 69 68 
Push:36
The heap: 2 16 12 19 36 37 20 33 23 68 92 67 53 87 32 74 43 69 68 83 
Pop:2
The heap: 12 16 20 19 36 37 32 33 23 68 92 67 53 87 83 74 43 69 68 
Pop:12
The heap: 16 19 20 23 36 37 32 33 68 68 92 67 53 87 83 74 43 69 
Pop:16
The heap: 19 23 20 33 36 37 32 43 68 68 92 67 53 87 83 74 69 
Pop:19
The heap: 20 23 32 33 36 37 69 43 68 68 92 67 53 87 83 74 
Pop:20
The heap: 23 33 32 43 36 37 69 74 68 68 92 67 53 87 83 
Pop:23
The heap: 32 33 37 43 36 53 69 74 68 68 92 67 83 87 
Pop:32
The heap: 33 36 37 43 68 53 69 74 68 87 92 67 83 
Pop:33
The heap: 36 43 37 68 68 53 69 74 83 87 92 67 
Pop:36
The heap: 37 43 53 68 68 67 69 74 83 87 92 
Pop:37
The heap: 43 68 53 74 68 67 69 92 83 87 
Pop:43
The heap: 53 68 67 74 68 87 69 92 83 
Pop:53
The heap: 67 68 69 74 68 87 83 92 
Pop:67
The heap: 68 68 69 74 92 87 83 
Pop:68
The heap: 68 74 69 83 92 87 
Pop:68
The heap: 69 74 87 83 92 
Pop:69
The heap: 74 83 87 92 
Pop:74
The heap: 83 92 87 
Pop:83
The heap: 87 92 
Pop:87
The heap: 92 
Pop:92
The heap:
2. 
**���**
Push:1
The heap: 1 
Push:64
The heap: 1 64 
Push:95
The heap: 1 64 95 
Push:12
The heap: 1 12 95 64 
Push:77
The heap: 1 12 95 64 77 
Push:54
The heap: 1 12 54 64 77 95 
Push:52
The heap: 1 12 52 64 77 95 54 
Push:68
The heap: 1 12 52 64 77 95 54 68 
Push:95
The heap: 1 12 52 64 77 95 54 68 95 
Push:13
The heap: 1 12 52 64 13 95 54 68 95 77 
Push:97
The heap: 1 12 52 64 13 95 54 68 95 77 97 
Push:56
The heap: 1 12 52 64 13 56 54 68 95 77 97 95 
Push:21
The heap: 1 12 21 64 13 52 54 68 95 77 97 95 56 
Push:50
The heap: 1 12 21 64 13 52 50 68 95 77 97 95 56 54 
Push:94
The heap: 1 12 21 64 13 52 50 68 95 77 97 95 56 54 94 
Push:2
The heap: 1 2 21 12 13 52 50 64 95 77 97 95 56 54 94 68 
Push:65
The heap: 1 2 21 12 13 52 50 64 95 77 97 95 56 54 94 68 65 
Push:5
The heap: 1 2 21 5 13 52 50 64 12 77 97 95 56 54 94 68 65 95 
Push:85
The heap: 1 2 21 5 13 52 50 64 12 77 97 95 56 54 94 68 65 95 85 
Push:45
The heap: 1 2 21 5 13 52 50 64 12 45 97 95 56 54 94 68 65 95 85 77 
Pop:1
The heap: 2 5 21 12 13 52 50 64 77 45 97 95 56 54 94 68 65 95 85 
Pop:2
The heap: 5 12 21 64 13 52 50 65 77 45 97 95 56 54 94 68 85 95 
Pop:5
The heap: 12 13 21 64 45 52 50 65 77 95 97 95 56 54 94 68 85 
Pop:12
The heap: 13 45 21 64 85 52 50 65 77 95 97 95 56 54 94 68 
Pop:13
The heap: 21 45 50 64 85 52 54 65 77 95 97 95 56 68 94 
Pop:21
The heap: 45 64 50 65 85 52 54 94 77 95 97 95 56 68 
Pop:45
The heap: 50 64 52 65 85 56 54 94 77 95 97 95 68 
Pop:50
The heap: 52 64 54 65 85 56 68 94 77 95 97 95 
Pop:52
The heap: 54 64 56 65 85 95 68 94 77 95 97 
Pop:54
The heap: 56 64 68 65 85 95 97 94 77 95 
Pop:56
The heap: 64 65 68 77 85 95 97 94 95 
Pop:64
The heap: 65 77 68 94 85 95 97 95 
Pop:65
The heap: 68 77 95 94 85 95 97 
Pop:68
The heap: 77 85 95 94 97 95 
Pop:77
The heap: 85 94 95 95 97 
Pop:85
The heap: 94 95 95 97 
Pop:94
The heap: 95 97 95 
Pop:95
The heap: 95 97 
Pop:95
The heap: 97 
Pop:97
The heap:
## �������
�����ĸ߶�Ϊh,����ڵ�ĸ���Ϊn��
1. ���� pop �� push ��������������Ҫ�Ӹ���������Ҷ�ӽڵ�,���ߴ�Ҷ�ӽڵ���������ڵ㣬ʱ�临�Ӷ�Ϊ O(h)��Ҳ���� O(lnn)�����һ��ʼ��ʼ��һ����СΪ n ��С���ѣ�Ҳ����Ҫ�������� n �β����������ʱ�临�Ӷ�Ϊ O(nln(n)).
1. ÿ��ִ��push��pop�������������ǴӸ���������Ҷ�ӽڵ�,���ߴ�Ҷ�ӽڵ���������ڵ㣬����Ҫ��ʱ�临�Ӷ�ΪO(n logn)��
2. ��Ϊÿ�β����ɾ����Ҫִ��push����pop,����ʱ�临�Ӷ�ΪO(n log n)��
3. �������������������ʱ�临�Ӷ�ΪO(n log n)��