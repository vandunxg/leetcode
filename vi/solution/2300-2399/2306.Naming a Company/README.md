---
comments: true
difficulty: Hard
rating: 2305
source: Weekly Contest 297 Q4
tags:
    - Bit Manipulation
    - Array
    - Hash Table
    - String
    - Enumeration
---

<!-- problem:start -->

# [2306. Naming a Company](https://leetcode.com/problems/naming-a-company)

[Tài liệu tiếng Trung](/solution/2300-2399/2306.Naming%20a%20Company/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng chuỗi <code>ideas</code>, biểu diễn danh sách các tên được dùng trong quá trình đặt tên cho một công ty. Quy trình đặt tên công ty như sau:</p>

<ol>
	<li>Chọn 2 tên <strong>khác nhau</strong> từ <code>ideas</code>, gọi chúng là <code>idea<sub>A</sub></code> và <code>idea<sub>B</sub></code>.</li>
	<li>Đổi chỗ chữ cái đầu tiên của <code>idea<sub>A</sub></code> và <code>idea<sub>B</sub></code> cho nhau.</li>
	<li>Nếu <strong>cả hai</strong> tên mới đều không xuất hiện trong <code>ideas</code> ban đầu, thì tên <code>idea<sub>A</sub> idea<sub>B</sub></code> (phép <strong>nối</strong> <code>idea<sub>A</sub></code> và <code>idea<sub>B</sub></code>, được ngăn cách bởi một dấu cách) là một tên công ty hợp lệ.</li>
	<li>Ngược lại, đó không phải là một tên hợp lệ.</li>
</ol>

<p>Trả về <em>số lượng tên hợp lệ <strong>khác nhau</strong> của công ty</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> ideas = [&quot;coffee&quot;,&quot;donuts&quot;,&quot;time&quot;,&quot;toffee&quot;]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Các lựa chọn sau là hợp lệ:
- (&quot;coffee&quot;, &quot;donuts&quot;): Tên công ty được tạo ra là &quot;doffee conuts&quot;.
- (&quot;donuts&quot;, &quot;coffee&quot;): Tên công ty được tạo ra là &quot;conuts doffee&quot;.
- (&quot;donuts&quot;, &quot;time&quot;): Tên công ty được tạo ra là &quot;tonuts dime&quot;.
- (&quot;donuts&quot;, &quot;toffee&quot;): Tên công ty được tạo ra là &quot;tonuts doffee&quot;.
- (&quot;time&quot;, &quot;donuts&quot;): Tên công ty được tạo ra là &quot;dime tonuts&quot;.
- (&quot;toffee&quot;, &quot;donuts&quot;): Tên công ty được tạo ra là &quot;doffee tonuts&quot;.
Vì vậy, có tổng cộng 6 tên công ty khác nhau.

Sau đây là một số lựa chọn không hợp lệ:
- (&quot;coffee&quot;, &quot;time&quot;): Tên &quot;toffee&quot; được tạo ra sau khi đổi chỗ đã tồn tại trong mảng ban đầu.
- (&quot;time&quot;, &quot;toffee&quot;): Cả hai tên vẫn không đổi sau khi đổi chỗ và đều tồn tại trong mảng ban đầu.
- (&quot;coffee&quot;, &quot;toffee&quot;): Cả hai tên được tạo ra sau khi đổi chỗ đều đã tồn tại trong mảng ban đầu.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> ideas = [&quot;lack&quot;,&quot;back&quot;]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có lựa chọn hợp lệ nào. Vì vậy, kết quả trả về là 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= ideas.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= ideas[i].length &lt;= 10</code></li>
	<li><code>ideas[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li>Tất cả các chuỗi trong <code>ideas</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê và đếm

<!-- thinking:start -->

> **Tư duy**
>
> Một tên hợp lệ đổi chữ cái đầu tiên của hai ý tưởng khác nhau, đồng thời cả hai kết quả đều không được xuất hiện trong tập ban đầu. Kiểm tra từng cặp có độ phức tạp $O(n^2)$ nên không phù hợp với $n \le 5 \times 10^4$.
>
> Chỉ có $26$ chữ cái đầu tiên; việc các hậu tố có trùng nhau hay không sẽ quyết định một phép đổi chỗ có hợp lệ hay không. Sau khi lưu các ý tưởng vào một tập hợp, ta đếm $f[i][j]$: số chuỗi bắt đầu bằng chữ cái $i$ mà có thể đổi chữ cái đầu tiên thành $j$ mà không trùng với tập hợp. Ở lượt duyệt thứ hai, ta cộng thêm $f[j][i]$ với mỗi ý tưởng có thể đổi thành $j$, nhờ đó đếm các cặp trong thời gian $O(n|\Sigma|)$.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là số chuỗi trong $\textit{ideas}$ bắt đầu bằng chữ cái thứ $i$ và khi thay bằng chữ cái thứ $j$ thì không tồn tại trong $\textit{ideas}$. Ban đầu, $f[i][j] = 0$. Ngoài ra, ta sử dụng một bảng băm $s$ để lưu các chuỗi trong $\textit{ideas}$, giúp nhanh chóng xác định một chuỗi có nằm trong $\textit{ideas}$ hay không.

Tiếp theo, ta duyệt các chuỗi trong $\textit{ideas}$. Với chuỗi hiện tại $v$, ta liệt kê chữ cái đầu tiên $j$ sau khi thay thế. Nếu chuỗi thu được sau khi thay $v$ không nằm trong $\textit{ideas}$, ta cập nhật $f[i][j] = f[i][j] + 1$.

Cuối cùng, ta duyệt lại các chuỗi trong $\textit{ideas}$. Với chuỗi hiện tại $v$, ta liệt kê chữ cái đầu tiên $j$ sau khi thay thế. Nếu chuỗi thu được sau khi thay $v$ không nằm trong $\textit{ideas}$, ta cập nhật đáp án $\textit{ans} = \textit{ans} + f[j][i]$.

Đáp án cuối cùng là $\textit{ans}$.

Độ phức tạp thời gian là $O(n \times m \times |\Sigma|)$, còn độ phức tạp không gian là $O(|\Sigma|^2)$. Trong đó, $n$ và $m$ lần lượt là số chuỗi trong $\textit{ideas}$ và độ dài lớn nhất của các chuỗi, còn $|\Sigma|$ là tập ký tự của các chuỗi, với $|\Sigma| \leq 26$ trong bài toán này.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def distinctNames(self, ideas: List[str]) -> int:
        s = set(ideas)
        f = [[0] * 26 for _ in range(26)]
        for v in ideas:
            i = ord(v[0]) - ord('a')
            t = list(v)
            for j in range(26):
                t[0] = chr(ord('a') + j)
                if ''.join(t) not in s:
                    f[i][j] += 1
        ans = 0
        for v in ideas:
            i = ord(v[0]) - ord('a')
            t = list(v)
            for j in range(26):
                t[0] = chr(ord('a') + j)
                if ''.join(t) not in s:
                    ans += f[j][i]
        return ans
```

#### Java

```java
class Solution {
    public long distinctNames(String[] ideas) {
        Set<String> s = new HashSet<>();
        for (String v : ideas) {
            s.add(v);
        }
        int[][] f = new int[26][26];
        for (String v : ideas) {
            char[] t = v.toCharArray();
            int i = t[0] - 'a';
            for (int j = 0; j < 26; ++j) {
                t[0] = (char) (j + 'a');
                if (!s.contains(String.valueOf(t))) {
                    ++f[i][j];
                }
            }
        }
        long ans = 0;
        for (String v : ideas) {
            char[] t = v.toCharArray();
            int i = t[0] - 'a';
            for (int j = 0; j < 26; ++j) {
                t[0] = (char) (j + 'a');
                if (!s.contains(String.valueOf(t))) {
                    ans += f[j][i];
                }
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
    long long distinctNames(vector<string>& ideas) {
        unordered_set<string> s(ideas.begin(), ideas.end());
        int f[26][26]{};
        for (auto v : ideas) {
            int i = v[0] - 'a';
            for (int j = 0; j < 26; ++j) {
                v[0] = j + 'a';
                if (!s.count(v)) {
                    ++f[i][j];
                }
            }
        }
        long long ans = 0;
        for (auto& v : ideas) {
            int i = v[0] - 'a';
            for (int j = 0; j < 26; ++j) {
                v[0] = j + 'a';
                if (!s.count(v)) {
                    ans += f[j][i];
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func distinctNames(ideas []string) (ans int64) {
	s := map[string]bool{}
	for _, v := range ideas {
		s[v] = true
	}
	f := [26][26]int{}
	for _, v := range ideas {
		i := int(v[0] - 'a')
		t := []byte(v)
		for j := 0; j < 26; j++ {
			t[0] = 'a' + byte(j)
			if !s[string(t)] {
				f[i][j]++
			}
		}
	}

	for _, v := range ideas {
		i := int(v[0] - 'a')
		t := []byte(v)
		for j := 0; j < 26; j++ {
			t[0] = 'a' + byte(j)
			if !s[string(t)] {
				ans += int64(f[j][i])
			}
		}
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
