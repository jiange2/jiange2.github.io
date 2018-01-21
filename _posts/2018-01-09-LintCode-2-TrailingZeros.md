---
layout:     post
title:      "2. 尾部的零(lintcode)"
author:     "jiange"
tag:        "lintcode"
previewImg: "http://t1.aixinxi.net/o_1c4chd7um1nla1s2uoe11iv11ml6a.png-w.jpg"
---

![](http://t1.aixinxi.net/o_1c4chiu4jqe71hon18oq1ainsema.png-w.jpg)

1、蛮力法：

Ⅰ、算出n!

Ⅱ、不断除10除到尾位不是0为止

该方法简单直接暴力，但阶乘数字很大，int类型最大能接受 15!,long类型最大能接受20!,所以除非使用BigDecimal，否则这种算法能求的范围极度有限。如果使用BigDecimal算法的效率不高。

2、我的思路

分析此题可以发现，因为只能2*5 = 10，每一对2和5产生一个0，阶乘的每一个数的因式分解，每一个数中2和5成对的对数。因为阶乘中2的个数远多于5的个数，所以统计5的个数即可

	public long trailingZeros(long n) {
        long count = 0;
        while (n >= 5) {
            //n含有最大的k (5^k的k)
            int deg = (int) (Math.log(n) / Math.log(5));
            //5^k 当前最大乘到的5^k
            long l1 = (long) Math.pow(5, deg);
            //当前n包含了 l2 个 5^k
            int l2 = (int) (n/l1);
            //统计0-5^k 含 5 的个数 (25含2个5 125含3个5)
            //统计5^k中5,5^1,5^2,...,5^k的倍数个数的和
            //5^0 (5^k的次数) + 5^1 (5^k的次数)+ 5^2 (5^k的次数)+ ... + 5^(k-1)(5的次数)
            //举例: 对于0-125来说 125倍数的个数为1(5^0) (125) , 25倍数的个数为5(5^1) (25,50,75,100,125),
            //5倍数的个数为25 (5^2) (5,10,15,...,125)
            //这种统计方式让5^k被统计k次,所以结果就是 0-5^k含5的个数
            //所以这里是等比数列的和  1 - q^k  / 1 - q, q = 5
            long d1 = (long) ((l1 - 1) / 4.0);
            count +=  l2 * d1;
			//统计剩余部分的5的个数
            n -= l2 * l1;
        }

        return  count;
    }


3、编程之美解法

	int numOfZero(int n)  
	{  
	    int num = 0, i;  
	    for(i=5; i<=n; i*=5)  
	    {  
	        num += n/i;  
	    }  
	    return num;  
	}  

> 公式：Z = [N/5] +[N/52] +[N/53] + …（不用担心这会是一个无穷的运算，因为总存在一个K，使得5K > N，[N/5K]=0。）
公式中，[N/5]表示不大于N 的数中5 的倍数贡献一个5，[N/52]表示不大于N 的数中52的倍数再贡献一个5，……那偶啥也不说了

ps:因为他的算法没用什么pow，log之类的，效率是我算法的10倍左右，不过算法效率都是o(logn)