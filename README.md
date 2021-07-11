# game
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <windows.h>
#define ROW 9
#define COL 9
#define ROWS ROW+2
#define COLS COL+2
#define layout 10
void initboard(char board[ROWS][COLS],int rows,int cols,char set);
void disboard(char board[ROWS][COLS],int row,int col);
void setmine(char mine[ROWS][COLS],int row,int col);
void play(char mine[ROWS][COLS],char show[ROWS][COLS],int row,int col);
void initboard(char board[ROWS][COLS],int rows,int cols,char set)
{
	int i,j;
	for(i=0;i<=rows;i++)
	{
		for(j=0;j<=cols;j++)
		{
			board[i][j] = set;
		}
	}
}
void disboard(char board[ROWS][COLS],int row,int col)
{
	int i,j;
	for(j=0;j<=col;j++)
	{
		printf("%d ",j);   //这是列标 
	}
	printf("\n");
	for(i=1;i<=row;i++)
	{
		printf("%d ",i);  //行标 
		for(j=1;j<=col;j++)
		{
			printf("%c ",board[i][j]);	
		}
		printf("\n");
	}
	printf("\n");
}
void setmine(char mine[ROWS][COLS],int row,int col)
{
	int x,y;
	int count = layout;   //10个雷 
	while(count)
	{
		//生成随机数 
		x = rand()%row+1;  
		y = rand()%col+1;
		//由于生成的随机数比它本身的范围少1，所以+1 
		if(mine[x][y]=='0')
		{
			mine[x][y]='1';
			count--;
		}
	}		
}
int statistic(char mine[ROWS][COLS],int x,int y)
{
	return 
	mine[x-1][y]+
	mine[x+1][y]+
	mine[x][y-1]+
	mine[x][y+1]+
	mine[x-1][y+1]+
	mine[x+1][y+1]+
	mine[x+1][y-1]+
	mine[x-1][y-1] - 8*'0'; 
	/*坐标周围的雷的个数
	这里是把周围八个坐标每个坐标都转换成数字
     都加起来-8*'0'就是雷的个数*/   
}
void play(char mine[ROWS][COLS],char show[ROWS][COLS],int row,int col)
{
	int x,y;
	int win = 0;  //计数用户排雷时非雷的个数	
	while(win<row*col-layout)   //非雷个数 
	{
		printf("请输入坐标\n");
		scanf("%d%d",&x,&y);
		system("cls");
	 	if(x>=1&&x<=row && y>=1&&y<=col)
		{
			if(mine[x][y]=='1')
			{
				printf("你被炸死了!!!\n");
				disboard(mine,ROW,COL);
				break;
			}
			else
			{
				int count = statistic(mine,x,y);  //统计坐标周围雷的个数
				show[x][y] = count+'0';    //再把得到的数组转换成字符赋给玩家视图 
				disboard(show,ROW,COL);   //显示排雷后的show视图 
				win++;
			}
		}
	}
	if(win==row*col-layout)
	{
		printf("恭喜你，排雷成功\n");
	}
		
}
#include "game2.h"
void mune()
{
	printf("|-----------------------|\n");
	printf("|     1.begin 0.exit    |\n");
	printf("|-----------------------|\n");
}
void game()
{
	char mine[ROWS][COLS]={0};
	char show[ROWS][COLS]={0};
	//初始化模块 
	initboard(mine,ROWS,COLS,'0');  
	initboard(show,ROWS,COLS,'*');
	//打印字符数组模块
	//disboard(mine,ROW,COL);
	disboard(show,ROW,COL);
	//布置雷
	setmine(mine,ROW,COL);
	//disboard(mine,ROW,COL); //布置完雷后把程序视图打印一遍
	//扫雷
	play(mine,show,ROW,COL);
	
}
void test()
{
	int input = 0;
	mune();  //菜单 
	srand((int)time(NULL));
	do
	{
	   printf("请选择>\n");
	   scanf("%d",&input);
	   system("cls");
	   switch(input)
	   {
		  case 1:game();
		    break;
		  case 0:
		  	printf("退出\n");
		  	break;
		  default:printf("请重新选择\n");
		    break;
	   }
		
	}while(input);
}
int main()
{
	test();
	return 0;
}
