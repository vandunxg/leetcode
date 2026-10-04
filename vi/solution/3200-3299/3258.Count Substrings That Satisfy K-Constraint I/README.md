---
comments: true
difficulty: Easy
rating: 1258
source: Weekly Contest 411 Q1
tags:
    - String
    - Sliding Window
---

<!-- problem:start -->

# [3258. Count Substrings That Satisfy K-Constraint I](https://leetcode.com/problems/count-substrings-that-satisfy-k-constraint-i)

[中文文档](/solution/3200-3299/3258.Count%20Substrings%20That%20Satisfy%20K-Constraint%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Đã cho một chuỗi <strong>nhị phân</strong> <code>s</code> và một số nguyên <code>k</code>.</p>

<p>Một <strong>chuỗi nhị phân</strong> thỏa mãn <strong>ràng buộc k</strong> nếu <strong>một trong hai</strong> điều kiện sau được thỏa mãn:</p>

<ul>
	<li>Số lượng <code>0</code> trong chuỗi không vượt quá <code>k</code>.</li>
	<li>Số lượng <code>1</code> trong chuỗi không vượt quá <code>k</code>.</li>
</ul>

<p>Trả về một số nguyên biểu thị số lượng <span data-keyword="substring-nonempty">chuỗi con</span> của <code>s</code> thỏa mãn <strong>ràng buộc k</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;10101&quot;, k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mọi chuỗi con của <code>s</code>, ngoại trừ các chuỗi con <code>&quot;1010&quot;</code>, <code>&quot;10101&quot;</code> và <code>&quot;0101&quot;</code>, đều thỏa mãn ràng buộc k.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;1010101&quot;, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">25</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mọi chuỗi con của <code>s</code> có độ dài không vượt quá 5 đều thỏa mãn ràng buộc k.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;11111&quot;, k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">15</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mọi chuỗi con của <code>s</code> đều thỏa mãn ràng buộc k.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 50 </code></li>
	<li><code>1 &lt;= k &lt;= s.length</code></li>
	<li><code>s[i]</code> là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Cửa sổ trượt

<!-- thinking:start -->

> **Tư duy**
>
> Ràng buộc $k$ yêu cầu một chuỗi con có nhiều nhất $k$ số 0 hoặc nhiều nhất $k$ số 1. Với $n\le 50$, ta có thể liệt kê các chuỗi con, nhưng tính hợp lệ có tính đơn điệu khi mở rộng về bên phải: một khi cả hai số lượng đều vượt quá $k$, ta phải dịch điểm đầu sang phải.
>
> Cửa sổ trượt duy trì $c_0,c_1$ và thu hẹp cửa sổ khi bị vượt giới hạn. Khi đó, mọi chuỗi con kết thúc tại $r$ có điểm đầu trong $[l,r]$ đều hợp lệ, nên có tất cả $r-l+1$ chuỗi con. Ta chỉ cần duyệt chuỗi một lần.

<!-- thinking:end -->

Ta dùng hai biến $\textit{cnt0}$ và $\textit{cnt1}$ để lần lượt ghi nhận số lượng $0$ và $1$ trong cửa sổ hiện tại. Ta dùng $\textit{ans}$ để ghi nhận số lượng chuỗi con thỏa mãn ràng buộc $k$, và $l$ để ghi nhận biên trái của cửa sổ.

Khi dịch cửa sổ sang phải, nếu số lượng số $0$ và số $1$ trong cửa sổ đều vượt quá $k$, ta cần dịch cửa sổ sang trái cho đến khi số lượng số $0$ và số $1$ trong cửa sổ đều không vượt quá $k$. Khi đó, mọi chuỗi con trong cửa sổ đều thỏa mãn ràng buộc $k$, và số lượng các chuỗi con này là $r - l + 1$, trong đó $r$ là biên phải của cửa sổ. Ta cộng số lượng này vào $\textit{ans}$.

Cuối cùng, ta trả về $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countKConstraintSubstrings(self, s: str, k: int) -> int:
        cnt = [0, 0]
        ans = l = 0
        for r, x in enumerate(map(int, s)):
            cnt[x] += 1
            while cnt[0] > k and cnt[1] > k:
                cnt[int(s[l])] -= 1
                l += 1
            ans += r - l + 1
        return ans
```

#### Java

```java
class Solution {
    public int countKConstraintSubstrings(String s, int k) {
        int[] cnt = new int[2];
        int ans = 0, l = 0;
        for (int r = 0; r < s.length(); ++r) {
            ++cnt[s.charAt(r) - '0'];
            while (cnt[0] > k && cnt[1] > k) {
                cnt[s.charAt(l++) - '0']--;
            }
            ans += r - l + 1;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countKConstraintSubstrings(string s, int k) {
        int cnt[2]{};
        int ans = 0, l = 0;
        for (int r = 0; r < s.length(); ++r) {
            cnt[s[r] - '0']++;
            while (cnt[0] > k && cnt[1] > k) {
                cnt[s[l++] - '0']--;
            }
            ans += r - l + 1;
        }
        return ans;
    }
};
```

#### Go

```go
func countKConstraintSubstrings(s string, k int) (ans int) {
	cnt := [2]int{}
	l := 0
	for r, c := range s {
		cnt[c-'0']++
		for ; cnt[0] > k && cnt[1] > k; l++ {
			cnt[s[l]-'0']--
		}
		ans += r - l + 1
	}
	return
}
```

#### TypeScript

```ts
function countKConstraintSubstrings(s: string, k: number): number {
    const cnt: [number, number] = [0, 0];
    let [ans, l] = [0, 0];
    for (let r = 0; r < s.length; ++r) {
        cnt[+s[r]]++;
        while (cnt[0] > k && cnt[1] > k) {
            cnt[+s[l++]]--;
        }
        ans += r - l + 1;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_k_constraint_substrings(s: String, k: i32) -> i32 {
        let mut cnt = [0; 2];
        let mut l = 0;
        let mut ans = 0;
        let s = s.as_bytes();

        for (r, &c) in s.iter().enumerate() {
            cnt[(c - b'0') as usize] += 1;
            while cnt[0] > k && cnt[1] > k {
                cnt[(s[l] - b'0') as usize] -= 1;
                l += 1;
            }
            ans += r - l + 1;
        }

        ans as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
