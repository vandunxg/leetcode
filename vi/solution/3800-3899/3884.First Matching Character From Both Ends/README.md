---
comments: true
difficulty: Easy
rating: 1161
source: Weekly Contest 495 Q1
tags:
    - Two Pointers
    - String
---

<!-- problem:start -->

# [3884. First Matching Character From Both Ends](https://leetcode.com/problems/first-matching-character-from-both-ends)

[中文文档](/solution/3800-3899/3884.First%20Matching%20Character%20From%20Both%20Ends/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> có độ dài <code>n</code>, gồm các chữ cái tiếng Anh viết thường.</p>

<p>Hãy trả về chỉ số nhỏ nhất <code>i</code> sao cho <code>s[i] == s[n - i - 1]</code>.</p>

<p>Nếu không tồn tại chỉ số nào như vậy, hãy trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abcacbd&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tại chỉ số <code>i = 1</code>, <code>s[1]</code> và <code>s[5]</code> đều là <code>&#39;b&#39;</code>.</p>

<p>Không có chỉ số nhỏ hơn nào thỏa mãn điều kiện, nên đáp án là 1.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abc&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>​​​​​​​Tại chỉ số <code>i = 1</code>, hai vị trí được so sánh trùng nhau, nên cả hai ký tự đều là <code>&#39;b&#39;</code>.</p>

<p>Không có chỉ số nhỏ hơn nào thỏa mãn điều kiện, nên đáp án là 1.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abcdab&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>​​​​​​​Với mọi chỉ số <code>i</code>, các ký tự ở vị trí <code>i</code> và <code>n - i - 1</code> đều khác nhau.</p>

<p>Do đó, không tồn tại chỉ số hợp lệ nào, nên đáp án là -1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Tìm $i$ nhỏ nhất sao cho $s[i]=s[n-i-1]$. Vì độ dài chuỗi $\le 100$, ta chỉ cần duyệt nửa đầu.
>
> Các ký tự khác không ảnh hưởng đến phép kiểm tra đối xứng này.
>
> Kiểm tra $i=0,1,\ldots,\lfloor n/2 \rfloor$ và trả về ngay khi tìm thấy kết quả phù hợp đầu tiên.
>
> Nếu không tìm thấy, trả về $-1$.

<!-- thinking:end -->

Ta duyệt nửa đầu của chuỗi $s$. Với mỗi chỉ số $i$, ta kiểm tra xem các ký tự ở vị trí $i$ và vị trí $n - i - 1$ có bằng nhau hay không. Nếu bằng nhau, ta trả về chỉ số $i$. Nếu duyệt hết mà không tìm thấy chỉ số nào như vậy, ta trả về -1.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def firstMatchingIndex(self, s: str) -> int:
        for i in range(len(s) // 2 + 1):
            if s[i] == s[-i - 1]:
                return i
        return -1
```

#### Java

```java
class Solution {
    public int firstMatchingIndex(String s) {
        int n = s.length();
        for (int i = 0; i < n / 2 + 1; ++i) {
            if (s.charAt(i) == s.charAt(n - i - 1)) {
                return i;
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int firstMatchingIndex(string s) {
        int n = s.size();
        for (int i = 0; i < n / 2 + 1; ++i) {
            if (s[i] == s[n - i - 1]) {
                return i;
            }
        }
        return -1;
    }
};
```

#### Go

```go
func firstMatchingIndex(s string) int {
	n := len(s)
	for i := 0; i < n/2+1; i++ {
		if s[i] == s[n-i-1] {
			return i
		}
	}
	return -1
}
```

#### TypeScript

```ts
function firstMatchingIndex(s: string): number {
    const n = s.length;
    for (let i = 0; i < Math.floor(n / 2) + 1; i++) {
        if (s[i] === s[n - i - 1]) {
            return i;
        }
    }
    return -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
