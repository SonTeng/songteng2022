# C语言常用代码模板

##### 1.VS中写代码的格式

```C
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<stdlib.h>

int main()
{
    printf("hello world!\n");
	system("pause");
	return 0;
}


/**
*说明：
*#define _CRT_SECURE_NO_WARNINGS  是为了避免VS提示更安全的函数来替换标准C函数。
*#include<stdio.h>   是为了输入输出打印 即printf函数。
*#include<stdlib.h>  是为了使用system函数，system("pause");可以避免程序执行时黑窗口一闪而过。
*    system("mspaint"); //启动win下的画图板
*    system("D:\\Mp3tag\\Mp3tag.exe"); //成功打开该程序
*    system("D:/DevCpp/DevCpp.exe");//成功打开该程序
*/
```



##### 2.交换两个数的函数实现--以int为例

```C
void swap_two_nums(int *a, int *b)
{
    int temp;
    temp = *a;
    *a = *b;
    *b = temp;
}

//变量调用
int num1,num2;
swap_two_nums(&num1, &num2);

//数组元素调用
int arr[] = {1,2,3,4,5}
swap_two_nums(&arr[j], &arr[i]);

```



##### 3.一维数组和二维数组打印

一维数组：

```C
//一维数组打印
void printArr(int num[], int len)
{
    int i;
    for (i = 0; i < len; i++)
    {
        printf("%-3d", num[i]);
    }
    printf("\n");
}
//调用方法
int a[] = { 0,9,1,8,1,9,2,7,3,6,4,5 };
int len = sizeof(a) / sizeof(a[0]);
printAarr(a, len);
```

二维数组：

```C
//二维数组打印
void printMatrix(int* a, int m, int n)//a为一个m行，n列的二维数组的首地址。
{
    int i, j;
    for (i = 0; i < m; i++)
    {
        for (j = 0; j < n; j++)
        {
            printf("%d,", a[i * n + j]);//a[i*n+j]也就是原始二维数组第i行第j列的元素。
        }
        printf("\n");//每行结束输出换行。
    }
}

//调用方法
//求元素个数，行数，列数
int a[3][4] = { {1,2,3,4},{5,6,7,8},{9,10,11,12} };
int len = sizeof(a) / sizeof(a[0][0]);//元素的个数
int row_nums = sizeof(a) / sizeof(a[0]);//行数  = 二维数组总大小除以 一行的大小
int col_nums = sizeof(a[0]) / sizeof(a[0][0]);//列 = 行大小除以 一个元素的大小

printMatrix(a, row_nums, col_nums);


    
```



##### 4.fgets的使用模板

去掉因按回车键而自动读取的\n

```C
//#include<string.h>
char buf[1024] = "";
fgets(buf,sizeof(buf),stdin);
buf[strlen(buf)-1]=0;
printf("%s\n",buf);
```



##### 5.键盘输入字符串，大小写字母互转，其他不变

```C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
int main(int argc, char const *argv[])
{
	char buf[64] = "";
	printf("请输入一段小于64字节的字符串\n");
	fgets(buf, sizeof(buf), stdin);

	int len = strlen(buf);
	for (int i = 0; i < len; ++i)
	{
		if(buf[i]>='a' && buf[i]<='z')
		{
			buf[i] -= 32;
		}
		else if(buf[i]>='A' && buf[i]<='Z')
		{
			buf[i] += 32;
		}

		printf("%c", buf[i]);
	}
	printf("\n");

	return 0;
}
```

##### 6.终端游戏开发：猜数字，猜测系统随机给出一个四位数。

