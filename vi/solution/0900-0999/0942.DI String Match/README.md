---
comments: true
difficulty: Easy
tags:
    - Greedy
    - Array
    - Two Pointers
    - String
---

<!-- problem:start -->

# [942. DI String Match](https://leetcode.com/problems/di-string-match)

[中文文档](/solution/0900-0999/0942.DI%20String%20Match/README.md)

## Mô tả

<!-- description:start -->

<p>Một hoán vị <code>perm</code> gồm tất cả <code>n + 1</code> số nguyên trong đoạn <code>[0, n]</code> có thể được biểu diễn bằng chuỗi <code>s</code> độ dài <code>n</code>, trong đó:</p>

<ul>
	<li><code>s[i] == &#39;I&#39;</code> nếu <code>perm[i] &lt; perm[i + 1]</code>, và</li>
	<li><code>s[i] == &#39;D&#39;</code> nếu <code>perm[i] &gt; perm[i + 1]</code>.</li>
</ul>

<p>Cho chuỗi <code>s</code>, hãy khôi phục và trả về hoán vị <code>perm</code>. Nếu có nhiều hoán vị hợp lệ, hãy trả về <strong>bất kỳ hoán vị nào trong số đó</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> s = "IDID"
<strong>Đầu ra:</strong> [0,4,1,3,2]
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> s = "III"
<strong>Đầu ra:</strong> [0,1,2,3]
</pre><p><strong class="example">Ví dụ 3:</strong></p>
<pre><strong>Đầu vào:</strong> s = "DDI"
<strong>Đầu ra:</strong> [3,2,0,1]
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s[i]</code> chỉ có thể là <code>&#39;I&#39;</code> hoặc <code>&#39;D&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thuật toán greedy

<!-- thinking:start -->

> **Tư duy**
>
> Tạo hoán vị của $0..n$ sao cho các bước tăng giảm giữa hai phần tử liền kề khớp với `'I'`/`'D'`. Vì $n\le 10^5$, không thể dùng backtracking. Với `'I'`, chọn giá trị nhỏ nhất hiện tại; với `'D'`, chọn giá trị lớn nhất hiện tại, rồi thêm giá trị còn lại cuối cùng. Lựa chọn greedy này đảm bảo điều kiện giữa mọi cặp phần tử liền kề.

<!-- thinking:end -->

Ta dùng hai con trỏ `low` và `high` lần lượt biểu diễn giá trị nhỏ nhất và lớn nhất hiện tại. Sau đó, duyệt chuỗi `s`. Nếu ký tự hiện tại là `I`, thêm `low` vào mảng kết quả rồi tăng `low` lên $1$; nếu ký tự là `D`, thêm `high` vào mảng kết quả rồi giảm `high` đi $1$.

Cuối cùng, thêm `low` vào mảng kết quả rồi trả về mảng đó.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi `s`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def diStringMatch(self, s: str) -> List[int]:
        low, high = 0, len(s)
        ans = []
        for c in s:
            if c == "I":
                ans.append(low)
                low += 1
            else:
                ans.append(high)
                high -= 1
        ans.append(low)
        return ans
```

#### Java

```java
class Solution {
    public int[] diStringMatch(String s) {
        int n = s.length();
        int low = 0, high = n;
        int[] ans = new int[n + 1];
        for (int i = 0; i < n; i++) {
            if (s.charAt(i) == 'I') {
                ans[i] = low++;
            } else {
                ans[i] = high--;
            }
        }
        ans[n] = low;
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> diStringMatch(string s) {
        int n = s.size();
        int low = 0, high = n;
        vector<int> ans(n + 1);
        for (int i = 0; i < n; ++i) {
            if (s[i] == 'I') {
                ans[i] = low++;
            } else {
                ans[i] = high--;
            }
        }
        ans[n] = low;
        return ans;
    }
};
```

#### Go

```go
func diStringMatch(s string) (ans []int) {
	low, high := 0, len(s)
	for _, c := range s {
		if c == 'I' {
			ans = append(ans, low)
			low++
		} else {
			ans = append(ans, high)
			high--
		}
	}
	ans = append(ans, low)
	return
}
```

#### TypeScript

```ts
function diStringMatch(s: string): number[] {
    const ans: number[] = [];
    let [low, high] = [0, s.length];
    for (const c of s) {
        if (c === 'I') {
            ans.push(low++);
        } else {
            ans.push(high--);
        }
    }
    ans.push(low);
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn di_string_match(s: String) -> Vec<i32> {
        let mut low = 0;
        let mut high = s.len() as i32;
        let mut ans = Vec::with_capacity(s.len() + 1);

        for c in s.chars() {
            if c == 'I' {
                ans.push(low);
                low += 1;
            } else {
                ans.push(high);
                high -= 1;
            }
        }

        ans.push(low);
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
