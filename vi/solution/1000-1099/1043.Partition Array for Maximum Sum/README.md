---
comments: true
difficulty: Medium
rating: 1916
source: Weekly Contest 136 Q3
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [1043. Partition Array for Maximum Sum](https://leetcode.com/problems/partition-array-for-maximum-sum)

[中文文档](/solution/1000-1099/1043.Partition%20Array%20for%20Maximum%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>arr</code>, hãy chia mảng thành các mảng con liên tiếp có độ dài <strong>tối đa</strong> <code>k</code>. Sau khi chia, thay mọi giá trị trong mỗi mảng con bằng giá trị lớn nhất của mảng con đó.</p>

<p>Trả về <em>tổng lớn nhất của mảng sau khi chia. Các test case được tạo sao cho đáp án nằm trong phạm vi số nguyên <strong>32 bit</strong>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,15,7,9,2,5,10], k = 3
<strong>Đầu ra:</strong> 84
<strong>Giải thích:</strong> arr trở thành [15,15,15,9,10,10,10]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,4,1,5,7,3,6,1,9,9,3], k = 4
<strong>Đầu ra:</strong> 83
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1], k = 1
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 500</code></li>
	<li><code>0 &lt;= arr[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= arr.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Số cách chia tăng theo hàm mũ; với $n\le 500$ và $k\le n$, không thể liệt kê tất cả. Đoạn cuối có độ dài tối đa $k$ và đóng góp giá trị lớn nhất của đoạn nhân với độ dài đó; phần prefix còn lại là bài toán tương tự trên mảng nhỏ hơn.
>
> $f[i]$ là tổng lớn nhất có thể đạt được từ $i$ phần tử đầu tiên. Khi duyệt $j$ lùi từ $i$, ta duy trì giá trị lớn nhất $\textit{mx}$ của đoạn đang xét và thử $f[j-1]+\textit{mx}\cdot(i-j+1)$.
>
> Tính lần lượt $i$ theo thứ tự tăng dần sẽ cho kết quả $f[n]$.

<!-- thinking:end -->

Ta định nghĩa $f[i]$ là tổng lớn nhất của $i$ phần tử đầu tiên sau khi chia chúng thành các mảng con. Ban đầu, $f[i]=0$, và đáp án là $f[n]$.

Ta xét cách tính $f[i]$ với $i \geq 1$.

Xét $f[i]$, phần tử cuối là $arr[i-1]$. Vì độ dài mỗi mảng con tối đa là $k$ và ta cần tìm giá trị lớn nhất trong mảng con đó, ta có thể duyệt từ phải sang trái để chọn phần tử đầu $arr[j - 1]$ của mảng con cuối, với $\max(0, i - k) \lt j \leq i$. Trong quá trình duyệt, duy trì biến $mx$ biểu diễn giá trị lớn nhất của mảng con. Công thức chuyển trạng thái là:

$$
f[i] = \max\{f[i], f[j - 1] + mx \times (i - j + 1)\}
$$

Đáp án cuối cùng là $f[n]$.

Độ phức tạp thời gian là $O(n \times k)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $arr$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSumAfterPartitioning(self, arr: List[int], k: int) -> int:
        n = len(arr)
        f = [0] * (n + 1)
        for i in range(1, n + 1):
            mx = 0
            for j in range(i, max(0, i - k), -1):
                mx = max(mx, arr[j - 1])
                f[i] = max(f[i], f[j - 1] + mx * (i - j + 1))
        return f[n]
```

#### Java

```java
class Solution {
    public int maxSumAfterPartitioning(int[] arr, int k) {
        int n = arr.length;
        int[] f = new int[n + 1];
        for (int i = 1; i <= n; ++i) {
            int mx = 0;
            for (int j = i; j > Math.max(0, i - k); --j) {
                mx = Math.max(mx, arr[j - 1]);
                f[i] = Math.max(f[i], f[j - 1] + mx * (i - j + 1));
            }
        }
        return f[n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxSumAfterPartitioning(vector<int>& arr, int k) {
        int n = arr.size();
        int f[n + 1];
        memset(f, 0, sizeof(f));
        for (int i = 1; i <= n; ++i) {
            int mx = 0;
            for (int j = i; j > max(0, i - k); --j) {
                mx = max(mx, arr[j - 1]);
                f[i] = max(f[i], f[j - 1] + mx * (i - j + 1));
            }
        }
        return f[n];
    }
};
```

#### Go

```go
func maxSumAfterPartitioning(arr []int, k int) int {
	n := len(arr)
	f := make([]int, n+1)
	for i := 1; i <= n; i++ {
		mx := 0
		for j := i; j > max(0, i-k); j-- {
			mx = max(mx, arr[j-1])
			f[i] = max(f[i], f[j-1]+mx*(i-j+1))
		}
	}
	return f[n]
}
```

#### TypeScript

```ts
function maxSumAfterPartitioning(arr: number[], k: number): number {
    const n: number = arr.length;
    const f: number[] = new Array(n + 1).fill(0);
    for (let i = 1; i <= n; ++i) {
        let mx: number = 0;
        for (let j = i; j > Math.max(0, i - k); --j) {
            mx = Math.max(mx, arr[j - 1]);
            f[i] = Math.max(f[i], f[j - 1] + mx * (i - j + 1));
        }
    }
    return f[n];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
