# ʵ���壺��callback��ǿ����ģ����ʵ�������в˵�С����V2.8
#### SA17225184 ���ξ�
## Ŀ¼
1. ʵ��Ҫ��
1. ʵ�����
1. ʵ����
1. ʵ���ĵ�  
## 1. ʵ��Ҫ��
- �´���һ��Ŀ¼lab5���ʵ�顣
- Ȼ��lab5-1.tar.gz�еĴ��루����ѹ��lab5.1/Ŀ¼�µ�Դ�ļ���ֱ�ӷŵ�lab5/Ŀ¼�¼�����ɺ����ʵ������
- ��ʵ�����ṩ�Ĵ�������Ͻ���
- ��lab5-1.tar.gz��bug��quit�����޷����е�bug
- ����callback��������ʹLinktable�Ĳ�ѯ�ӿڸ���ͨ��
- ע��ӿڵ���Ϣ���� 

## 2. ʵ�����
### 2.0 �����벢����ʵ���ļ���

``` shell
cd /home/lzj/homework/asework
git clone git@github.com:lzjustc/ase.git
cd ase
mkdir lab5
```
![image](img/img1.png)

### 2.1 ��quit�����޷����е�BUG 
#### 2.1.1. ��ѹʵ���ļ�

``` shell
cd lab5
tar -zxf /home/lzj/share/lab5-1.tar.gz
mv lab5.1/* .
rm -rf lab5.1
```
![image](img/img2.png)
#### 2.1.2 ������벢Debug
`gcc menu.c linktable.c -o menu`  
`warning: implicit declaration of function 'strcmp'`  
������ȱ�ٰ���ͷ�ļ�`<string.h>`�� ��menu.c�м���`#include<string.h>`��������  
���г�����ͼ���Է�������quit�����޷������˳���  
![image](img/img3.png)  
�鿴���룬����`linktable.c`�е�`SearchLinkTableNode`�����������⡣  
`while(pNode != pLinkTable ->pTail)`ʹ�ú������ҵ��������һ���ڵ�ʱ���˳�ѭ��,����޷��������һ���ڵ㡣  
��`while(pNode != pLinkTable -> pTail)`��Ϊ`while(pNode != NULL)`���ɣ���ͼ��ʾ��  
![image](img/img4.png)  
### 2.2 ����callback��������ʹLinktable�Ĳ�ѯ�ӿڸ���ͨ��  
#### 2.2.1 ׼������
��2.1�Ĵ�ʵ����ļ������`lab5/lab5.1`����lab4��ʵ���ļ�copy��lab5���Դ�Ϊ���������޸ġ�
`
``` shell
mkdir lab5.1
mv * lab5.1
cp ../lab4/*.c ../lab4/*.h .
```
#### 2.2.2 `linktable.h`��������
``` c++
#ifndef _LINK_TABLE_H_
#define _LINK_TABLE_H_

#define SUCCESS 0
#define FAILURE (-1)

typedef struct LinkTableNode
{
    struct LinkTableNode* pNext;
}tLinkTableNode;

typedef struct LinkTable
{
    tLinkTableNode *pHead;
    tLinkTableNode *pTail;
    int             sumOfNode;
}tLinkTable;

tLinkTable* CreateLinkTable();

int DeleteLinkTable(tLinkTable* pLinkTable);

int AddLinkTableNode(tLinkTable* pLinkTable, tLinkTableNode* pNode);

int DelLinkTableNode(tLinkTable* pLinkTable, tLinkTableNode* pNode);


tLinkTableNode * SearchLinkTableNode(tLinkTable *pLinkTable, int Conditon(tLinkTableNode * pNode));

tLinkTableNode* GetLinkTableHead(tLinkTable* pLinkTable);

tLinkTableNode * GetNextLinkTableNode(tLinkTable *pLinkTable,tLinkTableNode * pNode);

#endif /* _LINK_TABLE_H_ */
```
#### 2.2.3 `linktable.c`��������
``` c++
#include <stdio.h>
#include <stdlib.h>
#include "linktable.h"

tLinkTable* CreateLinkTable()
{
    tLinkTable* pLinkTable = (tLinkTable*)malloc(sizeof(tLinkTable));
    if(pLinkTable == NULL)
    {
        return NULL;
    }
    pLinkTable->pHead = NULL;
    pLinkTable->pTail = NULL;
    pLinkTable->sumOfNode = 0;
    return pLinkTable;
}

int DeleteLinkTable(tLinkTable* pLinkTable)
{
    if(pLinkTable == NULL)
    {
        return FAILURE;
    }
    while(pLinkTable->pHead != NULL)
    {
        tLinkTableNode* pNode = pLinkTable->pHead;
        pLinkTable->pHead = pLinkTable->pHead->pNext;
        free(pNode);  
    } 
    pLinkTable->pHead = NULL;
    pLinkTable->pTail = NULL;
    pLinkTable->sumOfNode = -1;
    free(pLinkTable);
    return SUCCESS;
}

