---
comments: true
difficulty: Easy
rating: 1248
source: Weekly Contest 484 Q1
tags:
    - Hash Table
    - String
---

<!-- problem:start -->

# [3803. Count Residue Prefixes](https://leetcode.com/problems/count-residue-prefixes)

[中文文档](/solution/3800-3899/3803.Count%20Residue%20Prefixes/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</p>

<p>Một <strong>tiền tố</strong> của <code>s</code> được gọi là <strong>residue</strong> nếu số lượng <strong>ký tự phân biệt</strong> trong <strong>tiền tố</strong> bằng <code>len(prefix) % 3</code>.</p>

<p>Hãy trả về số lượng tiền tố <strong>residue</strong> trong <code>s</code>.</p>
Một <strong>tiền tố</strong> của chuỗi là một <strong>chuỗi con không rỗng</strong> bắt đầu từ đầu chuỗi và kéo dài đến bất kỳ vị trí nào trong chuỗi đó.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abc&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<ul>
	<li>Tiền tố <code>&quot;a&quot;</code> có 1 ký tự phân biệt và độ dài modulo 3 bằng 1, nên đây là một residue.</li>
	<li>Tiền tố <code>&quot;ab&quot;</code> có 2 ký tự phân biệt và độ dài modulo 3 bằng 2, nên đây là một residue.</li>
	<li>Tiền tố <code>&quot;abc&quot;</code> không thỏa mãn điều kiện. Do đó, đáp án là 2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;dd&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tiền tố <code>&quot;d&quot;</code> có 1 ký tự phân biệt và độ dài modulo 3 bằng 1, nên đây là một residue.</li>
	<li>Tiền tố <code>&quot;dd&quot;</code> có 1 ký tự phân biệt nhưng độ dài modulo 3 bằng 2, nên đây không phải là một residue. Do đó, đáp án là 1.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;bob&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tiền tố <code>&quot;b&quot;</code> có 1 ký tự phân biệt và độ dài modulo 3 bằng 1, nên đây là một residue.</li>
	<li>Tiền tố <code>&quot;bo&quot;</code> có 2 ký tự phân biệt và độ dài modulo 3 bằng 2, nên đây là một residue. Do đó, đáp án là 2.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi tiền tố, ta cần kiểm tra xem số lượng ký tự phân biệt có bằng độ dài modulo $3$ hay không. Vì $|s| \le 100$, ta có thể duyệt lại từng tiền tố, nhưng cách đó sẽ đếm lại các ký tự giống nhau.
>
> Các tiền tố lồng nhau: tiền tố thứ $i$ chỉ thêm một ký tự vào tiền tố trước đó, nên tập hợp các ký tự phân biệt chỉ có thể thêm phần tử.
>
> Ta duy trì một set các ký tự đã gặp, cập nhật kích thước của set sau mỗi chỉ số, rồi so sánh với $i \bmod 3$.
>
> Chỉ cần duyệt chuỗi một lần từ trái sang phải là có thể đếm mọi tiền tố residue.

<!-- thinking:end -->

Ta sử dụng một bảng băm $\textit{st}$ để lưu tập hợp các ký tự phân biệt đã xuất hiện trong tiền tố hiện tại. Ta duyệt qua từng ký tự $c$ trong chuỗi $s$, thêm ký tự đó vào tập hợp $\textit{st}$, rồi kiểm tra xem độ dài của tiền tố hiện tại modulo $3$ có bằng kích thước của tập hợp $\textit{st}$ hay không. Nếu bằng nhau, tiền tố hiện tại là một tiền tố residue, và ta tăng đáp án lên $1$.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def residuePrefixes(self, s: str) -> int:
        st = set()
        ans = 0
        for i, c in enumerate(s, 1):
            st.add(c)
            if len(st) == i % 3:
                ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int residuePrefixes(String s) {
        Set<Character> st = new HashSet<>();
        int ans = 0;
        for (int i = 1; i <= s.length(); i++) {
            char c = s.charAt(i - 1);
            st.add(c);
            if (st.size() == i % 3) {
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
    int residuePrefixes(string s) {
        unordered_set<char> st;
        int ans = 0;
        for (int i = 1; i <= s.size(); i++) {
            char c = s[i - 1];
            st.insert(c);
            if (st.size() == i % 3) {
                ans++;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func residuePrefixes(s string) int {
	st := make(map[rune]struct{})
	ans := 0
	for i, c := range s {
		idx := i + 1
		st[c] = struct{}{}
		if len(st) == idx%3 {
			ans++
		}
	}
	return ans
}
```

#### TypeScript

```ts
function residuePrefixes(s: string): number {
    const st = new Set<string>();
    let ans = 0;
    for (let i = 0; i < s.length; i++) {
        const c = s[i];
        st.add(c);
        if (st.size === (i + 1) % 3) {
            ans++;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
