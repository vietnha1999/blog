---
title: "[Leetcode] Diagonal Traverse II (22/11/2023)"
datePublished: Wed Nov 22 2023 17:14:26 GMT+0000 (Coordinated Universal Time)
cuid: clpa0yauv00000ajrdn8nagg9
slug: leetcode-diagonal-traverse-ii-22112023

---

Hôm nay là ngày thứ 2 mình bắt đầu làm lại leetcode sau gần nửa năm bỏ bê. Nhưng là ngày đầu tiên mình thực sự viết về những gì mình suy nghĩ.

Với công việc mình đang làm và kinh nghiệm ít ỏi mình đang có thì làm leetcode cũng không để làm gì nhưng vì có vài chuyện buồn nên làm thôi.

May quá, mình chỉ gặp một bài medium, đề bài như sau:

Given a 2D integer array `nums`, return *all elements of* `nums` *in diagonal order as shown in the below images*.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/04/08/sample_1_1784.png align="left")

```java
Input: nums = [[1,2,3],[4,5,6],[7,8,9]]
Output: [1,4,2,7,5,3,8,6,9]
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/04/08/sample_2_1784.png align="left")

```java
Input: nums = [[1,2,3,4,5],[6,7],[8],[9,10,11],[12,13,14,15,16]]
Output: [1,6,2,8,7,3,9,4,12,10,5,13,11,14,15,16]
```

**Constraints:**

* `1 <= nums.length <= 10<sup>5</sup>`
    
* `1 <= nums[i].length <= 10<sup>5</sup>`
    
* `1 <= sum(nums[i].length) <= 10<sup>5</sup>`
    
* `1 <= nums[i][j] <= 10<sup>5</sup>`
    

```java
class Solution {
    public int[] findDiagonalOrder(List<List<Integer>> nums) {
        
    }
}
```

Đề bài ngắn nhỉ, mình thích đề bài ngắn =)) Tiếng Anh của mình không tốt lắm.  
Nhìn qua thì đề yêu cầu một cách duyệt đặc biệt nào đó, với một mảng 2 chiều, để nó đi theo đường chéo chứ không phải như thông thường: đi từng hàng một, mỗi hàng lại lần lượt từng cột.

Theo thói quen, mình soi phần hạn chế của đề trước để lấy gợi ý về độ phức tạp thời gian.  
Hơi buồn cười là khi, trong một lần phỏng vấn công ty đầu tiên, mình tự nhận tự tin phần CTDL & GT nhất. À mà đúng, phần khác không tự tin bằng chứ không phải mình giỏi CTDL & GT :v.  
Không lan man nữa, vì nums.length &lt;= 10^5 và nums\[i\].length cũng vậy nên giải pháp chấp nhận được phải có độ phức tạp O(n). Thực ra, bây giờ mình mới biết nhận xét này là sai, số lượng phần tử phải là sum(nums\[i\].length).

Mình bắt đầu để ý xem các phần tử cùng đường chéo thì có gì đặc biệt. Xét ví dụ 1 đi:

![](https://assets.leetcode.com/uploads/2020/04/08/sample_1_1784.png align="left")

1 nằm ở hàng 0, vị trí thứ 0  
4 nằm ở hàng 1, vị trí thứ 0  
2 nằm ở hàng 0, vị trí thứ 1  
7 nằm ở hàng 2, vị trí thứ 0  
...

Ra là các phần tử cùng đường chéo thì chỉ số hàng + vị trí của nó trong hàng sẽ bằng nhau. Nếu vậy thì mỗi phần tử chỉ cần tính con số này là biết vị trí nó thuộc đường chéo nào.

```java
int levelAtFirstId = 0;
level = 0;
for (var list: nums) {
    level = levelAtFirstId;
    for (var e: list) {
        // System.out.println(e + " e");
        listElementAtLevel[level].add(e);
        level++;
    }
    levelAtFirstId++;
}
```

Để trả về kết quả thì mình duyệt đường chéo từ "thấp nhất" và thêm vào mảng kết quả là xong. Tuy nhiên, khi test, mình nhận được thứ tự các phần tử cùng một đường chéo bị ngược so với mong đợi. Vì vậy, mình sửa lại là duyệt từ cuối mảng về đầu mảng là xong:

```java
int[] res = new int[totalElement];
int id = 0;
for (int i = 0; i < maxLevel; i++) {
    var list = listElementAtLevel[i];
    for (int j = list.size() - 1; j >= 0; j--) {
        res[id] = list.get(j);
        id++;
    }
}

return res;
```