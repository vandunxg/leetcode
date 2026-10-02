---
comments: true
difficulty: Medium
rating: 1747
source: Weekly Contest 154 Q3
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [1191. K-Concatenation Maximum Sum](https://leetcode.com/problems/k-concatenation-maximum-sum)

[中文文档](/solution/1100-1199/1191.K-Concatenation%20Maximum%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>arr</code> và số nguyên <code>k</code>. Hãy tạo mảng mới bằng cách lặp lại <code>arr</code> <code>k</code> lần.</p>

<p>Ví dụ, nếu <code>arr = [1, 2]</code> và <code>k = 3 </code>thì mảng mới là <code>[1, 2, 1, 2, 1, 2]</code>.</p>

<p>Hãy trả về tổng lớn nhất của một mảng con trong mảng mới. Lưu ý mảng con có thể có độ dài <code>0</code>; khi đó tổng của nó là <code>0</code>.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,2], k = 3
<strong>Đầu ra:</strong> 9
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,-2,1], k = 5
<strong>Đầu ra:</strong> 2
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [-1,-2], k = 7
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>4</sup> &lt;= arr[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum + Case Discussion

<!-- thinking:start -->

> **Tư duy**
>
> Sau khi lặp $arr$ $k$ lần, mảng con tối ưu không cần trải dài quá toàn bộ mảng ghép. Thuật toán Kadane trên một bản sao cho $mxSub$, cùng prefix lớn nhất và prefix nhỏ nhất (từ đó suy ra suffix lớn nhất). Với $k=1$, $mxSub$ là đáp án; nếu không, xét thêm tổng prefix và suffix, và nếu tổng toàn mảng dương thì cộng thêm $k-2$ bản sao đầy đủ. Vì $k$ có thể tới $10^5$, ta không tạo mảng ghép thật sự.

<!-- thinking:end -->

Ký hiệu tổng tất cả phần tử trong mảng $arr$ là $s$, tổng prefix lớn nhất là $mxPre$, tổng prefix nhỏ nhất là $miPre$, và tổng mảng con lớn nhất là $mxSub$.

Ta duyệt mảng $arr$. Với mỗi phần tử $x$, cập nhật $s = s + x$, $mxPre = \max(mxPre, s)$, $miPre = \min(miPre, s)$ và $mxSub = \max(mxSub, s - miPre)$.

Tiếp theo, xét giá trị của $k$:

- Nếu $k = 1$, đáp án là $mxSub$.
- Nếu $k \ge 2$ và mảng con lớn nhất trải qua hai bản sao $arr$, đáp án là $mxPre + mxSuf$, trong đó $mxSuf = s - miPre$.
- Nếu $k \ge 2$ và $s > 0$, mảng con lớn nhất có thể trải qua ba phần: đáp án là $(k - 2) \times s + mxPre + mxSuf$.

Cuối cùng, trả về đáp án modulo $10^9 + 7$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$. Trong đó, $n$ là độ dài của mảng $arr$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def kConcatenationMaxSum(self, arr: List[int], k: int) -> int:
        s = mx_pre = mi_pre = mx_sub = 0
        for x in arr:
            s += x
            mx_pre = max(mx_pre, s)
            mi_pre = min(mi_pre, s)
            mx_sub = max(mx_sub, s - mi_pre)
        ans = mx_sub
        mod = 10**9 + 7
        if k == 1:
            return ans % mod
        mx_suf = s - mi_pre
        ans = max(ans, mx_pre + mx_suf)
        if s > 0:
            ans = max(ans, (k - 2) * s + mx_pre + mx_suf)
        return ans % mod
```

#### Java

```java
class Solution {
    public int kConcatenationMaxSum(int[] arr, int k) {
        long s = 0, mxPre = 0, miPre = 0, mxSub = 0;
        for (int x : arr) {
            s += x;
            mxPre = Math.max(mxPre, s);
            miPre = Math.min(miPre, s);
            mxSub = Math.max(mxSub, s - miPre);
        }
        long ans = mxSub;
        final int mod = (int) 1e9 + 7;
        if (k == 1) {
            return (int) (ans % mod);
        }
        long mxSuf = s - miPre;
        ans = Math.max(ans, mxPre + mxSuf);
        if (s > 0) {
            ans = Math.max(ans, (k - 2) * s + mxPre + mxSuf);
        }
        return (int) (ans % mod);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int kConcatenationMaxSum(vector<int>& arr, int k) {
        long s = 0, mxPre = 0, miPre = 0, mxSub = 0;
        for (int x : arr) {
            s += x;
            mxPre = max(mxPre, s);
            miPre = min(miPre, s);
            mxSub = max(mxSub, s - miPre);
        }
        long ans = mxSub;
        const int mod = 1e9 + 7;
        if (k == 1) {
            return ans % mod;
        }
        long mxSuf = s - miPre;
        ans = max(ans, mxPre + mxSuf);
        if (s > 0) {
            ans = max(ans, mxPre + (k - 2) * s + mxSuf);
        }
        return ans % mod;
    }
};
```

#### Go

```go
func kConcatenationMaxSum(arr []int, k int) int {
	var s, mxPre, miPre, mxSub int
	for _, x := range arr {
		s += x
		mxPre = max(mxPre, s)
		miPre = min(miPre, s)
		mxSub = max(mxSub, s-miPre)
	}
	const mod = 1e9 + 7
	ans := mxSub
	if k == 1 {
		return ans % mod
	}
	mxSuf := s - miPre
	ans = max(ans, mxSuf+mxPre)
	if s > 0 {
		ans = max(ans, mxSuf+(k-2)*s+mxPre)
	}
	return ans % mod
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
