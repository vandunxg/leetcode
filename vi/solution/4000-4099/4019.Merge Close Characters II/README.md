---
comments: true
difficulty: Medium
tags:
    - Hash Table
    - String
---

<!-- problem:start -->

# [4019. Merge Close Characters II 🔒](https://leetcode.com/problems/merge-close-characters-ii)

[中文文档](/solution/4000-4099/4019.Merge%20Close%20Characters%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường và một số nguyên <code>k</code>.</p>

<p>Hai ký tự giống nhau <code>s[i]</code> và <code>s[j]</code>, với <code>0 &lt;= i &lt; j &lt; s.length</code>, được xem là <strong>gần nhau</strong> nếu <code>j - i &lt;= k</code>. Tất cả chỉ số đều tham chiếu đến chuỗi <strong>hiện tại</strong>.</p>

<p>Thực hiện lặp lại thao tác sau cho đến khi không còn cặp gần nhau nào:</p>

<ul>
	<li>Trong tất cả các cặp gần nhau <code>(i, j)</code>, chọn cặp có <code>i</code> nhỏ nhất. Nếu có nhiều cặp có cùng <code>i</code>, chọn cặp có <code>j</code> nhỏ nhất.</li>
	<li>Gộp ký tự bên phải vào ký tự bên trái bằng cách xóa <code>s[j]</code> khỏi <code>s</code>. Ký tự <code>s[i]</code> được giữ nguyên, các ký tự còn lại được đánh lại chỉ số.</li>
</ul>

<p>Trả về chuỗi thu được sau khi thực hiện tất cả các phép gộp có thể.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abca&quot;, k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;abc&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các ký tự <code>&#39;a&#39;</code> ở chỉ số 0 và 3 gần nhau vì <code>3 - 0 = 3 &lt;= k</code>.</li>
	<li>Xóa <code>&#39;a&#39;</code> bên phải, thu được <code>s = &quot;abc&quot;</code>.</li>
	<li>Không còn cặp gần nhau nào, nên không thực hiện thêm phép gộp nào.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;aabca&quot;, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;abca&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các ký tự <code>&#39;a&#39;</code> ở chỉ số 0 và 1 gần nhau vì <code>1 - 0 = 1 &lt;= k</code>.</li>
	<li>Xóa <code>&#39;a&#39;</code> bên phải, thu được <code>s = &quot;abca&quot;</code>.</li>
	<li>Các ký tự <code>&#39;a&#39;</code> còn lại ở chỉ số 0 và 3. Vì <code>3 - 0 = 3 &gt; k</code>, không thể thực hiện thêm phép gộp nào.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;yybyzybz&quot;, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;ybzybz&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các ký tự <code>&#39;y&#39;</code> ở chỉ số 0 và 1 gần nhau vì <code>1 - 0 = 1 &lt;= k</code>. Cặp này có chỉ số trái nhỏ nhất trong tất cả các cặp gần nhau.</li>
	<li>Xóa <code>&#39;y&#39;</code> bên phải, thu được <code>s = &quot;ybyzybz&quot;</code>.</li>
	<li>Các ký tự <code>&#39;y&#39;</code> ở chỉ số 0 và 2 lúc này gần nhau vì <code>2 - 0 = 2 &lt;= k</code>.</li>
	<li>Xóa <code>&#39;y&#39;</code> bên phải, thu được <code>s = &quot;ybzybz&quot;</code>.</li>
	<li>Không còn cặp gần nhau nào, nên không thực hiện thêm phép gộp nào.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 5 * 10<sup>5</sup></code></li>
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
> Mỗi phép gộp luôn xóa ký tự bên phải, nên kết quả là một subsequence từ trái sang phải, trong đó hai chữ cái giống nhau cách nhau hơn $k$ vị trí. Nếu liên tục quét và gộp, mỗi lượt có thể chỉ xóa được một ký tự.
>
> Khi quét từ trái sang phải, nếu chữ cái hiện tại cách lần xuất hiện gần nhất được giữ lại không quá $k$ vị trí, thì một phép gộp về sau sẽ xóa nó; ngược lại, ta nên giữ lại chữ cái đó.
>
> Dùng một hash table để lưu chỉ số cuối cùng của mỗi chữ cái trong đáp án, sau đó áp dụng quy tắc trên trong một lượt là có thể tạo ra chuỗi cuối cùng.

<!-- thinking:end -->

Ta dùng một hash table $\textit{last}$ để lưu lần xuất hiện cuối cùng của mỗi ký tự trong chuỗi đáp án. Ta duyệt từng ký tự trong $s$ từ trái sang phải. Gọi $\textit{cur}$ là độ dài hiện tại của đáp án. Nếu ký tự đã xuất hiện trước đó và hiệu giữa $\textit{cur}$ với vị trí xuất hiện cuối cùng của nó không vượt quá $k$, ta bỏ qua ký tự này; nếu không, ta thêm ký tự vào đáp án và cập nhật vị trí của nó trong hash table.

Mỗi phép gộp luôn xóa ký tự bên phải, nên các vị trí trong đáp án chính là các chỉ số trong chuỗi hiện tại. Quy trình greedy này tương đương với việc thực hiện lặp lại các phép gộp theo yêu cầu.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(|\Sigma|)$, trong đó $n$ là độ dài chuỗi và $|\Sigma|$ là kích thước của tập ký tự. Trong bài toán này, tập ký tự gồm các chữ cái tiếng Anh viết thường, nên $|\Sigma|$ là một hằng số.

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
        return ''.join(ans)
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
