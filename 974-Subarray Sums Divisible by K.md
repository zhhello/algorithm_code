# 974. 和可被 K 整除的子数组

给定一个整数数组 A，返回其中元素之和可被 K 整除的（连续、非空）子数组的数目。

```
输入：A = [4,5,0,-2,-3,1], K = 5
输出：7
解释：
有 7 个子数组满足其元素之和可被 K = 5 整除：
[4, 5, 0, -2, -3, 1], [5], [5, 0], [5, 0, -2, -3], [0], [0, -2, -3], [-2, -3]

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/subarray-sums-divisible-by-k
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

## 解法1
耗时不通过
```
class Solution {
public:
    int subarraysDivByK(vector<int>& A, int K) {
        if(0 == K || A.size() == 0)
            return 0;
        
        int res = 0;
        int sec_sum = 0;
        for(int i = 0; i < A.size(); i++) {
            sec_sum = A[i];
            if(sec_sum % K == 0)
                res++;
            for(int j = i + 1; j < A.size(); j++) {
                sec_sum += A[j];
                if(sec_sum % K == 0)
                    res++;
            }
        }

        return res;
    }
};
```

## 解法2
利用哈希数组，参考560 题进行优化，仍然超过耗时要求
```
class Solution {
public:
    int subarraysDivByK(vector<int>& A, int K) {
        if(0 == K || A.size() == 0)
            return 0;

        int pre_sum = 0;
        int res = 0;
        unordered_map<int, int> sum_cnt;
        for(int i = 0; i < A.size(); i++) {
            pre_sum += A[i];
            if(pre_sum % K == 0)
                res++;
            
            for(unordered_map<int,int>::iterator it = sum_cnt.begin(); it != sum_cnt.end(); it++) {
                if((pre_sum - it->first) % K == 0)
                    res += it->second;
            }

            sum_cnt[pre_sum]++;
        }

        return res;
    }
};
```

## 方法3
根据数学公式进行优化

分析：
我们令 P[i] = A[0] + A[1] + ... + A[i]P[i]=A[0]+A[1]+...+A[i]。

那么每个连续子数组的和 sum(i,j) 就可以写成 P[j] - P[i-1]（其中 0 < i < j0<i<j）的形式。

此时，判断子数组的和能否被 K 整除就等价于判断 (P[j] - P[i-1]) mod K == 0，根据 同余定理，只要 P[j] mod K == P[i-1] mod K，就可以保证上面的等式成立


时间复杂度O(N),
空间复杂度O(N)
```
class Solution {
public:
    int subarraysDivByK(vector<int>& A, int K) {
        if(0 == K || A.size() == 0)
            return 0;

        int pre_sum = 0;
        int res = 0;
        int key = 0;
        unordered_map<int, int> sum_cnt;
        for(int i = 0; i < A.size(); i++) {
            pre_sum += A[i];
            // 考虑到数组中存在负数的可能
            // 方法1
            // key = (pre_sum % K + K) % K;

            // 方法2
            key = pre_sum % K;
            if(key < 0)
                key += K;

            if(key == 0)
                res++;
            
            if(sum_cnt.count(key))
                res += sum_cnt[key];

            sum_cnt[key]++;
        }

        return res;
    }
};
```