```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
void start_game(void);
void game_cmd(void)
{
	printf("按1 进入游戏\n");
	printf("按2 查看游戏规则\n");
	printf("按0 退出游戏\n");
}
void game_rule(void)
{
	printf("猜数字:系统会随机产生一个四位数，在不同难度\n");
	printf("下你有相应的次数去猜测这个数字。系统会给提示\n");
	printf("信息，在规定次数内猜对即获胜。\n");

	int cmd = 0;
	printf("按1 进入游戏\n");
	printf("按0 退出游戏\n");
	scanf("%d", &cmd);
	getchar();

	switch (cmd)
	{
	case 1:
		start_game();
		break;
	case 0:
		exit(-1);
	}
}
void start_game(void)
{
	//四位随机数
	int dst[4] = { 0 };
	int lev[3] = { 10,7,5 };
	for (int i = 0; i < sizeof(dst) / sizeof(dst[0]); ++i)
	{
		dst[i] = rand() % 10;//获取一位数 123%10==3
	}

	printf("请选择游戏难度级别：1(10次)、2(7次)、3(5次)\n");
	int level = 0;
	scanf("%d", &level);
	getchar();

	printf("Lev.%d\n", level);
	printf("开始游戏，请输入数值:\n");


	for (int i = 0; i < lev[level - 1]; ++i)
	{
		int flag = 0;
		int data = 0;
		scanf("%d", &data);
		getchar();

		//提取输入数据的每个位
		int src[4] = { 0 };
		for (int j = sizeof(src) / sizeof(src[0]) - 1; j >= 0; --j)
		{
			src[j] = data % 10;
			data = data / 10;
		}

		//src与dst逐位比较
		for (int j = 0; j < sizeof(src) / sizeof(src[0]); ++j)
		{
			if (src[j] == dst[j])
			{
				printf("第%d位相等\n", j + 1);
				flag++;
			}
			else if (src[j] < dst[j])
			{
				printf("第%d位小于正确数\n", j + 1);
			}
			else if (src[j] > dst[j])
			{
				printf("第%d位大于正确数\n", j + 1);
			}

		}

		if (flag == 4)//正确
		{
			printf("你输入正确，任意键退出\n");
			getchar();
			exit(-1);
		}
		if (i + 1 < lev[level - 1])
			printf("再次输入数据\n");

	}


	printf("失败了\n");
	return;

}
int main(int argc, char const* argv[])
{

	//设置随机数种子
	srand(time(NULL));
	while (1)
	{
		game_cmd();
		int cmd = 0;
		scanf("%d", &cmd);
		getchar();//去掉回车

		switch (cmd)
		{
		case 0:
			return 0;
		case 1:
			//进入游戏
			start_game();
			break;
		case 2:
			game_rule();
			break;
		}
	}

	return 0;
}

```

##### 7.比较两个字符串

```C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
int main(int argc, char const *argv[])
{
	printf("比较两字符串的大小\n");
	printf("请输入字符串1：");
	char str1[64] = "";
	fgets(str1,sizeof(str1), stdin);
	str1[strlen(str1)-1] = 0;//去掉字符中回车
	
	printf("请输入字符串2：");
	char str2[64] = "";
	fgets(str2,sizeof(str2), stdin);
	str2[strlen(str2)-1] = 0;//去掉字符中回车

	int flag = 0;
	for (int i = 0; (str1[i] !=0) || (str2[i] !=0); ++i)
	{
		if(str1[i] < str2[i])
		{
			printf("字符串1小于字符串2\n");
			flag = 1;
			break;
		}
		else if(str1[i] > str2[i])
		{
			printf("字符串1大于字符串2\n");	
			flag = 1;
			break;
		}
	}

	if(flag == 0)
	{
		printf("字符串1等于字符串2\n");
	}
	return 0;
}
```



##### 8.复制字符串--指针实现1

```C
#include <stdio.h>
#include<string.h>

void my_strcpy(char dest[],char src[])
{
    char *p = src;
    char *q = dest;

    while(*p != '\0')
    {
        *q++ = *p++;
    }

    *q = '\0';
}

int main()
{
    char src[100] = "hello world!";
    char dest[100];

    my_strcpy(dest,src);

    printf("src  = %s\n", src);
    printf("dest = %s\n", dest);

    return 0;
}
```



##### 9.字符串的拷贝--3种方式

```C
//字符串拷贝实现
//1、利用[] 实现
void copyString01(char * dest , char * src)
{
	int len =strlen(src);
	for (int i = 0; i < len;i++)
	{
		dest[i] = src[i];
	}
	dest[len] = '\0';
}

//2、利用字符串指针
void copyString02(char * dest, char * src)
{
	while (*src != '\0')
	{
		*dest = *src;

		dest++;
		src++;
	}
	*dest = '\0';
}

//3
void copyString03(char * dest, char * src)
{
	while (*dest++ = *src++){}
}

void test02()
{
	char * str = "hello world";
	
	char buf[1024];

	//copyString01(buf, str);
	//copyString02(buf, str);
	copyString03(buf, str);
	printf("%s\n", buf);
}
```



##### 10.字符串翻转

```C
//字符串翻转
void reverseString01(char * str)
{
	//利用[]
	int len = strlen(str);
	int start = 0;
	int end = len - 1;

	while (start < end)
	{
		char temp = str[start];
		str[start] = str[end];
		str[end] = temp;

		start++;
		end--;
	}

}

void reverseString02(char * str)
{
	int len = strlen(str);
	char * start = str;
	char * end = str + len - 1;

	while (start < end)
	{
		char temp = *start;
		*start = *end;
		*end = temp;
		start++;
		end--;
	}
}

void test03()
{
	char str[] = "abcdefg";

	//reverseString01(str);
	reverseString02(str);
	printf("%s\n",str);
}
```



