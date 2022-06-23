#include "stdio.h"
#include "stdlib.h"
#include "string.h"
typedef struct
{
	int num;          //账户
	char name[50];   //姓名
	int password;   //密码
	double cash;    //余额
	int type;       //状态
	
}Account;

void kaihu(Account *arr);      //开户

void cunkuan(Account *arr);    //存款

void qukuan(Account *arr);     //取款

void find(Account *arr);       //查询

void zhuanzhang(Account *arr); //转账

void guashi(Account *arr);     //挂失

void jiegua(Account *arr);     //解挂

void xiaohu(Account *arr);     //销户

void gaimi(Account *arr);      //改密

void map();

int main()
{
	
	Account zh[100];

	int i;
	FILE *p;
	if((p=fopen("D:\\账户.txt","a+"))==NULL)
	{
		printf("打开文件失败!\n");
		exit(0);
	}
	fclose(p);
	p=fopen("D:\\账户.txt","r");
	for(i=0;i<100;i++)
	{
		fread(&zh[i],sizeof(Account),1,p);
	}
	
	fclose(p);
	do
	{	
		map();
		int a;
		scanf("%d",&a);
		if(a==10)
			break;
		
		switch(a)
		{
		case 1:
			kaihu(zh);
			break;
			
		case 2:
			cunkuan(zh);
			break;
			
		case 3:
			qukuan(zh);
			break;
			
		case 4:
			find(zh);
			break;
			
		case 5:
			zhuanzhang(zh);
			break;
			
		case 6:
			guashi(zh);
			break;
			
		case 7:
			jiegua(zh);
			break;
			
		case 8:
			xiaohu(zh);
			break;
			
		case 9:
			gaimi(zh);
			break;
			
		default :
			printf("没有此项操作!\n");
			break;
		}
		
	}while(1);
	
	return 0;
}

void map()
{
	printf("------------------------------------\n");
	printf("1.开户  2.存款  3.取款  4.查询  5.转账     \n");
	printf("6.挂失  7.解挂  8.销户  9.改密  10.退出系统\n");
	printf("------------------------------------\n");
	printf("输入你想要的进行的操作:");
}

void kaihu(Account *arr)    //开户
{
	
	int index=10000,i,n;
	
	for(i=0;i<100;i++)
	{
		if(strlen(arr[i].name)>=40)
		{
			break;
		}
		index++;
	}
	
	arr[i].num=index;

	printf("账户:%d\n",(arr+i)->num);
	printf("用户名:");
	getchar();
	gets((arr+i)->name);

	printf("请输入6位数字密码:");
	scanf("%d",&n);
	
	if(n<100000||n>999999)
	{
		printf("请输入6位数字密码:");
		
		scanf("%d",&n);
		if(n<100000||n>999999)
		{
			printf("非法操作!\n");
			return;
		}
	}
	printf("确认密码:");
	scanf("%d",&(arr+i)->password);

	if(n!=(arr+i)->password)
	{
		printf("两次密码不一致,未成功开户!\n");
		return;
	}

	printf("成功开户!\n");
	(arr+i)->cash=0;
	(arr+i)->type=0;
	(arr+i)->num=index;
	FILE *q;
    q=fopen("D:\\账户.txt","w");
	int j;
	
	for(j=0;j<100;j++)
	{
		fwrite(&arr[j],sizeof(Account),1,q);
	}
	
	fclose(q);
	
}

void cunkuan(Account *arr)
{
	int index,m,n;
	double t;
	printf("请输入账户:\n");
	scanf("%d",&m);
	
	for(index=0;index<100;index++)
	{
		
		if(arr[index].num==m)
		{
			break;
		}
	}
	
	if(index==100)
	{
		printf("对不起,无此账号!\n");
		return;
	}
	
	printf("请输入6位数字密码:\n");
	scanf("%d",&n);
	
	if((arr+index)->password!=n)
	{
		printf("密码错误,请重新输入:\n");
		scanf("%d",&n);
		
		if((arr+index)->password!=n)
		{
			printf("密码错误,请重新输入:\n");
			scanf("%d",&n);
			
			if((arr+index)->password!=n)
			{
				return;
			}
		}
	}
	printf("户主姓名:%s\n",(arr+index)->name);

	printf("请输入需要存钱金额:");
	scanf("%lf",&t);

	(arr+index)->cash+=t;
	printf("您当前的余额为:%lf元\n",(arr+index)->cash);

	FILE *q;
                q=fopen("D:\\账户.txt","w");
	
	int i;

	for(i=0;i<100;i++)
	{
		fwrite(&arr[i],sizeof(Account),1,q);
	}

	fclose(q);
}

