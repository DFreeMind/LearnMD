# 617.Merge Two Binary Trees（E）

Given two binary trees and imagine that when you put one of them to cover the other, some nodes of the two trees are overlapped while the others are not.

You need to merge them into a new binary tree. The merge rule is that if two nodes overlap, then sum node values up as the new value of the merged node. Otherwise, the NOT null node will be used as the node of new tree.

**Example 1:**

```
Input: 
	Tree 1                     Tree 2                  
          1                         2                             
         / \                       / \                            
        3   2                     1   3                        
       /                           \   \                      
      5                             4   7                  
Output: 
Merged tree:
	     3
	    / \
	   4   5
	  / \   \ 
	 5   4   7
```

**Note:** The merging process must start from the root nodes of both trees.

## Approach #1 Using Recursion [Accepted]

We can traverse both the given trees in a preorder fashion. At every step, we check if the current node exists(isn't null) for both the trees. If so, we add the values in the current nodes of both the trees and update the value in the current node of the first tree to reflect this sum obtained. At every step, we also call the original function `mergeTrees()` with the left children and then with the right children of the current nodes of the two trees. If at any step, one of these children happens to be null, we return the child of the other tree(representing the corresponding child subtree) to be added as a child subtree to the calling parent node in the first tree. At the end, the first tree will represent the required resultant merged binary tree.

The following animation illustrates the process.

https://leetcode.com/articles/merge-two-binary-trees/

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if (t1 == null)
            return t2;
        if (t2 == null)
            return t1;
        t1.val += t2.val;
        t1.left = mergeTrees(t1.left, t2.left);
        t1.right = mergeTrees(t1.right, t2.right);
        return t1;
    }
}
```

**Complexity Analysis**

- Time complexity : $O(m)$. A total of m nodes need to be traversed. Here, $m$ represents the minimum number of nodes from the two given trees.
- Space complexity : O(m). The depth of the recursion tree can go upto $m$ in the case of a skewed tree. In average case, depth will be $O(logm)$.

## Approach #2 Iterative Method [Accepted]

**Algorithm**

In the current approach, we again traverse the two trees, but this time we make use of a stack*s**t**a**c**k* to do so instead of making use of recursion. Each entry in the $stack$ strores data in the form $[node_{tree1}, node_{tree2}]$. Here, $node_{tree1}$ and $node_{tree2}$ are the nodes of the first tree and the second tree respectively.

We start off by pushing the root nodes of both the trees onto the stack*s**t**a**c**k*. Then, at every step, we remove a node pair from the top of the stack. For every node pair removed, we add the values corresponding to the two nodes and update the value of the corresponding node in the first tree. Then, if the left child of the first tree exists, we push the left child(pair) of both the trees onto the stack. If the left child of the first tree doesn't exist, we append the left child(subtree) of the second tree to the current node of the first tree. We do the same for the right child pair as well.

If, at any step, both the current nodes are null, we continue with popping the next nodes from the $stack$.

The following animation depicts the process.

https://leetcode.com/articles/merge-two-binary-trees/

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if (t1 == null)
            return t2;
        Stack < TreeNode[] > stack = new Stack < > ();
        stack.push(new TreeNode[] {t1, t2});
        while (!stack.isEmpty()) {
            TreeNode[] t = stack.pop();
            if (t[0] == null || t[1] == null) {
                continue;
            }
            t[0].val += t[1].val;
            if (t[0].left == null) {
                t[0].left = t[1].left;
            } else {
                stack.push(new TreeNode[] {t[0].left, t[1].left});
            }
            if (t[0].right == null) {
                t[0].right = t[1].right;
            } else {
                stack.push(new TreeNode[] {t[0].right, t[1].right});
            }
        }
        return t1;
    }
}

```

**Complexity Analysis**

- Time complexity : O(n). We traverse over a total of $n$ nodes. Here, $n​$ refers to the smaller of the number of nodes in the two trees.
- Space complexity : $O(n)$. The depth of stack can grow upto $n$ in case of a skewed tree.

