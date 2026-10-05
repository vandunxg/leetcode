---
comments: true
difficulty: Medium
rating: 1608
source: Weekly Contest 501 Q2
---

<!-- problem:start -->

# [3926. Count Valid Word Occurrences](https://leetcode.com/problems/count-valid-word-occurrences)

[中文文档](/solution/3900-3999/3926.Count%20Valid%20Word%20Occurrences/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng chuỗi <code>chunks</code>. Hãy nối tất cả các chuỗi trong <code>chunks</code> theo thứ tự để tạo thành một chuỗi <code>s</code>.</p>

<p>Bạn cũng được cho một mảng chuỗi <code>queries</code>.</p>

<p>Một <strong>gạch nối liên kết</strong> là ký tự gạch nối <code>&#39;-&#39;</code> trong <code>s</code> mà cả ký tự trước và sau nó đều tồn tại và là các chữ cái tiếng Anh viết thường.</p>

<p>Một <strong>từ</strong> là một <span data-keyword="substring-nonempty">chuỗi con</span> <strong>cực đại</strong> của <code>s</code>, chỉ gồm các chữ cái tiếng Anh viết thường và <strong>gạch nối liên kết</strong>.</p>

<p>Tất cả các ký tự khác, bao gồm khoảng trắng và các gạch nối không phải <strong>gạch nối liên kết</strong>, được xem là dấu phân cách.</p>

<p>Trả về một mảng số nguyên <code>ans</code>, trong đó <code>ans[i]</code> là số lần <code>queries[i]</code> xuất hiện dưới dạng một từ trong <code>s</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">chunks = [&quot;hello wor&quot;,&quot;ld hello&quot;], queries = [&quot;hello&quot;,&quot;world&quot;,&quot;wor&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,1,0]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Sau khi nối tất cả các chuỗi trong <code>chunks</code>, <code>s = &quot;hello world hello&quot;</code>.</li>
	<li>Các từ là <code>&quot;hello&quot;</code>, <code>&quot;world&quot;</code> và <code>&quot;hello&quot;</code>.</li>
	<li>Chuỗi con <code>&quot;wor&quot;</code> xuất hiện bên trong <code>&quot;world&quot;</code>, nhưng nó không phải là một từ hoàn chỉnh.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">chunks = [&quot;a-b a--b &quot;,&quot;a-&quot;,&quot;b&quot;], queries = [&quot;a-b&quot;,&quot;a&quot;,&quot;b&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,1,1]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Sau khi nối tất cả các chuỗi trong <code>chunks</code>, <code>s = &quot;a-b a--b a-b&quot;</code>.</li>
	<li>Trong <code>&quot;a-b&quot;</code>, gạch nối là một gạch nối liên kết vì nó nằm giữa hai chữ cái tiếng Anh viết thường, nên <code>&quot;a-b&quot;</code> là một từ.</li>
	<li>Trong <code>&quot;a--b&quot;</code>, không gạch nối nào là gạch nối liên kết, nên chuỗi này được tách thành các từ <code>&quot;a&quot;</code> và <code>&quot;b&quot;</code>.</li>
	<li>Do đó, các từ là <code>&quot;a-b&quot;</code>, <code>&quot;a&quot;</code>, <code>&quot;b&quot;</code> và <code>&quot;a-b&quot;</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">chunks = [&quot;-cat dog- mouse&quot;], queries = [&quot;cat&quot;,&quot;dog&quot;,&quot;mouse&quot;,&quot;cat-dog&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,1,1,0]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Sau khi nối tất cả các chuỗi trong <code>chunks</code>, <code>s = &quot;-cat dog- mouse&quot;</code>.</li>
	<li>Gạch nối ở trước <code>&quot;cat&quot;</code> và gạch nối ở sau <code>&quot;dog&quot;</code> không phải là gạch nối liên kết, nên chúng là các dấu phân cách.</li>
	<li>Các từ là <code>&quot;cat&quot;</code>, <code>&quot;dog&quot;</code> và <code>&quot;mouse&quot;</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= chunks.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= chunks[i].length &lt;= 10<sup>5</sup></code></li>
	<li>Tổng độ dài của tất cả các chuỗi trong <code>chunks</code> không vượt quá <code>10<sup>5</sup></code>.</li>
	<li><code>chunks[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường, khoảng trắng và <code>&#39;-&#39;</code>.</li>
	<li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= queries[i].length &lt;= 10<sup>5</sup></code></li>
	<li>Tổng độ dài của tất cả các chuỗi trong <code>queries</code> không vượt quá <code>10<sup>5</sup></code>.</li>
	<li><code>queries[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường và <code>&#39;-&#39;</code>.</li>
	<li><code>queries[i]</code> là một từ hợp lệ: không bắt đầu hoặc kết thúc bằng <code>&#39;-&#39;</code> và không chứa hai gạch nối liên tiếp.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Tổng số ký tự của các chuỗi trong chunks và queries đều là $10^5$, nên ta không thể quét lại văn bản cho từng query. Một từ bắt đầu bằng một chữ cái và kết thúc ở khoảng trắng hoặc một gạch nối không hợp lệ, vì vậy ta có thể tách văn bản đã nối chỉ một lần.
>
> Nối các chuỗi trong $\textit{chunks}$ thành $s$ rồi quét: bỏ qua các dấu phân cách, mở rộng cho đến khi gặp khoảng trắng hoặc một gạch nối không được theo sau bởi một chữ cái, sau đó đếm các token trong hash map. Mỗi query chỉ cần tra cứu một lần.
>
> Việc tách token và trả lời các query đều có độ phức tạp tuyến tính theo tổng độ dài chuỗi.

<!-- thinking:end -->

Đầu tiên, ta nối tất cả các chuỗi trong $\textit{chunks}$ để thu được một chuỗi duy nhất $s$.

Vì ký tự đầu tiên của một từ hợp lệ phải là một chữ cái tiếng Anh viết thường, ta duyệt $s$ từ trái sang phải. Khi gặp một chữ cái tiếng Anh viết thường, ta tiếp tục duyệt sang phải. Nếu gặp khoảng trắng hoặc một gạch nối không hợp lệ, nghĩa là ta đã tìm thấy một từ. Ta thêm từ này vào hash table và đếm số lần xuất hiện. Cuối cùng, ta duyệt từng chuỗi trong $\textit{queries}$, tra cứu số lần xuất hiện của chuỗi đó trong hash table và thêm kết quả vào mảng đáp án.

Độ phức tạp thời gian là $O(n + m)$, trong đó $n$ là tổng độ dài của tất cả các chuỗi trong $\textit{chunks}$, còn $m$ là tổng độ dài của tất cả các chuỗi trong $\textit{queries}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countWordOccurrences(self, chunks: list[str], queries: list[str]) -> list[int]:
        s = "".join(chunks)
        n = len(s)
        cnt = defaultdict(int)
        i = 0
        while i < n:
            if s[i] in " -":
                i += 1
                continue
            j = i
            while (
                j < n
                and s[j] != " "
                and (s[j] != "-" or (j + 1 < n and s[j + 1] not in " -"))
            ):
                j += 1
            cnt[s[i:j]] += 1
            i = j
        return [cnt[q] for q in queries]
```

#### Java

```java
class Solution {
    public int[] countWordOccurrences(String[] chunks, String[] queries) {
        StringBuilder sb = new StringBuilder();
        for (String chunk : chunks) {
            sb.append(chunk);
        }
        String s = sb.toString();
        int n = s.length();
        Map<String, Integer> cnt = new HashMap<>();
        int i = 0;
        while (i < n) {
            char c = s.charAt(i);
            if (c == ' ' || c == '-') {
                i++;
                continue;
            }
            int j = i;
            while (j < n) {
                char cj = s.charAt(j);
                if (cj == ' ') {
                    break;
                }
                if (cj == '-') {
                    if (j + 1 < n) {
                        char cnext = s.charAt(j + 1);
                        if (cnext == ' ' || cnext == '-') {
                            break;
                        }
                    } else {
                        break;
                    }
                }
                j++;
            }
            String word = s.substring(i, j);
            cnt.put(word, cnt.getOrDefault(word, 0) + 1);
            i = j;
        }
        int[] ans = new int[queries.length];
        for (int k = 0; k < queries.length; k++) {
            ans[k] = cnt.getOrDefault(queries[k], 0);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> countWordOccurrences(vector<string>& chunks, vector<string>& queries) {
        string s = "";
        for (const string& chunk : chunks) {
            s += chunk;
        }
        int n = s.length();
        unordered_map<string, int> cnt;
        int i = 0;
        while (i < n) {
            if (s[i] == ' ' || s[i] == '-') {
                i++;
                continue;
            }
            int j = i;
            while (j < n && s[j] != ' ' && (s[j] != '-' || (j + 1 < n && s[j + 1] != ' ' && s[j + 1] != '-'))) {
                j++;
            }
            cnt[s.substr(i, j - i)]++;
            i = j;
        }
        vector<int> ans;
        ans.reserve(queries.size());
        for (const string& q : queries) {
            ans.push_back(cnt[q]);
        }
        return ans;
    }
};
```

#### Go

```go
func countWordOccurrences(chunks []string, queries []string) []int {
	s := strings.Join(chunks, "")
	n := len(s)
	cnt := make(map[string]int)
	i := 0
	for i < n {
		if s[i] == ' ' || s[i] == '-' {
			i++
			continue
		}
		j := i
		for j < n && s[j] != ' ' && (s[j] != '-' || (j+1 < n && s[j+1] != ' ' && s[j+1] != '-')) {
			j++
		}
		cnt[s[i:j]]++
		i = j
	}
	ans := make([]int, len(queries))
	for k, q := range queries {
		ans[k] = cnt[q]
	}
	return ans
}
```

#### TypeScript

```ts
function countWordOccurrences(chunks: string[], queries: string[]): number[] {
    const s = chunks.join('');
    const n = s.length;
    const cnt = new Map<string, number>();
    let i = 0;
    while (i < n) {
        if (s[i] === ' ' || s[i] === '-') {
            i++;
            continue;
        }
        let j = i;
        while (
            j < n &&
            s[j] !== ' ' &&
            (s[j] !== '-' || (j + 1 < n && s[j + 1] !== ' ' && s[j + 1] !== '-'))
        ) {
            j++;
        }
        const word = s.substring(i, j);
        cnt.set(word, (cnt.get(word) || 0) + 1);
        i = j;
    }
    return queries.map(q => cnt.get(q) || 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
