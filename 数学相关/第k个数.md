## 有些数的素因子只有 3，5，7，请设计一个算法找出第 k 个数。注意，不是必须有这些素因子，而是必须不包含其他的素因子。例如，前几个数按顺序应该是 1，3，5，7，9，15，21
```
class Solution {
    public int getKthMagicNumber(int k) {
        //定义3个指针，p3指向的数是有前一个数乘3得到的，p5,p7同理
        int p3=0,p5=0,p7=0;
        //magic number序列
        int[]dp=new int[k];
        dp[0]=1;
        //如果这个数是由前一个数*3得到的，那就移动p3获得下一个数
        for (int i = 1; i < k; i++) {
            dp[i]=Math.min(dp[p3]*3,Math.min(dp[p5]*5,dp[p7]*7));
            if(dp[i]==dp[p3]*3)p3++;
            if(dp[i]==dp[p5]*5)p5++;
            if(dp[i]==dp[p7]*7)p7++;
        }
        return dp[k-1];
    }
}
```