##### 11.链表的基本使用 -- 13个函数，初始化（头插、尾插）、遍历打印、获取元素总个数、插入（按值前插/后插、按位置）、删除（按值、按位置）、逆置、清空、销毁

示例代码：三个文件，linklist.h、linklist.c 和Demo.c

linklist.h代码：

```C
#pragma once

#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

//定义结点
struct LinkNode
{
	int num;
	struct LinkNode* next;
};

//01.初始化链表：尾插法：可以使遍历链表得出的数据和输入的数据顺序相同
struct LinkNode* initLinkListTail();

//02.初始化链表：头插法：可以使遍历链表得出的数据和输入的数据顺序相反
struct LinkNode* initLinkListHead();

//03.遍历链表，并打印输出
void foreach_LinkList(struct LinkNode* pHeader);

//04.获取链表长度，获取链表中元素个数
int getElementNums(struct LinkNode* pHeader);

//05.获取链表中某位置对应的结点的值，假设头结点为第0个结点，pos为链表中第pos个结点
int getValueByPos(struct LinkNode* pHeader, int pos);

//06.向链表插入新结点（根据结点值）：到现存结点前面或末尾
void insert_LinkListByValueBefore(struct LinkNode* pHeader, int oldVal, int newVal);

//07.向链表插入新结点（根据结点值）：到现存结点后面或末尾，尾插法
void insert_LinkListByValueAfter(struct LinkNode* pHeader, int oldVal, int newVal);

//08.向链表插入新节点（根据给定位置）：位置需要为大于0的数
void insert_LinkListByPos(struct LinkNode* pHeader, int pos, int newVal);

//09.删除链表（根据给定结点值）：删除链表中某个结点
void delete_LinkListByValue(struct LinkNode* pHeader, int delVal);

//10.删除链表（根据给定位置）：删除链表中某个结点
void delete_LinkListByPos(struct LinkNode* pHeader, int pos);

//11.反转链表：逆置
void reverse_LinkList(struct LinkNode* pHeader);

//12.清空链表（可以复用）：清空各结点数据域，保留指针域，保留头结点
void clear_LinkList(struct LinkNode* pHeader);

//13.销毁链表：头结点和之后所有结点全部删除，一个不留
void destroy_LinkList(struct LinkNode* pHeader);

```

linklist.c代码如下：

