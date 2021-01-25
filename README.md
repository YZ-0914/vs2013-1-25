# vs2013-1-25
#game.h
#define _CRT_SECURE_NO_WARNINGS 1
#define ROW 3
#define COL 3

#include<stdio.h>
#include<stdlib.h>
#include<time.h>

//声明
void InitBoard(int board[ROW][COL], int row, int col);
void DisplayBoard(char board[ROW][COL], int row, int col);
void PlayerMove(char board[ROW][COL],int row,int col);
void ComputerMove(char board[ROW][COL], int row, int col);
//告诉我们四种游戏状态
//玩家赢-'*'
//电脑赢-'#'
//平局-'Q'
//继续-'C'
char IsWin(char board[ROW][COL], int row, int col);
int IsFull(char board[ROW][COL], int row, int col);

#game.c
#define _CRT_SECURE_NO_WARNINGS 1
#include"game.h"
#include<stdio.h>
void InitBoard(int board[ROW][COL], int row, int col)
{
	int i = 0;
	int j = 0;
	for (i = 0; i < row; i++)
	{
		for (j = 0; j < col; j++)
		{
			board[i][j] = ' ';
		}
	}
}

//void DisplayBoard(char board[ROW][COL], int row, int col)
//{
//	int i = 0;
//	for (i = 0; i < row; i++)
//	{
//		//1.打印一行的数据
//		printf(" %c | %c | %c \n",board[i][0],board[i][1],board[i][2]);
//		//2.打印分割行
//		if (i<row-1)
//			printf("---|---|---\n");
//		
//	}
//}

void DisplayBoard(char board[ROW][COL], int row, int col)
{
	int i = 0;
	for (i = 0; i < row; i++)
	{
		//1.打印一行的数据
		int j = 0;
		for (j = 0; j < col; j++)
		{
			printf(" %c ", board[i][j]);
			if (j<col-1)
				printf("|");
		}
		printf("\n");
		//2.打印分割行
		if (i < row - 1)
		{
			for (j = 0; j < col; j++)
			{
				printf("---");
				if (j<col-1)
				    printf("|");
			}
			printf("\n");
		}

	}
}

void PlayerMove(char board[ROW][COL], int row, int col)
{
	int x = 0;
	int y = 0;
	printf("玩家走:>\n");
	while (1)
	{
		printf("请输入要下的坐标:>");
		scanf("%d%d", &x, &y);
		//判断x,y坐标的合法性
		if (x >= 1 && x <= row && y >= 1 && y <= col)
		{
			if (board[x - 1][y - 1] == ' ')
			{
				board[x - 1][y - 1] = '*';
				break;
			}
			else
			{
				printf("该坐标被占用\n");
			}
		}
		else
		{
			printf("坐标非法，请重新输入！\n");
		}
	}
}

void ComputerMove(char board[ROW][COL], int row, int col)
{
	int x = 0;
	int y = 0;
	printf("电脑走:>\n");
	while (1)
	{
		x = rand() % row;
		y = rand() % col;
		if (board[x][y] == ' ');
		{
			board[x][y] = '#';
			break;
		}
		
	}
	

}
//返回1表示棋盘满了
//返回0表示棋盘没满
int IsFull(char board[ROW][COL], int row, int col)
{
	int i = 0;
	int j = 0;
	for (i = 0; i < row; i++)
	{
		for (j = 0; j < col; j++)
		{
			if (board[i][j] == ' ')
			{
				return 0;
			}
		}
	}
	return 1;//满了
}
char IsWin(char board[ROW][COL], int row, int col)
{
	int i = 0;
	//横三行
	for (i = 0; i < row; i++)
	{
		if (board[i][0] == board[i][1] && board[i][1] == board[i][2] &&board[i][1] != ' ')
		{
			return board[i][1];
		}
	}
	//竖三列
	for (i = 0; i < col; i++)
	{
		if (board[0][i] == board[1][i] && board[1][i] == board[2][i] && board[1][i] != ' ')
		{
			return board[1][i];
		}
	}
	//两个对角线
	if (board[0][0] == board[1][1] && board[1][1] == board[2][2] && board[1][1] != ' ')
		{
			return board[1][1];
		}
	if (board[2][0] == board[1][1] && board[1][1] == board[0][2] && board[1][1] != ' ')
	{
		return board[1][1];
	}
	//判断是否平局
	if (1 == IsFull(board, ROW, COL))
	{
		return 'Q';
	}
	return 'C';
}