void qukuan(Account *arr)
{	
	int index,m,n;
	double t;
	printf("请输入账户:\n");
	scanf("%d",&m);
	
	for(index=0;index<100;index++)
	{
		if((arr+index)->num==m)
		{
			break;
		}
	}
	
	if(index==100)
	{
		printf("对不起,无此账号!\n");
		return; 
	}
	
	printf("请输入6位数字密码:\n");
	scanf("%d",&n);
	
	if((arr+index)->password!=n)
	{
		printf("密码错误,请重新输入:\n");
		scanf("%d",&n);
		
		if((arr+index)->password!=n)
		{
			printf("密码错误,请重新输入:\n");
			scanf("%d",&n);
			
			if((arr+index)->password!=n)
			{
				return;
			}
		}
	}
	printf("户主姓名:%s\n",(arr+index)->name);
	printf("请输入取款金额:");
	scanf("%lf",&t);
	
	if(t>(arr+index)->cash)
	{
		printf("余额不足,取款失败!\n");
		return;
	}
    (arr+index)->cash-=t;
	printf("您当前的余额为:%lf元\n",(arr+index)->cash);
	FILE *q;
    q=fopen("D:\\账户.txt","w");
	
	int i;
	for(i=0;i<100;i++)
	{
		fwrite(&arr[i],sizeof(Account),1,q);
	}
	fclose(q);
}

void find(Account *arr)
{
	int zh;
	
	printf("请输入您的账号\n");
	
	scanf("%d",&zh);
	
	for(int i=0;i<100;i++)
	{
		if(zh==arr[i].num)
		{
			break;
		}
	}
	
	if(i==100)
	{
		printf("未知账号\n");
		return ;
	}
	
	printf("请输入您的密码\n");
	
	int mima;
	
	scanf("%d",&mima);
	
	if((arr+i)->password!=mima)
	{
		printf("密码错误\n");
		return ;
	}
	
	printf("信息认证成功，您好: %s 先生/女士\n",(arr+i)->name);
	
	printf("您当前的余额为%lf\n",(arr+i)->cash);	
}



void zhuanzhang(Account *arr)
{
	int zh1;
	
	printf("请输入您的账号\n");
	
	scanf("%d",&zh1);
	
	for(int i1=0;i1<100;i1++)
	{
		if(zh1==arr[i1].num)
		{
			break;
		}
	}
	
	if(i1==100){
		printf("未知账号\n");
		return ;
	}
    
	printf("请输入您的密码\n");
	
	int mima;
	
	scanf("%d",&mima);
	
	if((arr+i1)->password!=mima)
	{
		printf("密码错误\n");
		return ;
	}
	
	printf("信息认证成功，您好: %s 先生/女士\n",(arr+i1)->name);
	
	printf("请输入转账的对象的账号\n");
	
	int zh2;
	
	scanf("%d",&zh2);
	
	for(int i2=0;i2<100;i2++)
	{
		if(zh2==(arr+i2)->num)
		{
			break;
		}
	}
	
	if(i2==100){
		printf("未知账号\n");
		return ;
	}
	

	printf("请输入转账金额.\n");
	
	int money;
	
	scanf("%d",&money);
	
	if((arr+i1)->cash-money<0)
	{
		printf("对不起，您的账户余额不足\n");
		return ;
	}
	
                 (arr+i1)->cash-=money;
	
	(arr+i2)->cash+=money;
	
	int index=0;
	
	for(int x=0;x<100;x++)
	{
		if(strlen((arr+x)->name)>=10)
		{
			break;
		}
		index++;
	}
	
	FILE *fp;            //全部写入文件
	
	if((fp=fopen("D://.txt","w"))==NULL)
	{
		printf("不能打开文件\n");
		exit(0);
	}
	
	for(int m=0;m<index;m++)
	{
		fwrite(&arr[m],sizeof(Account),1,fp);
		
	}
	
	fclose(fp);
}

