# 599.Minimum Index Sum of Two Lists(E)

Suppose Andy and Doris want to choose a restaurant for dinner, and they both have a list of favorite restaurants represented by strings.

You need to help them find out their **common interest** with the **least list index sum**. If there is a choice tie between answers, output all of them with no order requirement. You could assume there always exists an answer.

**Example 1:**

```
Input:
["Shogun", "Tapioca Express", "Burger King", "KFC"]
["Piatti", "The Grill at Torrey Pines", "Hungry Hunter Steakhouse", "Shogun"]
Output: ["Shogun"]
Explanation: The only restaurant they both like is "Shogun".
```

**Example 2:**

```
Input:
["Shogun", "Tapioca Express", "Burger King", "KFC"]
["KFC", "Shogun", "Burger King"]
Output: ["Shogun"]
Explanation: The restaurant they both like and have the least index sum is "Shogun" with index sum 1 (0+1).
```

**Note:**

1. The length of both lists will be in the range of [1, 1000].
2. The length of strings in both lists will be in the range of [1, 30].
3. The index is starting from 0 to the list length minus 1.
4. No duplicates in both lists.

## Approach #1 Using HashMap [Accepted]

In this approach, we compare every string in $list1$ and $list2$ by traversing over the whole list $list2$ for every string chosen from $list1$. We make use of a hashmap $map$, which contains elements of the form ($sum : list_{sum}$). Here, $sum$ refers to the sum of indices of matching elements and $list_{sum}$ refers to the list of matching strings whose indices' sum equals $sum$.

Thus, while doing the comparisons, whenever a match between a string at $i^{th}$ index of $list1$ and $j^{th}$ index of $list2$ is found, we make an entry in the $map$ corresponding to the sum $i + j$, if this entry isn't already present. If an entry with this sum already exists, we need to keep a track of all the strings which lead to the same index sum. Thus, we append the current string to the list of strings corresponding to sum $i + j$.

At the end, we traverse over the keys of the $map$ and find out the list of strings corresponding to the key reprsenting the minimum sum.

```java
public class Solution {
    public String[] findRestaurant(String[] list1, String[] list2) {
        HashMap < Integer, List < String >> map = new HashMap < > ();
        for (int i = 0; i < list1.length; i++) {
            for (int j = 0; j < list2.length; j++) {
                if (list1[i].equals(list2[j])) {
                    if (!map.containsKey(i + j))
                        map.put(i + j, new ArrayList < String > ());
                    map.get(i + j).add(list1[i]);
                }
            }
        }
        int min_index_sum = Integer.MAX_VALUE;
        for (int key: map.keySet())
            min_index_sum = Math.min(min_index_sum, key);
        String[] res = new String[map.get(min_index_sum).size()];
        return map.get(min_index_sum).toArray(res);
    }
}
```

**Complexity Analysis**

- Time complexity : $O(l_1*l_2*x)$. Every item of $list1$ is compared with all the items of $list2. l_1$ and $l_2$ are the lengths of $list1$ and $list2$ respectively. And $x$ refers to average string length.
- Space complexity : $O(l_1*l_2*x)$. In worst case all items of $list1$ and $list2$ are same. In that case, hashmap size grows upto $l_1*l_2*x$, where $x$ refers to average string length.

## Approach #2 Without Using HashMap [Accepted]

**Algorithm**

Another method could be to traverse over the various $sum$(index sum) values and determine if any such string exists in $list1$ and $list2$ such that the sum of its indices in the two lists equals $sum$.

Now, we know that the value of index sum, $sum$ could range from 0 to $m + n - 1$. Here, $m$ and $n$ refer to the length of lists $list1$ and $list2$ respectively. Thus, we choose every value of $sum$ in ascending order. For every $sum$ chosen, we iterate over $list1$. Suppose, currently the string at $i^{th}$ index in $list1$ is being considered. Now, in order for the index sum $sum$ to be the one corresponding to matching strings in $list1$ and $list2$, the string at index $j$ in $list2$ should match the string at index $i$ in $list1$, such that $sum = i + j$.

Or, stating in other terms, the string at index $j$ in $list2$ should be equal to the string at index $i$ in $list1$, such that $j = sum - i$. Thus, for a particular $sum$ and $i$(from $list1$), we can directly determine that we need to check the element at index $j= sum - i$ in $list2$, instead of traversing over the whole $list2$.

Doing such checks/comparisons, iterate over all the indices of $list1$ for every $sum$ value chosen. Whenver a match occurs between $list1$ and $list2$, we put the matching string in a list $res​$.

We do the same process of checking the strings for all the values of sum*s**u**m* in ascending order. After completing every iteration over $list1$ for a particular $sum$, we check if the $res$ list is empty or not. If it is empty, we need to continue the process with the next $sum$ value considered. If not, the current $res$ gives the required list with minimum index sum. This is because we are already considering the index sum values in ascending order. So, the first list to be found is the required resultant list.

The following example depicts the process:https://leetcode.com/articles/minimum-index-sum-of-two-lists/

```java
public class Solution {
    public String[] findRestaurant(String[] list1, String[] list2) {
        List < String > res = new ArrayList < > ();
        for (int sum = 0; sum < list1.length + list2.length - 1; sum++) {
            for (int i = 0; i <= sum; i++) {
                if (i < list1.length && sum - i < list2.length && list1[i].equals(list2[sum - i]))
                    res.add(list1[i]);
            }
            if (res.size() > 0)
                break;
        }
        return res.toArray(new String[res.size()]);
    }
}
```