#三子棋.c
#define _CRT_SECURE_NO_WARNINGS 1
#include<stdio.h>
#include"game.h"
//测试三子棋游戏
void menu()
{
	printf("**************************************\n");
	printf("*********   1.play   0.exit  *********\n");
	printf("**************************************\n");
}

//   |   |
//---|---|---
//   |   |   
//---|---|---
//   |   |
//游戏的整个算法实现
void game()
{
	char ret = 0;
	//数组-存放走出的棋盘信息
	char board[ROW][COL] = { 0 };//全部空格
	//初始化棋盘
	InitBoard(board, ROW, COL);
	//打印棋盘
	DisplayBoard(board,ROW,COL);
	//下棋
	while (1)
	{
		//玩家下棋
		PlayerMove(board,ROW,COL);
		DisplayBoard(board, ROW, COL);
		//判断玩家是否赢
		ret = IsWin(board, ROW, COL);
		if (ret != 'C')
		{
			break;
		}
		//电脑下棋
		ComputerMove(board, ROW, COL);
		DisplayBoard(board, ROW, COL);
		//判断电脑是否赢
		ret = IsWin(board, ROW, COL);
		if (ret != 'C')
		{
			break;
		}
	}
	if (ret == '*')
	{
		printf("玩家赢\n");
	}
	else if (ret=='#')
	{
		printf("电脑赢\n");
	}
}

void test()
{
	int input = 0;
	srand((unsigned int)time(NULL));
	do
	{
		menu();
		printf("请选择:>");
		scanf("%d", &input);
		switch (input)
		{
		case 1:
			game();
			//printf("三子棋\n");
			break;
		case 0:
			printf("退出游戏\n");
			break;
		default:
			printf("选择错误，请重新选择！\n");
			break;
		}
	} while (input);
}

int main()
{
	test();
	return 0;
}

#扫雷.c
#define _CRT_SECURE_NO_WARNINGS 1
#include"game.h"
void 开始游戏()
{
	char 扫雷界面[LINES][ROWS] = { 0 };
	char 显雷界面[LINES][ROWS] = { 0 };
	初始化扫雷界面(扫雷界面, LINES, ROWS, '0');
	初始化扫雷界面(显雷界面, LINES, ROWS, '*');
	打印扫雷界面(扫雷界面,LINE, ROW);
}
void menu()
{
	printf("********************************\n");
	printf("******  1.play   2.exit   ******\n");
	printf("********************************\n");
}

void 扫雷小游戏()
{
	int input = 0;
	do
	{
		menu();
		printf("请选择:>");
		scanf("%d", &input);
		switch (input)
		{
		case 1:
			开始游戏();
			break;
		case 0:
			printf("退出游戏");
		default:
			printf("输入错误");
			break;
		}
	} while (input);
}

int main()
{
	扫雷小游戏();
	return 0;
}

#game.h
#define _CRT_SECURE_NO_WARNINGS 1
#include<stdio.h>
#define LINE 9
#define ROW 9

#define LINES LINE+2

#define ROWS ROW+2

void 初始化扫雷界面(char 扫雷界面[LINES][ROWS], int lines, int rows, char set);
void 打印扫雷界面(char 扫雷界面[LINES][ROWS], int line, int row);

#game.c
#define _CRT_SECURE_NO_WARNINGS 1
#include"game.h"
void 初始化扫雷界面(char 扫雷界面[LINES][ROWS], int lines, int rows, char set)
{
	int i = 0;
	int j = 0;
	for (i = 0; i < lines; i++)
	{
		for (j = 0; j < rows; j++)
		{
			扫雷界面[i][j] = set;
		}
	}
}

void 打印扫雷界面(char 扫雷界面[LINES][ROWS], int line, int row)
{
	int i = 0;
	int j = 0;
	for (i = 0; i < line; i++)
	{
		for (j = 0; j < row; j++)
		{
			printf("%c ", 扫雷界面[i][j]);
		}
		printf("\n");
	}
}