Analysis written by: [@vinod23](https://leetcode.com/vinod23)

# 637.Average of Levels in Binary Tree （E）

Given a non-empty binary tree, return the average value of the nodes on each level in the form of an array.

**Example 1:**

```
Input:
    3
   / \
  9  20
    /  \
   15   7
Output: [3, 14.5, 11]
Explanation:
The average value of nodes on level 0 is 3,  on level 1 is 14.5, and on level 2 is 11. Hence return [3, 14.5, 11].
```

**Note:**

1. The range of node's value is in the range of 32-bit signed integer.

## Approach #1 Using Depth First Search [Accepted]

**Algorithm**

One of the methods to solve the given problem is to make use of Depth First Search. In DFS, we try to exhaust each branch of the given tree during the tree traversal before moving onto the next branch.

To make use of DFS to solve the given problem, we make use of two lists $count$ and $res$. Here, $count[i]$ refers to the total number of nodes found at the $i^{th}$ level(counting from root at level 0) till now, and $res[i]$ refers to the sum of the nodes at the $i^{th}$ level encountered till now during the Depth First Search.

We make use of a function `average(t, i, res, count)`, which is used to fill the res*r**e**s* and $count$ array if we start the DFS from the node t*t* at the $i^{th}​$ level in the given tree. We start by making the function call `average(root, 0, res, count)`. After this, we do the following at every step:

1. Add the value of the current node to the $res$(or $sum$) at the index corresponding to the current level. Also, increment the $count$ at the index corresponding to the current level.
2. Call the same function, `average`, with the left and the right child of the current node. Also, update the current level used in making the function call.
3. Repeat the above steps till all the nodes in the given tree have been considered once.
4. Populate the averages in the resultant array to be returned.

The following animation illustrates the process.

![image](https://ws2.sinaimg.cn/large/69d4185bly1g08b8f7tz5j20py0e1q4q.jpg)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public List < Double > averageOfLevels(TreeNode root) {
        List < Integer > count = new ArrayList < > ();
        List < Double > res = new ArrayList < > ();
        average(root, 0, res, count);
        for (int i = 0; i < res.size(); i++)
            res.set(i, res.get(i) / count.get(i));
        return res;
    }
    public void average(TreeNode t, int i, List < Double > sum, List < Integer > count) {
        if (t == null)
            return;
        if (i < sum.size()) {
            sum.set(i, sum.get(i) + t.val);
            count.set(i, count.get(i) + 1);
        } else {
            sum.add(1.0 * t.val);
            count.add(1);
        }
        average(t.left, i + 1, sum, count);
        average(t.right, i + 1, sum, count);
    }
}
```

**Complexity Analysis**

- Time complexity : $O(n)$. The whole tree is traversed once only. Here, n*n* refers to the total number of nodes in the given binary tree.
- Space complexity : $O(h)$. $res$ and $count$ array of size $h$ are used. Here, $h$ refers to the height(maximum number of levels) of the given binary tree. Further, the depth of the recursive tree could go upto $h$ only.



## Approach #2 Breadth First Search [Accepted]

**Algorithm**

Another method to solve the given problem is to make use of a Breadth First Search. In BFS, we start by pushing the root node into a $queue$. Then, we remove an element(node) from the front of the $queue$. For every node removed from the $queue$, we add all its children to the back of the same $queue$. We keep on continuing this process till the $queue$ becomes empty. In this way, we can traverse the given tree on a level-by-level basis.

But, in the current implementation, we need to do a slight modification, since we need to separate the nodes on one level from that of the other.

The steps to be performed are listed below:

1. Put the root node into the $queue​$.
2. Initialize sum*s**u**m* and $count$ as 0 and $temp$ as an empty queue.
3. Pop a node from the front of the $queue$. Add this node's value to the $sum$ corresponding to the current level. Also, update the $count$ corresponding to the current level.
4. Put the children nodes of the node last popped into the a $temp$ queue(instead of $queue$).
5. Continue steps 3 and 4 till $queue$ becomes empty. (An empty queue*q**u**e**u**e* indicates that one level of the tree has been considered).
6. Reinitialize $queue$ with its value as $temp$.
7. Populate the $res$ array with the average corresponding to the current level.
8. Repeat steps 2 to 7 till the $queue$ and $temp$ become empty.

At the end, $res$ is the required result.

The following animation illustrates the process. https://leetcode.com/articles/average-of-levels/

```java