void guashi(Account *arr)        //挂失
{
	
	
	int index=0;
	
	for(int x=0;x<100;x++)
	{            //index为已经创建的用户.
		if(strlen((arr+x)->name)>=10)
		{
			break;
		}
		index++;
	}
	
	int zh;
	
	printf("请输入您的账号\n");
	
	scanf("%d",&zh);
	
	for(int i=0;i<100;i++)
	{        //确认用户信息的位置，为下标;
		if(zh==(arr+i)->num)
		{
			break;
		}
	}
	
	if(i==100){
		printf("未知账号\n");
		return ;
	}
	
	printf("请输入您的密码\n");
	
	int mima;
	
	scanf("%d",&mima);
	
	if((arr+i)->password!=mima)
	{
		printf("密码错误\n");
		return ;
	}
	
	printf("信息认证成功，您好: %s 先生/女士\n",(arr+i)->name);
	
	printf("您确定要冻结该用户?  ( 1: 确认 ; 0: 取消)\n");
	
	int judge;
	
	scanf("%d",&judge);
	
	if(judge==0||judge==1)
	{
		
		if(judge==0)
		{
			printf("已取消冻结该用户.\n");
			return ;
		}
	}
	else
	{
		printf("错误的操作.\n");
		return ;
	}
	
	FILE *fp;
	
	if((fp=fopen("D://账户.txt","w"))==NULL)
	{
		printf("不能打开文件\n");
		exit(0);
	}
	
	for(int m=0;m<index;m++)
	{
		fwrite(&arr[m],sizeof(Account),1,fp);
		
	}
	
	fclose(fp);
	
	printf("已冻结该用户\n");
}

void jiegua(Account *arr)      //解挂
{
	int index=0;
	
	for(int x=0;x<100;x++)
	{            //index为已经创建的用户.
		if(strlen((arr+x)->name)>=10)
		{
			break;
		}
		index++;
	}
	
	int zh;
	
	printf("请输入您的账号\n");
	
	scanf("%d",&zh);
	
	for(int i=0;i<100;i++)
	{        //确认用户信息的位置，为下标;
		if(zh==(arr+i)->num)
		{
			break;
		}
	}
	
	if(i==100)
	{
		printf("未知账号\n");
		return ;
	}
	
	printf("请输入您的密码\n");
	
	int mima;
	
	scanf("%d",&mima);
	
	if((arr+i)->password!=mima)
	{
		printf("密码错误\n");
		return ;
	}
	
	printf("信息认证成功，您好: %s 先生/女士\n",(arr+i)->name);
	
	printf("您确定要解冻该用户?  ( 1: 确认 ; 0: 取消)\n");
	
	int judge;
	
	scanf("%d",&judge);
	
	if(judge==0||judge==1)
	{
		
		if(judge==0)
		{
			printf("已取消解冻该用户.\n");
			return ;
		}
	}
	else{
		printf("错误的操作.\n");
		return ;
	}
	
	//arr[i].zt=1;
	
	FILE *fp;
	
	if((fp=fopen("D://账户.txt","w"))==NULL)
	{
		printf("不能打开文件\n");
		exit(0);
	}
	
	for(int m=0;m<index;m++)
	{
		fwrite(&arr[i],sizeof(Account),1,fp);  //  每次修改信息后全部重新写入文件.
	}
	
	fclose(fp);
	
	printf("该用户已解冻\n");
	
	
}

