---
comments: true
difficulty: Easy
rating: 1240
source: Biweekly Contest 176 Q1
tags:
    - Array
    - String
    - Simulation
---

<!-- problem:start -->

# [3838. Weighted Word Mapping](https://leetcode.com/problems/weighted-word-mapping)

[中文文档](/solution/3800-3899/3838.Weighted%20Word%20Mapping/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng chuỗi <code>words</code>, trong đó mỗi chuỗi là một từ chỉ gồm các chữ cái tiếng Anh viết thường.</p>

<p>Bạn cũng được cho một mảng số nguyên <code>weights</code> có độ dài 26, trong đó <code>weights[i]</code> biểu thị trọng số của chữ cái tiếng Anh viết thường thứ <code>i<sup>th</sup></code>.</p>

<p><strong>Trọng số</strong> của một từ được định nghĩa là <strong>tổng</strong> trọng số của các ký tự trong từ đó.</p>

<p>Với mỗi từ, lấy trọng số của từ đó modulo 26 rồi ánh xạ kết quả thành một chữ cái tiếng Anh viết thường theo thứ tự bảng chữ cái ngược (<code>0 -&gt; &#39;z&#39;, 1 -&gt; &#39;y&#39;, ..., 25 -&gt; &#39;a&#39;</code>).</p>

<p>Trả về một chuỗi được tạo bằng cách nối các chữ cái sau khi ánh xạ của tất cả các từ theo đúng thứ tự.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">words = [&quot;abcd&quot;,&quot;def&quot;,&quot;xyz&quot;], weights = [5,3,12,14,1,2,3,2,10,6,6,9,7,8,7,10,8,9,6,9,9,8,3,7,7,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;rij&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Trọng số của <code>&quot;abcd&quot;</code> là <code>5 + 3 + 12 + 14 = 34</code>. Kết quả modulo 26 là <code>34 % 26 = 8</code>, ánh xạ thành <code>&#39;r&#39;</code>.</li>
	<li>Trọng số của <code>&quot;def&quot;</code> là <code>14 + 1 + 2 = 17</code>. Kết quả modulo 26 là <code>17 % 26 = 17</code>, ánh xạ thành <code>&#39;i&#39;</code>.</li>
	<li>Trọng số của <code>&quot;xyz&quot;</code> là <code>7 + 7 + 2 = 16</code>. Kết quả modulo 26 là <code>16 % 26 = 16</code>, ánh xạ thành <code>&#39;j&#39;</code>.</li>
</ul>

<p>Vì vậy, chuỗi được tạo bằng cách nối các chữ cái sau khi ánh xạ là <code>&quot;rij&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">words = [&quot;a&quot;,&quot;b&quot;,&quot;c&quot;], weights = [1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;yyy&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mỗi từ có trọng số bằng 1. Kết quả modulo 26 là <code>1 % 26 = 1</code>, ánh xạ thành <code>&#39;y&#39;</code>.</p>

<p>Vì vậy, chuỗi được tạo bằng cách nối các chữ cái sau khi ánh xạ là <code>&quot;yyy&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">words = [&quot;abcd&quot;], weights = [7,5,3,4,3,5,4,9,4,2,2,7,10,2,5,10,6,1,2,2,4,1,3,4,4,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;g&quot;</span></p>

<p><strong>Giải thích:​​​​​​​</strong></p>

<p>Trọng số của <code>&quot;abcd&quot;</code> là <code>7 + 5 + 3 + 4 = 19</code>. Kết quả modulo 26 là <code>19 % 26 = 19</code>, ánh xạ thành <code>&#39;g&#39;</code>.</p>

<p>Vì vậy, chuỗi được tạo bằng cách nối các chữ cái sau khi ánh xạ là <code>&quot;g&quot;</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 100</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 10</code></li>
	<li><code>weights.length == 26</code></li>
	<li><code>1 &lt;= weights[i] &lt;= 100</code></li>
	<li><code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Trọng số của một từ là tổng trọng số các chữ cái, sau đó lấy modulo $26$ và ánh xạ ngược qua bảng chữ cái. Tổng độ dài nhỏ, nên ta làm theo đúng định nghĩa.
>
> Các từ không phụ thuộc lẫn nhau, nên không cần cấu trúc dữ liệu dùng chung.
>
> Với mỗi từ, tính tổng $weights[c-'a']$, lấy modulo $26$, rồi ánh xạ thành chữ cái lùi $s\bmod 26$ bước từ $\texttt{z}$.
>
> Nối các chữ cái sau khi ánh xạ theo thứ tự xuất hiện trong đầu vào.

<!-- thinking:end -->

Ta duyệt qua từng từ $w$ trong $\textit{words}$ và tính trọng số $s$ của từ đó, tức là tổng trọng số của tất cả các ký tự trong từ. Sau đó, ta lấy $s$ modulo 26, ánh xạ kết quả thành một chữ cái tiếng Anh viết thường, cuối cùng nối tất cả các ký tự sau khi ánh xạ và trả về kết quả.

Độ phức tạp thời gian là $O(L)$, trong đó $L$ là tổng độ dài của tất cả các từ trong $\textit{words}$. Độ phức tạp không gian là $O(W)$, trong đó $W$ là độ dài của $\textit{words}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def mapWordWeights(self, words: List[str], weights: List[int]) -> str:
        ans = []
        for w in words:
            s = sum(weights[ord(c) - ord('a')] for c in w)
            ans.append(ascii_lowercase[25 - s % 26])
        return ''.join(ans)
```

#### Java

```java
class Solution {
    public String mapWordWeights(String[] words, int[] weights) {
        var ans = new StringBuilder();
        for (var w : words) {
            int s = 0;
            for (char c : w.toCharArray()) {
                s = (s + weights[c - 'a']) % 26;
            }
            ans.append((char) ('a' + (25 - s)));
        }
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string mapWordWeights(vector<string>& words, vector<int>& weights) {
        string ans;
        for (const string& w : words) {
            int s = 0;
            for (char c : w) {
                s = (s + weights[c - 'a']) % 26;
            }
            ans.push_back(char('a' + (25 - s)));
        }
        return ans;
    }
};
```

#### Go

```go
func mapWordWeights(words []string, weights []int) string {
	ans := make([]byte, 0, len(words))
	for _, w := range words {
		s := 0
		for i := 0; i < len(w); i++ {
			s = (s + weights[int(w[i]-'a')]) % 26
		}
		ans = append(ans, byte('a'+(25-s)))
	}
	return string(ans)
}
```

#### TypeScript

```ts
function mapWordWeights(words: string[], weights: number[]): string {
    const ans: string[] = [];
    for (const w of words) {
        let s = 0;
        for (const c of w) {
            s = (s + weights[c.charCodeAt(0) - 97]) % 26;
        }
        ans.push(String.fromCharCode(97 + (25 - s)));
    }
    return ans.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
