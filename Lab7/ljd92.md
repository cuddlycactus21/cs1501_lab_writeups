# Lab 7: Flood Fill (LC #733)
Luke Douglas
## Code Solution
```java
class Solution {
    public int[][] floodFill(int[][] image, int sr, int sc, int color) {
        if (image[sr][sc] != color)
            floodFill_rec(image, sr, sc, color, image[sr][sc]);
        return image;
    }

    private void floodFill_rec(int[][] image, int r, int c, int color, int originalColor) {
        image[r][c] = color;
        if (r - 1 >= 0 && image[r - 1][c] == originalColor) {
            floodFill_rec(image, r - 1, c, color, originalColor);
        }
        if (r + 1 < image.length && image[r + 1][c] == originalColor) {
            floodFill_rec(image, r + 1, c, color, originalColor);
        }
        if (c - 1 >= 0 && image[r][c - 1] == originalColor) {
            floodFill_rec(image, r, c - 1, color, originalColor);
        }
        if (c + 1 < image[0].length && image[r][c + 1] == originalColor) {
            floodFill_rec(image, r, c + 1, color, originalColor);
        }
    }
}
```
## Code Explanation
First, we check that our starting cell is not already the given color (if it is, there is no work to be done).
Then, we recursively check all the cells in the same color region as our initial cell. The original color is stored in the `originalColor` variable, passed through all the function calls.
At each step in the recursive process, we set our cell's color and check if any of the four cells adjacent to ours (if they exist) are currently colored the original color. If they are, we call our function on them.
## Time and Space Analysis
In the worst case, all cells will be one color, and we will have to change each of them to a different color, so our runtime complexity is $O(n)$, where $n$ is the number of cells in `image`.
The main contributor to our space complexity is our recursion. We can never be more than $n$ calls deep, though, so our space complexity is also $O(n)$.