#pragma once
#include<iostream>
using namespace std;
struct Nums {											//链表结点类定义
	int data;											//链表结点数据
	Nums* lLink,*rLink;									//前驱，后继指针
	Nums(Nums* left = NULL, Nums* right = NULL)			//构造函数
	{
		lLink = left; rLink = right;
	}
	Nums(int na, Nums* left = NULL,Nums* right=NULL)	//构造函数
	{
		data = na; lLink = left; rLink = right;
	}
};

class Big_Data {										//大数运算的类定义
public:
	Big_Data();											//构造函数
	Big_Data(Big_Data& B);								//复制构造函数
	Nums* GetHead()const { return first; }				//获得双向循环链表的头指针
	Nums* GetLast()const {								//获得双向循环链表的尾指针
		return last;
	}									
	void Factor(int n);									//大数阶乘运算
	void Print();										//将大数阶乘的结果显示到屏幕上
private:
	Nums* first;										//双向链表的头指针
	Nums* last;											//双向链表的尾指针
};
Big_Data::Big_Data() {
	//构造函数，在头指针后增加新的结点，结点的数据为1。
	first = new Nums;
	if (first == NULL) { cerr << "存储分配出错！" << endl; exit(1); }
	Nums* newNode = new Nums(1);						//在头指针后插数据为1的结点
	first->rLink = first->lLink = newNode;
	newNode->rLink = newNode->lLink = first;
	last = newNode;										//尾指针设置为newNode
}
Big_Data::Big_Data(Big_Data& B) {
	//复制构造函数，用已有的大数对象B初始化当前大数。
	first = new Nums;
	if (first == NULL) { cerr << "存储分配出错！" << endl; exit(1); }
	Nums* destptr = first->rLink, * srcptr = B.GetHead()->rLink;
	while (srcptr != NULL) {
		destptr->data = srcptr->data;
		destptr->lLink = srcptr->lLink;
		destptr->rLink = srcptr->rLink;
		srcptr = srcptr->rLink;
	}
}
void Big_Data::Factor(int n) {
	//阶乘运算，输入n，从而计算n的阶乘
	int flag;											//flag设置为低位的进位
	for (int i = 1; i <= n; ++i) {
		flag = 0;										//每次循环设置为0
		Nums* current = first->rLink;
		while (current != first) {
			current->data = current->data * i + flag;	//当前的值乘i加上低位的进位flag就是当前结点应该取的值
			if (current->data > 999 && current->rLink == first) {
				//如果当前的数值大于999必须进位，如果后续结点为空需要添加一个新结点
				Nums* newNode = new Nums(0);			//新结点数据设为0
				newNode->rLink = current->rLink;
				current->rLink = newNode;
				newNode->lLink = first->lLink;
				first->lLink = newNode;
				last = newNode;							//last更新
			}
			flag = current->data / 1000;				//当前结点的数值对一千取商即为flag的值
			current->data = current->data % 1000;		//当前结点的数值对一千取余即为该结点应取的值
			current = current->rLink;					//切换到下一个结点，继续乘i
		}
	}
}
void Big_Data::Print() {
	//将阶乘所得的大数输出到屏幕上
	Nums* Last = GetLast();								//获取尾指针
	cout << Last->data;									//首先将尾指针的数据输出
	Nums* ptr = Last->lLink;
	while (ptr->lLink != Last) {						//利用双向链表反向遍历
		cout.fill('0');									//设置填充字符
		cout.width(3);									//设置域宽
		cout << ptr->data;								//将前面的结点的数据依次输出
		ptr = ptr->lLink;
	}
}
