---
comments: true
difficulty: Easy
rating: 1353
source: Weekly Contest 228 Q1
tags:
    - String
---

<!-- problem:start -->

# [1758. Minimum Changes To Make Alternating Binary String](https://leetcode.com/problems/minimum-changes-to-make-alternating-binary-string)

[中文文档](/solution/1700-1799/1758.Minimum%20Changes%20To%20Make%20Alternating%20Binary%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> chỉ gồm các ký tự <code>&#39;0&#39;</code> và <code>&#39;1&#39;</code>. Trong một thao tác, bạn có thể đổi bất kỳ <code>&#39;0&#39;</code> nào thành <code>&#39;1&#39;</code> hoặc ngược lại.</p>

<p>Chuỗi được gọi là xen kẽ nếu không có hai ký tự liền kề nào giống nhau. Ví dụ, chuỗi <code>&quot;010&quot;</code> là chuỗi xen kẽ, còn chuỗi <code>&quot;0100&quot;</code> thì không.</p>

<p>Trả về <em>số thao tác <strong>nhỏ nhất</strong> cần thực hiện để biến</em> <code>s</code> <em>thành chuỗi xen kẽ</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;0100&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Nếu đổi ký tự cuối thành &#39;1&#39;, s sẽ là &quot;0101&quot;, một chuỗi xen kẽ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;10&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> s đã là chuỗi xen kẽ.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;1111&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Cần hai thao tác để biến thành &quot;0101&quot; hoặc &quot;1010&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>4</sup></code></li>
	<li><code>s[i]</code> is either <code>&#39;0&#39;</code> or <code>&#39;1&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Single Pass

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ có hai chuỗi xen kẽ đích: $0101\ldots$ và $1010\ldots$. Nếu một chuỗi cần $cnt$ lần đổi, chuỗi còn lại cần $n-cnt$ lần.
>
> Đếm số vị trí khác với $0101\ldots$ trong một lượt và trả về $\min(cnt,n-cnt)$.

<!-- thinking:end -->

Theo đề bài, nếu số thao tác cần để thu được chuỗi xen kẽ `01010101...` là $\textit{cnt}$ thì số thao tác cần để thu được chuỗi xen kẽ `10101010...` là $n - \textit{cnt}$.

Do đó, ta chỉ cần duyệt chuỗi $s$ một lần, đếm giá trị $\textit{cnt}$, và đáp án là $\min(\textit{cnt}, n - \textit{cnt})$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, s: str) -> int:
        cnt = sum(c != '01'[i & 1] for i, c in enumerate(s))
        return min(cnt, len(s) - cnt)
```

#### Java

```java
class Solution {
    public int minOperations(String s) {
        int cnt = 0, n = s.length();
        for (int i = 0; i < n; ++i) {
            cnt += (s.charAt(i) != "01".charAt(i & 1) ? 1 : 0);
        }
        return Math.min(cnt, n - cnt);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minOperations(string s) {
        int cnt = 0, n = s.size();
        for (int i = 0; i < n; ++i) cnt += s[i] != "01"[i & 1];
        return min(cnt, n - cnt);
    }
};
```

#### Go

```go
func minOperations(s string) int {
	cnt := 0
	for i, c := range s {
		if c != []rune("01")[i&1] {
			cnt++
		}
	}
	return min(cnt, len(s)-cnt)
}
```

#### TypeScript

```ts
function minOperations(s: string): number {
    let cnt = 0;
    const n = s.length;
    for (let i = 0; i < n; ++i) {
        if (s[i] !== '01'[i & 1]) {
            ++cnt;
        }
    }
    return Math.min(cnt, n - cnt);
}
```

#### Rust

```rust
impl Solution {
    pub fn min_operations(s: String) -> i32 {
        let mut cnt: i32 = 0;
        let n: i32 = s.len() as i32;
        let bytes = s.as_bytes();

        for i in 0..n as usize {
            if bytes[i] != b"01"[i & 1] {
                cnt += 1;
            }
        }

        cnt.min(n - cnt)
    }
}
```

#### C

```c
int minOperations(char* s) {
    int cnt = 0;
    int n = strlen(s);
    for (int i = 0; i < n; ++i) {
        if (s[i] != "01"[i & 1]) {
            ++cnt;
        }
    }
    return cnt < (n - cnt) ? cnt : (n - cnt);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
