---
comments: true
difficulty: Medium
rating: 1392
source: Weekly Contest 199 Q2
tags:
    - Greedy
    - String
---

<!-- problem:start -->

# [1529. Minimum Suffix Flips](https://leetcode.com/problems/minimum-suffix-flips)

[中文文档](/solution/1500-1599/1529.Minimum%20Suffix%20Flips/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi nhị phân <code>target</code> có độ dài <code>n</code> và được đánh chỉ số từ <strong>0-indexed</strong>. Có một chuỗi nhị phân khác <code>s</code> độ dài <code>n</code>, ban đầu gồm toàn số 0. Mục tiêu là làm cho <code>s</code> bằng <code>target</code>.</p>

<p>Trong một thao tác, bạn có thể chọn chỉ số <code>i</code> với <code>0 &lt;= i &lt; n</code> và đảo mọi bit trong đoạn <strong>bao gồm hai đầu mút</strong> <code>[i, n - 1]</code>. Đảo nghĩa là đổi <code>&#39;0&#39;</code> thành <code>&#39;1&#39;</code> và <code>&#39;1&#39;</code> thành <code>&#39;0&#39;</code>.</p>

<p>Trả về <em>số thao tác ít nhất cần thực hiện để </em><code>s</code><em> bằng </em><code>target</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> target = &quot;10111&quot;
<strong>Output:</strong> 3
<strong>Explanation:</strong> Initially, s = &quot;00000&quot;.
Choose index i = 2: &quot;00<u>000</u>&quot; -&gt; &quot;00<u>111</u>&quot;
Choose index i = 0: &quot;<u>00111</u>&quot; -&gt; &quot;<u>11000</u>&quot;
Choose index i = 1: &quot;1<u>1000</u>&quot; -&gt; &quot;1<u>0111</u>&quot;
Ta cần ít nhất 3 thao tác đảo để tạo target.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> target = &quot;101&quot;
<strong>Output:</strong> 3
<strong>Explanation:</strong> Initially, s = &quot;000&quot;.
Choose index i = 0: &quot;<u>000</u>&quot; -&gt; &quot;<u>111</u>&quot;
Choose index i = 1: &quot;1<u>11</u>&quot; -&gt; &quot;1<u>00</u>&quot;
Choose index i = 2: &quot;10<u>0</u>&quot; -&gt; &quot;10<u>1</u>&quot;
Ta cần ít nhất 3 thao tác đảo để tạo target.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> target = &quot;00000&quot;
<strong>Output:</strong> 0
<strong>Explanation:</strong> We do not need any operations since the initial s already equals target.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == target.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>target[i]</code> là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Một thao tác đảo một hậu tố; ta phải biến các số 0 thành $target$. Vì $n\le 10^5$, không thể mô phỏng mọi hậu tố. Khi một tiền tố đã khớp, lần đảo tiếp theo phải bắt đầu tại vị trí sai khác đầu tiên phía sau.
>
> Duyệt từ trái sang phải: tính chẵn lẻ của số lần đảo biểu diễn bit hiện tại. Nếu khác $target[i]$, ta phải đảo hậu tố tại $i$. Mỗi lần đảo sửa bit sai khác ngoài cùng bên trái mà các thao tác trước không thể thay đổi, nên số lần là ít nhất.

<!-- thinking:end -->

Ta duyệt chuỗi $\textit{target}$ từ trái sang phải và dùng biến $\textit{ans}$ để ghi số lần đảo. Khi đến chỉ số $i$, nếu tính chẵn lẻ của số lần đảo hiện tại $\textit{ans}$ khác $\textit{target}[i]$, ta cần đảo tại chỉ số $i$ và tăng $\textit{ans}$ thêm $1$.

Độ phức tạp thời gian là $O(n)$, với $n$ là độ dài chuỗi. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minFlips(self, target: str) -> int:
        ans = 0
        for v in target:
            if (ans & 1) ^ int(v):
                ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int minFlips(String target) {
        int ans = 0;
        for (int i = 0; i < target.length(); ++i) {
            int v = target.charAt(i) - '0';
            if (((ans & 1) ^ v) != 0) {
                ++ans;
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
    int minFlips(string target) {
        int ans = 0;
        for (char c : target) {
            int v = c - '0';
            if ((ans & 1) ^ v) {
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minFlips(target string) int {
	ans := 0
	for _, c := range target {
		v := int(c - '0')
		if ((ans & 1) ^ v) != 0 {
			ans++
		}
	}
	return ans
}
```

#### TypeScript

```ts
function minFlips(target: string): number {
    let ans = 0;
    for (const c of target) {
        if (ans % 2 !== +c) {
            ++ans;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_flips(target: String) -> i32 {
        let mut ans = 0;
        for c in target.chars() {
            let bit = (c as u8 - b'0') as i32;
            if ans % 2 != bit {
                ans += 1;
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