/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public List < Double > averageOfLevels(TreeNode root) {
        List < Double > res = new ArrayList < > ();
        Queue < TreeNode > queue = new LinkedList < > ();
        queue.add(root);
        while (!queue.isEmpty()) {
            long sum = 0, count = 0;
            Queue < TreeNode > temp = new LinkedList < > ();
            while (!queue.isEmpty()) {
                TreeNode n = queue.remove();
                sum += n.val;
                count++;
                if (n.left != null)
                    temp.add(n.left);
                if (n.right != null)
                    temp.add(n.right);
            }
            queue = temp;
            res.add(sum * 1.0 / count);
        }
        return res;
    }
}
```

**Complexity Analysis**

- Time complexity : $O(n)$. The whole tree is traversed atmost once. Here, $n​$ refers to the number of nodes in the given binary tree.
- Space complexity : $O(m)$. The size of $queue$ or $temp$ can grow upto atmost the maximum number of nodes at any level in the given binary tree. Here, $m$ refers to the maximum mumber of nodes at any level in the input tree.

Analysis written by: [@vinod23](https://leetcode.com/vinod23)

# 662.Maximum Width of Binary Tree(M)

Given a binary tree, write a function to get the maximum width of the given tree. The width of a tree is the maximum width among all levels. The binary tree has the same structure as a **full binary tree**, but some nodes are null.

The width of one level is defined as the length between the end-nodes (the leftmost and right most non-null nodes in the level, where the `null` nodes between the end-nodes are also counted into the length calculation.

**Example 1:**

```
Input: 

           1
         /   \
        3     2
       / \     \  
      5   3     9 

Output: 4
Explanation: The maximum width existing in the third level with the length 4 (5,3,null,9).
```

**Example 2:**

```
Input: 

          1
         /  
        3    
       / \       
      5   3     

Output: 2
Explanation: The maximum width existing in the third level with the length 2 (5,3).
```

**Example 3:**

```
Input: 

          1
         / \
        3   2 
       /        
      5      

Output: 2
Explanation: The maximum width existing in the second level with the length 2 (3,2).
```

**Example 4:**

```
Input: 

          1
         / \
        3   2
       /     \  
      5       9 
     /         \
    6           7
Output: 8
Explanation:The maximum width existing in the fourth level with the length 8 (6,null,null,null,null,null,null,7).
```

**Note:** Answer will in the range of 32-bit signed integer.

**Explanation**

As we need to reach every node in the given tree, we will have to traverse the tree, either with a depth-first search, or with a breadth-first search.

The main idea in this question is to give each node a `position` value. If we go down the left neighbor, then `position -> position * 2`; and if we go down the right neighbor, then `position -> position * 2 + 1`. This makes it so that when we look at the position values `L` and `R` of two nodes with the same depth, the width will be `R - L + 1`.

## Approach #1: Breadth-First Search [Accepted]

**Intuition and Algorithm**

Traverse each node in breadth-first order, keeping track of that node's position. For each depth, the first node reached is the left-most, while the last node reached is the right-most.

```python
def widthOfBinaryTree(self, root):
    queue = [(root, 0, 0)]
    cur_depth = left = ans = 0
    for node, depth, pos in queue:
        if node:
            queue.append((node.left, depth+1, pos*2))
            queue.append((node.right, depth+1, pos*2 + 1))
            if cur_depth != depth:
                cur_depth = depth
                left = pos
            ans = max(pos - left + 1, ans)

    return ans
```

**Complexity Analysis**

- Time Complexity: $O(N)$ where $N$ is the number of nodes in the input tree. We traverse every node.
- Space Complexity: $O(N)$, the size of our `queue`.



## Approach #2: Depth-First Search [Accepted]

**Intuition and Algorithm**

Traverse each node in depth-first order, keeping track of that node's position. For each depth, the position of the first node reached of that depth will be kept in `left[depth]`.

Then, for each node, a candidate width is `pos - left[depth] + 1`. We take the maximum of the candidate answers.

```python
class Solution(object):
    def widthOfBinaryTree(self, root):
        self.ans = 0
        left = {}
        def dfs(node, depth = 0, pos = 0):
            if node:
                left.setdefault(depth, pos)
                self.ans = max(self.ans, pos - left[depth] + 1)
                dfs(node.left, depth + 1, pos * 2)
                dfs(node.right, depth + 1, pos * 2 + 1)

        dfs(root)
        return self.ans
```

**Complexity Analysis**

- Time Complexity: $O(N)$ where $N$ is the number of nodes in the input tree. We traverse every node.
- Space Complexity: $O(N)$, the size of the implicit call stack in our DFS.

Analysis written by: [@awice](https://leetcode.com/awice).

# 226.Invert Binary Tree（E）

Invert a binary tree.

**Example:**

Input:

```
     4
   /   \
  2     7
 / \   / \
