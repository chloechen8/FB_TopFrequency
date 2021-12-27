# 117. Populating Next Right Pointers in Each Node II

Created Time: September 11, 2021 5:11 PM
Difficulty: Medium
Last edited time: December 27, 2021 5:09 PM
No: 117

**Follow-up:**

- You may only use constant extra space.
- The recursive approach is fine. You may assume implicit stack space does not count as extra space for this problem.

和112的区别是这个不是perfect binary tree, 有可能没有left child, 有right child.

思路：

![0d2702843a5d35b6286dd8dd579a7f9](C:\Users\Xin\AppData\Local\Temp\WeChat Files\0d2702843a5d35b6286dd8dd579a7f9.jpg)

# Solutoin1 Stack

Time & Space: O(n)

```java
if(i < size - 1)
head.next = queue.peek();
```

```java
class Solution {
    public Node connect(Node root) {
        if(root == null) return null;
        Queue<Node> queue = new LinkedList<>();
        queue.offer(root);
        
        while(!queue.isEmpty()){
            int size = queue.size();
            for(int i = 0; i < size; i++){
                Node head = queue.poll();
                
                if(i < size - 1)
                    head.next = queue.peek();
                
                if(head.left != null){
                    queue.offer(head.left);
                }
                if(head.right != null){
                    queue.offer(head.right);
                }
            }
        }
        return root;
    }
}
```

# Solution2 Contant space O(1)

Time: O(n), Space: O(1)

```java
class Solution {
    public Node connect(Node root) {
        Node cur = root;
        Node pre = null;
        Node head = null;
        if(root == null) return root;
        
        while(cur != null){
            while(cur != null){
                if(cur.left != null){
                    if(head == null){
                        head = cur.left;
                        pre = cur.left;
                    }
                    else{
                        pre.next = cur.left;
                        pre = pre.next;
                    }
                }
                
                if(cur.right != null){
                    if(head == null){
                        head = cur.right;
                        pre = cur.right;
                    }
                    else{
                        pre.next = cur.right;
                        pre = pre.next;
                    }
                }
                cur = cur.next;
            }
            
            //cur == null
            cur = head;
            head = null;
            pre = null;
        }
        
        return root;
    }
}
```

## Solution3 Recursion

```java
class Solution {
    public Node connect(Node root) {
        if(root == null) return null;
        
        if(root.left != null){
            if(root.right != null) root.left.next = root.right;
            else root.left.next = findNext(root);
        }
        
        if(root.right != null){
            root.right.next = findNext(root);
        }
        
			 //这里的顺序需要注意
        connect(root.right);
        connect(root.left);
        
        return root;
    }
    
    public Node findNext(Node root){
        //下一层第一个不为null的节点
		    //这里需要注意，需要先确保root.next的值有效
        while(root.next != null){
            root = root.next;
            if(root.left != null) return root.left;
            if(root.right != null) return root.right;
            
        }
        return null;
    }
}
```