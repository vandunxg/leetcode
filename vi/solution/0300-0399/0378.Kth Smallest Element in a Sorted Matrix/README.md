---
comments: true
difficulty: Medium
tags:
    - Array
    - Binary Search
    - Matrix
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [378. Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix)

[中文文档](/solution/0300-0399/0378.Kth%20Smallest%20Element%20in%20a%20Sorted%20Matrix/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ma trận <code>n x n</code> <code>matrix</code>, trong đó mỗi hàng và mỗi cột đều được sắp xếp theo thứ tự tăng dần. Hãy trả về <em>phần tử nhỏ thứ</em> <code>k<sup>th</sup></code> <em>trong ma trận</em>.</p>

<p>Lưu ý, đây là phần tử nhỏ thứ <code>k<sup>th</sup></code> <strong>theo thứ tự sắp xếp</strong>, không phải phần tử <strong>khác biệt</strong> thứ <code>k<sup>th</sup></code>.</p>

<p>Bạn phải tìm lời giải có độ phức tạp không gian nhỏ hơn <code>O(n<sup>2</sup>)</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> matrix = [[1,5,9],[10,11,13],[12,13,15]], k = 8
<strong>Đầu ra:</strong> 13
<strong>Giải thích:</strong> Các phần tử trong ma trận là [1,5,9,10,11,12,13,<u><strong>13</strong></u>,15], và số nhỏ thứ 8 là 13
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> matrix = [[-5]], k = 1
<strong>Đầu ra:</strong> -5
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == matrix.length == matrix[i].length</code></li>
	<li><code>1 &lt;= n &lt;= 300</code></li>
	<li><code>-10<sup>9</sup> &lt;= matrix[i][j] &lt;= 10<sup>9</sup></code></li>
	<li>Đảm bảo tất cả hàng và cột của <code>matrix</code> đều được sắp xếp theo <strong>thứ tự không giảm</strong>.</li>
	<li><code>1 &lt;= k &lt;= n<sup>2</sup></code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong></p>

<ul>
	<li>Bạn có thể giải bài này với bộ nhớ hằng số (tức độ phức tạp không gian <code>O(1)</code>) không?</li>
	<li>Bạn có thể giải bài này với độ phức tạp thời gian <code>O(n)</code> không? Lời giải có thể hơi nâng cao cho phỏng vấn, nhưng bạn có thể thấy thú vị khi đọc <a href="http://www.cse.yorku.ca/~andy/pubs/X+Y.pdf" target="_blank">bài báo này</a>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các hàng và cột đều tăng dần; cần tìm phần tử nhỏ thứ $k$. Chuyển ma trận thành mảng rồi sắp xếp tốn $O(n^2\log n)$. Đáp án nằm trong đoạn $[matrix[0][0], matrix[n-1][n-1]]$, nên ta tìm kiếm nhị phân trên giá trị.
>
> `check(mid)` đếm số phần tử $\le mid$ trong $O(n)$, bắt đầu từ góc dưới bên trái và tận dụng tính đơn điệu. Nếu số lượng đếm được $\ge k$, thu hẹp cận phải. Giá trị $mid$ nhỏ nhất thỏa điều kiện chính là đáp án.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def kthSmallest(self, matrix: List[List[int]], k: int) -> int:
        def check(matrix, mid, k, n):
            count = 0
            i, j = n - 1, 0
            while i >= 0 and j < n:
                if matrix[i][j] <= mid:
                    count += i + 1
                    j += 1
                else:
                    i -= 1
            return count >= k

        n = len(matrix)
        left, right = matrix[0][0], matrix[n - 1][n - 1]
        while left < right:
            mid = (left + right) >> 1
            if check(matrix, mid, k, n):
                right = mid
            else:
                left = mid + 1
        return left
```

#### Java

```java
class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        int n = matrix.length;
        int left = matrix[0][0], right = matrix[n - 1][n - 1];
        while (left < right) {
            int mid = (left + right) >>> 1;
            if (check(matrix, mid, k, n)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }

    private boolean check(int[][] matrix, int mid, int k, int n) {
        int count = 0;
        int i = n - 1, j = 0;
        while (i >= 0 && j < n) {
            if (matrix[i][j] <= mid) {
                count += (i + 1);
                ++j;
            } else {
                --i;
            }
        }
        return count >= k;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int kthSmallest(vector<vector<int>>& matrix, int k) {
        int n = matrix.size();
        int left = matrix[0][0], right = matrix[n - 1][n - 1];
        while (left < right) {
            int mid = left + right >> 1;
            if (check(matrix, mid, k, n)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }

private:
    bool check(vector<vector<int>>& matrix, int mid, int k, int n) {
        int count = 0;
        int i = n - 1, j = 0;
        while (i >= 0 && j < n) {
            if (matrix[i][j] <= mid) {
                count += (i + 1);
                ++j;
            } else {
                --i;
            }
        }
        return count >= k;
    }
};
```

#### Go

```go
func kthSmallest(matrix [][]int, k int) int {
	n := len(matrix)
	left, right := matrix[0][0], matrix[n-1][n-1]
	for left < right {
		mid := (left + right) >> 1
		if check(matrix, mid, k, n) {
			right = mid
		} else {
			left = mid + 1
		}
	}
	return left
}

func check(matrix [][]int, mid, k, n int) bool {
	count := 0
	i, j := n-1, 0
	for i >= 0 && j < n {
		if matrix[i][j] <= mid {
			count += (i + 1)
			j++
		} else {
			i--
		}
	}
	return count >= k
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
