---
comments: true
difficulty: Medium
rating: 1896
source: Weekly Contest 242 Q3
tags:
    - String
    - Dynamic Programming
    - Prefix Sum
    - Sliding Window
---

<!-- problem:start -->

# [1871. Jump Game VII](https://leetcode.com/problems/jump-game-vii)

[中文文档](/solution/1800-1899/1871.Jump%20Game%20VII/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi nhị phân <strong>đánh chỉ số từ 0</strong> <code>s</code> và hai số nguyên <code>minJump</code>, <code>maxJump</code>. Ban đầu, bạn đứng tại chỉ số <code>0</code>, có giá trị bằng <code>&#39;0&#39;</code>. Bạn có thể di chuyển từ chỉ số <code>i</code> đến chỉ số <code>j</code> nếu thỏa mãn các điều kiện sau:</p>

<ul>
	<li><code>i + minJump &lt;= j &lt;= min(i + maxJump, s.length - 1)</code>, and</li>
	<li><code>s[j] == &#39;0&#39;</code>.</li>
</ul>

<p>Trả về <code>true</code><i> nếu bạn có thể đến chỉ số </i><code>s.length - 1</code><i> trong </i><code>s</code><em>, ngược lại trả về </em><code>false</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;<u>0</u>11<u>0</u>1<u>0</u>&quot;, minJump = 2, maxJump = 3
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong>
Bước đầu tiên, di chuyển từ chỉ số 0 đến chỉ số 3.
Bước thứ hai, di chuyển từ chỉ số 3 đến chỉ số 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;01101110&quot;, minJump = 2, maxJump = 3
<strong>Đầu ra:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s[i]</code> is either <code>&#39;0&#39;</code> or <code>&#39;1&#39;</code>.</li>
	<li><code>s[0] == &#39;0&#39;</code></li>
	<li><code>1 &lt;= minJump &lt;= maxJump &lt; s.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum + Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Từ chỉ số $0$, ta có thể nhảy đến một ký tự `'0'` có khoảng cách nằm trong $[minJump,maxJump]$. Kiểm tra toàn bộ cửa sổ nhảy tại mỗi chỉ số là quá chậm với $n\le 10^5$.
>
> $f[i]$ là true khi và chỉ khi $s[i]='0'$ và có một chỉ số có thể đi đến nằm trong $[i-maxJump,i-minJump]$. Prefix sum của $f$ giúp trả lời khoảng này trong $O(1)$, nên ta điền $f$ từ trái sang phải.

<!-- thinking:end -->

Ta định nghĩa mảng prefix sum $pre$ có độ dài $n+1$, trong đó $pre[i]$ biểu thị số vị trí có thể đi đến trong $i$ vị trí đầu tiên của $s$. Ta định nghĩa mảng boolean $f$ có độ dài $n$, trong đó $f[i]$ cho biết có thể đi đến $s[i]$ hay không. Ban đầu, $pre[1] = 1$ và $f[0] = true$.

Xét $i \in [1, n)$, nếu $s[i] = 0$, ta cần xác định liệu có vị trí $j$ trong $i$ vị trí đầu tiên của $s$ sao cho có thể đi đến $j$ và khoảng cách từ $j$ đến $i$ nằm trong $[minJump, maxJump]$ hay không. Nếu tồn tại vị trí $j$ như vậy thì $f[i] = true$, ngược lại $f[i] = false$. Khi xác định $j$ có tồn tại vị trí $j$ như vậy hay không, ta có thể dùng mảng prefix sum $pre$ để kiểm tra trong thời gian $O(1)$.

Đáp án cuối cùng là $f[n-1]$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canReach(self, s: str, minJump: int, maxJump: int) -> bool:
        n = len(s)
        pre = [0] * (n + 1)
        pre[1] = 1
        f = [True] + [False] * (n - 1)
        for i in range(1, n):
            if s[i] == "0":
                l, r = max(0, i - maxJump), i - minJump
                f[i] = l <= r and pre[r + 1] - pre[l] > 0
            pre[i + 1] = pre[i] + f[i]
        return f[-1]
```

#### Java

```java
class Solution {
    public boolean canReach(String s, int minJump, int maxJump) {
        int n = s.length();
        int[] pre = new int[n + 1];
        pre[1] = 1;
        boolean[] f = new boolean[n];
        f[0] = true;
        for (int i = 1; i < n; ++i) {
            if (s.charAt(i) == '0') {
                int l = Math.max(0, i - maxJump);
                int r = i - minJump;
                f[i] = l <= r && pre[r + 1] - pre[l] > 0;
            }
            pre[i + 1] = pre[i] + (f[i] ? 1 : 0);
        }
        return f[n - 1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canReach(string s, int minJump, int maxJump) {
        int n = s.size();
        vector<int> pre(n + 1);
        pre[1] = 1;
        vector<bool> f(n);
        f[0] = true;
        for (int i = 1; i < n; ++i) {
            if (s[i] == '0') {
                int l = max(0, i - maxJump);
                int r = i - minJump;
                f[i] = l <= r && pre[r + 1] - pre[l];
            }
            pre[i + 1] = pre[i] + f[i];
        }
        return f[n - 1];
    }
};
```

#### Go

```go
func canReach(s string, minJump int, maxJump int) bool {
	n := len(s)
	pre := make([]int, n+1)
	pre[1] = 1
	f := make([]bool, n)
	f[0] = true
	for i := 1; i < n; i++ {
		if s[i] == '0' {
			l, r := max(0, i-maxJump), i-minJump
			f[i] = l <= r && pre[r+1]-pre[l] > 0
		}
		pre[i+1] = pre[i]
		if f[i] {
			pre[i+1]++
		}
	}
	return f[n-1]
}
```

#### TypeScript

```ts
function canReach(s: string, minJump: number, maxJump: number): boolean {
    const n = s.length;
    const pre: number[] = Array(n + 1).fill(0);
    pre[1] = 1;
    const f: boolean[] = Array(n).fill(false);
    f[0] = true;
    for (let i = 1; i < n; ++i) {
        if (s[i] === '0') {
            const [l, r] = [Math.max(0, i - maxJump), i - minJump];
            f[i] = l <= r && pre[r + 1] - pre[l] > 0;
        }
        pre[i + 1] = pre[i] + (f[i] ? 1 : 0);
    }
    return f[n - 1];
}
```

#### Rust

```rust
impl Solution {
    pub fn can_reach(s: String, min_jump: i32, max_jump: i32) -> bool {
        let s = s.as_bytes();
        let n = s.len();
        let min_jump = min_jump as usize;
        let max_jump = max_jump as usize;

        let mut pre = vec![0; n + 1];
        pre[1] = 1;

        let mut f = vec![false; n];
        f[0] = true;

        for i in 1..n {
            if s[i] == b'0' {
                let l = i.saturating_sub(max_jump);
                if i >= min_jump {
                    let r = i - min_jump;
                    f[i] = l <= r && pre[r + 1] - pre[l] > 0;
                }
            }
            pre[i + 1] = pre[i] + if f[i] { 1 } else { 0 };
        }

        f[n - 1]
    }
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @param {number} minJump
 * @param {number} maxJump
 * @return {boolean}
 */
var canReach = function (s, minJump, maxJump) {
    const n = s.length;
    const pre = Array(n + 1).fill(0);
    pre[1] = 1;
    const f = Array(n).fill(false);
    f[0] = true;
    for (let i = 1; i < n; ++i) {
        if (s[i] === '0') {
            const [l, r] = [Math.max(0, i - maxJump), i - minJump];
            f[i] = l <= r && pre[r + 1] - pre[l] > 0;
        }
        pre[i + 1] = pre[i] + (f[i] ? 1 : 0);
    }
    return f[n - 1];
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
