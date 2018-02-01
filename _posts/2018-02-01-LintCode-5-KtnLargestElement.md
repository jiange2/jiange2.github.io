---
layout:     post
title:      "5. 第k大元素(lintcode)"
author:     "jiange"
tag:        "lintcode"
previewImg: "https://raw.githubusercontent.com/jiange2/photo/master/lintcode/5.第k大元素/5.PNG"
---

题目:[http://www.lintcode.com/zh-cn/problem/kth-largest-element/](http://www.lintcode.com/zh-cn/problem/kth-largest-element/)

![](https://raw.githubusercontent.com/jiange2/photo/master/lintcode/5.第k大元素/5.PNG)

利用快排的思想。

根据基准值(一般为数组的第一个数)。进行左右划分,比基准小的放到基准值右边，反之放左边(因为是第k大，所以逆序，也可以是顺序，想顺序的话下面就反着看)。

左右划分后，有三种情况

1、基准值下标大于k，进行递归,在 0 - （基准值小标 - 1）范围找第k大元素

2、基准值下标小于k，进行递归,在（基准值小标 + 1）- n 范围找第(k - (基准值下标 + 1))大元素

3、基准值下标等于k，那么基准值就是k

代码:

	public int kthLargestElement(int k, int[] nums) {
        if(k<1 || k >nums.length){
            return -1;
        }
        return getKthLargestElementByQuickSort(nums.length-k+1, nums, 0, nums.length-1);
    }

    private int getKthLargestElementByQuickSort(int k, int[] nums, int start, int end) {
        int left = start, right = end;
        //基值
        int t = nums[left];
        while (left < right) {

            while (nums[right] > t && left < right)
                right--;

            if (left < right)
                nums[left++] = nums[right];

            while (nums[left] < t && left < right)
                left++;

            if (left < right)
                nums[right--] = nums[left];
        }
        nums[left] = t;
        //基值为当前区间数组的第pos大数
        int pos = left-start+1;
        if (pos == k) { //基值就是结果
            return nums[left];
        } else if (pos > k) { //因为比基值小的数的数量大于k，所以k在基值左边
            return getKthLargestElementByQuickSort(k, nums, start, left - 1);
        } else {    //反之，基值在右，且问题转变为求k-pos
            return getKthLargestElementByQuickSort(k-pos, nums, left + 1, end);
        }
    }	

效率分析:

算法的效率和快排的有点像，当基准值每次都在最中间,效率最好，为

n + n/2 + n/4 + ... + n/n = (1 + 1/2 + 1/4 + ... ) * n = (2 - (1/2)<sup>n</sup>) * < 2n; 

所以算法的最好效率是 o(2n) = o(n);

当数组每次都需要把右边的数放到左边(也就是每次基准值最小),且k = 1,算法的最坏效率

n + (n - 1) + (n - 2) + ... + 1 = (1 + n) * n / 2 = 1/2 * n^2 + 1/2 * n

所以算法最坏效率为o(n ^ 2)

