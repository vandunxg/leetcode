---
comments: true
difficulty: Hard
rating: 1919
source: Biweekly Contest 24 Q4
tags:
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [1416. Restore The Array](https://leetcode.com/problems/restore-the-array)

[中文文档](/solution/1400-1499/1416.Restore%20The%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Một chương trình đáng lẽ phải in ra một mảng số nguyên. Chương trình đã quên in khoảng trắng nên mảng được in thành một chuỗi các chữ số <code>s</code>. Ta chỉ biết rằng mọi số nguyên trong mảng đều nằm trong đoạn <code>[1, k]</code> và mảng không có số 0 ở đầu.</p>

<p>Cho chuỗi <code>s</code> và số nguyên <code>k</code>, hãy trả về <em>số lượng mảng có thể được in ra thành </em><code>s</code><em> bằng chương trình nói trên</em>. Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;1000&quot;, k = 10000
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Mảng duy nhất có thể là [1000]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;1000&quot;, k = 10
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không thể có mảng nào được in theo cách này mà mọi số nguyên đều &gt;= 1 và &lt;= 10.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;1317&quot;, k = 2000
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Các mảng có thể có là [1317],[131,7],[13,17],[1,317],[13,1,7],[1,31,7],[1,3,17],[1,3,1,7]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ số và không có số 0 ở đầu.</li>
	<li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta phải chia $s$ thành các số nguyên trong đoạn $[1,k]$ và không có số 0 ở đầu. Vì $n\le 10^5$, việc liệt kê mọi vị trí cắt là không khả thi. Do $k\le 10^9$, một số bắt đầu tại $i$ có nhiều nhất $10$ chữ số.
>
> Gọi $f(i)$ là số cách khôi phục $s[i:]$. Nếu bắt đầu bằng số 0 thì không có cách nào; ngược lại, ta thử các chỉ số kết thúc $j$ khi giá trị vẫn $\le k$ và cộng $f(j+1)$. Có thể ghi nhớ kết quả hoặc tính từ phải sang trái.

<!-- thinking:end -->

Tính $f(i)$ từ phải sang trái, với $f(n)=1$. Nếu $s[i]$ là `0`, thì $f(i)=0$. Nếu không, mở rộng một số bắt đầu từ chỉ số $i$ và dừng khi số đó vượt quá $k$, cộng $f(j+1)$ cho mỗi vị trí cắt hợp lệ. Đáp án là $f(0)$ theo modulo $10^9+7$.

Độ phức tạp thời gian là $O(n \times d)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của $s$ và $d$ là số chữ số thập phân của $k$, nhiều nhất là $10$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfArrays(self, s: str, k: int) -> int:
        mod = 10**9 + 7
        n = len(s)
        f = [0] * (n + 1)
        f[n] = 1
        for i in range(n - 1, -1, -1):
            if s[i] == '0':
                continue
            x = 0
            for j in range(i, n):
                x = x * 10 + int(s[j])
                if x > k:
                    break
                f[i] = (f[i] + f[j + 1]) % mod
        return f[0]
```

#### Java

```java
class Solution {
    public int numberOfArrays(String s, int k) {
        final int mod = 1_000_000_007;
        int n = s.length();
        int[] f = new int[n + 1];
        f[n] = 1;
        for (int i = n - 1; i >= 0; --i) {
            if (s.charAt(i) == '0') {
                continue;
            }
            long x = 0;
            for (int j = i; j < n; ++j) {
                x = x * 10 + s.charAt(j) - '0';
                if (x > k) {
                    break;
                }
                f[i] = (int) ((f[i] + f[j + 1]) % mod);
            }
        }
        return f[0];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfArrays(string s, int k) {
        const int mod = 1e9 + 7;
        int n = s.size();
        vector<int> f(n + 1);
        f[n] = 1;
        for (int i = n - 1; i >= 0; --i) {
            if (s[i] == '0') {
                continue;
            }
            long long x = 0;
            for (int j = i; j < n; ++j) {
                x = x * 10 + s[j] - '0';
                if (x > k) {
                    break;
                }
                f[i] = (f[i] + f[j + 1]) % mod;
            }
        }
        return f[0];
    }
};
```

#### Go

```go
func numberOfArrays(s string, k int) int {
	const mod = int(1e9 + 7)
	n := len(s)
	f := make([]int, n+1)
	f[n] = 1
	for i := n - 1; i >= 0; i-- {
		if s[i] == '0' {
			continue
		}
		x := 0
		for j := i; j < n; j++ {
			x = x*10 + int(s[j]-'0')
			if x > k {
				break
			}
			f[i] = (f[i] + f[j+1]) % mod
		}
	}
	return f[0]
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
