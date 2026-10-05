---
comments: true
difficulty: Medium
rating: 1490
source: Weekly Contest 471 Q2
tags:
    - Hash Table
    - String
    - Counting
    - Enumeration
---

<!-- problem:start -->

# [3713. Longest Balanced Substring I](https://leetcode.com/problems/longest-balanced-substring-i)

[中文文档](/solution/3700-3799/3713.Longest%20Balanced%20Substring%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> gồm các chữ cái tiếng Anh viết thường.</p>

<p>Một <strong><span data-keyword="substring-nonempty">chuỗi con</span></strong> của <code>s</code> được gọi là <strong>cân bằng</strong> nếu tất cả các ký tự <strong>khác nhau</strong> trong <strong>chuỗi con</strong> xuất hiện <strong>cùng số lần</strong>.</p>

<p>Hãy trả về <strong>độ dài</strong> của <strong>chuỗi con cân bằng dài nhất</strong> của <code>s</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abbac&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi con cân bằng dài nhất là <code>&quot;abba&quot;</code> vì cả hai ký tự khác nhau <code>&#39;a&#39;</code> và <code>&#39;b&#39;</code> đều xuất hiện đúng 2 lần.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;zzabccy&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi con cân bằng dài nhất là <code>&quot;zabc&quot;</code> vì các ký tự khác nhau <code>&#39;z&#39;</code>, <code>&#39;a&#39;</code>, <code>&#39;b&#39;</code> và <code>&#39;c&#39;</code> đều xuất hiện đúng 1 lần.​​​​​​​</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;aba&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong>​​​​​​​</strong>Một trong các chuỗi con cân bằng dài nhất là <code>&quot;ab&quot;</code> vì cả hai ký tự khác nhau <code>&#39;a&#39;</code> và <code>&#39;b&#39;</code> đều xuất hiện đúng 1 lần. Một chuỗi con cân bằng dài nhất khác là <code>&quot;ba&quot;</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê + Đếm

<!-- thinking:start -->

> **Tư duy**
>
> $n\le 1000$ cho phép liệt kê mọi chuỗi con. Cân bằng nghĩa là mọi ký tự xuất hiện đều có cùng số lần xuất hiện, tức là $\textit{maxFreq}\times\textit{kinds}=\textit{length}$. Cố định đầu trái và quét sang phải, đồng thời duy trì tần suất và số loại ký tự, ta có thể cập nhật đáp án trong $O(n^2)$.

<!-- thinking:end -->

Ta có thể liệt kê vị trí bắt đầu $i$ của các chuỗi con trong khoảng $[0,..n-1]$, sau đó liệt kê vị trí kết thúc $j$ của các chuỗi con trong khoảng $[i,..,n-1]$, đồng thời dùng bảng băm $\textit{cnt}$ để ghi lại tần suất của mỗi ký tự trong chuỗi con $s[i..j]$. Ta dùng biến $\textit{mx}$ để ghi lại tần suất lớn nhất của các ký tự trong chuỗi con, và biến $v$ để ghi lại số lượng ký tự khác nhau trong chuỗi con. Nếu tại một vị trí $j$ nào đó, ta có $\textit{mx} \times v = j - i + 1$, điều đó có nghĩa chuỗi con $s[i..j]$ là chuỗi con cân bằng, khi đó cập nhật đáp án $\textit{ans} = \max(\textit{ans}, j - i + 1)$.

Độ phức tạp thời gian là $O(n^2)$, trong đó $n$ là độ dài chuỗi. Độ phức tạp không gian là $O(|\Sigma|)$, trong đó $|\Sigma|$ là kích thước của tập ký tự, và trong bài toán này $|\Sigma| = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestBalanced(self, s: str) -> int:
        n = len(s)
        ans = 0
        for i in range(n):
            cnt = Counter()
            mx = v = 0
            for j in range(i, n):
                cnt[s[j]] += 1
                mx = max(mx, cnt[s[j]])
                if cnt[s[j]] == 1:
                    v += 1
                if mx * v == j - i + 1:
                    ans = max(ans, j - i + 1)
        return ans
```

#### Java

```java
class Solution {
    public int longestBalanced(String s) {
        int n = s.length();
        int[] cnt = new int[26];
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            Arrays.fill(cnt, 0);
            int mx = 0, v = 0;
            for (int j = i; j < n; ++j) {
                int c = s.charAt(j) - 'a';
                if (++cnt[c] == 1) {
                    ++v;
                }
                mx = Math.max(mx, cnt[c]);
                if (mx * v == j - i + 1) {
                    ans = Math.max(ans, j - i + 1);
                }
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
    int longestBalanced(string s) {
        int n = s.size();
        vector<int> cnt(26, 0);
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            fill(cnt.begin(), cnt.end(), 0);
            int mx = 0, v = 0;
            for (int j = i; j < n; ++j) {
                int c = s[j] - 'a';
                if (++cnt[c] == 1) {
                    ++v;
                }
                mx = max(mx, cnt[c]);
                if (mx * v == j - i + 1) {
                    ans = max(ans, j - i + 1);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func longestBalanced(s string) (ans int) {
	n := len(s)
	for i := 0; i < n; i++ {
		cnt := [26]int{}
		mx, v := 0, 0
		for j := i; j < n; j++ {
			c := s[j] - 'a'
			cnt[c]++
			if cnt[c] == 1 {
				v++
			}
			mx = max(mx, cnt[c])
			if mx*v == j-i+1 {
				ans = max(ans, j-i+1)
			}
		}
	}

	return ans
}
```

#### TypeScript

```ts
function longestBalanced(s: string): number {
    const n = s.length;
    let ans: number = 0;
    for (let i = 0; i < n; ++i) {
        const cnt: number[] = Array(26).fill(0);
        let [mx, v] = [0, 0];
        for (let j = i; j < n; ++j) {
            const c = s[j].charCodeAt(0) - 97;
            if (++cnt[c] === 1) {
                ++v;
            }
            mx = Math.max(mx, cnt[c]);
            if (mx * v === j - i + 1) {
                ans = Math.max(ans, j - i + 1);
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn longest_balanced(s: String) -> i32 {
        let n: i32 = s.len() as i32;
        let bytes = s.as_bytes();
        let mut ans: i32 = 0;

        for i in 0..n {
            let mut cnt: [i32; 26] = [0; 26];
            let mut mx: i32 = 0;
            let mut v: i32 = 0;

            for j in i..n {
                let c: usize = (bytes[j as usize] - b'a') as usize;
                cnt[c] += 1;

                if cnt[c] == 1 {
                    v += 1;
                }

                mx = mx.max(cnt[c]);

                if mx * v == j - i + 1 {
                    ans = ans.max(j - i + 1);
                }
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
