---
comments: true
difficulty: Medium
rating: 1593
source: Weekly Contest 407 Q3
tags:
    - Greedy
    - String
    - Counting
---

<!-- problem:start -->

# [3228. Maximum Number of Operations to Move Ones to the End](https://leetcode.com/problems/maximum-number-of-operations-to-move-ones-to-the-end)

[中文文档](/solution/3200-3299/3228.Maximum%20Number%20of%20Operations%20to%20Move%20Ones%20to%20the%20End/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một <span data-keyword="binary-string">chuỗi nhị phân</span> <code>s</code>.</p>

<p>Bạn có thể thực hiện thao tác sau trên chuỗi <strong>bất kỳ</strong> số lần nào:</p>

<ul>
    <li>Chọn <strong>bất kỳ</strong> chỉ số <code>i</code> nào trong chuỗi sao cho <code>i + 1 &lt; s.length</code>, <code>s[i] == &#39;1&#39;</code> và <code>s[i + 1] == &#39;0&#39;</code>.</li>
    <li>Di chuyển ký tự <code>s[i]</code> sang <strong>phải</strong> cho đến khi nó đến cuối chuỗi hoặc gặp một ký tự <code>&#39;1&#39;</code> khác. Ví dụ, với <code>s = &quot;010010&quot;</code>, nếu chọn <code>i = 1</code>, chuỗi kết quả sẽ là <code>s = &quot;0<strong><u>001</u></strong>10&quot;</code>.</li>
</ul>

<p>Trả về số thao tác <strong>lớn nhất</strong> có thể thực hiện.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;1001101&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể thực hiện các thao tác sau:</p>

<ul>
    <li>Chọn chỉ số <code>i = 0</code>. Chuỗi kết quả là <code>s = &quot;<u><strong>001</strong></u>1101&quot;</code>.</li>
    <li>Chọn chỉ số <code>i = 4</code>. Chuỗi kết quả là <code>s = &quot;0011<u><strong>01</strong></u>1&quot;</code>.</li>
    <li>Chọn chỉ số <code>i = 3</code>. Chuỗi kết quả là <code>s = &quot;001<strong><u>01</u></strong>11&quot;</code>.</li>
    <li>Chọn chỉ số <code>i = 2</code>. Chuỗi kết quả là <code>s = &quot;00<strong><u>01</u></strong>111&quot;</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;00111&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
    <li><code>s[i]</code> là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác biến $10$ thành $01$, dịch một $1$ sang phải một bước. Với $n\le 10^5$, mô phỏng từng lần dịch có thể dẫn đến độ phức tạp bậc hai. Một thao tác là việc một $1$ "đi qua" số $0$ tiếp theo, và mỗi $1$ đã gặp có thể đi qua từng dãy $0$ ở phía sau.
>
> Đếm các số 1 vào $\textit{cnt}$; tại biên $1\to 0$, mỗi trong số $\textit{cnt}$ đó có thể di chuyển thêm một lần, nên cộng $\textit{cnt}$. Chỉ cần duyệt tuyến tính một lần.

<!-- thinking:end -->

Ta dùng một biến $\textit{ans}$ để lưu đáp án và một biến $\textit{cnt}$ để đếm số $1$ hiện tại.

Sau đó, ta duyệt qua chuỗi $s$. Nếu ký tự hiện tại là $1$, ta tăng $\textit{cnt}$. Ngược lại, nếu có ký tự trước đó và ký tự trước đó là $1$, thì $\textit{cnt}$ số $1$ trước đó có thể được di chuyển tiếp, nên ta cộng $\textit{cnt}$ vào đáp án.

Cuối cùng, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxOperations(self, s: str) -> int:
        ans = cnt = 0
        for i, c in enumerate(s):
            if c == "1":
                cnt += 1
            elif i and s[i - 1] == "1":
                ans += cnt
        return ans
```

#### Java

```java
class Solution {
    public int maxOperations(String s) {
        int ans = 0, cnt = 0;
        int n = s.length();
        for (int i = 0; i < n; ++i) {
            if (s.charAt(i) == '1') {
                ++cnt;
            } else if (i > 0 && s.charAt(i - 1) == '1') {
                ans += cnt;
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
    int maxOperations(string s) {
        int ans = 0, cnt = 0;
        int n = s.size();
        for (int i = 0; i < n; ++i) {
            if (s[i] == '1') {
                ++cnt;
            } else if (i && s[i - 1] == '1') {
                ans += cnt;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxOperations(s string) (ans int) {
    cnt := 0
    for i, c := range s {
        if c == '1' {
            cnt++
        } else if i > 0 && s[i-1] == '1' {
            ans += cnt
        }
    }
    return
}
```

#### TypeScript

```ts
function maxOperations(s: string): number {
    let [ans, cnt] = [0, 0];
    const n = s.length;
    for (let i = 0; i < n; ++i) {
        if (s[i] === '1') {
            ++cnt;
        } else if (i && s[i - 1] === '1') {
            ans += cnt;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_operations(s: String) -> i32 {
        let mut ans = 0;
        let mut cnt = 0;
        let n = s.len();
        let bytes = s.as_bytes();
        for i in 0..n {
            if bytes[i] == b'1' {
                cnt += 1;
            } else if i > 0 && bytes[i - 1] == b'1' {
                ans += cnt;
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
