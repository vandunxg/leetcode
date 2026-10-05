---
comments: true
difficulty: Medium
rating: 1471
source: Biweekly Contest 177 Q2
tags:
    - Hash Table
    - String
---

<!-- problem:start -->

# [3853. Merge Close Characters](https://leetcode.com/problems/merge-close-characters)

[中文文档](/solution/3800-3899/3853.Merge%20Close%20Characters/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường và một số nguyên <code>k</code>.</p>

<p>Hai ký tự <strong>bằng nhau</strong> trong chuỗi <code>s</code> <strong>hiện tại</strong> được gọi là <strong>gần nhau</strong> nếu khoảng cách giữa các chỉ số của chúng <strong>không vượt quá</strong> <code>k</code>.</p>

<p>Khi hai ký tự <strong>gần nhau</strong>, ký tự bên phải được gộp vào ký tự bên trái. Các phép gộp diễn ra <strong>từng lần một</strong>, và sau mỗi phép gộp, chuỗi được cập nhật cho đến khi không thể thực hiện thêm phép gộp nào.</p>

<p>Hãy trả về chuỗi thu được sau khi thực hiện tất cả các phép gộp có thể.</p>

<p><strong>Lưu ý</strong>: Nếu có nhiều phép gộp có thể thực hiện, luôn gộp cặp có chỉ số bên trái <strong>nhỏ nhất</strong>. Nếu có nhiều cặp cùng chỉ số bên trái nhỏ nhất, chọn cặp có chỉ số bên phải <strong>nhỏ nhất</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abca&quot;, k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;abc&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><strong>​​​​​​​</strong>Các ký tự <code>&#39;a&#39;</code> ở các chỉ số <code>i = 0</code> và <code>i = 3</code> gần nhau vì <code>3 - 0 = 3 &lt;= k</code>.</li>
	<li>Gộp chúng vào ký tự <code>&#39;a&#39;</code> bên trái, khi đó <code>s = &quot;abc&quot;</code>.</li>
	<li>Không còn các ký tự bằng nhau nào ở gần nhau, nên không thực hiện thêm phép gộp nào.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;aabca&quot;, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;abca&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các ký tự <code>&#39;a&#39;</code> ở các chỉ số <code>i = 0</code> và <code>i = 1</code> gần nhau vì <code>1 - 0 = 1 &lt;= k</code>.</li>
	<li>Gộp chúng vào ký tự <code>&#39;a&#39;</code> bên trái, khi đó <code>s = &quot;abca&quot;</code>.</li>
	<li>Bây giờ, các ký tự <code>&#39;a&#39;</code> còn lại ở các chỉ số <code>i = 0</code> và <code>i = 3</code> không gần nhau vì <code>k &lt; 3</code>, nên không thực hiện thêm phép gộp nào.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;yybyzybz&quot;, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;ybzybz&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các ký tự <code>&#39;y&#39;</code> ở các chỉ số <code>i = 0</code> và <code>i = 1</code> gần nhau vì <code>1 - 0 = 1 &lt;= k</code>.</li>
	<li>Gộp chúng vào ký tự <code>&#39;y&#39;</code> bên trái, khi đó <code>s = &quot;ybyzybz&quot;</code>.</li>
	<li>Bây giờ, các ký tự <code>&#39;y&#39;</code> ở các chỉ số <code>i = 0</code> và <code>i = 2</code> gần nhau vì <code>2 - 0 = 2 &lt;= k</code>.</li>
	<li>Gộp chúng vào ký tự <code>&#39;y&#39;</code> bên trái, khi đó <code>s = &quot;ybzybz&quot;</code>.</li>
	<li>Không còn các ký tự bằng nhau nào ở gần nhau, nên không thực hiện thêm phép gộp nào.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>1 &lt;= k &lt;= s.length</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Các ký tự bằng nhau cách nhau không quá $k$ trong chuỗi hiện tại sẽ được gộp từ phải vào trái, luôn chọn cặp ngoài cùng bên trái. Dù $n \le 100$, việc viết lại chuỗi sau mỗi lần gộp khá rắc rối.
>
> Gộp có nghĩa là: một ký tự mới sẽ được hấp thụ nếu nó nằm trong phạm vi $k$ so với chỉ số cuối cùng của ký tự đó trong đáp án.
>
> Một map lưu chỉ số gần nhất trong đáp án của mỗi chữ cái. Ta duyệt đầu vào: bỏ qua nếu có thể gộp, nếu không thì thêm vào và cập nhật.
>
> Việc luôn ưu tiên cặp ngoài cùng bên trái tương ứng với cách xây dựng từ trái sang phải này.

<!-- thinking:end -->

Ta dùng một hash table $\textit{last}$ để ghi lại vị trí xuất hiện cuối cùng của mỗi ký tự. Ta duyệt qua từng ký tự trong chuỗi. Nếu ký tự hiện tại đã xuất hiện trước đó và chênh lệch giữa chỉ số hiện tại với chỉ số xuất hiện cuối cùng của nó không vượt quá $k$, ta bỏ qua ký tự này; nếu không, ta thêm ký tự vào đáp án và cập nhật vị trí của nó trong hash table.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(|\Sigma|)$, trong đó $n$ là độ dài chuỗi và $|\Sigma|$ là kích thước của tập ký tự. Trong bài toán này, tập ký tự gồm các chữ cái tiếng Anh viết thường, nên $|\Sigma|$ là một hằng số.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def mergeCharacters(self, s: str, k: int) -> str:
        last = {}
        ans = []
        for c in s:
            cur = len(ans)
            if c in last and cur - last[c] <= k:
                continue
            ans.append(c)
            last[c] = cur
        return "".join(ans)
```

#### Java

```java
class Solution {
    public String mergeCharacters(String s, int k) {
        Map<Character, Integer> last = new HashMap<>();
        StringBuilder ans = new StringBuilder();
        for (char c : s.toCharArray()) {
            int cur = ans.length();
            if (last.containsKey(c) && cur - last.get(c) <= k) {
                continue;
            }
            ans.append(c);
            last.put(c, cur);
        }
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string mergeCharacters(string s, int k) {
        unordered_map<char, int> last;
        string ans;
        for (char c : s) {
            int cur = ans.size();
            if (last.count(c) && cur - last[c] <= k) {
                continue;
            }
            ans += c;
            last[c] = cur;
        }
        return ans;
    }
};
```

#### Go

```go
func mergeCharacters(s string, k int) string {
	last := make(map[byte]int)
	var ans []byte
	for i := 0; i < len(s); i++ {
		c := s[i]
		cur := len(ans)
		if lastIdx, ok := last[c]; ok && cur-lastIdx <= k {
			continue
		}
		ans = append(ans, c)
		last[c] = cur
	}
	return string(ans)
}
```

#### TypeScript

```ts
function mergeCharacters(s: string, k: number): string {
    const last = new Map<string, number>();
    const ans: string[] = [];
    for (const c of s) {
        const cur = ans.length;
        if (last.has(c) && cur - last.get(c)! <= k) {
            continue;
        }
        ans.push(c);
        last.set(c, cur);
    }
    return ans.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
