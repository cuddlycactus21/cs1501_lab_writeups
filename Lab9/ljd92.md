# Lab 9: Min Cost Climbing Stairs (LC #746)
Luke Douglas
## Code Solution
```java
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        for (int i = cost.length - 3; i >= 0; i--) {
            cost[i] += Math.min(cost[i + 1], cost[i + 2]);
        }
        return Math.min(cost[0], cost[1]);
    }
}
```
## Code Explanation
We calculate the minimum costs to get to the top starting from each step in reverse, starting at the top and working our way down. The cost to get to the top from a given step is just going to be the cost to leave that step plus the minimum of the cost to get to the top from the next step and the cost to get to the top from the step after that. This is what the inside of my for loop is doing. We're allowed to start from either the first or second steps, so at the end, I return the minimum of those two options. (Note: I initialize `i` as `cost.length - 3` instead of `cost.length - 1` because the costs from the top two steps are not going to change.)
## Complexity Analysis.
My time complexity is $O(n)$ because this is a simple for loop over `cost`. My space complexity is $O(1)$ because we don't create any new variables other than `i`.