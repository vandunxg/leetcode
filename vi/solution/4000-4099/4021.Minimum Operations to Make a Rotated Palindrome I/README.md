---
comments: true
difficulty: Medium
rating: 1517
source: Biweekly Contest 189 Q2
tags:
    - Math
    - String
    - Enumeration
---

<!-- problem:start -->

# [4021. Minimum Operations to Make a Rotated Palindrome I](https://leetcode.com/problems/minimum-operations-to-make-a-rotated-palindrome-i)

[中文文档](/solution/4000-4099/4021.Minimum%20Operations%20to%20Make%20a%20Rotated%20Palindrome%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</p>

<p>Bạn có thể thực hiện các thao tác sau bất kỳ số lần nào (kể cả không lần nào) và theo bất kỳ thứ tự nào:</p>

<ul>
	<li><strong>Tăng</strong>: Chọn một chỉ số bất kỳ <code>i</code> và thay <code>s[i]</code> bằng chữ cái tiếng Anh viết thường tiếp theo. Chữ cái sau <code>&#39;z&#39;</code> là <code>&#39;a&#39;</code>.</li>
	<li><strong>Xoay trái</strong>: Chuyển ký tự đầu tiên của chuỗi xuống cuối.</li>
</ul>

<p>Trả về số thao tác <strong>nhỏ nhất</strong> cần thực hiện để biến <code>s</code> thành một <span data-keyword="palindrome-string">palindrome</span>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abc&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>
Một cách tối ưu là:

<ul>
	<li>Xoay trái chuỗi: <code>&quot;abc&quot; -&gt; &quot;bca&quot;</code>.</li>
	<li>Tăng <code>&#39;a&#39;</code> thành <code>&#39;b&#39;</code>: <code>&quot;bca&quot; -&gt; &quot;bcb&quot;</code>.</li>
	<li><code>&quot;bcb&quot;</code> là một palindrome. Vì vậy, đáp án là 2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;yb&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tăng ký tự đầu tiên ba lần: <code>&quot;yb&quot; -&gt; &quot;zb&quot; -&gt; &quot;ab&quot; -&gt; &quot;bb&quot;</code>.</li>
	<li><code>&quot;bb&quot;</code> là một palindrome. Vì vậy, đáp án là 3.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= s.length &lt;= 2000</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ có $n\le 2000$ phép xoay trái, và việc ghép cặp các ký tự sau mỗi phép xoay tốn $O(n^2)$, phù hợp với giới hạn đề bài.
>
> Các chữ cái chỉ có thể tăng theo vòng tròn bảng chữ cái, nên cách rẻ nhất để đưa một cặp về cùng chữ cái là đi theo cung ngắn hơn $\min(d,26-d)$; chữ cái đích tối ưu luôn là một trong hai chữ cái ban đầu.
>
> Cộng chi phí xoay $k$ vào chi phí tăng của từng cặp rồi lấy giá trị nhỏ nhất sẽ cho đáp án.

<!-- thinking:end -->

Ta liệt kê số lần xoay trái $k$ ($0 \leq k < n$), với chi phí là $k$ thao tác. Sau $k$ lần xoay trái, chỉ số $i$ trong chuỗi mới tương ứng với chỉ số $(i + k) \bmod n$ trong chuỗi ban đầu.

Với mỗi cặp vị trí đối xứng, ta cần dùng các thao tác tăng để đưa hai ký tự về giống nhau. Vì ta chỉ có thể tăng theo chiều tiến (`'z'` quay vòng về `'a'`), số lần tăng nhỏ nhất để hai chữ cái giống nhau là độ dài cung ngắn hơn trên vòng chữ cái, tức là $\min(d, 26 - d)$, trong đó $d$ là hiệu tuyệt đối giữa chỉ số của hai chữ cái. Chữ cái đích tối ưu luôn là một trong hai chữ cái ban đầu.

Ta lấy giá trị nhỏ nhất trên mọi $k$.

Độ phức tạp thời gian là $O(n^2)$, còn độ phức tạp không gian là $O(1)$, trong đó $n$ là độ dài chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, s: str) -> int:
        n = len(s)
        ans = inf
        for k in range(n):
            t = k
            i, j = 0, n - 1
            while i < j:
                x = ord(s[(i + k) % n]) - ord('a')
                y = ord(s[(j + k) % n]) - ord('a')
                d = abs(x - y)
                t += min(d, 26 - d)
                i, j = i + 1, j - 1
            ans = min(ans, t)
        return ans
```

#### Java

```java
class Solution {
    public int minOperations(String s) {
        int n = s.length();
        int ans = Integer.MAX_VALUE;

        for (int k = 0; k < n; k++) {
            int t = k;
            int i = 0, j = n - 1;

            while (i < j) {
                int x = s.charAt((i + k) % n) - 'a';
                int y = s.charAt((j + k) % n) - 'a';

                int d = Math.abs(x - y);
                t += Math.min(d, 26 - d);

                i++;
                j--;
            }

            ans = Math.min(ans, t);
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
        int n = s.size();
        int ans = INT_MAX;

        for (int k = 0; k < n; ++k) {
            int t = k;
            int i = 0, j = n - 1;

            while (i < j) {
                int x = s[(i + k) % n] - 'a';
                int y = s[(j + k) % n] - 'a';

                int d = abs(x - y);
                t += min(d, 26 - d);

                ++i;
                --j;
            }

            ans = min(ans, t);
        }

        return ans;
    }
};
```

#### Go

```go
func minOperations(s string) int {
	n := len(s)
	ans := int(^uint(0) >> 1)

	for k := 0; k < n; k++ {
		t := k
		i, j := 0, n-1

		for i < j {
			x := int(s[(i+k)%n] - 'a')
			y := int(s[(j+k)%n] - 'a')

			d := abs(x - y)
			t += min(d, 26-d)

			i++
			j--
		}

		ans = min(ans, t)
	}

	return ans
}

func abs(x int) int {
	return max(x, -x)
}
```

#### TypeScript

```ts
function minOperations(s: string): number {
    const n = s.length;
    let ans = Infinity;

    for (let k = 0; k < n; k++) {
        let t = k;
        let i = 0;
        let j = n - 1;

        while (i < j) {
            const x = s.charCodeAt((i + k) % n) - 97;
            const y = s.charCodeAt((j + k) % n) - 97;

            const d = Math.abs(x - y);
            t += Math.min(d, 26 - d);

            i++;
            j--;
        }

        ans = Math.min(ans, t);
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
