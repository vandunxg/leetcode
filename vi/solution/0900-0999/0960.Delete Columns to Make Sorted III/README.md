---
comments: true
difficulty: Hard
tags:
    - Array
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [960. Delete Columns to Make Sorted III](https://leetcode.com/problems/delete-columns-to-make-sorted-iii)

[中文文档](/solution/0900-0999/0960.Delete%20Columns%20to%20Make%20Sorted%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng gồm <code>n</code> chuỗi <code>strs</code>, tất cả có cùng độ dài.</p>

<p>Ta có thể chọn các chỉ số cột cần xóa; với mỗi chuỗi, xóa ký tự ở các chỉ số đó.</p>

<p>Ví dụ, nếu <code>strs = [&quot;abcdef&quot;,&quot;uvwxyz&quot;]</code> và các chỉ số cột cần xóa là <code>{0, 2, 3}</code>, thì mảng sau khi xóa là <code>[&quot;bef&quot;, &quot;vyz&quot;]</code>.</p>

<p>Giả sử ta chọn một tập chỉ số cột <code>answer</code> để xóa, sao cho sau khi xóa, <strong>từng chuỗi (từng hàng) trong mảng đều theo thứ tự từ điển</strong> (tức là <code>(strs[0][0] &lt;= strs[0][1] &lt;= ... &lt;= strs[0][strs[0].length - 1])</code>, <code>(strs[1][0] &lt;= strs[1][1] &lt;= ... &lt;= strs[1][strs[1].length - 1])</code>, v.v.). Trả về <em>giá trị nhỏ nhất có thể của</em> <code>answer.length</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> strs = [&quot;babca&quot;,&quot;bbazb&quot;]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Sau khi xóa các cột 0, 1 và 4, mảng thu được là strs = [&quot;bc&quot;, &quot;az&quot;].
Mỗi hàng đều theo thứ tự từ điển (tức là strs[0][0] &lt;= strs[0][1] và strs[1][0] &lt;= strs[1][1]).
Lưu ý, strs[0] &gt; strs[1] — bản thân mảng strs không nhất thiết phải theo thứ tự từ điển.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> strs = [&quot;edcba&quot;]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Nếu xóa ít hơn 4 cột, hàng duy nhất sẽ không được sắp xếp theo thứ tự từ điển.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> strs = [&quot;ghi&quot;,&quot;def&quot;,&quot;abc&quot;]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Tất cả các hàng đã theo thứ tự từ điển.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == strs.length</code></li>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>1 &lt;= strs[i].length &lt;= 100</code></li>
	<li><code>strs[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<ul>
	<li>&nbsp;</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần xóa ít cột nhất sao cho các cột còn lại không giảm trong từng hàng. Đây là bài toán tìm dãy con dài nhất gồm các cột không giảm ở mọi hàng; số cột bị xóa bằng $n$ trừ độ dài dãy đó. $f[i]$ là dãy kết thúc tại cột $i$, có thể nối tiếp từ mọi $j<i$ nếu $s[j]\le s[i]$ ở tất cả các hàng.

<!-- thinking:end -->

Ta định nghĩa $f[i]$ là độ dài dãy con không giảm dài nhất kết thúc ở cột $i$. Ban đầu, $f[i] = 1$, và đáp án cuối cùng là $n - \max(f)$.

Để tính $f[i]$, ta duyệt mọi $j < i$. Nếu với tất cả chuỗi $s$, ta có $s[j] \leq s[i]$, thì cập nhật $f[i]$ như sau:
$$ f[i] = \max(f[i], f[j] + 1) $$

Cuối cùng, trả về $n - \max(f)$.

Độ phức tạp thời gian là $O(n^2 \times m)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài mỗi chuỗi trong mảng $\textit{strs}$, còn $m$ là số chuỗi trong mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minDeletionSize(self, strs: List[str]) -> int:
        n = len(strs[0])
        f = [1] * n
        for i in range(n):
            for j in range(i):
                if all(s[j] <= s[i] for s in strs):
                    f[i] = max(f[i], f[j] + 1)
        return n - max(f)
```

#### Java

```java
class Solution {
    public int minDeletionSize(String[] strs) {
        int n = strs[0].length();
        int[] f = new int[n];
        Arrays.fill(f, 1);
        for (int i = 1; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                boolean ok = true;
                for (String s : strs) {
                    if (s.charAt(j) > s.charAt(i)) {
                        ok = false;
                        break;
                    }
                }
                if (ok) {
                    f[i] = Math.max(f[i], f[j] + 1);
                }
            }
        }
        return n - Arrays.stream(f).max().getAsInt();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minDeletionSize(vector<string>& strs) {
        int n = strs[0].size();
        vector<int> f(n, 1);
        for (int i = 1; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                if (ranges::all_of(strs, [&](const string& s) { return s[j] <= s[i]; })) {
                    f[i] = max(f[i], f[j] + 1);
                }
            }
        }
        return n - ranges::max(f);
    }
};
```

#### Go

```go
func minDeletionSize(strs []string) int {
	n := len(strs[0])
	f := make([]int, n)
	for i := range f {
		f[i] = 1
	}
	for i := 1; i < n; i++ {
		for j := 0; j < i; j++ {
			ok := true
			for _, s := range strs {
				if s[j] > s[i] {
					ok = false
					break
				}
			}
			if ok {
				f[i] = max(f[i], f[j]+1)
			}
		}
	}
	return n - slices.Max(f)
}
```

#### TypeScript

```ts
function minDeletionSize(strs: string[]): number {
    const n = strs[0].length;
    const f: number[] = Array(n).fill(1);
    for (let i = 1; i < n; i++) {
        for (let j = 0; j < i; j++) {
            let ok = true;
            for (const s of strs) {
                if (s[j] > s[i]) {
                    ok = false;
                    break;
                }
            }
            if (ok) {
                f[i] = Math.max(f[i], f[j] + 1);
            }
        }
    }
    return n - Math.max(...f);
}
```

#### Rust

```rust
impl Solution {
    pub fn min_deletion_size(strs: Vec<String>) -> i32 {
        let n = strs[0].len();
        let mut f = vec![1; n];

        for i in 1..n {
            for j in 0..i {
                if strs.iter().all(|s| s.as_bytes()[j] <= s.as_bytes()[i]) {
                    f[i] = f[i].max(f[j] + 1);
                }
            }
        }

        (n - *f.iter().max().unwrap()) as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
