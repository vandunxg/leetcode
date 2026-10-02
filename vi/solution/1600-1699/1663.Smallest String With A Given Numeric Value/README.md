---
comments: true
difficulty: Medium
rating: 1460
source: Weekly Contest 216 Q2
tags:
    - Greedy
    - String
---

<!-- problem:start -->

# [1663. Smallest String With A Given Numeric Value](https://leetcode.com/problems/smallest-string-with-a-given-numeric-value)

[中文文档](/solution/1600-1699/1663.Smallest%20String%20With%20A%20Given%20Numeric%20Value/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Giá trị số</strong> của một <strong>ký tự viết thường</strong> là vị trí <code>(1-indexed)</code> của nó trong bảng chữ cái. Vì vậy, giá trị số của <code>a</code> là <code>1</code>, của <code>b</code> là <code>2</code>, của <code>c</code> là <code>3</code>, v.v.</p>

<p><strong>Giá trị số</strong> của một <strong>chuỗi</strong> gồm các ký tự viết thường là tổng giá trị số của các ký tự. Ví dụ, giá trị số của chuỗi <code>&quot;abe&quot;</code> bằng <code>1 + 2 + 5 = 8</code>.</p>

<p>Cho hai số nguyên <code>n</code> và <code>k</code>. Hãy trả về <em><strong>chuỗi nhỏ nhất theo thứ tự từ điển</strong> có <strong>độ dài</strong> bằng <code>n</code> và <strong>giá trị số</strong> bằng <code>k</code>.</em></p>

<p>Lưu ý chuỗi <code>x</code> nhỏ hơn chuỗi <code>y</code> theo thứ tự từ điển nếu <code>x</code> đứng trước <code>y</code>, tức là <code>x</code> là tiền tố của <code>y</code>, hoặc tại vị trí đầu tiên <code>i</code> sao cho <code>x[i] != y[i]</code>, thì <code>x[i]</code> đứng trước <code>y[i]</code> theo thứ tự bảng chữ cái.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> n = 3, k = 27
<strong>Output:</strong> &quot;aay&quot;
<strong>Explanation:</strong> Giá trị số của chuỗi là 1 + 1 + 25 = 27, và đây là chuỗi nhỏ nhất có giá trị và độ dài như vậy.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> n = 5, k = 73
<strong>Output:</strong> &quot;aaszz&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>n &lt;= k &lt;= 26 * n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Trong các chuỗi độ dài $n$ có tổng giá trị số bằng $k$, chuỗi nhỏ nhất theo thứ tự từ điển đặt các ký tự `a`s ở bên trái và các ký tự lớn ở bên phải. Vì $n$ có thể bằng $10^5$, ta điền `z` từ cuối chuỗi.
>
> Khởi tạo toàn bộ bằng `a`s, phần còn lại là $d=k-n$. Khi $d>25$, đặt `z` và trừ $25$; sau đó cộng phần dư vào vị trí hiện tại.

<!-- thinking:end -->

Đầu tiên, ta khởi tạo mọi ký tự của chuỗi là `'a'`, còn lại giá trị $d=k-n$.

Sau đó, ta duyệt chuỗi từ cuối về đầu. Ở mỗi bước, ta tham lam thay ký tự hiện tại bằng `'z'` để giảm phần còn lại, cho đến khi phần còn lại không vượt quá $25$. Cuối cùng, ta cộng phần còn lại vào vị trí đang xét.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi. Không tính không gian của đáp án, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getSmallestString(self, n: int, k: int) -> str:
        ans = ['a'] * n
        i, d = n - 1, k - n
        while d > 25:
            ans[i] = 'z'
            d -= 25
            i -= 1
        ans[i] = chr(ord(ans[i]) + d)
        return ''.join(ans)
```

#### Java

```java
class Solution {
    public String getSmallestString(int n, int k) {
        char[] ans = new char[n];
        Arrays.fill(ans, 'a');
        int i = n - 1, d = k - n;
        for (; d > 25; d -= 25) {
            ans[i--] = 'z';
        }
        ans[i] = (char) ('a' + d);
        return String.valueOf(ans);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string getSmallestString(int n, int k) {
        string ans(n, 'a');
        int i = n - 1, d = k - n;
        for (; d > 25; d -= 25) {
            ans[i--] = 'z';
        }
        ans[i] += d;
        return ans;
    }
};
```

#### Go

```go
func getSmallestString(n int, k int) string {
	ans := make([]byte, n)
	for i := range ans {
		ans[i] = 'a'
	}
	i, d := n-1, k-n
	for ; d > 25; i, d = i-1, d-25 {
		ans[i] = 'z'
	}
	ans[i] += byte(d)
	return string(ans)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
