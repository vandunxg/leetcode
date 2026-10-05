---
comments: true
difficulty: Medium
tags:
    - Array
    - String
---

<!-- problem:start -->

# [3706. Maximum Distance Between Unequal Words in Array II 🔒](https://leetcode.com/problems/maximum-distance-between-unequal-words-in-array-ii)

[中文文档](/solution/3700-3799/3706.Maximum%20Distance%20Between%20Unequal%20Words%20in%20Array%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng chuỗi <code>words</code>.</p>

<p>Hãy tìm <strong>khoảng cách lớn nhất</strong> giữa hai chỉ số <strong>khác nhau</strong> <code>i</code> và <code>j</code> sao cho:</p>

<ul>
	<li><code>words[i] != words[j]</code>, và</li>
	<li>khoảng cách được định nghĩa là <code>j - i + 1</code>.</li>
</ul>

<p>Trả về khoảng cách lớn nhất trong tất cả các cặp như vậy. Nếu không tồn tại cặp hợp lệ, trả về 0.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">words = [&quot;leetcode&quot;,&quot;leetcode&quot;,&quot;codeforces&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Trong ví dụ này, <code>words[0]</code> và <code>words[2]</code> không bằng nhau, đồng thời có khoảng cách lớn nhất là <code>2 - 0 + 1 = 3</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">words = [&quot;a&quot;,&quot;b&quot;,&quot;c&quot;,&quot;a&quot;,&quot;a&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Trong ví dụ này, <code>words[1]</code> và <code>words[4]</code> có khoảng cách lớn nhất là <code>4 - 1 + 1 = 4</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">words = [&quot;z&quot;,&quot;z&quot;,&quot;z&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong>​​​​​​​</strong>Trong ví dụ này, tất cả các từ đều bằng nhau, nên đáp án là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 10<sup>5</sup></code><span style="display: none;"> </span></li>
	<li><code>1 &lt;= words[i].length &lt;= 10</code></li>
	<li><code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải: Duyệt một lần

<!-- thinking:start -->

> **Tư duy**
>
> $n\le 10^5$ không cho phép liệt kê tất cả các cặp không bằng nhau. Nếu cả hai đầu mút của một cặp tối ưu đều không nằm ở biên, thì các từ ở hai biên phải lần lượt trùng với đầu mút gần hơn; nếu không, ta có thể mở rộng cặp đó. Khi ấy, hai từ ở hai biên sẽ khác nhau và bản thân chúng đã tạo ra khoảng cách $n$, mâu thuẫn với tính tối ưu. Vì vậy, một đầu mút tối ưu phải nằm ở biên, và chỉ cần so sánh mỗi vị trí với từ đầu tiên và từ cuối cùng.

<!-- thinking:end -->

Ta có thể nhận thấy rằng ít nhất một trong hai từ có khoảng cách lớn nhất phải nằm ở một trong hai đầu mảng (tức là ở chỉ số $0$ hoặc $n - 1$). Nếu không, giả sử hai từ có khoảng cách lớn nhất nằm ở các chỉ số $i$ và $j$, trong đó $0 < i < j < n - 1$. Khi đó, $\textit{words}[0]$ phải giống $\textit{words}[j]$, và $\textit{words}[n - 1]$ phải giống $\textit{words}[i]$ (nếu không thì khoảng cách sẽ lớn hơn). Điều này có nghĩa là $\textit{words}[0]$ và $\textit{words}[n - 1]$ khác nhau, và khoảng cách của chúng $n - 1 - 0 + 1 = n$ chắc chắn lớn hơn $j - i + 1$, mâu thuẫn với giả định của chúng ta. Do đó, ít nhất một trong hai từ có khoảng cách lớn nhất phải nằm ở một trong hai đầu mảng.

Vì vậy, ta chỉ cần duyệt qua mảng, tính khoảng cách giữa mỗi từ với các từ ở hai đầu mảng, rồi cập nhật khoảng cách lớn nhất.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{words}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxDistance(self, words: List[str]) -> int:
        n = len(words)
        ans = 0
        for i in range(n):
            if words[i] != words[0]:
                ans = max(ans, i + 1)
            if words[i] != words[-1]:
                ans = max(ans, n - i)
        return ans
```

#### Java

```java
class Solution {
    public int maxDistance(String[] words) {
        int n = words.length;
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            if (!words[i].equals(words[0])) {
                ans = Math.max(ans, i + 1);
            }
            if (!words[i].equals(words[n - 1])) {
                ans = Math.max(ans, n - i);
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
    int maxDistance(vector<string>& words) {
        int n = words.size();
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            if (words[i] != words[0]) {
                ans = max(ans, i + 1);
            }
            if (words[i] != words[n - 1]) {
                ans = max(ans, n - i);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxDistance(words []string) int {
	n := len(words)
	ans := 0
	for i := 0; i < n; i++ {
		if words[i] != words[0] {
			ans = max(ans, i+1)
		}
		if words[i] != words[n-1] {
			ans = max(ans, n-i)
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maxDistance(words: string[]): number {
    const n = words.length;
    let ans = 0;
    for (let i = 0; i < n; i++) {
        if (words[i] !== words[0]) {
            ans = Math.max(ans, i + 1);
        }
        if (words[i] !== words[n - 1]) {
            ans = Math.max(ans, n - i);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
