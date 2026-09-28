# Lab 3: Remove Sub-Folders from the Filesystem (LC #1233)
Luke Douglas
## Code Solution
```java
class Solution {
    public List<String> removeSubfolders(String[] folder) {
        Arrays.sort(folder);
        List<String> output = new ArrayList<>();
        String lastAdded = "0"; // definitely not a prefix
        for (int i = 0; i < folder.length; i++) {
            if (!folder[i].startsWith(lastAdded)) {
                output.add(folder[i]);
                lastAdded = folder[i] + "/"; // without this, /a/b is a prefix of /a/bx
            }
        }
        return output;
    }
}
```
## Code Explanation
First, I use `Arrays.sort()` to alphabetize `folder`, our input list of (sub)folders. Next, I initialize two variables: `output`, which we will add our subset of surviving folders to, and `lastAdded`, which we will use to keep track of the last folder we added to `output`. Because folder names only contain lowercase English letters, I initialized `lastAdded` to `"0"` to ensure that it would not be a prefix of any of the folder names.

Next, we begin looping over `folder`. The key idea that makes this algorithm work is the fact that since we sorted `folder`, any subfolders of a given folder will appear directly after it. Because of this, once we add a valid, non-removed folder to `output`, we only need to check if subsequent folders are subfolders of that valid folder, until the next valid folder is added.

To check if a folder is a subfolder of `lastAdded`, we use the `.startsWith()` method. Also, importantly, whenever we update the value of `lastAdded`, we append a `"/"` to its end to prevent errors where something like `"/a/bd"` is mistakenly marked as a subfolder of `"/a/b"`.
## Time and Space Analysis
Our input is an array of strings, each of which can have a variable length. I will assume that each of the strings are roughly the same length (the LeetCode problem specifies an upper bound on each folder's length, so this is not an outlandish assumption). The initial sorting of `folder` takes $O(n\log n)$ time. The rest of the algorithm, which is a one-time traversal of the `folder` list, is only $O(n)$ because each `.startsWith()` comparison is $O(1)$ since our strings' lengths are bounded above by a constant. Therefore, our runtime complexity is $O(n\log n)$.

According to Google, when `Arrays.sort()` is used to sort an array of objects, TimSort is used, which has $O(n)$ space complexity in the worst case. The only other part of this algorithm that takes up a sufficient amount of memory is `output`. In the worst case, none of our folders will be subfolders of each other, and they will all be put into `output`, making its individual memory complexity also $O(n)$. Thus, our algorithm's total space complexity is $O(n)+O(n)=O(n)$.