1   3 6   9
```

Output:

```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```

**Trivia:**
This problem was inspired by [this original tweet](https://twitter.com/mxcl/status/608682016205344768) by [Max Howell](https://twitter.com/mxcl):

> Google: 90% of our engineers use the software you wrote (Homebrew), but you can’t invert a binary tree on a whiteboard so f*** off.

## Approach #1 (Recursive) [Accepted]

This is a classic tree problem that is best-suited for a recursive approach.

**Algorithm**

The inverse of an empty tree is the empty tree. The inverse of a tree with root $r$, and subtrees rightright and leftleft, is a tree with root $r$, whose right subtree is the inverse of $left$, and whose left subtree is the inverse of $right$.

**Java**

```java
public TreeNode invertTree(TreeNode root) {
    if (root == null) {
        return null;
    }
    TreeNode right = invertTree(root.right);
    TreeNode left = invertTree(root.left);
    root.left = right;
    root.right = left;
    return root;
}
```

**Complexity Analysis**

Since each node in the tree is visited only once, the time complexity is $O(n)$, where $n​$ is the number of nodes in the tree. We cannot do better than that, since at the very least we have to visit each node to invert it.

Because of recursion, $O(h)$function calls will be placed on the stack in the worst case, where h*h* is the height of the tree. Because $h\in O(n)$, the space complexity is $O(n)$.

## Approach #2 (Iterative) [Accepted]

Alternatively, we can solve the problem iteratively, in a manner similar to breadth-first search.

**Algorithm**

The idea is that we need to swap the left and right child of all nodes in the tree. So we create a queue to store nodes whose left and right child have not been swapped yet. Initially, only the root is in the queue. As long as the queue is not empty, remove the next node from the queue, swap its children, and add the children to the queue. Null nodes are not added to the queue. Eventually, the queue will be empty and all the children swapped, and we return the original root.

**Java**

```java
public TreeNode invertTree(TreeNode root) {
    if (root == null) return null;
    Queue<TreeNode> queue = new LinkedList<TreeNode>();
    queue.add(root);
    while (!queue.isEmpty()) {
        TreeNode current = queue.poll();
        TreeNode temp = current.left;
        current.left = current.right;
        current.right = temp;
        if (current.left != null) queue.add(current.left);
        if (current.right != null) queue.add(current.right);
    }
    return root;
}
```

**Complexity Analysis**

Since each node in the tree is visited / added to the queue only once, the time complexity is O(n), where n*n*is the number of nodes in the tree.

Space complexity is O(n), since in the worst case, the queue will contain all nodes in one level of the binary tree. For a full binary tree, the leaf level has $\lceil \frac{n}{2}\rceil=O(n)$ leaves.

Analysis written by: @noran

# 965.Univalued Binary Tree

A binary tree is *univalued* if every node in the tree has the same value.

Return `true` if and only if the given tree is univalued.

**Example 1:**

```
Input: [1,1,1,1,1,null,1]
Output: true
```

**Example 2:**

```
Input: [2,2,2,5,2]
Output: false
```

**Note:**

1. The number of nodes in the given tree will be in the range `[1, 100]`.
2. Each node's value will be an integer in the range `[0, 99]`.

## Approach 1: Depth-First Search

**Intuition and Algorithm**

Let's output all the values of the array. After, we can check that they are all equal.

To output all the values of the array, we perform a depth-first search.

```python
class Solution(object):
    def isUnivalTree(self, root):
        vals = []

        def dfs(node):
            if node:
                vals.append(node.val)
                dfs(node.left)
                dfs(node.right)

        dfs(root)
        return len(set(vals)) == 1
```

**Complexity Analysis**

- Time Complexity: $O(N)$, where N*N* is the number of nodes in the given tree.
- Space Complexity: $O(N)$. 

## Approach 2: Recursion

**Intuition and Algorithm**

A tree is univalued if both its children are univalued, plus the root node has the same value as the child nodes.

We can write our function recursively. `left_correct` will represent that the left child is correct: ie., that it is univalued, and the root value is equal to the left child's value. `right_correct` will represent the same thing for the right child. We need both of these properties to be true.

```python
class Solution(object):
    def isUnivalTree(self, root):
        left_correct = (not root.left or root.val == root.left.val
                and self.isUnivalTree(root.left))
        right_correct = (not root.right or root.val == root.right.val
                and self.isUnivalTree(root.right))
        return left_correct and right_correct