```C
#include "linklist.h"



//01.尾插法：初始化链表，函数返回值是创建好的链表的头结点
struct LinkNode* initLinkListTail()
{
	struct LinkNode* pHeader = (struct LinkNode*)malloc(sizeof(struct LinkNode));
	if (pHeader == NULL)
	{
		return NULL;
	}

	pHeader->next = NULL;
	struct LinkNode* pTail = pHeader;
	int val = -1;
	while (1)
	{
		printf("请初始化链表，输入一个数字，并按Enter键，如果输入-1则代表结束\n");
		scanf("%d", &val);
		if (val == -1)
		{
			break;
		}
		struct LinkNode* newNode = (struct LinkNode*)malloc(sizeof(struct LinkNode));
		newNode->num = val;
		newNode->next = NULL;

		pTail->next = newNode;  
		pTail = newNode;
	}

	return pHeader;
}


//02.头插法：初始化链表，函数返回值是创建好的链表的头结点
struct LinkNode* initLinkListHead()
{
	struct LinkNode* pHeader = (struct LinkNode*)malloc(sizeof(struct LinkNode));
	if (pHeader == NULL)
	{
		return NULL;
	}

	pHeader->next = NULL;
	//struct LinkNode* pTail = pHeader;
	int val = -1;
	while (1)
	{
		printf("请初始化链表，如果输入-1代表结束\n");
		scanf("%d", &val);
		if (val == -1)
		{
			break;
		}
		struct LinkNode* newNode = (struct LinkNode*)malloc(sizeof(struct LinkNode));
		newNode->num = val;
		newNode->next = pHeader->next;

		pHeader->next = newNode;
		//pTail = newNode;
	}

	return pHeader;
}


//03.遍历链表，并打印输出
void foreach_LinkList(struct LinkNode* pHeader)
{
	if (pHeader == NULL)
	{
		return;
	}
	struct LinkNode* pCurrent = pHeader->next;
	int i = 0;
	while (pCurrent != NULL)
	{
		printf("链表中第%d个结点的数据域的值为： %d\n", i + 1, pCurrent->num);
		i++;
		pCurrent = pCurrent->next;
	}
}


//04.获取链表长度，获取链表中元素个数
int getElementNums(struct LinkNode* pHeader)
{
	int nums = 0;
	if (pHeader == NULL)
	{
		return 0;
	}
	struct LinkNode* pCurrent = pHeader->next;
	
	while (pCurrent != NULL)
	{
		nums++;
		pCurrent = pCurrent->next;
	}
	return nums;
}


//05.获取链表中某位置对应的结点的值，假设头结点为第0个结点，pos为链表中第pos个结点
int getValueByPos(struct LinkNode* pHeader, int pos)
{
	struct LinkNode* tmpHeader = pHeader;
	if (pHeader == NULL)
	{
		return -1;
	}

	int lens = getElementNums(pHeader);
	if (pos <= 0)
	{
		printf("位置值不得为负数或0。请输入正确的位置\n");
		return -1;
	}
	if (pos > lens)
	{
		printf("位置值不得大于元素中的个数值，现在链表中已有%d个元素。请输入正确的位置\n", lens);
		return -1;
	}

	int i = 0;
	while (i < pos)
	{
		tmpHeader = tmpHeader->next;
		i++;
	}
	int value = tmpHeader->num;

	return value;
}


//06.向链表插入新结点（根据结点值）：到现存结点前面或末尾，如果存在该0ldVal值，就创建新节点，赋值为newVal，并插入到oldVal结点前面，如果不存在，则创建一个新结点，并插入到现有链表的尾部，新节点成为链表中最后一个结点。
void insert_LinkListByValueBefore(struct LinkNode* pHeader, int oldVal, int newVal)
{
	if (pHeader == NULL)
	{
		return;
	}

	//创建两个临时的节点
	struct LinkNode* pPrve = pHeader;
	struct LinkNode* pCurrent = pHeader->next;

	while (pCurrent != NULL)
	{
		if (pCurrent->num == oldVal)
		{
			break;
		}
		//如果没找到对应的位置,辅助指针向后移动
		pPrve = pCurrent;
		pCurrent = pCurrent->next;  //如果没找到oldVal，则pCurrent = pCurrent->next = NULL;
	}

	//创建新节点
	struct LinkNode* newNode = malloc(sizeof(struct LinkNode));
	newNode->num = newVal;
	newNode->next = NULL;


	//建立关系
	newNode->next = pCurrent; //如果没找到，则pCurrent=NULL
	pPrve->next = newNode;  //将新节点挂在pPrve的后面，即如果找到就是oldVal前面的一个结点，如果没找到就是链表最后一个结点。

}


//07.向链表插入新结点（根据结点值）：到现存结点后面或末尾，如果存在该0ldVal值，就创建新节点，赋值为newVal，并插入到oldVal结点前面，如果不存在，则创建一个新结点，并插入到现有链表的尾部，新节点成为链表中最后一个结点。
void insert_LinkListByValueAfter(struct LinkNode* pHeader, int oldVal, int newVal)
{
	if (pHeader == NULL)
	{
		return;
	}

	//创建两个临时的节点
	struct LinkNode * pTail = pHeader;
	struct LinkNode* pCurrent = pHeader->next;
	int flag = 0;

	while (pCurrent != NULL)
	{
		if (pCurrent->num == oldVal)
		{
			flag = 1;
			break;
		}
		//如果没找到对应的位置,辅助指针向后移动
		pTail = pCurrent;
		pCurrent = pCurrent->next;  //如果没找到oldVal，则pCurrent = pCurrent->next = NULL;
	}
	if (flag == 1)
	{
		struct LinkNode* newNode = malloc(sizeof(struct LinkNode));
		newNode->num = newVal;
		newNode->next = pCurrent->next;
		pCurrent->next = newNode;
	}
	else {
		//printf("未找到，失败\n");
		struct LinkNode* newNode = malloc(sizeof(struct LinkNode));
		newNode->num = newVal;
		newNode->next = NULL;

		//更改指针的指向
		pTail->next = newNode;
		//更新新的尾节点的指向
		pTail = newNode;
	}
}


//08.向链表插入新节点（根据给定位置）：位置需要为大于0的数
void insert_LinkListByPos(struct LinkNode* pHeader, int pos, int newVal)
{
	int num = getValueByPos(pHeader, pos);

	//printf("此时：pos= %d, num = %d\n", pos, num);  //此时pos等于i
	if (num == -1)
	{
		printf("当前pos参数输入错误，本次插入失败\n");
	}
	else {
		insert_LinkListByValueBefore(pHeader, num, newVal);
	}
}


//09.删除链表（根据给定结点值）：删除链表中某个结点
void delete_LinkListByValue(struct LinkNode* pHeader, int delVal)
{
	if (pHeader == NULL)
	{
		return;
	}

	//创建两个辅助指针变量
	struct LinkNode* pPrev = pHeader;
	struct LinkNode* pCurrent = pHeader->next;

	while (pCurrent != NULL)
	{
		if (pCurrent->num == delVal)
		{
			break;
		}
		//没有找到数据，辅助指针向后移动
		pPrev = pCurrent;
		pCurrent = pCurrent->next;
	}

	if (pCurrent == NULL) //没有找到用户要删除的数据
	{
		return;
	}

	//更改指针的指向进行删除
	pPrev->next = pCurrent->next;

	//删除掉待删除的节点
	free(pCurrent);
	pCurrent = NULL;
}


//10.删除链表（根据给定位置）：删除链表中某个结点
void delete_LinkListByPos(struct LinkNode* pHeader, int pos)
{
	int num = getValueByPos(pHeader, pos);
	if (num == -1)
	{
		printf("当前pos参数输入错误，本次删除失败\n");
	}
	else {
		delete_LinkListByValue(pHeader, num);
	}
}


//11.反转链表：逆置 -- 我写的
void reverse_LinkList(struct LinkNode* pHeader)
{
	if (pHeader == NULL)
	{
		return;
	}

	struct LinkNode* pCurrent = pHeader->next;

	if (pCurrent == NULL)
	{
		printf("空链表，逆置失败。\n");
		return;
	}

	pHeader->next = NULL;

	int tmpValue = pCurrent->next;
	while (pCurrent != NULL)
	{
		insert_LinkListByValueBefore(pHeader, tmpValue, pCurrent->num);
		tmpValue = pCurrent->num;
		pCurrent = pCurrent->next;		
	}
	//删除掉待删除的节点
	free(pCurrent);
	pCurrent = NULL;
}

//11-1.反转链表：逆置 -- 课程老师给定
void reverse_LinkList(struct LinkNode* pHeader)
{
	if (pHeader == NULL)
	{
		return;
	}

	struct LinkNode* pCurrent = pHeader->next;
	struct LinkNode* pPrev = NULL;
	struct LinkNode* pNext = NULL;

	while (pCurrent != NULL)
	{
		pNext = pCurrent->next;
		//更改指针指向
		pCurrent->next = pPrev;

		//移动辅助指针
		pPrev = pCurrent;
		pCurrent = pNext;
	}

	//更新头结点
	pHeader->next = pPrev;

}

//12.清空链表（可以复用）：清空各结点数据域，保留指针域，保留头结点
void clear_LinkList(struct LinkNode* pHeader)
{
	if (pHeader == NULL)
	{
		return;
	}

	struct LinkNode* pCurrent = pHeader->next;

	while (pCurrent != NULL)
	{
		//先保存住下一个节点的位置
		struct LinkNode* nextNode = pCurrent->next;

		free(pCurrent);

		pCurrent = nextNode;
	}

	pHeader->next = NULL;
}


//13.销毁链表：头结点和之后所有结点全部删除，一个不留
void destroy_LinkList(struct LinkNode* pHeader)
{
	if (pHeader == NULL)
	{
		return;
	}

	//先清空链表
	clear_LinkList(pHeader);

	//再释放头节点

	free(pHeader);
	pHeader = NULL;
}
```

