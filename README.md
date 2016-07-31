#include<stdio.h>
#include<string.h>
#include<stdlib.h>
#include"user.h"
#include<malloc.h>
#include <time.h>
struct user{
	char name[20];//用户姓名
	int password;//密码
	int games;
	int win;//赢记录
	int lose;//输记录
}card[100];
int length=0;
//记录制卡数

//读取文件
void Rcard()
{ 
	FILE *fp;
	int i;//循环变量
	if((fp=fopen("card.txt","r"))!=NULL)    /*以输出方式打开*/  
		{   
			i=0;    
			while (!feof(fp))  //循环读取购物卡信息
			{  
				fscanf(fp,"%s %d %d %d %d",card[i].name ,&card[i].password, &card[i].win, &card[i].lose, &card[i].games);  
				i++;  
			}    
			length=i-1;   /*记录制卡数*/  
			fclose(fp);//关闭文件
	} 
}

//写文件，关闭程序
void write_card() 
	/*以只写方式*/ 
{  
	FILE *fp;  
	int i;   
	if((fp=fopen("card.txt","w"))==NULL)/*以输出方式打开 */  
	{   
		printf("写入文件错误!");   

		exit(0); 
	}
	else   
	{
		for(i=0;i<length;i++)
		{  
			fprintf(fp,"%s %d  %d %d %d\n",card[i].name,card[i].password,card[i].win,card[i].lose,card[i].games);  
		}
	}
	fclose(fp); 
}

//申请新的用户
void apply()
{
	int i;
	char name[20];
	int password1,password2;
	char no[3]={'no'};
	printf("\t\t请输入新用户名\n");
	do{
		printf("\t\t");
		scanf("%s",name);
		for(i=0;i<length;i++)
			if(strcmp(card[i].name,name)==0)
			{
				printf("\t\t改名字已存在，请重新输入\n");
				break;
			}
	}while(i!=length);

	printf("\t\t请设定数字密码\n");
	printf("\t\t");
	scanf("%d",&password1);
	printf("\n");
	printf("\t\t请再次输入密码\n");
	printf("\t\t");
	scanf("%d",&password2);
	printf("\n");
	if(password1==password2)
	{
		strcpy(card[i].name,name);
		card[i].password=password1;
		card[i].games=0;
		card[i].lose=0;
		card[i].win=0;
		i++;
		length++;
		printf("\t\t用户申请成功，请记住用户名及密码\n");
		printf("\t\t请输入任意键继续……");
		getch();
	}
	else
	{
		printf("\t\t两次输入密码不相同，注册失败！\n");
		printf("\t\t请输入任意键继续……");
		getch();
	}
	
}

//输出账号信息
void read_card()
{
	int i;
	i=online();
	if(i<0||i>=length)
	{
		printf("\t\t该用户不存在\n");
		printf("\t\t请输入任意键继续……");
		getch();
		return;
	}
	printf("\t\t账号名\t赢/盘\t输/盘\t总局数\n");
	printf("\t\t%s\t\t%d\t\t%d\t\t%d\n",card[i].name,card[i].win,card[i].lose,card[i].games);

	printf("\t\t请输入任意键继续……\n");
	getch();

}

//登录
int online()
{
	int i;
	char name[20];
	int password;
	printf("\t\t请输入账号\n");
	printf("\t\t");
	scanf("%s",name);

	for(i=0;i<length;i++)
		if(strcmp(card[i].name,name)==0)
		{
				printf("\t\t请输入密码\n");
				printf("\t\t");
				scanf("%d",&password);
				if(card[i].password==password)
				{
					return i;
				}
				else
				{
					return -1;
				}
		}
		else{
			return -1;
		}
		return -1;
}

//游戏主体
int game()
{
	//登陆
	int i;
	i=online();
	if(i<0||i>=length)
	{
		printf("\t\t该用户不存在或密码不对\n");
		printf("\t\t请输入任意键继续……");
		getch();
		return 0;
	}
	//游戏
	{
		int m;	//表示选择的是哪个
		int a,b;	//分别表示人和电脑
		printf("\t\t(1)剪刀, (2)石头, (3)布 （请输入对应的数字）:\n\t\t");
		scanf("%d",&m);
		if(m!=1 && m!=2 && m!=3)
			printf("Input error!\n");
		else	//分别用0,1,2代表石头，剪刀，布
		{
			srand(time(NULL));
			a = rand()%3;
			switch(a)
			{
				case 0:
					printf("\t\t人出的是石头\n");
					break;
				case 1:
					printf("\t\t人出的是剪刀\n");
					break;
				case 2:
					printf("\t\t人出的是布\n");
					break;
				default:
					break;
			}
			b = rand()%3;
			switch(b)
			{
				case 0:
					printf("\t\t电脑出的是石头\n");
					break;
				case 1:
					printf("\t\t电脑出的是剪刀\n");
					break;
				case 2:
					printf("\t\t电脑出的是布\n");
					break;
				default:
					break;
			}
		if(a > b)
		{
			printf("\t\t人赢了！\n");
			card[i].win++;
			card[i].games++;
		}
		else if(a == b)
		{
			printf("\t\t平局！\n");
			card[i].games++;
		}
		else{
			printf("\t\t电脑赢了！\n");
			card[i].lose++;
			card[i].games++;
			}
		}
		
		getch();
		return 0;
	}
}

//玩家排行榜
void rank()
{
	int i,index,j;
	int sum=0;
	for(i=0;i<length-1;i++)
	{
		index=i;
		for(j=i+1;j<length;j++)
		{
			if(card[j].win>card[index].win)
				index=j;
		}
		card[length]=card[i];
		card[i]=card[index];
		card[index]=card[length];
	}
	printf("\t\t账号名\t赢/盘\t输/盘\t总局数\t排名\n");
	for(i=0;i<length;i++)
	{
		if((i+1)%10==0)
		{
			printf("\t\t请输入任意键继续进行查看下一页……\n");
			getch();
			printf("\n\t\t第%d页\n",((i+1)/10)+1);
			printf("\t\t账号名\t赢/盘\t输/盘\t总局数\t排名\n");
		}
		printf("\t\t%s\t%d\t%d\t%d\t%d\n",card[i].name,card[i].win,card[i].lose,card[i].games,i+1);
		
	}
	printf("\n\t\t游戏已经成功运行：");
	for(i=0;i<length;i++)
	{
		sum+=card[i].games;
	}
	printf("%d次",sum);
	printf("\n\t\t请输入任意键返回……");
	getch();
}
