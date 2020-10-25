# 560-Subarray Sum Equals K
给定一个整数数组和一个整数 k，你需要找到该数组中和为 k 的连续的子数组的个数。

```
输入:nums = [1,1,1], k = 2
输出: 2 , [1,1] 与 [1,1] 为两种不同的情况。
```

## 解法1
两层for循环遍历的思路，优化了数组区间求和的思路

O(N^2)时间复杂度

```
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        int N = nums.size();
        int res = 0;
        if(N <= 0)
            return res;
        
        // 如果定义dp[N][N],代表区间【i,j】之间连续子数组之和，导致stack overflow, 因为开辟的数组空间大了，需要优化栈使用
        //优化为dp[N],代表每次i,j区间的和
        // 继续优化，因为dp[i]取决于dp[i-1]，所以其实不需要dp[N]
        //int dp[N] = {0};
        int sum = 0;
        for(int i = 0; i < N; i++){
            sum = nums[i];
            if(sum == k)
                res++;
            
            for(int j = i+1; j < N; j++){
                sum = sum + nums[j];
                if(sum == k)
                    res++;
            }
        }
        
        return res;
    }
};
```

## 解法2
未经优化的前缀和，时间复杂度仍然为O(N^2)

```
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        // 按照前缀和思路解决
        if(nums.size() == 0)
            return 0;

        vector<int> pre_sum; // 前缀和
        int res = 0;
        for(int val : nums) {
            res += val;
            pre_sum.push_back(res);
        }

        res = 0;
        for(int i = 0; i < nums.size(); i++) {
            if(pre_sum[i] == k)
                res++;
                
            for(int j = i + 1; j < nums.size(); j++) {
                if(pre_sum[j] - pre_sum[i] == k)
                    res++;
            }
        }

        return res;
    }
};
```

## 解法3
经过优化的前缀和算法，
利用前缀和思路， 且采用哈希列表优化了时间耗时


时间复杂度：O(N)

空间复杂度：O(N)

```
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        // 按照前缀和思路解决
        if(nums.size() == 0)
            return 0;

        unordered_map<int, int> sum_cnt;
        int cur_sum = 0;
        int res = 0;
        for(int i = 0; i < nums.size(); i++) {
            cur_sum += nums[i]; // 前缀和
            if(cur_sum == k)
                res++;
            
            // 在更新sum_cnt之前进行统计，可以保证只统计当前位置之前的子数组
            if(sum_cnt.count(cur_sum -k)) 
                res += sum_cnt[cur_sum-k];
            
            sum_cnt[cur_sum]++;
        }

        return res;
    }
};
```
