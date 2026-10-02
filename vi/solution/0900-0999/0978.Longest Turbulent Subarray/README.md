---
comments: true
difficulty: Medium
tags:
    - Array
    - Dynamic Programming
    - Sliding Window
---

<!-- problem:start -->

# [978. Longest Turbulent Subarray](https://leetcode.com/problems/longest-turbulent-subarray)

[中文文档](/solution/0900-0999/0978.Longest%20Turbulent%20Subarray/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>arr</code>, hãy trả về <em>độ dài của mảng con turbulent dài nhất trong</em> <code>arr</code>.</p>

<p>Một mảng con được gọi là <strong>turbulent</strong> nếu dấu so sánh giữa mỗi cặp phần tử liền kề thay đổi luân phiên.</p>

<p>Cụ thể hơn, mảng con <code>[arr[i], arr[i + 1], ..., arr[j]]</code> của <code>arr</code> được gọi là turbulent khi và chỉ khi:</p>

<ul>
	<li>Với <code>i &lt;= k &lt; j</code>:

    <ul>
    	<li><code>arr[k] &gt; arr[k + 1]</code> khi <code>k</code> lẻ, và</li>
    	<li><code>arr[k] &lt; arr[k + 1]</code> khi <code>k</code> chẵn.</li>
    </ul>
    </li>
    <li>Hoặc với <code>i &lt;= k &lt; j</code>:
    <ul>
    	<li><code>arr[k] &gt; arr[k + 1]</code> khi <code>k</code> chẵn, và</li>
    	<li><code>arr[k] &lt; arr[k + 1]</code> khi <code>k</code> lẻ.</li>
    </ul>
    </li>

</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [9,4,2,10,7,8,8,1,9]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> arr[1] &gt; arr[2] &lt; arr[3] &gt; arr[4] &lt; arr[5]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [4,8,12,16]
<strong>Đầu ra:</strong> 2
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [100]
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 4 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= arr[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Trong mảng con turbulent, các phép so sánh nghiêm ngặt luân phiên tăng giảm. Kiểm tra mọi mảng con sẽ mất thời gian bậc hai. Với mảng con kết thúc tại $i$, ta chỉ cần lưu độ dài lớn nhất của đoạn kết thúc bằng chiều tăng và chiều giảm; mỗi trạng thái nối tiếp trạng thái ngược lại tại $i-1$, hoặc đặt lại thành $1$ nếu hai phần tử bằng nhau. Chỉ cần hai biến cập nhật luân phiên.

<!-- thinking:end -->

Ta định nghĩa $f[i]$ là độ dài mảng con turbulent dài nhất kết thúc tại $\textit{nums}[i]$ với trạng thái tăng, và $g[i]$ là độ dài mảng con turbulent dài nhất kết thúc tại $\textit{nums}[i]$ với trạng thái giảm. Ban đầu, $f[0] = 1$, $g[0] = 1$. Đáp án là $\max(f[i], g[i])$.

Với $i \gt 0$, nếu $\textit{nums}[i] \gt \textit{nums}[i - 1]$ thì $f[i] = g[i - 1] + 1$, ngược lại $f[i] = 1$; nếu $\textit{nums}[i] \lt \textit{nums}[i - 1]$ thì $g[i] = f[i - 1] + 1$, ngược lại $g[i] = 1$.

Vì $f[i]$ và $g[i]$ chỉ phụ thuộc vào $f[i - 1]$ và $g[i - 1]$, ta có thể dùng hai biến thay cho các mảng.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxTurbulenceSize(self, arr: List[int]) -> int:
        ans = f = g = 1
        for a, b in pairwise(arr):
            ff = g + 1 if a < b else 1
            gg = f + 1 if a > b else 1
            f, g = ff, gg
            ans = max(ans, f, g)
        return ans
```

#### Java

```java
class Solution {
    public int maxTurbulenceSize(int[] arr) {
        int ans = 1, f = 1, g = 1;
        for (int i = 1; i < arr.length; ++i) {
            int ff = arr[i - 1] < arr[i] ? g + 1 : 1;
            int gg = arr[i - 1] > arr[i] ? f + 1 : 1;
            f = ff;
            g = gg;
            ans = Math.max(ans, Math.max(f, g));
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxTurbulenceSize(vector<int>& arr) {
        int ans = 1, f = 1, g = 1;
        for (int i = 1; i < arr.size(); ++i) {
            int ff = arr[i - 1] < arr[i] ? g + 1 : 1;
            int gg = arr[i - 1] > arr[i] ? f + 1 : 1;
            f = ff;
            g = gg;
            ans = max({ans, f, g});
        }
        return ans;
    }
};
```

#### Go

```go
func maxTurbulenceSize(arr []int) int {
	ans, f, g := 1, 1, 1
	for i, x := range arr[1:] {
		ff, gg := 1, 1
		if arr[i] < x {
			ff = g + 1
		}
		if arr[i] > x {
			gg = f + 1
		}
		f, g = ff, gg
		ans = max(ans, max(f, g))
	}
	return ans
}
```

#### TypeScript

```ts
function maxTurbulenceSize(arr: number[]): number {
    let f = 1;
    let g = 1;
    let ans = 1;
    for (let i = 1; i < arr.length; ++i) {
        const ff = arr[i - 1] < arr[i] ? g + 1 : 1;
        const gg = arr[i - 1] > arr[i] ? f + 1 : 1;
        f = ff;
        g = gg;
        ans = Math.max(ans, f, g);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_turbulence_size(arr: Vec<i32>) -> i32 {
        let mut ans = 1;
        let mut f = 1;
        let mut g = 1;

        for i in 1..arr.len() {
            let ff = if arr[i - 1] < arr[i] { g + 1 } else { 1 };
            let gg = if arr[i - 1] > arr[i] { f + 1 } else { 1 };
            f = ff;
            g = gg;
            ans = ans.max(f.max(g));
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
