# 191.Number of 1 bits(E)

Write a function that takes an unsigned integer and return the number of '1' bits it has (also known as the [Hamming weight](http://en.wikipedia.org/wiki/Hamming_weight)).

**Example 1:**

```
Input: 00000000000000000000000000001011
Output: 3
Explanation: The input binary string 00000000000000000000000000001011 has a total of three '1' bits.
```

**Example 2:**

```
Input: 00000000000000000000000010000000
Output: 1
Explanation: The input binary string 00000000000000000000000010000000 has a total of one '1' bit.
```

**Example 3:**

```
Input: 11111111111111111111111111111101
Output: 31
Explanation: The input binary string 11111111111111111111111111111101 has a total of thirty one '1' bits.
```

**Note:**

- Note that in some languages such as Java, there is no unsigned integer type. In this case, the input will be given as signed integer type and should not affect your implementation, as the internal binary representation of the integer is the same whether it is signed or unsigned.
- In Java, the compiler represents the signed integers using [2's complement notation](https://en.wikipedia.org/wiki/Two%27s_complement). Therefore, in **Example 3** above the input represents the signed integer `-3`.

**Follow up**:

If this function is called many times, how would you optimize it?

## #1: Loop and Flip

The solution is straight-forward. We check each of the 3232 bits of the number. If the bit is 11, we add one to the number of 11-bits.

We can check the i^{th}*i**t**h* bit of a number using a *bit mask*. We start with a mask m=1*m*=1, because the binary representation of 11 is,

0000 0000 0000 0000 0000 0000 0000 000100000000000000000000000000000001 Clearly, a logical AND between any number and the mask 11 gives us the least significant bit of this number. To check the next bit, we shift the mask to the left by one.

0000 0000 0000 0000 0000 0000 0000 001000000000000000000000000000000010

And so on.

```java
public int hammingWeight(int n) {
    int bits = 0;
    int mask = 1;
    for (int i = 0; i < 32; i++) {
        if ((n & mask) != 0) {
            bits++;
        }
        mask <<= 1;
    }
    return bits;
}
```

anther method:

```python
def hammingWeight(self, n):
    """
    :type n: int
    :rtype: int
    """
    rtn = 0
    while n:
    	rtn += n & 1
    	n >>= 1

    return rtn
```

**Complexity Analysis**

The run time depends on the number of bits in n*n*. Because n*n* in this piece of code is a 32-bit integer, the time complexity is $O(1)$.

The space complexity is $O(1)$, since no additional space is allocated.

## #2: Bit Manipulation Trick

We can make the previous algorithm simpler and a little faster. Instead of checking every bit of the number, we repeatedly flip the least-significant $1$-bit of the number to $0$, and add $1$ to the sum. As soon as the number becomes $0$, we know that it does not have any more $1$-bits, and we return the sum.

The key idea here is to realize that for any number n*n*, doing a bit-wise AND of $n$ and $n - 1$ flips the least-significant $1$-bit in $n$ to $0$. Why? Consider the binary representations of n*n* and $n - 1$.

![Number of 1 Bits](https://leetcode.com/media/original_images/191_Number_Of_Bits.png)

*Figure 1. AND-ing nn and n-1 flips the least-significant 1-bit to 0.*

In the binary representation, the least significant 1-bit in n*n* always corresponds to a 0-bit in $n - 1$. Therefore, anding the two numbers $n$ and $n - 1$ always flips the least significant 1-bit in $n$ to $0$, and keeps all other bits the same.

Using this trick, the code becomes very simple.

```java
public int hammingWeight(int n) {
    int sum = 0;
    while (n != 0) {
        sum++;
        n &= (n - 1);
    }
    return sum;
}
```

**Complexity Analysis**

The run time depends on the number of 1-bits in n*n*. In the worst case, all bits in n are 1-bits. In case of a 32-bit integer, the run time is $O(1)$.

The space complexity is $O(1)$, since no additional space is allocated.

Analysis written by: @noran.



# 371.Sum of Two Integers(E)

ref:

1. [python位运算（一）概述](http://www.361way.com/bitwise-operators/5778.html)
2. [Python实现：用位运算符实现加法，不允许使用 “+”](https://blog.csdn.net/qq_25774883/article/details/81784118)
3. [从一道Leetcode题看Python奇葩的位运算](https://zhuanlan.zhihu.com/p/49104244)
4. [A summary: how to use bit manipulation to solve problems easily and efficiently](https://leetcode.com/problems/sum-of-two-integers/discuss/84278/A-summary%3A-how-to-use-bit-manipulation-to-solve-problems-easily-and-efficiently)
5. [Simple explanation on how to arrive at the solution ](https://leetcode.com/problems/sum-of-two-integers/discuss/132479/Simple-explanation-on-how-to-arrive-at-the-solution)
6. [Add Two Numbers Without The "+" Sign (Bit Shifting Basics)](https://www.youtube.com/watch?v=qq64FrA2UXQ)



Calculate the sum of two integers *a* and *b*, but you are **not allowed** to use the operator `+` and `-`.

**Example 1:**

```
Input: a = 1, b = 2
Output: 3
```

**Example 2:**

```
Input: a = -2, b = 3
Output: 1
```

## wiki

Bit manipulation is the act of algorithmically manipulating bits or other pieces of data shorter than a word. Computer programming tasks that require bit manipulation include low-level device control, error detection and correction algorithms, data compression, encryption algorithms, and optimization. For most other tasks, modern programming languages allow the programmer to work directly with abstractions instead of bits that represent those abstractions. Source code that does bit manipulation makes use of the bitwise operations: $AND, OR, XOR, NOT$ and $bit \; shifts$.

Bit manipulation, in some cases, can obviate or reduce the need to loop over a data structure and can give many-fold speed ups, as bit manipulations are processed in parallel, but the code can become more difficult to write and maintain.

## Basics

At the heart of bit manipulation are the bit-wise operators & (and), | (or), ~ (not) and ^ (exclusive-or, xor) and shift operators a << b and a >> b.

> There is no boolean operator counterpart to bitwise exclusive-or, but there is a simple explanation. The exclusive-or operation takes two inputs and returns a 1 if either one or the other of the inputs is a 1, but not if both are. That is, if both inputs are 1 or both inputs are 0, it returns 0. Bitwise exclusive-or, with the operator of a caret, ^, performs the exclusive-or operation on each pair of bits. Exclusive-or is commonly abbreviated XOR.

- Set union A | B
- Set intersection A & B
- Set subtraction A & ~B
- Set negation ALL_BITS ^ A or ~A
- Set bit A |= 1 << bit
- Clear bit A &= ~(1 << bit)
- Test bit (A & 1 << bit) != 0
- Extract last bit A&-A or A&~(A-1) or x^(x&(x-1))
- Remove last bit A&(A-1)
- Get all 1-bits ~0

## Solution

Let's try this with our hand 3 + 2 = 5 , the carry will be with in the brackets i.e "()"

```python
3 => 011 
2 => 010
     ____
     1(1)01
```

Here we will forward the carry at the second bit to get the result.
So which bitwise operator can do this ? A simple observation says that XOR can do that,but it just falls short in dealing with the carry properly, but correctly adds when there is no need to deal with carry.
For Eg:

```python
1   =>  001 
2   =>  010 
1^2 =>  011 (2+1 = 3) 
```

So now when we have carry, to deal with, we can see the result as :

```python
3  => 011 
2  => 010 
3^2=> 001  
```

Here we can see XOR just fell short with the carry generated at the second bit.
So how can we find the carry ? The carry is generated when both the bits are set, i.e (1,1) will generate carry but (0,1 or 1,0 or 0,0) won't generate a carry, so which bitwise operator can do that ? AND gate ofcourse.

To find the carry we can do

```python
3    =>  011 
2    =>  010 
3&2  =>  010
```

now we need to add it to the previous value we generated i.e ( 3 ^ 2), but the carry should be added to the left bit of the one which genereated it.
so we left shift it by one so that it gets added at the right spot.

Hence (3&2)<<1 => 100
so we can now do

```python
3 ^2        =>  001 
(3&2)<<1    =>  100 

Now xor them, which will give 101(5) , we can continue this until the carry becomes zero.
```

java version:

```
class Solution {
    public int getSum(int a, int b) {
      int c; 
      while(b !=0 ) {
        c = (a&b);
        a = a ^ b;
        b = (c)<<1;
      }
      return a;
        
    }
}
```

python version:

```python
class Solution(object):
    def getSum(self, a, b):
        """
        :type a: int
        :type b: int
        :rtype: int
        """
        while b!=0:
            ta = a
            a = a ^ b
            b = ((ta & b) << 1) & 0xffffffff
        hibit = (a & 0x80000000) >> 31
        if hibit == 1:
            return -(((~a)+1) & 0xffffffff)
        else:
            return a & 0xffffffff
```



## Other Examples

Count the number of ones in the binary representation of the given number

```c
int count_one(int n) {
    while(n) {
        n = n&(n-1);
        count++;
    }
    return count;
}
```

Is power of four (actually map-checking, iterative and recursive methods can do the same)

```c
bool isPowerOfFour(int n) {
    return !(n&(n-1)) && (n&0x55555555);
    //check the 1-bit location;
}
```

### `^` tricks

Use `^` to remove even exactly same numbers and save the odd, or save the distinct bits and remove the same.

#### Sum of Two Integers

Use `^` and `&` to add two integers

```c
int getSum(int a, int b) {
    return b==0? a:getSum(a^b, (a&b)<<1); //be careful about the terminating condition;
}
```

#### Missing Number

Given an array containing n distinct numbers taken from 0, 1, 2, ..., n, find the one that is missing from the array. For example, Given nums = [0, 1, 3] return 2. (Of course, you can do this by math.)

```c
int missingNumber(vector<int>& nums) {
    int ret = 0;
    for(int i = 0; i < nums.size(); ++i) {
        ret ^= i;
        ret ^= nums[i];
    }
    return ret^=nums.size();
}
```

### `|` tricks

Keep as many 1-bits as possible

Find the largest power of 2 (most significant bit in binary form), which is less than or equal to the given number N.

```c
long largest_power(long N) {
    //changing all right side bits to 1.
    N = N | (N>>1);
    N = N | (N>>2);
    N = N | (N>>4);
    N = N | (N>>8);
    N = N | (N>>16);
    return (N+1)>>1;
}
```

#### Reverse Bits

Reverse bits of a given 32 bits unsigned integer.

```c
uint32_t reverseBits(uint32_t n) {
    unsigned int mask = 1<<31, res = 0;
    for(int i = 0; i < 32; ++i) {
        if(n & 1) res |= mask;
        mask >>= 1;
        n >>= 1;
    }
    return res;
}
```

```c
uint32_t reverseBits(uint32_t n) {
	uint32_t mask = 1, ret = 0;
	for(int i = 0; i < 32; ++i){
		ret <<= 1;
		if(mask & n) ret |= 1;
		mask <<= 1;
	}
	return ret;
}
```

### `&` tricks

Just selecting certain bits

Reversing the bits in integer

```c
x = ((x & 0xaaaaaaaa) >> 1) | ((x & 0x55555555) << 1);
x = ((x & 0xcccccccc) >> 2) | ((x & 0x33333333) << 2);
x = ((x & 0xf0f0f0f0) >> 4) | ((x & 0x0f0f0f0f) << 4);
x = ((x & 0xff00ff00) >> 8) | ((x & 0x00ff00ff) << 8);
x = ((x & 0xffff0000) >> 16) | ((x & 0x0000ffff) << 16);
```

#### Bitwise AND of Numbers Range

Given a range [m, n] where 0 <= m <= n <= 2147483647, return the bitwise AND of all numbers in this range, inclusive. For example, given the range [5, 7], you should return 4.

```c
int rangeBitwiseAnd(int m, int n) {
    int a = 0;
    while(m != n) {
        m >>= 1;
        n >>= 1;
        a++;
    }
    return m<<a; 
}
```

#### Number of 1 Bits

Write a function that takes an unsigned integer and returns the number of ’1' bits it has (also known as the Hamming weight).

```c
int hammingWeight(uint32_t n) {
	int count = 0;
	while(n) {
		n = n&(n-1);
		count++;
	}
	return count;
}
```

```c
int hammingWeight(uint32_t n) {
    ulong mask = 1;
    int count = 0;
    for(int i = 0; i < 32; ++i){ //31 will not do, delicate;
        if(mask & n) count++;
        mask <<= 1;
    }
    return count;
}
```

## Application

### Repeated DNA Sequences

All DNA is composed of a series of nucleotides abbreviated as A, C, G, and T, for example: "ACGAATTCCG". When studying DNA, it is sometimes useful to identify repeated sequences within the DNA. Write a function to find all the 10-letter-long sequences (substrings) that occur more than once in a DNA molecule.
For example,
Given s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT",
Return: ["AAAAACCCCC", "CCCCCAAAAA"].

```c
class Solution {
public:
    vector<string> findRepeatedDnaSequences(string s) {
        int sLen = s.length();
        vector<string> v;
        if(sLen < 11) return v;
        char keyMap[1<<21]{0};
        int hashKey = 0;
        for(int i = 0; i < 9; ++i) hashKey = (hashKey<<2) | (s[i]-'A'+1)%5;
        for(int i = 9; i < sLen; ++i) {
            if(keyMap[hashKey = ((hashKey<<2)|(s[i]-'A'+1)%5)&0xfffff]++ == 1)
                v.push_back(s.substr(i-9, 10));
        }
        return v;
    }
};
```

But the above solution can be invalid when repeated sequence appears too many times, in which case we should use `unordered_map<int, int> keyMap` to replace `char keyMap[1<<21]{0}`here.

### Majority Element

Given an array of size n, find the majority element. The majority element is the element that appears more than ⌊ n/2 ⌋ times. (bit-counting as a usual way, but here we actually also can adopt sorting and Moore Voting Algorithm)

```c
nt majorityElement(vector<int>& nums) {
    int len = sizeof(int)*8, size = nums.size();
    int count = 0, mask = 1, ret = 0;
    for(int i = 0; i < len; ++i) {
        count = 0;
        for(int j = 0; j < size; ++j)
            if(mask & nums[j]) count++;
        if(count > size/2) ret |= mask;
        mask <<= 1;
    }
    return ret;
}
```

### Maximum Product of Word Lengths

Given a string array words, find the maximum value of length(word[i]) * length(word[j]) where the two words do not share common letters. You may assume that each word will contain only lower case letters. If no such two words exist, return 0.

```
Example 1:
Given ["abcw", "baz", "foo", "bar", "xtfn", "abcdef"]
Return 16
The two words can be "abcw", "xtfn".

Example 2:
Given ["a", "ab", "abc", "d", "cd", "bcd", "abcd"]
Return 4
The two words can be "ab", "cd".

Example 3:
Given ["a", "aa", "aaa", "aaaa"]
Return 0
No such pair of words.
```

Since we are going to use the length of the word very frequently and we are to compare the letters of two words checking whether they have some letters in common:

- using an array of int to pre-store the length of each word reducing the frequently *measuring* process;
- since int has 4 bytes, a 32-bit type, and there are only 26 different letters, so we can just use one bit to indicate the existence of the letter in a word.

```c
int maxProduct(vector<string>& words) {
    vector<int> mask(words.size());
    vector<int> lens(words.size());
    for(int i = 0; i < words.size(); ++i) lens[i] = words[i].length();
    int result = 0;
    for (int i=0; i<words.size(); ++i) {
        for (char c : words[i])
            mask[i] |= 1 << (c - 'a');
        for (int j=0; j<i; ++j)
            if (!(mask[i] & mask[j]))
                result = max(result, lens[i]*lens[j]);
    }
    return result;
}
```

**Attention**

- result after shifting left(or right) too much is undefined
- right shifting operations on negative values are undefined
- right operand in shifting should be non-negative, otherwise the result is undefined
- The & and | operators have lower precedence than comparison operators

### Sets

All the subsets
A big advantage of bit manipulation is that it is trivial to iterate over all the subsets of an N-element set: every N-bit value represents some subset. Even better, `if A is a subset of B then the number representing A is less than that representing B`, which is convenient for some dynamic programming solutions.

It is also possible to iterate over all the subsets of a particular subset (represented by a bit pattern), provided that you don’t mind visiting them in reverse order (if this is problematic, put them in a list as they’re generated, then walk the list backwards). The trick is similar to that for finding the lowest bit in a number. If we subtract 1 from a subset, then the lowest set element is cleared, and every lower element is set. However, we only want to set those lower elements that are in the superset. So the iteration step is just `i = (i - 1) & superset`.

```c
vector<vector<int>> subsets(vector<int>& nums) {
    vector<vector<int>> vv;
    int size = nums.size(); 
    if(size == 0) return vv;
    int num = 1 << size;
    vv.resize(num);
    for(int i = 0; i < num; ++i) {
        for(int j = 0; j < size; ++j)
            if((1<<j) & i) vv[i].push_back(nums[j]);   
    }
    return vv;
}
```

Actually there are two more methods to handle this using `recursion` and `iteration` respectively.

### Bitset

A [bitset](http://www.cplusplus.com/reference/bitset/bitset/?kw=bitset) stores bits (elements with only two possible values: 0 or 1, true or false, ...).
The class emulates an array of bool elements, but optimized for space allocation: generally, each element occupies only one bit (which, on most systems, is eight times less than the smallest elemental type: char).

```c
// bitset::count
#include <iostream>       // std::cout
#include <string>         // std::string
#include <bitset>         // std::bitset

int main () {
  std::bitset<8> foo (std::string("10110011"));
  std::cout << foo << " has ";
  std::cout << foo.count() << " ones and ";
  std::cout << (foo.size()-foo.count()) << " zeros.\n";
  return 0;
}
```

Always welcom new ideas and `practical` tricks, just leave them in the comments!

# 476.Number Complement(E)

Given a positive integer, output its complement number. The complement strategy is to flip the bits of its binary representation.

**Note:**

1. The given integer is guaranteed to fit within the range of a 32-bit signed integer.
2. You could assume no leading zero bit in the integer’s binary representation.

**Example 1:**

```
Input: 5
Output: 2
Explanation: The binary representation of 5 is 101 (no leading zero bits), and its complement is 010. So you need to output 2.
```

**Example 2:**

```
Input: 1
Output: 0
Explanation: The binary representation of 1 is 1 (no leading zero bits), and its complement is 0. So you need to output 0.
```

**Solution**

```python
class Solution(object):
    def findComplement(self, num):
        """
        :type num: int
        :rtype: int
        """
        xor = 1
        while xor <= num:
            xor <<= 1
        # xor = 10000
        return (xor - 1) ^ num
```

# 784.Letter Case Permutation(E)

tag: `bit` / `backtracking` / `BFS`

ref: [LeetCode\] Letter Case Permutation 字母大小写全排列](https://www.cnblogs.com/grandyang/p/9065702.html)

Given a string S, we can transform every letter individually to be lowercase or uppercase to create another string.  Return a list of all possible strings we could create.

```
Examples:
Input: S = "a1b2"
Output: ["a1b2", "a1B2", "A1b2", "A1B2"]

Input: S = "3z4"
Output: ["3z4", "3Z4"]

Input: S = "12345"
Output: ["12345"]
```

**Note:**

- `S` will be a string with length between `1` and `12`.
- `S` will consist only of letters or digits.

## 1: BFS

```python
class Solution(object):
    def letterCasePermutation(self, S):
        """
        :type S: str
        :rtype: List[str]
        """
        rtn = [S]
        for i,char in enumerate(S):
            if char.isalpha():
                rtn.extend([s[:i] + char.swapcase() + s[i+1:] for s in rtn])
                    
        return rtn
```

or bit operate: `'A' = 'a' ^ 32`

```python
def letterCasePermutation(self, S):
        """
        :type S: str
        :rtype: List[str]
        """
        rtn = [S]
        for i,char in enumerate(S):
            if char.isalpha():
                rtn.extend([s[:i] + chr(ord(char)^32) + s[i+1:] for s in rtn])
                    
        return rtn
```

## 2.Binary Mask

如果我们仔细观察S = "abc"这个例子，结果会返回8个，为什么是8个呢，因为每个字母都有大小写两种可能，那么三个字母就有2x2x2=8，正好是2的三次方，那么不正好和二进制数相对应么，如果1对应大写字母，0对应小写字母，则有：

```
000 -> ABC
001 -> ABc
010 -> AbC
011 -> Abc
100 -> aBC
101 -> aBc
110 -> abC
111 -> abc
```

这样的话，我们只需要先统计出S中字母的个数cnt，然后遍历 [0, 2^cnt) 中的每个数字，对于每个数字，再遍历S中的每个字符，如果是字母，那么如果当前位是1，则加入小写字母，反之加入大写字母；如果是数字，则直接加入即可。

```c
class Solution {
public:
    vector<string> letterCasePermutation(string S) {
        vector<string> res;
        int cnt = 0;
        for (char c : S) {
            if (c > '9') ++cnt;
        }
        for (int i = 0; i < (1 << cnt); ++i) {
            int j = 0;
            string word = "";
            for (char c : S) {
                if (c > '9') {
                    if (((i >> j++) & 1) == 1) {
                        word.push_back(tolower(c));
                    } else {
                        word.push_back(toupper(c));
                    }
                } else {
                    word.push_back(c);
                }
            }
            res.push_back(word);
        }
        return res;
    }
};
```



# 137.Single Number II(M)

Given a **non-empty** array of integers, every element appears *three* times except for one, which appears exactly once. Find that single one.

**Note:**

Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

**Example 1:**

```
Input: [2,2,3,2]
Output: 3
```

**Example 2:**

```
Input: [0,1,0,1,0,1,99]
Output: 99
```

## 1: Normal Solution

```python
class Solution(object):
    def singleNumber(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        d = {}
        for num in nums:
            if num in d:
                d[num] += 1
            else:
                d[num] = 1
                
        for key,val in d.items():
            if val == 1:
                return key
```

## 2.Bitwise operation

ref:

1. [Detailed explanation and generalization of the bitwise operation method for single numbers](https://leetcode.com/problems/single-number-ii/discuss/43295/Detailed-explanation-and-generalization-of-the-bitwise-operation-method-for-single-numbers)
2. [leetcode-137-Single Number II-第二种解法](https://cloud.tencent.com/developer/article/1131945)



### I -- Statement of our problem

"Given an array of integers, every element appears `k` (`k > 1`) times except for one, which appears `p` times (`p >= 1, p % k != 0`). Find that single one."

---

### II -- Special case with 1-bit numbers

As others pointed out, in order to apply the bitwise operations, we should rethink how integers are represented in computers -- by bits. To start, let's consider only one bit for now. Suppose we have an array of **1-bit** numbers (which can only be `0` or `1`), we'd like to count the number of `1`'s in the array such that whenever the counted number of `1`reaches a certain value, say `k`, the count returns to zero and starts over (in case you are curious, this `k` will be the same as the one in the problem statement above). To keep track of how many `1`'s we have encountered so far, we need a counter. Suppose the counter has `m` bits in binary form: `xm, ..., x1` (from most significant bit to least significant bit). We can conclude at least the following four properties of the counter:

1. There is an initial state of the counter, which for simplicity is zero;
2. For each input from the array, if we hit a `0`, the counter should remain unchanged;
3. For each input from the array, if we hit a `1`, the counter should increase by one;
4. In order to cover `k` counts, we require `2^m >= k`, which implies `m >= logk`.



Here is the key part: how each bit in the counter (`x1` to `xm`) changes as we are scanning the array. Note we are prompted to use bitwise operations. In order to satisfy the second property, recall what bitwise operations will not change the operand if the other operand is `0`? Yes, you got it: `x = x | 0` and `x = x ^ 0`.

Okay, we have an expression now: `x = x | i` or `x = x ^ i`, where `i` is the scanned element from the array. Which one is better? We don't know yet. So, let's just do the actual counting.

At the beginning, all bits of the counter is initialized to zero, i.e., `xm = 0, ..., x1 = 0`. Since we are gonna choose bitwise operations that guarantee all bits of the counter remain unchanged if we hit `0`'s, the counter will be `0` until we hit the first `1` in the array. After we hit the first `1`, we got: `xm = 0, ...,x2 = 0, x1 = 1`. Let's continue until we hit the second `1`, after which we have: `xm = 0, ..., x2 = 1, x1 = 0`. Note that `x1` changed from `1` to `0`. For `x1 = x1 | i`, after the second count, `x1` will still be `1`. So it's clear we should use `x1 = x1 ^ i`. What about `x2, ..., xm`? The idea is to find the condition under which `x2, ..., xm` will change their values. Take `x2` as an example. If we hit a `1` and need to change the value of `x2`, what must be the value of `x1` right before we do the change? The answer is: `x1` must be `1` otherwise we shouldn't change `x2` because changing `x1` from `0` to `1` will do the job. So `x2` will change value only if `x1` and `i` are both `1`, or mathematically, `x2 = x2 ^ (x1 & i)`. Similarly `xm` will change value only when `xm-1, ..., x1` and `i` are all `1`: `xm = xm ^ (xm-1 & ... & x1 & i)`. Bingo, we've found the bitwise operations!

However, you may notice that the bitwise operations found above will count from `0` until `2^m - 1`, instead of `k`. If `k < 2^m - 1`, we need some "cutting" mechanism to reinitialize the counter to `0` when the count reaches `k`. To this end, we apply bitwise **AND** to `xm,..., x1` with some variable called `mask`, i.e., `xm = xm & mask, ..., x1 = x1 & mask`. If we can make sure that `mask` will be `0` only when the count reaches `k` and be `1` for all other count cases, then we are done. How do we achieve that? Try to think what distinguishes the case with `k` count from all other count cases. Yes, it's the count of `1`'s! For each count, we have unique values for each bit of the counter, which can be regarded as its state. If we write `k` in its binary form: `km,..., k1`, we can construct `mask` as follows:

`mask = ~(y1 & y2 & ... & ym)`, where `yj = xj` if `kj = 1`, and `yj = ~xj` if `kj = 0` (`j = 1` to `m`).

Let's do some examples:

`k = 3: k1 = 1, k2 = 1, mask = ~(x1 & x2)`;

`k = 5: k1 = 1, k2 = 0, k3 = 1, mask = ~(x1 & ~x2 & x3)`;

In summary, our algorithm will go like this (`nums` is the input array):

```java
for (int i : nums) {
    xm ^= (xm-1 & ... & x1 & i);
    xm-1 ^= (xm-2 & ... & x1 & i);
    .....
    x1 ^= i;
    
    mask = ~(y1 & y2 & ... & ym) where yj = xj if kj = 1, and yj = ~xj if kj = 0 (j = 1 to m).

    xm &= mask;
    ......
    x1 &= mask;
}
```

### III -- General case with 32-bit numbers

Now it's time to generalize our results from 1-bit number case to 32-bit integers. One straightforward way would be creating `32` counters for each bit in the integer. You've probably already seen this in other posted [solutions](https://discuss.leetcode.com/topic/455/constant-space-solution/4). However, if we take advantage of bitwise operations, we may be able to manage all the `32` counters "collectively". By saying "collectively", we mean using `m` **32-bit** integers instead of `32` **m-bit** counters, where `m` is the minimum integer that satisfies `m >= logk`. The reason is that bitwise operations apply only to each bit so operations on different bits are independent of each other (kind obvious, right?). This allows us to group the corresponding bits of the `32`counters into one 32-bit integer. Here is a schematic diagram showing how this is done.

![0_1510941016426_137. Single Number II .png](https://discuss.leetcode.com/assets/uploads/files/1510941017203-137.single-number-ii-resized.png)

The top row is the 32-bit integer, where for each bit, we have a corresponding m-bit counter (shown by the column below the upward arrow). Since bitwise operations on each of the `32` bits are independent of each other, we can group, say the `m-th` bit of all counters, into one 32-bit number (shown by the orange box). All bits in this 32-bit number (denoted as `xm`) will follow the same bitwise operations. Since each counter has `m` bits, we end up with `m` 32-bit numbers, which correspond to `x1, ..., xm` defined in part `II`, but now they are 32-bit integers instead of 1-bit numbers. Therefore, in the algorithm developed above, we just need to regard `x1` to `xm` as 32-bit integers instead of 1-bit numbers. Everything else will be the same and we are done. Easy, hum?

------

### IV -- What to return

The last thing is what value we should return, or equivalently which one of `x1` to `xm` will equal the single element. To get the correct answer, we need to understand what the `m` 32-bit integers `x1` to `xm` represent. Take `x1` as an example. `x1` has `32` bits and let's label them as `r` (`r = 1` to `32`). After we are done scanning the input array, the value for the `r-th` bit of `x1` will be determined by the `r-th` bit of all the elements in the array (more specifically, suppose the total count of `1` for the `r-th` bit of all the elements in the array is `q`, `q' = q % k` and in its binary form: `q'm,...,q'1`, then by definition the `r-th` bit of `x1` will be equal to `q'1`). Now you can ask yourself this question: what does it imply if the `r-th` bit of `x1` is `1`?

The answer is to find what can contribute to this `1`. Will an element that appears `k` times contribute? No. Why? Because for an element to contribute, it has to satisfy at least two conditions at the same time: the `r-th` bit of this element is `1` and the number of appearance of this `1` is not an integer multiple of `k`. The first condition is trivial. The second comes from the fact that whenever the number of `1` hit is `k`, the counter will go back to zero, which means the corresponding bit in `x1` will be reset to `0`. For an element that appears `k` times, it's impossible to meet these two conditions simultaneously so it won't contribute. At last, only the single element which appears `p` (`p % k != 0`) times will contribute. If `p > k`, then the first `k * [p/k]` (`[p/k]`denotes the integer part of `p/k`) single elements won't contribute either. So we can always set `p' = p % k` and say the single element appears effectively `p'` times.

Let's write `p'` in its binary form: `p'm, ..., p'1` (note that `p' < k`, so it will fit into `m` bits). Here I **claim the condition** for `xj` to equal the single element is `p'j = 1` (`j = 1` to `m`), with a quick proof given below.

If the `r-th` bit of `xj` is `1`, we can safely say the `r-th` bit of the single element is also `1` (otherwise nothing can make the `r-th` bit of `xj` to be `1`). We are left to prove that if the `r-th` bit of `xj` is `0`, then the `r-th` bit of the single element can only be `0`. Just suppose in this case the `r-th` bit of the single element is `1`, let's see what will happen. At the end of the scan, this `1` will be counted `p'` times. By definition the `r-th` bit of `xj` will be equal to `p'j`, which is `1`. This contradicts with the presumption that the `r-th` bit of `xj` is `0`. Therefore we conclude the `r-th` bit of `xj` will always be the same as the `r-th` bit of the single number as long as `p'j = 1`. Since this is true for all bits in `xj` (i.e., true for `r = 1` to `32`), we conclude `xj` will equal the single element as long as `p'j = 1`.

So now it's clear what we should return. Just express `p' = p % k` in its binary form and return any of the corresponding `xj` as long as `p'j = 1`. In total, the algorithm will run in `O(n * logk)` time and `O(logk)` space.

---

**Side note**: There is a general formula relating each bit of `xj` to `p'j` and each bit of the single number `s`, which is given by `(xj)_r = s_r & p'j`, with `(xj)_r` and `s_r`denoting respectively the `r-th` bit of `xj` and the single number `s`. From this formula, it's easy to see that `(xj)_r = s_r` if `p'j = 1`, that is, `xj = s` as long as `p'j = 1`, as shown above. Furthermore, we have `(xj)_r = 0` if `p'j = 0`, regardless of the value of the single number, that is, `xj = 0` as long as `p'j = 0`. So in summary we obtain: `xj = s` if `p'j = 1`, and `xj = 0` if `p'j = 0`. This implies the expression (`x1 | x2 | ... | xm`) will also be evaluated to the single number `s`, since the expression will essentially take the `OR` operations of the single number with itself and some `0`s, which boils down to the single number eventually.

---

### V -- Quick examples

Here is a list of few quick examples to show how the algorithm works (you can easily come up with other examples):

1、`k = 2, p = 1`
`k` is `2`, then `m = 1`, we need only one 32-bit integer (`x1`) as the counter. And `2^m = k` so we do not even need a mask! A complete java program will look like:

```java
    public int singleNumber(int[] nums) {
        int x1 = 0;
         
        for (int i : nums) {
            x1 ^= i;
        }
         
        return x1;
    }
```

2、`k = 3, p = 1`
`k` is `3`, then `m = 2`, we need two 32-bit integers(`x2`, `x1`) as the counter. And `2^m > k` so we do need a mask. Write `k` in its binary form: `k = '11'`, then `k1 = 1`, `k2 = 1`, so we have `mask = ~(x1 & x2)`. A complete java program will look like:

```java
    public int singleNumber(int[] nums) {
        int x1 = 0, x2 = 0, mask = 0;
         
        for (int i : nums) {
            x2 ^= x1 & i;
            x1 ^= i;
            mask = ~(x1 & x2);
            x2 &= mask;
            x1 &= mask;
        }
		// Since p = 1, in binary form p = '01', then p1 = 1, so we should return x1. 
        return x1; 
        // If p = 2, in binary form p = '10', then p2 = 1, and we should return x2.
        // Or alternatively we can simply return (x1 | x2).
    }
```

3、`k = 5, p = 3`
`k` is `5`, then `m = 3`, we need three 32-bit integers(`x3`, `x2`, `x1`) as the counter. And `2^m > k` so we need a mask. Write `k` in its binary form: `k = '101'`, then `k1 = 1`, `k2 = 0`, `k3 = 1`, so we have `mask = ~(x1 & ~x2 & x3)`. A complete java program will look like:

```java
    public int singleNumber(int[] nums) {
        int x1 = 0, x2 = 0, x3  = 0, mask = 0;
   
        for (int i : nums) {
            x3 ^= x2 & x1 & i;
            x2 ^= x1 & i;
            x1 ^= i;
            mask = ~(x1 & ~x2 & x3);
            x3 &= mask;
            x2 &= mask;
            x1 &= mask;
        }
        
        return x1;  
        // Since p = 3, in binary form p = '011', then p1 = p2 = 1, so we can return either x1 or x2.            
        // If p = 4, in binary form p = '100', only p3 = 1, which implies we can only return x3.     
        // Or alternatively we can simply return (x1 | x2 | x3).
    }
```

Lastly I would like to thank those for providing feedbacks to make this post better. Hope it helps and happy coding!

# 477.Total Hamming Distance(M)

The [Hamming distance](https://en.wikipedia.org/wiki/Hamming_distance) between two integers is the number of positions at which the corresponding bits are different.

Now your job is to find the total Hamming distance between all pairs of the given numbers.

**Example:**

```
Input: 4, 14, 2

Output: 6

Explanation: In binary representation, the 4 is 0100, 14 is 1110, and 2 is 0010 (just
showing the four bits relevant in this case). So the answer will be:
HammingDistance(4, 14) + HammingDistance(4, 2) + HammingDistance(14, 2) = 2 + 2 + 2 = 6
```

**Note:**

1. Elements of the given array are in the range of `0 `to `10^9`
2. Length of the array will not exceed `10^4`.

```python
class Solution(object):
    def totalHammingDistance(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        record = [[0, 0] for _ in range(32)]
        
        for num in nums:
            for bit in record:
                bit[num%2] += 1
                num //= 2
                
        return sum(x * y for x, y in record)
```

or

```python
class Solution(object):
    def totalHammingDistance(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        rtn = 0
        for b in zip(*map(':032b'.format(), nums)):
            rtn += b.count('0') * b.count('1')
        return rtn
```









