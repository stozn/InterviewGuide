---
layout:  post
category:  hunting_job
title:  面试高频算法真题
tagline:  by 阿秀
tags:
    - 原创
    - 场景题
    - 智力题
    - 校招
    - 阿秀
excerpt: 面试高频算法真题
comment: false
---

<p id="精选高频面试题"></p>

## <h2 align="center">面试高频算法真题</h2>

> 阿秀自己刷过的算法部分经过整理后是按照不同基础、不同人群分类的，如果你不知道自己该看哪个部分的算法题，可以先看一下这里，[戳我直达](/notes/03-hunting_job/03-algorithm/01-basic-algorithm/01-introduce.md)。

以下是本部分正文：

阿秀仅在此中为大家盘点一下互联网大厂面试考察频率比较高的几道手撕算法题，希望我的整理对大家有一点点用处，那我就很高兴了！

说实话，算法这种东西没得快速提升，算法能力的提升需要日积月累慢慢累积而成的。

在互联网招聘中，不管是笔试还是面试中的手撕算法，可以考察的算法题简直不要太多。比如链表、树、数组、动态规划、回溯算法、贪心算法、甚至是拓扑都有可能考察到。

而一般说来笔试的难度是比面试稍微高一些的，面试中的手撕算法难度一般是力扣的 medium 水平，也有一些 easy 的，而笔试至少都是力扣 medium 难度以上的。

<p id="合并有序链表"></p>


## **1、合并有序链表**

[力扣链接](https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)

将两个有序的链表合并为一个新链表，要求新的链表是通过拼接两个链表的节点来生成的。

输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4

~~~cpp
#include <iostream>
using namespace std;

struct myList {
    int val;
    myList* next;
    myList(int _val) :val(_val), next(nullptr) {}
};

myList* merge(myList* l1, myList* l2) {

    if (l1 == nullptr) return l2;
    if (l2 == nullptr) return l1;
    myList head(0);
    myList* node = &head;
    while (l1 != nullptr && l2 != nullptr) {
        if (l1->val < l2->val) {
            node->next = l1;
            l1 = l1->next;

        }
        else {
            node->next = l2;
            l2 = l2->next;
        }
        node = node->next;
    }

    if (l1 == nullptr)
        node->next = l2;
    if (l2 == nullptr)
        node->next = l1;

    return head.next;

};

int main(void) {

    myList* node0 = new myList(0);
    myList* node1 = new myList(1);
    myList* node2 = new myList(2);
    myList* node3 = new myList(3);

    myList* node4 = new myList(1);
    myList* node5 = new myList(4);
    node0->next = node1;
    node1->next = node2;
    node2->next = node3;
    node3->next = nullptr;
    node4->next = node5;
    node5->next = nullptr;

    auto node = merge(node0, node4);
    while (node != nullptr) {
        cout << node->val << endl;
        node = node->next;
    }

    return 0;
}
~~~



<p id="反转链表"></p>


## **2、反转链表**

 定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。 

输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL

### **第一种做法**

~~~cpp
#include<algorithm>
#include<unordered_map>
#include <iostream>
#include<vector>

using namespace std;

struct node {
	int  data;
	struct node* next;
	node(int _data) :data(_data), next(nullptr) {
	}
};

struct node* init() {
	node* head = new node(1);
	node* node1 = new node(2);
	node* node2 = new node(3);
	node* node3 = new node(4);
	node* node4 = new node(5);

	head->next = node1;
	node1->next = node2;
	node2->next = node3;
	node3->next = node4;
	node4->next = nullptr;

	return head;
}

struct node* reverse(node* head) {
	struct node* pre = new node(-1);
	struct node* temp = new node(-1);
	pre = head;
	temp = head->next;
	pre->next = nullptr;	
	struct node* cur = new node(-1);
	cur = temp;
	while (cur != nullptr) {
		temp = cur;
		cur = cur->next;
		temp->next = pre;
		pre = temp;
	}

	return pre;
}

int main(){
	auto head = init();
	head = reverse(head);
	while (head != nullptr) {
		cout << head->data << endl;
		head = head->next;
	}

	return 0;
}
~~~



**第二种做法**