**Complexity Analysis**

- Time complexity : $O((l_1+l_2)^2*x)​$. There are two nested loops upto $l_1+l_2​$ and string comparison takes $x​$ time. Here, $x​$ refers to the average string length.
- Space complexity : $O(r*x)$. $res$ list is used to store the result. Assuming $r$ is the length of $res$.

## Approach #3 Using HashMap (linear) [Accepted]

We make use of a HashMap to solve the given problem in a different way in this approach. Firstly, we traverse over the whole $list1$ and create an entry for each element of $list1$ in a HashMap $map$, of the form ($list[i], i$). Here, i*i* refers to the index of the $i^{th}$ element, and $list[i]$ is the $i^{th}$ element itself. Thus, we create a mapping from the elements of $list1$ to their indices.

Now, we traverse over $list2$. For every element $,list2[j]$, of $list2$ encountered, we check if the same element already exists as a key in the $map$. If so, it means that the element exists in both $list1$ and $list2$. Thus, we find out the sum of indices corresponding to this element in the two lists, given by $sum = map.get(list[j]) + j$. If this $sum$ is lesser than the minimum sum obtained till now, we update the resultant list to be returned, $res$, with the element $list2[j]$ as the only entry in it.

If the $sum$ is equal to the minimum sum obtained till now, we put an extra entry corresponding to the element $list2[j]$ in the $res$ list.

