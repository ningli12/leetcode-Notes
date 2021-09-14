
## 230. Kth Smallest Element in a BST
 https://leetcode.com/problems/binary-tree-inorder-traversal/

Given the root of a binary search tree, and an integer k, return the kth (1-indexed) smallest element in the tree.

Example 1:
Input: root = [3,1,4,null,2], k = 1
Output: 1

Example 2:
Input: root = [5,3,6,2,4,null,null,1], k = 3
Output: 3

Constraints:
The number of nodes in the tree is n.
1 <= k <= n <= 104
0 <= Node.val <= 104

### 思路
binary tree - inorder travsal tree, get result that array sorted in the ascending order
answer is the k - 1th element of this array.

### 代码
Iterate
```
class Solution {
    public int kthSmallest(TreeNode root, int k) {
        if(root == null) return -1;
        
        //inorder tree traverse
        List<Integer> res = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();

        while(root != null || !stack.isEmpty() ){
            while(root != null){
                stack.add(root);
                root = root.left;
            }
            TreeNode cur = stack.pop();
            res.add(cur.val);
            root = cur.right;
        }
        
        return res.get(k - 1);
    }
}
```
优化
```
class Solution {
  public int kthSmallest(TreeNode root, int k) {
    LinkedList<TreeNode> stack = new LinkedList<TreeNode>();

    while (true) {
      while (root != null) {
        stack.add(root);
        root = root.left;
      }
      root = stack.removeLast();
      if (--k == 0) return root.val;
      root = root.right;
    }
  }
}
```
Recursive
```
class Solution {
    public int kthSmallest(TreeNode root, int k) {
        List<Integer> res = new ArrayList<>();
        helper(root, res);
        return res.get(k - 1);
    }
    private void helper(TreeNode root, List<Integer> res) {
        if(root != null) {
            if(root.left != null) helper( root.left, res);
            res.add(root.val);
            if(root.right != null) helper( root.right, res);
        }
    }
}
```