```

**Complexity Analysis**

- Time Complexity: $O(N)$, where $N$ is the number of nodes in the given tree.
- Space Complexity: $O(H)$, where $H$ is the height of the given tree. 

Analysis written by: [@awice](https://leetcode.com/awice).

# 101.Symmetric Tree(E)

Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

For example, this binary tree `[1,2,2,3,4,4,3]` is symmetric:

```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```

But the following `[1,2,2,null,3,null,3]` is not:

```
    1
   / \
  2   2
   \   \
   3    3
```

**Note:**
Bonus points if you could solve it both recursively and iteratively.

## Approach 1: Recursive

A tree is symmetric if the left subtree is a mirror reflection of the right subtree.

![Push an element in stack](https://leetcode.com/media/original_images/101_Symmetric.png)

Therefore, the question is: when are two trees a mirror reflection of each other?

Two trees are a mirror reflection of each other if:

1. Their two roots have the same value.
2. The right subtree of each tree is a mirror reflection of the left subtree of the other tree.

![Push an element in stack](https://leetcode.com/media/original_images/101_Symmetric_Mirror.png)

This is like a person looking at a mirror. The reflection in the mirror has the same head, but the reflection's right arm corresponds to the actual person's left arm, and vice versa.

The explanation above translates naturally to a recursive function as follows.

java version:

```java
public boolean isSymmetric(TreeNode root) {
    return isMirror(root, root);
}

public boolean isMirror(TreeNode t1, TreeNode t2) {
    if (t1 == null && t2 == null) return true;
    if (t1 == null || t2 == null) return false;
    return (t1.val == t2.val)
        && isMirror(t1.right, t2.left)
        && isMirror(t1.left, t2.right);
}
```

python version:

```python
class Solution(object):
    def isSymmetric(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        return self.dfs(root, root)
    
    def dfs(self, root1, root2):
        if not root1 and not root2: return True
        if not root1 or not root2: return False
        
        return root1.val == root2.val and \
                self.dfs(root1.left, root2.right) and \
                self.dfs(root1.right, root2.left)
```

**Complexity Analysis**

- Time complexity : $O(n)$. Because we traverse the entire input tree once, the total run time is $O(n)$, where n is the total number of nodes in the tree.
- Space complexity : The number of recursive calls is bound by the height of the tree. In the worst case, the tree is linear and the height is in $O(n)$. Therefore, space complexity due to recursive calls on the stack is $O(n)$ in the worst case. 



## Approach 2: Iterative

Instead of recursion, we can also use iteration with the aid of a queue. Each two consecutive nodes in the queue should be equal, and their subtrees a mirror of each other. Initially, the queue contains `root` and `root`. Then the algorithm works similarly to BFS, with some key differences. Each time, two nodes are extracted and their values compared. Then, the right and left children of the two nodes are inserted in the queue in opposite order. The algorithm is done when either the queue is empty, or we detect that the tree is not symmetric (i.e. we pull out two consecutive nodes from the queue that are unequal).

java version:

```java
public boolean isSymmetric(TreeNode root) {
    Queue<TreeNode> q = new LinkedList<>();
    q.add(root);
    q.add(root);
    while (!q.isEmpty()) {
        TreeNode t1 = q.poll();
        TreeNode t2 = q.poll();
        if (t1 == null && t2 == null) continue;
        if (t1 == null || t2 == null) return false;
        if (t1.val != t2.val) return false;
        q.add(t1.left);
        q.add(t2.right);
        q.add(t1.right);
        q.add(t2.left);
    }
    return true;
}
```

python version:

```python
class Solution(object):
    def isSymmetric(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        if not root: return True
        stack = [root, root]
        while stack:
            n1 = stack.pop()
            n2 = stack.pop()
            
            if n1 is None and n2 is None: continue
            if not n1 or not n2: return False
            if n1.val != n2.val: return False
            stack.append(n1.left)
            stack.append(n2.right)
            stack.append(n1.right)
            stack.append(n2.left)
        return True
```

**Complexity Analysis**

- Time complexity : $O(n)$. Because we traverse the entire input tree once, the total run time is $O(n)$, where $n$ is the total number of nodes in the tree.
- Space complexity : There is additional space required for the search queue. In the worst case, we have to insert $O(n)$ nodes in the queue. Therefore, space complexity is $O(n)$.

