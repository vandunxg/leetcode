---
comments: true
difficulty: Medium
rating: 1414
source: Weekly Contest 466 Q2
tags:
    - Greedy
    - String
---

<!-- problem:start -->

# [3675. Minimum Operations to Transform String](https://leetcode.com/problems/minimum-operations-to-transform-string)

[中文文档](/solution/3600-3699/3675.Minimum%20Operations%20to%20Transform%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</p>

<p>Bạn có thể thực hiện thao tác sau bất kỳ số lần nào (kể cả 0 lần):</p>

<ul>
	<li>
	<p>Chọn một ký tự <code>c</code> bất kỳ trong chuỗi và thay thế <strong>mọi</strong> lần xuất hiện của <code>c</code> bằng chữ cái viết thường <strong>tiếp theo</strong> trong bảng chữ cái tiếng Anh.</p>
	</li>
</ul>

<p>Trả về số thao tác <strong>nhỏ nhất</strong> cần thực hiện để biến <code>s</code> thành một chuỗi <strong>chỉ</strong> gồm các ký tự <code>&#39;a&#39;</code>.</p>

<p><strong>Lưu ý: </strong>Hãy coi bảng chữ cái là vòng tròn, do đó <code>&#39;a&#39;</code> đứng sau <code>&#39;z&#39;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;yz&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Đổi <code>&#39;y&#39;</code> thành <code>&#39;z&#39;</code> để được <code>&quot;zz&quot;</code>.</li>
	<li>Đổi <code>&#39;z&#39;</code> thành <code>&#39;a&#39;</code> để được <code>&quot;aa&quot;</code>.</li>
	<li>Vậy đáp án là 2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;a&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chuỗi <code>&quot;a&quot;</code> chỉ gồm các ký tự <code>&#39;a&#39;</code>​​​​​​​ . Vậy đáp án là 0.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 5 * 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lần

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác đẩy mọi lần xuất hiện của một chữ cái được chọn tiến lên một bước. Vì chuỗi đích chỉ gồm các ký tự $a$, mỗi ký tự khác $a$ phải tiến về phía trước cho đến khi trở thành $a$.
>
> Các thao tác trên cùng một chữ cái được thực hiện song song, nên tổng số thao tác là khoảng cách xa nhất đến $a$, tức là giá trị lớn nhất của $26-(c-\texttt{a})$.
>
> Chuỗi chỉ gồm các ký tự $a$ cần $0$ thao tác. Chỉ cần duyệt một lần để ghi nhận khoảng cách lớn nhất đó.

<!-- thinking:end -->

Theo mô tả bài toán, ta luôn bắt đầu từ ký tự 'b' và lần lượt đổi từng ký tự thành ký tự tiếp theo cho đến khi nó trở thành 'a'. Vì vậy, ta chỉ cần tìm ký tự trong chuỗi có khoảng cách xa 'a' nhất và tính khoảng cách của nó đến 'a' để nhận được đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, s: str) -> int:
        return max((26 - (ord(c) - 97) for c in s if c != "a"), default=0)
```

#### Java

```java
class Solution {
    public int minOperations(String s) {
        int ans = 0;
        for (char c : s.toCharArray()) {
            if (c != 'a') {
                ans = Math.max(ans, 26 - (c - 'a'));
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
    int minOperations(string s) {
        int ans = 0;
        for (char c : s) {
            if (c != 'a') {
                ans = max(ans, 26 - (c - 'a'));
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minOperations(s string) (ans int) {
	for _, c := range s {
		if c != 'a' {
			ans = max(ans, 26-int(c-'a'))
		}
	}
	return
}
```

#### TypeScript

```ts
function minOperations(s: string): number {
    let ans = 0;
    for (const c of s) {
        if (c !== 'a') {
            ans = Math.max(ans, 26 - (c.charCodeAt(0) - 97));
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
