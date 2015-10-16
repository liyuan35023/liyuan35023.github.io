---
layout: post
title: Unix Network Programming(1)---基本概念说法
date: 2015-10-16
categories: blog
tags: [Unix]
description: Keep learning~
---

#一个简单的时间获取函数

    #include 'unp'   //包含自己编写的头文件，该头文件包含大多数网络程序都需要的许多系统头文件，
                       并定义了所用到的各种常值
    int main(int argc, char **argv)    //main函数形参是命令行参数
    {
        int sockfd,n;
        char recvline[MAXLINE+1];
        struct sockaddr_in servaddr;

        if (argc!=2)
            err_quit("usage:a.out <IPaddress>");

        /* socket函数创建一个网际(AF_INET)字节流(SOCK_STREAM)套接字。该函数返回一个小整数描述符，
           以后所有函数调用(如随后read和connect)都用该描述符描述该套接字。*/
        if ( (sockfd=socket(AF_INET,SOCK_STREAM,0) )<0)    
            err_sys("socket error");
        
        /*指定服务器的IP地址和端口*/
        bzero(&servaddr, sizeof(servaddr));
        servaddr.sin_family=AF_INET;
        servaddr.sin_port   =htons(13);    //daytime server
        if (inet_pton(AF_INET, argv[1], &servaddr.sin_addr)<=0)
            err_quit("inet_pton error for %s", argv[1]);

        /*建立与服务器的连接*/
        if (connect(sockfd, (SA *) &servaddr, sizeof(servaddr))<0)　　
            err_sys("connect error");       //SA在unp.h中是用#define定义为struct sockaddr

        /*读入并输出服务器的应答*/
        while ( (n = read(sockfd, recvline, MAXLINE)) > 0)
        {
	        recvline[n] = 0;	/* null terminate */
	        if (fputs(recvline, stdout) == EOF)
	                 err_sys("fputs error");
        }
        if (n < 0)
             err_sys("read error");
        exit(0);
    }

#Key Points

* bzero()函数将结构清零, 两个参数.memset()函数也可以实现同样功能,但它有三个参数.
* 网际套接字地址结构中的IP地址与端口号这两个成员必须是用特定的格式.

  库函数htons("主机到网络短整数")去转换二进制端口号.

  库函数inet_pton("呈现形式到数值")去把ASCII命令行参数转化位合适格式.
* SA代表struct sockaddr,即通用套接字地址结构.***每当一个套接字函数需要一个指向某个套接字地址结构的指针时,这个指针必须强制类型转化成一个指向通用套接字地址结构的指针.***
* read()函数读取服务器的应答,标准io函数fputs输出结果.如果数据很大,我们不能确保一次read调用就能返回服务器的整个应答,因此从TCP套接字读取数据时,我们总是需要把read编写在某个循环中,read返回0(表明对端关闭连接)或负值(表明发生错误)时终止循环.