Demo.c代码如下：

```C
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include "linklist.h"

//如果是学习之用，可以将注释分布取消，以观察输出的结果，如果是用例的调用需要，可以直接将注释全部删除
void test01()
{
	//初始化
	struct LinkNode* pHeader = initLinkListTail();  //假设输入1,2,3,4,5,-1  ，尾插法
	//struct LinkNode* pHeader = initLinkListHead();
	
	//遍历
	foreach_LinkList(pHeader); //尾插法：1,2,3,4,5 ；头插法：5,4,3,2,1

	//获取元素个数
	int lens = getElementNums(pHeader);
	printf("lens = %d\n", lens);  //lens = 5
	printf("间隔行：1*********************************************\n\n");
	
	//向链表插入新结点（根据结点值）：到现存结点前面或末尾
	//insert_LinkListByValueBefore(pHeader,3, 8);  //在链表中如果找到对应给定值的结点，就在该节点的前面插入
	//foreach_LinkList(pHeader);//1,2,8,3,4,5
	//printf("间隔行：2*********************************************\n\n");
	//insert_LinkListByValueBefore(pHeader, 6, 9); //在链表中如果没有找到给定值的结点，就在最后插入
	//foreach_LinkList(pHeader);//1,2,8,3,4,5,9
	//printf("间隔行：3*********************************************\n\n");
	
	//向链表插入新结点（根据结点值）：到现存结点后面或末尾，尾插法
	//insert_LinkListByValueAfter(pHeader, 3, 10);//在链表中如果找到给定值的结点，就在该结点的后面插入
	//foreach_LinkList(pHeader);//1,2,8,3,10,4,5,9
	//printf("间隔行：4*********************************************\n\n");
	//insert_LinkListByValueAfter(pHeader, 6, 11);//在链表中如果没有找到给定值的结点，就在最后插入
	//foreach_LinkList(pHeader);//1,2,8,3,10,4,5,9,11
	//printf("间隔行：5*********************************************\n\n");

	//向链表插入新节点（根据给定位置）:位置需要为大于0的数
	//insert_LinkListByPos(pHeader, 0,12);
	insert_LinkListByPos(pHeader, 3, 12);
	//insert_LinkListByPos(pHeader, 5, 12);
	//insert_LinkListByPos(pHeader, 6, 12);
	foreach_LinkList(pHeader);  //1,2,12,3,4,5
	printf("间隔行：6*********************************************\n\n");

	//删除链表（根据给定结点值）：删除链表中某个结点
	delete_LinkListByValue(pHeader, 12);
	foreach_LinkList(pHeader);  //1,2,3,4,5
	printf("间隔行：7*********************************************\n\n");
	//delete_LinkListByValue(pHeader, 4);
	//foreach_LinkList(pHeader);  //1,2,12,3,4,5
	//printf("间隔行：8*********************************************\n\n");
	//delete_LinkListByValue(pHeader, 8);  //链表中并没有8，不做删除
	//foreach_LinkList(pHeader);  //1,2,12,3,4,5
	//printf("间隔行：9********************************************\n\n");

	//删除链表（根据给定位置）：删除链表中某个结点
	delete_LinkListByPos(pHeader, 3);
	foreach_LinkList(pHeader);  //1,2,4,5
	lens = getElementNums(pHeader);
	printf("lens = %d\n", lens);  //lens = 4
	delete_LinkListByPos(pHeader, lens);
	foreach_LinkList(pHeader);  //1,2,4
	lens = getElementNums(pHeader);
	printf("lens = %d\n", lens);  //lens = 3
	printf("间隔行：10*********************************************\n\n");


	//反转链表：逆置
	reverse_LinkList(pHeader);
	foreach_LinkList(pHeader);  //4,2,1
	lens = getElementNums(pHeader);
	printf("lens = %d\n", lens);  //lens = 3
	printf("间隔行：11*********************************************\n\n");

	//清空链表（可以复用）：清空各结点数据域，保留指针域，保留头结点
	clear_LinkList(pHeader);
	foreach_LinkList(pHeader);  //不打印
	lens = getElementNums(pHeader);
	printf("lens = %d\n", lens);  //lens = 0
	printf("间隔行：12*********************************************\n\n");

	//销毁链表：头结点和之后所有结点全部删除，一个不留
	destroy_LinkList(pHeader);
	pHeader = NULL;
	printf("间隔行：13*********************************************\n\n");
	
	
}

int main()
{
	test01();

	printf("\n");
	system("pause");
	return 0;
}
```



