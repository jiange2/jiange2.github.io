---
layout:     post
title:      "3. 统计数字(lintcode)"
author:     "jiange"
tag:        "lintcode"
previewImg: "http://t1.aixinxi.net/o_1c4c67jtf1ommpdcg6k83cdda.png-w.jpg"
---

----------
题目：

计算数字k在0到n中的出现的次数，k可能是0~9的一个值

样例：

例如n=12，k=1，在 [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]，我们发现1出现了5次 (1, 10, 11, 12)

思路：

&nbsp;&nbsp;&nbsp;&nbsp;对于数字n，k的出现次数 = 当n的个(十百千万...)位为2其他位数的全排列数量的总和.例如数字为n=302,k=2那么 在n中k出现的次数 = 2\*\*全排列数量 + \*2\*全排列数量 + *\*2全排列数量,

&nbsp;&nbsp;&nbsp;&nbsp;对于n中个十百千万...任一位数 = k时的全排列数量数量可以分开两部分统计，高位-1 和 高位保持不变两种情况统计。高位-1则当前位必然可以选为 k 且 低位可以0-9任选，所以:

&nbsp;&nbsp;&nbsp;&nbsp;高位-1时当前位选k的全排列数量=(高位-1 + 1)*10^低位的数量。

&nbsp;&nbsp;&nbsp;&nbsp;对于n=33022,k=2，当前位为百时  高位-1时当前位选k的全排列数量 = (33 - 1 + 1) * 10 ^ 2。(高位可选范围0-32，低位可选范围0-99)

对于高位保持不变部分的统计分三种情况:

&nbsp;&nbsp;&nbsp;&nbsp;当前位 > k :

&nbsp;&nbsp;&nbsp;&nbsp;低位可以0-9任选，所以数量 = 10^低位数量

&nbsp;&nbsp;&nbsp;&nbsp;例如对于33322百位的统计,低位可选范围0-99,保持高位不变的数量 = 10^2

&nbsp;&nbsp;&nbsp;&nbsp;当前位 = k :

&nbsp;&nbsp;&nbsp;&nbsp;低位可以0-原数据最大值任选，所以数量 = 低位原数据 + 1

&nbsp;&nbsp;&nbsp;&nbsp;例如对于33322百位的统计,低位可选范围0-22,保持高位不变的数量 = 22 + 1

&nbsp;&nbsp;&nbsp;&nbsp;当前位 < k :

&nbsp;&nbsp;&nbsp;&nbsp;当前为高位保持不变时不能选k，所以数量 = 0

代码:

    public int digitCounts(int k, int n) {
		//特殊情况
    	if(k == 0 && n == 0){
    		return 1;
    	}
    	
    	int count = 0;
		//获取的数字是和n一样长的1000....的数字，方便做除数获得当前位的数字，以及统计10^低位长度
		//假如n=33022,那么zeros = 10000
    	int zeros = getZeros(n);
		//获取头位数 33022/10000 = 3
    	int quotient = n / zeros;
    
		//最大位的统计,当k=0不用统计头位数
		//对于33022，选头位为0时，03022 = 3022,0消失
    	if(k != 0){	
			//当头位数 > k
    		if(quotient > k){
    			count += zeros;
			//当头位数 == k
    		}else if(quotient == k){
    			count += n%zeros + 1;
    		}
    	}
    	//开始统计下一位数
    	zeros /= 10;
    
		//循环统计除头位数以外的位数
    	while (zeros > 0){
			//获取高位数
    		quotient = n / zeros;
			//高位-1时当前位选k的全排列数量=(高位-1 + 1)*10^低位的数量
    		count += quotient/10 * zeros;
			//高位保持不变的统计
	    	if(quotient % 10 > k){
	    		count += zeros;
	    	}else if(quotient % 10 == k){
	    		count += n % zeros + 1;
	    	}
    		zeros /= 10;
    	}
    
    	return count;
    }
    
    private int getZeros(int n){
    	int r = 1;
    	while (n >= 10){
    		r *= 10;
    		n /= 10;
    	}
    	return r;
    }


代码优化:

上面的代码是从头位统计到尾位.我们可以从头位统计到尾位统计到头位可以去掉getZeros的操作

	public int digitCounts(int k, int n) {
        if (k == 0 && n == 0) {
            return 1;
        }

        int count = 0,zeros = 1,quotient,t = n / 10;

        while (zeros <= t) {
            quotient = n / zeros;
            count += quotient / 10 * zeros;
            if (quotient % 10 > k) {
                count += zeros;
            } else if (quotient % 10 == k) {
                count += n % zeros + 1;
            }
            zeros *= 10;
            //System.out.println(count);
        }

        quotient = n / zeros;
        if (k != 0) {
            if (quotient > k) {
                count += zeros;
            } else if (quotient == k) {
                count += n % zeros + 1;
            }
        }

        //System.out.println(count);

        return count;
    }







