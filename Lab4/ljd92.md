# Lab 4: Maximum Product of Two Elements in an Array (LC #1464)
Luke Douglas
## Code Solution
```java
class Solution {
    public int maxProduct(int[] nums) {
        int maxVal;
        int secVal;
        if (nums[0] >= nums[1]) {
            maxVal = nums[0];
            secVal = nums[1];
        } else {
            maxVal = nums[1];
            secVal = nums[0];
        }
        for (int i = 2; i < nums.length; i++) {
            if (nums[i] > maxVal) {
                secVal = maxVal;
                maxVal = nums[i];
            } else if (nums[i] > secVal) {
                secVal = nums[i];
            }
        }
        return (maxVal - 1) * (secVal - 1);
    }
}
```
## Code Explanation
My code traverses through `nums` once, keeping track of the highest and second highest values as we go.
First, we look at the first two values in nums to initialize our `maxVal` and `secVal` (second-maximum value). If `nums[0]` is greater, it is the max value and `nums[1]` is the second-maximum value. Otherwise, the reverse is true.
As we traverse through the list in our for loop, if we come to a number that is greater than our current `maxVal`, then we shift `maxVal`'s value down to `secVal` and replace `maxVal` with our new maximum value. If we come to a value that is greater than `secVal`, but not greater than `maxVal`, then we set `secVal` to that value.
After we finish going through the loop, we return the maximized value of `(maxVal - 1) * (secVal - 1)`.
## Time and Space Analysis
Because my algorithm does a simple linear traversal of the `nums` array, and the amount of work done on each iteration of the for loop is bounded above by a constant, this implementation has a time complexity of $O(n)$.
My algorithm only creates the variables `maxVal`, `secVal`, and `i`, so it has a space complexity of $O(1)$.