void xiaohu(Account *arr)    //销户
{
	int index=0;
	
	for(int x=0;x<100;x++)
	{            //index为已经创建的用户.
		if(strlen((arr+x)->name)>=10){
			break;
		}
		index++;
	}
	
	int zh;
	
	printf("请输入您的账号\n");
	
	scanf("%d",&zh);
	
	for(int i=0;i<100;i++)
	{        //确认用户信息的位置，为下标;
		if(zh==(arr+i)->num)
		{
			break;
		}
	}
	
	if(i==100)
	{
		printf("未知账号\n");
		return ;
	}
	
	printf("请输入您的密码\n");
	
	int mima;
	
	scanf("%d",&mima);
	
	if((arr+i)->password!=mima)
	{
		printf("密码错误\n");
		return ;
	}
	
	printf("信息认证成功，您好: %s 先生/女士\n",(arr+i)->name);
	
	printf("确认要注销此账户?  ( 1: 是, 0: 否)\n");
	
	int judge;
	
	scanf("%d",&judge);
	
	if(judge==0||judge==1)
	{
		
		if(judge==0)
		{
			printf("已取消注销该用户.\n");
			return ;
		}
	}
	else
	{
		printf("错误的操作.\n");
		return ;
	}
	
	FILE *fp;
	
	if((fp=fopen("D://账户.txt","w"))==NULL)
	{
		printf("不能打开文件\n");
		exit(0);
	}
	
	for(int m1=0;m1<i;m1++)
	{
		fwrite(&arr[m1],sizeof(Account),1,fp);   //  写入销户账号上半部分.
	}
	
	fclose(fp);
	
	if((fp=fopen("D://账户.txt","w"))==NULL)
	{
		printf("不能打开文件\n");
		exit(0);
	}
	
	for(int m2=i+1;m2<index;m2++)
	{
		fwrite(&arr[m2],sizeof(Account),1,fp);    //  写入销户账号下半部分.
	}
	
	fclose(fp);
	
	if((fp=fopen("D://账户.txt","r"))==NULL)
	{
		printf("不能打开文件\n");
		exit(0);
	}
	
	for(int b=0;b<100;b++)
	{
		fread(&arr[b],sizeof(Account),1,fp);    //  重新读取文件中的信息进入数组
	}
	
	fclose(fp);
	
	printf("已注销该用户，请重新登录.\n");
	
	system("pause");
	
	exit(0);
}

void gaimi(Account *arr)   //改密
{
	int index=0;
	
	for(int x=0;x<100;x++)
	{            //index为已经创建的用户.
		if(strlen((arr+x)->name)>=10)
		{
			break;
		}
		index++;
	}
	
	int zh;
	
	printf("请输入您的账号\n");
	
	scanf("%d",&zh);
	
	for(int i=0;i<100;i++)
	{        //确认用户信息的位置，为下标;
		if(zh==(arr+i)->num)
		{
			break;
		}
	}
	if(i==100)
	{
		printf("未知账号\n");
		return ;
	}
	
	printf("请输入您的密码\n");
	
	int mima;
	
	scanf("%d",&mima);
	
	if((arr+i)->password!=mima)
	{
		printf("密码错误\n");
		return ;
	}
	
	printf("信息认证成功，您好: %s 先生/女士\n",(arr+i)->name);
	
	int gmima;
	
	while(1){
		
		printf("请输入新密码(六位数)\n");
		
		scanf("%d",&(arr+i)->password);
		
		if(100000>(arr+i)->password||(arr+i)->password>=1000000)
		{
			printf("请输入6位数的密码\n");
			continue;
		}
		
		printf("请再次输入密码\n");
		
		scanf("%d",&gmima);
		
		if((arr+i)->password!=gmima)
		{
			printf("两次密码输入不同\n");
			continue;
		}
		
		break;
		
	}
	
	FILE *fp;
	
	if((fp=fopen("D://账户.txt","w"))==NULL)
	{
		printf("不能打开文件\n");
		exit(0);
	}
	
	for(int a=0;a<index;a++)
	{
		fwrite(&arr[a],sizeof(Account),1,fp);
	}
	
	fclose(fp);
	
	printf("当前账号为:%d , 用户名为: , %s 密码为: %d\n",arr[i].num,arr[i].name,arr[i].password);
}
