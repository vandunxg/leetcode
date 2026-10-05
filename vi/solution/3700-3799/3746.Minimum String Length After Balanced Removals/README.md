---
comments: true
difficulty: Medium
rating: 1326
source: Weekly Contest 476 Q2
tags:
    - Stack
    - String
    - Counting
---

<!-- problem:start -->

# [3746. Minimum String Length After Balanced Removals](https://leetcode.com/problems/minimum-string-length-after-balanced-removals)

[中文文档](/solution/3700-3799/3746.Minimum%20String%20Length%20After%20Balanced%20Removals/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> chỉ gồm các ký tự <code>&#39;a&#39;</code> và <code>&#39;b&#39;</code>.</p>

<p>Bạn được phép liên tục xóa <strong>bất kỳ <span data-keyword="substring-nonempty">chuỗi con nào</span></strong> có số lượng ký tự <code>&#39;a&#39;</code> bằng số lượng ký tự <code>&#39;b&#39;</code>. Sau mỗi lần xóa, các phần còn lại của chuỗi được nối lại với nhau mà không có khoảng trống.</p>

<p>Trả về một số nguyên biểu thị <strong>độ dài nhỏ nhất có thể</strong> của chuỗi sau khi thực hiện bất kỳ số lần thao tác nào như vậy.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = <code>&quot;aabbab&quot;</code></span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi con <code>&quot;aabbab&quot;</code> có ba ký tự <code>&#39;a&#39;</code> và ba ký tự <code>&#39;b&#39;</code>. Vì số lượng của chúng bằng nhau, ta có thể xóa trực tiếp toàn bộ chuỗi. Độ dài nhỏ nhất là 0.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = <code>&quot;aaaa&quot;</code></span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mọi chuỗi con của <code>&quot;aaaa&quot;</code> đều chỉ chứa các ký tự <code>&#39;a&#39;</code>. Không có chuỗi con nào có thể bị xóa, nên độ dài nhỏ nhất vẫn là 4.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = <code>&quot;aaabb&quot;</code></span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Trước tiên, xóa chuỗi con <code>&quot;ab&quot;</code>, còn lại <code>&quot;aab&quot;</code>. Tiếp theo, xóa chuỗi con <code>&quot;ab&quot;</code> mới tạo, còn lại <code>&quot;a&quot;</code>. Không thể thực hiện thêm lần xóa nào, nên độ dài nhỏ nhất là 1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s[i]</code> là <code>&#39;a&#39;</code> hoặc <code>&#39;b&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Mọi chuỗi con có số lượng $a$ và $b$ bằng nhau đều có thể bị xóa, và phần còn lại không thể xóa thêm khi các ký tự còn lại đều giống nhau. Vì vậy, đáp án là giá trị tuyệt đối của hiệu giữa hai số lượng; không cần mô phỏng các lần xóa.

<!-- thinking:end -->

Theo mô tả đề bài, miễn là hai ký tự kề nhau khác nhau, ta có thể xóa chúng. Do đó, chuỗi còn lại cuối cùng chỉ chứa một loại ký tự, hoặc toàn bộ là 'a' hoặc toàn bộ là 'b'. Vì vậy, ta chỉ cần đếm số lượng 'a' và 'b' trong chuỗi, rồi độ dài nhỏ nhất cuối cùng là giá trị tuyệt đối của hiệu giữa hai số lượng.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minLengthAfterRemovals(self, s: str) -> int:
        a = s.count("a")
        b = len(s) - a
        return abs(a - b)
```

#### Java

```java
class Solution {
    public int minLengthAfterRemovals(String s) {
        int n = s.length();
        int a = 0;
        for (int i = 0; i < n; ++i) {
            if (s.charAt(i) == 'a') {
                ++a;
            }
        }
        int b = n - a;
        return Math.abs(a - b);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minLengthAfterRemovals(string s) {
        int a = 0;
        for (char c : s) {
            if (c == 'a') {
                ++a;
            }
        }
        int b = s.size() - a;
        return abs(a - b);
    }
};
```

#### Go

```go
func minLengthAfterRemovals(s string) int {
	a := strings.Count(s, "a")
	b := len(s) - a
	return abs(a - b)
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function minLengthAfterRemovals(s: string): number {
    let a = 0;
    for (const c of s) {
        if (c === 'a') {
            ++a;
        }
    }
    const b = s.length - a;
    return Math.abs(a - b);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
