# Task02：动态规划（4天）
**基本思想**

动态规划算法与分治法类似，其基本思想也是将待求解问题分解成若干子问题，先求解子问题，然后从这些子问题的解得到原问题的解。与分治法不同的是，适合于用动态规划法的求解的问题，**经分解得到的子问题往往不是相互独立的**。若用分治法解这类问题，这分解得到的子问题太多，以至于最后解决原问题需要耗费指数时间。然而，不同子问题的数目常常只有多项式级。在**用分治法求解时，有些子问题被重复计算了许多次**。如果能够**保存已解决的子问题的答案，而在需要时再找出以求得的答案，就可以避免大量重复计算，从而得到多项式时间的算法**。为了达到这个目的，可以用一个表来记录所有已解决的子问题的答案。不管该子问题以后是否会被用到，只要它被计算过，就将其结果填入表中。这就是动态规划法的基本思想。

**基本步骤**

（1）找出最优解的性质，并刻画其结构特征。

（2）递归的定义最优值。

（3）以自底向上的方式计算出最优值。

（4）根据计算最优值时得到的信息，构造最优解。

**作业**

[Leetcode 5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)
```c++
class Solution {
public:
    string longestPalindrome(string s) {
        int len = s.length();
        vector< vector<int> > a(len, vector<int>(len));
        int start(0), l(1);
        for(int i = 0; i < len; i++)
        {
            a[i][i] = 1;
            if(i + 1 < len && s[i] == s[i + 1])
            {
                a[i][i + 1] = 1;
                start = i;
                l = 2;
            }
        }
        for(int r = 3; r <= len; r++)
        {
            for(int i = 0; i < len - r + 1; i++)
            {
                int j = i + r - 1;
                if(s[i] == s[j] && a[i + 1][j - 1] == 1)
                {
                    a[i][j] = 1;
                    start = i;
                    l = r;
                }
                
            }
        }
        return s.substr(start, l);
        
    }
};
```

[Leetcode 72. 编辑距离](https://leetcode-cn.com/problems/edit-distance/)
```c++
class Solution {
public:
    int minDistance(string word1, string word2) {
        int n = word1.length();
        int m = word2.length();

        // 有一个字符串为空串
        if (n * m == 0) return n + m;

        // DP 数组
        int D[n + 1][m + 1];

        // 边界状态初始化
        for (int i = 0; i < n + 1; i++) {
            D[i][0] = i;
        }
        for (int j = 0; j < m + 1; j++) {
            D[0][j] = j;
        }

        // 计算所有 DP 值
        for (int i = 1; i < n + 1; i++) {
            for (int j = 1; j < m + 1; j++) {
                int left = D[i - 1][j] + 1;
                int down = D[i][j - 1] + 1;
                int left_down = D[i - 1][j - 1];
                if (word1[i - 1] != word2[j - 1]) left_down += 1;
                D[i][j] = min(left, min(down, left_down));

            }
        }
        return D[n][m];
    }
};
```
[Leetcode 198. 打家劫舍](https://leetcode-cn.com/problems/house-robber/)
```c++
#define max(a, b) (a > b ? a : b)
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        if(n == 0) return 0;
        vector<int> a(n + 1), b(n + 1);
        a[0] = nums[0];
        b[0] = 0;
        for(int i = 1; i < n; i++) {
            a[i] = b[i - 1] + nums[i];
            b[i] = max(a[i - 1], b[i -1]);
        }
        return max(a[n - 1], b[n - 1]);
    }
};
```
[Leetcode 213. 打家劫舍 II](https://leetcode-cn.com/problems/house-robber-ii/)
```c++
#define max(a, b) (a > b ? a : b)
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        if(n == 1) return nums[0];
        if(n == 0) return 0;
        int a, b; // 偷（不偷）第i间房拿到的最高金额
        a = b = 0;
        int tmp;
        for(int i = 2; i < n - 1; i++){
            tmp = a;
            a = b + nums[i];
            b = max(tmp, b);
        }
        int c = max(a, b) + nums[0];
        a = b = 0;
        for(int i = 1; i < n; i++){
            tmp = a;
            a = b + nums[i];
            b = max(tmp, b);
        }
        int d = max(a, b);
        return max(c, d);
    }
};
```
[Leetcode 516. 最长回文子序列](https://leetcode-cn.com/problems/longest-palindromic-subsequence/)
```c++
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        if (s.empty()) 
        {
            return 0;
        }
        int len = s.size();
        vector<vector<int>> dp(len, vector<int>(len, 0));
        for (int i = 0; i < len; ++i)
        {
            dp[i][i] = 1;
        }
        
        for (int i = len - 1; i >= 0; --i)
        {
            for (int j = i + 1; j < len; ++j)
            {
                if (s[i] == s[j])
                {
                    dp[i][j] = 2 + dp[i + 1][j - 1];
                }
                else
                {
                    dp[i][j] = max(dp[i][j - 1], dp[i + 1][j]);
                }
            }
        }

        return dp[0][len - 1];
    }
};
```
[Leetcode 674. 最长连续递增序列](https://leetcode-cn.com/problems/longest-continuous-increasing-subsequence/)
```c++
class Solution {
public:
    int findLengthOfLCIS(vector<int>& nums) {
        if (nums.empty()) return 0;
        int ans = 1;
        int i = 0, j = 0;
        while (j < nums.size())
        {
            int tmp = 1;
            for (j = i + 1; j < nums.size() && nums[j] > nums[j - 1]; j++)
            {
                tmp++;
            }
            i = j;
            ans = max(ans, tmp);
        }
        return ans;
        
    }
};
```