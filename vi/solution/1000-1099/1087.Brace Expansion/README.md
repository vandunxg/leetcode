---
comments: true
difficulty: Medium
rating: 1480
source: Biweekly Contest 2 Q3
tags:
    - Stack
    - Breadth-First Search
    - String
    - Backtracking
    - Sorting
---

<!-- problem:start -->

# [1087. Brace Expansion 🔒](https://leetcode.com/problems/brace-expansion)

[中文文档](/solution/1000-1099/1087.Brace%20Expansion/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho chuỗi <code>s</code> biểu diễn một danh sách từ. Mỗi chữ cái trong từ có một hoặc nhiều lựa chọn.</p>

<ul>
	<li>Nếu chỉ có một lựa chọn, chữ cái được giữ nguyên như vậy.</li>
	<li>Nếu có nhiều lựa chọn, các lựa chọn được đặt trong dấu ngoặc nhọn. Ví dụ, <code>&quot;{a,b,c}&quot;</code> biểu diễn các lựa chọn <code>[&quot;a&quot;, &quot;b&quot;, &quot;c&quot;]</code>.</li>
</ul>

<p>Ví dụ, nếu <code>s = &quot;a{b,c}&quot;</code>, ký tự đầu tiên luôn là <code>&#39;a&#39;</code>, còn ký tự thứ hai có thể là <code>&#39;b&#39;</code> hoặc <code>&#39;c&#39;</code>. Danh sách từ ban đầu là <code>[&quot;ab&quot;, &quot;ac&quot;]</code>.</p>

<p>Trả về tất cả các từ có thể tạo theo cách này, được <strong>sắp xếp</strong> theo thứ tự từ điển.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Input:</strong> s = "{a,b}c{d,e}f"
<strong>Output:</strong> ["acdf","acef","bcdf","bcef"]
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Input:</strong> s = "abcd"
<strong>Output:</strong> ["abcd"]
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 50</code></li>
	<li><code>s</code> chỉ gồm dấu ngoặc nhọn <code>&#39;{}&#39;</code>, dấu phẩy&nbsp;<code>&#39;,&#39;</code> và các chữ cái tiếng Anh viết thường.</li>
	<li>Đảm bảo <code>s</code> là đầu vào hợp lệ.</li>
	<li>Các dấu ngoặc nhọn không lồng nhau.</li>
	<li>Tất cả ký tự nằm trong cùng một cặp ngoặc nhọn mở và đóng đều khác nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Biểu thức là sự nối tiếp của các đoạn chữ cái và các lựa chọn trong ngoặc nhọn ở một cấp. Khai triển biểu thức tương đương với tích Descartes; chỉ cần backtracking rồi sắp xếp kết quả.
>
> `convert` tách các phần trong ngoặc nhọn và các tiền tố thông thường thành những danh sách lựa chọn. `dfs` chọn một token từ mỗi danh sách.
>
> Các kết quả ở lá được thu thập rồi sắp xếp theo thứ tự từ điển.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def expand(self, s: str) -> List[str]:
        def convert(s):
            if not s:
                return
            if s[0] == '{':
                j = s.find('}')
                items.append(s[1:j].split(','))
                convert(s[j + 1 :])
            else:
                j = s.find('{')
                if j != -1:
                    items.append(s[:j].split(','))
                    convert(s[j:])
                else:
                    items.append(s.split(','))

        def dfs(i, t):
            if i == len(items):
                ans.append(''.join(t))
                return
            for c in items[i]:
                t.append(c)
                dfs(i + 1, t)
                t.pop()

        items = []
        convert(s)
        ans = []
        dfs(0, [])
        ans.sort()
        return ans
```

#### Java

```java
class Solution {
    private List<String> ans;
    private List<String[]> items;

    public String[] expand(String s) {
        ans = new ArrayList<>();
        items = new ArrayList<>();
        convert(s);
        dfs(0, new ArrayList<>());
        Collections.sort(ans);
        return ans.toArray(new String[0]);
    }

    private void convert(String s) {
        if ("".equals(s)) {
            return;
        }
        if (s.charAt(0) == '{') {
            int j = s.indexOf("}");
            items.add(s.substring(1, j).split(","));
            convert(s.substring(j + 1));
        } else {
            int j = s.indexOf("{");
            if (j != -1) {
                items.add(s.substring(0, j).split(","));
                convert(s.substring(j));
            } else {
                items.add(s.split(","));
            }
        }
    }

    private void dfs(int i, List<String> t) {
        if (i == items.size()) {
            ans.add(String.join("", t));
            return;
        }
        for (String c : items.get(i)) {
            t.add(c);
            dfs(i + 1, t);
            t.remove(t.size() - 1);
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