~~~cpp
//头插法来做，将元素开辟在栈上，这样会避免内存泄露
ListNode* ReverseList(ListNode* pHead) {

	// 头插法
	if (pHead == nullptr || pHead->next == nullptr) return pHead;
	ListNode dummyNode = ListNode(0);
	ListNode* pre = &(dummyNode);
	pre->next = pHead;
	ListNode* cur = pHead->next;
	pHead->next = nullptr;
	//pre = cur;
	ListNode* temp = nullptr;
	while (cur != nullptr) {
		temp = cur;
		cur = cur->next;
		temp->next = pre->next;
		pre->next = temp;
	}
	return dummyNode.next;

}

~~~

<p id="单例模式"></p>


## **3、单例模式**

### **饿汉模式**

~~~cpp
class singlePattern {
private:
	singlePattern() {};
	static singlePattern* p;
public:
	static singlePattern* instance();

	class CG {
	public:
		~CG() {
			if (singlePattern::p != nullptr) {
				delete singlePattern::p;
				singlePattern::p = nullptr;
			}
		}
	};
};

singlePattern* singlePattern::p = new singlePattern();
singlePattern* singlePattern::instance() {
	return p;
}
~~~

>update1: instance 手误写成  instacne，微信好友“卷轴”提出，已修正，感谢！- 20210407



### **懒汉模式**

~~~cpp
class singlePattern {
private:
	static singlePattern* p;
	singlePattern(){}
public:
	static singlePattern* instance();
	class CG {
	public:
		~CG() {
			if (singlePattern::p != nullptr) {
				delete singlePattern::p;
				singlePattern::p = nullptr;
			}
		}
	};
};
singlePattern* singlePattern::p = nullptr;
singlePattern* singlePattern::instance() {
	if (p == nullptr) {
		return new singlePattern();
	}
	return p;
}
~~~

<p id="简单工厂模式"></p>


## **4、简单工厂模式**

~~~cpp
typedef enum productType {
	TypeA,
	TypeB,
	TypeC
} productTypeTag;

class Product {

public:
	virtual void show() = 0;
	virtual ~Product() = 0;
};

class ProductA :public Product {
public:
	void show() {
		cout << "ProductA" << endl;
	}
	~ProductA() {
		cout << "~ProductA" << endl;
	}
};

class ProductB :public Product {
public:
	void show() {
		cout << "ProductB" << endl;
	}
	~ProductB() {
		cout << "~ProductB" << endl;
	}
};

class ProductC :public Product {
public:
	void show() {
		cout << "ProductC" << endl;
	}
	~ProductC() {
		cout << "~ProductC" << endl;
	}
};

class Factory {

public:
	Product* createProduct(productType type) {
		switch (type) {
		case TypeA:
			return new ProductA();
		case TypeB:
			return new ProductB();
		case TypeC:
			return new ProductC();
		default:
			return nullptr;
		}
	}
};
~~~

<p id="快速排序"></p>


## **5、快速排序**

~~~cpp
void quickSort(vector<int>& data, int low, int high) {
	//for_each(data.begin(), data.end(), [](const auto a) {cout << a << " "; });
	//cout << endl;
	if (low >= high) return;
	int key = data[low], begin = low, end = high;
	while (begin < end) {
		while (begin<end && data[end]>key) {
			end--;
		}
		if (begin < end) data[begin++] = data[end];

		while (begin<end && data[begin]<= key) {
			begin++;
		}
		if (begin < end) data[end--] = data[begin];


	}

	data[begin] = key;
	quickSort(data, low, begin - 1);
	quickSort(data, begin + 1,high);
}
~~~

<p id="归并排序"></p>


## **6、归并排序**

~~~cpp
void mergeSort(vector<int>& data, vector<int>& copy, int begin, int end) {
	if (begin >= end) return;
	int mid = begin + (end - begin) / 2;
	int low1 = begin, high1 = mid, low2 = mid + 1, high2 = end, index = begin;
	mergeSort(copy, data, low1, high1);
	mergeSort(copy, data, low2, high2);
	while (low1 <= high1 && low2 <= high2) {

		copy[index++] = data[low1] < data[low2] ? data[low1++] : data[low2++];
	}
	while (low1 <= high1) {
		copy[index++] = data[low1++];
	}

	while (low2 <= high2) {
		copy[index++] = data[low2++];
	}
}

