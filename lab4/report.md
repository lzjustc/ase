# ʵ���ģ��ÿ����õ�����ģ����ʵ�������в˵�С����V2.5
#### SA17225184 ���ξ�
## Ŀ¼
1. ʵ��Ҫ��
1. ʵ�����
1. ʵ����
1. ʵ������������  
## 1. ʵ��Ҫ��
- �ÿ����õ�����ģ����ʵ�������в˵�С����ִ��ĳ������ʱ����һ���ض��ĺ�����Ϊִ�ж�����
- ����ģ��Ľӿ����Ҫ�㹻ͨ�ã������в˵�С����Ĺ��ܱ��ֲ��䣻
- ���Խ�ͨ�õ�Linktableģ�鼯�ɵ����ǵ�menu�����У�
- �ӿڹ淶��
## 2. ʵ�����
1. ����ʵ���ļ��У��½�һ��`linktable.h`���༭����
``` bash
cd /home/lzj/homework/asework/ase
mkdir lab4
cd lab4
vim linktable.h
```
`linktable.h`�������£�
``` c++
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

tLinkTableNode* GetLinkTableHead(tLinkTable* pLinkTable);

tLinkTableNode* GetNextLinkTableNode(tLinkTable* pLinkTable, tLinkTableNode* pNode);
```
### 2. �½�һ��`linktable.c`���༭���������£�
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

tLinkTableNode* GetLinkTableHead(tLinkTable* pLinkTable)
{
    if (pLinkTable == NULL || pLinkTable->pHead == NULL)
    {
        return NULL;
    }
    return pLinkTable->pHead;
}

tLinkTableNode* GetNextLinkTableNode(tLinkTable* pLinkTable, tLinkTableNode* pNode)
{
    if (pLinkTable == NULL || pNode == NULL)
    {
        return NULL;
    }
    tLinkTableNode* p = pLinkTable->pHead;
    while (p != NULL)
    {
        if (p == pNode)
        {
            return p->pNext;
        }
        p = p->pNext;
    }
    return NULL;
}
```
### 3. �½�`menu.c`�� �������£�
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

#define CMD_MAX_LENGTH 128

typedef struct DataNode
{
    tLinkTableNode* pNext;
    char* cmd;
    char* desc;
    void (*handler)();
}tDataNode;

tDataNode* FindCmd(tLinkTable* head, char* cmd)
{
    tDataNode* pNode = (tDataNode*)GetLinkTableHead(head);
    while(pNode != NULL)
    {
        if(strcmp(pNode->cmd, cmd) == 0)
        {
            return pNode;
        }
        pNode = (tDataNode*)GetNextLinkTableNode(head, (tLinkTableNode*)pNode);
    }
    return NULL;
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

tLinkTable* InitMenuData(tCMDNode* Head)
{
    tLinkTable* pLinkTable = CreateLinkTable();
    int i;
    for(i=0; pLinkTable->sumOfNode < 7; i++)
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
    tLinkTable* head = InitMenuData(Head);
    char cmd[CMD_MAX_LENGTH];    
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
    tLinkTable* head = InitMenuData(Head);
    ShowAllCmd(head);
    printf("*************************************************************\n");
}

void Version()
{
    printf("menu version v3.0\n");
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
    printf("\t\t\tAuthor infomation\n\n");
    printf("Name:\t\t Li Zhijun (���ξ�)\n");
    printf("Student ID:\t SA17225184\n");
    printf("Class:\t\t ����2��\n");
    printf("\t\t School of software engineering, USTC\n");
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
### 4. ���ļ��������У�����Դ���봫������
``` bash
gcc menu.c linklist.c -o menu
./ menu
```
``` bash
git add lab4/menu.c lab4/linklist.c lab4/linklist.h
git commit -m "add lab4"
git push
```
## 3. ʵ����
![image](/lab4/img/img1.png)  
![image](/lab4/img/img2.png)  
## 4. ʵ������������
��ʦ�ĳ�ʼ����������˶�Ĵ����ظ�������ʵ���ܲ��ˡ������뵽��֮ǰ�ĳ�ʼ���������޸ġ� 
�����˽ṹ����ⲿ����`Head`
``` c++
tCMDNode cmdHead[] = 
{
    {"help", "This is help command.", Help, &cmdHead[1]},
    {"version","Show version of this menu program.", Version, &cmdHead[2]},
    {"quit", "Quit and back to OS.", Quit, &cmdHead[3]},
    {"hello", "Say hello to users.", Hello, &cmdHead[4]},
    {"time", "Show system date and time.", Time, &cmdHead[5]},
    {"author", "Show author information.", Author, &cmdHead[6]},
    {"triangle", "Show a big triangle on screen.", Triangle, &cmdHead[7]},
    {"heart", "Show a big heart on screen.", Heart, NULL}
};
```
Ȼ��ͨ��InitMenuData����������һ������Ҫ��linktable�ṹ��
``` c++
tLinkTable* InitMenuData(tCMDNode* Head)
{
    tLinkTable* pLinkTable = CreateLinkTable();
    int i;
    for(i=0; pLinkTable->sumOfNode < 7; i++)
    {
        tDataNode* pNode = (tDataNode*)malloc(sizeof(tDataNode));
        pNode->cmd = Head[i].cmd;
        pNode->desc = Head[i].desc;
        pNode->handler = Head[i].handler;
        AddLinkTableNode(pLinkTable, (tLinkTableNode*)pNode);
    }
    return pLinkTable;
}
```
����һ������ĺ�«��ư�������ⲿ����`head`
``` c++
tLinkTable* head = InitMenuData(Head);
```
����Ϳ�����`main()`����ֱ��ʹ���ˡ�ȥ������ô����ظ�����ÿ��ġ�  
���룬�����Ȼ�����⣬error��
```
menu.c:85:20: ���󣺳�ʼֵ�趨Ԫ�ز��ǳ���
 tLinkTable* head = InitMenuData(Head);
```
![image](/lab4/img/img3.png)  
��ͷ�ŷ��֣�ԭ��C������ⲿ���������ʼ��Ϊ����������ͨ������������һ���ǳ�����ֵ��  
���ǣ���`tLinkTable* head = InitMenuData(Head);`������`main()`�ڲ�������һЩ��Ӧ���޸ģ�ͨ����  
  
[��ʵ������git��ַ](https://github.com/lzjustc/ase/tree/master/lab4)