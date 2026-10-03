---
comments: true
difficulty: Medium
rating: 1688
source: Weekly Contest 253 Q3
tags:
    - Stack
    - Greedy
    - Two Pointers
    - String
    - Parentheses
---

<!-- problem:start -->

# [1963. Minimum Number of Swaps to Make the String Balanced](https://leetcode.com/problems/minimum-number-of-swaps-to-make-the-string-balanced)

[中文文档](/solution/1900-1999/1963.Minimum%20Number%20of%20Swaps%20to%20Make%20the%20String%20Balanced/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <strong>được đánh chỉ số từ 0</strong> <code>s</code> có độ dài <strong>chẵn</strong> <code>n</code>. Chuỗi gồm <strong>đúng</strong> <code>n / 2</code> dấu ngoặc mở <code>&#39;[&#39;</code> và <code>n / 2</code> dấu ngoặc đóng <code>&#39;]&#39;</code>.</p>

<p>Một chuỗi được gọi là <strong>cân bằng</strong> khi và chỉ khi:</p>

<ul>
	<li>Đó là chuỗi rỗng, hoặc</li>
	<li>Có thể viết dưới dạng <code>AB</code>, trong đó cả <code>A</code> và <code>B</code> đều là chuỗi <strong>cân bằng</strong>, hoặc</li>
	<li>Có thể viết dưới dạng <code>[C]</code>, trong đó <code>C</code> là một chuỗi <strong>cân bằng</strong>.</li>
</ul>

<p>Bạn có thể đổi chỗ các dấu ngoặc ở <strong>bất kỳ</strong> hai chỉ số nào với số lần <strong>bất kỳ</strong>.</p>

<p>Hãy trả về <em>số lần đổi chỗ <strong>ít nhất</strong> để biến </em><code>s</code> <em>thành chuỗi <strong>cân bằng</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;][][&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Bạn có thể biến chuỗi thành chuỗi cân bằng bằng cách đổi chỗ chỉ số 0 và 3.
Chuỗi nhận được là &quot;[[]]&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;]]][[[&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Bạn có thể thực hiện các bước sau để biến chuỗi thành chuỗi cân bằng:
- Đổi chỗ chỉ số 0 và 4. s = &quot;[]][][&quot;.
- Đổi chỗ chỉ số 1 và 5. s = &quot;[[][]]&quot;.
Chuỗi nhận được là &quot;[[][]]&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;[]&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Chuỗi đã cân bằng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == s.length</code></li>
	<li><code>2 &lt;= n &lt;= 10<sup>6</sup></code></li>
	<li><code>n</code> là số chẵn.</li>
	<li><code>s[i]</code> là <code>&#39;[&#39; </code> hoặc <code>&#39;]&#39;</code>.</li>
	<li>Số lượng dấu ngoặc mở <code>&#39;[&#39;</code> bằng <code>n / 2</code>, và số lượng dấu ngoặc đóng <code>&#39;]&#39;</code> bằng <code>n / 2</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Có thể đổi chỗ hai dấu ngoặc bất kỳ. Sau khi ghép cặp, phần còn lại có dạng một số dấu `]` đứng trước các dấu `[`.
>
> Nếu còn $x$ dấu ngoặc mở chưa được ghép, mỗi lần đổi chỗ hai đầu sẽ loại bỏ hai dấu trong số đó, nên cần $\lfloor(x+1)/2\rfloor$ lần đổi chỗ là đủ. Chỉ cần một biến đếm để tính $x$.

<!-- thinking:end -->

Ta dùng biến $x$ để ghi lại số dấu ngoặc mở chưa được ghép hiện tại. Ta duyệt chuỗi $s$, với mỗi ký tự $c$:

- Nếu $c$ là dấu ngoặc mở, ta tăng $x$ lên một;
- Nếu $c$ là dấu ngoặc đóng, ta kiểm tra xem $x$ có lớn hơn không. Nếu có, ta ghép dấu ngoặc đóng hiện tại với dấu ngoặc mở chưa được ghép gần nhất ở bên trái, tức là giảm $x$ đi một.

Sau khi duyệt xong, chắc chắn ta thu được một chuỗi có dạng `"]]]...[[[..."`. Sau đó, mỗi lần ta tham lam đổi chỗ hai dấu ngoặc ở hai đầu, có thể loại bỏ $2$ dấu ngoặc mở chưa được ghép. Vì vậy, tổng số lần đổi chỗ cần thiết là $\left\lfloor \frac{x + 1}{2} \right\rfloor$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minSwaps(self, s: str) -> int:
        x = 0
        for c in s:
            if c == "[":
                x += 1
            elif x:
                x -= 1
        return (x + 1) >> 1
```

#### Java

```java
class Solution {
    public int minSwaps(String s) {
        int x = 0;
        for (int i = 0; i < s.length(); ++i) {
            char c = s.charAt(i);
            if (c == '[') {
                ++x;
            } else if (x > 0) {
                --x;
            }
        }
        return (x + 1) / 2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minSwaps(string s) {
        int x = 0;
        for (char& c : s) {
            if (c == '[') {
                ++x;
            } else if (x) {
                --x;
            }
        }
        return (x + 1) / 2;
    }
};
```

#### Go

```go
func minSwaps(s string) int {
	x := 0
	for _, c := range s {
		if c == '[' {
			x++
		} else if x > 0 {
			x--
		}
	}
	return (x + 1) / 2
}
```

#### TypeScript

```ts
function minSwaps(s: string): number {
    let x = 0;
    for (const c of s) {
        if (c === '[') {
            ++x;
        } else if (x) {
            --x;
        }
    }
    return (x + 1) >> 1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
