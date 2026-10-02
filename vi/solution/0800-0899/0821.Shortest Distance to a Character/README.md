---
comments: true
difficulty: Easy
tags:
    - Array
    - Two Pointers
    - String
---

<!-- problem:start -->

# [821. Shortest Distance to a Character](https://leetcode.com/problems/shortest-distance-to-a-character)

[中文文档](/solution/0800-0899/0821.Shortest%20Distance%20to%20a%20Character/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> và ký tự <code>c</code> xuất hiện trong <code>s</code>. Hãy trả về <em>mảng số nguyên </em><code>answer</code><em> sao cho </em><code>answer.length == s.length</code><em> và </em><code>answer[i]</code><em> là <strong>khoảng cách</strong> từ chỉ số </em><code>i</code><em> đến vị trí xuất hiện </em><code>c</code><em> <strong>gần nhất</strong> trong </em><code>s</code>.</p>

<p><strong>Khoảng cách</strong> giữa hai chỉ số <code>i</code> và <code>j</code> là <code>abs(i - j)</code>, trong đó <code>abs</code> là hàm giá trị tuyệt đối.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;loveleetcode&quot;, c = &quot;e&quot;
<strong>Đầu ra:</strong> [3,2,1,0,1,0,0,1,2,2,1,0]
<strong>Giải thích:</strong> Ký tự &#39;e&#39; xuất hiện tại các chỉ số 3, 5, 6 và 11 (đánh chỉ số từ 0).
Vị trí xuất hiện &#39;e&#39; gần chỉ số 0 nhất là chỉ số 3, nên khoảng cách là abs(0 - 3) = 3.
Vị trí xuất hiện &#39;e&#39; gần chỉ số 1 nhất là chỉ số 3, nên khoảng cách là abs(1 - 3) = 2.
Với chỉ số 4, hai vị trí chứa &#39;e&#39; tại chỉ số 3 và 5 có khoảng cách bằng nhau: abs(4 - 3) == abs(4 - 5) = 1.
Vị trí xuất hiện &#39;e&#39; gần chỉ số 8 nhất là chỉ số 6, nên khoảng cách là abs(8 - 6) = 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aaab&quot;, c = &quot;b&quot;
<strong>Đầu ra:</strong> [3,2,1,0]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>4</sup></code></li>
	<li><code>s[i]</code> và <code>c</code> là các chữ cái tiếng Anh viết thường.</li>
	<li>Đảm bảo <code>c</code> xuất hiện ít nhất một lần trong <code>s</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi chỉ số, cần tìm khoảng cách đến $c$ gần nhất. Quét cả hai phía từ từng vị trí sẽ tốn thời gian bậc hai và không cần thiết với $n\le 10^4$. Ký tự $c$ gần nhất nằm ở bên trái hoặc bên phải.
>
> Lượt duyệt từ trái sang phải ghi nhận vị trí $c$ gần nhất ở bên trái; lượt duyệt từ phải sang trái chọn khoảng cách nhỏ hơn giữa hai phía. Hai lượt duyệt tuyến tính là đủ để điền đáp án.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def shortestToChar(self, s: str, c: str) -> List[int]:
        n = len(s)
        ans = [n] * n
        pre = -inf
        for i, ch in enumerate(s):
            if ch == c:
                pre = i
            ans[i] = min(ans[i], i - pre)
        suf = inf
        for i in range(n - 1, -1, -1):
            if s[i] == c:
                suf = i
            ans[i] = min(ans[i], suf - i)
        return ans
```

#### Java

```java
class Solution {
    public int[] shortestToChar(String s, char c) {
        int n = s.length();
        int[] ans = new int[n];
        final int inf = 1 << 30;
        Arrays.fill(ans, inf);
        for (int i = 0, pre = -inf; i < n; ++i) {
            if (s.charAt(i) == c) {
                pre = i;
            }
            ans[i] = Math.min(ans[i], i - pre);
        }
        for (int i = n - 1, suf = inf; i >= 0; --i) {
            if (s.charAt(i) == c) {
                suf = i;
            }
            ans[i] = Math.min(ans[i], suf - i);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> shortestToChar(string s, char c) {
        int n = s.size();
        const int inf = 1 << 30;
        vector<int> ans(n, inf);
        for (int i = 0, pre = -inf; i < n; ++i) {
            if (s[i] == c) {
                pre = i;
            }
            ans[i] = min(ans[i], i - pre);
        }
        for (int i = n - 1, suf = inf; ~i; --i) {
            if (s[i] == c) {
                suf = i;
            }
            ans[i] = min(ans[i], suf - i);
        }
        return ans;
    }
};
```

#### Go

```go
func shortestToChar(s string, c byte) []int {
	n := len(s)
	ans := make([]int, n)
	const inf int = 1 << 30
	pre := -inf
	for i := range s {
		if s[i] == c {
			pre = i
		}
		ans[i] = i - pre
	}
	suf := inf
	for i := n - 1; i >= 0; i-- {
		if s[i] == c {
			suf = i
		}
		ans[i] = min(ans[i], suf-i)
	}
	return ans
}
```

#### TypeScript

```ts
function shortestToChar(s: string, c: string): number[] {
    const n = s.length;
    const inf = 1 << 30;
    const ans: number[] = new Array(n).fill(inf);
    for (let i = 0, pre = -inf; i < n; ++i) {
        if (s[i] === c) {
            pre = i;
        }
        ans[i] = i - pre;
    }
    for (let i = n - 1, suf = inf; i >= 0; --i) {
        if (s[i] === c) {
            suf = i;
        }
        ans[i] = Math.min(ans[i], suf - i);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn shortest_to_char(s: String, c: char) -> Vec<i32> {
        let c = c as u8;
        let s = s.as_bytes();
        let n = s.len();
        let mut res = vec![i32::MAX; n];
        let mut pre = i32::MAX;
        for i in 0..n {
            if s[i] == c {
                pre = i as i32;
            }
            res[i] = i32::abs((i as i32) - pre);
        }
        pre = i32::MAX;
        for i in (0..n).rev() {
            if s[i] == c {
                pre = i as i32;
            }
            res[i] = res[i].min(i32::abs((i as i32) - pre));
        }
        res
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
