# Lab Week 15. 
## ʵ�����ݣ�Double Hashing
Consider a hash table with M = 26 = 64 bins. Given a 12-bit unsigned number, take the lowest 6 bits as the primary hash function to get bin number, and the highest 6 bits as the secondary 
hash function to get jump size. 
Refer to the example in Lecture 28, Slide 93-107.
(1) Determine the array structures for a hash table
(2) Hash a list of 40 random 12-bit positive numbers
(3) Calculate the average number of probes
##  ʵ���ص��������� 
1. �������Ԫ�ص�λ��Hash_bin�Ľṹ�壬�������ڹ�ϣ���е��±�index,��ŵ�Ԫ��val�����Ƿ�Ϊ�յı�־empty,emptyΪ1ʱ��λ��Ϊ�գ�Ϊ0ʱ��Ϊ�գ��Ƿ�Ϊ�յı�־empty�������Ǻ��������жϴ�λ���Ƿ�Ϊ�գ������Ƿ�Ҫ�����ȥ����Ѱ��������λ��
```c
typedef struct Hash_bin{
    int empty;
    int val;
    int index;
}Hash_bin;
```
2. ������ϣ��Ľṹ�壬����һ�����Ԫ�ص�����,size��¼����Ĵ�С��cur_size��¼��ϣ����Ԫ�ص���Ŀ��probe_time��¼̽�������
```c
typedef struct Hash_table{
    int size;
    int cur_size;
    int probe_time;
    Hash_bin hash_table[Size];
}Hash_table;
```
3. ��ʼ����ϣ����size��Ϊ����Ĵ�С����cur_size��probe_time��ֵΪ0������������λ�õ�empty��־��Ϊ1����ʾ��λ�á�
```c
void initial_hashtable(Hash_table*hashtable){
hashtable->size=Size;
hashtable->probe_time=0;
hashtable->cur_size=0;
for(int i=0;i<hashtable->size;i++){
    hashtable->hash_table[i].empty=1;
    hashtable->hash_table[i].index=i;
}
}
```
4. ȡԪ�صĵ���λ��Ϊ��ϣֵ������λ�������
```c
int hash(int x,Hash_table*hashtable){
    int m=hashtable->size;
    return x&(m-1);
}
```
5. ȡԪ�صĸ���λ��Ϊ��Լ�Ŀ�ȣ�����λ�������
```c
int hash_jump(int x,Hash_table*hashtable){
    int j=(x>>6);
    if(j%2==1)return j;
    else return j|1;

}
```
6. ��ϣ��Ĳ��뺯��������ϣ�������򷵻ء������д˲���������ȡԪ�صĹ�ϣֵ������ϣ���Ԫ�صĹ�ϣֵλ��Ϊ�գ���ֱ�Ӳ��룬��probe_time++��hashtable->cur_size��һ������һ�β����λ�������������õڶ��ι�ϣ�õ�̽��ķ��ȣ������ڳ�ʼ�Ĺ�ϣֵλ�����̽�⣬ֱ��Ѱ�ҵ��˿�λ�ã�ÿ��̽����ǲ���hashtable->cur_size��һ
```c
void insert(int val,Hash_table*hashtable){
if(hashtable->cur_size==hashtable->size){
    printf("The hash table is full!\n");
    return;
}
int m=hashtable->size;
int h=hash(val,hashtable);
while(!hashtable->hash_table[h].empty){
hashtable->probe_time++;
h=(h+hash_jump(val,hashtable))%m;
}
hashtable->hash_table[h].empty=0;
hashtable->hash_table[h].val=val;
hashtable->probe_time++;
hashtable->cur_size++;
}
```
7. ��ӡ�����ϣ���е�����
```c
void print_hashtable(Hash_table*hashtable){
    printf("The hashtable:\n");
    for(int i=0;i<hashtable->size;i++){
        if(hashtable->hash_table[i].empty==0)printf("index:%d   element:%d\n",i,hashtable->hash_table[i].val);
        else printf("index:%d   element:NULL\n");
    }
}
```
## ��������
1. ����һ
Insert 1990
Insert 1737
Insert 1897
Insert 2681
Insert 2219
Insert 1381
Insert 251
Insert 472
Insert 2452
Insert 987
Insert 3907
Insert 581
Insert 3518
Insert 327
Insert 2685
Insert 606
Insert 866
Insert 3281
Insert 1986
Insert 2853
Insert 1671
Insert 3375
Insert 639
Insert 2983
Insert 1562
Insert 364
Insert 2597
Insert 4060
Insert 3904
Insert 3816
Insert 2082
Insert 3791
Insert 747
Insert 161
Insert 529
Insert 1022
Insert 2476
Insert 2992
Insert 3117
Insert 2052
The hashtable: 
index:0   element:3904
index:1   element:NULL
index:2   element:1986
index:3   element:3907
index:4   element:2052
index:5   element:581
index:6   element:1990
index:7   element:327
index:8   element:NULL
index:9   element:1737
index:10   element:NULL
index:11   element:NULL
index:12   element:NULL
index:13   element:1022
index:14   element:2597
index:15   element:3791
index:16   element:NULL
index:17   element:3281
index:18   element:2853
index:19   element:2476
index:20   element:2452
index:21   element:NULL
index:22   element:NULL
index:23   element:NULL
index:24   element:472
index:25   element:NULL
index:26   element:1562
index:27   element:987
index:28   element:4060
index:29   element:NULL
index:30   element:606
index:31   element:NULL
index:32   element:NULL
index:33   element:161
index:34   element:866
index:35   element:529
index:36   element:2082
index:37   element:1381
index:38   element:NULL
index:39   element:2983
index:40   element:3816
index:41   element:1897
index:42   element:NULL
index:43   element:2219
index:44   element:364
index:45   element:3117
index:46   element:NULL
index:47   element:3375
index:48   element:2992
index:49   element:NULL
index:50   element:NULL
index:51   element:1671
index:52   element:NULL
index:53   element:NULL
index:54   element:747
index:55   element:NULL
index:56   element:NULL
index:57   element:2681
index:58   element:NULL
index:59   element:251
index:60   element:NULL
index:61   element:2685
index:62   element:3518
index:63   element:639
The probe time is 53
The average is 1.325000
2. ������
Insert 2160
Insert 3599
Insert 1042
Insert 598
Insert 2000
Insert 771
Insert 2355
Insert 3901
Insert 895
Insert 775
Insert 312
Insert 2871
Insert 3764
Insert 576
Insert 1605
Insert 1066
Insert 2762
Insert 2928
Insert 2975
Insert 1432
Insert 1357
Insert 1472
Insert 3170
Insert 3198
Insert 3157
Insert 1981
Insert 626
Insert 636
Insert 3346
Insert 2976
Insert 2483
Insert 1077
Insert 169
Insert 2258
Insert 893
Insert 1785
Insert 2878
Insert 1711
Insert 3658
Insert 418
The hashtable: 
index:0   element:576
index:1   element:NULL
index:2   element:NULL
index:3   element:771
index:4   element:NULL
index:5   element:1605
index:6   element:NULL
index:7   element:775
index:8   element:NULL
index:9   element:NULL
index:10   element:2762
index:11   element:NULL
index:12   element:418
index:13   element:1357
index:14   element:NULL
index:15   element:3599
index:16   element:2000
index:17   element:NULL
index:18   element:1042
index:19   element:NULL
index:20   element:NULL
index:21   element:3157
index:22   element:598
index:23   element:1472
index:24   element:1432
index:25   element:NULL
index:26   element:2483
index:27   element:NULL
index:28   element:1981
index:29   element:2928
index:30   element:NULL
index:31   element:2975
index:32   element:2976
index:33   element:NULL
index:34   element:3170
index:35   element:NULL
index:36   element:893
index:37   element:NULL
index:38   element:NULL
index:39   element:NULL
index:40   element:NULL
index:41   element:169
index:42   element:1066
index:43   element:2878
index:44   element:NULL
index:45   element:NULL
index:46   element:3658
index:47   element:1711
index:48   element:2160
index:49   element:3346
index:50   element:626
index:51   element:2355
index:52   element:3764
index:53   element:1077
index:54   element:NULL
index:55   element:2871
index:56   element:312
index:57   element:1785
index:58   element:NULL
index:59   element:2258
index:60   element:636
index:61   element:3901
index:62   element:3198
index:63   element:895
The probe time is 64
The average is 1.600000
## ��������
```c
#include<stdio.h>
#include<stdlib.h>
#include<time.h>
#include<math.h>
#define Size 64
typedef struct Hash_bin{
    int empty;
    int val;
    int index;
}Hash_bin;
typedef struct Hash_table{
    int size;
    int cur_size;
    int probe_time;
    Hash_bin hash_table[Size];
}Hash_table;

void initial_hashtable(Hash_table*hashtable){
hashtable->size=Size;
hashtable->probe_time=0;
hashtable->cur_size=0;
for(int i=0;i<hashtable->size;i++){
    hashtable->hash_table[i].empty=1;
    hashtable->hash_table[i].index=i;
}
}
int hash(int x,Hash_table*hashtable){
    int m=hashtable->size;
    return x&(m-1);
}
int hash_jump(int x,Hash_table*hashtable){
    int j=(x>>6);
    if(j%2==1)return j;
    else return j|1;

}
void insert(int val,Hash_table*hashtable){
if(hashtable->cur_size==hashtable->size){
    printf("The hash table is full!\n");
    return;
}
int m=hashtable->size;
int h=hash(val,hashtable);
while(!hashtable->hash_table[h].empty){
hashtable->probe_time++;
h=(h+hash_jump(val,hashtable))%m;
}
hashtable->hash_table[h].empty=0;
hashtable->hash_table[h].val=val;
hashtable->probe_time++;
hashtable->cur_size++;
}
void print_hashtable(Hash_table*hashtable){
    printf("The hashtable:\n");
    for(int i=0;i<hashtable->size;i++){
        if(hashtable->hash_table[i].empty==0)printf("index:%d   element:%d\n",i,hashtable->hash_table[i].val);
        else printf("index:%d   element:NULL\n");
    }
}
int main(){
	srand((unsigned)time(NULL));
    Hash_table hashtable;
    initial_hashtable(&hashtable);
	int n=pow(2,12);
    for(int i=0;i<40;i++){
        int x=rand()%n;
        insert(x,&hashtable);
        printf("Insert %d\n",x);
    }
    print_hashtable(&hashtable);
    printf("The probe time is %d\n",hashtable.probe_time);
    printf("The average is %f\n",(double)(hashtable.probe_time)/hashtable.cur_size);
	return 0;
} 
```
## ������� 
������Ԫ�ظ���Ϊn,hashtable ������Ϊ n��
1. ÿ�ι�ϣ�����Ԫ�ص���������ǵ�һ�ξ�����ϣ�����õ���λ��Ϊ�գ�ֱ�Ӳ��룬ʵ�ֲ����ʱ�临�Ӷ�ΪO(1)�����������ʱ��͸��Ӷ�ΪO(n)�������Ų����Ԫ�ظ���nԽ��Խ��ÿ��ʵ��O(1)����Ŀ�����Խ��ԽС��Խ��Խ���ܷ�����ϣ��ͻ��Ҫ�������ι�ϣ�������ͻ��
2. ����,���� hashtable ������Ϊ n ���� hashtable ֻʣ��һ����λ���Ҹպõ� n ��̽����ҵ�����ʱ��ʱ�临�Ӷ�Ϊ O(n)�� 
3. ����������ÿ�ι�ϣ�����Ԫ�ص�ʱ�临�Ӷ�ΪO(1)��ֻ����󼸴β���Ҳ���ܻ�ﵽO(n)�����ﵽO(n)�Ĳ���ʱ�临�Ӷ�ʱ��������Ҫ�����Ƿ�Ҫ����ϣ�����ݡ�
4. �������������ǵ������Ԫ�ظ���nԽ��Խ��Խ��Խ���ܷ�����ϣ��ͻ�����������ʱ�临�ӶȲ�����1�Ŀ�����Խ��ԽС����ƽ������½ӽ� O(1)��������Ų����Ԫ�ظ���nԽ��Խ����̽��ʱ��ӽ�n��������Ҫ�����Ƿ�Ҫ����ϣ�����ݡ�

