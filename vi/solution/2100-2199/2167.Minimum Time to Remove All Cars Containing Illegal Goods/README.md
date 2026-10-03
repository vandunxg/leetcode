---
comments: true
difficulty: Hard
rating: 2219
source: Weekly Contest 279 Q4
tags:
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [2167. Minimum Time to Remove All Cars Containing Illegal Goods](https://leetcode.com/problems/minimum-time-to-remove-all-cars-containing-illegal-goods)

[中文文档](/solution/2100-2199/2167.Minimum%20Time%20to%20Remove%20All%20Cars%20Containing%20Illegal%20Goods/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi nhị phân <code>s</code> <strong>được đánh chỉ số từ 0</strong>, biểu diễn một dãy toa tàu. <code>s[i] = &#39;0&#39;</code> cho biết toa thứ <code>i<sup>th</sup></code> <strong>không</strong> chứa hàng hóa bất hợp pháp, còn <code>s[i] = &#39;1&#39;</code> cho biết toa thứ <code>i<sup>th</sup></code> có chứa hàng hóa bất hợp pháp.</p>

<p>Với vai trò người điều khiển tàu, bạn muốn loại bỏ tất cả các toa chứa hàng hóa bất hợp pháp. Bạn có thể thực hiện <strong>bất kỳ</strong> số lần nào một trong ba thao tác sau:</p>

<ol>
	<li>Loại bỏ một toa tàu từ đầu <strong>bên trái</strong> (tức là loại bỏ <code>s[0]</code>), mất 1 đơn vị thời gian.</li>
	<li>Loại bỏ một toa tàu từ đầu <strong>bên phải</strong> (tức là loại bỏ <code>s[s.length - 1]</code>), mất 1 đơn vị thời gian.</li>
	<li>Loại bỏ một toa tàu ở <strong>bất kỳ vị trí nào</strong> trong dãy, mất 2 đơn vị thời gian.</li>
</ol>

<p>Hãy trả về <em>thời gian <strong>nhỏ nhất</strong> để loại bỏ tất cả các toa chứa hàng hóa bất hợp pháp</em>.</p>

<p>Lưu ý rằng một dãy toa rỗng được xem là không chứa toa nào có hàng hóa bất hợp pháp.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;<strong><u>11</u></strong>00<strong><u>1</u></strong>0<strong><u>1</u></strong>&quot;
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong>
Một cách để loại bỏ tất cả các toa chứa hàng hóa bất hợp pháp khỏi dãy là
- loại bỏ một toa từ đầu bên trái 2 lần. Thời gian cần thiết là 2 * 1 = 2.
- loại bỏ một toa từ đầu bên phải. Thời gian cần thiết là 1.
- loại bỏ toa chứa hàng hóa bất hợp pháp ở giữa. Thời gian cần thiết là 2.
Tổng thời gian là 2 + 1 + 2 = 5.

Một cách khác là
- loại bỏ một toa từ đầu bên trái 2 lần. Thời gian cần thiết là 2 * 1 = 2.
- loại bỏ một toa từ đầu bên phải 3 lần. Thời gian cần thiết là 3 * 1 = 3.
Tổng thời gian theo cách này cũng là 2 + 3 = 5.

5 là thời gian nhỏ nhất để loại bỏ tất cả các toa chứa hàng hóa bất hợp pháp.
Không có cách nào khác loại bỏ chúng trong thời gian ngắn hơn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;00<strong><u>1</u></strong>0&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Một cách để loại bỏ tất cả các toa chứa hàng hóa bất hợp pháp khỏi dãy là
- loại bỏ một toa từ đầu bên trái 3 lần. Thời gian cần thiết là 3 * 1 = 3.
Tổng thời gian là 3.

Một cách khác để loại bỏ tất cả các toa chứa hàng hóa bất hợp pháp khỏi dãy là
- loại bỏ toa chứa hàng hóa bất hợp pháp ở giữa. Thời gian cần thiết là 2.
Tổng thời gian là 2.

Một cách khác nữa là
- loại bỏ một toa từ đầu bên phải 2 lần. Thời gian cần thiết là 2 * 1 = 2.
Tổng thời gian là 2.

2 là thời gian nhỏ nhất để loại bỏ tất cả các toa chứa hàng hóa bất hợp pháp.
Không có cách nào khác loại bỏ chúng trong thời gian ngắn hơn.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 2 * 10<sup>5</sup></code></li>
	<li><code>s[i]</code> là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các toa chứa hàng bất hợp pháp có thể được loại bỏ bằng một tiền tố bên trái, một hậu tố bên phải hoặc từng toa riêng lẻ với chi phí $2$. Ta cần chọn sự kết hợp có chi phí thấp nhất. Việc liệt kê hai vị trí cắt và quét lại phần giữa sẽ có độ phức tạp ít nhất là bậc hai.
>
> $\textit{pre}[i]$ là chi phí nhỏ nhất cho tiền tố có độ dài $i$: ký tự `'0'` sao chép giá trị trước đó, còn ký tự `'1'` chọn $\min(\textit{pre}[i-1]+2,i)$. $\textit{suf}$ là DP hậu tố đối xứng.
>
> Đáp án là $\min_i(\textit{pre}[i]+\textit{suf}[i])$ trên các điểm chia phủ toàn bộ chuỗi.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumTime(self, s: str) -> int:
        n = len(s)
        pre = [0] * (n + 1)
        suf = [0] * (n + 1)
        for i, c in enumerate(s):
            pre[i + 1] = pre[i] if c == '0' else min(pre[i] + 2, i + 1)
        for i in range(n - 1, -1, -1):
            suf[i] = suf[i + 1] if s[i] == '0' else min(suf[i + 1] + 2, n - i)
        return min(a + b for a, b in zip(pre[1:], suf[1:]))
```

#### Java

```java
class Solution {
    public int minimumTime(String s) {
        int n = s.length();
        int[] pre = new int[n + 1];
        int[] suf = new int[n + 1];
        for (int i = 0; i < n; ++i) {
            pre[i + 1] = s.charAt(i) == '0' ? pre[i] : Math.min(pre[i] + 2, i + 1);
        }
        for (int i = n - 1; i >= 0; --i) {
            suf[i] = s.charAt(i) == '0' ? suf[i + 1] : Math.min(suf[i + 1] + 2, n - i);
        }
        int ans = Integer.MAX_VALUE;
        for (int i = 1; i <= n; ++i) {
            ans = Math.min(ans, pre[i] + suf[i]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumTime(string s) {
        int n = s.size();
        vector<int> pre(n + 1);
        vector<int> suf(n + 1);
        for (int i = 0; i < n; ++i) pre[i + 1] = s[i] == '0' ? pre[i] : min(pre[i] + 2, i + 1);
        for (int i = n - 1; ~i; --i) suf[i] = s[i] == '0' ? suf[i + 1] : min(suf[i + 1] + 2, n - i);
        int ans = INT_MAX;
        for (int i = 1; i <= n; ++i) ans = min(ans, pre[i] + suf[i]);
        return ans;
    }
};
```

#### Go

```go
func minimumTime(s string) int {
	n := len(s)
	pre := make([]int, n+1)
	suf := make([]int, n+1)
	for i, c := range s {
		pre[i+1] = pre[i]
		if c == '1' {
			pre[i+1] = min(pre[i]+2, i+1)
		}
	}
	for i := n - 1; i >= 0; i-- {
		suf[i] = suf[i+1]
		if s[i] == '1' {
			suf[i] = min(suf[i+1]+2, n-i)
		}
	}
	ans := 0x3f3f3f3f
	for i := 1; i <= n; i++ {
		ans = min(ans, pre[i]+suf[i])
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
