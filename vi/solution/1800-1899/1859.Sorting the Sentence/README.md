---
comments: true
difficulty: Easy
rating: 1290
source: Biweekly Contest 52 Q1
tags:
    - String
    - Bubble Sort
    - Sorting
---

<!-- problem:start -->

# [1859. Sorting the Sentence](https://leetcode.com/problems/sorting-the-sentence)

[中文文档](/solution/1800-1899/1859.Sorting%20the%20Sentence/README.md)

## Mô tả

<!-- description:start -->

<p>Một <strong>câu</strong> là danh sách các từ được phân tách bằng một dấu cách duy nhất, không có dấu cách ở đầu hoặc cuối. Mỗi từ gồm các chữ cái tiếng Anh viết thường và viết hoa.</p>

<p>Một câu có thể được <strong>xáo trộn</strong> bằng cách thêm <strong>vị trí của từ, đánh số từ 1</strong> vào cuối mỗi từ, sau đó sắp xếp lại các từ trong câu.</p>

<ul>
	<li>Ví dụ, câu <code>&quot;This is a sentence&quot;</code> có thể được xáo trộn thành <code>&quot;sentence4 a3 is2 This1&quot;</code> hoặc <code>&quot;is2 sentence4 This1 a3&quot;</code>.</li>
</ul>

<p>Cho một <strong>câu đã xáo trộn</strong> <code>s</code> có không quá <code>9</code> từ, hãy khôi phục và trả về <em>câu ban đầu</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;is2 sentence4 This1 a3&quot;
<strong>Đầu ra:</strong> &quot;This is a sentence&quot;
<strong>Giải thích:</strong> Sắp xếp các từ trong s theo vị trí ban đầu thành &quot;This1 is2 a3 sentence4&quot;, sau đó xóa các chữ số.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;Myself2 Me1 I4 and3&quot;
<strong>Đầu ra:</strong> &quot;Me Myself and I&quot;
<strong>Giải thích:</strong> Sắp xếp các từ trong s theo vị trí ban đầu thành &quot;Me1 Myself2 and3 I4&quot;, sau đó xóa các chữ số.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= s.length &lt;= 200</code></li>
	<li><code>s</code> gồm các chữ cái tiếng Anh viết thường, viết hoa, dấu cách và các chữ số từ <code>1</code> đến <code>9</code>.</li>
	<li>Số từ trong <code>s</code> nằm trong khoảng từ <code>1</code> đến <code>9</code>.</li>
	<li>Các từ trong <code>s</code> được phân tách bằng một dấu cách duy nhất.</li>
	<li><code>s</code> không có dấu cách ở đầu hoặc cuối.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tách chuỗi

<!-- thinking:start -->

> **Tư duy**
>
> Các từ đã xáo trộn mang theo chỉ số ban đầu ở chữ số cuối. Các chỉ số này là một hoán vị của $1..n$, nên chỉ cần đặt mỗi từ vào đúng vị trí một lần.
>
> Tách theo dấu cách, đặt mỗi từ (không bao gồm chữ số) vào vị trí $\textit{digit}-1$, rồi nối các từ theo thứ tự.

<!-- thinking:end -->

Trước tiên, ta tách chuỗi $s$ theo dấu cách để thu được mảng chuỗi $\textit{ws}$. Sau đó, ta duyệt mảng $\textit{ws}$, trừ ký tự '1' khỏi ký tự cuối của mỗi từ để thu được chỉ số của từ trong kết quả. Ta lấy tiền tố của từ làm nội dung của từ. Cuối cùng, ta nối các từ theo thứ tự chỉ số.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sortSentence(self, s: str) -> str:
        ws = s.split()
        ans = [None] * len(ws)
        for w in ws:
            ans[int(w[-1]) - 1] = w[:-1]
        return " ".join(ans)
```

#### Java

```java
class Solution {
    public String sortSentence(String s) {
        String[] ws = s.split(" ");
        int n = ws.length;
        String[] ans = new String[n];
        for (int i = 0; i < n; ++i) {
            String w = ws[i];
            ans[w.charAt(w.length() - 1) - '1'] = w.substring(0, w.length() - 1);
        }
        return String.join(" ", ans);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string sortSentence(string s) {
        istringstream iss(s);
        string w;
        vector<string> ws;
        while (iss >> w) {
            ws.push_back(w);
        }
        vector<string> ss(ws.size());
        for (auto& w : ws) {
            ss[w.back() - '1'] = w.substr(0, w.size() - 1);
        }
        string ans;
        for (auto& w : ss) {
            ans += w + " ";
        }
        ans.pop_back();
        return ans;
    }
};
```

#### Go

```go
func sortSentence(s string) string {
	ws := strings.Split(s, " ")
	ans := make([]string, len(ws))
	for _, w := range ws {
		ans[w[len(w)-1]-'1'] = w[:len(w)-1]
	}
	return strings.Join(ans, " ")
}
```

#### TypeScript

```ts
function sortSentence(s: string): string {
    const ws = s.split(' ');
    const ans = Array(ws.length);
    for (const w of ws) {
        ans[w.charCodeAt(w.length - 1) - '1'.charCodeAt(0)] = w.slice(0, -1);
    }
    return ans.join(' ');
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @return {string}
 */
var sortSentence = function (s) {
    const ws = s.split(' ');
    const ans = Array(ws.length);
    for (const w of ws) {
        ans[w.charCodeAt(w.length - 1) - '1'.charCodeAt(0)] = w.slice(0, -1);
    }
    return ans.join(' ');
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
