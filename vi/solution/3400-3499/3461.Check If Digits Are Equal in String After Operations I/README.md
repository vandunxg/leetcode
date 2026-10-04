---
comments: true
difficulty: Easy
rating: 1189
source: Weekly Contest 438 Q1
tags:
    - Math
    - String
    - Combinatorics
    - Number Theory
    - Simulation
---

<!-- problem:start -->

# [3461. Check If Digits Are Equal in String After Operations I](https://leetcode.com/problems/check-if-digits-are-equal-in-string-after-operations-i)

[中文文档](/solution/3400-3499/3461.Check%20If%20Digits%20Are%20Equal%20in%20String%20After%20Operations%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> chỉ gồm các chữ số. Lặp lại thao tác sau cho đến khi chuỗi có <strong>đúng</strong> hai chữ số:</p>

<ul>
	<li>Với mỗi cặp chữ số liên tiếp trong <code>s</code>, bắt đầu từ chữ số đầu tiên, tính một chữ số mới bằng tổng của hai chữ số đó <strong>modulo</strong> 10.</li>
	<li>Thay <code>s</code> bằng dãy các chữ số mới được tính, <em>giữ nguyên thứ tự</em> mà chúng được tính.</li>
</ul>

<p>Trả về <code>true</code> nếu hai chữ số cuối cùng trong <code>s</code> <strong>giống nhau</strong>; ngược lại, trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;3902&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Ban đầu, <code>s = &quot;3902&quot;</code></li>
	<li>Thao tác đầu tiên:
	<ul>
		<li><code>(s[0] + s[1]) % 10 = (3 + 9) % 10 = 2</code></li>
		<li><code>(s[1] + s[2]) % 10 = (9 + 0) % 10 = 9</code></li>
		<li><code>(s[2] + s[3]) % 10 = (0 + 2) % 10 = 2</code></li>
		<li><code>s</code> trở thành <code>&quot;292&quot;</code></li>
	</ul>
	</li>
	<li>Thao tác thứ hai:
	<ul>
		<li><code>(s[0] + s[1]) % 10 = (2 + 9) % 10 = 1</code></li>
		<li><code>(s[1] + s[2]) % 10 = (9 + 2) % 10 = 1</code></li>
		<li><code>s</code> trở thành <code>&quot;11&quot;</code></li>
	</ul>
	</li>
	<li>Vì các chữ số trong <code>&quot;11&quot;</code> giống nhau, kết quả là <code>true</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;34789&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Ban đầu, <code>s = &quot;34789&quot;</code>.</li>
	<li>Sau thao tác đầu tiên, <code>s = &quot;7157&quot;</code>.</li>
	<li>Sau thao tác thứ hai, <code>s = &quot;862&quot;</code>.</li>
	<li>Sau thao tác thứ ba, <code>s = &quot;48&quot;</code>.</li>
	<li>Vì <code>&#39;4&#39; != &#39;8&#39;</code>, kết quả là <code>false</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ gồm các chữ số.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi bước thay chuỗi bằng các tổng của hai chữ số kề nhau theo modulo $10$. Vì $n\le 100$, mô phỏng $O(n^2)$ là đủ.
>
> Không cần lưu các chuỗi trung gian: ghi $(t[i]+t[i+1])\bmod 10$ trực tiếp vào mảng khi độ dài giảm từ $n-1$ xuống $2$.
>
> Cuối cùng, so sánh $t[0]$ với $t[1]$.

<!-- thinking:end -->

Ta có thể mô phỏng các thao tác được mô tả trong đề bài cho đến khi chuỗi $s$ chỉ còn đúng hai chữ số, sau đó kiểm tra xem hai chữ số này có giống nhau hay không.

Độ phức tạp thời gian là $O(n^2)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def hasSameDigits(self, s: str) -> bool:
        t = list(map(int, s))
        n = len(t)
        for k in range(n - 1, 1, -1):
            for i in range(k):
                t[i] = (t[i] + t[i + 1]) % 10
        return t[0] == t[1]
```

#### Java

```java
class Solution {
    public boolean hasSameDigits(String s) {
        char[] t = s.toCharArray();
        int n = t.length;
        for (int k = n - 1; k > 1; --k) {
            for (int i = 0; i < k; ++i) {
                t[i] = (char) ((t[i] - '0' + t[i + 1] - '0') % 10 + '0');
            }
        }
        return t[0] == t[1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool hasSameDigits(string s) {
        int n = s.size();
        string t = s;
        for (int k = n - 1; k > 1; --k) {
            for (int i = 0; i < k; ++i) {
                t[i] = (t[i] - '0' + t[i + 1] - '0') % 10 + '0';
            }
        }
        return t[0] == t[1];
    }
};
```

#### Go

```go
func hasSameDigits(s string) bool {
	t := []byte(s)
	n := len(t)
	for k := n - 1; k > 1; k-- {
		for i := 0; i < k; i++ {
			t[i] = (t[i]-'0'+t[i+1]-'0')%10 + '0'
		}
	}
	return t[0] == t[1]
}
```

#### TypeScript

```ts
function hasSameDigits(s: string): boolean {
    const t = s.split('').map(Number);
    const n = t.length;
    for (let k = n - 1; k > 1; --k) {
        for (let i = 0; i < k; ++i) {
            t[i] = (t[i] + t[i + 1]) % 10;
        }
    }
    return t[0] === t[1];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
