---
comments: true
difficulty: Easy
rating: 1242
source: Biweekly Contest 188 Q1
tags:
    - String
    - Counting
---

<!-- problem:start -->

# [4006. Count Valid Prefixes](https://leetcode.com/problems/count-valid-prefixes)

[中文文档](/solution/4000-4099/4006.Count%20Valid%20Prefixes/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một <span data-keyword="binary-string">chuỗi nhị phân</span> <code>s</code>.</p>

<p>Một <span data-keyword="string-prefix">tiền tố</span> của <code>s</code> được xem là <strong>hợp lệ</strong> nếu các ký tự của nó có thể được sắp xếp lại để tạo thành một chuỗi <strong>xen kẽ</strong>.</p>

<p>Trả về số tiền tố hợp lệ của <code>s</code>.</p>

<p>Một chuỗi được xem là <strong>xen kẽ</strong> nếu không có hai ký tự liền kề nào bằng nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;00101&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các tiền tố hợp lệ là:</p>

<ul>
	<li><code>&quot;0&quot;</code>: Đây đã là một chuỗi xen kẽ.</li>
	<li><code>&quot;001&quot;</code>: Có thể sắp xếp lại thành <code>&quot;010&quot;</code>, là một chuỗi xen kẽ.</li>
	<li><code>&quot;00101&quot;</code>: Có thể sắp xếp lại thành <code>&quot;01010&quot;</code>, là một chuỗi xen kẽ.</li>
</ul>

<p>Vậy đáp án là 3.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;101&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mọi tiền tố của <code>s = &quot;101&quot;</code> đều đã là chuỗi xen kẽ. Vậy đáp án là 3.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ gồm <code>&#39;0&#39;</code> và <code>&#39;1&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Một tiền tố hợp lệ khi và chỉ khi độ chênh lệch tuyệt đối giữa số lượng `'0'` và `'1'` không vượt quá $1$. Nếu đếm lại cả hai ký tự trên mọi tiền tố, độ phức tạp sẽ là bậc hai.
>
> Một biến duy nhất $t$ theo dõi độ chênh lệch khi duyệt từ trái sang phải: gặp `'1'` thì tăng, gặp `'0'` thì giảm. Tại mỗi chỉ số, ta kiểm tra $|t|\le 1$.
>
> Vì vậy, toàn bộ quá trình đếm chỉ cần một lượt duyệt tuyến tính; ta không cần lưu một mảng đếm cho từng tiền tố.

<!-- thinking:end -->

Một chuỗi có thể được sắp xếp lại thành chuỗi xen kẽ khi và chỉ khi số lượng `'0'` và `'1'` trong chuỗi chênh lệch nhau không quá $1$.

Do đó, ta duyệt chuỗi $s$ và duy trì biến $t$ bằng số lượng `'1'` trừ số lượng `'0'` trong tiền tố hiện tại (gặp `'1'` thì tăng một, gặp `'0'` thì giảm một). Nếu $|t| \leq 1$, tiền tố hiện tại hợp lệ và ta tăng đáp án lên một.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countValidPrefixes(self, s: str) -> int:
        ans = t = 0
        for c in s:
            t += 1 if c == '1' else -1
            ans += 1 if abs(t) <= 1 else 0
        return ans
```

#### Java

```java
class Solution {
    public int countValidPrefixes(String s) {
        int ans = 0, t = 0;
        for (char c : s.toCharArray()) {
            t += c == '1' ? 1 : -1;
            if (Math.abs(t) <= 1) {
                ans++;
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
    int countValidPrefixes(string s) {
        int ans = 0, t = 0;
        for (char c : s) {
            t += c == '1' ? 1 : -1;
            if (abs(t) <= 1) {
                ans++;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countValidPrefixes(s string) int {
	ans, t := 0, 0
	for _, c := range s {
		if c == '1' {
			t++
		} else {
			t--
		}
		if t >= -1 && t <= 1 {
			ans++
		}
	}
	return ans
}
```

#### TypeScript

```ts
function countValidPrefixes(s: string): number {
    let ans = 0;
    let t = 0;
    for (const c of s) {
        t += c === '1' ? 1 : -1;
        if (Math.abs(t) <= 1) {
            ans++;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
