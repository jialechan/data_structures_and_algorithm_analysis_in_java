# 数据结构与算法分析知识整理

* [最大子序列和问题](https://github.com/jialechan/data_structures_and_algorithm_analysis_in_java#%E6%9C%80%E5%A4%A7%E5%AD%90%E5%BA%8F%E5%88%97%E5%92%8C%E9%97%AE%E9%A2%98)

## 最大子序列和问题
```txt
给定（可能有负数）整数序列A1, A2, A3..., An求这个序列中子序列和的最大值。   
（为方便起见，如果所有整数均为负数，则最大子序列和为0）。   
例如：输入整数序列： -2, 11, 8, -4, -1, 16, 5, 0，则输出答案为35，即从A2～A6。
```
### 算法一：穷举法
```java
public static int maxSub(int[] a) {
    int maxSum = 0;
    for(int i=0; i<a.length; i++) {        //i为子序列的左边界
        for(int j=i; j<a.length; j++) {    //j为子序列的右边界
            int thisSum = 0;
            for(int k=0; k<=j; k++)        //迭代子序列中的每一个元素，求和
                thisSum += a[k];
            if(thisSum > maxSum)
                maxSum = thisSum;
        }
    }
    return maxSum;
}
```
时间复杂度为O(N^3)
### 算法二：改进穷举法
```java
/** 
 * 对前面计算的结果加以利用 
 * 减去一层循环 
 */  
public static int maxSub(int a[]) {  
    int maxSum = 0;  
    for (int i = 0; i < a.length; i++) {  
        int sum = 0;  
        for (int j = i; j < a.length; j++) {  
            sum += a[j];  
            if (maxSum < sum)  
                maxSum = sum;  
        }  
    }  
    return maxSum;  
}  
```
时间复杂度为O(N^2)
### 算法三：分治
最大子序列的和只可能出现在3个地方：   
1. 出现在输入数据的左半部分   
2. 出现在输入数据的右半部分   
3. 跨越输入数据的中部而位于左右两个部分   
前两种情况可以递归求解，第三种情况的最大和可以通过求出前半部分（包含前半部分的最后一个元素）的最大和以及后半部分（包括后半部分的第一个元素）的最大和，再将二者相加得到。
```java
public int maxSub(int[] a,int left,int right) {  
    if(left==right)  
        if(a[left]>0)  
            return a[left];  
        else  
            return 0;  
    int center = (left+right)/2;//分解  
    int maxLeftSum= maxSub(a,left,center);//左边递归  
    int maxRightSum = maxSub(a,center+1,right);//右边递归  
    //处理中间包含center左右边界的最大和情况  
    int maxLeftBorderSum = 0,leftBoderSum = 0;  
    for(int i=center;i>=left;i--) {  
        leftBoderSum +=a[i];  
        if(maxLeftBorderSum<leftBoderSum)  
            maxLeftBorderSum = leftBoderSum;  
    }  
    int maxRighttBorderSum = 0,rightBoderSum = 0;  
    for(int i=center+1;i<=right;i++) {  
        rightBoderSum +=a[i];  
        if(maxRighttBorderSum<<span>rightBoderSum</span>)  
            maxRighttBorderSum = rightBoderSum;  
    }  
    //问题合并(治)  
    return max(maxLeftSum,maxLeftBorderSum+maxRighttBorderSum,maxRightSum);  
}  

//可变参数的使用，jdk1.5及以上  
public int max(int ...args) {  
    int max=args[0];  
    for(int i=1;i<args.length;i++)  
        if(max<args[i])  
            max= args[i];  
    return max;  
} 
```
时间复杂度为O(NlogN)
### 算法四：
```java
public int maxSub(int[] a) {  
    int sum = 0,maxSum =0;  
    for(int i=0;i<a.length;i++) {  
        sum +=a[i];  
        if(sum>maxSum)  
            maxSum =sum;  
        else if(sum<0)  
            sum = 0; //如果sum<0,就没有必要将前面的序列继续代入了，将sum=0  
    }  
    return 0;  
} 
```
时间复杂度为O(N)

