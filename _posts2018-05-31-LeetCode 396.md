---
title: LeetCode 396 Rotate Function
date: 2018-05-31
tag: LeetCode
category: Amazon
---
## Discription
Given an array of integers A and let n to be its length.  

Assume Bk to be an array obtained by rotating the array A k positions clock-wise, we define a "rotation function" F on A as follow:  

$$ F(k) = 0 * Bk[0] + 1 * Bk[1] + ... + (n-1) * Bk[n-1]. $$

Calculate the maximum value of F(0), F(1), ..., F(n-1).

Note:
n is guaranteed to be less than 105.

Example:

A = [4, 3, 2, 6]  

F(0) = (0 * 4) + (1 * 3) + (2 * 2) + (3 * 6) = 0 + 3 + 4 + 18 = 25   
F(1) = (0 * 6) + (1 * 4) + (2 * 3) + (3 * 2) = 0 + 4 + 6 + 6 = 16    
F(2) = (0 * 2) + (1 * 6) + (2 * 4) + (3 * 3) = 0 + 6 + 8 + 9 = 23   
F(3) = (0 * 3) + (1 * 2) + (2 * 6) + (3 * 4) = 0 + 2 + 12 + 12 = 26   

So the maximum value of F(0), F(1), F(2), F(3) is F(3) = 26.  

## Solution 1: Brute Force

Time: O($n^2$)
Space: O(1)

```java
public int maxRotateFunction(int[] A) {
    if(A == null || A.length == 0) {
        return 0;
    }
    int result = Integer.MIN_VALUE;
    for (int k = 0; k < A.length; k++) {
        int cur = 0;
        for (int j = 0; j < A.length; j++) {
            cur += j * A[(j + k) % A.length];
        }
        result = Math.max(result, cur);
    }
    return result;
}
```
## Solution 2: Math
F(0) = (0 * 4) + (1 * 3) + (2 * 2) + (3 * 6)   
F(1) = (0 * 6) + (1 * 4) + (2 * 3) + (3 * 2) = F(0) + 4 + 3 + 2 - 3 * 6 = F(0) + Sum - 4 * 6  
F(2) = (0 * 2) + (1 * 6) + (2 * 4) + (3 * 3) = F(1) + Sum - 4 * 2  
F(3) = (0 * 3) + (1 * 2) + (2 * 6) + (3 * 4) = F(2) + Sum - 4 * 3

F(i) = F(i - 1) + Sum - n * A[n - i]  

Time: O(n)  
Space: O(1) 
```java
public int maxRotateFunction(int[] A) {
    if(A == null || A.length == 0) {
        return 0;
    }
    int len = A.length;
    int sum = 0;
    int f = 0;
    for(int i = 0; i < len; i++) {
        sum += A[i];
        f += i * A[i];
    }
    int result = f;
    for (int k = 1; k < len; k++) {
        f += sum - len * A[len - k];
        result = Math.max(result, f);
    }
    return result;
}
```
