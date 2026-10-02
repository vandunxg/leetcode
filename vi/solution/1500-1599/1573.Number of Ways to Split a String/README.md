---
comments: true
difficulty: Medium
rating: 1590
source: Biweekly Contest 34 Q2
tags:
    - Math
    - String
---

<!-- problem:start -->

# [1573. Number of Ways to Split a String](https://leetcode.com/problems/number-of-ways-to-split-a-string)

[中文文档](/solution/1500-1599/1573.Number%20of%20Ways%20to%20Split%20a%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi nhị phân <code>s</code>, bạn có thể chia <code>s</code> thành 3 chuỗi <strong>không rỗng</strong> <code>s1</code>, <code>s2</code> và <code>s3</code> sao cho <code>s1 + s2 + s3 = s</code>.</p>

<p>Trả về số cách chia <code>s</code> sao cho số lượng số 1 trong <code>s1</code>, <code>s2</code> và <code>s3</code> bằng nhau. Vì đáp án có thể rất lớn, hãy trả về phần dư <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;10101&quot;
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Có bốn cách chia s thành 3 phần sao cho mỗi phần chứa cùng số ký tự &#39;1&#39;.
&quot;1|010|1&quot;
&quot;1|01|01&quot;
&quot;10|10|1&quot;
&quot;10|1|01&quot;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;1001&quot;
<strong>Đầu ra:</strong> 0
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;0000&quot;
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Có ba cách chia s thành 3 phần.
&quot;0|0|00&quot;
&quot;0|00|0&quot;
&quot;00|0|0&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s[i]</code> là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Chia chuỗi thành ba phần có cùng số lượng số 1. Vì $n\le 10^5$, ta không thể thử mọi cặp vị trí cắt. Mỗi phần phải chứa đúng một phần ba tổng số số 1.
>
> Nếu tổng không chia hết cho ba thì không thể chia; nếu không có số 1 nào, chọn bất kỳ hai vị trí cắt nào cũng được và số cách là $C_{n-1}^{2}$. Nếu không, vị trí cắt thứ nhất có thể nằm ở bất kỳ đâu giữa số 1 thứ $cnt$ và $(cnt+1)$, còn vị trí cắt thứ hai nằm giữa số 1 thứ $2cnt$ và $(2cnt+1)$. Tích độ dài của hai khoảng trống đó là đáp án.

<!-- thinking:end -->

Đầu tiên, ta duyệt chuỗi $s$ và đếm số ký tự $1$, ký hiệu là $cnt$. Nếu $cnt$ không chia hết cho $3$, ta không thể chia chuỗi nên trả về ngay $0$. Nếu $cnt$ bằng $0$, chuỗi không có ký tự $1$. Ta có thể chọn hai vị trí bất kỳ trong $n-1$ vị trí để chia chuỗi thành ba chuỗi con, nên số cách là $C_{n-1}^2$.

Nếu $cnt \gt 0$, ta cập nhật $cnt$ thành $\frac{cnt}{3}$, là số ký tự $1$ trong mỗi chuỗi con.

Tiếp theo, ta tìm chỉ số nhỏ nhất của biên phải chuỗi con thứ nhất, ký hiệu là $i_1$, và chỉ số lớn nhất của biên phải chuỗi con thứ nhất (không bao gồm biên), ký hiệu là $i_2$. Tương tự, ta tìm các chỉ số tương ứng $j_1$, $j_2$ cho chuỗi con thứ hai. Khi đó số cách là $(i_2 - i_1) \times (j_2 - j_1)$.

Lưu ý rằng đáp án có thể rất lớn, nên ta cần lấy modulo $10^9+7$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là độ dài chuỗi $s$.

Các bài tương tự:

- [927. Three Equal Parts](https://github.com/doocs/leetcode/blob/main/solution/0900-0999/0927.Three%20Equal%20Parts/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numWays(self, s: str) -> int:
        def find(x):
            t = 0
            for i, c in enumerate(s):
                t += int(c == '1')
                if t == x:
                    return i

        cnt, m = divmod(sum(c == '1' for c in s), 3)
        if m:
            return 0
        n = len(s)
        mod = 10**9 + 7
        if cnt == 0:
            return ((n - 1) * (n - 2) // 2) % mod
        i1, i2 = find(cnt), find(cnt + 1)
        j1, j2 = find(cnt * 2), find(cnt * 2 + 1)
        return (i2 - i1) * (j2 - j1) % (10**9 + 7)
```

#### Java

```java
class Solution {
    private String s;

    public int numWays(String s) {
        this.s = s;
        int cnt = 0;
        int n = s.length();
        for (int i = 0; i < n; ++i) {
            if (s.charAt(i) == '1') {
                ++cnt;
            }
        }
        int m = cnt % 3;
        if (m != 0) {
            return 0;
        }
        final int mod = (int) 1e9 + 7;
        if (cnt == 0) {
            return (int) (((n - 1L) * (n - 2) / 2) % mod);
        }
        cnt /= 3;
        long i1 = find(cnt), i2 = find(cnt + 1);
        long j1 = find(cnt * 2), j2 = find(cnt * 2 + 1);
        return (int) ((i2 - i1) * (j2 - j1) % mod);
    }

    private int find(int x) {
        int t = 0;
        for (int i = 0;; ++i) {
            t += s.charAt(i) == '1' ? 1 : 0;
            if (t == x) {
                return i;
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numWays(string s) {
        int cnt = 0;
        for (char& c : s) {
            cnt += c == '1';
        }
        int m = cnt % 3;
        if (m) {
            return 0;
        }
        const int mod = 1e9 + 7;
        int n = s.size();
        if (cnt == 0) {
            return (n - 1LL) * (n - 2) / 2 % mod;
        }
        cnt /= 3;
        auto find = [&](int x) {
            int t = 0;
            for (int i = 0;; ++i) {
                t += s[i] == '1';
                if (t == x) {
                    return i;
                }
            }
        };
        int i1 = find(cnt), i2 = find(cnt + 1);
        int j1 = find(cnt * 2), j2 = find(cnt * 2 + 1);
        return (1LL * (i2 - i1) * (j2 - j1)) % mod;
    }
};
```

#### Go

```go
func numWays(s string) int {
	cnt := 0
	for _, c := range s {
		if c == '1' {
			cnt++
		}
	}
	m := cnt % 3
	if m != 0 {
		return 0
	}
	const mod = 1e9 + 7
	n := len(s)
	if cnt == 0 {
		return (n - 1) * (n - 2) / 2 % mod
	}
	cnt /= 3
	find := func(x int) int {
		t := 0
		for i := 0; ; i++ {
			if s[i] == '1' {
				t++
				if t == x {
					return i
				}
			}
		}
	}
	i1, i2 := find(cnt), find(cnt+1)
	j1, j2 := find(cnt*2), find(cnt*2+1)
	return (i2 - i1) * (j2 - j1) % mod
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
