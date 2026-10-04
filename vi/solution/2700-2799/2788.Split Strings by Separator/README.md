---
comments: true
difficulty: Easy
rating: 1239
source: Weekly Contest 355 Q1
tags:
    - Array
    - String
---

<!-- problem:start -->

# [2788. Split Strings by Separator](https://leetcode.com/problems/split-strings-by-separator)

[中文文档](/solution/2700-2799/2788.Split%20Strings%20by%20Separator/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng chuỗi <code>words</code> và một ký tự <code>separator</code>, hãy <strong>tách</strong> từng chuỗi trong <code>words</code> bằng <code>separator</code>.</p>

<p>Trả về <em>một mảng chuỗi chứa các chuỗi mới được tạo thành sau khi tách, <strong>loại bỏ các chuỗi rỗng</strong>.</em></p>

<p><strong>Lưu ý</strong></p>

<ul>
	<li><code>separator</code> được dùng để xác định vị trí tách, nhưng không được đưa vào các chuỗi kết quả.</li>
	<li>Một lần tách có thể tạo ra nhiều hơn hai chuỗi.</li>
	<li>Các chuỗi kết quả phải giữ nguyên thứ tự như khi chúng được cho ban đầu.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;one.two.three&quot;,&quot;four.five&quot;,&quot;six&quot;], separator = &quot;.&quot;
<strong>Đầu ra:</strong> [&quot;one&quot;,&quot;two&quot;,&quot;three&quot;,&quot;four&quot;,&quot;five&quot;,&quot;six&quot;]
<strong>Giải thích: </strong>Trong ví dụ này, ta tách như sau:

&quot;one.two.three&quot; được tách thành &quot;one&quot;, &quot;two&quot;, &quot;three&quot;
&quot;four.five&quot; được tách thành &quot;four&quot;, &quot;five&quot;
&quot;six&quot; được tách thành &quot;six&quot;

Do đó, mảng kết quả là [&quot;one&quot;,&quot;two&quot;,&quot;three&quot;,&quot;four&quot;,&quot;five&quot;,&quot;six&quot;].</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;$easy$&quot;,&quot;$problem$&quot;], separator = &quot;$&quot;
<strong>Đầu ra:</strong> [&quot;easy&quot;,&quot;problem&quot;]
<strong>Giải thích:</strong>Trong ví dụ này, ta tách như sau:

&quot;$easy$&quot; được tách thành &quot;easy&quot; (loại bỏ các chuỗi rỗng)
&quot;$problem$&quot; được tách thành &quot;problem&quot; (loại bỏ các chuỗi rỗng)

Do đó, mảng kết quả là [&quot;easy&quot;,&quot;problem&quot;].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;|||&quot;], separator = &quot;|&quot;
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong>Trong ví dụ này, kết quả tách &quot;|||&quot; chỉ chứa các chuỗi rỗng, nên ta trả về một mảng rỗng []. </pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 100</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 20</code></li>
	<li>các ký tự trong <code>words[i]</code> là các chữ cái tiếng Anh viết thường hoặc các ký tự trong chuỗi <code>&quot;.,|$#@&quot;</code> (không bao gồm dấu ngoặc kép)</li>
	<li><code>separator</code> là một ký tự trong chuỗi <code>&quot;.,|$#@&quot;</code> (không bao gồm dấu ngoặc kép)</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Tách mọi chuỗi theo một dấu phân cách cho trước rồi loại bỏ các phần rỗng. Việc tự duyệt chuỗi tương đương với $split$ kết hợp với việc lọc các phần không rỗng.
>
> Với mỗi từ, dùng $split$ với dấu phân cách rồi giữ lại các mảnh không rỗng.

<!-- thinking:end -->

Ta duyệt qua mảng chuỗi $words$. Với mỗi chuỗi $w$, ta dùng `separator` làm dấu phân cách để tách chuỗi. Nếu chuỗi sau khi tách không rỗng, ta thêm chuỗi đó vào mảng kết quả.

Độ phức tạp thời gian là $O(n \times m)$, còn độ phức tạp không gian là $O(m)$, trong đó $n$ là độ dài của mảng chuỗi $words$ và $m$ là độ dài lớn nhất của các chuỗi trong mảng $words$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def splitWordsBySeparator(self, words: List[str], separator: str) -> List[str]:
        return [s for w in words for s in w.split(separator) if s]
```

#### Java

```java
import java.util.regex.Pattern;

class Solution {
    public List<String> splitWordsBySeparator(List<String> words, char separator) {
        List<String> ans = new ArrayList<>();
        for (var w : words) {
            for (var s : w.split(Pattern.quote(String.valueOf(separator)))) {
                if (s.length() > 0) {
                    ans.add(s);
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
    vector<string> splitWordsBySeparator(vector<string>& words, char separator) {
        vector<string> ans;
        for (const auto& w : words) {
            istringstream ss(w);
            string s;
            while (getline(ss, s, separator)) {
                if (!s.empty()) {
                    ans.push_back(s);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func splitWordsBySeparator(words []string, separator byte) (ans []string) {
	for _, w := range words {
		for _, s := range strings.Split(w, string(separator)) {
			if s != "" {
				ans = append(ans, s)
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function splitWordsBySeparator(words: string[], separator: string): string[] {
    return words.flatMap(w => w.split(separator).filter(s => s.length > 0));
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
