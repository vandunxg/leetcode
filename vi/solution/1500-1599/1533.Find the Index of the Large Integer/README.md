---
comments: true
difficulty: Medium
tags:
    - Array
    - Binary Search
    - Interactive
---

<!-- problem:start -->

# [1533. Find the Index of the Large Integer 🔒](https://leetcode.com/problems/find-the-index-of-the-large-integer)

[中文文档](/solution/1500-1599/1533.Find%20the%20Index%20of%20the%20Large%20Integer/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>arr</code>, trong đó mọi số nguyên trong <code>arr</code> đều bằng nhau ngoại trừ một số <strong>lớn hơn</strong> các số còn lại. Bạn không được truy cập trực tiếp vào mảng, thay vào đó có một <strong>API</strong> <code>ArrayReader</code> với các hàm sau:</p>

<ul>
	<li><code>int compareSub(int l, int r, int x, int y)</code>: where <code>0 &lt;= l, r, x, y &lt; ArrayReader.length()</code>, <code>l &lt;= r and</code> <code>x &lt;= y</code>. The function compares the sum of sub-array <code>arr[l..r]</code> with the sum of the sub-array <code>arr[x..y]</code> and returns:

    <ul>
    	<li><strong>1</strong> if <code>arr[l]+arr[l+1]+...+arr[r] &gt; arr[x]+arr[x+1]+...+arr[y]</code>.</li>
    	<li><strong>0</strong> if <code>arr[l]+arr[l+1]+...+arr[r] == arr[x]+arr[x+1]+...+arr[y]</code>.</li>
    	<li><strong>-1</strong> if <code>arr[l]+arr[l+1]+...+arr[r] &lt; arr[x]+arr[x+1]+...+arr[y]</code>.</li>
    </ul>
    </li>
    <li><code>int length()</code>: Returns the size of the array.</li>

</ul>

<p>Bạn được phép gọi <code>compareSub()</code> nhiều nhất <b>20 lần</b>. Có thể giả sử cả hai hàm chạy trong thời gian <code>O(1)</code>.</p>

<p>Trả về <em>chỉ số của phần tử lớn nhất trong mảng <code>arr</code></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [7,7,7,7,10,7,7,7]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Các lần gọi API sau
reader.compareSub(0, 0, 1, 1) // returns 0 this is a query comparing the sub-array (0, 0) with the sub array (1, 1), (i.e. compares arr[0] with arr[1]).
Thus we know that arr[0] and arr[1] doesn&#39;t contain the largest element.
reader.compareSub(2, 2, 3, 3) // returns 0, we can exclude arr[2] and arr[3].
reader.compareSub(4, 4, 5, 5) // returns 1, thus for sure arr[4] is the largest element in the array.
Notice that we made only 3 calls, so the answer is valid.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [6,6,12]
<strong>Đầu ra:</strong> 2
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= arr.length &lt;= 5 * 10<sup>5</sup></code></li>
	<li><code>1 &lt;= arr[i] &lt;= 100</code></li>
	<li>All elements of <code>arr</code> are equal except for one element which is larger than all other elements.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong></p>

<ul>
	<li>What if there are two numbers in <code>arr</code> that are bigger than all other numbers?</li>
	<li>What if there is one number that is bigger than other numbers and one number that is smaller than other numbers?</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tất cả phần tử ngoại trừ một giá trị lớn hơn đều bằng nhau. Ta chỉ có thể so sánh tổng của hai đoạn, trong khi độ dài có thể là $5\times 10^5$, nên phải thu hẹp khoảng theo cấp số logarit.
>
> Chia đoạn hiện tại thành ba phần gần bằng nhau và so sánh $[t_1,t_2]$ với $[t_2+1,t_3]$. Nếu hai tổng bằng nhau, giá trị lớn nằm ở phần ba bên phải còn lại; nếu tổng trái lớn hơn, nó nằm bên trái; ngược lại nó nằm ở giữa. Mỗi bước loại khoảng một phần ba cho đến khi hai đầu gặp nhau.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
# """
# This is ArrayReader's API interface.
# You should not implement it, or speculate about its implementation
# """
# class ArrayReader(object):
# 	 # Compares the sum of arr[l..r] with the sum of arr[x..y]
# 	 # return 1 if sum(arr[l..r]) > sum(arr[x..y])
# 	 # return 0 if sum(arr[l..r]) == sum(arr[x..y])
# 	 # return -1 if sum(arr[l..r]) < sum(arr[x..y])
#    def compareSub(self, l: int, r: int, x: int, y: int) -> int:
#
# 	 # Returns the length of the array
#    def length(self) -> int:
#


class Solution:
    def getIndex(self, reader: 'ArrayReader') -> int:
        left, right = 0, reader.length() - 1
        while left < right:
            t1, t2, t3 = (
                left,
                left + (right - left) // 3,
                left + ((right - left) // 3) * 2 + 1,
            )
            cmp = reader.compareSub(t1, t2, t2 + 1, t3)
            if cmp == 0:
                left = t3 + 1
            elif cmp == 1:
                right = t2
            else:
                left, right = t2 + 1, t3
        return left
```

#### Java

```java
/**
 * // This is ArrayReader's API interface.
 * // You should not implement it, or speculate about its implementation
 * interface ArrayReader {
 *     // Compares the sum of arr[l..r] with the sum of arr[x..y]
 *     // return 1 if sum(arr[l..r]) > sum(arr[x..y])
 *     // return 0 if sum(arr[l..r]) == sum(arr[x..y])
 *     // return -1 if sum(arr[l..r]) < sum(arr[x..y])
 *     public int compareSub(int l, int r, int x, int y) {}
 *
 *     // Returns the length of the array
 *     public int length() {}
 * }
 */

class Solution {
    public int getIndex(ArrayReader reader) {
        int left = 0, right = reader.length() - 1;
        while (left < right) {
            int t1 = left, t2 = left + (right - left) / 3, t3 = left + (right - left) / 3 * 2 + 1;
            int cmp = reader.compareSub(t1, t2, t2 + 1, t3);
            if (cmp == 0) {
                left = t3 + 1;
            } else if (cmp == 1) {
                right = t2;
            } else {
                left = t2 + 1;
                right = t3;
            }
        }
        return left;
    }
}
```

#### C++

```cpp
/**
 * // This is the ArrayReader's API interface.
 * // You should not implement it, or speculate about its implementation
 * class ArrayReader {
 *   public:
 *     // Compares the sum of arr[l..r] with the sum of arr[x..y]
 *     // return 1 if sum(arr[l..r]) > sum(arr[x..y])
 *     // return 0 if sum(arr[l..r]) == sum(arr[x..y])
 *     // return -1 if sum(arr[l..r]) < sum(arr[x..y])
 *     int compareSub(int l, int r, int x, int y);
 *
 *     // Returns the length of the array
 *     int length();
 * };
 */

class Solution {
public:
    int getIndex(ArrayReader& reader) {
        int left = 0, right = reader.length() - 1;
        while (left < right) {
            int t1 = left, t2 = left + (right - left) / 3, t3 = left + (right - left) / 3 * 2 + 1;
            int cmp = reader.compareSub(t1, t2, t2 + 1, t3);
            if (cmp == 0) {
                left = t3 + 1;
            } else if (cmp == 1) {
                right = t2;
            } else {
                left = t2 + 1;
                right = t3;
            }
        }
        return left;
    }
};
```

#### Go

```go
/**
 * // This is the ArrayReader's API interface.
 * // You should not implement it, or speculate about its implementation
 * type ArrayReader struct {
 * }
 * // Compares the sum of arr[l..r] with the sum of arr[x..y]
 * // return 1 if sum(arr[l..r]) > sum(arr[x..y])
 * // return 0 if sum(arr[l..r]) == sum(arr[x..y])
 * // return -1 if sum(arr[l..r]) < sum(arr[x..y])
 * func (this *ArrayReader) compareSub(l, r, x, y int) int {}
 *
 * // Returns the length of the array
 * func (this *ArrayReader) length() int {}
 */

func getIndex(reader *ArrayReader) int {
	left, right := 0, reader.length()-1
	for left < right {
		t1, t2, t3 := left, left+(right-left)/3, left+(right-left)/3*2+1
		cmp := reader.compareSub(t1, t2, t2+1, t3)
		if cmp == 0 {
			left = t3 + 1
		} else if cmp == 1 {
			right = t2
		} else {
			left, right = t2+1, t3
		}
	}
	return left
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