##### 12.回调函数案例1 -- 打印任意类型数据

//回调函数：函数指针做函数参数

```C
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<string.h>
#include<stdlib.h>

//提供一个打印函数，可以打印任意类型的数据--基本数据类型，自定义数据类型
void printText(void* data, void(*myPrint)(void*))
{
	myPrint(data);

}
//打印int类型数据 -- 回调函数
void myPrintInt(void* data)
{
	/*int* num = (int*)data;*/
	int* num = data;
	printf("%d\n", *num);
}
//打印double类型数据
void myPrintDouble(void* data)
{
	double* num = data;
	printf("%f\n", *num);
}
//打印char类型数据
void myPrintChar(void* data)
{
	char* num = data;
	printf("%c\n", *num);
}

void test01()
{
	int a = 10;
	printText(&a, myPrintInt);

	double b = 3.14;
	printText(&b, myPrintDouble);

	char ch = 'a';
	printText(&ch, myPrintChar);
}


struct Person
{
	char name[64];
	int age;
};

void myPrintPerson(void* data)
{
	struct Person* p = data;
	printf("姓名： %s 年龄： %d\n", p->name, p->age);
}

void test02()
{
	struct Person p = { "Tom", 18 };

	printText(&p, myPrintPerson);

}

int main() {

	test01();
	test02();
	system("pause");
	return EXIT_SUCCESS;
}
```



##### 13.回调函数案例2 -- 打印任意类型数组（基本数据类型、字符串数组、结构体数组）

