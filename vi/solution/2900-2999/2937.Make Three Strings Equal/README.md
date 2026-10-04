---
comments: true
difficulty: Easy
rating: 1347
source: Weekly Contest 372 Q1
tags:
    - String
---

<!-- problem:start -->

# [2937. Make Three Strings Equal](https://leetcode.com/problems/make-three-strings-equal)

[中文文档](/solution/2900-2999/2937.Make%20Three%20Strings%20Equal/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ba chuỗi: <code>s1</code>, <code>s2</code> và <code>s3</code>. Trong một thao tác, bạn có thể chọn một trong ba chuỗi này và xóa ký tự <strong>ngoài cùng bên phải</strong> của nó. Lưu ý rằng bạn <strong>không thể</strong> xóa toàn bộ một chuỗi.</p>

<p>Trả về <em>số thao tác nhỏ nhất</em> cần thực hiện để biến các chuỗi thành bằng nhau<em>. </em>Nếu không thể biến chúng thành bằng nhau, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">s1 = &quot;abc&quot;, s2 = &quot;abb&quot;, s3 = &quot;ab&quot;</span></p>

<p><strong>Đầu ra: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">2</span></p>

<p><strong>Giải thích:&nbsp;</strong>Xóa ký tự ngoài cùng bên phải của cả <code>s1</code> và <code>s2</code> sẽ tạo ra ba chuỗi bằng nhau.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">s1 = &quot;dac&quot;, s2 = &quot;bac&quot;, s3 = &quot;cac&quot;</span></p>

<p><strong>Đầu ra: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">-1</span></p>

<p><strong>Giải thích:</strong> Vì ký tự đầu tiên của <code>s1</code> và <code>s2</code> khác nhau nên không thể biến chúng thành bằng nhau.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s1.length, s2.length, s3.length &lt;= 100</code></li>
	<li><font face="monospace"><code>s1</code>,</font> <code><font face="monospace">s2</font></code><font face="monospace"> và</font> <code><font face="monospace">s3</font></code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Vì chỉ có thể xóa ký tự cuối của một chuỗi, ba chuỗi trở thành bằng nhau khi và chỉ khi chúng có chung một tiền tố khác rỗng, và đó là chuỗi kết quả cuối cùng. Duyệt chỉ số $i$ cho đến khi ba ký tự khác nhau; khi đó độ dài tiền tố là $i$.
>
> Nếu $i=0$ thì không tồn tại chuỗi bằng nhau khác rỗng. Ngược lại, số lần xóa bằng tổng độ dài trừ đi $3i$. Nếu ba chuỗi không khác nhau, sử dụng độ dài nhỏ nhất. Với $n \le 100$, chỉ cần duyệt một lần theo các vị trí tương ứng.

<!-- thinking:end -->

Theo mô tả bài toán, ta biết rằng nếu ba chuỗi bằng nhau sau khi xóa các ký tự thì chúng có chung một tiền tố có độ dài lớn hơn $1$. Vì vậy, ta có thể duyệt vị trí $i$ của tiền tố chung. Nếu ba ký tự tại chỉ số hiện tại $i$ không hoàn toàn giống nhau, thì độ dài của tiền tố chung là $i$. Khi đó, ta kiểm tra xem $i$ có bằng $0$ hay không. Nếu có, trả về $-1$. Ngược lại, trả về $s - 3 \times i$, trong đó $s$ là tổng độ dài của ba chuỗi.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài nhỏ nhất của ba chuỗi. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findMinimumOperations(self, s1: str, s2: str, s3: str) -> int:
        s = len(s1) + len(s2) + len(s3)
        n = min(len(s1), len(s2), len(s3))
        for i in range(n):
            if not s1[i] == s2[i] == s3[i]:
                return -1 if i == 0 else s - 3 * i
        return s - 3 * n
```

#### Java

```java
class Solution {
    public int findMinimumOperations(String s1, String s2, String s3) {
        int s = s1.length() + s2.length() + s3.length();
        int n = Math.min(Math.min(s1.length(), s2.length()), s3.length());
        for (int i = 0; i < n; ++i) {
            if (!(s1.charAt(i) == s2.charAt(i) && s2.charAt(i) == s3.charAt(i))) {
                return i == 0 ? -1 : s - 3 * i;
            }
        }
        return s - 3 * n;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findMinimumOperations(string s1, string s2, string s3) {
        int s = s1.size() + s2.size() + s3.size();
        int n = min({s1.size(), s2.size(), s3.size()});
        for (int i = 0; i < n; ++i) {
            if (!(s1[i] == s2[i] && s2[i] == s3[i])) {
                return i == 0 ? -1 : s - 3 * i;
            }
        }
        return s - 3 * n;
    }
};
```

#### Go

```go
func findMinimumOperations(s1 string, s2 string, s3 string) int {
	s := len(s1) + len(s2) + len(s3)
	n := min(len(s1), len(s2), len(s3))
	for i := range s1[:n] {
		if !(s1[i] == s2[i] && s2[i] == s3[i]) {
			if i == 0 {
				return -1
			}
			return s - 3*i
		}
	}
	return s - 3*n
}
```

#### TypeScript

```ts
function findMinimumOperations(s1: string, s2: string, s3: string): number {
    const s = s1.length + s2.length + s3.length;
    const n = Math.min(s1.length, s2.length, s3.length);
    for (let i = 0; i < n; ++i) {
        if (!(s1[i] === s2[i] && s2[i] === s3[i])) {
            return i === 0 ? -1 : s - 3 * i;
        }
    }
    return s - 3 * n;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
