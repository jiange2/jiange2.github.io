---
layout:     post
title:      "4. 丑数(lintcode)"
author:     "jiange"
tag:        "lintcode"
previewImg: "https://raw.githubusercontent.com/jiange2/photo/master/lintcode/4.%E4%B8%91%E6%95%B0/title.PNG"
---

![](https://raw.githubusercontent.com/jiange2/photo/master/lintcode/4.%E4%B8%91%E6%95%B0/title.PNG)

因为丑数只含素因子2，3，5。所以 任一丑数 = 2或3或5 * 更小的丑数（因为丑数素因子只由2 3 5组成,所以分解出了一个2或3或5，剩下部分仍是丑数）

所以用2 3 5 * 其他丑数，可以求得所有丑数

加入an是丑数数列，那丑数的全集就是

2*a1 2*a2 ... 2*an
3*a1 3*a2 ... 2*an
5*a1 5*a2 ... 2*an

我们把1作为丑数的第一项a1

不难看出这是一个 从左到右从上到下递增 的二维数组。我们可以把每行的第一个 (2\*a1,3\*a1,5\*a1) 进行比较,然后选出其最小值(2\*a1),就是第二项。然后把刚刚选出最小值那行的下一个(2\*a2)以及刚刚没被选上的数(3\*a1,5\*a2)进行比较得出第三项.以此类推...

需要注意的是因为3项比较有可能出现值相等的情况(2\*a3(2\*3) 和 3\*a2(3\*2))，这个时候需要把都为最小值的行都推到下一个.(2\*a3 和 3\*a2 下一轮变成 2\*a4 和 3\*a3)。

代码:

	public int nthUglyNumber(int n) {
        int[] r = new int[n];
        r[0] = 1;
        //分别指向 2 3 5 乘到了第几项
        int t2 = 0,t3 = 0,t5 = 0;
        int i = 1;
        while (i < n){
            r[i] = Math.min(2*r[t2],Math.min(3*r[t3],5*r[t5]));
            // 2那行被选 t2移到下一项 
            if(2*r[t2] == r[i]) ++t2;
            if(3*r[t3] == r[i]) ++t3;
            if(5*r[t5] == r[i]) ++t5;
            ++i;
        }
        return r[n-1];
    }




