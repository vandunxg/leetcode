---
comments: true
difficulty: Easy
rating: 1237
source: Weekly Contest 186 Q1
tags:
    - String
    - Prefix Sum
---

<!-- problem:start -->

# [1422. Maximum Score After Splitting a String](https://leetcode.com/problems/maximum-score-after-splitting-a-string)

[中文文档](/solution/1400-1499/1422.Maximum%20Score%20After%20Splitting%20a%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một string <code>s</code> gồm các số 0 và 1, <em>hãy trả về điểm số lớn nhất sau khi chia string thành hai substring <strong>không rỗng</strong></em> (tức là substring <strong>left</strong> và substring <strong>right</strong>).</p>

<p>Điểm số sau khi chia một string là số lượng <strong>zeros</strong> trong substring <strong>left</strong> cộng với số lượng <strong>ones</strong> trong substring <strong>right</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;011101&quot;
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong>
Tất cả các cách có thể chia s thành hai substring không rỗng là:
left = &quot;0&quot; và right = &quot;11101&quot;, score = 1 + 4 = 5
left = &quot;01&quot; và right = &quot;1101&quot;, score = 1 + 3 = 4
left = &quot;011&quot; và right = &quot;101&quot;, score = 1 + 2 = 3
left = &quot;0111&quot; và right = &quot;01&quot;, score = 1 + 1 = 2
left = &quot;01110&quot; và right = &quot;1&quot;, score = 2 + 1 = 3
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;00111&quot;
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Khi left = &quot;00&quot; và right = &quot;111&quot;, ta nhận được điểm số lớn nhất = 2 + 3 = 5
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;1111&quot;
<strong>Đầu ra:</strong> 3
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= s.length &lt;= 500</code></li>
	<li>String <code>s</code> chỉ gồm các ký tự <code>&#39;0&#39;</code> và <code>&#39;1&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> $n\le 500$ cho phép đếm lại ở mỗi vị trí chia, nhưng điểm số chỉ là số 0 ở bên trái cộng với số 1 ở bên phải.
>
> Bắt đầu với tổng số 1 ở bên phải. Khi di chuyển vị trí chia: một $0$ làm tăng phần bên trái, một $1$ làm giảm phần bên phải. Theo dõi giá trị lớn nhất của $l+r$, và không bao giờ chia sau ký tự cuối cùng.

<!-- thinking:end -->

Ta sử dụng hai biến $l$ và $r$ lần lượt để ghi nhận số lượng 0 trong substring bên trái và số lượng 1 trong substring bên phải. Ban đầu, $l = 0$, còn $r$ bằng số lượng 1 trong string $s$.

Ta duyệt qua $n - 1$ ký tự đầu tiên của string $s$. Với mỗi vị trí $i$, nếu $s[i] = 0$ thì tăng $l$ thêm 1; ngược lại, giảm $r$ đi 1. Sau đó, cập nhật đáp án thành giá trị lớn nhất của $l + r$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của string $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxScore(self, s: str) -> int:
        l, r = 0, s.count("1")
        ans = 0
        for x in s[:-1]:
            l += int(x) ^ 1
            r -= int(x)
            ans = max(ans, l + r)
        return ans
```

#### Java

```java
class Solution {
    public int maxScore(String s) {
        int l = 0, r = 0;
        int n = s.length();
        for (int i = 0; i < n; ++i) {
            if (s.charAt(i) == '1') {
                ++r;
            }
        }
        int ans = 0;
        for (int i = 0; i < n - 1; ++i) {
            l += (s.charAt(i) - '0') ^ 1;
            r -= s.charAt(i) - '0';
            ans = Math.max(ans, l + r);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxScore(string s) {
        int l = 0, r = count(s.begin(), s.end(), '1');
        int ans = 0;
        for (int i = 0; i < s.size() - 1; ++i) {
            l += (s[i] - '0') ^ 1;
            r -= s[i] - '0';
            ans = max(ans, l + r);
        }
        return ans;
    }
};
```

#### Go

```go
func maxScore(s string) (ans int) {
	l, r := 0, strings.Count(s, "1")
	for _, c := range s[:len(s)-1] {
		if c == '0' {
			l++
		} else {
			r--
		}
		ans = max(ans, l+r)
	}
	return
}
```

#### TypeScript

```ts
function maxScore(s: string): number {
    let [l, r] = [0, 0];
    for (const c of s) {
        r += c === '1' ? 1 : 0;
    }
    let ans = 0;
    for (let i = 0; i < s.length - 1; ++i) {
        if (s[i] === '0') {
            ++l;
        } else {
            --r;
        }
        ans = Math.max(ans, l + r);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_score(s: String) -> i32 {
        let mut l = 0;
        let mut r = s.bytes().filter(|&b| b == b'1').count() as i32;
        let mut ans = 0;
        let cs = s.as_bytes();
        for i in 0..s.len() - 1 {
            l += ((cs[i] - b'0') ^ 1) as i32;
            r -= (cs[i] - b'0') as i32;
            ans = ans.max(l + r);
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
