# Lab 6: Find the Town Judge (LC #997)
Luke Douglas
## Code Solution
```java
class Solution {
    public int findJudge(int n, int[][] trust) {
        boolean[] trustsOthers = new boolean[n];
        int[] trustedByOthers = new int[n];
        for (int i = 0; i < trust.length; i++) {
            trustsOthers[trust[i][0] - 1] = true;
            trustedByOthers[trust[i][1] - 1]++;
        }

        for (int i = 0; i < n; i++) {
            if (!trustsOthers[i] && trustedByOthers[i] == n - 1)
                return i + 1;
        }
        return -1;
    }
}
```
## Code Explanation
First, we initialize two arrays to keep track of requirements (1) and (2) of being the town judge. `trustsOthers` keeps track of if each person trusts anyone else, and `trustedByOthers` keeps track of how many people trust each person.
Then, we iterate through `trust`, updating our two arrays as appropriate.
Finally, we iterate through our two arrays, looking for a person who trusts no one and is trusted by `n - 1` other people (that is, everyone else). If we find such a person, we return their number, and if we do not find anyone like this, we return -1.
## Time and Space Analysis
Because we do a for loop with `trust.length` iterations and then a for loop with `n` iterations, our runtime complexity is $O(n+t)$, where $t$ is `trust.length`.
Our space complexity is $O(n)$ because we create two arrays with exactly `n` length.