---
comments: true
difficulty: Easy
tags:
    - Two Pointers
    - String
---

<!-- problem:start -->

# [696. Count Binary Substrings](https://leetcode.com/problems/count-binary-substrings)

[中文文档](/solution/0600-0699/0696.Count%20Binary%20Substrings/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi nhị phân <code>s</code>, hãy trả về số chuỗi con không rỗng có cùng số lượng <code>0</code> và <code>1</code>, trong đó tất cả các <code>0</code> và tất cả các <code>1</code> trong chuỗi con đều nằm thành từng nhóm liên tiếp.</p>

<p>Nếu một chuỗi con xuất hiện nhiều lần thì tính đủ số lần xuất hiện đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;00110011&quot;
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Có 6 chuỗi con có số lượng các số 1 liên tiếp bằng số lượng các số 0 liên tiếp: &quot;0011&quot;, &quot;01&quot;, &quot;1100&quot;, &quot;10&quot;, &quot;0011&quot; và &quot;01&quot;.
Lưu ý, một số chuỗi con bị lặp và được tính theo số lần chúng xuất hiện.
Ngoài ra, &quot;00110011&quot; không phải chuỗi con hợp lệ vì các số 0 (và số 1) không nằm thành từng nhóm liên tiếp.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;10101&quot;
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Có 4 chuỗi con: &quot;10&quot;, &quot;01&quot;, &quot;10&quot;, &quot;01&quot;, có số lượng các số 1 liên tiếp bằng số lượng các số 0 liên tiếp.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s[i]</code> là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt và đếm

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các chuỗi con gồm một nhóm $0$s nằm cạnh một nhóm $1$s có cùng độ dài. Kiểm tra mọi chuỗi con sẽ có độ phức tạp bậc hai.
>
> Chia chuỗi thành các nhóm ký tự giống nhau liên tiếp. Mỗi cặp nhóm liền kề đóng góp $\min(\textit{pre},\textit{cur})$ chuỗi con hợp lệ. Chỉ cần duyệt các nhóm một lần.

<!-- thinking:end -->

Ta có thể duyệt chuỗi $s$, dùng biến $\textit{pre}$ để ghi nhận số ký tự trong nhóm liên tiếp trước đó và biến $\textit{cur}$ để ghi nhận số ký tự trong nhóm hiện tại. Số chuỗi con hợp lệ kết thúc tại ký tự hiện tại là $\min(\textit{pre}, \textit{cur})$. Ta cộng giá trị này vào đáp án, gán $\textit{cur}$ cho $\textit{pre}$, rồi tiếp tục duyệt chuỗi $s$ đến hết.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countBinarySubstrings(self, s: str) -> int:
        n = len(s)
        ans = i = 0
        pre = 0
        while i < n:
            j = i + 1
            while j < n and s[j] == s[i]:
                j += 1
            cur = j - i
            ans += min(pre, cur)
            pre = cur
            i = j
        return ans
```

#### Java

```java
class Solution {
    public int countBinarySubstrings(String s) {
        int n = s.length();
        int ans = 0;
        int i = 0;
        int pre = 0;
        while (i < n) {
            int j = i + 1;
            while (j < n && s.charAt(j) == s.charAt(i)) {
                j++;
            }
            int cur = j - i;
            ans += Math.min(pre, cur);
            pre = cur;
            i = j;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countBinarySubstrings(string s) {
        int n = s.size();
        int ans = 0;
        int i = 0;
        int pre = 0;
        while (i < n) {
            int j = i + 1;
            while (j < n && s[j] == s[i]) {
                ++j;
            }
            int cur = j - i;
            ans += min(pre, cur);
            pre = cur;
            i = j;
        }
        return ans;
    }
};
```

#### Go

```go
func countBinarySubstrings(s string) (ans int) {
	n := len(s)
	i := 0
	pre := 0
	for i < n {
		j := i + 1
		for j < n && s[j] == s[i] {
			j++
		}
		cur := j - i
		ans += min(pre, cur)
		pre = cur
		i = j
	}
	return
}
```

#### TypeScript

```ts
function countBinarySubstrings(s: string): number {
    const n = s.length;
    let ans = 0;
    let i = 0;
    let pre = 0;

    while (i < n) {
        let j = i + 1;
        while (j < n && s[j] === s[i]) {
            j++;
        }
        const cur = j - i;
        ans += Math.min(pre, cur);
        pre = cur;
        i = j;
    }

    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_binary_substrings(s: String) -> i32 {
        let bytes = s.as_bytes();
        let n: usize = bytes.len();

        let mut ans: i32 = 0;
        let mut i: usize = 0;
        let mut pre: i32 = 0;

        while i < n {
            let mut j: usize = i + 1;
            while j < n && bytes[j] == bytes[i] {
                j += 1;
            }
            let cur: i32 = (j - i) as i32;
            ans += pre.min(cur);
            pre = cur;
            i = j;
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
