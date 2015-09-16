# ugly-2048
2048 in C, and it's ugly without UI

#include<stdio.h>
#include<stdlib.h>
#include<conio.h>
#define N 4
int main()
{
    void sym_ctl(int ALL_2048[N][N]);
    void sym_dia(int ALL_2048[N][N]);
    void sym_bkdia(int ALL_2048[N][N]);
    void leftall(int ALL_2048[N][N]);
    void randnewn(int ALL_2048[N][N]);
    printf("version 0.0.1\n");

    int ALL_2048[N][N]={{0}},temp_2048[N][N]={{0}},back1_2048[N][N]={{0}};
    randnewn(ALL_2048);
    randnewn(ALL_2048);
    int i,j;
    char direc;
    int ifchan=1;


    while(1)
    {   
    
//为数据进行初始备份，并用于后续比较数据是否变化  

        for(i=0;i<N;i++)
            for(j=0;j<N;j++)
                temp_2048[i][j]=ALL_2048[i][j];

//输入方向并处理

        direc=getch();
        if (direc=='q'||direc=='Q')break;
        switch(direc)
        {
            case 'A' :case 'a':
            {
                leftall(ALL_2048);
                break;
            }

            case 'D' :case 'd':
            {
                sym_ctl(ALL_2048);
                leftall(ALL_2048);
                sym_ctl(ALL_2048);
                break;
            }

            case 'W' :case 'w':
            {
                sym_dia(ALL_2048);
                leftall(ALL_2048);
                sym_dia(ALL_2048);
                break;
            }
            case 'S' :case 's':
            {
                sym_bkdia(ALL_2048);
                leftall(ALL_2048);
                sym_bkdia(ALL_2048);
                break;
            }
            case 'B' :case 'b':
            {
                for(i=0;i<N;i++)
                    for(j=0;j<N;j++)
                    ALL_2048[i][j]=back1_2048[i][j];
                break;
            }
            default :;
        }
        
//留下上一次的备份

        for(i=0;i<N;i++)
            for(j=0;j<N;j++)
                back1_2048[i][j]=temp_2048[i][j];

//判断数据是否变化 ，若变化，则产生随机数

        ifchan=0;
        for(i=0;i<N;i++)
            for(j=0;j<N;j++)
                if(temp_2048[i][j]!=ALL_2048[i][j])ifchan=1;
        if(ifchan==1&&direc!='b'&&direc!='B')
            {randnewn(ALL_2048);}

//输出此次操作结果

        for(i=0;i<N;i++)
           {
                for(j=0;j<N;j++) printf("%d\t",ALL_2048[i][j]);
                printf("\n\n");
            }
        printf("\n\n");
    }
    return 0;
}

//中心线对称

void sym_ctl(int ALL_2048[N][N])
{
    int i,j,temp;
    for(i=0;i<N;i++)
        for(j=0;j<(N/2);j++)
        {
            temp=ALL_2048[i][j];
            ALL_2048[i][j]=ALL_2048[i][N-1-j];
            ALL_2048[i][N-1-j]=temp;
        }
}

//正向对角线对称

void sym_dia(int ALL_2048[N][N])
{
    int i,j,temp;
    for(i=0;i<N-1;i++)
        for(j=i+1;j<N;j++)
        {
            temp=ALL_2048[i][j];
            ALL_2048[i][j]=ALL_2048[j][i];
            ALL_2048[j][i]=temp;
        }
}

//反向对角线对称

void sym_bkdia(int ALL_2048[N][N])
{
    int i,j,temp;
    for(i=0;i<N-1;i++)
        for(j=0;j+i<N-1;j++)
        {
            temp=ALL_2048[i][j];
            ALL_2048[i][j]=ALL_2048[N-1-j][N-1-i];
            ALL_2048[N-1-j][N-1-i]=temp;
        }
}

//整体数据左移

void leftall(int ALL_2048[N][N])
{
    int i;
    void leftadd(int array[N]);//单行数据整体左移
    for(i=0;i<N;i++)
        leftadd(ALL_2048[i]);
}

//单行数据整体左移

void leftadd(int array[N])
{

    /*将所有0往右移动*/
    
    int n=N,m,i;
    for(m=0;m<n-1;m++)
    {
        while(array[m]==0)
        {

            for(i=m;i<n-1;i++)array[i]=array[i+1];//完成m位置0提取至最右
            array[n-1]=0;
            int num=0;
            for(i=m;i<n;i++)num+=array[i];//提取0后获取m位置起后续数字的和
            if(num==0)break;
        }
    }

    /*若有两两相等，则往左合并*/
    
    for(m=0;m<n-1;m++)
        if(array[m]==array[m+1])
            {
                array[m]+=array[m+1];
                array[m+1]=0;
            }


    /*再次将所有0往右移动*/
    
    for(m=0;m<n-1;m++)
    {
        while(array[m]==0)
        {

            for(i=m;i<n-1;i++)array[i]=array[i+1];//完成m位置0提取至最右
            array[n-1]=0;
            int num=0;
            for(i=m;i<n;i++)num+=array[i];//提取0后获取m位置起后续数字的和
            if(num==0)break;
        }
    }
}

//随机产生2或4

void randnewn(int ALL_2048[N][N])
{
    int randi,randj,randnum;
    while(1)
    {
        randi=rand()%4;randj=rand()%4;
        randnum=(rand()%2+1)*2;
        if(ALL_2048[randi][randj]==0)
        {
            ALL_2048[randi][randj]=randnum;
            break;
        }

    }
}

