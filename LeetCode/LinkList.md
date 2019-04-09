# 141.Linked List Cycle(E)

Given a linked list, determine if it has a cycle in it.

To represent a cycle in the given linked list, we use an integer `pos` which represents the position (0-indexed) in the linked list where tail connects to. If `pos` is `-1`, then there is no cycle in the linked list.

 

**Example 1:**

```
Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where tail connects to the second node.
```

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

**Example 2:**

```
Input: head = [1,2], pos = 0
Output: true
Explanation: There is a cycle in the linked list, where tail connects to the first node.
```

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png)

**Example 3:**

```
Input: head = [1], pos = -1
Output: false
Explanation: There is no cycle in the linked list.
```

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png)

 

**Follow up:**

Can you solve it using $O(1)$  (i.e. constant) memory?

This article is for beginners. It introduces the following ideas: Linked List, Hash Table and Two Pointers.

## Approach 1: Hash Table

**Intuition**

To detect if a list is cyclic, we can check whether a node had been visited before. A natural way is to use a hash table.

**Algorithm**

We go through each node one by one and record each node's reference (or memory address) in a hash table. If the current node is `null`, we have reached the end of the list and it must not be cyclic. If current node’s reference is in the hash table, then return true.

```java
public boolean hasCycle(ListNode head) {
    Set<ListNode> nodesSeen = new HashSet<>();
    while (head != null) {
        if (nodesSeen.contains(head)) {
            return true;
        } else {
            nodesSeen.add(head);
        }
        head = head.next;
    }
    return false;
}
```

**Complexity analysis**

- Time complexity : $O(n)$. We visit each of the n*n* elements in the list at most once. Adding a node to the hash table costs only $O(1)$ time.
- Space complexity: $O(n)$. The space depends on the number of elements added to the hash table, which contains at most n*n* elements. 

## Approach 2: Two Pointers

**Intuition**

Imagine two runners running on a track at different speed. What happens when the track is actually a circle?

**Algorithm**

The space complexity can be reduced to O(1)*O*(1) by considering two pointers at **different speed** - a slow pointer and a fast pointer. The slow pointer moves one step at a time while the fast pointer moves two steps at a time.

If there is no cycle in the list, the fast pointer will eventually reach the end and we can return false in this case.

Now consider a cyclic list and imagine the slow and fast pointers are two runners racing around a circle track. The fast runner will eventually meet the slow runner. Why? Consider this case (we name it case A) - The fast runner is just one step behind the slow runner. In the next iteration, they both increment one and two steps respectively and meet each other.

How about other cases? For example, we have not considered cases where the fast runner is two or three steps behind the slow runner yet. This is simple, because in the next or next's next iteration, this case will be reduced to case A mentioned above.

```java
public boolean hasCycle(ListNode head) {
    if (head == null || head.next == null) {
        return false;
    }
    ListNode slow = head;
    ListNode fast = head.next;
    while (slow != fast) {
        if (fast == null || fast.next == null) {
            return false;
        }
        slow = slow.next;
        fast = fast.next.next;
    }
    return true;
}
```

**Complexity analysis**

- Time complexity : $O(n)$. Let us denote $n$ as the total number of nodes in the linked list. To analyze its time complexity, we consider the following two cases separately.

  - **List has no cycle:**
    The fast pointer reaches the end first and the run time depends on the list's length, which is $O(n)$.
  - **List has a cycle:**
    We break down the movement of the slow pointer into two steps, the non-cyclic part and the cyclic part:
    1. The slow pointer takes "non-cyclic length" steps to enter the cycle. At this point, the fast pointer has already reached the cycle. $\text{Number of iterations} = \text{non-cyclic length} = N$ 
    2. Both pointers are now in the cycle. Consider two runners running in a cycle - the fast runner moves 2 steps while the slow runner moves 1 steps at a time. Since the speed difference is 1, it takes $\dfrac{\text{distance between the 2 runners}}{\text{difference of speed}}$ loops for the fast runner to catch up with the slow runner. As the distance is at most "$\text{cyclic length K}$" and the speed difference is 1, we conclude that 
       $\text{Number of iterations} = \text{almost} "\text{cyclic length K}"$.

  Therefore, the worst case time complexity is $O(N+K)$, which is $O(n)$.

- Space complexity : $O(1)$. We only use two nodes (slow and fast) so the space complexity is $O(1)$.

# 206. Reverse Linked List(E)

Reverse a singly linked list.

**Example:**

```
Input: 1->2->3->4->5->NULL
Output: 5->4->3->2->1->NULL
```

**Follow up:**

A linked list can be reversed either iteratively or recursively. Could you implement both?

## Approach #1 (Iterative) [Accepted]

Assume that we have linked list `1 → 2 → 3 → Ø`, we would like to change it to `Ø ← 1 ← 2 ← 3`.

While you are traversing the list, change the current node's next pointer to point to its previous element. Since a node does not have reference to its previous node, you must store its previous element beforehand. You also need another pointer to store the next node before changing the reference. Do not forget to return the new head reference at the end!

```java
public ListNode reverseList(ListNode head) {
    ListNode prev = null;
    ListNode curr = head;
    while (curr != null) {
        ListNode nextTemp = curr.next;
        curr.next = prev;
        prev = curr;
        curr = nextTemp;
    }
    return prev;
}
```

