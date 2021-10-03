## Delete Sublist to Make Sum Divisible By K.MD
https://binarysearch.com/problems/Delete-Sublist-to-Make-Sum-Divisible-By-K

You are given a list of positive integers nums and a positive integer k. Return the length of the shortest sublist (can be empty sublist ) you can delete such that the resulting list's sum is divisible by k. You cannot delete the entire list. If it's not possible, return -1.

Constraints

1 ≤ n ≤ 100,000 where n is the length of nums
Example 1
Input
nums = [1, 8, 6, 4, 5]
k = 7
Output
2
Explanation
We can remove the sublist [6, 4] to get [1, 8, 5] which sums to 14 and is divisible by 7.

### Notes
If A%k == B%k then (A - B)%k == 0.
The problem can be converted to find the shortest sublist nums[i, j] that totalSum%k == sum[i, j]%k, so that (totalSum - sum[i, j])%k == 0.
It's about sublist sum, so can use presum. sum[i, j] = presum[j + 1] - presum[i]. presum[i + 1] = presum[i] + nums[i].
So totalSum%k == (presum[j+1]-presum[i])%k ==> (presum[j+1] - totalsum)%k == presum[i]%k. The problem is converted to find the i, j that (presum[j+1] - totalsum)%k == presum[i]%k.
Since need to find the shortest sublist, can use a hashmap to store the presum%k and its corresponding index, and always update the map with the latest index if the presums are the same.
When iterating the nums, look for a key from the map that equals to the (presum[j + 1] - totalSum)%k. Once it's found, get the min between result and j - i + 1 (the length of the nums[i, j]).
In java, the mod result of a negative integer by a positive integer is negative, i.e. -1%4 == -1. To make it positive, have to use Math.floorMod(-1, 4).
Solution
```

class Solution {
    public int solve(int[] nums, int k) {
        int result = nums.length;
        Map<Integer, Integer> map = new HashMap<>();

        int sum = 0;
        for(int n: nums){
            sum += n;
        }

        // sum[i, j] = presum[j + 1] - presum[i]
        // presum[i + 1] = presum[i] + nums[i]
        // presum[0] == 0, presum[1] = presum[0] + nums[0]
        int presum = 0;
        int mod = Math.floorMod(presum, k);
        map.put(mod, 0);

        for(int i = 0; i < nums.length; i++){
            presum += nums[i];
            mod = Math.floorMod(presum, k);
            map.put(mod, i + 1); // update the map always with the latest index

            int target = Math.floorMod(presum - sum, k);
            if(map.containsKey(target)){
                result = Math.min(result, i - map.get(target) + 1); // compare result and j - i + 1
            }
        } 

        return result == nums.length? -1 : result;
    }
}
```
### Complexity
- Time: O(n) to scan the nums (n is the length of nums).
- Space: O(min(n, k)) the key of the hash map is presum%k, and the value of the hash map will be always updated with the latest index. So the size of the map is min(n, k).


### Algorithm
Let sum by the sum of the array. If sum % p = x, then the problem is the find the shortest sub-array whose sum is represented by subsum, such that subsum % p = x.
We iterate over the array and calculate the prefix sum modulo of each value.
To avoid overflow, we use the following equation: If A%p = x, then (A+B)%p = (x+B)%p.
We use a map to keep the prefix sum modulo and the corresponding index.
The applicable sub-array A[i-j] has the following property:
sum(A[i-j]) = preSum[j] - preSum[i-1]
(preSum[j] - preSum[i-1]) % p = sum % p => (preSum[j] - preSum[i-1] - sum % p) % p = 0 => (preSum[j] - sum % p) % p = preSum[i-1] % p
So we simply need to find the key equal to (preSum[j] - sum % p) in the map.

### Complexity
Time Complexity: O(N). We iterate the array twice.
Space Complexity: O(min(N, p)) which is the size of the map.

Code
Language: Java
```
public int minSubarray(int[] nums, int p) {
    int target = 0;
    for (int num : nums) {
        // Use modulo to sum to avoid overflow
        target = (target + num) % p;
    }

    Map<Integer, Integer> map = new HashMap<>();
    int prefixSumMod = 0;
    int result = nums.length;

    map.put(0, -1);
    for (int i = 0; i < nums.length; i++) {
        // Use modulo to sum to avoid overflow
        prefixSumMod = (prefixSumMod + nums[i]) % p;
        map.put(prefixSumMod, i);

        int targetKey = Math.floorMod(prefixSumMod - target, p);
        if (map.containsKey(targetKey)) {
            result = Math.min(result, i - map.get(targetKey));
        }
    }
    return result == nums.length ? -1 : result;
}
```
