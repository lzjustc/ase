# ʵ�������ڲ�ģ�黯����˵�С����V2.0
#### SA17225184 ���ξ�
## Ŀ¼
1. ʵ��Ҫ��
1. ʵ�����
1. ʵ����
1. ʵ���ĵ�  
## 1. ʵ��Ҫ��
ע������ҵ���߼������ݴ洢֮��ķ��룬����ϵͳ����Ϊ�����㼶���˵�ҵ���߼��Ͳ˵����ݴ洢  
Ҫ��
1. ���ش�����淶���ο����������ƹ淶��һЩ������
2. �����ҵ���߼������ݴ洢ʹ�ò�ͬ��Դ�ļ�ʵ�֣���Ӧ����2��.c��һ��.h��Ϊ�ӿ��ļ���
## 2. ʵ�����
1. ����ʵ���ļ��У��½�һ��linklist.h���༭����
``` bash
cd /home/lzj/homework/asework/ase
mkdir lab3
cd lab3
vim linklist.h
```
linklist.h�������£�
``` c++
typedef struct CMDNode
{
    char* cmd;
    char* disc;
    void (*handler)();
    struct CMDNode *next;
}tCMDNode;

tCMDNode* FindCmd(tCMDNode* head, char* cmd);

void ShowAllCmd(tCMDNode* head);
```
2. �½�һ��linklist.c���༭���������£�  

``` c++
#include <stdio.h>
#include <string.h>
#include <assert.h>
#include "linklist.h"

tCMDNode* FindCmd(tCMDNode* head, char* cmd)
{
    if(head == NULL)
    {
        return NULL;
    }
    tCMDNode* p = head;
    while(p != NULL)
    {
        if(strcmp(p->cmd, cmd) == 0)
        {
            if(p->handler != NULL)
            {
                return p;
            }
        }
        p = p->next;
    }
    assert(p == NULL);
    return p;
}

void ShowAllCmd(tCMDNode* head)
{
    if(head == NULL)
    {
        return;
    }
    tCMDNode* p = head;
    while(p != NULL)
    {
        printf("%10s   -----   %s\n", p->cmd, p->disc);
        p = p->next;
    }
    return;
}
```
3. �½�menu.c�� �������£�

``` c++
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

void Help();
void Versi#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include "linklist.h"

void Help();
void Version();
void Quit();
void Hello();
void Time();
void Author();
void Triangle();
void Heart();

#define CMD_MAX_LENGTH 128

static tCMDNode head[] = 
{
    {"help", "This is help command.", Help, &head[1]},
    {"version","Show version of this menu program.", Version, &head[2]},
    {"quit", "Quit and back to OS.", Quit, &head[3]},
    {"hello", "Say hello to users.", Hello, &head[4]},
    {"time", "Show system date and time.", Time, &head[5]},
    {"author", "Show author information.", Author, &head[6]},
    {"triangle", "Show a big triangle on screen.", Triangle, &head[7]},
    {"heart", "Show a big heart on screen.", Heart, NULL}
};

int main()
{
    char cmd[CMD_MAX_LENGTH];
    while(1)
    {
        printf("menu cmd-> ");
        scanf("%s", cmd);
        tCMDNode* p = FindCmd(head, cmd);      
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
    ShowAllCmd(head);
    printf("*************************************************************\n");
}

void Version()
{
    printf("menu version v2.0\n");
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
֮���ļ��������У�����Դ���봫������
``` bash
gcc menu.c linklist.c -o menu
./ menu
```
``` bash
git add lab3/menu.c lab3/linklist.c lab3/linklist.h
git commit -m "add lab3"
git push
```
## 3. ʵ����
![image](/lab3/img/img1.png)  
![image](/lab3/img/img2.png)  
![image](/lab3/img/img3.png)  

## 4. ʵ���ĵ�
- ѧϰ��������ͨ��������ķ�ʽ�������ҵ�����ָ����ִ����Ӧ�ĺ�������ϰ��C���Ժ����ݽṹ�����֪ʶ��
- ��д����ʱӦ������ҵ���߼��������ݲ�ķ��룬��ߴ���Ŀ���չ�ԺͿ������ԡ�

