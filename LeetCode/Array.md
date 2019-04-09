# 27.Remove Element（E）

Given an array *nums* and a value *val*, remove all instances of that value [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) and return the new length.

Do not allocate extra space for another array, you must do this by **modifying the input array in-place** with O(1) extra memory.

The order of elements can be changed. It doesn't matter what you leave beyond the new length.

**Example 1:**

```
Given nums = [3,2,2,3], val = 3,

Your function should return length = 2, with the first two elements of nums being 2.

It doesn't matter what you leave beyond the returned length.
```

**Example 2:**

```
Given nums = [0,1,2,2,3,0,4,2], val = 2,

Your function should return length = 5, with the first five elements of nums containing 0, 1, 3, 0, and 4.

Note that the order of those five elements can be arbitrary.

It doesn't matter what values are set beyond the returned length.
```

**Clarification:**

Confused why the returned value is an integer but your answer is an array?

Note that the input array is passed in by **reference**, which means modification to the input array will be known to the caller as well.

Internally you can think of this:

```
// nums is passed in by reference. (i.e., without making a copy)
int len = removeElement(nums, val);

// any modification to nums in your function would be known by the caller.
// using the length returned by your function, it prints the first len elements.
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```



This is a pretty easy problem, but one may get confused by the term "in-place" and thought it is impossible to remove an element from the array without making a copy of the array.

hits：

1. Try two pointers.
2. Did you use the property of "the order of elements can be changed"?
3. What happens when the elements to remove are rare?

## Approach 1: Two Pointers

**Intuition**

Since question asked us to remove all elements of the given value in-place, we have to handle it with $O(1)$ extra space. How to solve it? We can keep two pointers $i$ and $j$, where $i$ is the slow-runner while $j$ is the fast-runner.

**Algorithm**

When $nums[j]$ equals to the given value, skip this element by incrementing $j$. As long as $nums[j] \neq val$, we copy $nums[j]$ to $nums[i]$ and increment both indexes at the same time. Repeat the process until $j$ reaches the end of the array and the new length is $i$.