void mergeTest() {
	vector<int> nums = { -5,-10,6,5,12,96,1,2,3 };
	vector<int> copy(nums);
	mergeSort(nums, copy, 0, nums.size() - 1);
	nums.assign(copy.begin(), copy.end());
	for_each(nums.begin(), nums.end(), [](const auto& a) {cout << a << " "; });
}
~~~

<p id="实现一个堆排序"></p>


## **7、实现一个堆排序**

堆排序的基本过程：

* 将n个元素的序列构建一个大顶堆或小顶堆
* 将堆顶的元素放到序列末尾
* 将前n-1个元素重新构建大顶堆或小顶堆，重复这个过程，直到所有元素都已经排序

整体时间复杂度为nlogn

```C++
#include<iostream>
#include<vector>
using namespace std;
void swap(vector<int>& arr, int a,int b){
    arr[a]=arr[a]^arr[b];
    arr[b]=arr[a]^arr[b];
    arr[a]=arr[a]^arr[b];
}
void adjust(vector<int>& arr,int len,int index){
    int maxid=index;
    // 计算左右子节点的下标   left=2*i+1  right=2*i+2  parent=(i-1)/2
    int left=2*index+1,right=2*index+2;

    // 寻找当前以index为根的子树中最大/最小的元素的下标
    if(left<len and arr[left]<arr[maxid]) maxid=left;
    if(right<len and arr[right]<arr[maxid]) maxid=right;

    // 进行交换，记得要递归进行adjust,传入的index是maxid
    if(maxid!=index){
        swap(arr,maxid,index);
        adjust(arr,len,maxid);
    }
}
void heapsort(vector<int>&arr,int len){
    // 初次构建堆，i要从最后一个非叶子节点开始，所以是(len-1-1)/2，0这个位置要加等号
    for(int i=(len-1-1)/2;i>=0;i--){
        adjust(arr,len,i);
    }

    // 从最后一个元素的下标开始往前遍历，每次将堆顶元素交换至当前位置，并且缩小长度（i为长度），从0处开始adjust
    for(int i=len-1;i>0;i--){
        swap(arr,0,i);
        adjust(arr,i,0);// 注意每次adjust是从根往下调整，所以这里index是0！
    }
}
int main(){
    vector<int> arr={3,4,2,1,5,8,7,6};

    cout<<"before: "<<endl;
    for(int item:arr) cout<<item<<" ";
    cout<<endl;

    heapsort(arr,arr.size());

    cout<<"after: "<<endl;
    for(int item:arr)cout<<item<<" ";
    cout<<endl;

    return 0;
}
```

<p id="设计缓存"></p>


## **8、设计LRU缓存**

