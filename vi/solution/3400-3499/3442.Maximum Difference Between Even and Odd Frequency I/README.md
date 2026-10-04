---
comments: true
difficulty: Easy
rating: 1220
source: Weekly Contest 435 Q1
tags:
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [3442. Maximum Difference Between Even and Odd Frequency I](https://leetcode.com/problems/maximum-difference-between-even-and-odd-frequency-i)

[中文文档](/solution/3400-3499/3442.Maximum%20Difference%20Between%20Even%20and%20Odd%20Frequency%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</p>

<p>Nhiệm vụ của bạn là tìm độ chênh lệch <strong>lớn nhất</strong> <code>diff = freq(a<sub>1</sub>) - freq(a<sub>2</sub>)</code> giữa số lần xuất hiện của các ký tự <code>a<sub>1</sub></code> và <code>a<sub>2</sub></code> trong chuỗi, sao cho:</p>

<ul>
	<li><code>a<sub>1</sub></code> có <strong>tần suất lẻ</strong> trong chuỗi.</li>
	<li><code>a<sub>2</sub></code> có <strong>tần suất chẵn</strong> trong chuỗi.</li>
</ul>

<p>Trả về độ chênh lệch <strong>lớn nhất</strong> này.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;aaaaabbc&quot;</span></p>

<p><strong>Đầu ra:</strong> 3</p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Ký tự <code>&#39;a&#39;</code> có <strong>tần suất lẻ</strong> là <code><font face="monospace">5</font></code><font face="monospace">,</font> còn <code>&#39;b&#39;</code> có <strong>tần suất chẵn</strong> là <code><font face="monospace">2</font></code>.</li>
	<li>Độ chênh lệch lớn nhất là <code>5 - 2 = 3</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abcabcab&quot;</span></p>

<p><strong>Đầu ra:</strong> 1</p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Ký tự <code>&#39;a&#39;</code> có <strong>tần suất lẻ</strong> là <code><font face="monospace">3</font></code><font face="monospace">,</font> còn <code>&#39;c&#39;</code> có <strong>tần suất chẵn</strong> là <font face="monospace">2</font>.</li>
	<li>Độ chênh lệch lớn nhất là <code>3 - 2 = 1</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>s</code> chứa ít nhất một ký tự có tần suất lẻ và một ký tự có tần suất chẵn.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần lấy tần suất lẻ lớn nhất trừ đi tần suất chẵn nhỏ nhất. Chỉ cần đếm một lần các chữ cái viết thường.
>
> Đề bài xét toàn bộ chuỗi, không phải một chuỗi con.
>
> Duyệt bảng đếm, lấy giá trị lẻ lớn nhất và giá trị chẵn nhỏ nhất, rồi trả về hiệu của chúng.

<!-- thinking:end -->

Ta có thể dùng một hash table hoặc một mảng $\textit{cnt}$ để ghi nhận số lần xuất hiện của từng ký tự trong chuỗi $s$. Sau đó, ta duyệt qua $\textit{cnt}$ để tìm tần suất lớn nhất $a$ của các ký tự xuất hiện lẻ lần và tần suất nhỏ nhất $b$ của các ký tự xuất hiện chẵn lần. Cuối cùng, ta trả về $a - b$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Độ phức tạp không gian là $O(|\Sigma|)$, trong đó $\Sigma$ là tập ký tự. Trong bài này, $|\Sigma| = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxDifference(self, s: str) -> int:
        cnt = Counter(s)
        a, b = 0, inf
        for v in cnt.values():
            if v % 2:
                a = max(a, v)
            else:
                b = min(b, v)
        return a - b
```

#### Java

```java
class Solution {
    public int maxDifference(String s) {
        int[] cnt = new int[26];
        for (char c : s.toCharArray()) {
            ++cnt[c - 'a'];
        }
        int a = 0, b = 1 << 30;
        for (int v : cnt) {
            if (v % 2 == 1) {
                a = Math.max(a, v);
            } else if (v > 0) {
                b = Math.min(b, v);
            }
        }
        return a - b;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxDifference(string s) {
        int cnt[26]{};
        for (char c : s) {
            ++cnt[c - 'a'];
        }
        int a = 0, b = 1 << 30;
        for (int v : cnt) {
            if (v % 2 == 1) {
                a = max(a, v);
            } else if (v > 0) {
                b = min(b, v);
            }
        }
        return a - b;
    }
};
```

#### Go

```go
func maxDifference(s string) int {
	cnt := [26]int{}
	for _, c := range s {
		cnt[c-'a']++
	}
	a, b := 0, 1<<30
	for _, v := range cnt {
		if v%2 == 1 {
			a = max(a, v)
		} else if v > 0 {
			b = min(b, v)
		}
	}
	return a - b
}
```

#### TypeScript

```ts
function maxDifference(s: string): number {
    const cnt: Record<string, number> = {};
    for (const c of s) {
        cnt[c] = (cnt[c] || 0) + 1;
    }
    let [a, b] = [0, Infinity];
    for (const [_, v] of Object.entries(cnt)) {
        if (v % 2 === 1) {
            a = Math.max(a, v);
        } else {
            b = Math.min(b, v);
        }
    }
    return a - b;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_difference(s: String) -> i32 {
        let mut cnt = [0; 26];
        for c in s.bytes() {
            cnt[(c - b'a') as usize] += 1;
        }
        let mut a = 0;
        let mut b = 1 << 30;
        for &v in cnt.iter() {
            if v % 2 == 1 {
                a = a.max(v);
            } else if v > 0 {
                b = b.min(v);
            }
        }
        a - b
    }
}
```

#### C#

```cs
public class Solution {
    public int MaxDifference(string s) {
        int[] cnt = new int[26];
        foreach (char c in s) {
            ++cnt[c - 'a'];
        }
        int a = 0, b = 1 << 30;
        foreach (int v in cnt) {
            if (v % 2 == 1) {
                a = Math.Max(a, v);
            } else if (v > 0) {
                b = Math.Min(b, v);
            }
        }
        return a - b;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