```C
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<string.h>
#include<stdlib.h>

//提供一个函数，实现可以打印任意类型的数组 
void printAllArray(void* pArray, int eleSize, int len, void(*myPrint)(void*)) //本函数的作用就是一个桥梁
{
	char* p = pArray;  //利用p指针接收数组首地址

	for (int i = 0; i < len; i++)
	{
		//获取数组中每个元素的首地址
		char* eleAddr = p + eleSize * i;
		//printf("%d\n", *(int *)eleAddr);
		//交还给用户做打印操作
		myPrint(eleAddr);
	}
	printf("\n");

}  
//打印int类型数组
void myPrintInt(void* data)
{
	int* num = data;
	printf("%-2d", *num);
}
//打印字符数组
void myPrintChar(void* data)
{
	char* num = data;
	printf("%c", *num);
}
//打印字符串数组
void myPrintStr(void* data)
{
	char** num = data;
	printf("%s  ", *num);
}

struct Person
{
	char name[64];
	int age;
};

//打印自定义类型：结构体数组
void myPrintperson(void* data)
{
	struct Person* p = data;
	printf("姓名：%s  年龄：%d \n", p->name, p->age);
}

void test01()
{
	//打印int类型数组
	int arr[5] = { 1, 2, 3, 4, 5 };
	int len = sizeof(arr) / sizeof(int);
	//printAllArray(arr, sizeof(int), len, myPrintInt);
	printAllArray(arr, sizeof(arr[0]), len, myPrintInt);

	//打印char类型数组
	//char str[5] = { 'h','e','l','l','o'};
	char str[] = "hello world"; //"hello\0world"：会出现空格  //"hello\nworld"：会换行打印
	len = sizeof(str) / sizeof(str[0]);
	printAllArray(str, sizeof(str[0]), len, myPrintChar);

	//打印char *类型数组，即字符串数组
	char* num[3] = { "heihei" ,"haha","xixi" };
	len = sizeof(num) / sizeof(num[0]);
	printAllArray(num, sizeof(num[0]), len, myPrintStr);
    
    //打印自定义类型：结构体数组
	struct Person personArray[] =
	{
		{ "aaa", 10 },
		{ "bbb", 20 },
		{ "ccc", 30 },
		{ "ddd", 40 },
	};
	len = sizeof(personArray) / sizeof(struct Person);
	printAllArray(personArray, sizeof(struct Person), len, myPrintperson);
}

int main() {

	test01();
	
	system("pause");
	return EXIT_SUCCESS;
}
```



##### 14.回调函数案例3 -- 查找数组中的元素（基本数据类型、字符串数组、结构体数组）

```C
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<string.h>
#include<stdlib.h>

struct Person
{
	char name[64];
	int age;
};

//查找数组中的元素是否存在
//参数1  数组首地址   参数2  每个元素的大小  参数3  数组元素个数  参数4 查找数据
int findArrayEle(void * pArray, int eleSize, int len, void * data ,  int(*myCompare)(void* ,void* )  )
{
	char * p = pArray;

	for (int i = 0; i < len;i++)
	{
		//每个元素的首地址
		char * eleAddr = p + eleSize * i;

		//if ( 数组中的变量的元素 == 用户传入的元素)
		if ( myCompare(eleAddr,data)  )
		{
			return 1;
		}
	}

	return 0;

}

int myComparePerson(void * data1,void * data2)
{
	struct Person * p1 = data1;
	struct Person * p2 = data2;

	//if ( strcmp( p1->name , p2->name) == 0  &&  p1->age == p2->age)
	//{
	//	return 1;
	//}
	//return  0;

	return   strcmp(p1->name, p2->name) == 0 && p1->age == p2->age;

}

int myCompareInt(void* data1, void* data2)
{
	int* p1 = data1;
	int* p2 = data2;

	return   ((*p1) == (*p2));

}

int myCompareStr(void* data1, void* data2)
{
	char* p1 = data1;
	char* p2 = data2;

	//if ( strcmp( p1->name , p2->name) == 0  &&  p1->age == p2->age)
	//{
	//	return 1;
	//}
	//return  0;

	return  strcmp(p1, p2) == 0;

}

void test02()
{
	struct Person personArray[] =
	{
		{ "aaa", 10 },
		{ "bbb", 20 },
		{ "ccc", 30 },
		{ "ddd", 40 },
	};
	int len = sizeof(personArray) / sizeof(struct Person);

	//查找自定义类型--结构体数组中指定的元素是否存在
	struct Person p = { "ccc", 30 };

	int ret = findArrayEle(personArray, sizeof(struct Person), len, &p, myComparePerson);

	if (ret)
	{
		printf("找到了元素\n");
	}
	else
	{
		printf("未找到\n");
	}

	//查找int类型数组中指定的元素是否存在
	int arr[5] = { 1, 2, 3, 4, 5 };
	len = sizeof(arr) / sizeof(int);
	int numTmp = 3;
	ret = findArrayEle(arr, sizeof(int), len, &numTmp, myCompareInt);
	if (ret)
	{
		printf("找到了元素\n");
	}
	else
	{
		printf("未找到\n");
	}

	//查找char *类型数组，即字符串数组
	char* num[3] = { "heihei" ,"haha","xixi" };
	len = sizeof(num) / sizeof(num[0]);
	char* strTmp = "haha";
	ret = findArrayEle(num, sizeof(num[0]), len, &strTmp, myCompareStr);
	if (ret)
	{
		printf("找到了元素\n");
	}
	else
	{
		printf("未找到\n");
	}
}

int main()
{

	test02();
	system("pause");
	return EXIT_SUCCESS;
}
```



