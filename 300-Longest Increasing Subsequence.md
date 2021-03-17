# 300. 最长递增子序列
给你一个整数数组 nums ，找到其中最长严格递增子序列的长度。

子序列是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，[3,6,2,7] 是数组 [0,3,1,6,2,2,7] 的子序列。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/longest-increasing-subsequence
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解法
```
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        // dp[i] 是以i结尾 的 最长子序列的长度
        int N = nums.size();
        if(N <= 1)
            return N;
        
        vector<int> dp(N, 1);
        int res = 1;
        for(int i = 1; i < N; i++){
            for(int j = 0; j < i; j++){
                if(nums[i] > nums[j])
                    dp[i] = max(dp[i], dp[j]+1);
            }
            
            res = max(res, dp[i]);
        }
        
        return res;
    }
};

```