int AddLinkTableNode(tLinkTable* pLinkTable, tLinkTableNode* pNode)
{
    if (pLinkTable == NULL || pNode == NULL)
    {
        return FAILURE;
    }
    pNode->pNext = NULL;
    if (pLinkTable->pHead == NULL)
    {
        pLinkTable->pHead = pNode;
    }
    if (pLinkTable->pTail == NULL)
    {
        pLinkTable->pTail = pNode;
    }
    else
    {
        pLinkTable->pTail->pNext = pNode;
        pLinkTable->pTail = pNode;
    }
    pLinkTable->sumOfNode = pLinkTable->sumOfNode + 1;
    return SUCCESS;
}

int DelLinkTableNode(tLinkTable* pLinkTable, tLinkTableNode* pNode)
{
    if (pLinkTable == NULL || pNode == NULL)
    {
        return FAILURE;
    }
    if (pLinkTable->pHead == pNode)
    {
        pLinkTable->pHead = pLinkTable->pHead->pNext;
        pLinkTable->sumOfNode = pLinkTable->sumOfNode - 1;
        if (pLinkTable->sumOfNode == 0)
        {
            pLinkTable->pTail == NULL;
        }
        return SUCCESS;
    }
    tLinkTableNode* p = pLinkTable->pHead;
    while (p != NULL)
    {
        if (p->pNext == pNode)
        {
            p->pNext = p->pNext->pNext;
            pLinkTable->sumOfNode = pLinkTable->sumOfNode - 1;
            if (pLinkTable->sumOfNode == 0)
            {
                pLinkTable->pTail = NULL;
            }
            return SUCCESS;
        }
        p = p->pNext;
    }
    return FAILURE;
}

tLinkTableNode * SearchLinkTableNode(tLinkTable *pLinkTable, int Conditon(tLinkTableNode * pNode))
{
    if(pLinkTable == NULL || Conditon == NULL)
    {
        return NULL;
    }
    tLinkTableNode * pNode = pLinkTable->pHead;
    while(pNode != NULL)
    {    
        if(Conditon(pNode) == SUCCESS)
        {
            return pNode;				    
        }
        pNode = pNode->pNext;
    }
    return NULL;
}

tLinkTableNode* GetLinkTableHead(tLinkTable* pLinkTable)
{
    if (pLinkTable == NULL || pLinkTable->pHead == NULL)
    {
        return NULL;
    }
    return pLinkTable->pHead;
}

tLinkTableNode * GetNextLinkTableNode(tLinkTable *pLinkTable,tLinkTableNode * pNode)
{
    if (pLinkTable == NULL || pNode == NULL)
    {
        return NULL;
    }
    tLinkTableNode* pTempNode = pLinkTable->pHead;
    while (pTempNode != NULL)
    {
        if (pTempNode == pNode)
        {
            return pTempNode->pNext;
        }
        pTempNode = pTempNode->pNext;
    }
    return NULL;
}
```
#### 2.2.4 `menu.c`��������
``` c++
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include "linktable.h"

void Help();
void Version();
void Quit();
void Hello();
void Time();
void Author();
void Triangle();
void Heart();

#define CMD_MAX_LEN 128
#define DESC_LEN    1024
#define CMD_NUM     10

char cmd[CMD_MAX_LEN]; 

typedef struct DataNode
{
    tLinkTableNode* pNext;
    char* cmd;
    char* desc;
    void (*handler)();
}tDataNode;

int SearchCondition(tLinkTableNode * pLinkTableNode)
{
    tDataNode * pNode = (tDataNode *)pLinkTableNode;
    if(strcmp(pNode->cmd, cmd) == 0)
    {
        return  SUCCESS;  
    }
    return FAILURE;	       
}

tDataNode* FindCmd(tLinkTable* head, char* cmd)
{
    return  (tDataNode*)SearchLinkTableNode(head,SearchCondition);
}

int ShowAllCmd(tLinkTable* head)
{
    tDataNode* pNode = (tDataNode*) GetLinkTableHead(head);
    while(pNode != NULL)
    {
        printf("%10s   -----   %s\n", pNode->cmd, pNode->desc);
        pNode = (tDataNode*)GetNextLinkTableNode(head, (tLinkTableNode*)pNode);
    }
    return 0;
}

typedef struct CMDNode
{
    char* cmd;
    char* desc;
    void (*handler)();
    struct CMDNode *next;
}tCMDNode;
    
