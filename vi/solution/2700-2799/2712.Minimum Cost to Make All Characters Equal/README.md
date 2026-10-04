---
comments: true
difficulty: Medium
rating: 1791
source: Weekly Contest 347 Q3
tags:
    - Greedy
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [2712. Minimum Cost to Make All Characters Equal](https://leetcode.com/problems/minimum-cost-to-make-all-characters-equal)

[中文文档](/solution/2700-2799/2712.Minimum%20Cost%20to%20Make%20All%20Characters%20Equal/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi nhị phân <code>s</code> có độ dài <code>n</code>, được đánh chỉ số từ <strong>0</strong>, trên đó bạn có thể thực hiện hai loại thao tác:</p>

<ul>
	<li>Chọn một chỉ số <code>i</code> và đảo tất cả ký tự từ chỉ số <code>0</code> đến chỉ số <code>i</code> (bao gồm cả hai đầu), với chi phí là <code>i + 1</code></li>
	<li>Chọn một chỉ số <code>i</code> và đảo tất cả ký tự từ chỉ số <code>i</code> đến <code>n - 1</code> (bao gồm cả hai đầu), với chi phí là <code>n - i</code></li>
</ul>

<p>Hãy trả về <em><strong>chi phí nhỏ nhất</strong> để làm cho tất cả ký tự trong chuỗi <strong>giống nhau</strong></em>.</p>

<p><strong>Đảo</strong> một ký tự nghĩa là nếu giá trị của nó là &#39;0&#39; thì chuyển thành &#39;1&#39; và ngược lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;0011&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Áp dụng thao tác thứ hai với <code>i = 2</code> để nhận được <code>s = &quot;0000&quot; for a cost of 2</code>. Có thể chứng minh rằng 2 là chi phí nhỏ nhất để làm cho tất cả ký tự trong chuỗi giống nhau.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;010101&quot;
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong> Áp dụng thao tác thứ nhất với i = 2 để nhận được s = &quot;101101&quot; với chi phí là 3.
Áp dụng thao tác thứ nhất với i = 1 để nhận được s = &quot;011101&quot; với chi phí là 2.
Áp dụng thao tác thứ nhất với i = 0 để nhận được s = &quot;111101&quot; với chi phí là 1.
Áp dụng thao tác thứ hai với i = 4 để nhận được s = &quot;111110&quot; với chi phí là 2.
Áp dụng thao tác thứ hai với i = 5 để nhận được s = &quot;111111&quot; với chi phí là 1.
Tổng chi phí để làm cho tất cả ký tự trong chuỗi giống nhau là 9. Có thể chứng minh rằng 9 là chi phí nhỏ nhất để làm cho tất cả ký tự trong chuỗi giống nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length == n &lt;= 10<sup>5</sup></code></li>
	<li><code>s[i]</code> là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thuật toán tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Một thao tác lật một tiền tố hoặc hậu tố và có chi phí bằng độ dài của nó. Việc tìm chuỗi thao tác là bất khả thi khi độ dài là $10^5$.
>
> Mỗi cặp ký tự kề nhau khác nhau phải được một số lẻ lần lật tiền tố hoặc hậu tố bao phủ. Gán vị trí cắt tại $i$ cho tiền tố tốn $i$, còn cho hậu tố tốn $n-i$. Chọn phương án rẻ hơn tại mỗi vị trí cắt là tối ưu vì các vị trí cắt không ảnh hưởng lẫn nhau.

<!-- thinking:end -->

Theo mô tả bài toán, nếu $s[i] \neq s[i - 1]$, ta phải thực hiện một thao tác; nếu không thì không thể làm cho tất cả ký tự trong chuỗi giống nhau.

Ta có thể chọn đảo tất cả ký tự từ $s[0..i-1]$, với chi phí là $i$, hoặc đảo tất cả ký tự từ $s[i..n-1]$, với chi phí là $n - i$. Ta chọn giá trị nhỏ hơn trong hai chi phí này.

Bằng cách duyệt chuỗi $s$ và cộng chi phí của tất cả các đoạn cần đảo, ta có thể tính được chi phí nhỏ nhất.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumCost(self, s: str) -> int:
        ans, n = 0, len(s)
        for i in range(1, n):
            if s[i] != s[i - 1]:
                ans += min(i, n - i)
        return ans
```

#### Java

```java
class Solution {
    public long minimumCost(String s) {
        long ans = 0;
        int n = s.length();
        for (int i = 1; i < n; ++i) {
            if (s.charAt(i) != s.charAt(i - 1)) {
                ans += Math.min(i, n - i);
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minimumCost(string s) {
        long long ans = 0;
        int n = s.size();
        for (int i = 1; i < n; ++i) {
            if (s[i] != s[i - 1]) {
                ans += min(i, n - i);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minimumCost(s string) (ans int64) {
	n := len(s)
	for i := 1; i < n; i++ {
		if s[i] != s[i-1] {
			ans += int64(min(i, n-i))
		}
	}
	return
}
```

#### TypeScript

```ts
function minimumCost(s: string): number {
    let ans = 0;
    const n = s.length;
    for (let i = 1; i < n; ++i) {
        if (s[i] !== s[i - 1]) {
            ans += Math.min(i, n - i);
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn minimum_cost(s: String) -> i64 {
        let mut ans = 0;
        let n = s.len();
        let s = s.as_bytes();
        for i in 1..n {
            if s[i] != s[i - 1] {
                ans += i.min(n - i);
            }
        }
        ans as i64
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
