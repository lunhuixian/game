#include<stdio.h>
#include<string.h>
#include<stdlib.h>
#include"user.h"

void main()
{
	int i=1;
	Rcard();
	while(i!=0)
	{
	  
		printf("\n\n");  
		printf("\t\t**********************************************\n"); 
		printf("\t\t****************** 主菜单 ********************\n");
		printf("\t\t**                      **\n");  
		printf("\t\t** 1. 注册用户          **\n"); 
		printf("\t\t** 2. 开始游戏          **\n"); 
		printf("\t\t** 3. 游戏排行榜        **\n"); 
		printf("\t\t** 0. 退出              **\n"); 
		printf("\t\t**********************************************\n\n");
		printf("\t\t请选择您所需服务(0-5):");
		scanf("%d",&i);
		switch(i)
		{
				
				case 1:apply();break;
				case 2:game();break;
				case 3:rank();break;
				case 0:write_card() ;break;
		}	
	printf("\n");
	system("cls");
	}
}
