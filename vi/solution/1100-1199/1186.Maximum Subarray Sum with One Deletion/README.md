---
comments: true
difficulty: Medium
rating: 1799
source: Weekly Contest 153 Q3
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [1186. Maximum Subarray Sum with One Deletion](https://leetcode.com/problems/maximum-subarray-sum-with-one-deletion)

[中文文档](/solution/1100-1199/1186.Maximum%20Subarray%20Sum%20with%20One%20Deletion/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên, hãy trả về tổng lớn nhất của một mảng con <strong>không rỗng</strong> (các phần tử liên tiếp) khi được phép xóa tối đa một phần tử. Nói cách khác, hãy chọn một mảng con và có thể xóa một phần tử khỏi đó, sao cho vẫn còn ít nhất một phần tử và tổng các phần tử còn lại lớn nhất có thể.</p>

<p>Lưu ý rằng sau khi xóa một phần tử, mảng con vẫn phải <strong>không rỗng</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,-2,0,3]
<strong>Đầu ra:</strong> 4
<strong>Giải thích: </strong>Ta có thể chọn [1, -2, 0, 3] và bỏ -2, khi đó mảng con [1, 0, 3] có tổng lớn nhất.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,-2,-2,3]
<strong>Đầu ra:</strong> 3
<strong>Giải thích: </strong>Ta chỉ cần chọn [3], đây là mảng con có tổng lớn nhất.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [-1,-1,-1,-1]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong>&nbsp;Mảng con cuối cùng phải không rỗng. Không thể chọn [-1] rồi xóa -1 để tạo thành mảng con rỗng có tổng bằng 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>4</sup> &lt;= arr[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Ta được phép bỏ tối đa một phần tử khỏi mảng con. Chạy Kadane từ hai đầu sẽ cho tổng lớn nhất của đoạn kết thúc hoặc bắt đầu tại mỗi chỉ số; nếu không xóa phần tử nào, đáp án là giá trị lớn nhất trong các tổng này. Nếu xóa $arr[i]$, cộng tổng tốt nhất ở bên trái kết thúc tại $i-1$ với tổng tốt nhất ở bên phải bắt đầu tại $i+1$. Sau khi tính xong $left$ và $right$, liệt kê các vị trí ở giữa có thể xóa.

<!-- thinking:end -->

Ta có thể tiền xử lý mảng $\textit{arr}$ để tìm tổng mảng con lớn nhất kết thúc và bắt đầu tại mỗi phần tử, rồi lưu lần lượt vào các mảng $\textit{left}$ và $\textit{right}$.

Nếu không xóa phần tử nào, tổng mảng con lớn nhất là giá trị lớn nhất trong $\textit{left}[i]$ hoặc $\textit{right}[i]$. Nếu xóa một phần tử, ta có thể duyệt từng vị trí $i$ trong $[1..n-2]$, tính $\textit{left}[i-1] + \textit{right}[i+1]$ rồi lấy giá trị lớn nhất.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{arr}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumSum(self, arr: List[int]) -> int:
        n = len(arr)
        left = [0] * n
        right = [0] * n
        s = 0
        for i, x in enumerate(arr):
            s = max(s, 0) + x
            left[i] = s
        s = 0
        for i in range(n - 1, -1, -1):
            s = max(s, 0) + arr[i]
            right[i] = s
        ans = max(left)
        for i in range(1, n - 1):
            ans = max(ans, left[i - 1] + right[i + 1])
        return ans
```

#### Java

```java
class Solution {
    public int maximumSum(int[] arr) {
        int n = arr.length;
        int[] left = new int[n];
        int[] right = new int[n];
        int ans = -(1 << 30);
        for (int i = 0, s = 0; i < n; ++i) {
            s = Math.max(s, 0) + arr[i];
            left[i] = s;
            ans = Math.max(ans, left[i]);
        }
        for (int i = n - 1, s = 0; i >= 0; --i) {
            s = Math.max(s, 0) + arr[i];
            right[i] = s;
        }
        for (int i = 1; i < n - 1; ++i) {
            ans = Math.max(ans, left[i - 1] + right[i + 1]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumSum(vector<int>& arr) {
        int n = arr.size();
        int left[n];
        int right[n];
        for (int i = 0, s = 0; i < n; ++i) {
            s = max(s, 0) + arr[i];
            left[i] = s;
        }
        for (int i = n - 1, s = 0; ~i; --i) {
            s = max(s, 0) + arr[i];
            right[i] = s;
        }
        int ans = *max_element(left, left + n);
        for (int i = 1; i < n - 1; ++i) {
            ans = max(ans, left[i - 1] + right[i + 1]);
        }
        return ans;
    }
};
```

#### Go

```go
func maximumSum(arr []int) int {
	n := len(arr)
	left := make([]int, n)
	right := make([]int, n)
	for i, s := 0, 0; i < n; i++ {
		s = max(s, 0) + arr[i]
		left[i] = s
	}
	for i, s := n-1, 0; i >= 0; i-- {
		s = max(s, 0) + arr[i]
		right[i] = s
	}
	ans := slices.Max(left)
	for i := 1; i < n-1; i++ {
		ans = max(ans, left[i-1]+right[i+1])
	}
	return ans
}
```

#### TypeScript

```ts
function maximumSum(arr: number[]): number {
    const n = arr.length;
    const left: number[] = Array(n).fill(0);
    const right: number[] = Array(n).fill(0);
    for (let i = 0, s = 0; i < n; ++i) {
        s = Math.max(s, 0) + arr[i];
        left[i] = s;
    }
    for (let i = n - 1, s = 0; i >= 0; --i) {
        s = Math.max(s, 0) + arr[i];
        right[i] = s;
    }
    let ans = Math.max(...left);
    for (let i = 1; i < n - 1; ++i) {
        ans = Math.max(ans, left[i - 1] + right[i + 1]);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn maximum_sum(arr: Vec<i32>) -> i32 {
        let n = arr.len();
        let mut left = vec![0; n];
        let mut right = vec![0; n];
        let mut s = 0;
        for i in 0..n {
            s = (s.max(0)) + arr[i];
            left[i] = s;
        }
        s = 0;
        for i in (0..n).rev() {
            s = (s.max(0)) + arr[i];
            right[i] = s;
        }
        let mut ans = *left.iter().max().unwrap();
        for i in 1..n - 1 {
            ans = ans.max(left[i - 1] + right[i + 1]);
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