[力扣链接](https://leetcode-cn.com/problems/lru-cache-lcci)

设计和构建一个“最近最少使用”缓存，该缓存会删除最近最少使用的项目。缓存应该从键映射到值(允许你插入和检索特定键对应的值)，并在初始化时指定最大容量。当缓存被填满时，它应该删除最近最少使用的项目。

它应该支持以下操作： 获取数据 get 和 写入数据 put 。

获取数据 get(key) - 如果密钥 (key) 存在于缓存中，则获取密钥的值（总是正数），否则返回 -1。
写入数据 put(key, value) - 如果密钥不存在，则写入其数据值。当缓存容量达到上限时，它应该在写入新数据之前删除最近最少使用的数据值，从而为新的数据值留出空间。

样例

~~~cpp
LRUCache cache = new LRUCache( 2 /* 缓存容量 */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // 返回  1
cache.put(3, 3);    // 该操作会使得密钥 2 作废
cache.get(2);       // 返回 -1 (未找到)
cache.put(4, 4);    // 该操作会使得密钥 1 作废
cache.get(1);       // 返回 -1 (未找到)
cache.get(3);       // 返回  3
cache.get(4);       // 返回  4
~~~



~~~cpp
struct DoubleList {
	int key, val;
	DoubleList* pre, * next;
	DoubleList(int _key,int _val):key(_key),val(_val),pre(nullptr),next(nullptr){ }
};

class LRU {
private:
	int capacity;
	DoubleList* head, * tail;
	unordered_map<int, DoubleList*> memory;
public:
	LRU(int _capacity) {
		this->capacity = _capacity;
		head = new DoubleList(-1, -1);
		tail = new DoubleList(-1, -1);
		head->next = tail;
		tail->pre = head;
	}
	~LRU(){
		if (head != nullptr) {
			delete head;
			head = nullptr;
		}
		if (tail != nullptr) {
			delete tail;
			tail = nullptr;
		}
		for (auto& a : memory) {
			if (a.second != nullptr) {
				delete a.second;
				a.second = nullptr;
			}
		}
	}
	void set(int _key, int _val) {
		if (memory.find(_key) != memory.end()) {
			DoubleList* node = memory[_key];
			removeNode(node);
            node->val = _val;
			pushNode(node);
			return ;
		}
		if (memory.size() == this->capacity) {// 这里很重要，也很爱错，千万记得更新memory
			int topKey = head->next->key;//取得key值，方便在后面删除
			removeNode(head->next);//移除头部的下一个
			memory.erase(topKey);//在memory中删除当前头部的值
		}
		DoubleList* node = new DoubleList(_key, _val);//新增node
		pushNode(node);//放在尾部
		memory[_key] = node;//记得在memory中添加进去
	}
	int get(int _key) {
		if (memory.find(_key) != memory.end()) {
			DoubleList* node = memory[_key];
			removeNode(node);
			pushNode(node);
			return node->val;
		}
		return - 1;
	}

	void removeNode(DoubleList* node) {
		node->pre->next = node->next;
		node->next->pre = node->pre;
	}
	void pushNode(DoubleList* node) {
		tail->pre->next = node;
		node->pre = tail->pre;
		node->next = tail;
		tail->pre = node;
	}
};
~~~

<p id="重排链表"></p>


## **9、重排链表**

[力扣链接](https://leetcode-cn.com/problems/reorder-list/)

给定一个单链表 L：L0→L1→…→Ln-1→Ln ，
将其重新排列后变为： L0→Ln→L1→Ln-1→L2→Ln-2→…

你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

```cpp
示例 1:
给定链表 1->2->3->4, 重新排列为 1->4->2->3.
示例 2:
给定链表 1->2->3->4->5, 重新排列为 1->5->2->4->3.
```



~~~cpp
#include<iostream>
#include<string>
#include<vector>
#include<algorithm>
#include<unordered_map>

using namespace std;

struct ListNode {
	int val;
	ListNode * next;
	ListNode(int _val):val(_val),next(nullptr){}
};

ListNode* myReverseList(ListNode*head) {

	if (head == nullptr || head->next == nullptr) return head;
	ListNode dumyhead(0);
	ListNode* pre = &dumyhead;
	pre->next = head;
	ListNode* cur = head->next;
	head->next = nullptr;
	ListNode* node = new ListNode(-1);
	while (cur != nullptr) {
		node = cur;
		cur = cur->next;
		node->next = pre->next;
		pre->next = node;
	}

	return dumyhead.next;
}

ListNode* myMerge(ListNode* p1, ListNode* p2) {
	if (p1 == nullptr) return p2;
	if (p2 == nullptr) return p1;

	ListNode dumyhead(0);
	ListNode* pre = &dumyhead;
	while (p1 != nullptr && p2 != nullptr) {
		pre->next = p1;
		p1 = p1->next;
		pre = pre->next;
		pre->next = p2;
		p2 = p2->next;
		pre = pre->next;
	}
	if (p1 != nullptr) pre->next = p1;
	return dumyhead.next;

}

ListNode* myReverOrderList(ListNode *head) {
	if (head == nullptr || head->next == nullptr) return head;
	ListNode* slow = head, * fast = head->next;
	while (fast != nullptr && fast->next != nullptr) {
		slow = slow->next;
		fast = fast->next->next;
	}

	ListNode* second = slow->next;
	slow->next = nullptr;
	second = myReverseList(second);
	head = myMerge(head, second);
	return head;
}

int  main() {
	ListNode* head = new ListNode(1);
	ListNode* node1 = new ListNode(2);
	ListNode* node2 = new ListNode(3);
	ListNode* node3 = new ListNode(4);
	ListNode* node4 = new ListNode(5);
	ListNode* node5 = new ListNode(6);

	head->next = node1;
	node1->next = node2;
	node2->next = node3;
	node3->next = node4;
	node4->next = node5;
	node5->next = nullptr;


	head = myReverOrderList(head);
	while (head != nullptr) {
		cout << head->val << endl;
		head = head->next;
	}

	return 0;
}
~~~

<p id="奇偶链表"></p>


## **10、奇偶链表**

[力扣链接](https://leetcode-cn.com/problems/odd-even-linked-list/)

给定一个单链表，把所有的奇数节点和偶数节点分别排在一起。请注意，这里的奇数节点和偶数节点指的是节点编号的奇偶性，而不是节点的值的奇偶性。

请尝试使用原地算法完成。你的算法的空间复杂度应为 O(1)，时间复杂度应为 O(nodes)，nodes 为节点总数。

~~~cpp
示例 1:
输入: 1->2->3->4->5->NULL
输出: 1->3->5->2->4->NULL
示例 2:
输入: 2->1->3->5->6->4->7->NULL 
输出: 2->3->6->7->1->5->4->NULL
~~~

说明:

应当保持奇数节点和偶数节点的相对顺序。
链表的第一个节点视为奇数节点，第二个节点视为偶数节点，以此类推。



### **第一种解法**

~~~cpp
 ListNode* oddEvenList(ListNode* head) {
 if(head==NULL || head->next==NULL)
        {
            return head;
        }
        ListNode* first=head;//奇链表头结点
        ListNode* second=head->next;//偶链表头结点
        ListNode* cur=second;//保存偶链表头结点
        while(second != nullptr && second->next  != nullptr)
        {
            first->next=second->next;
            second->next=first->next->next;
            first=first->next;
            second=second->next;
        }
        first->next=cur;
        return head;
    }

~~~

### **第二种解法**

~~~cpp
ListNode* oddEvenList(ListNode* head) 
{
 if(head == NULL)
    {
        return head;
    }
    
     ListNode* p = head;
     ListNode* q = head->next;
     ListNode* evenhead = q;

    while(q != NULL &&  q->next != NULL)
    {
        p->next = p->next->next;
        p = p->next;
        q->next = q->next->next;
        q = q->next;
    }

    p->next =  evenhead;

    return head;
}
~~~

<p id="三个线程"></p>


## **11、写三个线程交替打印ABC**

```C++
#include<iostream>
#include<thread>
#include<mutex>
#include<condition_variable>
using namespace std;

mutex mymutex;
condition_variable cv;
int flag=0;

void printa(){
    unique_lock<mutex> lk(mymutex);
    int count=0;
    while(count<10){
        while(flag!=0) cv.wait(lk);
        cout<<"thread 1: a"<<endl;
        flag=1;
        cv.notify_all();
        count++;
    }
    cout<<"my thread 1 finish"<<endl;
}
void printb(){
    unique_lock<mutex> lk(mymutex);
    for(int i=0;i<10;i++){
        while(flag!=1) cv.wait(lk);
        cout<<"thread 2: b"<<endl;
        flag=2;
        cv.notify_all();
    }
    cout<<"my thread 2 finish"<<endl;
}
void printc(){
    unique_lock<mutex> lk(mymutex);
    for(int i=0;i<10;i++){
        while(flag!=2) cv.wait(lk);
        cout<<"thread 3: c"<<endl;
        flag=0;
        cv.notify_all();
    }
    cout<<"my thread 3 finish"<<endl;
}
int main(){
    thread th2(printa);
    thread th1(printb);
    thread th3(printc);

    th1.join();
    th2.join();
    th3.join();
    cout<<" main thread "<<endl;


}
```

<p id="涛普问题"></p>


## **12、Top K问题**

**Top K 问题的常见形式：**

>给定10000个整数，找第K大（第K小）的数<br>
> 给定10000个整数，找出最大（最小）的前K个数<br>
>给定100000个单词，求前K词频的单词<br>

解决Top K问题若干种方法

* 使用最大最小堆。求最大的数用最小堆，求最小的数用最大堆。
* Quick Select算法。使用类似快排的思路，根据pivot划分数组。
* 使用排序方法，排序后再寻找top K元素。
* 使用选择排序的思想，对前K个元素部分排序。
* 将1000.....个数分成m组，每组寻找top K个数，得到m×K个数，在这m×k个数里面找top K个数。

1. 使用最大最小堆的思路 （以top K 最大元素为例）<br>
   按顺序扫描这10000个数，先取出K个元素构建一个大小为K的最小堆。每扫描到一个元素，如果这个元素大于堆顶的元素（这个堆最小的一个数），就放入堆中，并删除堆顶的元素，同时整理堆。如果这个元素小于堆顶的元素，就直接pass。最后堆中剩下的元素就是最大的前Top K个元素，最右的叶节点就是Top 第K大的元素。

>note：最小堆的插入时间复杂度为log(n)，n为堆中元素个数，在这里是K。最小堆的初始化时间复杂度是nlog(n)

C++中的最大最小堆要用标准库的priority_queue来实现。

```C++
struct Node {
    int value;
    int idx;
    Node (int v, int i): value(v), idx(i) {}
    friend bool operator < (const struct Node &n1, const struct Node &n2) ; 
};

inline bool operator < (const struct Node &n1, const struct Node &n2) {
    return n1.value < n2.value;
}

priority_queue<Node> pq; // 此时pq为最大堆
```

2. 使用Quick Select的思路（以寻找第K大的元素为例）<br>
   Quick Select脱胎于快速排序，提出这两个算法的都是同一个人。算法的过程是这样的：
   首先选取一个枢轴，然后将数组中小于该枢轴的数放到左边，大于该枢轴的数放到右边。
   此时，如果左边的数组中的元素个数大于等于K，则第K大的数肯定在左边数组中，继续对左边数组执行相同操作；
   如果左边的数组元素个数等于K-1，则第K大的数就是pivot；
   如果左边的数组元素个数小于K，则第K大的数肯定在右边数组中，对右边数组执行相同操作。

这个算法与快排最大的区别是，每次划分后只处理左半边或者右半边，而快排在划分后对左右半边都继续排序。

```java
//此为Java实现
public int findKthLargest(int[] nums, int k) {
  return quickSelect(nums, k, 0, nums.length - 1);
}

// quick select to find the kth-largest element
public int quickSelect(int[] arr, int k, int left, int right) {
  if (left == right) return arr[right];
  int index = partition(arr, left, right);
  if (index - left + 1 > k)
    return quickSelect(arr, k, left, index - 1);
  else if (index - left + 1 == k)
    return arr[index];
  else
    return quickSelect(arr, k - (index - left + 1), index + 1, right);

}
```

3. 使用选择排序的思想对前K个元素排序 （ 以寻找前K大个元素为例）<br>
   扫描一遍数组，选出最大的一个元素，然后再扫描一遍数组，找出第二大的元素，再扫描一遍数组，找出第三大的元素。。。。。以此类推，找K个元素，时间复杂度为O(N*K)

<p id ="布隆过滤器原理与优点"></p>


## **13、布隆过滤器原理与优点**

布隆过滤器是一个比特向量或者比特数组，它本质上是一种概率型数据结构，用来查找一个元素是否在集合中，支持高效插入和查询某条记录。常作为针对超大数据量下高效查找数据的一种方法。

**它的具体工作过程是这样子的：**

假设布隆过滤器的大小为m（比特向量的长度为m），有k个哈希函数，它对每个数据用这k个哈希函数计算哈希，得到k个哈希值，然后将向量中相应的位设为1。

在查询某个数据是否存在的时候，对这个数据用k个哈希函数得到k个哈希值，再在比特向量中相应的位查找是否为1，如果某一个相应的位不为1，那这个数据就肯定不存在。但是如果全找到了，则这个数据有可能存在。

**为什么说有可能存在呢？**

因为不同的数据经过哈希后可能有相同的哈希值，在比特向量上某个位置查找到1也可能是由于某个另外的数据映射得到的。

**支持删除操作吗**

目前布隆过滤器只支持插入和查找操作，不支持删除操作，如果要支持删除，就要另外使用一个计数变量，每次将相应的位置为1则计数加一，删除则减一。

布隆过滤器中哈希函数的个数需要选择。如果太多则很快所有位都置为1，如果太少会容易误报。

