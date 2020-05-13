## 1043. Partition Array for Maximum Sum
**Problem:**

*Given an integer array A, you partition the array into (contiguous) subarrays of length at most K.  
After partitioning, each subarray has their values changed to become the maximum value of that subarray.
Return the largest sum of the given array after partitioning.*

```diff
# Example:
# Input: A = [1,15,7,9,2,5,10], K = 3   Output: 84 
# Explanation: A becomes [15,15,15,9,10,10,10]
```

This problem can be solved by Dynamic Programming. List `dp[i]` record the maximum sum of the first i items in A.

For the upper example, `dp[0] = 1, dp[1] = 30, dp[2] = 45, ...` It is easy to get dp[0], since it obviously equals to A[0].

The key point here is that for every next A[i], we should consider it should be grouped with how many former items (from 1 to K). 
Let's say `j = 1, 2,..., K`, 
- when j = 1, it means that the former groups don't make a change, A[i] just be simply added to dp. `dp[i] = dp[i-1] + A[i]`. 
- when j = other, we need to find the maximum item in this j items and replace other j-1 items with it. `dp[i] = dp[i-j] + max(A[i-j+1:i+1])*j·`

In conclusion, this is the formula of dp[i]: `dp[i] = max(dp[i-j] + max(A[i-j+1:i+1])*j)`

In my solution, I firstly initiate the first K items in dp, they can be simply computed based on their previous items. So the following for loop
starts from K, it decreased the running time.

My solution:
```

class Solution:
    def maxSumAfterPartitioning(self, A: List[int], K: int) -> int:
        # Initiate the first K items in dp
        dp = [i * max(A[:i]) for i in range(1, K+1)]
        
        # Compute the following items
        for i in range(K, len(A)):
            # m is the maximum item among former K-1 items and A[i].
            m = 0
            # current_max is the maximum value in different js
            current_max = 0
            for j in range(1, K+1):
                m = max(m, A[i-j+1])
                current_max = max(current_max, dp[i-j] + m * j)
            dp.append(current_max)
                
        return dp[-1]
```
