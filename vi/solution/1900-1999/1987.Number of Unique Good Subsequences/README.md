---
comments: true
difficulty: Hard
rating: 2422
source: Weekly Contest 256 Q4
tags:
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [1987. Number of Unique Good Subsequences](https://leetcode.com/problems/number-of-unique-good-subsequences)

[中文文档](/solution/1900-1999/1987.Number%20of%20Unique%20Good%20Subsequences/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một chuỗi nhị phân <code>binary</code>. Một <strong>dãy con</strong> của <code>binary</code> được coi là <strong>hợp lệ</strong> nếu nó <strong>không rỗng</strong> và <strong>không có số 0 ở đầu</strong> (ngoại lệ là <code>&quot;0&quot;</code>).</p>

<p>Hãy tìm số lượng <strong>dãy con hợp lệ khác nhau</strong> của <code>binary</code>.</p>

<ul>
	<li>Ví dụ, nếu <code>binary = &quot;001&quot;</code>, tất cả <strong>dãy con hợp lệ</strong> là <code>[&quot;0&quot;, &quot;0&quot;, &quot;1&quot;]</code>, nên các dãy con hợp lệ <strong>khác nhau</strong> là <code>&quot;0&quot;</code> và <code>&quot;1&quot;</code>. Lưu ý rằng các dãy con <code>&quot;00&quot;</code>, <code>&quot;01&quot;</code> và <code>&quot;001&quot;</code> không hợp lệ vì có số 0 ở đầu.</li>
</ul>

<p>Trả về <em>số lượng <strong>dãy con hợp lệ khác nhau</strong> của </em><code>binary</code>. Vì đáp án có thể rất lớn, hãy trả về kết quả <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p><strong>Dãy con</strong> là một dãy có thể được tạo ra từ một dãy khác bằng cách xóa đi một số hoặc không xóa phần tử nào mà không thay đổi thứ tự của các phần tử còn lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> binary = &quot;001&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Các dãy con hợp lệ của binary là [&quot;0&quot;, &quot;0&quot;, &quot;1&quot;].
Các dãy con hợp lệ khác nhau là &quot;0&quot; và &quot;1&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> binary = &quot;11&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Các dãy con hợp lệ của binary là [&quot;1&quot;, &quot;1&quot;, &quot;11&quot;].
Các dãy con hợp lệ khác nhau là &quot;1&quot; và &quot;11&quot;.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> binary = &quot;101&quot;
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Các dãy con hợp lệ của binary là [&quot;1&quot;, &quot;0&quot;, &quot;1&quot;, &quot;10&quot;, &quot;11&quot;, &quot;101&quot;].
Các dãy con hợp lệ khác nhau là &quot;0&quot;, &quot;1&quot;, &quot;10&quot;, &quot;11&quot; và &quot;101&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= binary.length &lt;= 10<sup>5</sup></code></li>
	<li><code>binary</code> chỉ gồm các ký tự <code>&#39;0&#39;</code> và <code>&#39;1&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Các dãy con hợp lệ không bắt đầu bằng $0$, ngoại trừ dãy con đơn ký tự $0$. Khi đếm các dãy khác nhau, không được đếm trùng các dãy được nối thêm.
>
> $f$ là số dãy kết thúc bằng $1$; $g$ là số dãy bắt đầu bằng $1$ và kết thúc bằng $0$. Một $0$ có thể được nối vào cả hai loại; một $1$ có thể được nối vào cả hai loại hoặc bắt đầu một dãy mới.
>
> Nếu chuỗi từng xuất hiện $0$, ta cộng thêm dãy đơn ký tự $0$.

<!-- thinking:end -->

Ta định nghĩa $f$ là số dãy con hợp lệ khác nhau kết thúc bằng $1$, còn $g$ là số dãy con hợp lệ khác nhau kết thúc bằng $0$ và bắt đầu bằng $1$. Ban đầu, $f = g = 0$.

Với một chuỗi nhị phân, ta có thể duyệt từng bit từ trái sang phải. Giả sử bit hiện tại là $c$:

- Nếu $c = 0$, ta có thể nối $c$ vào các dãy con hợp lệ khác nhau được đếm bởi $f$ và $g$, nên cập nhật $g = (g + f) \bmod (10^9 + 7)$;
- Nếu $c = 1$, ta có thể nối $c$ vào các dãy con hợp lệ khác nhau được đếm bởi $f$ và $g$, đồng thời cũng có thể tạo riêng $c$, nên cập nhật $f = (f + g + 1) \bmod (10^9 + 7)$.

Nếu chuỗi chứa $0$, đáp án cuối cùng là $f + g + 1$; nếu không, đáp án là $f + g$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi. Độ phức tạp không gian là $O(1)$.

Bài tương tự:

- [940. Distinct Subsequences II](https://github.com/doocs/leetcode/blob/main/solution/0900-0999/0940.Distinct%20Subsequences%20II/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfUniqueGoodSubsequences(self, binary: str) -> int:
        f = g = 0
        ans = 0
        mod = 10**9 + 7
        for c in binary:
            if c == "0":
                g = (g + f) % mod
                ans = 1
            else:
                f = (f + g + 1) % mod
        ans = (ans + f + g) % mod
        return ans
```

#### Java

```java
class Solution {
    public int numberOfUniqueGoodSubsequences(String binary) {
        final int mod = (int) 1e9 + 7;
        int f = 0, g = 0;
        int ans = 0;
        for (int i = 0; i < binary.length(); ++i) {
            if (binary.charAt(i) == '0') {
                g = (g + f) % mod;
                ans = 1;
            } else {
                f = (f + g + 1) % mod;
            }
        }
        ans = (ans + f + g) % mod;
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfUniqueGoodSubsequences(string binary) {
        const int mod = 1e9 + 7;
        int f = 0, g = 0;
        int ans = 0;
        for (char& c : binary) {
            if (c == '0') {
                g = (g + f) % mod;
                ans = 1;
            } else {
                f = (f + g + 1) % mod;
            }
        }
        ans = (ans + f + g) % mod;
        return ans;
    }
};
```

#### Go

```go
func numberOfUniqueGoodSubsequences(binary string) (ans int) {
	const mod int = 1e9 + 7
	f, g := 0, 0
	for _, c := range binary {
		if c == '0' {
			g = (g + f) % mod
			ans = 1
		} else {
			f = (f + g + 1) % mod
		}
	}
	ans = (ans + f + g) % mod
	return
}
```

#### TypeScript

```ts
function numberOfUniqueGoodSubsequences(binary: string): number {
    let [f, g] = [0, 0];
    let ans = 0;
    const mod = 1e9 + 7;
    for (const c of binary) {
        if (c === '0') {
            g = (g + f) % mod;
            ans = 1;
        } else {
            f = (f + g + 1) % mod;
        }
    }
    ans = (ans + f + g) % mod;
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
