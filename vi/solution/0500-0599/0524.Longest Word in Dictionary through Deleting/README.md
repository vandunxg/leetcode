---
comments: true
difficulty: Medium
tags:
    - Array
    - Two Pointers
    - String
    - Sorting
---

<!-- problem:start -->

# [524. Longest Word in Dictionary through Deleting](https://leetcode.com/problems/longest-word-in-dictionary-through-deleting)

[中文文档](/solution/0500-0599/0524.Longest%20Word%20in%20Dictionary%20through%20Deleting/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> và mảng chuỗi <code>dictionary</code>, hãy trả về <em>chuỗi dài nhất trong dictionary có thể tạo thành bằng cách xóa một số ký tự khỏi chuỗi đã cho</em>. Nếu có nhiều kết quả phù hợp, hãy trả về từ dài nhất có thứ tự từ điển nhỏ nhất. Nếu không có kết quả nào, hãy trả về chuỗi rỗng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abpcplea&quot;, dictionary = [&quot;ale&quot;,&quot;apple&quot;,&quot;monkey&quot;,&quot;plea&quot;]
<strong>Đầu ra:</strong> &quot;apple&quot;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abpcplea&quot;, dictionary = [&quot;a&quot;,&quot;b&quot;,&quot;c&quot;]
<strong>Đầu ra:</strong> &quot;a&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>1 &lt;= dictionary.length &lt;= 1000</code></li>
	<li><code>1 &lt;= dictionary[i].length &lt;= 1000</code></li>
	<li><code>s</code> và <code>dictionary[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Kiểm tra dãy con

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm từ dài nhất trong dictionary là dãy con của $s$; nếu hòa, chọn từ có thứ tự từ điển nhỏ hơn. Việc liệt kê mọi dãy con của $s$ là không khả thi.
>
> Dùng two pointers để kiểm tra từng từ trong dictionary có phải dãy con của $s$ hay không, rồi giữ từ tốt nhất theo độ dài và thứ tự từ điển. Kích thước dictionary và độ dài các từ không lớn, nên tổng chi phí ở mức chấp nhận được.

<!-- thinking:end -->

Ta định nghĩa hàm $check(s, t)$ để xác định chuỗi $s$ có phải là dãy con của chuỗi $t$ hay không. Dùng two pointers $i$ và $j$, lần lượt trỏ đến đầu chuỗi $s$ và $t$, rồi liên tục di chuyển $j$. Nếu $s[i]$ bằng $t[j]$, ta tăng $i$. Cuối cùng, kiểm tra $i$ có bằng độ dài của $s$ hay không. Nếu bằng, $s$ là dãy con của $t$.

Khởi tạo chuỗi kết quả $ans$ là chuỗi rỗng. Sau đó, duyệt từng chuỗi $t$ trong mảng $dictionary$. Nếu $t$ là dãy con của $s$ và độ dài của $t$ lớn hơn độ dài của $ans$, hoặc hai chuỗi có cùng độ dài nhưng $t$ nhỏ hơn $ans$ theo thứ tự từ điển, ta cập nhật $ans$ thành $t$.

Độ phức tạp thời gian là $O(d \times (m + n))$, trong đó $d$ là số chuỗi trong danh sách, còn $m$ và $n$ lần lượt là độ dài của chuỗi $s$ và độ dài trung bình của các chuỗi trong danh sách. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findLongestWord(self, s: str, dictionary: List[str]) -> str:
        def check(s: str, t: str) -> bool:
            m, n = len(s), len(t)
            i = j = 0
            while i < m and j < n:
                if s[i] == t[j]:
                    i += 1
                j += 1
            return i == m

        ans = ""
        for t in dictionary:
            if check(t, s) and (len(ans) < len(t) or (len(ans) == len(t) and ans > t)):
                ans = t
        return ans
```

#### Java

```java
class Solution {
    public String findLongestWord(String s, List<String> dictionary) {
        String ans = "";
        for (String t : dictionary) {
            int a = ans.length(), b = t.length();
            if (check(t, s) && (a < b || (a == b && t.compareTo(ans) < 0))) {
                ans = t;
            }
        }
        return ans;
    }

    private boolean check(String s, String t) {
        int m = s.length(), n = t.length();
        int i = 0;
        for (int j = 0; i < m && j < n; ++j) {
            if (s.charAt(i) == t.charAt(j)) {
                ++i;
            }
        }
        return i == m;
    }
}
```

#### C++

```cpp
class Solution {
public:
    string findLongestWord(string s, vector<string>& dictionary) {
        string ans = "";
        auto check = [&](const string& s, const string& t) {
            int m = s.size(), n = t.size();
            int i = 0;
            for (int j = 0; i < m && j < n; ++j) {
                if (s[i] == t[j]) {
                    ++i;
                }
            }
            return i == m;
        };
        for (auto& t : dictionary) {
            int a = ans.size(), b = t.size();
            if (check(t, s) && (a < b || (a == b && ans > t))) {
                ans = t;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findLongestWord(s string, dictionary []string) string {
	ans := ""
	check := func(s, t string) bool {
		m, n := len(s), len(t)
		i := 0
		for j := 0; i < m && j < n; j++ {
			if s[i] == t[j] {
				i++
			}
		}
		return i == m
	}
	for _, t := range dictionary {
		a, b := len(ans), len(t)
		if check(t, s) && (a < b || (a == b && ans > t)) {
			ans = t
		}
	}
	return ans
}
```

#### TypeScript

```ts
function findLongestWord(s: string, dictionary: string[]): string {
    const check = (s: string, t: string): boolean => {
        const [m, n] = [s.length, t.length];
        let i = 0;
        for (let j = 0; i < m && j < n; ++j) {
            if (s[i] === t[j]) {
                ++i;
            }
        }
        return i === m;
    };
    let ans: string = '';
    for (const t of dictionary) {
        const [a, b] = [ans.length, t.length];
        if (check(t, s) && (a < b || (a === b && ans > t))) {
            ans = t;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_longest_word(s: String, dictionary: Vec<String>) -> String {
        let mut ans = String::new();
        for t in dictionary {
            let a = ans.len();
            let b = t.len();
            if Self::check(&t, &s) && (a < b || (a == b && t < ans)) {
                ans = t;
            }
        }
        ans
    }

    fn check(s: &str, t: &str) -> bool {
        let (m, n) = (s.len(), t.len());
        let mut i = 0;
        let mut j = 0;
        let s: Vec<char> = s.chars().collect();
        let t: Vec<char> = t.chars().collect();

        while i < m && j < n {
            if s[i] == t[j] {
                i += 1;
            }
            j += 1;
        }
        i == m
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
