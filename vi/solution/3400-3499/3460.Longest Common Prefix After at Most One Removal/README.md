---
comments: true
difficulty: Medium
tags:
    - Two Pointers
    - String
---

<!-- problem:start -->

# [3460. Longest Common Prefix After at Most One Removal 🔒](https://leetcode.com/problems/longest-common-prefix-after-at-most-one-removal)

[中文文档](/solution/3400-3499/3460.Longest%20Common%20Prefix%20After%20at%20Most%20One%20Removal/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai chuỗi <code>s</code> và <code>t</code>.</p>

<p>Trả về <strong>độ dài</strong> của <strong><span data-keyword="string-prefix">tiền tố chung dài nhất</span></strong> giữa <code>s</code> và <code>t</code> sau khi xóa <strong>nhiều nhất</strong> một ký tự khỏi <code>s</code>.</p>

<p><strong>Lưu ý:</strong> <code>s</code> có thể được giữ nguyên mà không xóa ký tự nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;madxa&quot;, t = &quot;madam&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Xóa <code>s[3]</code> khỏi <code>s</code> cho kết quả <code>&quot;mada&quot;</code>, chuỗi này có tiền tố chung dài nhất với <code>t</code> có độ dài 4.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;leetcode&quot;, t = &quot;eetcode&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<p>Xóa <code>s[0]</code> khỏi <code>s</code> cho kết quả <code>&quot;eetcode&quot;</code>, trùng với <code>t</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;one&quot;, t = &quot;one&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không cần xóa ký tự nào.</p>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;a&quot;, t = &quot;b&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>s</code> và <code>t</code> không có tiền tố chung.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= t.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> và <code>t</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể xóa nhiều nhất một ký tự khỏi $s$ để tối đa hóa tiền tố chung với $t$. Với $n,m\le 10^5$, không thể thử xóa từng ký tự.
>
> Chỉ được bỏ qua một ký tự, nên hai con trỏ sẽ dùng lượt bỏ qua đó ở lần không khớp đầu tiên và phải dừng ở lần không khớp tiếp theo.
>
> $i$ duyệt $s$ còn $j$ duyệt $t$. Khi hai ký tự bằng nhau, tăng cả hai; khi không khớp và $\textit{rem}$ vẫn chưa được dùng, chỉ tăng $i$. Giá trị cuối cùng của $j$ là độ dài tiền tố.

<!-- thinking:end -->

Ta lưu độ dài của hai chuỗi $s$ và $t$ lần lượt là $n$ và $m$. Sau đó, ta dùng hai con trỏ $i$ và $j$ trỏ đến đầu của hai chuỗi $s$ và $t$, đồng thời dùng biến boolean $\textit{rem}$ để ghi nhận xem đã xóa một ký tự hay chưa.

Tiếp theo, ta bắt đầu duyệt hai chuỗi $s$ và $t$. Nếu $s[i]$ khác $t[j]$, ta kiểm tra xem đã xóa một ký tự hay chưa. Nếu đã xóa, ta thoát vòng lặp; nếu chưa, ta đánh dấu đã xóa một ký tự và bỏ qua $s[i]$. Ngược lại, ta bỏ qua cả $s[i]$ và $t[j]$. Tiếp tục duyệt cho đến khi $i \geq n$ hoặc $j \geq m$.

Cuối cùng, trả về $j$.

Độ phức tạp thời gian là $O(n+m)$, trong đó $n$ và $m$ lần lượt là độ dài của hai chuỗi $s$ và $t$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestCommonPrefix(self, s: str, t: str) -> int:
        n, m = len(s), len(t)
        i = j = 0
        rem = False
        while i < n and j < m:
            if s[i] != t[j]:
                if rem:
                    break
                rem = True
            else:
                j += 1
            i += 1
        return j
```

#### Java

```java
class Solution {
    public int longestCommonPrefix(String s, String t) {
        int n = s.length(), m = t.length();
        int i = 0, j = 0;
        boolean rem = false;
        while (i < n && j < m) {
            if (s.charAt(i) != t.charAt(j)) {
                if (rem) {
                    break;
                }
                rem = true;
            } else {
                ++j;
            }
            ++i;
        }
        return j;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestCommonPrefix(string s, string t) {
        int n = s.length(), m = t.length();
        int i = 0, j = 0;
        bool rem = false;
        while (i < n && j < m) {
            if (s[i] != t[j]) {
                if (rem) {
                    break;
                }
                rem = true;
            } else {
                ++j;
            }
            ++i;
        }
        return j;
    }
};
```

#### Go

```go
func longestCommonPrefix(s string, t string) int {
	n, m := len(s), len(t)
	i, j := 0, 0
	rem := false
	for i < n && j < m {
		if s[i] != t[j] {
			if rem {
				break
			}
			rem = true
		} else {
			j++
		}
		i++
	}
	return j
}
```

#### TypeScript

```ts
function longestCommonPrefix(s: string, t: string): number {
    const [n, m] = [s.length, t.length];
    let [i, j] = [0, 0];
    let rem: boolean = false;
    while (i < n && j < m) {
        if (s[i] !== t[j]) {
            if (rem) {
                break;
            }
            rem = true;
        } else {
            ++j;
        }
        ++i;
    }
    return j;
}
```

#### Rust

```rust
impl Solution {
    pub fn longest_common_prefix(s: String, t: String) -> i32 {
        let (n, m) = (s.len(), t.len());
        let (mut i, mut j) = (0, 0);
        let mut rem = false;

        while i < n && j < m {
            if s.as_bytes()[i] != t.as_bytes()[j] {
                if rem {
                    break;
                }
                rem = true;
            } else {
                j += 1;
            }
            i += 1;
        }

        j as i32
    }
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @param {string} t
 * @return {number}
 */
var longestCommonPrefix = function (s, t) {
    const [n, m] = [s.length, t.length];
    let [i, j] = [0, 0];
    let rem = false;
    while (i < n && j < m) {
        if (s[i] !== t[j]) {
            if (rem) {
                break;
            }
            rem = true;
        } else {
            ++j;
        }
        ++i;
    }
    return j;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
