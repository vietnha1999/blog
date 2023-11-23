---
title: "[Leetcode] Arithmetic Subarrays (23/11/2023)"
datePublished: Thu Nov 23 2023 09:24:10 GMT+0000 (Coordinated Universal Time)
cuid: clpazle66000k09jvg17lb38q
slug: leetcode-arithmetic-subarrays-23112023

---

Một bài medium nữa, không phải hard. Mong đầu xuôi đuôi lọt.  
Hôm nay đề rất dài:

A sequence of numbers is called **arithmetic** if it consists of at least two elements, and the difference between every two consecutive elements is the same. More formally, a sequence `s` is arithmetic if and only if `s[i+1] - s[i] == s[1] - s[0]` for all valid `i`.

For example, these are **arithmetic** sequences:

```java
1, 3, 5, 7, 9
7, 7, 7, 7
3, -1, -5, -9
```

The following sequence is not **arithmetic**:

```java
1, 1, 2, 5, 7
```

You are given an array of `n` integers, `nums`, and two arrays of `m` integers each, `l` and `r`, representing the `m` range queries, where the `i-th` the query is the range `[l[i], r[i]]`. All the arrays are **0-indexed**.

Return *a list of* `boolean` *elements* `answer`*, where* `answer[i]` *is* `true` *if the subarray* `nums[l[i]], nums[l[i]+1], ..., nums[r[i]]` *can be* ***rearranged*** *to form an* ***arithmetic*** *sequence, and* `false` *otherwise.*

**Example 1:**

```java
Input: nums = [4,6,5,9,3,7], l = [0,0,2], r = [2,3,5]
Output: [true,false,true]
Explanation:
In the 0th query, the subarray is [4,6,5]. This can be rearranged as [6,5,4], which is an arithmetic sequence.
In the 1st query, the subarray is [4,6,5,9]. This cannot be rearranged as an arithmetic sequence.
In the 2nd query, the subarray is [5,9,3,7]. This can be rearranged as [3,5,7,9], which is an arithmetic sequence.
```

**Example 2:**

```java
Input: nums = [-12,-9,-3,-12,-6,15,20,-25,-20,-15,-10], l = [0,1,6,4,8,7], r = [4,4,9,7,9,10]
Output: [false,true,false,false,true,true]
```

**Constraints:**

* `n == nums.length`
    
* `m == l.length`
    
* `m == r.length`
    
* `2 <= n <= 500`
    
* `1 <= m <= 500`
    
* `0 <= l[i] < r[i] < n`
    
* `-10^5 <= nums[i] <= 10^5`
    

```java
class Solution {
    public List<Boolean> checkArithmeticSubarrays(int[] nums, int[] l, int[] r) {
        
    }
}
```

Không có ảnh nhưng có dấu mũ (hashnode không hiển thị được dấu mũ, không rõ tại sao), nên sao chép đề bài vẫn lâu.

Cứ nghĩ cách ngây thơ trước, trâu bò trước.

* Duyệt qua mảng l,r trước (m vòng lặp)
    
* Với mỗi l\[i\], r\[i\], kiểm tra xem các số từ l\[i\] -&gt; r\[i\] có thỏa mãn điều kiện đề yêu cầu hay không.
    
    Vẫn là ngây thơ nhất, thì cứ sort lại các số từ l\[i\] -&gt; r\[i\] rồi kiểm tra khoảng cách giữa hai phần tử liên tiếp
    

Độ phức tạp thời gian là O(m\*(n\*logn + n)) ~ O(m\*n\*logn)  
Nhưng mình lại nghĩ nhiều, cứ nghĩ cách nào đó chỉ cần sort một lần thôi, thế là mất kha khá thời gian.

```java
 int[][] a = new int[n][]; // original index and num
 for (int i = 0; i < n; i++) {
     a[i] = new int[] {
         i,
         nums[i]
     };
 }
 Arrays.sort(a, (int[] x, int[] y) -> Integer.compare(x[1], y[1]));
```

Mình sắp xếp lại nums và giữ lại cả chỉ mục ban đầu lẫn giá trị của nó. Cách này mình hay dùng khi làm leetcode.

Trong mỗi vòng lặp khi duyệt mảng l,r, mình duyệt mảng a để tìm số nhỏ nhất trong khoảng l\[i\] -&gt; r\[i\]:

```java
int j = 0;
while (a[j][0] < li || a[j][0] > ri) { // ngoai khoang xet
    j++;
}
int firstNum = a[j][1];
```

Làm tương tự để tìm số nhỏ thứ hai.  
Với các số còn lại trong mảng a, số nào nằm trong khoảng xét mà có hiệu với số nhỏ hơn nó trước đó khác với hiệu hai số nhỏ nhất thì mảng con này không phải là **arithmetic.**

```java
boolean isArithmetic = true;
while (j < n) {
    if (a[j][0] >= li && a[j][0] <= ri) {
        if (a[j][1] - lastNum != diff) {
            isArithmetic = false;
        }
        lastNum = a[j][1];
    }
    j++;
}
ans.add(isArithmetic);
```

Độ phức tạp thời gian của cách này là O(nlogn + m\*n).

Nhưng cách này mình chỉ đánh bại 33,52%.  
Sau khi xem Hint thì biết là cách họ gợi ý giống với cách ngây thơ mình nghĩ ban đầu. Làm lại thôi.  
Kỳ lạ, cách ngây thơ đánh bại 94,51%. Chắc là do cách O(n\*logn + m\*n) luôn phải duyệt cả mảng nums ban đầu nên chậm hơn.

Bài này còn có một cách nhanh hơn (mình xem lời giải) là lưu lại nums\[i\] trong khoảng từ min(nums\[i\]) cho tới max(nums\[i\]) và tính trước khoảng cách mong đợi giữa hai phần tử nhờ một công thức nào đó. Mình chưa làm cách này, chỉ hiểu qua loa giải pháp vậy thôi.