**Complexity analysis**

- Time complexity : $O(n)$. Assume that n*n* is the list's length, the time complexity is $O(n)$.
- Space complexity : $O(1)$.



## Approach #2 (Recursive) [Accepted]

The recursive version is slightly trickier and the key is to work backwards. Assume that the rest of the list had already been reversed, now how do I reverse the front part? Let's assume the list is: $n_1 → … → n_{k-1} → n_k → n_{k+1} → … → n_m → Ø$

Assume from node $n_{k+1}$ to $n_m$ had been reversed and you are at node $n_k$.

$n_1 → … → n_{k-1} →n_k → n_{k+1} ← … ← n_m$

We want $n_{k+1} $’s next node to point to nk.

So,

$n_k.next.next = n_k$;

Be very careful that $n_1$ 's next must point to Ø. If you forget about this, your linked list has a cycle in it. This bug could be caught if you test your code with a linked list of size 2.

```java
public ListNode reverseList(ListNode head) {
    if (head == null || head.next == null) return head;
    ListNode p = reverseList(head.next);
    head.next.next = head;
    head.next = null;
    return p;
}
```

**Complexity analysis**

- Time complexity : $O(n)$. Assume that n is the list's length, the time complexity is $O(n)$.
- Space complexity : $O(n)$. The extra space comes from implicit stack space due to recursion. The recursion could go up to n levels deep.

# 234. Palindrome Linked List-(E)

Given a singly linked list, determine if it is a palindrome.

**Example 1:**

```
Input: 1->2
Output: false
```

**Example 2:**

```
Input: 1->2->2->1
Output: true
```

**Follow up:**
Could you do it in O(n) time and O(1) space?