This solution is very similar to the solution to [Remove Duplicates from Sorted Array](https://leetcode.com/articles/remove-duplicates-from-sorted-array/).

```java
public int removeElement(int[] nums, int val) {
    int i = 0;
    for (int j = 0; j < nums.length; j++) {
        if (nums[j] != val) {
            nums[i] = nums[j];
            i++;
        }
    }
    return i;
}
```

**Complexity analysis**

- Time complexity : $O(n)$. Assume the array has a total of $n$ elements, both $i$ and $j$ traverse at most $2n$ steps.
- Space complexity : $O(1)$. 

## Approach 2: Two Pointers - when elements to remove are rare

**Intuition**

Now consider cases where the array contains few elements to remove. For example, nums = [1,2,3,5,4], val = 4. The previous algorithm will do unnecessary copy operation of the first four elements. Another example is $nums = [4,1,2,3,5], val = 4$. It seems unnecessary to move elements $[1,2,3,5]$ one step left as the problem description mentions that the order of elements could be changed.

**Algorithm**

When we encounter $nums[i] = val$, we can swap the current element out with the last element and dispose the last one. This essentially reduces the array's size by 1.

Note that the last element that was swapped in could be the value you want to remove itself. But don't worry, in the next iteration we will still check this element.

```java
public int removeElement(int[] nums, int val) {
    int i = 0;
    int n = nums.length;
    while (i < n) {
        if (nums[i] == val) {
            nums[i] = nums[n - 1];
            // reduce array size by one
            n--;
        } else {
            i++;
        }
    }
    return n;
}
```

**Complexity analysis**

- Time complexity : $O(n)$. Both i*i* and n*n* traverse at most n*n* steps. In this approach, the number of assignment operation is equal to the number of elements to remove. So it is more efficient if elements to remove are rare.
- Space complexity : $O(1)$.

# 121.Best Time to Buy and Sell Stock(E)

Say you have an array for which the $i^{th}$ element is the price of a given stock on day $i$.

If you were only permitted to complete at most one transaction (i.e., buy one and sell one share of the stock), design an algorithm to find the maximum profit.

Note that you cannot sell a stock before you buy one.

**Example 1:**

```
Input: [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
             Not 7-1 = 6, as selling price needs to be larger than buying price.
```

**Example 2:**

```
Input: [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.
```

We need to find out the maximum difference (which will be the maximum profit) between two numbers in the given array. Also, the second number (selling price) must be larger than the first one (buying price).

In formal terms, we need to find $\max(prices[j] - prices[i])$, for every $i$ and j*j* such that $j > i$.

## 1: Brute Force

```python
public class Solution {
    public int maxProfit(int prices[]) {
        int maxprofit = 0;
        for (int i = 0; i < prices.length - 1; i++) {
            for (int j = i + 1; j < prices.length; j++) {
                int profit = prices[j] - prices[i];
                if (profit > maxprofit)
                    maxprofit = profit;
            }
        }
        return maxprofit;
    }
}
```

**Complexity Analysis**

- Time complexity : $O(n^2)$. Loop runs $\dfrac{n (n-1)}{2}$ times.
- Space complexity : $O(1)$. Only two variables - $\text{maxprofit}$ and $\text{profit}$ are used. 

## 2: One Pass

**Algorithm**

Say the given array is:

[7, 1, 5, 3, 6, 4]

If we plot the numbers of the given array on a graph, we get:

![Profit Graph](https://leetcode.com/media/original_images/121_profit_graph.png)

The points of interest are the peaks and valleys in the given graph. We need to find the largest peak following the smallest valley. We can maintain two variables - minprice and maxprofit corresponding to the smallest valley and maximum profit (maximum difference between selling price and minprice) obtained so far respectively.

```python
public class Solution {
    public int maxProfit(int prices[]) {
        int minprice = Integer.MAX_VALUE;
        int maxprofit = 0;
        for (int i = 0; i < prices.length; i++) {
            if (prices[i] < minprice)
                minprice = prices[i];
            else if (prices[i] - minprice > maxprofit)
                maxprofit = prices[i] - minprice;
        }
        return maxprofit;
    }
}
```

**Complexity Analysis**

- Time complexity : $O(n)$. Only a single pass is needed.
- Space complexity : $O(1)$. Only two variables are used.

## 3.DP

ref: [DP solution & some thoughts](https://leetcode.com/problems/maximum-subarray/discuss/20193/DP-solution-and-some-thoughts)

First， we should change the input as the difference between two adjacent numbers:

```
input: [7,1,5,3,6,4]
change as:[0, -6, 4, -2, 3, -2]
```

now we can use dp to solute the problem.

Analysis of this problem:
Apparently, this is a optimization problem, which can be usually solved by DP. So when it comes to DP, the first thing for us to figure out is the format of the sub problem(or the state of each sub problem). The format of the sub problem can be helpful when we are trying to come up with the recursive relation.



At first, I think the sub problem should look like: `maxSubArray(int A[], int i, int j)`, which means the maxSubArray for A[i: j]. In this way, our goal is to figure out what `maxSubArray(A, 0, A.length - 1)` is. However, if we define the format of the sub problem in this way, it's hard to find the connection from the sub problem to the original problem(at least for me). In other words, I can't find a way to divided the original problem into the sub problems and use the solutions of the sub problems to somehow create the solution of the original one.



So I change the format of the sub problem into something like: `maxSubArray(int A[], int i)`, which means the maxSubArray for A[0:i ] which must has A[i] as the end element. Note that now the sub problem's format is less flexible and less powerful than the previous one because there's a limitation that A[i] should be contained in that sequence and we have to keep track of each solution of the sub problem to update the global optimal value. However, now the connect between the sub problem & the original one becomes clearer:

```
maxSubArray(A, i) = maxSubArray(A, i - 1) > 0 ? maxSubArray(A, i - 1) : 0 + A[i]; 
```

And here's the code

```java
public int maxSubArray(int[] A) {
        int n = A.length;
        int[] dp = new int[n];//dp[i] means the maximum subarray ending with A[i];
        dp[0] = A[0];
        int max = dp[0];
        
        for(int i = 1; i < n; i++){
            dp[i] = A[i] + (dp[i - 1] > 0 ? dp[i - 1] : 0);
            max = Math.max(max, dp[i]);
        }
        
        return max;
}
```

or like this:[Easy Python Way](https://leetcode.com/problems/maximum-subarray/discuss/20396/Easy-Python-Way)

```python
for i in range(1, len(nums)):
        if nums[i-1] > 0:
            nums[i] += nums[i-1]
    return max(nums)
```



# 283.Move Zeroes（E）

tag: `Array`、`Two Points`

Given an array `nums`, write a function to move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.

**Example:**

```
Input: [0,1,0,3,12]
Output: [1,3,12,0,0]
```

**Note**:

1. You must do this **in-place** without making a copy of the array.
2. Minimize the total number of operations.



This question comes under a broad category of "Array Transformation". This category is the meat of tech interviews. Mostly because arrays are such a simple and easy to use data structure. Traversal or representation doesn't require any boilerplate code and most of your code will look like the Pseudocode itself.

The 2 requirements of the question are:

1. Move all the 0's to the end of array.
2. All the non-zero elements must retain their original order.

It's good to realize here that both the requirements are mutually exclusive, i.e., you can solve the individual sub-problems and then combine them for the final solution.

## Approach #1 (Space Sub-Optimal) [Accepted]

```c++
void moveZeroes(vector<int>& nums) {
    int n = nums.size();

    // Count the zeroes
    int numZeroes = 0;
    for (int i = 0; i < n; i++) {
        numZeroes += (nums[i] == 0);
    }

    // Make all the non-zero elements retain their original order.
    vector<int> ans;
    for (int i = 0; i < n; i++) {
        if (nums[i] != 0) {
            ans.push_back(nums[i]);
        }
    }

    // Move all zeroes to the end
    while (numZeroes--) {
        ans.push_back(0);
    }

    // Combine the result
    for (int i = 0; i < n; i++) {
        nums[i] = ans[i];
    }
}
```

**Complexity Analysis**

Space Complexity : $O(n)$. Since we are creating the "ans" array to store results.

Time Complexity: $O(n)​$. However, the total number of operations are sub-optimal. We can achieve the same result in less number of operations.

If asked in an interview, the above solution would be a good start. You can explain the interviewer(not code) the above and build your base for the next Optimal Solution.

## Approach #2 (Space Optimal, Operation Sub-Optimal) [Accepted]

This approach works the same way as above, i.e. , first fulfills one requirement and then another. The catch? It does it in a clever way. The above problem can also be stated in alternate way, " Bring all the non 0 elements to the front of array keeping their relative order same".

This is a 2 pointer approach. The fast pointer which is denoted by variable "cur" does the job of processing new elements. If the newly found element is not a 0, we record it just after the last found non-0 element. The position of last found non-0 element is denoted by the slow pointer "lastNonZeroFoundAt" variable. As we keep finding new non-0 elements, we just overwrite them at the "lastNonZeroFoundAt + 1" 'th index. This overwrite will not result in any loss of data because we already processed what was there(if it were non-0,it already is now written at it's corresponding index,or if it were 0 it will be handled later in time).

After the "cur" index reaches the end of array, we now know that all the non-0 elements have been moved to beginning of array in their original order. Now comes the time to fulfil other requirement, "Move all 0's to the end". We now simply need to fill all the indexes after the "lastNonZeroFoundAt" index with 0.

```c++
void moveZeroes(vector<int>& nums) {
    int lastNonZeroFoundAt = 0;
    // If the current element is not 0, then we need to
    // append it just in front of last non 0 element we found. 
    for (int i = 0; i < nums.size(); i++) {
        if (nums[i] != 0) {
            nums[lastNonZeroFoundAt++] = nums[i];
        }
    }
    // After we have finished processing new elements,
    // all the non-zero elements are already at beginning of array.
    // We just need to fill remaining array with 0's.
    for (int i = lastNonZeroFoundAt; i < nums.size(); i++) {
        nums[i] = 0;
    }
}
```

**Complexity Analysis**

Space Complexity : $O(1)$. Only constant space is used.

Time Complexity: $O(n)$. However, the total number of operations are still sub-optimal. The total operations (array writes) that code does is $n$ (Total number of elements).

## Approach #3 (Optimal) [Accepted]

The total number of operations of the previous approach is sub-optimal. For example, the array which has all (except last) leading zeroes: [0, 0, 0, ..., 0, 1].How many write operations to the array? For the previous approach, it writes 0's $n-1​$ times, which is not necessary. We could have instead written just once. How? ..... By only fixing the non-0 element,i.e., 1.

The optimal approach is again a subtle extension of above solution. A simple realization is if the current element is non-0, its' correct position can at best be it's current position or a position earlier. If it's the latter one, the current position will be eventually occupied by a non-0 ,or a 0, which lies at a index greater than 'cur' index. We fill the current position by 0 right away,so that unlike the previous solution, we don't need to come back here in next iteration.

In other words, the code will maintain the following invariant:

> 1. All elements before the slow pointer (lastNonZeroFoundAt) are non-zeroes.
> 2. All elements between the current and slow pointer are zeroes.

Therefore, when we encounter a non-zero element, we need to swap elements pointed by current and slow pointer, then advance both pointers. If it's zero element, we just advance current pointer.

With this invariant in-place, it's easy to see that the algorithm will work.

```c++
void moveZeroes(vector<int>& nums) {
    for (int lastNonZeroFoundAt = 0, cur = 0; cur < nums.size(); cur++) {
        if (nums[cur] != 0) {
            swap(nums[lastNonZeroFoundAt++], nums[cur]);
        }
    }
}
```

**Complexity Analysis**

Space Complexity : $O(1)$. Only constant space is used.

Time Complexity: $O(n)$. However, the total number of operations are optimal. The total operations (array writes) that code does is Number of non-0 elements.This gives us a much better best-case (when most of the elements are 0) complexity than last solution. However, the worst-case (when all elements are non-0) complexity for both the algorithms is same.

Analysis written by: @spandan.pathak

# 169.Majority Element（E）

Given an array of size *n*, find the majority element. The majority element is the element that appears **more than** `⌊ n/2 ⌋` times.

You may assume that the array is non-empty and the majority element always exist in the array.

**Example 1:**

```
Input: [3,2,3]
Output: 3
```

**Example 2:**

```
Input: [2,2,1,1,1,2,2]
Output: 2
```

## Approach 1: Brute Force

**Intuition**

We can exhaust the search space in quadratic time by checking whether each element is the majority element.

**Algorithm**

The brute force algorithm iterates over the array, and then iterates again for each number to count its occurrences. As soon as a number is found to have appeared more than any other can possibly have appeared, return it.

```python
class Solution:
    def majorityElement(self, nums):
        majority_count = len(nums)//2
        for num in nums:
            count = sum(1 for elem in nums if elem == num)
            if count > majority_count:
                return num
```

**Complexity Analysis**

- Time complexity : $O(n^2)$

  The brute force algorithm contains two nested `for` loops that each run for n*n* iterations, adding up to quadratic time complexity.

- Space complexity : $O(1)​$

  The brute force solution does not allocate additional space proportional to the input size.

## Approach 2: HashMap

**Intuition**

We know that the majority element occurs more than $\lfloor \dfrac{n}{2} \rfloor$ times, and a `HashMap` allows us to count element occurrences efficiently.

**Algorithm**

We can use a `HashMap` that maps elements to counts in order to count occurrences in linear time by looping over `nums`. Then, we simply return the key with maximum value.

```python
class Solution:
    def majorityElement(self, nums):
        counts = collections.Counter(nums)
        return max(counts.keys(), key=counts.get)
```

**Complexity Analysis**

- Time complexity : $O(n)$

  We iterate over `nums` once and make a constant time `HashMap` insertion on each iteration. Therefore, the algorithm runs in $O(n)​$ time.

- Space complexity : $O(n)​$

  At most, the `HashMap` can contain $n - \lfloor \dfrac{n}{2} \rfloor$ associations, so it occupies $O(n)$ space. This is because an arbitrary array of length $n$ can contain n distinct values, but `nums` is guaranteed to contain a majority element, which will occupy (at minimum) $\lfloor \dfrac{n}{2} \rfloor + 1$ array indices. Therefore, $n - (\lfloor \dfrac{n}{2} \rfloor + 1)$ indices can be occupied by distinct, non-majority elements (plus 1 for the majority element itself), leaving us with (at most) $n - \lfloor \dfrac{n}{2} \rfloor$ distinct elements.

## Approach 3: Sorting

**Intuition**

If the elements are sorted in monotonically increasing (or decreasing) order, the majority element can be found at index $\lfloor \dfrac{n}{2} \rfloor$ (and $\lfloor \dfrac{n}{2} \rfloor + 1$, incidentally, if n is even).

**Algorithm**

For this algorithm, we simply do exactly what is described: sort `nums`, and return the element in question. To see why this will always return the majority element (given that the array has one), consider the figure below (the top example is for an odd-length array and the bottom is for an even-length array):

![Sorting middle index overlap](https://leetcode.com/articles/Figures/169/sorting.png)

For each example, the line below the array denotes the range of indices that are covered by a majority element that happens to be the array minimum. As you might expect, the line above the array is similar, but for the case where the majority element is also the array maximum. In all other cases, this line will lie somewhere between these two, but notice that even in these two most extreme cases, they overlap at index $\lfloor \dfrac{n}{2} \rfloor$ for both even- and odd-length arrays. Therefore, no matter what value the majority element has in relation to the rest of the array, returning the value at $\lfloor \dfrac{n}{2} \rfloor$ will never be wrong.

```python
class Solution:
    def majorityElement(self, nums):
        nums.sort()
        return nums[len(nums)//2]
```

**Complexity Analysis**

- Time complexity : $O(nlgn)​$

  Sorting the array costs $O(nlgn)$ time in Python and Java, so it dominates the overall runtime.

- Space complexity : $O(1)$ or ($O(n)​$)

  We sorted `nums` in place here - if that is not allowed, then we must spend linear additional space on a copy of `nums` and sort the copy instead.

## Approach 4: Randomization

**Intuition**

Because more than $\lfloor \dfrac{n}{2} \rfloor$ array indices are occupied by the majority element, a random array index is likely to contain the majority element.

**Algorithm**

Because a given index is likely to have the majority element, we can just select a random index, check whether its value is the majority element, return if it is, and repeat if it is not. The algorithm is verifiably correct because we ensure that the randomly chosen value is the majority element before ever returning.

```python
import random

class Solution:
    def majorityElement(self, nums):
        majority_count = len(nums)//2
        while True:
            candidate = random.choice(nums)
            if sum(1 for elem in nums if elem == candidate) > majority_count:
                return candidate
```

**Complexity Analysis**

- Time complexity : $O(\infty)​$

  It is technically possible for this algorithm to run indefinitely (if we never manage to randomly select the majority element), so the worst possible runtime is unbounded. However, the expected runtime is far better - linear, in fact. For ease of analysis, convince yourself that because the majority element is guaranteed to occupy *more* than half of the array, the expected number of iterations will be less than it would be if the element we sought occupied exactly *half* of the array. Therefore, we can calculate the expected number of iterations for this modified version of the problem and assert that our version is easier.
  $$
  \begin{aligned} EV(iters_{prob}) &\leq EV(iters_{mod}) \\ &= \lim_{n\to\infty} \sum_{i=1}^{n} i \cdot \frac{1}{2^i} \\ &= 2 \end{aligned}​
  $$
  Because the series converges, the expected number of iterations for the modified problem is constant. Based on an expected-constant number of iterations in which we perform linear work, the expected runtime is linear for the modifed problem. Therefore, the expected runtime for our problem is also linear, as the runtime of the modifed problem serves as an upper bound for it.

- Space complexity : $O(1)$

  Much like the brute force solution, the randomized approach runs with constant additional space.

#### Approach 5: Divide and Conquer

**Intuition**

If we know the majority element in the left and right halves of an array, we can determine which is the global majority element in linear time.

**Algorithm**

Here, we apply a classical divide & conquer approach that recurses on the left and right halves of an array until an answer can be trivially achieved for a length-1 array. Note that because actually passing copies of subarrays costs time and space, we instead pass `lo` and `hi` indices that describe the relevant slice of the overall array. In this case, the majority element for a length-1 slice is trivially its only element, so the recursion stops there. If the current slice is longer than length-1, we must combine the answers for the slice's left and right halves. If they agree on the majority element, then the majority element for the overall slice is obviously the same[1](https://leetcode.com/articles/majority-element/#fn:1). If they disagree, only one of them can be "right", so we need to count the occurrences of the left and right majority elements to determine which subslice's answer is globally correct. The overall answer for the array is thus the majority element between indices 0 and n*n*.

```python
class Solution:
    def majorityElement(self, nums, lo=0, hi=None):
        def majority_element_rec(lo, hi):
            # base case; the only element in an array of size 1 is the majority
            # element.
            if lo == hi:
                return nums[lo]

            # recurse on left and right halves of this slice.
            mid = (hi-lo)//2 + lo
            left = majority_element_rec(lo, mid)
            right = majority_element_rec(mid+1, hi)

            # if the two halves agree on the majority element, return it.
            if left == right:
                return left

            # otherwise, count each element and return the "winner".
            left_count = sum(1 for i in range(lo, hi+1) if nums[i] == left)
            right_count = sum(1 for i in range(lo, hi+1) if nums[i] == right)

            return left if left_count > right_count else right

        return majority_element_rec(0, len(nums)-1)
```

**Complexity Analysis**

- Time complexity : $O(nlgn)$

  Each recursive call to `majority_element_rec` performs two recursive calls on subslices of size $\frac{n}{2}$and two linear scans of length $n$. Therefore, the time complexity of the divide & conquer approach can be represented by the following recurrence relation:
  $$
  T(n) = 2T(\frac{n}{2}) + 2n
  $$
  By the [master theorem](https://en.wikipedia.org/wiki/Master_theorem_(analysis_of_algorithms)), the recurrence satisfies case 2, so the complexity can be analyzed as such:
  $$
  \begin{aligned} T(n) &= \Theta(n^{log_{b}a}\log n) \\ &= \Theta(n^{log_{2}2}\log n) \\ &= \Theta(n \log n) \\ \end{aligned}
  $$

- Space complexity : $O(lgn)$

  Although the divide & conquer does not explicitly allocate any additional memory, it uses a non-constant amount of additional memory in stack frames due to recursion. Because the algorithm "cuts" the array in half at each level of recursion, it follows that there can only be $O(lgn)$ "cuts" before the base case of 1 is reached. It follows from this fact that the resulting recursion tree is balanced, and therefore all paths from the root to a leaf are of length $O(lgn)$. Because the recursion tree is traversed in a depth-first manner, the space complexity is therefore equivalent to the length of the longest path, which is, of course, $O(lgn)$.

## Approach 6: Boyer-Moore Voting Algorithm

**Intuition**

If we had some way of counting instances of the majority element as +1+1 and instances of any other element as -1−1, summing them would make it obvious that the majority element is indeed the majority element.

**Algorithm**

Essentially, what Boyer-Moore does is look for a suffix suf*s**u**f* of `nums` where suf[0]*s**u**f*[0] is the majority element in that suffix. To do this, we maintain a count, which is incremented whenever we see an instance of our current candidate for majority element and decremented whenever we see anything else. Whenever `count`equals 0, we effectively forget about everything in `nums` up to the current index and consider the current number as the candidate for majority element. It is not immediately obvious why we can get away with forgetting prefixes of `nums` - consider the following examples (pipes are inserted to separate runs of nonzero `count`).

[7, 7, 5, 7, 5, 1 | 5, 7 | 5, 5, 7, 7 | 7, 7, 7, 7]

Here, the `7` at index 0 is selected to be the first candidate for majority element. `count` will eventually reach 0 after index 5 is processed, so the `5` at index 6 will be the next candidate. In this case, `7` is the true majority element, so by disregarding this prefix, we are ignoring an equal number of majority and minority elements - therefore, `7` will still be the majority element in the suffix formed by throwing away the first prefix.

[7, 7, 5, 7, 5, 1 | 5, 7 | 5, 5, 7, 7 | **5, 5, 5, 5**]

Now, the majority element is `5` (we changed the last run of the array from `7`s to `5`s), but our first candidate is still `7`. In this case, our candidate is not the true majority element, but we still cannot discard more majority elements than minority elements (this would imply that `count` could reach -1 before we reassign `candidate`, which is obviously false).

Therefore, given that it is impossible (in both cases) to discard more majority elements than minority elements, we are safe in discarding the prefix and attempting to recursively solve the majority element problem for the suffix. Eventually, a suffix will be found for which `count` does not hit `0`, and the majority element of that suffix will necessarily be the same as the majority element of the overall array.

```python
class Solution:
    def majorityElement(self, nums):
        count = 0
        candidate = None

        for num in nums:
            if count == 0:
                candidate = num
            count += (1 if num == candidate else -1)

        return candidate
```

**Complexity Analysis**

- Time complexity : $O(n)$

  Boyer-Moore performs constant work exactly n*n* times, so the algorithm runs in linear time.

- Space complexity : $O(1)$

  Boyer-Moore allocates only constant additional memory.

**Footnotes**

------

1. This is a constant optimization that could be excluded without hurting our overall runtime.



# 724.Find Pivot Index(E)

Given an array of integers `nums`, write a method that returns the "pivot" index of this array.

We define the pivot index as the index where the sum of the numbers to the left of the index is equal to the sum of the numbers to the right of the index.

If no such index exists, we should return -1. If there are multiple pivot indexes, you should return the left-most pivot index.

**Example 1:**

```
Input: 
nums = [1, 7, 3, 6, 5, 6]
Output: 3
Explanation: 
The sum of the numbers to the left of index 3 (nums[3] = 6) is equal to the sum of numbers to the right of index 3.
Also, 3 is the first index where this occurs.
```

**Example 2:**

```
Input: 
nums = [1, 2, 3]
Output: -1
Explanation: 
There is no index that satisfies the conditions in the problem statement.
```

**Note:**

- The length of `nums` will be in the range `[0, 10000]`.
- Each element `nums[i]` will be an integer in the range `[-1000, 1000]`.

## Prefix Sum

**Intuition and Algorithm**

We need to quickly compute the sum of values to the left and the right of every index.

Let's say we knew `S` as the sum of the numbers, and we are at index `i`. If we knew the sum of numbers `leftsum` that are to the left of index `i`, then the other sum to the right of the index would just be `S - nums[i] - leftsum`.

As such, we only need to know about `leftsum` to check whether an index is a pivot index in constant time. Let's do that: as we iterate through candidate indexes `i`, we will maintain the correct value of `leftsum`.

```python
class Solution(object):
    def pivotIndex(self, nums):
        S = sum(nums)
        leftsum = 0
        for i, x in enumerate(nums):
            if leftsum == (S - leftsum - x):
                return i
            leftsum += x
        return -1
```

**Complexity Analysis**

- Time Complexity: $O(N)$, where N*N* is the length of `nums`.
- Space Complexity: $O(1)$, the space used by `leftsum` and `S`.

Analysis written by: [@awice](https://leetcode.com/awice).

# 747.Largest Number At Least Twice of Others(E)

In a given integer array `nums`, there is always exactly one largest element.

Find whether the largest element in the array is at least twice as much as every other number in the array.

If it is, return the **index** of the largest element, otherwise return -1.

**Example 1:**

```
Input: nums = [3, 6, 1, 0]
Output: 1
Explanation: 6 is the largest integer, and for every other number in the array x,
6 is more than twice as big as x.  The index of value 6 is 1, so we return 1.
```

**Example 2:**

```
Input: nums = [1, 2, 3, 4]
Output: -1
Explanation: 4 isn't at least as big as twice the value of 3, so we return -1.
```

**Note:**

1. `nums` will have a length in the range `[1, 50]`.
2. Every `nums[i]` will be an integer in the range `[0, 99]`.

## Linear Scan

**Intuition and Algorithm**

Scan through the array to find the unique largest element `m`, keeping track of it's index `maxIndex`.

Scan through the array again. If we find some `x != m` with `m < 2*x`, we should return `-1`.

Otherwise, we should return `maxIndex`.

```python
class Solution(object):
    def dominantIndex(self, nums):
        m = max(nums)
        if all(m >= 2*x for x in nums if x != m):
            return nums.index(m)
        return -1
```

**Complexity Analysis**

- Time Complexity: $O(N)$ where N*N* is the length of `nums`.
- Space Complexity: $O(1)$, the space used by our `int` variables.

Analysis written by: [@awice](https://leetcode.com/awice).

# 905.Sort Array By Parity(E)

tag: `sort`/`two point`

Given an array `A` of non-negative integers, return an array consisting of all the even elements of `A`, followed by all the odd elements of `A`.

You may return any answer array that satisfies this condition.

**Example 1:**

```
Input: [3,1,2,4]
Output: [2,4,3,1]
The outputs [4,2,3,1], [2,4,1,3], and [4,2,1,3] would also be accepted.
```

**Note:**

1. `1 <= A.length <= 5000`
2. `0 <= A[i] <= 5000`

## 1: Sort

**Intuition and Algorithm**

Use a custom comparator when sorting, to sort by parity.

```python
class Solution(object):
    def sortArrayByParity(self, A):
        A.sort(key = lambda x: x % 2)
        return A
```

**Complexity Analysis**

- Time Complexity: $O(N \log N)$, where N*N* is the length of `A`.
- Space Complexity: $O(N)$ for the sort, depending on the built-in implementation of `sort`. 

## 2: Two Pass

**Intuition and Algorithm**

Write all the even elements first, then write all the odd elements.

```python
class Solution(object):
    def sortArrayByParity(self, A):
        return ([x for x in A if x % 2 == 0] +
                [x for x in A if x % 2 == 1])
```

**Complexity Analysis**

- Time Complexity: $O(N)$, where N*N* is the length of `A`.
- Space Complexity: $O(N)$, the space used by the answer. 

## 3: In-Place

**Intuition**

If we want to do the sort in-place, we can use quicksort, a standard textbook algorithm.

**Algorithm**

We'll maintain two pointers `i` and `j`. The loop invariant is everything below `i` has parity `0` (ie. `A[k] % 2 == 0` when `k < i`), and everything above `j` has parity `1`.

Then, there are 4 cases for `(A[i] % 2, A[j] % 2)`:

- If it is `(0, 1)`, then everything is correct: `i++` and `j--`.
- If it is `(1, 0)`, we swap them so they are correct, then continue.
- If it is `(0, 0)`, only the `i` place is correct, so we `i++` and continue.
- If it is `(1, 1)`, only the `j` place is correct, so we `j--` and continue.

Throughout all 4 cases, the loop invariant is maintained, and `j-i` is getting smaller. So eventually we will be done with the array sorted as desired.

```python
class Solution(object):
    def sortArrayByParity(self, A):
        i, j = 0, len(A) - 1
        while i < j:
            if A[i] % 2 > A[j] % 2:
                A[i], A[j] = A[j], A[i]

            if A[i] % 2 == 0: i += 1
            if A[j] % 2 == 1: j -= 1

        return A
```

**Complexity Analysis**

- Time Complexity: $O(N)$, where N*N* is the length of `A`. Each step of the while loop makes `j-i`decrease by at least one. (Note that while quicksort is $O(N \log N)$ normally, this is $O(N)$ because we only need one pass to sort the elements.)
- Space Complexity: $O(1)​$ in additional space complexity. 

Analysis written by: [@awice](https://leetcode.com/awice).

# 922.Sort Array by Parity II(E)

tag: `two point`

Given an array `A` of non-negative integers, half of the integers in A are odd, and half of the integers are even.

Sort the array so that whenever `A[i]` is odd, `i` is odd; and whenever `A[i]` is even, `i` is even.

You may return any answer array that satisfies this condition.

**Example 1:**

```
Input: [4,2,5,7]
Output: [4,5,2,7]
Explanation: [4,7,2,5], [2,5,4,7], [2,7,4,5] would also have been accepted.
```

**Note:**

1. `2 <= A.length <= 20000`
2. `A.length % 2 == 0`
3. `0 <= A[i] <= 1000`

## 1: Two Pass

**Intuition and Algorithm**

Read all the even integers and put them into places `ans[0]`, `ans[2]`, `ans[4]`, and so on.

Then, read all the odd integers and put them into places `ans[1]`, `ans[3]`, `ans[5]`, etc.

```python
class Solution(object):
    def sortArrayByParityII(self, A):
        N = len(A)
        ans = [None] * N

        t = 0
        for i, x in enumerate(A):
            if x % 2 == 0:
                ans[t] = x
                t += 2

        t = 1
        for i, x in enumerate(A):
            if x % 2 == 1:
                ans[t] = x
                t += 2

        # We could have also used slice assignment:
        # ans[::2] = (x for x in A if x % 2 == 0)
        # ans[1::2] = (x for x in A if x % 2 == 1)

        return ans
```

**Complexity Analysis**

- Time Complexity: $O(N),$ where N*N* is the length of `A`.
- Space Complexity: $O(N)$. 

## 2: Read / Write Heads

**Intuition**

We are motivated (perhaps by the interviewer) to pursue a solution where we modify the original array `A` in place.

First, it is enough to put all even elements in the correct place, since all odd elements will be in the correct place too. So let's only focus on `A[0], A[2], A[4], ...`

Ideally, we would like to have some partition where everything to the left is already correct, and everything to the right is undecided.

Indeed, this idea works if we separate it into two slices `even = A[0], A[2], A[4], ...` and `odd = A[1], A[3], A[5], ...`. Our invariant will be that everything less than `i` in the even slice is correct, and everything less than `j` in the odd slice is correct.

**Algorithm**

For each even `i`, let's make `A[i]` even. To do it, we will draft an element from the odd slice. We pass `j`through the odd slice until we find an even element, then swap. Our invariant is maintained, so the algorithm is correct.

```python
class Solution(object):
    def sortArrayByParityII(self, A):
        j = 1
        for i in xrange(0, len(A), 2):
            if A[i] % 2:
                while A[j] % 2:
                    j += 2
                A[i], A[j] = A[j], A[i]
        return A
```

**Complexity Analysis**

- Time Complexity: $O(N)$, where N*N* is the length of `A`.
- Space Complexity: $O(1)$. 

------

Analysis written by: [@awice](https://leetcode.com/awice).





# 977.Squares of a Sorted Array(E)

tag:`sort`/`two point`

Given an array of integers `A` sorted in non-decreasing order, return an array of the squares of each number, also in sorted non-decreasing order.

**Example 1:**

```
Input: [-4,-1,0,3,10]
Output: [0,1,9,16,100]
```

**Example 2:**

```
Input: [-7,-3,2,3,11]
Output: [4,9,9,49,121]
```

 **Note:**

1. `1 <= A.length <= 10000`
2. `-10000 <= A[i] <= 10000`
3. `A` is sorted in non-decreasing order.

## 1: Sort

**Intuition and Algorithm**

Create an array of the squares of each element, and sort them.

```python
class Solution(object):
    def sortedSquares(self, A):
        return sorted(x*x for x in A)
```

**Complexity Analysis**

- Time Complexity: $O(N \log N)$, where N*N* is the length of `A`.
- Space Complexity: $O(N)$. 

## 2:Two Pointer

**Intuition**

Since the array `A` is sorted, loosely speaking it has some negative elements with squares in decreasing order, then some non-negative elements with squares in increasing order.

For example, with `[-3, -2, -1, 4, 5, 6]`, we have the negative part `[-3, -2, -1]` with squares `[9, 4, 1]`, and the positive part `[4, 5, 6]` with squares `[16, 25, 36]`. Our strategy is to iterate over the negative part in reverse, and the positive part in the forward direction.

**Algorithm**

We can use two pointers to read the positive and negative parts of the array - one pointer `j` in the positive direction, and another `i` in the negative direction.

Now that we are reading two increasing arrays (the squares of the elements), we can merge these arrays together using a two-pointer technique.

```python
class Solution(object):
    def sortedSquares(self, A):
        N = len(A)
        # i, j: negative, positive parts
        j = 0
        while j < N and A[j] < 0:
            j += 1
        i = j - 1

        ans = []
        while 0 <= i and j < N:
            if A[i]**2 < A[j]**2:
                ans.append(A[i]**2)
                i -= 1
            else:
                ans.append(A[j]**2)
                j += 1

        while i >= 0:
            ans.append(A[i]**2)
            i -= 1
        while j < N:
            ans.append(A[j]**2)
            j += 1

        return ans
```

**Complexity Analysis**

- Time Complexity: $O(N)$, where N*N* is the length of `A`.
- Space Complexity: $O(N)​$. 

------

Analysis written by: [@awice](https://leetcode.com/awice).

# 985.Sum of Even Numbers After Queries(E)

tag: `constant`

We have an array `A` of integers, and an array `queries` of queries.

For the `i`-th query `val = queries[i][0], index = queries[i][1]`, we add val to `A[index]`.  Then, the answer to the `i`-th query is the sum of the even values of `A`.

*(Here, the given index = queries[i][1] is a 0-based index, and each query permanently modifies the array A.)*

Return the answer to all queries.  Your `answer` array should have `answer[i]` as the answer to the `i`-th query.

**Example 1:**

```
Input: A = [1,2,3,4], queries = [[1,0],[-3,1],[-4,0],[2,3]]
Output: [8,6,2,4]
Explanation: 
At the beginning, the array is [1,2,3,4].
After adding 1 to A[0], the array is [2,2,3,4], and the sum of even values is 2 + 2 + 4 = 8.
After adding -3 to A[1], the array is [2,-1,3,4], and the sum of even values is 2 + 4 = 6.
After adding -4 to A[0], the array is [-2,-1,3,4], and the sum of even values is -2 + 4 = 2.
After adding 2 to A[3], the array is [-2,-1,3,6], and the sum of even values is -2 + 6 = 4.
```

**Note:**

1. `1 <= A.length <= 10000`
2. `-10000 <= A[i] <= 10000`
3. `1 <= queries.length <= 10000`
4. `-10000 <= queries[i][0] <= 10000`
5. `0 <= queries[i][1] < A.length`

## 1: Maintain Array Sum

**Intuition and Algorithm**

Let's try to maintain `S`, the sum of the array throughout one query operation.

When acting on an array element `A[index]`, the rest of the values of `A` remain the same. Let's remove `A[index]` from `S` if it is even, then add `A[index] + val` back (if it is even.)

Here are some examples:

- If we have `A = [2,2,2,2,2]`, `S = 10`, and we do `A[0] += 4`: we will update `S -= 2`, then `S += 6`. At the end, we will have `A = [6,2,2,2,2]` and `S = 14`.
- If we have `A = [1,2,2,2,2]`, `S = 8`, and we do `A[0] += 3`: we will skip updating `S` (since `A[0]`is odd), then `S += 4`. At the end, we will have `A = [4,2,2,2,2]` and `S = 12`.
- If we have `A = [2,2,2,2,2]`, `S = 10` and we do `A[0] += 1`: we will update `S -= 2`, then skip updating `S` (since `A[0] + 1` is odd.) At the end, we will have `A = [3,2,2,2,2]` and `S = 8`.
- If we have `A = [1,2,2,2,2]`, `S = 8` and we do `A[0] += 2`: we will skip updating `S` (since `A[0]`is odd), then skip updating `S` again (since `A[0] + 2` is odd.) At the end, we will have `A = [3,2,2,2,2]` and `S = 8`.

These examples help illustrate that our algorithm actually maintains the value of `S` throughout each query operation.

```python
class Solution(object):
    def sumEvenAfterQueries(self, A, queries):
        S = sum(x for x in A if x % 2 == 0)
        ans = []

        for x, k in queries:
            if A[k] % 2 == 0: S -= A[k]
            A[k] += x
            if A[k] % 2 == 0: S += A[k]
            ans.append(S)

        return ans
```

**Complexity Analysis**

- Time Complexity: $O(N+Q)$, where $N$ is the length of `A` and $Q$ is the number of `queries`.
- Space Complexity: $O(Q)$, though we only allocate $O(1)​$ additional space. 

------

Analysis written by: [@awice](https://leetcode.com/awice).



# 54.Spiral Matrix(M)

Given a matrix of *m* x *n* elements (*m* rows, *n* columns), return all elements of the matrix in spiral order.

**Example 1:**

```
Input:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
Output: [1,2,3,6,9,8,7,4,5]
```

**Example 2:**

```
Input:
[
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9,10,11,12]
]
Output: [1,2,3,4,8,12,11,10,9,5,6,7]
```

## 1: Simulation

**Intuition**

Draw the path that the spiral makes. We know that the path should turn clockwise whenever it would go out of bounds or into a cell that was previously visited.

**Algorithm**

Let the array have $R$ rows and $C$ columns. $\text{seen[r][c]}$ denotes that the cell on the\text{r}r-th row and \text{c}-th column was previously visited. Our current position is $\text{(r, c)}$, facing direction $\text{di}$, and we want to visit  ​$\text{R} \times \text{C}$ total cells.

As we move through the matrix, our candidate next position is $\text{(cr, cc)}​$. If the candidate is in the bounds of the matrix and unseen, then it becomes our next position; otherwise, our next position is the one after performing a clockwise turn.

```python
class Solution(object):
    def spiralOrder(self, matrix):
        if not matrix: return []
        R, C = len(matrix), len(matrix[0])
        seen = [[False] * C for _ in matrix]
        ans = []
        dr = [0, 1, 0, -1]
        dc = [1, 0, -1, 0]
        r = c = di = 0
        for _ in range(R * C):
            ans.append(matrix[r][c])
            seen[r][c] = True
            cr, cc = r + dr[di], c + dc[di]
            if 0 <= cr < R and 0 <= cc < C and not seen[cr][cc]:
                r, c = cr, cc
            else:
                di = (di + 1) % 4
                r, c = r + dr[di], c + dc[di]
        return ans
```

## 2: Layer-by-Layer

**Intuition**

The answer will be all the elements in clockwise order from the first-outer layer, followed by the elements from the second-outer layer, and so on.

**Algorithm**

We define the $\text{k}$-th outer layer of a matrix as all elements that have minimum distance to some border equal to $\text{k}$. For example, the following matrix has all elements in the first-outer layer equal to 1, all elements in the second-outer layer equal to 2, and all elements in the third-outer layer equal to 3.

```
[[1, 1, 1, 1, 1, 1, 1],
 [1, 2, 2, 2, 2, 2, 1],
 [1, 2, 3, 3, 3, 2, 1],
 [1, 2, 2, 2, 2, 2, 1],
 [1, 1, 1, 1, 1, 1, 1]]
```

For each outer layer, we want to iterate through its elements in clockwise order starting from the top left corner. Suppose the current outer layer has top-left coordinates $\text{(r1, c1)}$ and bottom-right coordinates $\text{(r2, c2)}$.

Then, the top row is the set of elements $\text{(r1, c)}$ for $\text{c = c1,...,c2}$, in that order. The rest of the right side is the set of elements $\text{(r, c2)}$ for $\text{r = r1+1,...,r2}$, in that order. Then, if there are four sides to this layer (ie., $\text{r1 < r2}$ and $\text{c1 < c2}$), we iterate through the bottom side and left side as shown in the solutions below.

![SpiralMatrix](https://leetcode.com/articles/Figures/54_spiralmatrix.png)

```python
class Solution(object):
    def spiralOrder(self, matrix):
        def spiral_coords(r1, c1, r2, c2):
            for c in range(c1, c2 + 1):
                yield r1, c
            for r in range(r1 + 1, r2 + 1):
                yield r, c2
            if r1 < r2 and c1 < c2:
                for c in range(c2 - 1, c1, -1):
                    yield r2, c
                for r in range(r2, r1, -1):
                    yield r, c1

        if not matrix: return []
        ans = []
        r1, r2 = 0, len(matrix) - 1
        c1, c2 = 0, len(matrix[0]) - 1
        while r1 <= r2 and c1 <= c2:
            for r, c in spiral_coords(r1, c1, r2, c2):
                ans.append(matrix[r][c])
            r1 += 1; r2 -= 1
            c1 += 1; c2 -= 1
        return ans
```

**Complexity Analysis**

- Time Complexity: $O(N)$, where $N​$ is the total number of elements in the input matrix. We add every element in the matrix to our final answer.
- Space Complexity: $O(N)$, the information stored in `ans`.

# 78.Subsets(M)

Given a set of **distinct** integers, *nums*, return all possible subsets (the power set).

**Note:** The solution set must not contain duplicate subsets.

**Example:**

```
Input: nums = [1,2,3]
Output:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```

## 1: Iterative

ref: [3ms, easiest solution, no backtracking, no bit manipulation, no dfs, no bullshit](https://leetcode.com/problems/subsets/discuss/122645/3ms-easiest-solution-no-backtracking-no-bit-manipulation-no-dfs-no-bullshit)

While iterating through all numbers, for each new number, we can either pick it or not pick it

1. if pick, just add current number to every existing subset.
2. if not pick, just leave all existing subsets as they are.
   We just combine both into our result.

For example, $[1,2,3]$ intially we have an emtpy set as result $[ [ ] ]$
Considering 1, if not use it, still [ ], if use 1, add it to [ ], so we have [1] now
Combine them, now we have [ [ ], [1] ] as all possible subset

Next considering 2, if not use it, we still have [ [ ], [1] ], if use 2, just add 2 to each previous subset, we have [2], [1,2]
Combine them, now we have [ [ ], [1], [2], [1,2] ]

Next considering 3, if not use it, we still have [ [ ], [1], [2], [1,2] ], if use 3, just add 3 to each previous subset, we have [ [3], [1,3], [2,3], [1,2,3] ]
Combine them, now we have [ [ ], [1], [2], [1,2], [3], [1,3], [2,3], [1,2,3] ]

```java
public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        result.add(new ArrayList<>());
        for(int n : nums){
            int size = result.size();
            for(int i=0; i<size; i++){
                List<Integer> subset = new ArrayList<>(result.get(i));
                subset.add(n);
                result.add(subset);
            }
        }
        return result;
    }
```

## 2.Bit Manipulation

youtube: [bit manipulation](https://www.youtube.com/watch?v=3dEVYiyFKac)

To give all the possible subsets, we just need to exhaust all the possible combinations of the numbers. And each number has only two possibilities: either in or not in a subset. And this can be represented using a bit.

Using `[1, 2, 3]` as an example, `1` appears once in every two consecutive subsets, `2` appears twice in every four consecutive subsets, and `3` appears four times in every eight subsets (initially all subsets are empty):

```
[], [ ], [ ], [    ], [ ], [    ], [    ], [       ]
[], [1], [ ], [1   ], [ ], [1   ], [    ], [1      ]
[], [1], [2], [1, 2], [ ], [1   ], [2   ], [1, 2   ]
[], [1], [2], [1, 2], [3], [1, 3], [2, 3], [1, 2, 3]
```

c++

```c++
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        int n = nums.size(), p = 1 << n;
        vector<vector<int>> subs(p);
        for (int i = 0; i < p; i++) {
            for (int j = 0; j < n; j++) {
                if ((i >> j) & 1) {
                    subs[i].push_back(nums[j]);
                }
            }
        }
        return subs;
    }
};
```







# 373.Find K Pairs with Smallest Sums(M)

You are given two integer arrays **nums1** and **nums2** sorted in ascending order and an integer **k**.

Define a pair **(u,v)** which consists of one element from the first array and one element from the second array.

Find the k pairs **(u1,v1),(u2,v2) ...(uk,vk)** with the smallest sums.

**Example 1:**

```
Input: nums1 = [1,7,11], nums2 = [2,4,6], k = 3
Output: [[1,2],[1,4],[1,6]] 
Explanation: The first 3 pairs are returned from the sequence: 
             [1,2],[1,4],[1,6],[7,2],[7,4],[11,2],[7,6],[11,4],[11,6]
```

**Example 2:**

```
Input: nums1 = [1,1,2], nums2 = [1,2,3], k = 2
Output: [1,1],[1,1]
Explanation: The first 2 pairs are returned from the sequence: 
             [1,1],[1,1],[1,2],[2,1],[1,2],[2,2],[1,3],[1,3],[2,3]
```

**Example 3:**

```
Input: nums1 = [1,2], nums2 = [3], k = 3
Output: [1,3],[2,3]
Explanation: All possible pairs are returned from the sequence: [1,3],[2,3]
```

ref: [Slow 1-liner to Fast solutions](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/discuss/84550/Slow-1-liner-to-Fast-solutions)

Several solutions from naive to more elaborate. I found it helpful to visualize the input as an **m×n matrix** of sums, for example for nums1=[1,7,11], and nums2=[2,4,6]:

```python
      2   4   6
   +------------
 1 |  3   5   7
 7 |  9  11  13
11 | 13  15  17
```

Of course the smallest pair overall is in the top left corner, the one with sum 3. We don't even need to look anywhere else. After including that pair in the output, the next-smaller pair must be the next on the right (sum=5) or the next below (sum=9). We can keep a "horizon" of possible candidates, implemented as a heap / priority-queue, and roughly speaking we'll grow from the top left corner towards the right/bottom. That's what my solution 5 does. Solution 4 is similar, not quite as efficient but a lot shorter and my favorite.

## **Solution 1: Brute Force** (accepted in 560 ms)

Just produce all pairs, sort them by sum, and return the first k.

```python
def kSmallestPairs(self, nums1, nums2, k):
    return sorted(itertools.product(nums1, nums2), key=sum)[:k]
```

my solution:

```python
def kSmallestPairs(self, nums1, nums2, k):
        combine = []
        for i in nums1:
            for j in nums2:
                combine.append(([i, j]))
        combine.sort(key=sum)
        return combine[:k]
```



## **Solution 2: Clean Brute Force** (accepted in 532 ms)

The above produces tuples and while the judge doesn't care, it's cleaner to make them lists as requested:

```python
def kSmallestPairs(self, nums1, nums2, k):
    return map(list, sorted(itertools.product(nums1, nums2), key=sum)[:k])
```

## **Solution 3: Less Brute Force** (accepted in 296 ms)

Still going through all pairs, but only with a generator and `heapq.nsmallest`, which uses a heap of size k. So this only takes O(k) extra memory and O(mn log k) time.

```python
def kSmallestPairs(self, nums1, nums2, k):
    return map(list, heapq.nsmallest(k, itertools.product(nums1, nums2), key=sum))
```

Or (accepted in 368 ms):

```python
def kSmallestPairs(self, nums1, nums2, k):
    return heapq.nsmallest(k, ([u, v] for u in nums1 for v in nums2), key=sum)
```

## **Solution 4: Efficient** (accepted in 112 ms)

The brute force solutions computed the whole matrix (see visualization above). This solution doesn't. It turns each row into a generator of triples [u+v, u, v], only computing the next when asked for one. And then merges these generators with a heap. Takes $O(m + k*log(m))$ time and $O(m)$ extra space.

```python
def kSmallestPairs(self, nums1, nums2, k):
    streams = map(lambda u: ([u+v, u, v] for v in nums2), nums1)
    stream = heapq.merge(*streams)
    return [suv[1:] for suv in itertools.islice(stream, k)]
```

## **Solution 5: More efficient** (accepted in 104 ms)

The previous solution right away considered (the first pair of) all matrix rows (see visualization above). This one doesn't. It starts off only with the very first pair at the top-left corner of the matrix, and expands from there as needed. Whenever a pair is chosen into the output result, the next pair in the row gets added to the priority queue of current options. Also, if the chosen pair is the first one in its row, then the first pair in the next row is added to the queue.

```python
def kSmallestPairs(self, nums1, nums2, k):
    queue = []
    def push(i, j):
        if i < len(nums1) and j < len(nums2):
            heapq.heappush(queue, [nums1[i] + nums2[j], i, j])
    push(0, 0)
    pairs = []
    while queue and len(pairs) < k:
        _, i, j = heapq.heappop(queue)
        pairs.append([nums1[i], nums2[j]])
        push(i, j + 1)
        if j == 0:
            push(i + 1, 0)
    return pairs
```



# 950.Reveal Cards In Increasing Order(M)

In a deck of cards, every card has a unique integer.  You can order the deck in any order you want.

Initially, all the cards start face down (unrevealed) in one deck.

Now, you do the following steps repeatedly, until all cards are revealed:

1. Take the top card of the deck, reveal it, and take it out of the deck.
2. If there are still cards in the deck, put the next top card of the deck at the bottom of the deck.
3. If there are still unrevealed cards, go back to step 1.  Otherwise, stop.

Return an ordering of the deck that would reveal the cards in **increasing order.**

The first entry in the answer is considered to be the top of the deck.

**Example 1:**

```
Input: [17,13,11,2,3,5,7]
Output: [2,13,3,11,5,17,7]
Explanation: 
We get the deck in the order [17,13,11,2,3,5,7] (this order doesn't matter), and reorder it.
After reordering, the deck starts as [2,13,3,11,5,17,7], where 2 is the top of the deck.
We reveal 2, and move 13 to the bottom.  The deck is now [3,11,5,17,7,13].
We reveal 3, and move 11 to the bottom.  The deck is now [5,17,7,13,11].
We reveal 5, and move 17 to the bottom.  The deck is now [7,13,11,17].
We reveal 7, and move 13 to the bottom.  The deck is now [11,17,13].
We reveal 11, and move 17 to the bottom.  The deck is now [13,17].
We reveal 13, and move 17 to the bottom.  The deck is now [17].
We reveal 17.
Since all the cards revealed are in increasing order, the answer is correct.
```

**Note:**

1. `1 <= A.length <= 1000`
2. `1 <= A[i] <= 10^6`
3. `A[i] != A[j]` for all `i != j`

## 1: Simulation

**Intuition and Algorithm**

Simulate the revealing process with a deck set to `[0, 1, 2, ...]`. If for example this deck is revealed in the order `[0, 2, 4, ...]` then we know we need to put the smallest card in index `0`, the second smallest card in index `2`, the third smallest card in index `4`, etc.

anthor explain:[Java Queue Simulation, Step by Step Explanation](https://leetcode.com/problems/reveal-cards-in-increasing-order/discuss/200526/Java-Queue-Simulation-Step-by-Step-Explanation)

Simulate the process with a queue.



1. Sort the deck, it is actually the "final sequence" we want to get according to the question.
2. Then put it back to the result array, we just need to deal with the index now!
3. Simulate the process with a queue (initialized with 0,1,2...(n-1)), now how do we pick the card?
4. We first pick the index at the top: `res[q.poll()]=deck[i]`
5. Then we put the next index to the bottom: `q.add(q.poll());`
6. Repeat it n times, and you will have the result array!

**update**
Let's walk through the example:
Input: `[17,13,11,2,3,5,7]`
Output: `[2,13,3,11,5,17,7]`

1. Sort the deck: `[2,3,5,7,11,13,17]`, this is the increasing order we want to generate
2. Initialize the queue: `[0,1,2,3,4,5,6]`, this is the index of the result array
3. **The first card** we pick is `res[0]`, observe the deck, it should be `deck[0]==2`, so assign `res[0]=2`
4. Then we put `res[1]` to the bottom, so we re-insert `1` to the queue
5. **The second card** we pick is `res[2]`, which should be `deck[1]==3`, so assign `res[2]=3`
6. Then we re-insert `3` to the queue
7. Each time we assign 1 value to the res, so we repeat this n times.



Hope this helps.

```python
class Solution(object):
    def deckRevealedIncreasing(self, deck):
        N = len(deck)
        index = collections.deque(range(N))
        ans = [None] * N

        for card in sorted(deck):
            ans[index.popleft()] = card
            if index:
                index.append(index.popleft())

        return ans
```

**Complexity Analysis**

- Time Complexity: $O(N \log N)$, where N*N* is the length of `deck`.
- Space Complexity: $O(N)$. 

------

Analysis written by: [@awice](https://leetcode.com/awice).





my soluton:

```python
class Solution(object):
    def deckRevealedIncreasing(self, deck):
        """
        :type deck: List[int]
        :rtype: List[int]
        """
        deck.sort()
        if len(deck) <= 2:
            return deck
        rtn = [deck.pop(), deck.pop()]
        while len(deck) > 1:
            rtn.insert(0, deck.pop())
            rtn.insert(0, rtn.pop())
        rtn.insert(0, deck.pop())
        return rtn
```



