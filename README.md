# ATM2
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<time.h>

struct Transaction
{
	char username[100];
	time_t timestamp;
	int type;//0-取款，1-存款
	int amount;
	
	struct Transaction * next;
};
typedef struct Transaction Transaction;

//交易记录信息链表 
Transaction * tHead=NULL;
Transaction * tTail=NULL;


struct Account
{
	char username[100];
	char password[100];	
	
	int money;
	
	struct Account * next;
};
typedef struct Account Account;

//账户信息链表 
Account * head=NULL;//指向头结点的指针
Account * tail=NULL;//指向尾结点的指针 
Account * curAccount=NULL;//指向当前登录账户的指针 

//找到返回1，否则返回0 
int findAccount(Account a)
{
	Account *curP=head;
	while(curP!=NULL)
	{
		if((strcmp(curP->username,a.username)==0)&&(strcmp(curP->password,a.password)==0))
		{
			curAccount=curP;
			return 1;
		}
		curP=curP->next;
	} 
	return 0;
}

void updatePassword()
{
	printf("请输入旧密码:\n");
	char oldPassword[100]={'\0'};
	scanf("%s",oldPassword);
	if(strcmp(oldPassword,curAccount->password)==0)
	{
		//让其修改 
		printf("请输入新密码：\n");
		scanf("%s",curAccount->password);
		printf("修改成功！\n");
	}
	else
	{
		printf("旧密码输入错误，拒绝修改！\n");
	} 
	
}

void drawMoney()
{
	printf("请输入取款金额：");
	int money;
	scanf("%d",&money);
	
	//验证金额
	if(curAccount->money>=money)
	{
		printf("取款成功！\n");
		curAccount->money-=money;
		
		//产生交易记录...	
		
		//创建结点
		Transaction * newNode=(Transaction*)malloc(sizeof(Transaction)); 
		
		//结点初始化
		newNode->next=NULL;
		strcpy(newNode->username,curAccount->username);
		newNode->timestamp=time(NULL);
		newNode->type=0;
		newNode->amount=money;
		
		//添加结点到链表
		if(tHead==NULL)
		{
			tHead=newNode;
			tTail=newNode;
		}
		else
		{
			tTail->next=newNode;
			tTail=newNode;
		}
	} 
	else
	{
		printf("余额不足！\n");
	}
}

void saveMoney()
{
	printf("请输入存款金额：");
	int money;
	scanf("%d",&money);
	printf("存款成功！\n");
	curAccount->money+=money;
	
	//产生交易记录...	
	
}

void homePage()
{
	system("cls");
	//updatePassword(); 
	
	drawMoney();
	//saveMoney();
}

void signIn()
{
	for(int i=0;i<3;i++)
	{
		Account a; 
		printf("欢迎登录\n");
		printf("请输入账号：\n");
		scanf("%s",a.username);
		
		printf("请输入密码：\n");
		scanf("%s",a.password);
		
		if(findAccount(a))
		{
			homePage();
			return;
		}
		else
		{
			printf("登录失败!\n");
		}
	} 
}

/**
如果数据加载成功返回1
否则返回0 
*/
int loadData()
{
	FILE* fp=fopen("D:/atm.txt","r");
	if(fp==NULL)
	{
		return 0; 
	}
	else
	{
		while(!feof(fp))
		{
			//创建结点：在堆上申请一块内存空间，将其地址赋值给指针newNode
			Account * newNode=(Account *)malloc(sizeof(Account)); 
			
			//结点赋值：结点初始化
			newNode->next=NULL;
			fscanf(fp,"%s %s %d\n ",newNode->username,newNode->password,&newNode->money);
			
			//添加结点到链表
			if(head==NULL)
			{
				head=newNode;
				tail=newNode;
			}
			else
			{
				tail->next=newNode;
				tail=newNode;
			}
		}
		return 1;
	}

}

void saveTransaction()//交易记录 
{
	
}

int main()
{
	int status=loadData();//加载账户信息 
	//loadTransaction();//加载交易记录信息
	 
	if(status==1)
	{
		printf("加载成功！\n");
	}
	else
	{
		printf("加载失败！\n");
	}
	
	
	signIn();
	
	//saveData();//保存账户信息
	saveTransaction();//保存交易记录 
	return 0;
} 