[Python easy to understand solution with comments (operate nodes directly)](https://leetcode.com/problems/palindrome-linked-list/discuss/64689/Python-easy-to-understand-solution-with-comments-(operate-nodes-directly).)

```python
def isPalindrome(self, head):
    fast = slow = head
    # find the mid node
    while fast and fast.next:
        fast = fast.next.next
        slow = slow.next
    # reverse the second half
    node = None
    while slow:
        nxt = slow.next
        slow.next = node
        node = slow
        slow = nxt
    # compare the first and second half nodes
    while node: # while node and head:
        if node.val != head.val:
            return False
        node = node.next
        head = head.next
    return True
```

# 811.Subdomain Visit Count(E)

A website domain like "discuss.leetcode.com" consists of various subdomains. At the top level, we have "com", at the next level, we have "leetcode.com", and at the lowest level, "discuss.leetcode.com". When we visit a domain like "discuss.leetcode.com", we will also visit the parent domains "leetcode.com" and "com" implicitly.

Now, call a "count-paired domain" to be a count (representing the number of visits this domain received), followed by a space, followed by the address. An example of a count-paired domain might be "9001 discuss.leetcode.com".

We are given a list `cpdomains` of count-paired domains. We would like a list of count-paired domains, (in the same format as the input, and in any order), that explicitly counts the number of visits to each subdomain.

```
Example 1:
Input: 
["9001 discuss.leetcode.com"]
Output: 
["9001 discuss.leetcode.com", "9001 leetcode.com", "9001 com"]
Explanation: 
We only have one website domain: "discuss.leetcode.com". As discussed above, the subdomain "leetcode.com" and "com" will also be visited. So they will all be visited 9001 times.
```

```
Example 2:
Input: 
["900 google.mail.com", "50 yahoo.com", "1 intel.mail.com", "5 wiki.org"]
Output: 
["901 mail.com","50 yahoo.com","900 google.mail.com","5 wiki.org","5 org","1 intel.mail.com","951 com"]
Explanation: 
We will visit "google.mail.com" 900 times, "yahoo.com" 50 times, "intel.mail.com" once and "wiki.org" 5 times. For the subdomains, we will visit "mail.com" 900 + 1 = 901 times, "com" 900 + 50 + 1 = 951 times, and "org" 5 times.
```

**Notes:**

- The length of `cpdomains` will not exceed `100`. 
- The length of each domain name will not exceed `100`.
- Each address will have either 1 or 2 "." characters.
- The input count in any count-paired domain will not exceed `10000`.
- The answer output can be returned in any order.

## 1: Hash Map [Accepted]

**Intuition and Algorithm**

The algorithm is straightforward: we just do what the problem statement tells us to do.

For an address like `a.b.c`, we will count `a.b.c`, `b.c`, and `c`. For an address like `x.y`, we will count `x.y` and `y`.

To count these strings, we will use a hash map. To split the strings into the required pieces, we will use library `split` functions.

```python
class Solution(object):
    def subdomainVisits(self, cpdomains):
        ans = collections.Counter()
        for domain in cpdomains:
            count, domain = domain.split()
            count = int(count)
            frags = domain.split('.')
            for i in xrange(len(frags)):
                ans[".".join(frags[i:])] += count

        return ["{} {}".format(ct, dom) for dom, ct in ans.items()]
```

**Complexity Analysis**

- Time Complexity: $O(N)$, where N*N* is the length of `cpdomains`, and assuming the length of `cpdomains[i]` is fixed.
- Space Complexity: $O(N)$, the space used in our count.

Analysis written by: [@awice](https://leetcode.com/awice).



#  237. Delete Node in a Linked List(E)

Write a function to delete a node (except the tail) in a singly linked list, given only access to that node.

Given linked list -- head = [4,5,1,9], which looks like following:

![img](https://wx4.sinaimg.cn/large/69d4185bly1fzt2pm61ihj20az01t745.jpg)

**Example 1:**

```
Input: head = [4,5,1,9], node = 5
Output: [4,1,9]
Explanation: You are given the second node with value 5, the linked list should become 4 -> 1 -> 9 after calling your function.
```

**Example 2:**

```
Input: head = [4,5,1,9], node = 1
Output: [4,5,9]
Explanation: You are given the third node with value 1, the linked list should become 4 -> 5 -> 9 after calling your function.
```

 

**Note:**

- The linked list will have at least two elements.
- All of the nodes' values will be unique.
- The given node will not be the tail and it will always be a valid node of the linked list.
- Do not return anything from your function.

## Approach: Swap with Next Node [Accepted]

The usual way of deleting a node `node` from a linked list is to modify the `next` pointer of the node *before*it, to point to the node *after* it.

![img](https://leetcode.com/media/original_images/237_LinkedList.png)

Since we do not have access to the node *before* the one we want to delete, we cannot modify the `next`pointer of that node in any way. Instead, we have to replace the value of the node we want to delete with the value in the node after it, and then delete the node after it.

![img](https://leetcode.com/media/original_images/237_LinkedList2.png)

![img](https://leetcode.com/media/original_images/237_LinkedList3.png)

![img](https://leetcode.com/media/original_images/237_LinkedList4.png)

Because we know that the node we want to delete is not the tail of the list, we can guarantee that this approach is possible.

**Java**

```
public void deleteNode(ListNode node) {
    node.val = node.next.val;
    node.next = node.next.next;
}
```

**Complexity Analysis**

Time and space complexity are both $O(1)​$.



## Approach 3: Optimize Approach 2 by Priority Queue

**Algorithm**

Almost the same as the one above but optimize the **comparison process** by **priority queue**. You can refer [here](https://en.wikipedia.org/wiki/Priority_queue) for more information about it.

```python
from Queue import PriorityQueue

class Solution(object):
    def mergeKLists(self, lists):
        """
        :type lists: List[ListNode]
        :rtype: ListNode
        """
        head = point = ListNode(0)
        q = PriorityQueue()
        for l in lists:
            if l:
                q.put((l.val, l))
        while not q.empty():
            val, node = q.get()
            point.next = ListNode(val)
            point = point.next
            node = node.next
            if node:
                q.put((node.val, node))
        return head.next
```

**Complexity Analysis**

- Time complexity : $O(N\log k)$ where $\text{k}$  is the number of linked lists.
  - The comparison cost will be reduced to $O(\log k)$ for every pop and insertion to priority queue. But finding the node with the smallest value just costs $O(1)$  time.
  - There are $N$ nodes in the final linked list.
- Space complexity :
  - $O(n)$ Creating a new linked list costs $O(n)$ space.
  - $O(k)$ The code above present applies in-place method which cost $O(1)$ space. And the priority queue (often implemented with heaps) costs $O(k)$ space (it's far less than $N$ in most situations). 

## Approach 4: Merge lists one by one

**Algorithm**

Convert merge $\text{k}$ lists problem to merge 2 lists ($\text{k-1}$) times. Here is the [merge 2 lists](https://leetcode.com/problems/merge-two-sorted-lists/description/) problem page.

**Complexity Analysis**

- Time complexity : $O(kN)$ where $\text{k}$ is the number of linked lists.
  - We can merge two sorted linked list in $O(n)$ time where n*n* is the total number of nodes in two lists.
  - Sum up the merge process and we can get: $O(\sum_{i=1}^{k-1} (i*(\frac{N}{k}) + \frac{N}{k})) = O(kN)$.
- Space complexity : $O(1)$
  - We can merge two sorted linked list in $O(1)$ space. 

## Approach 5: Merge with Divide And Conquer

**Intuition & Algorithm**

This approach walks alongside the one above but is improved a lot. We don't need to traverse most nodes many times repeatedly

- Pair up $\text{k}$ lists and merge each pair.
- After the first pairing, $\text{k}$ lists are merged into $k/2$ lists with average $2N/k$ length, then $k/4, k/8$ and so on.
- Repeat this procedure until we get the final sorted linked list.

Thus, we'll traverse almost $N$ nodes per pairing and merging, and repeat this procedure about $\log_{2}{k}$ times.

![img](https://wx3.sinaimg.cn/large/69d4185bly1fzwxq8mw7zj20hp0e0gml.jpg)

```python
class Solution(object):
    def mergeKLists(self, lists):
        """
        :type lists: List[ListNode]
        :rtype: ListNode
        """
        amount = len(lists)
        interval = 1
        while interval < amount:
            for i in range(0, amount - interval, interval * 2):
                lists[i] = self.merge2Lists(lists[i], lists[i + interval])
            interval *= 2
        return lists[0] if amount > 0 else lists

    def merge2Lists(self, l1, l2):
        head = point = ListNode(0)
        while l1 and l2:
            if l1.val <= l2.val:
                point.next = l1
                l1 = l1.next
            else:
                point.next = l2
                l2 = l1
                l1 = point.next.next
            point = point.next
        if not l1:
            point.next=l2
        else:
            point.next=l1
        return head.next
```

**Complexity Analysis**

- Time complexity : $O(N\log k)$ where $\text{k}$ is the number of linked lists.
  - We can merge two sorted linked list in $O(n)$ time where $n$ is the total number of nodes in two lists.
  - Sum up the merge process and we can get: $O\big(\sum_{i=1}^{log_{2}{k}}N \big)= O(N\log k)$
- Space complexity : $O(1)$
  - We can merge two sorted linked lists in $O(1)$ space.

# 961.N-Repeated Element in Size 2N Array(E)

In a array `A` of size `2N`, there are `N+1` unique elements, and exactly one of these elements is repeated N times.

Return the element repeated `N` times.

**Example 1:**

```
Input: [1,2,3,3]
Output: 3
```

**Example 2:**

```
Input: [2,1,2,5,3,2]
Output: 2
```

**Example 3:**

```
Input: [5,1,5,2,5,3,5,4]
Output: 5 
```

**Note:**

1. `4 <= A.length <= 10000`
2. `0 <= A[i] < 10000`
3. `A.length` is even

## 1: Count

**Intuition and Algorithm**

Let's count the number of elements. We can use a `HashMap` or an array - here, we use a `HashMap`.

After, the element with a count larger than 1 must be the answer.

```python
class Solution(object):
    def repeatedNTimes(self, A):
        count = collections.Counter(A)
        for k in count:
            if count[k] > 1:
                return k
```

**Complexity Analysis**

- Time Complexity: $O(N)$, where N*N* is the length of `A`.
- Space Complexity: $O(N)$. 

## 2: Compare

**Intuition and Algorithm**

If we ever find a repeated element, it must be the answer. Let's call this answer the *major element*.

Consider all subarrays of length 4. There must be a major element in at least one such subarray.

This is because either:

- There is a major element in a length 2 subarray, or;
- Every length 2 subarray has exactly 1 major element, which means that a length 4 subarray that begins at a major element will have 2 major elements.

Thus, we only have to compare elements with their neighbors that are distance 1, 2, or 3 away.

```python
class Solution(object):
    def repeatedNTimes(self, A):
        for k in xrange(1, 4):
            for i in xrange(len(A) - k):
                if A[i] == A[i+k]:
                    return A[i]
```

**Complexity Analysis**

- Time Complexity: $O(N)$, where N*N* is the length of `A`.
- Space Complexity: $O(1)​$. 

Analysis written by: [@awice](https://leetcode.com/awice).



# 2.Add Two Numbers(M)

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order** and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example:**

```
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```

## Approach 1: Elementary Math

**Intuition**

Keep track of the carry using a variable and simulate digits-by-digits sum starting from the head of list, which contains the least-significant digit.

![Illustration of Adding two numbers](https://leetcode.com/articles/Figures/2_add_two_numbers.svg)

*Figure 1. Visualization of the addition of two numbers: 342 + 465 = 807.Each node contains a single digit and the digits are stored in reverse order.*

**Algorithm**

Just like how you would sum two numbers on a piece of paper, we begin by summing the least-significant digits, which is the head of $l1$ and $l2$. Since each digit is in the range of $0 \ldots 9$, summing two digits may "overflow". For example $5 + 7 = 12$. In this case, we set the current digit to $2$ and bring over the $carry = 1$ to the next iteration. $carry$ must be either $0$ or $1$ because the largest possible sum of two digits (including the carry) is $9 + 9 + 1 = 19​$.

The pseudocode is as following:

- Initialize current node to dummy head of the returning list.
- Initialize carry to $0​$.
- Initialize $p$ and $q$ to head of $l1$ and $l2​$ respectively.
- Loop through lists l1 and l2 until you reach both ends.
  - Set x*x* to node p's value. If p has reached the end of l1, set to 0.
  - Set y*y* to node q's value. If q has reached the end of l2, set to 0.
  - Set $sum = x + y + carry​$.
  - Update $carry = sum / 10$.
  - Create a new node with the digit value of ($sum \bmod 10$)and set it to current node's next, then advance current node to next.
  - Advance both $p$ and $q$.
- Check if $carry = 1$, if so append a new node with digit 11 to the returning list.
- Return dummy head's next node.

Take extra caution of the following cases:

| Test case                 | Explanation                                                  |
| ------------------------- | ------------------------------------------------------------ |
| $l1=[0,1]  \\ l2=[0,1,2]$ | When one list is longer than the other.                      |
| $l1=[] \\ l2=[0,1]$       | When one list is null, which means an empty list.            |
| $l1=[9,9] \\ l2=[1]$      | The sum could have an extra carry of one at the end, which is easy to forget. |

```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    ListNode dummyHead = new ListNode(0);
    ListNode p = l1, q = l2, curr = dummyHead;
    int carry = 0;
    while (p != null || q != null) {
        int x = (p != null) ? p.val : 0;
        int y = (q != null) ? q.val : 0;
        int sum = carry + x + y;
        carry = sum / 10;
        curr.next = new ListNode(sum % 10);
        curr = curr.next;
        if (p != null) p = p.next;
        if (q != null) q = q.next;
    }
    if (carry > 0) {
        curr.next = new ListNode(carry);
    }
    return dummyHead.next;
}
```

**Complexity Analysis**

- Time complexity : $O(\max(m, n))$. Assume that $m$ and $n$ represents the length of $l1$ and $l2$ respectively, the algorithm above iterates at most $\max(m, n)$ times.
- Space complexity : $O(\max(m, n))$. The length of the new list is at most $\max(m,n) + 1$.

**Follow up**

What if the the digits in the linked list are stored in non-reversed order? For example:

$(3 \to 4 \to 2) + (4 \to 6 \to 5) = 8 \to 0 \to 7$



# 19. Remove Nth Node From End of List(M)

Given a linked list, remove the *n*-th node from the end of list and return its head.

**Example:**

```
Given linked list: 1->2->3->4->5, and n = 2.

After removing the second node from the end, the linked list becomes 1->2->3->5.
```

**Note:**

Given *n* will always be valid.

**Follow up:**

Could you do this in one pass?



This article is for beginners. It introduces the following idea: Linked List traversal and removal of nth element from the end.

## Approach 1: Two pass algorithm

**Intuition**

We notice that the problem could be simply reduced to another one : Remove the $(L - n + 1)$ th node from the beginning in the list , where $L$ is the list length. This problem is easy to solve once we found list length $L$.

**Algorithm**

First we will add an auxiliary "dummy" node, which points to the list head. The "dummy" node is used to simplify some corner cases such as a list with only one node, or removing the head of the list. On the first pass, we find the list length $L$. Then we set a pointer to the dummy node and start to move it through the list till it comes to the $(L - n)$ th node. We relink `next` pointer of the $(L - n)$ th node to the $(L - n + 2)$ th node and we are done.

![img](https://ws2.sinaimg.cn/large/69d4185bly1fztmj6v90rj20d805yjs0.jpg)

```java
public ListNode removeNthFromEnd(ListNode head, int n) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    int length  = 0;
    ListNode first = head;
    while (first != null) {
        length++;
        first = first.next;
    }
    length -= n;
    first = dummy;
    while (length > 0) {
        length--;
        first = first.next;
    }
    first.next = first.next.next;
    return dummy.next;
}
```

**Complexity Analysis**

- Time complexity : $O(L)$.

  The algorithm makes two traversal of the list, first to calculate list length $L$ and second to find the $(L - n)$ th node. There are $2L-n$ operations and time complexity is $O(L)$.

- Space complexity : $O(1)$.

  We only used constant extra space. 

## Approach 2: One pass algorithm

**Algorithm**

The above algorithm could be optimized to one pass. Instead of one pointer, we could use two pointers. The first pointer advances the list by $n+1$ steps from the beginning, while the second pointer starts from the beginning of the list. Now, both pointers are exactly separated by n*n* nodes apart. We maintain this constant gap by advancing both pointers together until the first pointer arrives past the last node. The second pointer will be pointing at the n*n*th node counting from the last. We relink the next pointer of the node referenced by the second pointer to point to the node's next next node.

![img](https://ws2.sinaimg.cn/large/69d4185bly1fztmtimn1wj20et0ksdhg.jpg)

```java
public ListNode removeNthFromEnd(ListNode head, int n) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode first = dummy;
    ListNode second = dummy;
    // Advances first pointer so that the gap between first and second is n nodes apart
    for (int i = 1; i <= n + 1; i++) {
        first = first.next;
    }
    // Move first to the end, maintaining the gap
    while (first != null) {
        first = first.next;
        second = second.next;
    }
    second.next = second.next.next;
    return dummy.next;
}
```

**Complexity Analysis**

- Time complexity : $O(L)$.

  The algorithm makes one traversal of the list of L*L* nodes. Therefore time complexity is $O(L)$.

- Space complexity : $O(1)$.

  We only used constant extra space.



# 61.Rotate List（M）

Given a linked list, rotate the list to the right by *k* places, where *k* is non-negative.

**Example 1:**

```
Input: 1->2->3->4->5->NULL, k = 2
Output: 4->5->1->2->3->NULL
Explanation:
rotate 1 steps to the right: 5->1->2->3->4->NULL
rotate 2 steps to the right: 4->5->1->2->3->NULL
```

**Example 2:**

```
Input: 0->1->2->NULL, k = 4
Output: 2->0->1->NULL
Explanation:
rotate 1 steps to the right: 2->0->1->NULL
rotate 2 steps to the right: 1->2->0->NULL
rotate 3 steps to the right: 0->1->2->NULL
rotate 4 steps to the right: 2->0->1->NULL
```

**Intuition**

The nodes in the list are already linked, and hence the rotation basically means

- To close the linked list into the ring.
- To break the ring after the new tail and just in front of the new head.

![rotate](https://leetcode.com/articles/Figures/61/rotate.png)

> Where is the new head?

In the position `n - k`, where `n` is a number of nodes in the list. The new tail is just before, in the position`n - k - 1`.

> We were assuming that `k < n`. What about the case of `k >= n`?

`k` could be rewritten as a sum `k = (k // n) * n + k % n`, where the first term doesn't result in any rotation. Hence one could simply replace `k` by `k % n` to always have number of rotation places smaller than `n`.

**Algorithm**

The algorithm is quite straightforward :

- Find the old tail and connect it with the head `old_tail.next = head` to close the ring. Compute the length of the list `n` at the same time.
- Find the new tail, which is `(n - k % n - 1)`th node from the `head` and the new head, which is `(n - k % n)`th node.
- Break the ring `new_tail.next = None` and return `new_head`.

![image](https://wx1.sinaimg.cn/large/69d4185bly1g0k6by5r9fj20e708s3zj.jpg)

```python
class Solution:
    def rotateRight(self, head: 'ListNode', k: 'int') -> 'ListNode':
        # base cases
        if not head:
            return None
        if not head.next:
            return head
        
        # close the linked list into the ring
        old_tail = head
        n = 1
        while old_tail.next:
            old_tail = old_tail.next
            n += 1
        old_tail.next = head
        
        # find new tail : (n - k % n - 1)th node
        # and new head : (n - k % n)th node
        new_tail = head
        for i in range(n - k % n - 1):
            new_tail = new_tail.next
        new_head = new_tail.next
        
        # break the ring
        new_tail.next = None
        
        return new_head
```

**Complexity Analysis**

- Time complexity : $\mathcal{O}(N)$ where N*N* is a number of elements in the list.
- Space complexity : $\mathcal{O}(1)$ since it's a constant space solution.

Analysis written by @[liaison](https://leetcode.com/liaison/) and @[andvary](https://leetcode.com/andvary/)

# 98.Validate Binary Search Tre(M)

Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

- The left subtree of a node contains only nodes with keys **less than** the node's key.
- The right subtree of a node contains only nodes with keys **greater than** the node's key.
- Both the left and right subtrees must also be binary search trees.

**Example 1:**

```
Input:
    2
   / \
  1   3
Output: true
```

**Example 2:**

```
    5
   / \
  1   4
     / \
    3   6
Output: false
Explanation: The input is: [5,1,4,null,null,3,6]. The root node's value
             is 5 but its right child's value is 4.
```

First of all, here is the definition of the `TreeNode` which we would use.

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None
```

---

#### Intuition

On the first sight, the problem is trivial. Let's traverse the tree and check at each step if `node.right.val > node.val` and `node.left.val < node.val`. This approach would even work for some trees![compute](https://leetcode.com/articles/Figures/98/98_not_bst.png)

The problem is this approach will not work for all cases. Not only the right child should be larger than the node but all the elements in the right subtree. Here is an example :

![compute](https://leetcode.com/articles/Figures/98/98_not_bst_3.png)

That means one should keep both upper and lower limits for each node while traversing the tree, and compare the node value not with children values but with these limits. 

## 1: Recursion

The idea above could be implemented as a recursion. One compares the node value with its upper and lower limits if they are available. Then one repeats the same step recursively for left and right subtrees.

![image](https://ws1.sinaimg.cn/large/69d4185bly1g0pvyxfkstj20b70gxgny.jpg)

```python
class Solution:
    def isValidBST(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        if not root:
            return True

        def isBSTHelper(node, lower_limit, upper_limit):
            if lower_limit is not None and node.val <= lower_limit:
                return False
            if upper_limit is not None and upper_limit <= node.val:
                return False

            left = isBSTHelper(node.left, lower_limit, node.val) if node.left else True
            if left:
                right = isBSTHelper(node.right, node.val, upper_limit) if node.right else True
                return right
            else:
                return False
        
        return isBSTHelper(root, None, None)
```

**Complexity Analysis**

- Time complexity : $\mathcal{O}(N)$ since we visit each node exactly once.
- Space complexity : $\mathcal{O}(N)$ since we keep up to the entire tree.

##  2: Iteration

The above recursion could be converted into iteration, with the help of stack. DFS would be better than BFS since it works faster here.

```python
class Solution:
    def isValidBST(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        if not root:
            return True
            
        stack = [(root, None, None), ] 
        while stack:
            root, lower_limit, upper_limit = stack.pop()
            if root.right:
                if root.right.val > root.val:
                    if upper_limit and root.right.val >= upper_limit:
                        return False
                    stack.append((root.right, root.val, upper_limit))
                else:
                    return False
            if root.left:
                if root.left.val < root.val:
                    if lower_limit and root.left.val <= lower_limit:
                        return False
                    stack.append((root.left, lower_limit, root.val))
                else:
                    return False
        return True  
```

**Complexity Analysis**

- Time complexity : $\mathcal{O}(N)$ since we visit each node exactly once.
- Space complexity : $\mathcal{O}(N)$ since we keep up to the entire tree. 

## 3: Inorder traversal

**Algorithm**

Let's use the order of nodes in the [inorder traversal](https://leetcode.com/articles/binary-tree-inorder-traversal/) `Left -> Node -> Right`.

![postorder](https://leetcode.com/articles/Figures/145_transverse.png)

Here the nodes are enumerated in the order you visit them, and you could follow `1-2-3-4-5` to compare different strategies.

`Left -> Node -> Right` order of inorder traversal means for BST that each element should be smaller than the next one.

Hence the algorithm with \mathcal{O}(N)O(*N*) time complexity and \mathcal{O}(N)O(*N*) space complexity could be simple:

- Compute inorder traversal list `inorder`.
- Check if each element in `inorder` is smaller than the next one.

![postorder](https://leetcode.com/articles/Figures/98/98_bst_inorder.png)

> Do we need to keep the whole `inorder` traversal list?

Actually, no. The last added inorder element is enough to ensure at each step that the tree is BST (or not). Hence one could merge both steps into one and reduce the used space.

```python
class Solution:
    def isValidBST(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        stack, inorder = [], float('-inf')
        
        while stack or root:
            while root:
                stack.append(root)
                root = root.left
            root = stack.pop()
            # If next element in inorder traversal
            # is smaller than the previous one
            # that's not BST.
            if root.val <= inorder:
                return False
            inorder = root.val
            root = root.right

        return True
```

**Complexity Analysis**

- Time complexity : $\mathcal{O}(N)​$ in the worst case when the tree is BST or the "bad" element is a rightmost leaf.
- Space complexity : $\mathcal{O}(N)$ to keep `stack`.

Analysis written by @[liaison](https://leetcode.com/liaison/) and @[andvary](https://leetcode.com/andvary/)

# 328.Odd Even Linked List（M）

Given a singly linked list, group all odd nodes together followed by the even nodes. Please note here we are talking about the node number and not the value in the nodes.

You should try to do it in place. The program should run in O(1) space complexity and O(nodes) time complexity.

**Example 1:**

```
Input: 1->2->3->4->5->NULL
Output: 1->3->5->2->4->NULL
```

**Example 2:**

```
Input: 2->1->3->5->6->4->7->NULL
Output: 2->3->6->7->1->5->4->NULL
```

**Note:**

- The relative order inside both the even and odd groups should remain as it was in the input.
- The first node is considered odd, the second node even and so on ...

**Intuition**

Put the odd nodes in a linked list and the even nodes in another. Then link the evenList to the tail of the oddList.

**Algorithm**

The solution is very intuitive. But it is not trivial to write a concise and bug-free code.

A well-formed `LinkedList` need two pointers head and tail to support operations at both ends. The variables `head` and `odd` are the head pointer and tail pointer of one `LinkedList` we call oddList; the variables `evenHead` and `even` are the head pointer and tail pointer of another `LinkedList` we call evenList. The algorithm traverses the original LinkedList and put the odd nodes into the oddList and the even nodes into the evenList. To traverse a LinkedList we need at least one pointer as an iterator for the current node. But here the pointers `odd` and `even` not only serve as the tail pointers but also act as the iterators of the original list.

The best way of solving any linked list problem is to visualize it either in your mind or on a piece of paper. An illustration of our algorithm is following:

![Illustration of odd even linked list](https://leetcode.com/articles/Figures/328_Odd_Even.svg)

​					*Figure 1. Step by step example of the odd and even linked list.*

```java
public class Solution {
    public ListNode oddEvenList(ListNode head) {
        if (head == null) return null;
        ListNode odd = head, even = head.next, evenHead = even;
        while (even != null && even.next != null) {
            odd.next = even.next;
            odd = odd.next;
            even.next = odd.next;
            even = even.next;
        }
        odd.next = evenHead;
        return head;
    }
}
```

**Complexity Analysis**

- Time complexity : $O(n)$. There are total n*n* nodes and we visit each node once.
- Space complexity : $O(1)​$. All we need is the four pointers.

# 692.Top K Frequent Words(M)

Given a non-empty list of words, return the *k* most frequent elements.

Your answer should be sorted by frequency from highest to lowest. If two words have the same frequency, then the word with the lower alphabetical order comes first.

**Example 1:**

```
Input: ["i", "love", "leetcode", "i", "love", "coding"], k = 2
Output: ["i", "love"]
Explanation: "i" and "love" are the two most frequent words.
    Note that "i" comes before "love" due to a lower alphabetical order.
```

**Example 2:**

```
Input: ["the", "day", "is", "sunny", "the", "the", "the", "sunny", "is", "is"], k = 4
Output: ["the", "is", "sunny", "day"]
Explanation: "the", "is", "sunny" and "day" are the four most frequent words,
    with the number of occurrence being 4, 3, 2 and 1 respectively.
```

**Note:**

1. You may assume *k* is always valid, 1 ≤ *k* ≤ number of unique elements.
2. Input words contain only lowercase letters.

**Follow up:**

1. Try to solve it in $O(n log k)​$ time and *O*(*n*) extra space.

## 1: Sorting

**Intuition and Algorithm**

Count the frequency of each word, and sort the words with a custom ordering relation that uses these frequencies. Then take the best `k` of them.

**Python**

```python
class Solution(object):
    def topKFrequent(self, words, k):
        count = collections.Counter(words)
        candidates = count.keys()
        candidates.sort(key = lambda w: (-count[w], w))
        return candidates[:k]
```

**Java**

```java
class Solution {
    public List<String> topKFrequent(String[] words, int k) {
        Map<String, Integer> count = new HashMap();
        for (String word: words) {
            count.put(word, count.getOrDefault(word, 0) + 1);
        }
        List<String> candidates = new ArrayList(count.keySet());
        Collections.sort(candidates, (w1, w2) -> count.get(w1).equals(count.get(w2)) ?
                w1.compareTo(w2) : count.get(w2) - count.get(w1));

        return candidates.subList(0, k);
```

**Complexity Analysis**

- Time Complexity: $O(N \log{N})$, where N*N* is the length of `words`. We count the frequency of each word in O(N)*O*(*N*) time, then we sort the given words in $O(N \log{N})$ time.
- Space Complexity: $O(N)$, the space used to store our `candidates`.

## 2: Heap

**Intuition and Algorithm**

Count the frequency of each word, then add it to heap that stores the best `k` candidates. Here, "best" is defined with our custom ordering relation, which puts the worst candidates at the top of the heap. At the end, we pop off the heap up to `k` times and reverse the result so that the best candidates are first.

In Python, we instead use `heapq.heapify`, which can turn a list into a heap in linear time, simplifying our work.

**Java**

```java
class Solution {
    public List<String> topKFrequent(String[] words, int k) {
        Map<String, Integer> count = new HashMap();
        for (String word: words) {
            count.put(word, count.getOrDefault(word, 0) + 1);
        }
        PriorityQueue<String> heap = new PriorityQueue<String>(
                (w1, w2) -> count.get(w1).equals(count.get(w2)) ?
                w2.compareTo(w1) : count.get(w1) - count.get(w2) );

        for (String word: count.keySet()) {
            heap.offer(word);
            if (heap.size() > k) heap.poll();
        }

        List<String> ans = new ArrayList();
        while (!heap.isEmpty()) ans.add(heap.poll());
        Collections.reverse(ans);
        return ans;
    }
}
```

**python**

```python
class Solution(object):
    def topKFrequent(self, words, k):
        count = collections.Counter(words)
        heap = [(-freq, word) for word, freq in count.items()]
        heapq.heapify(heap)
        return [heapq.heappop(heap)[1] for _ in xrange(k)]
```

**Complexity Analysis**

- Time Complexity: $O(N \log{k})$, where N*N* is the length of `words`. We count the frequency of each word in O(N)*O*(*N*) time, then we add N*N* words to the heap, each in O(\log {k}) time. Finally, we pop from the heap up to k*k* times. As $k \leq N$, this is $O(N \log{k})$ in total.

In Python, we improve this to $O(N + k \log {N})$: our `heapq.heapify` operation and counting operations are $O(N)$, and each of k*k* `heapq.heappop` operations are $O(\log {N})​$.

- Space Complexity: $O(N)$, the space used to store our `count`.

------

Analysis written by: [@awice](https://leetcode.com/awice).

# 23.Merge k Sorted List(H)

Merge *k* sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

**Example:**

```
Input:
[
  1->4->5,
  1->3->4,
  2->6
]
Output: 1->1->2->3->4->4->5->6
```

## Approach 1: Brute Force

**Intuition & Algorithm**

- Traverse all the linked lists and collect the values of the nodes into an array.
- Sort and iterate over this array to get the proper value of nodes.
- Create a new sorted linked list and extend it with the new nodes.

As for sorting, you can refer [here](https://www.cs.cmu.edu/~adamchik/15-121/lectures/Sorting%20Algorithms/sorting.html) for more about sorting algorithms.

```python
class Solution(object):
    def mergeKLists(self, lists):
        """
        :type lists: List[ListNode]
        :rtype: ListNode
        """
        self.nodes = []
        head = point = ListNode(0)
        for l in lists:
            while l:
                self.nodes.append(l.val)
                l = l.next
        for x in sorted(self.nodes):
            point.next = ListNode(x)
            point = point.next
        return head.next
```

**Complexity Analysis**

- Time complexity : $O(N\log N)$ where $N$ is  the total number of nodes.
  - Collecting all the values costs $O(N)$ time.
  - A stable sorting algorithm costs $O(N\log N)$ time.
  - Iterating for creating the linked list costs $O(N)$time.
- Space complexity : $O(N)$.
  - Sorting cost $O(N)$ space (depends on the algorithm you choose).
  - Creating a new linked list costs $O(N)$ space. 



## Approach 2: Compare one by one

**Algorithm**

- Compare every \text{k}k nodes (head of every linked list) and get the node with the smallest value.
- Extend the final sorted linked list with the selected nodes.

![image](https://ws4.sinaimg.cn/large/69d4185bly1fzwvagd09pg20hs0b4q6n.gif)



![image](https://ws1.sinaimg.cn/large/69d4185bly1fzwxb1en6hj20qv0hbtbr.jpg)

**Complexity Analysis**

- Time complexity : $O(kN)$ where $\text{k}$ is the number of linked lists.
  - Almost every selection of node in final linked costs $O(k)$ ($\text{k-1}$ times comparison).
  - There are $N$ nodes in the final linked list.
- Space complexity :
  - $O(n)$ Creating a new linked list costs $O(n)$  space.
  - $O(1)$ It's not hard to apply in-place method - connect selected nodes instead of creating new nodes to fill the new linked list.















