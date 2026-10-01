---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [249. Group Shifted Strings 🔒](https://leetcode.com/problems/group-shifted-strings)

[中文文档](/solution/0200-0299/0249.Group%20Shifted%20Strings/README.md)

## Mô tả

<!-- description:start -->

<p>Thực hiện các phép dịch sau trên một chuỗi:</p>

<ul>
	<li><strong>Dịch phải</strong>: Thay mỗi chữ cái bằng chữ cái <strong>liền sau</strong> trong bảng chữ cái tiếng Anh, trong đó &#39;z&#39; được thay bằng &#39;a&#39;. Ví dụ, có thể dịch phải <code>&quot;abc&quot;</code> thành <code>&quot;bcd&quot; </code> hoặc <code>&quot;xyz&quot;</code> thành <code>&quot;yza&quot;</code>.</li>
	<li><strong>Dịch trái</strong>: Thay mỗi chữ cái bằng chữ cái <strong>liền trước</strong> trong bảng chữ cái tiếng Anh, trong đó &#39;a&#39; được thay bằng &#39;z&#39;. Ví dụ, có thể dịch trái <code>&quot;bcd&quot;</code> thành <code>&quot;abc&quot;<font face="Times New Roman"> hoặc </font></code><code>&quot;yza&quot;</code> thành <code>&quot;xyz&quot;</code>.</li>
</ul>

<p>Ta có thể tiếp tục dịch chuỗi theo cả hai hướng để tạo thành một <strong>chuỗi dịch</strong> <strong>vô hạn</strong>.</p>

<ul>
	<li>Ví dụ, dịch <code>&quot;abc&quot;</code> để tạo thành chuỗi: <code>... &lt;-&gt; &quot;abc&quot; &lt;-&gt; &quot;bcd&quot; &lt;-&gt; ... &lt;-&gt; &quot;xyz&quot; &lt;-&gt; &quot;yza&quot; &lt;-&gt; ...</code>.<code> &lt;-&gt; &quot;zab&quot; &lt;-&gt; &quot;abc&quot; &lt;-&gt; ...</code></li>
</ul>

<p>Cho mảng chuỗi <code>strings</code>, hãy nhóm tất cả <code>strings[i]</code> thuộc cùng một chuỗi dịch. Bạn có thể trả về kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">strings = [&quot;abc&quot;,&quot;bcd&quot;,&quot;acef&quot;,&quot;xyz&quot;,&quot;az&quot;,&quot;ba&quot;,&quot;a&quot;,&quot;z&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[&quot;acef&quot;],[&quot;a&quot;,&quot;z&quot;],[&quot;abc&quot;,&quot;bcd&quot;,&quot;xyz&quot;],[&quot;az&quot;,&quot;ba&quot;]]</span></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">strings = [&quot;a&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[&quot;a&quot;]]</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= strings.length &lt;= 200</code></li>
	<li><code>1 &lt;= strings[i].length &lt;= 50</code></li>
	<li><code>strings[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Các chuỗi thuộc cùng một nhóm dịch có cùng dãy khoảng cách giữa các chữ cái liên tiếp. Chuẩn hóa mỗi chuỗi bằng cách dịch để ký tự đầu tiên thành $a$ sẽ tạo ra khóa dùng để nhóm.
>
> Dùng hash map để gom các chuỗi theo dạng chuẩn này; các nhóm trong hash map chính là kết quả.

<!-- thinking:end -->

Ta dùng hash table $g$ để lưu mỗi chuỗi sau khi dịch sao cho ký tự đầu tiên là '`a`'. Cụ thể, $g[t]$ biểu diễn tập hợp các chuỗi có thể dịch thành $t$.

Ta duyệt từng chuỗi. Với mỗi chuỗi, ta tính chuỗi đã dịch $t$, rồi thêm chuỗi đó vào $g[t]$.

Cuối cùng, ta lấy tất cả các giá trị trong $g$ làm kết quả.

Độ phức tạp thời gian là $O(L)$ và độ phức tạp không gian là $O(L)$, trong đó $L$ là tổng độ dài của tất cả các chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def groupStrings(self, strings: List[str]) -> List[List[str]]:
        g = defaultdict(list)
        for s in strings:
            diff = ord(s[0]) - ord("a")
            t = []
            for c in s:
                c = ord(c) - diff
                if c < ord("a"):
                    c += 26
                t.append(chr(c))
            g["".join(t)].append(s)
        return list(g.values())
```

#### Java

```java
class Solution {
    public List<List<String>> groupStrings(String[] strings) {
        Map<String, List<String>> g = new HashMap<>();
        for (var s : strings) {
            char[] t = s.toCharArray();
            int diff = t[0] - 'a';
            for (int i = 0; i < t.length; ++i) {
                t[i] = (char) (t[i] - diff);
                if (t[i] < 'a') {
                    t[i] += 26;
                }
            }
            g.computeIfAbsent(new String(t), k -> new ArrayList<>()).add(s);
        }
        return new ArrayList<>(g.values());
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<string>> groupStrings(vector<string>& strings) {
        unordered_map<string, vector<string>> g;
        for (auto& s : strings) {
            string t;
            int diff = s[0] - 'a';
            for (int i = 0; i < s.size(); ++i) {
                char c = s[i] - diff;
                if (c < 'a') {
                    c += 26;
                }
                t.push_back(c);
            }
            g[t].emplace_back(s);
        }
        vector<vector<string>> ans;
        for (auto& p : g) {
            ans.emplace_back(move(p.second));
        }
        return ans;
    }
};
```

#### Go

```go
func groupStrings(strings []string) [][]string {
	g := make(map[string][]string)
	for _, s := range strings {
		t := []byte(s)
		diff := t[0] - 'a'
		for i := range t {
			t[i] -= diff
			if t[i] < 'a' {
				t[i] += 26
			}
		}
		g[string(t)] = append(g[string(t)], s)
	}
	ans := make([][]string, 0, len(g))
	for _, v := range g {
		ans = append(ans, v)
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
