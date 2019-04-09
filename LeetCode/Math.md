# 29.Divide Two Integers(M)

Given two integers `dividend` and `divisor`, divide two integers without using multiplication, division and mod operator.

Return the quotient after dividing `dividend` by `divisor`.

The integer division should truncate toward zero.

**Example 1:**

```
Input: dividend = 10, divisor = 3
Output: 3
```

**Example 2:**

```
Input: dividend = 7, divisor = -3
Output: -2
```

**Note:**

- Both dividend and divisor will be 32-bit signed integers.
- The divisor will never be 0.
- Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: $[−2^{31},  2^{31} − 1]$. For the purpose of this problem, assume that your function returns $2^{31} − 1$ when the division result overflows.

ref: [C++ bit manipulations](https://leetcode.com/problems/divide-two-integers/discuss/13407/C%2B%2B-bit-manipulations)

The key observation is that the quotient of a division is just the number of times that we can subtract the `divisor` from the `dividend` without making it negative.

Suppose `dividend = 15` and `divisor = 3`, `15 - 3 > 0`. We now try to subtract more by *shifting* `3` to the left by `1` bit (`6`). Since `15 - 6 > 0`, shift `6` again to `12`. Now `15 - 12 > 0`, shift `12` again to `24`, which is larger than `15`. So we can at most subtract `12` from `15`. Since `12` is obtained by shifting `3` to left twice, it is `1 << 2 = 4`times of `3`. We add `4` to an answer variable (initialized to be `0`). The above process is like `15 = 3 * 4 + 3`. We now get part of the quotient (`4`), with a remaining dividend `3`.

Then we repeat the above process by subtracting `divisor = 3` from the remaining `dividend = 3` and obtain `0`. We are done. In this case, no shift happens. We simply add `1 << 0 = 1` to the answer variable.

This is the full algorithm to perform division using bit manipulations. The sign also needs to be taken into consideration. And we still need to handle one overflow case: `dividend = INT_MIN` and `divisor = -1`.

```c++
class Solution {
public:
    int divide(int dividend, int divisor) {
        if (dividend == INT_MIN && divisor == -1) {
            return INT_MAX;
        }
        long dvd = labs(dividend), dvs = labs(divisor), ans = 0;
        int sign = (dividend < 0) ^ (divisor < 0) ? -1 : 1;
        while (dvd >= dvs) {
            long temp = dvs, m = 1;
            while (dvd >= (temp << 1)) {
                temp <<= 1;
                m <<= 1;
            }
            dvd -= temp;
            ans += m;
        }
        return sign * ans;
    }
};
```

Python version:

```python
class Solution(object):
    def divide(self, dividend, divisor):
        """
        :type dividend: int
        :type divisor: int
        :rtype: int
        """
        if dividend == -2147483648 and divisor == -1:
            return 2147483647
        sign = -1 if (dividend < 0) ^ (divisor < 0) else 1
        dvd, dvs = abs(dividend), abs(divisor)
        rtn = 0
        while dvd >= dvs:
            temp = dvs, m = 1
            while dvd >= (temp << 1):
                temp  <<= 1
                m <<= 1
            dvd -= temp
            rtn += m
        return sign * rtn
```



# 628.Maximmum Product of Three Numbers

Given an integer array, find three numbers whose product is maximum and output the maximum product.

**Example 1:**

```
Input: [1,2,3]
Output: 6
```

 

**Example 2:**

```
Input: [1,2,3,4]
Output: 24
```

 

**Note:**

1. The length of the given array will be in range $[3,10^4]$ and all elements are in the range [-1000, 1000].
2. Multiplication of any three numbers in the input won't exceed the range of 32-bit signed integer.



## Approach #1 Brute Force [Time Limit Exceeded]

The simplest solution is to consider every triplet out of the given $nums$ array and check their product and find out the maximum product out of them.

**Complexity Analysis**

- Time complexity : $O(n^3)$. We need to consider every triplet from $nums$ array of length $n$.
- Space complexity : $O(1)​$. Constant extra space is used.

## Approach #2 Using Sorting [Accepted]

**Algorithm**

Another solution could be to sort the given $nums$ array(in ascending order) and find out the product of the last three numbers.

But, we can note that this product will be maximum only if all the numbers in $nums$ array are positive. But, in the given problem statement, negative elements could exist as well.

Thus, it could also be possible that two negative numbers lying at the left extreme end could also contribute to lead to a larger product if the third number in the triplet being considered is the largest positive number in the $nums​$ array.

Thus, either the product $nums[0] \times nums[1] \times nums[n-1]$ or $nums[n-3] \times nums[n-2] \times nums[n-1]$ will give the required result. Thus, we need to find the larger one from out of these values.

Java version:

```java
public class Solution {
    public int maximumProduct(int[] nums) {
        Arrays.sort(nums);
        return Math.max(nums[0] * nums[1] * nums[nums.length - 1], nums[nums.length - 1] * nums[nums.length - 2] * nums[nums.length - 3]);
    }
}
```

Python version:

```python
class Solution(object):
    def divide(self, dividend, divisor):
        """
        :type dividend: int
        :type divisor: int
        :rtype: int
        """
        if dividend == -2147483648 and divisor == -1:
            return 2147483647
        sign = -1 if (dividend < 0) ^ (divisor < 0) else 1
        dvd, dvs = abs(dividend), abs(divisor)
        rtn = 0
        while dvd >= dvs:
            temp = dvs, m = 1
            while dvd >= (temp << 1):
                temp  <<= 1
                m <<= 1
            dvd -= temp
            rtn += m
        return sign * rtn
```

**Complexity Analysis**

- Time complexity : $O\big(nlog(n)\big)$ Sorting the nums array takes $nlog(n)$ time.
- Space complexity : $O(log(n))$ . Sorting takes $O(log \;n)$ space.

## Approach #3 Single Scan [Accepted]

**Algorithm**

We need not necessarily sort the given $nums$ array to find the maximum product. Instead, we can only find the required 2 smallest values($min1$ and $min2$) and the three largest values($max1, max2, max3$) in the $nums$array, by iterating over the $nums$ array only once.

At the end, again we can find out the larger value out of $min1 \times min2 \times max1$ and $max1 \times max2 \times max3$ to find the required maximum product.

```java
public class Solution {
    public int maximumProduct(int[] nums) {
        int min1 = Integer.MAX_VALUE, min2 = Integer.MAX_VALUE;
        int max1 = Integer.MIN_VALUE, max2 = Integer.MIN_VALUE, max3 = Integer.MIN_VALUE;
        for (int n: nums) {
            if (n <= min1) {
                min2 = min1;
                min1 = n;
            } else if (n <= min2) {     // n lies between min1 and min2
                min2 = n;
            }
            if (n >= max1) {            // n is greater than max1, max2 and max3
                max3 = max2;
                max2 = max1;
                max1 = n;
            } else if (n >= max2) {     // n lies betweeen max1 and max2
                max3 = max2;
                max2 = n;
            } else if (n >= max3) {     // n lies betwen max2 and max3
                max3 = n;
            }
        }
        return Math.max(min1 * min2 * max1, max1 * max2 * max3);
    }
}
```

**Complexity Analysis**

- Time complexity : $O(n)$. Only one iteration over the $nums$ array of length $n$ is required.
- Space complexity : $O(1)$. Constant extra space is used.

Analysis written by: [@vinod23](https://leetcode.com/vinod23)

# 970.Powerful Integers

Given two non-negative integers `x` and `y`, an integer is *powerful* if it is equal to `x^i + y^j` for some integers `i >= 0` and `j >= 0`.

Return a list of all *powerful* integers that have value less than or equal to `bound`.

You may return the answer in any order.  In your answer, each value should occur at most once.

 

**Example 1:**

```
Input: x = 2, y = 3, bound = 10
Output: [2,3,4,5,7,9,10]
Explanation: 
2 = 2^0 + 3^0
3 = 2^1 + 3^0
4 = 2^0 + 3^1
5 = 2^1 + 3^1
7 = 2^2 + 3^1
9 = 2^3 + 3^0
10 = 2^0 + 3^2
```

**Example 2:**

```
Input: x = 3, y = 5, bound = 15
Output: [2,4,6,8,10,14]
```

 

**Note:**

- `1 <= x <= 100`
- `1 <= y <= 100`
- `0 <= bound <= 10^6`



**Intuition**

If $x^i \gt \text{bound}$, the sum $x^i + y^j$ can't be less than or equal to the bound. Similarly for $y^j$.

Thus, we only have to check for $0 \leq i, j \leq \log_x(\text{bound}) \lt 18$.

We can use a `HashSet` to store all the different values.

```python
class Solution(object): 
    def powerfulIntegers(self, x, y, bound):
        ans = set()
        # 2**18 > bound
        for i in xrange(18):
            for j in xrange(18):
                v = x**i + y**j
                if v <= bound:
                    ans.add(v)
        return list(ans)
```

**Complexity Analysis**

- Time Complexity: $O(\log^2{\text{bound}})$.
- Space Complexity: $O(\log^2{\text{bound}})$. 

Analysis written by: [@awice](https://leetcode.com/awice).



# 50. Pow(x, n) (M)

Implement [pow(*x*, *n*)](http://www.cplusplus.com/reference/valarray/pow/), which calculates *x* raised to the power *n* ($x^n$).

**Example 1:**

```
Input: 2.00000, 10
Output: 1024.00000
```

**Example 2:**

```
Input: 2.10000, 3
Output: 9.26100
```

**Example 3:**

```
Input: 2.00000, -2
Output: 0.25000
Explanation: 2^-2 = 1/2^2 = 1/4 = 0.25
```

**Note:**

- -100.0 < *x* < 100.0
- *n* is a 32-bit signed integer, within the range $[−2^{31}, 2^{31} − 1]$



ref: [ Iterative Log(N) solution with Clear Explanation](https://leetcode.com/problems/powx-n/discuss/19563/Iterative-Log(N)-solution-with-Clear-Explanation)

I couldn't find a clear explanation for an interative Log(n) solution so here's mine. The basic idea is to decompose the exponent into powers of 2, so that you can keep dividing the problem in half. For example, lets say $N = 9 = 2^3 + 2^0 = 1001$ in binary. Then:  $x^9 = x^{(2^3)} * x^{(2^0)}$ 

We can see that every time we encounter a 1 in the binary representation of N, we need to multiply the answer with $x^{(2^i)}$ where **i** is the **ith** bit of the exponent. Thus, we can keep a running total of repeatedly squaring $x - (x, x^2, x^4, x^8, etc)​$ and multiply it by the answer when we see a 1.

To handle the case where N=INTEGER_MIN we use a long (64-bit) variable. Below is solution:

```java
public class Solution {
    public double MyPow(double x, int n) {
        double ans = 1;
        long absN = Math.Abs((long)n);
        while(absN > 0) {
            if((absN&1)==1) ans *= x;
            absN >>= 1;
            x *= x;
        }
        return n < 0 ?  1/ans : ans;
    }
}
```

my python version:

```python
class Solution:
    def myPow(self, x, n):
        """
        :type x: float
        :type n: int
        :rtype: float
        """
        if n == 0: return 1.0
        posn = abs(n)
        total, posn, num = self.re(x, posn)
        while posn > 0:
            t, posn, count = self.re(x, posn)
            num += count
            total *= t
        for _ in range(abs(n) - num):
            total *= x
        if n < 0 and total != 0.0:
            return 1/total
        return total,num
    
    def re(self, x, n):
        total = 1.0 * x
        count = 1
        temp = n
        while n // 2 != 0:
            total *= total
            count *= 2
            n //= 2
        return total, temp - count, count
```



# 319.Bulb Switcher(M)

There are *n* bulbs that are initially off. You first turn on all the bulbs. Then, you turn off every second bulb. On the third round, you toggle every third bulb (turning on if it's off or turning off if it's on). For the *i*-th round, you toggle every *i* bulb. For the *n*-th round, you only toggle the last bulb. Find how many bulbs are on after *n* rounds.

**Example:**

```
Input: 3
Output: 1 
Explanation: 
At first, the three bulbs are [off, off, off].
After first round, the three bulbs are [on, on, on].
After second round, the three bulbs are [on, off, on].
After third round, the three bulbs are [on, off, off]. 

So you should return 1, because there is only one bulb is on.
```



ref: [Share my o(1) solution with explanation](https://leetcode.com/problems/bulb-switcher/discuss/77112/Share-my-o(1)-solution-with-explanation)

As we know that there are n bulbs, let's name them as $1, 2, 3, 4, ..., n$.

1. You first turn on all the bulbs.
2. Then, you turn off every second bulb.$(2, 4, 6, ...)$
3. On the third round, you toggle every third bulb.$(3, 6, 9, ...)$
4. For the ith round, you toggle every i bulb.$(i, 2i, 3i, ...)$
5. For the nth round, you only toggle the last bulb.$(n)$

If n > 6, you can find that bulb 6 is toggled in round 2 and 3.

Later, it will also be toggled in round 6, and round 6 will be the last round when bulb 6 is toggled.

Here, **2,3 and 6 are all factors of 6 (except 1).**

## Prove

We can come to the conclusion that **the bulb i is toggled $k$ times.**

Here, $k$ is **the number of i's factors (except 1)**. $k + 1$ will be **the total number of i's factors**

For example:

- **Factors of 6: 1, 2, 3, 6 (3 factors except 1, so it will be toggled 3 times)**
- **Factors of 7: 1, 7 (1 factors except 1, so it will be toggled once)**
  ....

Now, the key problem here is to judge **whether $k$ is even or odd.**

Since **all bulbs are on at the beginning**, we can get:

- **If k is odd, the bulb will be off in the end.(after odd times of toggling).**
- **If k is even, the bulb i will be on in the end (after even times of toggling).**

As we all know, **a natural number can divided by 1 and itself**, and **all factors appear in pairs**.

**When we know that p is i's factor, we are sure q = i/p is also i's factor.**

**If i has no factor p that makes p = i/p, k+ 1 is even.**

**If i has a factor p that makes p = i/p (i = p^2, i is a perfect square of p), k+ 1 is odd.**

So we get that **in the end**:

- If ***i*** is a **perfect square** , *k*+ 1 is odd, **k is even (bulb i is on)**.
- If ***i*** is **NOT** a **perfect square** , *k*+ 1 is even, **k is odd (bulb i is off)**.

We want to find **how many bulbs are on** after *n* rounds (**In the end**).

That means we need to find out **how many perfect square numbers are NO MORE than n**.

The **number of perfect square numbers which are no more than n**, is the **square root of the maximum perfect square number which is NO MORE than n**

## **Result:**

The **square root of the maximum perfect square number which is NO MORE than n** is the **integer part of sqrt(n).** (**If i = 1, it will NEVER be toggled, k is 0 (even) here which meets the requirement.**)

```c++
class Solution {
public:
    int bulbSwitch(int n) {
        return sqrt(n);
    }
};
```

 

my answer:

```python
class Solution:
    def bulbSwitch(self, n):
        """
        :type n: int
        :rtype: int
        """
        count = 0
        i = 1
        while n > 0:
            n = n - i * 2 - 1
            i += 1
            count += 1
            
        return count
```



# 670.Maximum Swap(M)

Given a non-negative integer, you could swap two digits **at most** once to get the maximum valued number. Return the maximum valued number you could get.

**Example 1:**

```
Input: 2736
Output: 7236
Explanation: Swap the number 2 and the number 7.
```

**Example 2:**

```
Input: 9973
Output: 9973
Explanation: No swap.
```

**Note:**

1. The given number is in the range $[0, 10^8]$

## Approach #1: Brute Force [Accepted]

**Intuition**

The number only has at most 8 digits, so there are only $\text{C}^{2}_{8}= 28$ available swaps. We can easily brute force them all.

**Algorithm**

We will store the candidates as lists of length \text{len(num)}len(num). For each candidate swap with positions $\text{(i, j)}​$ we swap the number and record if the candidate is larger than the current answer, then swap back to restore the original number.

The only detail is possibly to check that we didn't introduce a leading zero. We don't actually need to check it, because our original number doesn't have one.

```python
def maximumSwap(self, num):
    A = list(str(num))
    ans = A[:]
    for i in range(len(A)):
        for j in range(i+1, len(A)):
            A[i], A[j] = A[j], A[i]
            if A > ans: ans = A[:]
            A[i], A[j] = A[j], A[i]

    return int("".join(ans))
```

**Complexity Analysis**

- Time Complexity: $O(N^3)$, where N*N* is the total number of digits in the input number. For each pair of digits, we spend up to $O(N)​$ time to compare the final sequences.
- Space Complexity: $O(N)$, the information stored in $\text{A}$.

## Approach #2: Greedy [Accepted]

**Intuition**

At each digit of the input number in order, if there is a larger digit that occurs later, we know that the best swap must occur with the digit we are currently considering.

**Algorithm**

We will compute $\text{last[d] = i}$ , the index $\text{i}$ of the last occurrence of digit $\text{d}$ (if it exists).

Afterwards, when scanning the number from left to right, if there is a larger digit in the future, we will swap it with the largest such digit; if there are multiple such digits, we will swap it with the one that occurs the latest.

```python
class Solution(object):
    def maximumSwap(self, num):
        A = map(int, str(num))
        last = {x: i for i, x in enumerate(A)}
        for i, x in enumerate(A):
            for d in xrange(9, x, -1):
                if last.get(d, None) > i:
                    A[i], A[last[d]] = A[last[d]], A[i]
                    return int("".join(map(str, A)))
        return num
```

**Complexity Analysis**

- Time Complexity: $O(N)$, where $N$ is the total number of digits in the input number. Every digit is considered at most once.
- Space Complexity: $O(1)$. The additional space used by $\text{last}$ only has up to 10 values.

Analysis written by: [@awice](https://leetcode.com/awice)

# 991.Broken Calculator(M)

On a broken calculator that has a number showing on its display, we can perform two operations:

- **Double**: Multiply the number on the display by 2, or;
- **Decrement**: Subtract 1 from the number on the display.

Initially, the calculator is displaying the number `X`.

Return the minimum number of operations needed to display the number `Y`.

**Example 1:**

```
Input: X = 2, Y = 3
Output: 2
Explanation: Use double operation and then decrement operation {2 -> 4 -> 3}.
```

**Example 2:**

```
Input: X = 5, Y = 8
Output: 2
Explanation: Use decrement and then double {5 -> 4 -> 8}.
```

**Example 3:**

```
Input: X = 3, Y = 10
Output: 3
Explanation:  Use double, decrement and double {3 -> 6 -> 5 -> 10}.
```

**Example 4:**

```
Input: X = 1024, Y = 1
Output: 1023
Explanation: Use decrement operations 1023 times.
```

**Note:**

1. `1 <= X <= 10^9`
2. `1 <= Y <= 10^9`

#### Approach 1: Work Backwards

**Intuition**

Instead of multiplying by 2 or subtracting 1 from `X`, we could divide by 2 (when `Y` is even) or add 1 to `Y`.

The motivation for this is that it turns out we always greedily divide by 2:

- If say `Y` is even, then if we perform 2 additions and one division, we could instead perform one division and one addition for less operations [`(Y+2) / 2` vs `Y/2 + 1`].
- If say `Y` is odd, then if we perform 3 additions and one division, we could instead perform 1 addition, 1 division, and 1 addition for less operations [`(Y+3) / 2` vs `(Y+1) / 2 + 1`].

**Algorithm**

While `Y` is larger than `X`, add 1 if it is odd, else divide by 2. After, we need to do `X - Y` additions to reach `X`.

```python
class Solution(object):
    def brokenCalc(self, X, Y):
        ans = 0
        while Y > X:
            ans += 1
            if Y%2: Y += 1
            else: Y /= 2

        return ans + X-Y
```

**Complexity Analysis**

- Time Complexity: $O(\log Y)$.
- Space Complexity: $O(1)​$. 

Analysis written by: [@awice](https://leetcode.com/awice).