Below code is inspired by [@cloud.runner](https://leetcode.com/cloud.runner)

```java
public class Solution {
    public String[] findRestaurant(String[] list1, String[] list2) {
        HashMap < String, Integer > map = new HashMap < String, Integer > ();
        for (int i = 0; i < list1.length; i++)
            map.put(list1[i], i);
        List < String > res = new ArrayList < > ();
        int min_sum = Integer.MAX_VALUE, sum;
        for (int j = 0; j < list2.length && j <= min_sum; j++) {
            if (map.containsKey(list2[j])) {
                sum = j + map.get(list2[j]);
                if (sum < min_sum) {
                    res.clear();
                    res.add(list2[j]);
                    min_sum = sum;
                } else if (sum == min_sum)
                    res.add(list2[j]);
            }
        }
        return res.toArray(new String[res.size()]);
    }
}
```

**Complexity Analysis**

- Time complexity : $O(l_1+l_2)$. Every item of list2 is checked in a map of $list1. l_1$ and $l_2$ are the lengths of list1 and list2 respectively.
- Space complexity : $O(l_1*x)$. hashmap size grows upto $l_1*x$, where $x$ refers to average string length.



# 20.Valid Parentheses(E)

Given a string containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.

Note that an empty string is also considered valid.

**Example 1:**

```
Input: "()"
Output: true
```

**Example 2:**

```
Input: "()[]{}"
Output: true
```

**Example 3:**

```
Input: "(]"
Output: false
```

**Example 4:**

```
Input: "([)]"
Output: false
```

**Example 5:**

```
Input: "{[]}"
Output: true
```



## Intuition

Imagine you are writing a small compiler for your college project and one of the tasks (or say sub-tasks) for the compiler would be to detect if the parenthesis are in place or not.

The algorithm we will look at in this article can be then used to process all the parenthesis in the program your compiler is compiling and checking if all the parenthesis are in place. This makes checking if a given string of parenthesis is valid or not, an important programming problem.

The expressions that we will deal with in this problem can consist of three different type of parenthesis:

- `()`,
- `{}` and
- `[]`

Before looking at how we can check if a given expression consisting of these parenthesis is valid or not, let us look at a simpler version of the problem that consists of just one type of parenthesis. So, the expressions we can encounter in this simplified version of the problem are e.g.

```
(((((()))))) -- VALID

()()()()     -- VALID

(((((((()    -- INVALID

((()(())))   -- VALID
```

Let's look at a simple algorithm to deal with this problem.

1. We process the expression one bracket at a time starting from the left.
2. Suppose we encounter an opening bracket i.e. `(`, it may or may not be an invalid expression because there can be a matching ending bracket somewhere in the remaining part of the expression. Here, we simply increment the counter keeping track of left parenthesis till now. `left += 1`
3. If we encounter a closing bracket, this has two meanings:
   1. One, there was no matching opening bracket for this closing bracket and in that case we have an invalid expression. This is the case when `left == 0` i.e. when there are no unmatched left brackets available.
   2. We had `some unmatched` opening bracket available to match this closing bracket. This is the case when `left > 0` i.e. we have unmatched left brackets available.
4. If we encounter a closing bracket i.e. `)` when `left == 0`, then we have an invalid expression on our hands. Else, we decrement `left` thus reducing the number of unmatched left parenthesis available.
5. Continue processing the string until all parenthesis have been processed.
6. If in the end we still have unmatched left parenthesis available, this implies an invalid expression.

The reason we discussed this particular algorithm here is because the approach for the original problem derives its inspiration from this very solution. Have a look at the following dry run of the algorithm we discussed to have a better understanding.

![image](https://wx4.sinaimg.cn/large/69d4185bly1g0eaxegudsg20hs0b442g.gif)

If we try and follow the same approach for our original problem, then it simply won't work. The reason a simple counter based approach works above is because all the parenthesis are of the same type. So when we encounter a closing bracket, we simply assume a corresponding opening matching bracket to be available i.e. if `left > 0`.

But, in our problem, if we encounter say `]`, we don't really know if there is a corresponding opening `[`available or not. You could say:

> Why not maintain a separate counter for the different types of parenthesis?

This doesn't work because the relative placement of the parenthesis also matters here. e.g.:

```
[{]
```

If we simply keep counters here, then as soon as we encounter the closing square bracket, we would know there is an unmatched opening square bracket available as well. But, the **closest unmatched opening bracket available is a curly bracket and not a square bracket** and hence the counting approach breaks here. 

## Approach 1: Stacks

An interesting property about a valid parenthesis expression is that a sub-expression of a valid expression should also be a valid expression. (Not every sub-expression) e.g.



![img](https://leetcode.com/articles/Figures/20/20-Valid-Parentheses-Recursive-Property.png)



Also, if you look at the above structure carefully, the color coded cells mark the opening and closing pairs of parenthesis. The entire expression is valid, but sub portions of it are also valid in themselves. This lends a sort of a recursive structure to the problem. For e.g. Consider the expression enclosed within the two green parenthesis in the diagram above. The opening bracket at index `1` and the corresponding closing bracket at index `6`.

> What if whenever we encounter a matching pair of parenthesis in the expression, we simply remove it from the expression?

Let's have a look at this idea below where remove the smaller expressions one at a time from the overall expression and since this is a valid expression, we would be left with an empty string in the end.

![image](https://wx4.sinaimg.cn/large/69d4185bly1g0eb3okqz7j20fa0eq77p.jpg)



> The stack data structure can come in handy here in representing this recursive structure of the problem. We can't really process this from the inside out because we don't have an idea about the overall structure. But, the stack can help us process this recursively i.e. from outside to inwards.

Let us have a look at the algorithm for this problem using stacks as the intermediate data structure.

**Algorithm**

1. Initialize a stack S.
2. Process each bracket of the expression one at a time.
3. If we encounter an opening bracket, we simply push it onto the stack. This means we will process it later, let us simply move onto the **sub-expression** ahead.
4. If we encounter a closing bracket, then we check the element on top of the stack. If the element at the top of the stack is an opening bracket `of the same type`, then we pop it off the stack and continue processing. Else, this implies an invalid expression.
5. In the end, if we are left with a stack still having elements, then this implies an invalid expression.

We'll have a look a dry run for the algorithm and then move onto the implementation.

![image](https://ws1.sinaimg.cn/large/69d4185bly1g0ebbqataog20hs0b443d.gif)

Let us now have a look at the implementation for this algorithm.

```python
class Solution(object):
    def isValid(self, s):
        """
        :type s: str
        :rtype: bool
        """

        # The stack to keep track of opening brackets.
        stack = []

        # Hash map for keeping track of mappings. This keeps the code very clean.
        # Also makes adding more types of parenthesis easier
        mapping = {")": "(", "}": "{", "]": "["}

        # For every bracket in the expression.
        for char in s:

            # If the character is an closing bracket
            if char in mapping:

                # Pop the topmost element from the stack, if it is non empty
                # Otherwise assign a dummy value of '#' to the top_element variable
                top_element = stack.pop() if stack else '#'

                # The mapping for the opening bracket in our hash and the top
                # element of the stack don't match, return False
                if mapping[char] != top_element:
                    return False
            else:
                # We have an opening bracket, simply push it onto the stack.
                stack.append(char)

        # In the end, if the stack is empty, then we have a valid expression.
        # The stack won't be empty for cases like ((()
        return not stack
```

**Complexity analysis**

- Time complexity : $O(n)$ because we simply traverse the given string one character at a time and push and pop operations on a stack take $O(1)$ time.
- Space complexity : $O(n)$ as we push all opening brackets onto the stack and in the worst case, we will end up pushing all the brackets onto the stack. e.g. `((((((((((`.

# 3.Longest Substring Without Repeating Characters(M)

tag: `hastset`、`two point`、`slide window`、`HashMap`

Given a string, find the length of the **longest substring** without repeating characters.

**Example 1:**

```
Input: "abcabcbb"
Output: 3 
Explanation: The answer is "abc", with the length of 3. 
```

**Example 2:**

```
Input: "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

**Example 3:**

```
Input: "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3. 
             Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

## 1: Brute Force

**Intuition**

Check all the substring one by one to see if it has no duplicate character.

**Algorithm**

Suppose we have a function `boolean allUnique(String substring)` which will return true if the characters in the substring are all unique, otherwise false. We can iterate through all the possible substrings of the given string `s` and call the function `allUnique`. If it turns out to be true, then we update our answer of the maximum length of substring without duplicate characters.

Now let's fill the missing parts:

1. To enumerate all substrings of a given string, we enumerate the start and end indices of them. Suppose the start and end indices are i*i* and $j$, respectively. Then we have $0 \leq i \lt j \leq n$ (here end index $j$ is exclusive by convention). Thus, using two nested loops with $i$ from 0 to $n - 1$ and $j$ from $i+1$ to $n$, we can enumerate all the substrings of `s`.
2. To check if one string has duplicate characters, we can use a set. We iterate through all the characters in the string and put them into the `set` one by one. Before putting one character, we check if the set already contains it. If so, we return `false`. After the loop, we return `true`.

```java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length();
        int ans = 0;
        for (int i = 0; i < n; i++)
            for (int j = i + 1; j <= n; j++)
                if (allUnique(s, i, j)) ans = Math.max(ans, j - i);
        return ans;
    }

    public boolean allUnique(String s, int start, int end) {
        Set<Character> set = new HashSet<>();
        for (int i = start; i < end; i++) {
            Character ch = s.charAt(i);
            if (set.contains(ch)) return false;
            set.add(ch);
        }
        return true;
    }
}
```

**Complexity Analysis**

- Time complexity : $O(n^3)$.

  To verify if characters within index range $[i, j)$ are all unique, we need to scan all of them. Thus, it costs $O(j - i)$ time.

  For a given `i`, the sum of time costed by each $j \in [i+1, n]$ is
  $$
  \large{\sum_{i+1}^{n}O(j - i)}
  $$
  Thus, the sum of all the time consumption is:
  $$
  \large{O\left(\sum_{i = 0}^{n - 1}\left(\sum_{j = i + 1}^{n}(j - i)\right)\right) = O\left(\sum_{i = 0}^{n - 1}\frac{(1 + n - i)(n - i)}{2}\right) = O(n^3)}
  $$

- Space complexity : $O(min(n, m))$. We need $O(k)$ space for checking a substring has no duplicate characters, where $k$ is the size of the `Set`. The size of the Set is upper bounded by the size of the string $n$ and the size of the charset/alphabet $m$. 

## 2: Sliding Window

**Algorithm**

The naive approach is very straightforward. But it is too slow. So how can we optimize it?

In the naive approaches, we repeatedly check a substring to see if it has duplicate character. But it is unnecessary. If a substring $s_{ij}$ from index $i$ to $j−1$ is already checked to have no duplicate characters. We only need to check if $s[j]$ is already in the substring $s_{ij}$.

To check if a character is already in the substring, we can scan the substring, which leads to an $O(n^2)$ algorithm. But we can do better.

By using HashSet as a sliding window, checking if a character in the current can be done in $O(1)$.

A sliding window is an abstract concept commonly used in array/string problems. A window is a range of elements in the array/string which usually defined by the start and end indices, i.e. $[i, j)$(left-closed, right-open). A sliding window is a window "slides" its two boundaries to the certain direction. For example, if we slide $[i, j)$ to the right by $1$ element, then it becomes $[i+1, j+1)$ (left-closed, right-open).

Back to our problem. We use HashSet to store the characters in current window $[i, j)$ (j = i*j*=*i* initially). Then we slide the index $j$ to the right. If it is not in the HashSet, we slide $j$ further. Doing so until $s[j]$ is already in the HashSet. At this point, we found the maximum size of substrings without duplicate characters start with index $i$. If we do this for all i*i*, we get our answer.

```java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length();
        Set<Character> set = new HashSet<>();
        int ans = 0, i = 0, j = 0;
        while (i < n && j < n) {
            // try to extend the range [i, j]
            if (!set.contains(s.charAt(j))){
                set.add(s.charAt(j++));
                ans = Math.max(ans, j - i);
            }
            else {
                set.remove(s.charAt(i++));
            }
        }
        return ans;
    }
}
```

**Complexity Analysis**

- Time complexity : $O(2n) = O(n)$. In the worst case each character will be visited twice by $i$ and $j$.
- Space complexity : $O(min(m, n))​$. Same as the previous approach. We need $O(k)​$ space for the sliding window, where $k​$ is the size of the `Set`. The size of the Set is upper bounded by the size of the string n and the size of the charset/alphabet $m​$. 

## 3: Sliding Window Optimized

The above solution requires at most 2n steps. In fact, it could be optimized to require only n steps. Instead of using a set to tell if a character exists or not, we could define a mapping of the characters to its index. Then we can skip the characters immediately when we found a repeated character.

The reason is that if $s[j]$ have a duplicate in the range $[i, j)$ with index $j$, we don't need to increase $i$ little by little. We can skip all the elements in the range $[i, j']$ and let $i$ to be $j' + 1$ directly.

**Java (Using HashMap)**

```java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length(), ans = 0;
        Map<Character, Integer> map = new HashMap<>(); // current index of character
        // try to extend the range [i, j]
        for (int j = 0, i = 0; j < n; j++) {
            if (map.containsKey(s.charAt(j))) {
                i = Math.max(map.get(s.charAt(j)), i);
            }
            ans = Math.max(ans, j - i + 1);
            map.put(s.charAt(j), j + 1);
        }
        return ans;
    }
}
```

**Java (Assuming ASCII 128)**

The previous implements all have no assumption on the charset of the string `s`.

If we know that the charset is rather small, we can replace the `Map` with an integer array as direct access table.

Commonly used tables are:

- `int[26]` for Letters 'a' - 'z' or 'A' - 'Z'
- `int[128]` for ASCII
- `int[256]` for Extended ASCII

```java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length(), ans = 0;
        int[] index = new int[128]; // current index of character
        // try to extend the range [i, j]
        for (int j = 0, i = 0; j < n; j++) {
            i = Math.max(index[s.charAt(j)], i);
            ans = Math.max(ans, j - i + 1);
            index[s.charAt(j)] = j + 1;
        }
        return ans;
    }
}
```

**Complexity Analysis**

- Time complexity : $O(n)$. Index $j$ will iterate n*n* times.
- Space complexity (HashMap) : $O(min(m, n))$. Same as the previous approach.
- Space complexity (Table): $O(m)$. $m$ is the size of the charset.





# 946.Validate Stack Sequences(M)

Given two sequences `pushed` and `popped` **with distinct values**, return `true` if and only if this could have been the result of a sequence of push and pop operations on an initially empty stack.

 

**Example 1:**

```
Input: pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
Output: true
Explanation: We might do the following sequence:
push(1), push(2), push(3), push(4), pop() -> 4,
push(5), pop() -> 5, pop() -> 3, pop() -> 2, pop() -> 1
```

**Example 2:**

```
Input: pushed = [1,2,3,4,5], popped = [4,3,5,1,2]
Output: false
Explanation: 1 cannot be popped before 2.
```

**Note:**

1. `0 <= pushed.length == popped.length <= 1000`
2. `0 <= pushed[i], popped[i] < 1000`
3. `pushed` is a permutation of `popped`.
4. `pushed` and `popped` have distinct values.

## Approach 1: Greedy

**Intuition**

We have to push the items in order, so when do we pop them?

If the stack has say, `2` at the top, then if we have to pop that value next, we must do it now. That's because any subsequent push will make the top of the stack different from `2`, and we will never be able to pop again.

**Algorithm**

For each value, push it to the stack.

Then, greedily pop values from the stack if they are the next values to pop.

At the end, we check if we have popped all the values successfully.

```python
class Solution(object):
    def validateStackSequences(self, pushed, popped):
        j = 0
        stack = []
        for x in pushed:
            stack.append(x)
            while stack and j < len(popped) and stack[-1] == popped[j]:
                stack.pop()
                j += 1

        return j == len(popped)
```

**Complexity Analysis**

- Time Complexity: $O(N)$, where N*N* is the length of `pushed` and `popped`.
- Space Complexity: $O(N)$. 

Analysis written by: [@awice](https://leetcode.com/awice).

# 560.Subarray Sum Equals K(M)

Given an array of integers and an integer **k**, you need to find the total number of continuous subarrays whose sum equals to **k**.

**Example 1:**

```
Input:nums = [1,1,1], k = 2
Output: 2
```

**Note:**

1. The length of the array is in range [1, 20,000].
2. The range of numbers in the array is [-1000, 1000] and the range of the integer **k** is [-1e7, 1e7].

## #1 Brute Force

**Algorithm**

The simplest method is to consider every possible subarray of the given $nums$ array, find the sum of the elements of each of those subarrays and check for the equality of the sum obtained with the given $k$. Whenver the sum equals $k$, we can increment the $count$ used to store the required result.

```java
public class Solution {
    public int subarraySum(int[] nums, int k) {
        int count = 0;
        for (int start = 0; start < nums.length; start++) {
            for (int end = start + 1; end <= nums.length; end++) {
                int sum = 0;
                for (int i = start; i < end; i++)
                    sum += nums[i];
                if (sum == k)
                    count++;
            }
        }
        return count;
    }
}
```

**Complexity Analysis**

- Time complexity : $O(n^3)$. Considering every possible subarray takes $O(n^2)$ time. For each of the subarray we calculate the sum taking $O(n)$ time in the worst case, taking a total of $O(n^3)$ time.
- Space complexity : $O(1)$. Constant space is used.

## #2 Using Cummulative sum

**Algorithm**

Instead of determining the sum of elements everytime for every new subarray considered, we can make use of a cumulative sum array , $sum$. Then, in order to calculate the sum of elements lying between two indices, we can subtract the cumulative sum corresponding to the two indices to obtain the sum directly, instead of iterating over the subarray to obtain the sum.

In this implementation, we make use of a cumulative sum array, sum*s**u**m*, such that $sum[i]$ is used to store the cumulative sum of $nums$ array upto the element corresponding to the $(i-1)^{th}$ index. Thus, to determine the sum of elements for the subarray $nums[i:j]$, we can directly use $sum[j+1] - sum[i]$.

```java
public class Solution {
    public int subarraySum(int[] nums, int k) {
        int count = 0;
        int[] sum = new int[nums.length + 1];
        sum[0] = 0;
        for (int i = 1; i <= nums.length; i++)
            sum[i] = sum[i - 1] + nums[i - 1];
        for (int start = 0; start < nums.length; start++) {
            for (int end = start + 1; end <= nums.length; end++) {
                if (sum[end] - sum[start] == k)
                    count++;
            }
        }
        return count;
    }
}
```

**Complexity Analysis**

- Time complexity : $O(n^2)$. Considering every possible subarray takes $O(n^2)$ time. Finding out the sum of any subarray takes $O(1)$ time after the initial processing of $O(n)​$ for creating the cumulative sum array.
- Space complexity : $O(n)$. Cumulative sum array $sum$ of size $n+1$ is used.

## #3 Without space

**Algorithm**

Instead of considering all the $start$ and $end$ points and then finding the sum for each subarray corresponding to those points, we can directly find the sum on the go while considering different $end$ points. i.e. We can choose a particular $start$ point and while iterating over the $end$ points, we can add the element corresponding to the $end$ point to the sum formed till now. Whenver the $sum$ equals the required k*k* value, we can update the $count$ value. We do so while iterating over all the $end$ indices possible for every $start$ index. Whenver, we update the $start$ index, we need to reset the $sum$ value to 0.

```java
public class Solution {
    public int subarraySum(int[] nums, int k) {
        int count = 0;
        for (int start = 0; start < nums.length; start++) {
            int sum=0;
            for (int end = start; end < nums.length; end++) {
                sum+=nums[end];
                if (sum == k)
                    count++;
            }
        }
        return count;
    }
}
```

**Complexity Analysis**

- Time complexity : $O(n^2)$. We need to consider every subarray possible.
- Space complexity : $O(1)$. Constant space is used.

## #4 Using hashmap

**Algorithm**

The idea behind this approach is as follows: If the cumulative sum(repreesnted by $sum[i]$ for sum upto $i^{th}$ index) upto two indices is the same, the sum of the elements lying in between those indices is zero. Extending the same thought further, if the cumulative sum upto two indices, say $i$ and $j$ is at a difference of k*k* i.e. if $sum[i] - sum[j] = k$, the sum of elements lying between indices $i$ and $j$ is $k$.

Based on these thoughts, we make use of a hashmap map*m**a**p* which is used to store the cumulative sum upto all the indices possible along with the number of times the same sum occurs. We store the data in the form: ($sum_i$, no. of occurences of $sum_i$). We traverse over the array $nums$ and keep on finding the cumulative sum. Every time we encounter a new sum, we make a new entry in the hashmap corresponding to that sum. If the same sum occurs again, we increment the count corresponding to that sum in the hashmap. Further, for every sum encountered, we also determine the number of times the sum sum-k as occured already, since it will determine the number of times a subarray with sum $k$ has occured upto the current index. We increment the $count$ by the same amount.

After the complete array has been traversed, the $count$ gives the required result.

The animation below depicts the process. https://leetcode.com/articles/subarray-sum-equals-k/

```java
public class Solution {
    public int subarraySum(int[] nums, int k) {
        int count = 0, sum = 0;
        HashMap < Integer, Integer > map = new HashMap < > ();
        map.put(0, 1);
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
            if (map.containsKey(sum - k))
                count += map.get(sum - k);
            map.put(sum, map.getOrDefault(sum, 0) + 1);
        }
        return count;
    }
}
```

**Complexity Analysis**

- Time complexity : $O(n)$. The entire $nums$ array is traversed only once.
- Space complexity : $O(n)$. Hashmap $map$ can contain upto $n$ distinct entries in the worst case.

# 523.Continuous Subarray Sum(M)

Given a list of **non-negative** numbers and a target **integer** k, write a function to check if the array has a continuous subarray of size at least 2 that sums up to the multiple of **k**, that is, sums up to n*k where n is also an **integer**.

**Example 1:**

```
Input: [23, 2, 4, 6, 7],  k=6
Output: True
Explanation: Because [2, 4] is a continuous subarray of size 2 and sums up to 6.
```

**Example 2:**

```
Input: [23, 2, 6, 4, 7],  k=6
Output: True
Explanation: Because [23, 2, 6, 4, 7] is an continuous subarray of size 5 and sums up to 42.
```

**Note:**

1. The length of the array won't exceed 10,000.
2. You may assume the sum of all the numbers is in the range of a signed 32-bit integer.

reference:

- [Share my O(n) C++ accumulation-modulo solution with thinking process and explanation](https://leetcode.com/problems/continuous-subarray-sum/discuss/99545/Share-my-O(n)-C%2B%2B-accumulation-modulo-solution-with-thinking-process-and-explanation)
- [Simple Python (10 lines) with Explanation 58ms O(n) time - O(k) space](https://leetcode.com/problems/continuous-subarray-sum/discuss/99566/Simple-Python-(10-lines)-with-Explanation-58ms-O(n)-time-O(k)-space)

## Process

**Calculate the summation of a continuous subarray**

As we know *the summation of series*

$$
\large{S(n) = a(1) + a(2) + ... + a(n), n ≥ 1}
$$
which has a recursion formula

$$
\large{
S(n) = a(1), n = 1 \\
S(n) = a(n) + S(n - 1), n > 1
}
$$

Suppose the summation of a subarray from $a(i)$ to $a(j)$ is
$$
\large{T(i, j) = a(i) + a(i + 1) + ... + a(j - 1) + a(j), 1≤ i < j ≤ n}
$$

It can be inferred that
$$
\large{
T(i, j) = S(j), i = 1 \\
T(i, j) = S(j) - S(i - 1), i > 1
}
$$
**Define the multiple of k (k ≠ 0) by modulo**

The problem is to find a continuous subarray of size at least 2 that sums up to the multiple of k, which means

$$
\large{T(i, j) = n × k, 1≤ i < j ≤ n}
$$

That is to say
$$
\large{

S(j) = n × k , 1 = i < j \\
S(j) - S(i - 1) = n × k, 1 < i < j

}
$$
By doing the modulo, we get
$$
\large{

S(j) ≡ 0 \; mod \; k , 1 = i < j \\
S(j) ≡ S(i - 1) \; mod \; k, 1 < i < j
}
$$

## Algorithm

**Special cases**

- A. The size of array < 2

Since the size of subarray is at least 2, return false.

- B. k = 0

$$
\large{T(i, j) = a(i) + a(i + 1) + ... + a(j - 1) + a(j) = 0}
$$


As the array only contains non-negative numbers, that is to say
$$
\large{a(i) = a(i + 1) = ... = a(j - 1) = a(j) = 0}
$$

Since the size of subarray is at least 2:

1. if there are 2 adjacent zeros in the array, return true.

2. If not, return false.

## Normal situation

**Step 1: Summation**

Do iteration by using the recursion formula
$$
\large{
S(n) = a(1), n = 1 \\
S(n) = a(n) + S(n - 1), n > 1
}
$$
**Step 2: Modulo operation**

There are 2 situations:

$$
\large{
S(j) ≡ 0 \; mod \; k , 1 = i < j \\
S(j) ≡ S(i - 1) \; mod \; k, 1 < i < j
}
$$
When doing iteration from $j = 1$ to $j = n$, we need to judge

- A. When $j > 1$ and $S(j) ≡ 0 \; mod \; k$, return true.

- B. Use a hash table (the key is $S(i) \; mod \; k$) to record THE FIRST $i$. If a same key appears twice (means $S(j) ≡ S(i) \; mod \; k$) and $j - i > 1$, return true. (At first I didn't notice that the size is at least 2, thanks to @BavariaKing1822 )

- C. After the iteration, return false.

c++ version

```c++
class Solution {
public:
    bool checkSubarraySum(vector<int>& nums, int k) {
        if(nums.size() < 2) return false;
        if(k == 0)
        {
            for(int i = 1; i < nums.size(); i++)
            {
                if(nums[i] == 0 && nums[i - 1] == 0) return true;
            }
            return false;
        }else{
            int i = 0;
            map<int, int> res;
            while(true)
            {
                if(i != 0 && nums[i] % k == 0)
                {
                    return true;
                }else{
                    if(res.find(nums[i] % k) == res.end())
                    { 
                         res[nums[i] % k] = i;
                    }else{
                         if(i - res[nums[i] % k] > 1) return true;
                    }
                }
                i++;
                if(i == nums.size()) return false;
                nums[i] += nums[i - 1];
            }
        }
    }
};
```

python version

```python
class Solution(object):
    def checkSubarraySum(self, nums, k):
        if k == 0:
            return any(nums[i] == 0 and nums[i + 1] == 0 for i in xrange(len(nums) - 1))
        mods, cum_sum_mod_k = {0: -1}, 0
        for i, n in enumerate(nums):
            cum_sum_mod_k = (cum_sum_mod_k + n) % k
            if cum_sum_mod_k in mods and i - mods[cum_sum_mod_k] > 1:
                return True
            if cum_sum_mod_k not in mods:
                mods[cum_sum_mod_k] = i
        return False
```

**Complexity analysis**

- Time complexity $O(n)$ : As Step 1 and Step 2 in Section 3 can be merged to a single iteration from $j = 1$ to $j = n$.
- Space complexity $O(|k|)$ : As the hash table's key is a remainder from division based on integer $k$, the probable maximum size of the hash table is $|k|$.



# 381.Insert Delete GetRandom O(1) - Duplicates allowed(H)

Design a data structure that supports all following operations in *average* **O(1)** time.

Note: Duplicate elements are allowed.





1. `insert(val)`: Inserts an item val to the collection.
2. `remove(val)`: Removes an item val from the collection if present.
3. `getRandom`: Returns a random element from current collection of elements. The probability of each element being returned is **linearly related** to the number of same value the collection contains.

**Example:**

```
// Init an empty collection.
RandomizedCollection collection = new RandomizedCollection();

// Inserts 1 to the collection. Returns true as the collection did not contain 1.
collection.insert(1);

// Inserts another 1 to the collection. Returns false as the collection contained 1. Collection now contains [1,1].
collection.insert(1);

// Inserts 2 to the collection, returns true. Collection now contains [1,1,2].
collection.insert(2);

// getRandom should return 1 with the probability 2/3, and returns 2 with the probability 1/3.
collection.getRandom();

// Removes 1 from the collection, returns true. Collection now contains [1,2].
collection.remove(1);

// getRandom should return 1 and 2 both equally likely.
collection.getRandom();
```







```python
import random
import collections
class RandomizedCollection(object):

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.l = []
        self.d = collections.defaultdict(set)

    def insert(self, val):
        """
        Inserts a value to the collection. Returns true if the collection did not already contain the specified element.
        :type val: int
        :rtype: bool
        """
        b = val not in self.d
        
        self.d[val].add(len(self.l))
        self.l.append(val)
        
        return b

    def remove(self, val):
        """
        Removes a value from the collection. Returns true if the collection contained the specified element.
        :type val: int
        :rtype: bool
        """
        if val not in self.d:
            return False
        
        index, newVal = self.d[val].pop(), self.l[-1]
        if len(self.d[val]) == 0:
            self.d.pop(val)
        
        self.l[index] = newVal
        
        if newVal in self.d:
            self.d[newVal].add(index)
            self.d[newVal].discard(len(self.l)-1)
        self.l.pop()
        return True
            
    def getRandom(self):
        """
        Get a random element from the collection.
        :rtype: int
        """
        return random.choice(self.l)
```







# 895.Maximum Frequency Stack(H)

Implement `FreqStack`, a class which simulates the operation of a stack-like data structure.

`FreqStack` has two functions:

- `push(int x)`, which pushes an integer `x` onto the stack.
- `pop()`, which removes and returns the most frequent element in the stack.
  - If there is a tie for most frequent element, the element closest to the top of the stack is removed and returned.

**Example 1:**

```
Input: 
["FreqStack","push","push","push","push","push","push","pop","pop","pop","pop"],
[[],[5],[7],[5],[7],[4],[5],[],[],[],[]]
Output: [null,null,null,null,null,null,null,5,7,5,4]
Explanation:
After making six .push operations, the stack is [5,7,5,7,4,5] from bottom to top.  Then:

pop() -> returns 5, as 5 is the most frequent.
The stack becomes [5,7,5,7,4].

pop() -> returns 7, as 5 and 7 is the most frequent, but 7 is closest to the top.
The stack becomes [5,7,5,4].

pop() -> returns 5.
The stack becomes [5,7,4].

pop() -> returns 4.
The stack becomes [5,7].
```

**Note:**

- Calls to `FreqStack.push(int x)` will be such that `0 <= x <= 10^9`.
- It is guaranteed that `FreqStack.pop()` won't be called if the stack has zero elements.
- The total number of `FreqStack.push` calls will not exceed `10000` in a single test case.
- The total number of `FreqStack.pop` calls will not exceed `10000` in a single test case.
- The total number of `FreqStack.push` and `FreqStack.pop` calls will not exceed `150000` across all test cases.

## 1: Stack of Stacks

**Intuition**

Evidently, we care about the frequency of an element. Let `freq` be a `Map` from x*x* to the number of occurrences of x*x*.

Also, we (probably) care about `maxfreq`, the current maximum frequency of any element in the stack. This is clear because we must pop the element with the maximum frequency.

The main question then becomes: among elements with the same (maximum) frequency, how do we know which element is most recent? We can use a stack to query this information: the top of the stack is the most recent.

To this end, let `group` be a map from frequency to a stack of elements with that frequency. We now have all the required components to implement `FreqStack`.

**Algorithm**

Actually, as an implementation level detail, if `x` has frequency `f`, then we'll have `x` in all `group[i] (i <= f)`, not just the top. This is because each `group[i]` will store information related to the `i`th copy of `x`.

Afterwards, our goal is just to maintain `freq`, `group`, and `maxfreq` as described above.

```python
class FreqStack(object):

    def __init__(self):
        self.freq = collections.Counter()
        self.group = collections.defaultdict(list)
        self.maxfreq = 0

    def push(self, x):
        f = self.freq[x] + 1
        self.freq[x] = f
        if f > self.maxfreq:
            self.maxfreq = f
        self.group[f].append(x)

    def pop(self):
        x = self.group[self.maxfreq].pop()
        self.freq[x] -= 1
        if not self.group[self.maxfreq]:
            self.maxfreq -= 1

        return x
```

**Complexity Analysis**

- Time Complexity: $O(1)$ for both `push` and `pop` operations.
- Space Complexity: $O(N)$, where `N` is the number of elements in the `FreqStack`. 

------

Analysis written by: [@awice](https://leetcode.com/awice).

## 2.another stack of stacks method:

```python
class FreqStack(object):

    def __init__(self):
        self.stacks = [[]]
        self.counts = {}

    def push(self, x):
        """
        :type x: int
        :rtype: void
        """
        if x in self.counts:
            self.counts[x] += 1
            pos = self.counts[x]
        else:
            self.counts[x] = 1
            pos = 1
        
        if len(self.stacks) < pos+1:
            self.stacks.append ([x])
        else:
            self.stacks[pos].append (x)

    def pop(self):
        """
        :rtype: int
        """
        if not self.stacks[-1]:
            self.stacks.pop()

        p = self.stacks[-1].pop()
        self.counts[p] -= 1
        
        return p
```

## 3.PriorityQueue

Storing `(count, index, number)` in min-heap and keeping map of counts. Since its a min-heap, I am negating the `count` and `index` while pushing in the heap.

The intuition is, heap will always keep the element with max count on top, and if two elements have same count, the second element (`index`) will be considered while doing `pop` operation. Also, the `count` map, is useful when the new occurence of the exisiting element is pushed.

```python
class FreqStack(object):

    def __init__(self):
        self.count = {}
        self.h = []
        self.index = 0

    def push(self, x):
        """
        :type x: int
        :rtype: None
        """
        self.index -= 1
        if x in self.count:
            self.count[x] -= 1
        else:
            self.count[x] = -1
            
        heappush(self.h, (self.count[x], self.index, x))
        # print(self.h)

    def pop(self):
        """
        :rtype: int
        """
        if len(self.h) > 0:
            t = heappop(self.h)
            self.count[t[2]] += 1
            return t[2]
```

ref:[Python Simple PriorityQueue](https://leetcode.com/problems/maximum-frequency-stack/discuss/163435/Python-Simple-PriorityQueue)



