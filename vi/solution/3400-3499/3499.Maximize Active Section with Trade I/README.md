---
comments: true
difficulty: Medium
rating: 1729
source: Biweekly Contest 153 Q2
tags:
    - String
    - Enumeration
---

<!-- problem:start -->

# [3499. Maximize Active Section with Trade I](https://leetcode.com/problems/maximize-active-section-with-trade-i)

[中文文档](/solution/3400-3499/3499.Maximize%20Active%20Section%20with%20Trade%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi nhị phân <code>s</code> có độ dài <code>n</code>, trong đó:</p>

<ul>
	<li><code>&#39;1&#39;</code> biểu thị một đoạn <strong>đang hoạt động</strong>.</li>
	<li><code>&#39;0&#39;</code> biểu thị một đoạn <strong>không hoạt động</strong>.</li>
</ul>

<p>Bạn có thể thực hiện <strong>nhiều nhất một lần giao dịch</strong> để tối đa hóa số đoạn đang hoạt động trong <code>s</code>. Trong một lần giao dịch, bạn sẽ:</p>

<ul>
	<li>Chuyển một đoạn liên tiếp gồm các <code>&#39;1&#39;</code> được bao quanh bởi các <code>&#39;0&#39;</code> thành toàn các <code>&#39;0&#39;</code>.</li>
	<li>Sau đó, chuyển một đoạn liên tiếp gồm các <code>&#39;0&#39;</code> được bao quanh bởi các <code>&#39;1&#39;</code> thành toàn các <code>&#39;1&#39;</code>.</li>
</ul>

<p>Trả về <strong>số lượng</strong> đoạn đang hoạt động lớn nhất trong <code>s</code> sau khi thực hiện giao dịch tối ưu.</p>

<p><strong>Lưu ý:</strong> Hãy coi <code>s</code> như được <strong>mở rộng</strong> bằng một <code>&#39;1&#39;</code> ở cả hai đầu, tạo thành <code>t = &#39;1&#39; + s + &#39;1&#39;</code>. Các <code>&#39;1&#39;</code> được thêm vào <strong>không</strong> được tính vào kết quả cuối cùng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;01&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Vì không có đoạn nào gồm các <code>&#39;1&#39;</code> được bao quanh bởi các <code>&#39;0&#39;</code>, nên không thể thực hiện giao dịch hợp lệ. Số đoạn đang hoạt động lớn nhất là 1.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;0100&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chuỗi <code>&quot;0100&quot;</code> &rarr; Mở rộng thành <code>&quot;101001&quot;</code>.</li>
	<li>Chọn <code>&quot;0100&quot;</code>, chuyển <code>&quot;10<u><strong>1</strong></u>001&quot;</code> &rarr; <code>&quot;1<u><strong>0000</strong></u>1&quot;</code> &rarr; <code>&quot;1<u><strong>1111</strong></u>1&quot;</code>.</li>
	<li>Chuỗi cuối cùng sau khi bỏ phần mở rộng là <code>&quot;1111&quot;</code>. Số đoạn đang hoạt động lớn nhất là 4.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;1000100&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chuỗi <code>&quot;1000100&quot;</code> &rarr; Mở rộng thành <code>&quot;110001001&quot;</code>.</li>
	<li>Chọn <code>&quot;000100&quot;</code>, chuyển <code>&quot;11000<u><strong>1</strong></u>001&quot;</code> &rarr; <code>&quot;11<u><strong>000000</strong></u>1&quot;</code> &rarr; <code>&quot;11<u><strong>111111</strong></u>1&quot;</code>.</li>
	<li>Chuỗi cuối cùng sau khi bỏ phần mở rộng là <code>&quot;1111111&quot;</code>. Số đoạn đang hoạt động lớn nhất là 7.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;01010&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chuỗi <code>&quot;01010&quot;</code> &rarr; Mở rộng thành <code>&quot;1010101&quot;</code>.</li>
	<li>Chọn <code>&quot;010&quot;</code>, chuyển <code>&quot;10<u><strong>1</strong></u>0101&quot;</code> &rarr; <code>&quot;1<u><strong>000</strong></u>101&quot;</code> &rarr; <code>&quot;1<u><strong>111</strong></u>101&quot;</code>.</li>
	<li>Chuỗi cuối cùng sau khi bỏ phần mở rộng là <code>&quot;11110&quot;</code>. Số đoạn đang hoạt động lớn nhất là 4.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s[i]</code> là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Một lần giao dịch chuyển các đoạn $0$ ở hai phía của một đoạn $1$ thành $1$. Phần tăng thêm là tổng độ dài của hai đoạn $0$ đó, cộng với mọi $1$ ban đầu. Vì $n\le 10^5$, ta duyệt qua các đoạn.
>
> Một lần giao dịch không thể nối các đoạn $0$ không kề nhau, nên chỉ cần xét các cặp đoạn $0$ liền kề.
>
> Hai con trỏ chia chuỗi thành các đoạn: các đoạn $1$ được cộng vào đáp án cơ sở; các đoạn $0$ liền kề cập nhật $\textit{mx}$ bằng $\textit{pre}+\textit{cur}$. Đáp án là số lượng $1$ cộng với $\textit{mx}$.

<!-- thinking:end -->

Về bản chất, bài toán tương đương với việc tìm số ký tự `'1'` trong chuỗi $\textit{s}$, cộng với số ký tự `'0'` lớn nhất trong hai đoạn `'0'` liên tiếp kề nhau.

Do đó, ta có thể dùng hai con trỏ để duyệt qua chuỗi $\textit{s}$. Dùng biến $\textit{mx}$ để ghi nhận số ký tự `'0'` lớn nhất trong hai đoạn `'0'` liên tiếp kề nhau. Ta cũng cần biến $\textit{pre}$ để ghi nhận số ký tự `'0'` trong đoạn `'0'` liên tiếp trước đó.

Mỗi lần, ta đếm số ký tự giống nhau liên tiếp $\textit{cnt}$. Nếu ký tự hiện tại là `'1'`, cộng $\textit{cnt}$ vào đáp án. Nếu ký tự hiện tại là `'0'`, cập nhật $\textit{mx}$ bằng $\textit{mx} = \max(\textit{mx}, \textit{pre} + \textit{cnt})$, rồi cập nhật $\textit{pre}$ thành $\textit{cnt}$. Cuối cùng, cộng $\textit{mx}$ vào đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $\textit{s}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxActiveSectionsAfterTrade(self, s: str) -> int:
        n = len(s)
        ans = i = 0
        pre, mx = -inf, 0
        while i < n:
            j = i + 1
            while j < n and s[j] == s[i]:
                j += 1
            cur = j - i
            if s[i] == "1":
                ans += cur
            else:
                mx = max(mx, pre + cur)
                pre = cur
            i = j
        ans += mx
        return ans
```

#### Java

```java
class Solution {
    public int maxActiveSectionsAfterTrade(String s) {
        int n = s.length();
        int ans = 0, i = 0;
        int pre = Integer.MIN_VALUE, mx = 0;

        while (i < n) {
            int j = i + 1;
            while (j < n && s.charAt(j) == s.charAt(i)) {
                j++;
            }
            int cur = j - i;
            if (s.charAt(i) == '1') {
                ans += cur;
            } else {
                mx = Math.max(mx, pre + cur);
                pre = cur;
            }
            i = j;
        }

        ans += mx;
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxActiveSectionsAfterTrade(std::string s) {
        int n = s.length();
        int ans = 0, i = 0;
        int pre = INT_MIN, mx = 0;

        while (i < n) {
            int j = i + 1;
            while (j < n && s[j] == s[i]) {
                j++;
            }
            int cur = j - i;
            if (s[i] == '1') {
                ans += cur;
            } else {
                mx = std::max(mx, pre + cur);
                pre = cur;
            }
            i = j;
        }

        ans += mx;
        return ans;
    }
};
```

#### Go

```go
func maxActiveSectionsAfterTrade(s string) (ans int) {
	n := len(s)
	pre, mx := math.MinInt, 0

	for i := 0; i < n; {
		j := i + 1
		for j < n && s[j] == s[i] {
			j++
		}
		cur := j - i
		if s[i] == '1' {
			ans += cur
		} else {
			mx = max(mx, pre+cur)
			pre = cur
		}
		i = j
	}

	ans += mx
	return
}
```

#### TypeScript

```ts
function maxActiveSectionsAfterTrade(s: string): number {
    let n = s.length;
    let [ans, mx] = [0, 0];
    let pre = Number.MIN_SAFE_INTEGER;

    for (let i = 0; i < n;) {
        let j = i + 1;
        while (j < n && s[j] === s[i]) {
            j++;
        }
        let cur = j - i;
        if (s[i] === '1') {
            ans += cur;
        } else {
            mx = Math.max(mx, pre + cur);
            pre = cur;
        }
        i = j;
    }

    ans += mx;
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_active_sections_after_trade(s: String) -> i32 {
        let (ones_count, _, max_gain) = s.as_bytes().chunk_by(|left, right| left == right).fold(
            (0, i32::MIN, 0),
            |(ones_count, previous_zeros, max_gain), block| {
                let block_length = block.len() as i32;
                if block[0] == b'1' {
                    (ones_count + block_length, previous_zeros, max_gain)
                } else {
                    (
                        ones_count,
                        block_length,
                        max_gain.max(previous_zeros + block_length),
                    )
                }
            },
        );
        ones_count + max_gain
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
