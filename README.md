# Check-Valid-Partition-For-The-Array
You are given a 0-indexed integer array nums. You have to partition the array into one or more contiguous subarrays.   

We call a partition of the array valid if each of the obtained subarrays satisfies one of the following conditions:   

The subarray consists of exactly 2 equal elements. For example, the subarray [2,2] is good.   
The subarray consists of exactly 3 equal elements. For example, the subarray [4,4,4] is good.   
The subarray consists of exactly 3 consecutive increasing elements, that is, the difference between adjacent elements is 1. For example, the subarray [3,4,5] is good, but the subarray [1,3,5] is not.    
Return true if the array has at least one valid partition. Otherwise, return false.    

Example 1:   
Input: nums = [4,4,4,5,6]    
Output: true    
Explanation: The array can be partitioned into the subarrays [4,4] and [4,5,6].   
This partition is valid, so we return true.    

Example 2:    
Input: nums = [1,1,1,2]    
Output: false    
Explanation: There is no valid partition for this array.   

Constraints:    
2 <= nums.length <= 105    
1 <= nums[i] <= 106    


=> Solution:

Approach : Top-Down Dynamic Programming

The recursive dynamic programming approach can be used to solve this problem. Here, the idea is to create a recursive function prefixIsValid(i) which checks whether a valid partition exists for the prefix subarray nums[0 ~ i]. Therefore, for nums of length n, prefixIsValid(n - 1) represents whether there is a valid partition for the whole array.
To determine prefixIsValid(i) at every index i, we have three possibilities plus one base case to check:

=> base case: If i < 0, then prefixIsValid(i) is true, since it denotes an empty subarray that always has a valid partition.

The last two elements nums[i] and nums[i - 1] form a subarray of two equal elements. In this case, if prefixIsValid(i - 2) is true, it indicates that prefixIsValid(i) is also true. Since the valid partition for nums[0 ~ i - 2] can be appended by the subarray [nums[i - 1], nums[i]] to form a valid partition for nums[0 ~ i].

The last three elements nums[i], nums[i - 1], and nums[i - 2] form a subarray of three equal elements. In this case, if prefixIsValid(i - 3) is true, it indicates that prefixIsValid(i) is also true. Since the valid partition for nums[0 ~ i - 3] can be appended by the subarray [nums[i - 2], nums[i - 1], nums[i]] to form a valid partition for nums[0 ~ i].

The last three elements nums[i], nums[i - 1], and nums[i - 2] form a subarray of three consecutive increasing elements. In this case, if prefixIsValid(i - 3) is true, it indicates that prefixIsValid(i) is also true. Since the valid partition for nums[0 ~ i - 3] can be appended by the subarray [nums[i - 2], nums[i - 1], nums[i]] to form a valid partition for nums[0 ~ i].

In summary, if any of the following conditions is true, we can conclude that prefixIsValid(i) is true:

To optimize the time complexity, we can make use of memoization (caching previously calculated results) to avoid recomputing the same values multiple times. For instance, if we already know that a valid partition exists starting from the index i, we can save it in a hash map memo as memo[i] = true, therefore, we don't need to check it again the next time we encounter the same index.

Algorithm
Initialize a hash map memo, and set memo[-1] = true since an empty array always has a valid partition.    
Define a function prefixIsValid(i) as whether the prefix subarray nums[0 ~ i] has a valid partition.   

If i is stored in memo, return memo[i].   
Otherwise, set ans = false.   
If i > 0 and nums[i] = nums[i - 1], we update ans as ans |= prefixIsValid(i - 2).     
If i > 1 and nums[i] = nums[i - 1] = nums[i - 2], update ans |= prefixIsValid(i - 3).     
If i > 1 and nums[i] = nums[i - 1] + 1 = nums[i - 2] + 2, update ans |= prefixIsValid(i - 3).    
Set memo[i] = ans and return ans.     
Return prefixIsValid(n - 1).    

    class Solution {
    Map<Integer, Boolean> memo = new HashMap<>();

    // Determine if the prefix array nums[0 ~ i] has a valid partition
    boolean prefixIsValid(int[] nums, int i) {
        if (memo.containsKey(i)) {
            return memo.get(i);
        }
        boolean ans = false;

        // Check 3 possibilities
        if (i > 0 && nums[i] == nums[i - 1]) {
            ans |= prefixIsValid(nums, i - 2);
        }
        if (i > 1 && nums[i] == nums[i - 1] && nums[i - 1] == nums[i - 2]) {
            ans |= prefixIsValid(nums, i - 3);
        }
        if (i > 1 && nums[i] == nums[i - 1] + 1 && nums[i - 1] == nums[i - 2] + 1) {
            ans |= prefixIsValid(nums, i - 3);
        }
        memo.put(i, ans);
        return ans;
    }

    public boolean validPartition(int[] nums) {
        int n = nums.length;
        memo.put(-1, true);

        return prefixIsValid(nums, n - 1);
    }
    }