##### 15.回调函数案例4 -- 对任意类型的数组进行排序（基本数据类型数组、字符串数组、结构体数组）

```C
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<string.h>
#include<stdlib.h>

void selectSort(void* pAddr, int elesize, int len, int(*myCompare)(void*, void*))
{
	char* temp = malloc(elesize);

	//选择排序：结果将从大到小输出
	for (int i = 0; i < len; i++)
	{
		int minOrMax = i; //定义最小值 或者 最大值 下标
		for (int j = i + 1; j < len; j++)
		{
			//定义出 j元素地址
			char* pJ = (char*)pAddr + elesize * j;
			char* pMinOrMax = (char*)pAddr + elesize * minOrMax;  //此处pMinOrMax其实是max的意思
			//if ( pAddr[j] < pAddr[minOrMax])

			/* 从大到小
			if ( *num1 > *num2)
			{
			return 1;
			}
			return 0;
			*/

			if (myCompare(pJ, pMinOrMax))
			{
				minOrMax = j; //更新最小值或者最大值下标  //此时本质是max
			}
		}

		if (i != minOrMax)
		{
			//交换i和minOrMax 下标元素
			char* pI = (char*)pAddr + i * elesize;
			char* pMinOrMax = (char*)pAddr + minOrMax * elesize;
            
            //通过内存拷贝实现交换
			memcpy(temp, pI, elesize);
			memcpy(pI, pMinOrMax, elesize);
			memcpy(pMinOrMax, temp, elesize);

		}

	}

	if (temp != NULL)
	{
		free(temp);
		temp = NULL;
	}

}

int myCompareInt(void* data1, void* data2)
{
	int* num1 = data1;
	int* num2 = data2;

    //降序
	if (*num1 > *num2)
	{
		return 1;
	}
	return 0;
}

int myCompareStr(void* data1, void* data2)
{
	char* p1 = data1;
	char* p2 = data2;

	//if ( strcmp( p1->name , p2->name) == 0  &&  p1->age == p2->age)
	//{
	//	return 1;
	//}
	//return  0;
	if(strcmp(p1, p2) >0)
	{
		return 1;
	}
	return 0;
}

void test01()
{
	//对int类型数组进行排序
	int arr[] = { 10, 30, 20, 60, 50, 40 };
	int len = sizeof(arr) / sizeof(int);
	selectSort(arr, sizeof(int), len, myCompareInt);

	for (int i = 0; i < len; i++)  //60  50  40  30  20  10
	{
		printf("%-4d", arr[i]);
	}
	printf("\n\n");

	char* num[4] = { "heihei" ,"heik","yaha","xixi" };
	len = sizeof(num) / sizeof(num[0]);
	selectSort(num, sizeof(num[0]), len, myCompareInt);
	for (int i = 0; i < len; i++)  //xixi   yaha   heik   heihei
	{
		printf("%s   ", num[i]);
	}
	printf("\n\n");

}


struct Person
{
	char name[64];
	int age;
};

int myComparePerson(void* data1, void* data2)
{
	struct Person* p1 = data1;
	struct Person* p2 = data2;

	//if ( p1->age < p2->age)
	//{
	//	return  1;
	//}
	//return 0;
	//按照年龄 升序排序
	return  p1->age < p2->age;

}

void test02()
{
	//对自定义数据类型struct数组进行排序
	struct Person pArray[] =
	{
		{ "aaa", 10 },
		{ "bbb", 40 },
		{ "ccc", 20 },
		{ "ddd", 30 },
	};
	int len = sizeof(pArray) / sizeof(struct Person);
	selectSort(pArray, sizeof(struct Person), len, myComparePerson);

	for (int i = 0; i < len; i++)
	{
		printf("姓名：%s 年龄:%d\n", pArray[i].name, pArray[i].age);
	}
	printf("\n");
}


int main() {
	test01();
	test02();

	system("pause");
	return EXIT_SUCCESS;
}
```



##### 16. 