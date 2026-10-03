---
comments: true
difficulty: Medium
rating: 1828
source: Weekly Contest 227 Q3
tags:
    - Greedy
    - Two Pointers
    - String
---

<!-- problem:start -->

# [1754. Largest Merge Of Two Strings](https://leetcode.com/problems/largest-merge-of-two-strings)

[中文文档](/solution/1700-1799/1754.Largest%20Merge%20Of%20Two%20Strings/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>word1</code> và <code>word2</code>. Bạn cần tạo chuỗi <code>merge</code> theo cách sau: khi <code>word1</code> hoặc <code>word2</code> còn khác rỗng, chọn <strong>một</strong> trong các phương án sau:</p>

<ul>
	<li>Nếu <code>word1</code> khác rỗng, thêm ký tự <strong>đầu tiên</strong> của <code>word1</code> vào <code>merge</code> rồi xóa ký tự đó khỏi <code>word1</code>.

    <ul>
    <li>Ví dụ, nếu <code>word1 = &quot;abc&quot; </code>và <code>merge = &quot;dv&quot;</code>, sau thao tác này, <code>word1 = &quot;bc&quot;</code> và <code>merge = &quot;dva&quot;</code>.</li>
    </ul>
    </li>
    <li>Nếu <code>word2</code> khác rỗng, thêm ký tự <strong>đầu tiên</strong> của <code>word2</code> vào <code>merge</code> rồi xóa ký tự đó khỏi <code>word2</code>.
    <ul>
    <li>Ví dụ, nếu <code>word2 = &quot;abc&quot; </code>và <code>merge = &quot;&quot;</code>, sau thao tác này, <code>word2 = &quot;bc&quot;</code> và <code>merge = &quot;a&quot;</code>.</li>
    </ul>
    </li>

</ul>

<p>Trả về <em><strong>lớn nhất theo thứ tự từ điển</strong> </em><code>merge</code><em> mà bạn có thể tạo</em>.</p>

<p>Chuỗi <code>a</code> lớn hơn chuỗi <code>b</code> theo thứ tự từ điển (khi có cùng độ dài) nếu tại vị trí đầu tiên mà <code>a</code> và <code>b</code> khác nhau, ký tự của <code>a</code> lớn hơn hẳn ký tự tương ứng của <code>b</code>. Ví dụ, <code>&quot;abcd&quot;</code> lớn hơn <code>&quot;abcc&quot;</code> theo thứ tự từ điển vì vị trí khác nhau đầu tiên là ký tự thứ tư, và <code>d</code> lớn hơn <code>c</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> word1 = &quot;cabaa&quot;, word2 = &quot;bcaaa&quot;
<strong>Đầu ra:</strong> &quot;cbcabaaaaa&quot;
<strong>Giải thích:</strong> Một cách để tạo merge lớn nhất theo thứ tự từ điển là:
- Lấy từ word1: merge = &quot;c&quot;, word1 = &quot;abaa&quot;, word2 = &quot;bcaaa&quot;
- Lấy từ word2: merge = &quot;cb&quot;, word1 = &quot;abaa&quot;, word2 = &quot;caaa&quot;
- Lấy từ word2: merge = &quot;cbc&quot;, word1 = &quot;abaa&quot;, word2 = &quot;aaa&quot;
- Lấy từ word1: merge = &quot;cbca&quot;, word1 = &quot;baa&quot;, word2 = &quot;aaa&quot;
- Lấy từ word1: merge = &quot;cbcab&quot;, word1 = &quot;aa&quot;, word2 = &quot;aaa&quot;
- Thêm 5 ký tự a còn lại của word1 và word2 vào cuối merge.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> word1 = &quot;abcabc&quot;, word2 = &quot;abdcaba&quot;
<strong>Đầu ra:</strong> &quot;abdcabcabcaba&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= word1.length, word2.length &lt;= 3000</code></li>
	<li><code>word1</code> và <code>word2</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi bước lấy ký tự đầu tiên của một chuỗi. Merge lớn nhất theo thứ tự từ điển chọn phía có hậu tố còn lại lớn hơn, chứ không chỉ chọn ký tự kế tiếp lớn hơn.
>
> Hai con trỏ so sánh $word1[i:]$ và $word2[j:]$, thêm ký tự đầu tiên của phía lớn hơn rồi nối phần còn lại.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestMerge(self, word1: str, word2: str) -> str:
        i = j = 0
        ans = []
        while i < len(word1) and j < len(word2):
            if word1[i:] > word2[j:]:
                ans.append(word1[i])
                i += 1
            else:
                ans.append(word2[j])
                j += 1
        ans.append(word1[i:])
        ans.append(word2[j:])
        return "".join(ans)
```

#### Java

```java
class Solution {
    public String largestMerge(String word1, String word2) {
        int m = word1.length(), n = word2.length();
        int i = 0, j = 0;
        StringBuilder ans = new StringBuilder();
        while (i < m && j < n) {
            boolean gt = word1.substring(i).compareTo(word2.substring(j)) > 0;
            ans.append(gt ? word1.charAt(i++) : word2.charAt(j++));
        }
        ans.append(word1.substring(i));
        ans.append(word2.substring(j));
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string largestMerge(string word1, string word2) {
        int m = word1.size(), n = word2.size();
        int i = 0, j = 0;
        string ans;
        while (i < m && j < n) {
            bool gt = word1.substr(i) > word2.substr(j);
            ans += gt ? word1[i++] : word2[j++];
        }
        ans += word1.substr(i);
        ans += word2.substr(j);
        return ans;
    }
};
```

#### Go

```go
func largestMerge(word1 string, word2 string) string {
	m, n := len(word1), len(word2)
	i, j := 0, 0
	var ans strings.Builder
	for i < m && j < n {
		if word1[i:] > word2[j:] {
			ans.WriteByte(word1[i])
			i++
		} else {
			ans.WriteByte(word2[j])
			j++
		}
	}
	ans.WriteString(word1[i:])
	ans.WriteString(word2[j:])
	return ans.String()
}
```

#### TypeScript

```ts
function largestMerge(word1: string, word2: string): string {
    const m = word1.length;
    const n = word2.length;
    let ans = '';
    let i = 0;
    let j = 0;
    while (i < m && j < n) {
        ans += word1.slice(i) > word2.slice(j) ? word1[i++] : word2[j++];
    }
    ans += word1.slice(i);
    ans += word2.slice(j);
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn largest_merge(word1: String, word2: String) -> String {
        let word1 = word1.as_bytes();
        let word2 = word2.as_bytes();
        let m = word1.len();
        let n = word2.len();
        let mut ans = String::new();
        let mut i = 0;
        let mut j = 0;
        while i < m && j < n {
            if word1[i..] > word2[j..] {
                ans.push(word1[i] as char);
                i += 1;
            } else {
                ans.push(word2[j] as char);
                j += 1;
            }
        }
        word1[i..].iter().for_each(|c| ans.push(*c as char));
        word2[j..].iter().for_each(|c| ans.push(*c as char));
        ans
    }
}
```

#### C

```c
char* largestMerge(char* word1, char* word2) {
    int m = strlen(word1);
    int n = strlen(word2);
    int i = 0;
    int j = 0;
    char* ans = malloc((m + n + 1) * sizeof(char));
    while (i < m && j < n) {
        int k = 0;
        while (word1[i + k] && word2[j + k] && word1[i + k] == word2[j + k]) {
            k++;
        }
        if (word1[i + k] > word2[j + k]) {
            ans[i + j] = word1[i];
            i++;
        } else {
            ans[i + j] = word2[j];
            j++;
        };
    }
    while (word1[i]) {
        ans[i + j] = word1[i];
        i++;
    }
    while (word2[j]) {
        ans[i + j] = word2[j];
        j++;
    }
    ans[m + n] = '\0';
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
