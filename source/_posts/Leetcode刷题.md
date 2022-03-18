---
title: Leetcode刷题
date: 2020-04-29 20:00:00
tags: [Leetcode,编程,Python,SQL,题库]
categories: 题库
index_img: /img/leetcode.png
mathjax: true
math: true
toc: true
#top: true
---

<center>Leetcode刷题记录~</center>
<!--more-->

# 1. Two Sum
[题目详情](https://leetcode.com/problems/two-sum/)
```Python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        i = 0
        while nums:
            num = nums[0]
            nums.pop(0)
            if target - num in nums:
                return [i, nums.index(target-num) + i + 1]
            i += 1
```

# 9. Palindrome Number
[题目详情](https://leetcode.com/problems/palindrome-number/)
```Python
class Solution:
    def isPalindrome(self, x: int) -> bool:
        return str(x) == str(x)[::-1]
```

# 14. Longest Common Prefix
- [题目详情](https://leetcode.com/problems/longest-common-prefix/)
- TAG: 公共子串, 字符串
```Python
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        prefix = ""
        if strs == []:
            return prefix
        else:
            n = min([len(x) for x in strs])
            if n == 0:
                return prefix
            else:
                for i in range(n):
                    pre = [x[:i+1] for x in strs]
                    if len(set(pre)) == 1:
                        prefix = strs[0][:i+1]
                        if i == n-1:
                            return prefix
                        else:
                            pass
                return prefix
```

[题目详情]()
```Python

```

# 67. Add Binary
[题目详情](https://leetcode.com/problems/add-binary/)
```Python
class Solution:
    def addBinary(self, a: str, b: str) -> str:
        return bin(int(a, 2) + int(b, 2))[2:]
```




# 177. Nth Highest Salary
- [题目详情](https://leetcode.com/problems/nth-highest-salary/)
- TAG: SQL
```SQL
-- Runtime=553ms
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
DECLARE M INT;
SET M=N-1;
  RETURN (
      # Write your MySQL query statement below.
      SELECT Salary FROM 
            (SELECT DISTINCT Salary FROM Employee ORDER BY Salary DESC) a
      LIMIT 1 OFFSET M
  );
END
-- Runtime=416ms
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
DECLARE M INT;
SET M=N-1;
  RETURN (
      # Write your MySQL query statement below.
      SELECT DISTINCT Salary FROM Employee
      ORDER BY Salary DESC
      LIMIT 1 OFFSET M
  );
```

# 268. Missing Number
[题目详情](https://leetcode.com/problems/missing-number/)
```Python
## Runtime: 4364 ms, Memory Usage: 15 MB
class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        n = len(nums)
        for i in range(n+1):
            if i not in nums:
                return i
## Runtime: 128 ms, Memory Usage: 15.1 MB
class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        n = len(nums)
        return n*(n+1)//2 -sum(nums)
```

# 300. Longest Increasing Subsequence
- [题目详情](https://leetcode.com/problems/longest-increasing-subsequence/)
- TAG: 动态规划
- 思路：[GitHub-Fucking Algorithm-动态规划设计：最长递增子序列](https://labuladong.github.io/ebook/%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%E7%B3%BB%E5%88%97/%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%E8%AE%BE%E8%AE%A1%EF%BC%9A%E6%9C%80%E9%95%BF%E9%80%92%E5%A2%9E%E5%AD%90%E5%BA%8F%E5%88%97.html)
```Python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        n = len(nums)  ## 列表长度
        if n == 0:
            return 0
        dp = [1 for i in range(n)]  ## 初始值都是1
        for i in range(n):
            for j in range(i):
                if nums[i] > nums[j]:
                    dp[i] = max(dp[i], dp[j] + 1)
        return max(dp)
```

# 371. Sum of Two Integers
- [题目详情](https://leetcode.com/problems/sum-of-two-integers/)
- TAG: 数, amazing
- Calculate the sum of two integers a and b, but you are **not allowed** to use the operator + and -.
- 参考:https://leetcode.com/problems/sum-of-two-integers/discuss/607587/Python-two-liner-beats-100-(12-ms-runtime)-without-bit-manipulation-or-native-addition
```Python
class Solution:
    def getSum(self, a: int, b: int) -> int:
        base = 1.0000000001
        return round(math.log((base**a) * (base**b), base))
```
- $a=\log_z{z^a}$
- $b=\log_z{z^b}$
- $a+b=\log_z{z^a}+\log_z{z^b}=\log_z{(z^a\cdot z^b)}$

# 657. Robot Return to Origin
[题目详情](https://leetcode.com/problems/robot-return-to-origin/)
```Python
## Runtime=64ms, Memory=13.8MB
class Solution:
    def judgeCircle(self, moves: str) -> bool:
        pos = [0, 0]
        for move in moves:
            if move=='L':
                pos[0] += 1
            elif move=='R':
                pos[0] -= 1
            elif move=='U':
                pos[1] += 1
            else:
                pos[1] -= 1
        return pos==[0,0]
## Solution
## Runtime=60ms, Memory=13.8MB
class Solution:
    def judgeCircle(self, moves: str) -> bool:
        x = y = 0
        for move in moves:
            if move == 'U': y -= 1
            elif move == 'D': y += 1
            elif move == 'L': x -= 1
            elif move == 'R': x += 1

        return x == y == 0
```


# 728. Self Dividing Numbers
[题目详情](https://leetcode.com/problems/self-dividing-numbers/)
```Python
## Runtime=68ms, Memory=13.9MB
class Solution:
    def selfDividingNumbers(self, left: int, right: int) -> List[int]:
        result = []
        for num in range(left, right+1):
            nums = list(map(int, list(str(num))))
            count = 0
            if 0 in nums:
                pass
            else:
                for j in nums:
                    if num%j == 0:
                        count += 1
                if count == len(nums):
                    result.append(num)
        return result
## 参考 Solution
## Rumtime=36ms, Memory=14MB
class Solution:
    def selfDividingNumbers(self, left: int, right: int) -> List[int]:
        result = []
        for num in range(left, right+1):
            for x in str(num):
                if x == "0" or num % int(x) != 0:
                    break
            else:
                result.append(num)
        return result
```

# 796. Rotate String
[题目详情](https://leetcode.com/problems/rotate-string/)
```Python
# Solution
class Solution:
    def rotateString(self, A: str, B: str) -> bool:
        return len(A)==len(B) and B in A+A
```

[题目详情]()
```Python

```

# 832. Flipping an Image
[题目详情](https://leetcode.com/problems/flipping-an-image/)
```Python
## Runtime=56ms, Memory=13.9MB
class Solution:
    def flipAndInvertImage(self, A: List[List[int]]) -> List[List[int]]:
        for i in range(len(A)):
            A[i].reverse()
            A[i] = [1-x for x in A[i]]
        return A
## Runtime=52ms, Memory=14MB
class Solution:
    def flipAndInvertImage(self, A: List[List[int]]) -> List[List[int]]:
        for i in range(len(A)):
            A[i] = A[i][::-1]  ## difference
            A[i] = [1-x for x in A[i]]
        return A
## Runtime=48ms, Memory=14MB
class Solution:
    def flipAndInvertImage(self, A: List[List[int]]) -> List[List[int]]:
        for i, row in enumerate(A):  ## differencee
            A[i] = row[::-1]
            for j, col in enumerate(A[i]):  ## difference
                A[i][j] = 1 - col
        return A
```

# 852. Peak Index in a Mountain Array
[题目详情](https://leetcode.com/problems/peak-index-in-a-mountain-array/)
```Python
## Runtime=84ms, Memory=15.1MB
class Solution:
    def peakIndexInMountainArray(self, A: List[int]) -> int:
        idx = int((len(A)-1)/2 if len(A)%2==1 else len(A)/2)
        left, right = idx-1, idx+1
        while left>=0 and right<len(A):
            if A[idx]>A[left] and A[idx]>A[right]:
                break
            elif A[idx]<A[left]:
                left -= 1
                idx -= 1
                right -= 1
            else:
                left += 1
                idx += 1
                right += 1
        return idx
## Runtime=80ms, Memory=15MB
## Binary search in Solution
class Solution:
    def peakIndexInMountainArray(self, A: List[int]) -> int:
        lo, hi = 0, len(A) - 1
        while lo < hi:
            mi = int((lo + hi) / 2)
            if A[mi] < A[mi + 1]:
                lo = mi + 1
            else:
                hi = mi
        return lo
## Runtime=84ms, Memory=15.2MB
## Linear scan
class Solution:
    def peakIndexInMountainArray(self, A: List[int]) -> int:
        for i in range(len(A)):
            if A[i] > A[i+1]:
                return i
```

# 942. DI String Match
[题目详情](https://leetcode.com/problems/di-string-match/)
```Python
## 参考了 Solution
class Solution:
    def diStringMatch(self, S: str) -> List[int]:
        low, high = 0, len(S)
        result = []
        for s in S:
            if s == 'I':
                result.append(low)
                low += 1
            else:
                result.append(high)
                high -= 1
        return result + [low]
```

# 965. Univalued Binary Tree
[题目详情](https://leetcode.com/problems/univalued-binary-tree/)
```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isUnivalTree(self, root: TreeNode) -> bool:
        values = []
        
        def dfs(node):
            if node:
                values.append(node.val)
                dfs(node.left)
                dfs(node.right)
        
        dfs(root)
        return len(set(values)) == 1
```

# 977. Squares of a Sorted Array
[题目详情](https://leetcode.com/problems/squares-of-a-sorted-array/)
```Python
## Runtime=228ms, Memory=15.7MB
class Solution:
    def sortedSquares(self, A: List[int]) -> List[int]:
         return sorted([x**2 for x in A])
## Runtime=216ms, Memory=15.8MB
class Solution:
    def sortedSquares(self, A: List[int]) -> List[int]:
         return sorted([x*x for x in A])
```

# 1021. Remove Outermost Parentheses
[题目详情](https://leetcode.com/problems/remove-outermost-parentheses/)
```Python
## 参考了solutions in discuss
class Solution:
    def removeOuterParentheses(self, S: str) -> str:
        count, result = 0, []
        for s in S:
            if s == ")":  ##  完整配对count=0；增加一个")"count减一；增加一个"("count加一
                count -= 1
            if count > 0:
                result.append(s)
            if s == "(": 
                count += 1
        return "".join(result)
```


[题目详情]()
```Python

```

# 1108. Defanging an IP Address
[题目详情](https://leetcode.com/problems/defanging-an-ip-address/)
```Python
class Solution:
    def defangIPaddr(self, address: str) -> str:
        return address.replace(".", "[.]")
```

# 1221. Split a String in Balanced Strings
[题目详情](https://leetcode.com/problems/split-a-string-in-balanced-strings/)
```Python
class Solution:
    def balancedStringSplit(self, s: str) -> int:
        ans_count = 0
        L_num, R_num = 0, 0
        for ss in s:
            if ss=="L":
                L_num += 1
            else:
                R_num += 1
            if L_num==R_num:
                ans_count += 1
                L_num, R_num = 0, 0
        return ans_count
```

# 1252. Cells with Odd Values in a Matrix
[题目详情](https://leetcode.com/problems/cells-with-odd-values-in-a-matrix/)
```Python
class Solution:
    def oddCells(self, n: int, m: int, indices: List[List[int]]) -> int:
        nums = [[0]*m]*n  ## 快速创建0矩阵
        rows, cols = [0]*n, [0]*m
        lst = []
        for index in indices:
            ri, ci = index[0], index[1]
            rows[ri] += 1
            cols[ci] += 1
        for i in range(n):
            for j in range(m):
                nums[i][j] = rows[i] + cols[j]
            lst += nums[i]
        return sum([1 if x%2==1 else 0 for x in lst])
```

[题目详情]()
```Python

```

# 1266. Minimum Time Visiting All Points
[题目详情](https://leetcode.com/problems/minimum-time-visiting-all-points/)
```Python
class Solution:
    def minTimeToVisitAllPoints(self, points: List[List[int]]) -> int:
        steps = 0
        for i in range(len(points)-1):
            x1, y1 = points[i][0], points[i][1]
            x2, y2 = points[i+1][0], points[i+1][1]
            m, n = abs(x2 - x1), abs(y2 - y1)
            steps += max(m, n)
        return steps
```


# 1281. Subtract the Product and Sum of Digits of an Integer
[题目详情](https://leetcode.com/problems/subtract-the-product-and-sum-of-digits-of-an-integer/)
```Python
class Solution:
    def subtractProductAndSum(self, n: int) -> int:
        num = [x for x in str(n)]
        num = list(map(int, num))  ## 将int拆开成单个数字列表
        pp = 1
        ss = 0
        for nn in num:
            pp *= nn
            ss += nn
        return pp - ss
```
# 1290. Convert Binary Number in a Linked List to Integer
[题目详情](https://leetcode.com/problems/convert-binary-number-in-a-linked-list-to-integer/)
```Python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def getDecimalValue(self, head: ListNode) -> int:
        str_num = ""
        while head:  ## 当head非空
            str_num += str(head.val)
            head = head.next
        return int(str_num, 2)
```

[题目详情]()
```Python

```

# 1295. Find Numbers with Even Number of Digits
[题目详情](https://leetcode.com/problems/find-numbers-with-even-number-of-digits/)
```Python
class Solution:
    def findNumbers(self, nums: List[int]) -> int:
        ans_count = 0
        for num in nums:
            num_count = 0
            while num:
                num_count += 1
                num //= 10  ## num除以10的商
            if num_count % 2 == 0:  ## num_count除以2的余数等于0
                ans_count += 1
        return ans_count
```

# 1299. Replace Elements with Greatest Element on Right Side
[题目详情](https://leetcode.com/problems/replace-elements-with-greatest-element-on-right-side/)
```Python
## Runtime=4720ms
class Solution:
    def replaceElements(self, arr: List[int]) -> List[int]:
        out = []
        n = len(arr)
        for i in range(n-1):
            out.append(max(arr[(i+1):]))
        out.append(-1)
        return out
## A solution in 'Discuss'
## Runtime=125ms
class Solution:
    def replaceElements(self, arr: List[int]) -> List[int]:
        max_num = -1
        for i in range(len(arr)-1, -1, -1):
            hold = arr[i]
            arr[i] = max_num
            if hold > max_num:
                max_num = hold
        return arr
```

# 1304. Find N Unique Integers Sum up to Zero
[题目详情](https://leetcode.com/problems/find-n-unique-integers-sum-up-to-zero/)
```Python
class Solution:
    def sumZero(self, n: int) -> List[int]:
        result = []
        if n % 2 == 0:
            lst = [x for x in range(n-1)]
            result.extend(lst)
            result.append(-sum(lst))
        else:
            lst = [x for x in range(1,int((n+1)//2))]
            result.extend(lst)
            result.extend([-x for x in lst])
            result.append(0)
        return result
```

# 1309. Decrypt String from Alphabet to Integer Mapping
[题目详情](https://leetcode.com/problems/decrypt-string-from-alphabet-to-integer-mapping/)
```Python
class Solution:
    def freqAlphabets(self, s: str) -> str:
        result = ""
        dic = {}
        for i in range(97,123):
            if i <= 105:
                dic[str(i - 96)] = chr(i)
            else:
                dic[str(i - 96) + '#'] = chr(i)
        i = 0
        while i < len(s):
            if  i+2 < len(s) and s[i+2] == '#':
                key = s[i:i+3]
                i += 3
            else:
                key = s[i]
                i += 1
            result += dic[key]
        return result
```

# 1313. Decompress Run-Length Encoded List
[题目详情](https://leetcode.com/problems/decompress-run-length-encoded-list/)
```Python
class Solution:
    def decompressRLElist(self, nums: List[int]) -> List[int]:
        n = len(nums)
        ans = []
        for i in range(int(n/2)):
            n = nums[2*i]
            num = nums[2*i+1]
            for j in range(n):
                ans.append(num)
        return ans
```

# 1323. Maximum 69 Number
[题目详情](https://leetcode.com/problems/maximum-69-number/)
```Python
class Solution:
    def maximum69Number (self, num: int) -> int:
        i = 0
        out = [x for x in str(num)]
        while i<len(out):
            if out[i] == '6':
                out[i] = '9'
                break
            i += 1
        return int(''.join(out))
```

# 1342. Number of Steps to Reduce a Number to Zero
[题目详情](https://leetcode.com/problems/number-of-steps-to-reduce-a-number-to-zero/)
```Python
class Solution:
    def numberOfSteps (self, num: int) -> int:
        cou = 0
        while num>0:
            if num % 2 == 0:  ## if even
                num = int(num / 2)
            else:  ## if odd
                num -= 1
            cou += 1
        return cou
```

# 1351. Count Negative Numbers in a Sorted Matrix
[题目详情](https://leetcode.com/problems/count-negative-numbers-in-a-sorted-matrix/)
```Python
class Solution:
    def countNegatives(self, grid: List[List[int]]) -> int:
        nums = []
        for i in range(len(grid)):
            nums += grid[i]
        return sum([1 if x<0 else 0 for x in nums])
```

# 1370. Increasing Decreasing String
[题目详情](https://leetcode.com/problems/increasing-decreasing-string/)
```Python
## 参考了 solution in discuss
class Solution:
    def sortString(self, s: str) -> str:
        result = []
        s = [x for x in s]  ## 等价于 s = list(s)
        direct = 1  ## =1表示从小到大；=-1表示从大到小
        while s:  ## 当s非空时
            if direct == 1:
                ss = sorted(set(s))  ## 从小到大
            else: 
                ss = sorted(set(s))[::-1]  ## 从大到小
            result.extend(ss)
            for x in ss:
                s.remove(x)
            direct *= -1  ## 反向
        return "".join(result)
```

# 1374. Generate a String With Characters That Have Odd Counts
[题目详情](https://leetcode.com/problems/generate-a-string-with-characters-that-have-odd-counts/)
```Python
class Solution:
    def generateTheString(self, n: int) -> str:
        if n%2==0:
            return 'a'*(n-1) + 'b'
        else:
            return 'a'*n
```

# 1380. Lucky Numbers in a Matrix
[题目详情](https://leetcode.com/problems/lucky-numbers-in-a-matrix/)
```Python
class Solution:
    def luckyNumbers (self, matrix: List[List[int]]) -> List[int]:
        rows = [min(x) for x in matrix]
        n, m = len(matrix), len(matrix[0])
        cols, ans = [], []
        for j in range(m):
            col = [x[j] for x in matrix]
            cols.append(max(col))
        for i in range(n):
            for j in range(m):
                if matrix[i][j] == rows[i] and matrix[i][j] == cols[j]:
                    ans.append(matrix[i][j])
        return ans
```

# 1389. Create Target Array in the Given Order
[题目详情](https://leetcode.com/problems/create-target-array-in-the-given-order/)
```Python
class Solution:
    def createTargetArray(self, nums: List[int], index: List[int]) -> List[int]:
        ans = []
        for i in range(len(nums)):
            num, idx = nums[i], index[i]
            if idx >= len(ans):
                ans.append(num)
            else:
                a, b = ans[:idx], ans[idx:]
                ans = a + [num] + b
        return ans
```

[题目详情]()
```Python

```

[题目详情]()
```Python

```

[题目详情]()
```Python

```


[题目详情]()
```Python

```

# 推荐阅读
- [Leetcode-Python解题思路](https://www.ranxiaolang.com/static/leetcode_python.pdf)
- [labuladong的算法小抄](https://labuladong.github.io/ebook/)