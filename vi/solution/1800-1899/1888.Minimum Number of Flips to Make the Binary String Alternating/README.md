---
comments: true
difficulty: Medium
rating: 2005
source: Weekly Contest 244 Q3
tags:
    - String
    - Dynamic Programming
    - Sliding Window
---

<!-- problem:start -->

# [1888. Minimum Number of Flips to Make the Binary String Alternating](https://leetcode.com/problems/minimum-number-of-flips-to-make-the-binary-string-alternating)

[中文文档](/solution/1800-1899/1888.Minimum%20Number%20of%20Flips%20to%20Make%20the%20Binary%20String%20Alternating/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi nhị phân <code>s</code>. Bạn được phép thực hiện hai loại thao tác trên chuỗi theo bất kỳ thứ tự nào:</p>

<ul>
	<li><strong>Loại 1: Xóa</strong> ký tự ở đầu chuỗi <code>s</code> và <strong>thêm</strong> ký tự đó vào cuối chuỗi.</li>
	<li><strong>Loại 2: Chọn</strong> bất kỳ ký tự nào trong <code>s</code> và <strong>đảo</strong> giá trị của nó, tức là nếu giá trị là <code>&#39;0&#39;</code> thì đổi thành <code>&#39;1&#39;</code> và ngược lại.</li>
</ul>

<p>Trả về <em><strong>số thao tác</strong> <strong>loại 2</strong> nhỏ nhất cần thực hiện</em> <em>để </em><code>s</code> <em>trở thành một chuỗi <strong>xen kẽ</strong>.</em></p>

<p>Một chuỗi được gọi là <strong>xen kẽ</strong> nếu không có hai ký tự kề nhau bằng nhau.</p>

<ul>
	<li>Ví dụ, các chuỗi <code>&quot;010&quot;</code> và <code>&quot;1010&quot;</code> là chuỗi xen kẽ, còn chuỗi <code>&quot;0100&quot;</code> thì không.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;111000&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích</strong>: Dùng thao tác thứ nhất hai lần để được s = &quot;100011&quot;.
Sau đó, dùng thao tác thứ hai lên phần tử thứ ba và thứ sáu để được s = &quot;10<u>1</u>01<u>0</u>&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;010&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích</strong>: Chuỗi đã xen kẽ.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;1110&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích</strong>: Dùng thao tác thứ hai lên phần tử thứ hai để được s = &quot;1<u>0</u>10&quot;.
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

### Lời giải 1: Cửa sổ trượt

<!-- thinking:start -->

> **Tư duy**
>
> Thao tác loại 1 biến chuỗi thành một vòng tròn; thao tác loại 2 đảo bit để khớp một mẫu xen kẽ. So sánh từng phép xoay theo từng bit có độ phức tạp $O(n^2)$, quá chậm khi $n\le 10^5$.
>
> Chỉ có hai mẫu đích, và chi phí của chúng có tổng bằng $n$. Một cửa sổ độ dài $n$ trên chuỗi vòng tròn tương ứng với một phép xoay; ta trượt cửa sổ, cập nhật số sai khác và giữ $\min(cnt,n-cnt)$.

<!-- thinking:end -->

Ta nhận thấy thao tác $1$ thực chất biến chuỗi thành một vòng tròn, còn thao tác $2$ biến một chuỗi con độ dài $n$ trong vòng tròn thành chuỗi nhị phân xen kẽ.

Do đó, ta chỉ cần duyệt qua từng chuỗi con độ dài $n$, tính chi phí để biến nó thành chuỗi nhị phân xen kẽ và lấy giá trị nhỏ nhất.

Ta có thể tính trước số vị trí khác nhau giữa chuỗi $s$ và hai loại chuỗi nhị phân xen kẽ, gọi là $\textit{cnt}$. Chi phí để biến $s$ thành loại thứ nhất là $\textit{cnt}$, còn chi phí để biến $s$ thành loại thứ hai là $n - \textit{cnt}$. Ta khởi tạo $\textit{ans} = \min(\textit{cnt}, n - \textit{cnt})$.

Tiếp theo, ta duyệt qua từng chuỗi con độ dài $n$ và cập nhật $\textit{cnt}$. Với mỗi vị trí $i$, ta trừ đi sai khác của $s[i]$ với loại chuỗi nhị phân xen kẽ thứ nhất, rồi cộng sai khác của $s[i]$ với loại thứ hai. Ta cập nhật $\textit{ans} = \min(\textit{ans}, \textit{cnt}, n - \textit{cnt})$.

Cuối cùng, trả về $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minFlips(self, s: str) -> int:
        n = len(s)
        target = "01"
        cnt = sum(c != target[i & 1] for i, c in enumerate(s))
        ans = min(cnt, n - cnt)
        for i in range(n):
            cnt -= s[i] != target[i & 1]
            cnt += s[i] != target[(i + n) & 1]
            ans = min(ans, cnt, n - cnt)
        return ans
```

#### Java

```java
class Solution {
    public int minFlips(String s) {
        int n = s.length();
        String target = "01";
        int cnt = 0;
        for (int i = 0; i < n; ++i) {
            if (s.charAt(i) != target.charAt(i & 1)) {
                ++cnt;
            }
        }
        int ans = Math.min(cnt, n - cnt);
        for (int i = 0; i < n; ++i) {
            if (s.charAt(i) != target.charAt(i & 1)) {
                --cnt;
            }
            if (s.charAt(i) != target.charAt((i + n) & 1)) {
                ++cnt;
            }
            ans = Math.min(ans, Math.min(cnt, n - cnt));
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minFlips(string s) {
        int n = s.size();
        string target = "01";
        int cnt = 0;
        for (int i = 0; i < n; ++i) {
            if (s[i] != target[i & 1]) {
                ++cnt;
            }
        }
        int ans = min(cnt, n - cnt);
        for (int i = 0; i < n; ++i) {
            if (s[i] != target[i & 1]) {
                --cnt;
            }
            if (s[i] != target[(i + n) & 1]) {
                ++cnt;
            }
            ans = min({ans, cnt, n - cnt});
        }
        return ans;
    }
};
```

#### Go

```go
func minFlips(s string) int {
	n := len(s)
	target := "01"
	cnt := 0
	for i := range s {
		if s[i] != target[i&1] {
			cnt++
		}
	}
	ans := min(cnt, n-cnt)
	for i := range s {
		if s[i] != target[i&1] {
			cnt--
		}
		if s[i] != target[(i+n)&1] {
			cnt++
		}
		ans = min(ans, min(cnt, n-cnt))
	}
	return ans
}
```

#### TypeScript

```ts
function minFlips(s: string): number {
    const n = s.length;
    const target = '01';
    let cnt = 0;
    for (let i = 0; i < n; ++i) {
        if (s[i] !== target[i & 1]) {
            ++cnt;
        }
    }
    let ans = Math.min(cnt, n - cnt);
    for (let i = 0; i < n; ++i) {
        if (s[i] !== target[i & 1]) {
            --cnt;
        }
        if (s[i] !== target[(i + n) & 1]) {
            ++cnt;
        }
        ans = Math.min(ans, cnt, n - cnt);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_flips(s: String) -> i32 {
        let n: usize = s.len();
        let bytes = s.as_bytes();
        let target = b"01";
        let mut cnt: i32 = 0;

        for i in 0..n {
            if bytes[i] != target[i & 1] {
                cnt += 1;
            }
        }

        let mut ans = cnt.min(n as i32 - cnt);

        for i in 0..n {
            if bytes[i] != target[i & 1] {
                cnt -= 1;
            }
            if bytes[i] != target[(i + n) & 1] {
                cnt += 1;
            }
            ans = ans.min(cnt).min(n as i32 - cnt);
        }

        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @return {number}
 */
var minFlips = function (s) {
    const n = s.length;
    const target = '01';
    let cnt = 0;
    for (let i = 0; i < n; ++i) {
        if (s[i] !== target[i & 1]) {
            ++cnt;
        }
    }
    let ans = Math.min(cnt, n - cnt);
    for (let i = 0; i < n; ++i) {
        if (s[i] !== target[i & 1]) {
            --cnt;
        }
        if (s[i] !== target[(i + n) & 1]) {
            ++cnt;
        }
        ans = Math.min(ans, cnt, n - cnt);
    }
    return ans;
};
```

#### C#

```cs
public class Solution {
    public int MinFlips(string s) {
        int n = s.Length;
        string target = "01";
        int cnt = 0;

        for (int i = 0; i < n; ++i) {
            if (s[i] != target[i & 1]) {
                ++cnt;
            }
        }

        int ans = Math.Min(cnt, n - cnt);

        for (int i = 0; i < n; ++i) {
            if (s[i] != target[i & 1]) {
                --cnt;
            }
            if (s[i] != target[(i + n) & 1]) {
                ++cnt;
            }
            ans = Math.Min(ans, Math.Min(cnt, n - cnt));
        }

        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
