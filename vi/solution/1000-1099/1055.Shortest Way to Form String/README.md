---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Two Pointers
    - String
    - Binary Search
---

<!-- problem:start -->

# [1055. Shortest Way to Form String 🔒](https://leetcode.com/problems/shortest-way-to-form-string)

[中文文档](/solution/1000-1099/1055.Shortest%20Way%20to%20Form%20String/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Dãy con</strong> của một chuỗi là chuỗi mới được tạo bằng cách xóa một số ký tự (có thể không xóa ký tự nào) khỏi chuỗi gốc mà không làm thay đổi thứ tự tương đối của các ký tự còn lại. (Ví dụ, <code>&quot;ace&quot;</code> là dãy con của <code>&quot;<u>a</u>b<u>c</u>d<u>e</u>&quot;</code>, còn <code>&quot;aec&quot;</code> thì không.)</p>

<p>Cho hai chuỗi <code>source</code> và <code>target</code>, hãy trả về <em>số lượng <strong>dãy con</strong> của </em><code>source</code><em> ít nhất sao cho phép nối chúng tạo thành </em><code>target</code>. Nếu không thể, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> source = &quot;abc&quot;, target = &quot;abcbc&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Có thể tạo target &quot;abcbc&quot; bằng &quot;abc&quot; và &quot;bc&quot;, đều là dãy con của source &quot;abc&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> source = &quot;abc&quot;, target = &quot;acdbc&quot;
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không thể tạo target từ các dãy con của source vì target có ký tự &quot;d&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> source = &quot;xyz&quot;, target = &quot;xzyxz&quot;
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Có thể tạo target như sau: &quot;xz&quot; + &quot;y&quot; + &quot;xz&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= source.length, target.length &lt;= 1000</code></li>
	<li><code>source</code> và <code>target</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Two Pointers

<!-- thinking:start -->

> **Tư duy**
>
> Chia $target$ thành ít đoạn nhất, mỗi đoạn là một dãy con của $source$. Vì $m,n\le 1000$, ta có thể dùng hai pointer duyệt $source$ cho mỗi đoạn.
>
> Từ chỉ số hiện tại $j$ của $target$, khớp được càng nhiều ký tự càng tốt khi duyệt $source$. Nếu $j$ không thay đổi thì có ký tự không thể khớp. Nếu không, tính một đoạn và tiếp tục.
>
> Dừng khi $j$ đạt $n$.

<!-- thinking:end -->

Ta có thể dùng phương pháp two pointers, trong đó pointer $j$ trỏ đến chuỗi đích `target`. Sau đó, ta duyệt chuỗi nguồn `source` bằng pointer $i$. Nếu $source[i] = target[j]$, cả $i$ và $j$ cùng tiến lên một bước; nếu không, chỉ pointer $i$ tiến lên. Khi $i$ duyệt hết chuỗi, nếu không khớp được ký tự nào thì trả về $-1$; nếu có, tăng số dãy con lên một, đặt lại $i$ về $0$ rồi tiếp tục duyệt.

Sau khi duyệt xong, trả về số lượng dãy con.

Độ phức tạp thời gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là độ dài của `source` và `target`. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def shortestWay(self, source: str, target: str) -> int:
        def f(i, j):
            while i < m and j < n:
                if source[i] == target[j]:
                    j += 1
                i += 1
            return j

        m, n = len(source), len(target)
        ans = j = 0
        while j < n:
            k = f(0, j)
            if k == j:
                return -1
            j = k
            ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int shortestWay(String source, String target) {
        int m = source.length(), n = target.length();
        int ans = 0, j = 0;
        while (j < n) {
            int i = 0;
            boolean ok = false;
            while (i < m && j < n) {
                if (source.charAt(i) == target.charAt(j)) {
                    ok = true;
                    ++j;
                }
                ++i;
            }
            if (!ok) {
                return -1;
            }
            ++ans;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int shortestWay(string source, string target) {
        int m = source.size(), n = target.size();
        int ans = 0, j = 0;
        while (j < n) {
            int i = 0;
            bool ok = false;
            while (i < m && j < n) {
                if (source[i] == target[j]) {
                    ok = true;
                    ++j;
                }
                ++i;
            }
            if (!ok) {
                return -1;
            }
            ++ans;
        }
        return ans;
    }
};
```

#### Go

```go
func shortestWay(source string, target string) int {
	m, n := len(source), len(target)
	ans, j := 0, 0
	for j < n {
		ok := false
		for i := 0; i < m && j < n; i++ {
			if source[i] == target[j] {
				ok = true
				j++
			}
		}
		if !ok {
			return -1
		}
		ans++
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
