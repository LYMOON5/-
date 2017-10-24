#include<stdio.h>
#include<stdarg.h>
#include<stdlib.h>

#if 0
//求n个数的平均数
//n的第一个作用用于提示...的位置
//n的第二个作用用于记录...数据的个数
int Avg(char n,...)
{
	int sum = 0;
	va_list list;//定义一个指针 //char *list
	va_start(list ,n);//将指针定位到...的开头

	for(int i = 0;i<n;i++)
	{
		sum += va_arg(list,int);//从...中取出int型的数据
	}
	va_end(list);//关闭指针//list = NULL;
	return sum / n;
}

int main()
{
	printf("%d\n",Avg(1,10));
	printf("%d\n",Avg(2,10,20));
	printf("%d\n",Avg(3,10,20,30));
	printf("%d\n",Avg(4,10,20,30,40));
	printf("%d\n",Avg(5,10,20,30,40,50));
	return 0;
}
#endif

//可变参数实现printf函数
void MyPrint(const char *str,...)
{
	va_list list;
	char ch;
	int num;
	double f;
	char *pstr = NULL;
	va_start(list,str);

	while(*str != '\0')
	{
		if(*str == '%')
		{
			switch(*(str+1))
			{
			case 'd':
				num = va_arg(list,int);
				char arr[10];
				itoa(num,arr,10);
				fputs(arr,stdout);
				str += 2;
				break;
			case 'c':
				ch = va_arg(list,char);
				putchar(ch);
				str += 2;
				break;
			case 'f':
				f = va_arg(list,double);
				num = f;
				itoa(num,arr,10);
				fputs(arr,stdout);
				putchar('.');
				num = (f - num)*1000000;
				itoa(num,arr,10);
				fputs(arr,stdout);
				str += 2;
				break;
			case 's':
				pstr = va_arg(list,char *);
				fputs(pstr,stdout);
				str += 2;
				break;
			default:
				putchar(*str);
				break;
			}
		}
		else
		{
			putchar(*str);
			str++;
		}	
	}
	va_end(list);
}

int main()
{
	MyPrint("%d,%c,%f,%s\n",10,'x',3.141592,"hello");
	return 0;
}
