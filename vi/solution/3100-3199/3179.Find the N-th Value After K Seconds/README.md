---
comments: true
difficulty: Medium
rating: 1369
source: Weekly Contest 401 Q2
tags:
    - Array
    - Math
    - Combinatorics
    - Prefix Sum
    - Simulation
---

<!-- problem:start -->

# [3179. Find the N-th Value After K Seconds](https://leetcode.com/problems/find-the-n-th-value-after-k-seconds)

[Tài liệu tiếng Trung](/solution/3100-3199/3179.Find%20the%20N-th%20Value%20After%20K%20Seconds/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai số nguyên <code>n</code> và <code>k</code>.</p>

<p>Ban đầu, bạn có một mảng <code>a</code> gồm <code>n</code> số nguyên, trong đó <code>a[i] = 1</code> với mọi <code>0 &lt;= i &lt;= n - 1</code>. Sau mỗi giây, bạn đồng thời cập nhật mỗi phần tử thành tổng của tất cả các phần tử đứng trước nó cộng với chính phần tử đó. Ví dụ, sau một giây, <code>a[0]</code> giữ nguyên, <code>a[1]</code> trở thành <code>a[0] + a[1]</code>, <code>a[2]</code> trở thành <code>a[0] + a[1] + a[2]</code>, v.v.</p>

<p>Trả về <strong>giá trị</strong> của <code>a[n - 1]</code> sau <code>k</code> giây.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, k = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">56</span></p>

<p><strong>Giải thích:</strong></p>

<table border="1">
	<tbody>
		<tr>
			<th>Giây</th>
			<th>Trạng thái sau</th>
		</tr>
		<tr>
			<td>0</td>
			<td>[1,1,1,1]</td>
		</tr>
		<tr>
			<td>1</td>
			<td>[1,2,3,4]</td>
		</tr>
		<tr>
			<td>2</td>
			<td>[1,3,6,10]</td>
		</tr>
		<tr>
			<td>3</td>
			<td>[1,4,10,20]</td>
		</tr>
		<tr>
			<td>4</td>
			<td>[1,5,15,35]</td>
		</tr>
		<tr>
			<td>5</td>
			<td>[1,6,21,56]</td>
		</tr>
	</tbody>
</table>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">35</span></p>

<p><strong>Giải thích:</strong></p>

<table border="1">
	<tbody>
		<tr>
			<th>Giây</th>
			<th>Trạng thái sau</th>
		</tr>
		<tr>
			<td>0</td>
			<td>[1,1,1,1,1]</td>
		</tr>
		<tr>
			<td>1</td>
			<td>[1,2,3,4,5]</td>
		</tr>
		<tr>
			<td>2</td>
			<td>[1,3,6,10,15]</td>
		</tr>
		<tr>
			<td>3</td>
			<td>[1,4,10,20,35]</td>
		</tr>
	</tbody>
</table>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n, k &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi giây, mọi phần tử đều trở thành một prefix sum. Công thức đóng là $\binom{n+k-1}{n-1}$, nhưng với $n,k\le 1000$, mô phỏng trực tiếp sẽ đơn giản hơn khi tính theo modulo.
>
> Độ dài mảng luôn là $n$, vì vậy ta có thể tính các prefix sum ngay trên mảng từ trái sang phải.
>
> Khởi tạo mảng toàn số 1, lặp $a[i]+=a[i-1]$ theo modulo $10^9+7$ trong $k$ giây, rồi trả về $a[n-1]$.

<!-- thinking:end -->

Ta nhận thấy miền giá trị của số nguyên $n$ là $1 \leq n \leq 1000$, nên có thể mô phỏng trực tiếp quá trình này.

Ta định nghĩa một mảng $a$ có độ dài $n$ và khởi tạo tất cả phần tử bằng $1$. Sau đó, ta mô phỏng quá trình trong $k$ giây, cập nhật các phần tử của mảng $a$ sau mỗi giây cho đến khi đủ $k$ giây.

Cuối cùng, ta trả về $a[n - 1]$.

Độ phức tạp thời gian là $O(n \times k)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $a$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def valueAfterKSeconds(self, n: int, k: int) -> int:
        a = [1] * n
        mod = 10**9 + 7
        for _ in range(k):
            for i in range(1, n):
                a[i] = (a[i] + a[i - 1]) % mod
        return a[n - 1]
```

#### Java

```java
class Solution {
    public int valueAfterKSeconds(int n, int k) {
        final int mod = (int) 1e9 + 7;
        int[] a = new int[n];
        Arrays.fill(a, 1);
        while (k-- > 0) {
            for (int i = 1; i < n; ++i) {
                a[i] = (a[i] + a[i - 1]) % mod;
            }
        }
        return a[n - 1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int valueAfterKSeconds(int n, int k) {
        const int mod = 1e9 + 7;
        vector<int> a(n, 1);
        while (k-- > 0) {
            for (int i = 1; i < n; ++i) {
                a[i] = (a[i] + a[i - 1]) % mod;
            }
        }
        return a[n - 1];
    }
};
```

#### Go

```go
func valueAfterKSeconds(n int, k int) int {
	const mod int = 1e9 + 7
	a := make([]int, n)
	for i := range a {
		a[i] = 1
	}
	for ; k > 0; k-- {
		for i := 1; i < n; i++ {
			a[i] = (a[i] + a[i-1]) % mod
		}
	}
	return a[n-1]
}
```

#### TypeScript

```ts
function valueAfterKSeconds(n: number, k: number): number {
    const a: number[] = Array(n).fill(1);
    const mod: number = 10 ** 9 + 7;
    while (k--) {
        for (let i = 1; i < n; ++i) {
            a[i] = (a[i] + a[i - 1]) % mod;
        }
    }
    return a[n - 1];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
