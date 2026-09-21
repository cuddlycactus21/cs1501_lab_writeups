# Lab 2: Merge Two Binary Trees (LC #617)
Luke Douglas

## Code Solution
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        if (root1 == null && root2 == null) return null;
        if (root1 == null) {
            return new TreeNode(root2.val, mergeTrees(null, root2.left), mergeTrees(null, root2.right));
        }
        else if (root2 == null) {
            return new TreeNode(root1.val, mergeTrees(root1.left, null), mergeTrees(root1.right, null));
        }
        else {
            return new TreeNode(root1.val + root2.val, mergeTrees(root1.left, root2.left), mergeTrees(root1.right, root2.right));
        }
    }
}
```
## Code Explanation
On a very basic level, my code traverses through both trees in parallel, handling merging at the current node before recursively merging the left and right subtrees of that node.
However, the two trees are not guaranteed to have the same shape; there may be places where one or both of the (sub)trees passed into the function are null.
My code checks for these special cases first.
If the roots in both trees are null, then there is no merging to be done and we simply return null (this is the recursive base case).
If one of the roots is null and the other contains a value, we return a new node with that value. To generate the left and right subtrees of this new node, we act as if the null node had null left and right children and continue recursively.
If we have escaped all of these special cases, then we know that the corresponding nodes in both trees are not null. We return a new node with the sum of the two nodes' values and use recursion to generate its left and right children.
## Time and Space Analysis
Assuming that the merging of two nodes takes a constant amount of time (returning null, grabbing one of the nodes' values, or summing the two values), my code has a runtime complexity of $O(n)$, where $n$ is the sum of the numbers of nodes in the two input trees. This is because my function will have to run once for each node in the merged tree. Since there is no guarantee that the two input trees will be balanced, in the worst case they will only overlap at the root and will be completely divergent afterward. Therefore, in this case, we will have to perform one operation at the root, one at each non-root node in the first tree, and one at each non-root node in the second tree: $1 + (n_1-1) + (n_2-1)=n-1$ operations, which simplifies to $O(n)$.

My function is recursive, so it needs to store a constant amount of data in memory for each level of recursion it will need to do. In other words, it needs space proportional to the maximum of the heights of the two input trees. However, since our trees are not balanced, their height is at most $n$.
My function also needs to generate up to $n$ nodes as its output. Therefore, our space complexity is $O(n)+O(n)=O(n)$.