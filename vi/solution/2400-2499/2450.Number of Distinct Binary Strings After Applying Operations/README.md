---
comments: true
difficulty: Medium
tags:
    - Math
    - String
---

<!-- problem:start -->

# [2450. Number of Distinct Binary Strings After Applying Operations 🔒](https://leetcode.com/problems/number-of-distinct-binary-strings-after-applying-operations)

[中文文档](/solution/2400-2499/2450.Number%20of%20Distinct%20Binary%20Strings%20After%20Applying%20Operations/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <strong>nhị phân</strong> <code>s</code> và một số nguyên dương <code>k</code>.</p>

<p>Bạn có thể thực hiện thao tác sau trên chuỗi <strong>bao nhiêu lần cũng được</strong>:</p>

<ul>
	<li>Chọn một chuỗi con có kích thước <code>k</code> bất kỳ của <code>s</code> và <strong>đảo</strong> tất cả ký tự trong đó, tức là đổi mọi <code>1</code> thành <code>0</code>, và mọi <code>0</code> thành <code>1</code>.</li>
</ul>

<p>Trả về <em>số lượng chuỗi <strong>phân biệt</strong> mà bạn có thể nhận được</em>. Vì đáp án có thể rất lớn, hãy trả về kết quả <strong>lấy modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p><strong>Lưu ý</strong> rằng:</p>

<ul>
	<li>Chuỗi nhị phân là chuỗi gồm <strong>chỉ</strong> các ký tự <code>0</code> và <code>1</code>.</li>
	<li>Chuỗi con là một phần liên tiếp của chuỗi.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;1001&quot;, k = 3
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Ta có thể nhận được các chuỗi sau:
- Không thực hiện thao tác nào trên chuỗi, ta có s = &quot;1001&quot;.
- Thực hiện một thao tác trên chuỗi con bắt đầu tại chỉ số 0, ta có s = &quot;<u><strong>011</strong></u>1&quot;.
- Thực hiện một thao tác trên chuỗi con bắt đầu tại chỉ số 1, ta có s = &quot;1<u><strong>110</strong></u>&quot;.
- Thực hiện một thao tác trên cả hai chuỗi con bắt đầu tại các chỉ số 0 và 1, ta có s = &quot;<u><strong>0000</strong></u>&quot;.
Có thể chứng minh rằng ta không thể nhận được chuỗi nào khác, nên đáp án là 4.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;10110&quot;, k = 5
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta có thể nhận được các chuỗi sau:
- Không thực hiện thao tác nào trên chuỗi, ta có s = &quot;10110&quot;.
- Thực hiện một thao tác trên toàn bộ chuỗi, ta có s = &quot;01001&quot;.
Có thể chứng minh rằng ta không thể nhận được chuỗi nào khác, nên đáp án là 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s[i]</code> là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Việc đảo một cửa sổ có độ dài $k$ là một lựa chọn độc lập có hoặc không, và các tập con khác nhau sẽ tạo ra các chuỗi khác nhau. Có $n-k+1$ cửa sổ, nên số lượng là $2^{n-k+1}$ lấy modulo $10^9+7$.

<!-- thinking:end -->

Gọi độ dài của chuỗi $s$ là $n$. Khi đó có $n - k + 1$ chuỗi con có độ dài $k$, và mỗi chuỗi con có thể được đảo, nên có $2^{n - k + 1}$ cách đảo.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(1)$. Ở đây, $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countDistinctStrings(self, s: str, k: int) -> int:
        return pow(2, len(s) - k + 1) % (10**9 + 7)
```

#### Java

```java
class Solution {
    public static final int MOD = (int) 1e9 + 7;

    public int countDistinctStrings(String s, int k) {
        int ans = 1;
        for (int i = 0; i < s.length() - k + 1; ++i) {
            ans = (ans * 2) % MOD;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    const int mod = 1e9 + 7;

    int countDistinctStrings(string s, int k) {
        int ans = 1;
        for (int i = 0; i < s.size() - k + 1; ++i) {
            ans = (ans * 2) % mod;
        }
        return ans;
    }
};
```

#### Go

```go
func countDistinctStrings(s string, k int) int {
	const mod int = 1e9 + 7
	ans := 1
	for i := 0; i < len(s)-k+1; i++ {
		ans = (ans * 2) % mod
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