tCMDNode Head[] =
{
    {"help", "This is help command.", Help, &Head[1]},
    {"version","Show version of this menu program.", Version, &Head[2]},
    {"quit", "Quit and back to OS.", Quit, &Head[3]},
    {"hello", "Say hello to users.", Hello, &Head[4]},
    {"time", "Show system date and time.", Time, &Head[5]},
    {"author", "Show author information.", Author, &Head[6]},
    {"triangle", "Show a big triangle on screen.", Triangle, &Head[7]},
    {"heart", "Show a big heart on screen.", Heart, NULL},
};

tLinkTable* InitMenuData(tCMDNode* Head, int length)
{
    tLinkTable* pLinkTable = CreateLinkTable();
    int i;
    for(i=0; pLinkTable->sumOfNode < length; i++)
    {
        tDataNode* pNode = (tDataNode*)malloc(sizeof(tDataNode));
        pNode->cmd = Head[i].cmd;
        pNode->desc = Head[i].desc;
        pNode->handler = Head[i].handler;
        AddLinkTableNode(pLinkTable, (tLinkTableNode*)pNode);
    }
    return pLinkTable;
}

int main()
{
	tLinkTable* head = InitMenuData(Head, 8); 
    while(1)
    {
        printf("menu cmd-> ");
        scanf("%s", cmd);
        tDataNode* p = FindCmd(head, cmd);      
        if(p == NULL)
        {
            printf("error: Wrong command!\n");
        }
        else
        {
            p->handler();
        }
    }
    return 0;
}

void Help()
{
    printf("*************************************************************\n");
    printf("This is help command.\n\n");
    printf("commands:\n");
	tLinkTable* head = InitMenuData(Head, 8); 
    ShowAllCmd(head);
    printf("*************************************************************\n");
}

void Version()
{
    printf("menu version v2.8\n");
}

void Quit()
{
    printf("Goodbye\n");
    exit(0);
}

void Hello()
{
    printf("Hi, How are you?\n");
    char answer;
    printf("Input (Y/N), Y:I'm fine, thank you!  N:I'm feeling terrible\n");
    getchar();
    answer = getchar();
    if(answer == 'Y' || answer == 'y')
    {
        printf("Wish you happy everyday!\n");
    }
    else if(answer == 'N' || answer == 'n')
    {
        printf("Cheer up, things will work out for the best!\n");
    }
    else
    {
        printf("Sorry, I can't understand it!\n");
    }
}

void Time()
{
    system("date");
}

void Author()
{
    printf("*************************************************************\n");
    printf("Author infomation\n\n");
    printf("%12s :  Li Zhijun (���ξ�)\n", "name");
    printf("%12s :  SA17225184\n", "Student ID");
    printf("%12s :  ����2��\n", "Class");
    printf("School of software engineering, USTC\n");
    printf("*************************************************************\n");
}

void Triangle()
{
    int i,j;
    int n = 20;
    for(i = 0; i < n; i++)
    {
        for(j = n - i + 10; j > 0; j--)
        {
            putchar(' ');
        }
        for(j = 0; j < i * 2 - 1; j++)
        {
            putchar('*');
        }
        putchar('\n');
    }
}

void Heart()
{
    float x, y;
    for(y = 1.5f; y > -1.5f; y -= 0.1f)
    {
        for(x = -1.5f; x < 1.5f; x += 0.05f)
        {
            float a = x * x + y * y - 1;
            putchar(a * a * a - x * x * y * y * y <= 0.0f ? '*': ' ');
        }
        putchar('\n');
    }
}
```
### 3 ʵ����
![image](img/img5.png)  
![image](img/img6.png)  

### 4 ʵ���ĵ�
- �ص���������һ��ͨ������ָ����õĺ����������Ѻ�����ָ�루��ַ����Ϊ�������ݸ���һ�������������ָ�뱻������������ָ��ĺ���ʱ�����Ǿ�˵���ǻص��������ص����������ɸú�����ʵ�ַ�ֱ�ӵ��ã��������ض����¼�����������ʱ�������һ�����õģ����ڶԸ��¼�������������Ӧ��
- ��Ϊ���԰ѵ������뱻�����߷ֿ������Ե����߲�����˭�Ǳ������ߡ���ֻ��֪������һ�������ض�ԭ�ͺ����������ı����ú����������֮���ص��������������û�����Ҫ���õķ�����ָ����Ϊ�������ݸ�һ���������Ա�ú����ڴ��������¼���ʱ���������ʹ�ò�ͬ�ķ�����
- callback������ʹ������˴���������ԣ�ʵ���˸��ɵ���ϡ�ͬʱ��һЩ�ڲ��ṹ�ӿ����أ������û��ӿڣ�����֤��ʹ�ð�ȫ�������������