 
&ensp;&ensp;&ensp;&ensp;程序一般都需要检查**每个**函数调用是否返回错误。个别情况下，当函数返回错误时，我们想做的事并非简单的终止程序的运行，还可能需要检查系统调用是否被中断。
&ensp;&ensp;&ensp;&ensp;但当程序出错时终止程序运行是普遍情况，我们可以通过定义**包裹函数**来缩短程序。包裹函数的作用就是，完成实际的函数调用，并检查返回值，并在发生错误时终止进程。约定包裹函数名为实际函数名首字母大写形式。

``` c++
int
Socket(int family,int type,int protocol)
{
	int n;
	if ( (n=socket(family,type,protocol))<0)
		err_sys("socket error");
	return(n);
}
```

***FIXME：***
&ensp;&ensp;&ensp;&ensp;线程函数的包裹函数

###Unix errno值

&ensp;&ensp;&ensp;&ensp;只要一个unix函数中有错误发生,全局变量errno就被设置为一个指明该错误类型的正值,函数本身则通常返回-1。err_sys查看errno的值并输出相应的出错信息。
