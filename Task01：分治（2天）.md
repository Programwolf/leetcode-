# Task01：分治（2天）
**分治法的基本思想**

分治法的基本思想是将一个规模为n的问题分解为k个规模较小的子问题，这些子问题相互独立且与原问题**相同**。递归的解这些子问题，然后将各子问题的解合并得到原问题的解。它的一般算法设计模式如下；

```c++
divide_and_conquer(P)
{
    if (|P| <= n0) adhoc(P);
    divide P into smaller subinstances P1, P2, ... ,Pk;
    for (i = 1, i <= k, i++)
        yi = divide_and_conquer(Pi);
    return merge(y1, ..., yk);
}
```
其中|P|为问题P的规模。n0为一阈值，表示当前问题P的规模不超过n0时，问题已容易解出，不必再继续分解。`adhoc(P)`时该分治法中的基本子算法，用于直接解小规模的问题P。当P的规模不超过n0时，直接用算法`adhoc(P)`求解。算法`merge(y1, y1, ..., yk)`是该分治法中的合并子算法，用于将P的子问题P1，P2，...，Pk的解y1，y2，...，yk合并为P的解。

>在用分治法设计算法时，最好使子问题的规模大致相同。这种做法是出自一种平衡子问题的思想，它几乎总是比子问题的不等的做法要好。

**作业**

[leetcode 50.Pow(x, n)](https://leetcode-cn.com/problems/powx-n/)
```c++
class Solution {
public:
    double myPow(double x, int n) {;
        long long N = n;
        if (N < 0)
        {
            return 1.0 / quickMul(x, -N);
        }
        else
        {
            return quickMul(x, N);
        }
    }

    double quickMul(double x, long long n)
    {
        //快速幂+迭代
        double x_contribute = x;
        double ans = 1.0;

        while (n > 0)
        {
            if ((n & 1) == 1)
            {
                ans *= x_contribute;
            }
            x_contribute *= x_contribute;
            n = (n >> 1);
        }

        return ans;

        //快速幂+递归
        // if (n == 0) 
        // {
        //     return 1.0;
        // }
        // double y = quickMul(x, n / 2);
        // if ((n & 1) == 0)
        // {
        //     return y * y;
        // }
        // else
        // {
        //     return y * y * x;
        // }
        
    }
};
```

[leetcode 53.最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)
```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int len = nums.size();
        int ans = nums[0];
        vector<int> dp(len);
        dp[0] = nums[0];
        for(int i = 1; i < len; i++) {
            if(dp[i - 1] > 0){
                dp[i] = dp[i - 1] + nums[i];
            }
            else {
                dp[i] = nums[i];
            }
            if(ans < dp[i]) ans = dp[i];
        }
        return ans;
    }
};
```
[leetcode 169.多数排序](https://leetcode-cn.com/problems/majority-element/)
```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int n = nums.size();
        map<int, int> hashmap;
        for (int i = 0;i < n; i++)
        {
            if (hashmap.find(nums[i]) == hashmap.end())
            {
                hashmap[nums[i]] = 1;
            }
            else
            {
                hashmap[nums[i]]++;
            }
            if (hashmap[nums[i]] > n / 2)
            {
                return nums[i];
            }
        }
        return 0;
    }
};
```