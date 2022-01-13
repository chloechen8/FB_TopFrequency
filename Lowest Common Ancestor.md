## 235. Lowest Common Ancestor of a Binary Search Tree
- 如果node值比p,q小，说明LCA在左边
- 如果node值比p,q大，说明LCA在右边
- 否则就是node是LCA
#### Solution1 DFS
Time: O(n), Space: O(n): using stack
```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root.val < p.val && root.val < q.val) 
            return lowestCommonAncestor(root.right, p, q);
        if(root.val > p.val && root.val > q.val)
            return lowestCommonAncestor(root.left, p, q);
        
        return root;
    }
}
```
#### Solution2 Iterative
Time: O(n), Space: O(1)
```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        TreeNode curNode = root;
        
        while(curNode != null){
            if(curNode.val < p.val && curNode.val < q.val){
                curNode = curNode.right;
            }
            else if(curNode.val > p.val && curNode.val > q.val){
                curNode = curNode.left;
            }
            else return curNode;
        }
        
        return null;
    }
}
```

