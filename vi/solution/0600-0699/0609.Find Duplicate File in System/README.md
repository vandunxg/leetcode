---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [609. Find Duplicate File in System](https://leetcode.com/problems/find-duplicate-file-in-system)

[中文文档](/solution/0600-0699/0609.Find%20Duplicate%20File%20in%20System/README.md)

## Mô tả

<!-- description:start -->

<p>Cho danh sách <code>paths</code> chứa thông tin thư mục, bao gồm đường dẫn thư mục và tất cả file cùng nội dung trong thư mục đó. Hãy trả về <em>đường dẫn của tất cả file trùng lặp trong hệ thống</em>. Có thể trả lời theo <strong>thứ tự bất kỳ</strong>.</p>

<p>Một nhóm file trùng lặp gồm ít nhất hai file có cùng nội dung.</p>

<p>Mỗi chuỗi thông tin thư mục trong danh sách đầu vào có định dạng sau:</p>

<ul>
	<li><code>&quot;root/d1/d2/.../dm f1.txt(f1_content) f2.txt(f2_content) ... fn.txt(fn_content)&quot;</code></li>
</ul>

<p>Chuỗi này biểu thị thư mục &quot;<code>root/d1/d2/.../dm</code>&quot; có <code>n</code> file <code>(f1.txt, f2.txt ... fn.txt)</code> với nội dung tương ứng <code>(f1_content, f2_content ... fn_content)</code>. Lưu ý <code>n &gt;= 1</code> và <code>m &gt;= 0</code>. Nếu <code>m = 0</code>, thư mục đó chính là thư mục gốc.</p>

<p>Đầu ra là danh sách các nhóm đường dẫn file trùng lặp. Mỗi nhóm chứa đường dẫn của tất cả file có cùng nội dung. Đường dẫn file là một chuỗi có định dạng sau:</p>

<ul>
	<li><code>&quot;directory_path/file_name.txt&quot;</code></li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> paths = ["root/a 1.txt(abcd) 2.txt(efgh)","root/c 3.txt(abcd)","root/c/d 4.txt(efgh)","root 4.txt(efgh)"]
<strong>Đầu ra:</strong> [["root/a/2.txt","root/c/d/4.txt","root/4.txt"],["root/a/1.txt","root/c/3.txt"]]
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> paths = ["root/a 1.txt(abcd) 2.txt(efgh)","root/c 3.txt(abcd)","root/c/d 4.txt(efgh)"]
<strong>Đầu ra:</strong> [["root/a/2.txt","root/c/d/4.txt"],["root/a/1.txt","root/c/3.txt"]]
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= paths.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= paths[i].length &lt;= 3000</code></li>
	<li><code>1 &lt;= sum(paths[i].length) &lt;= 5 * 10<sup>5</sup></code></li>
	<li><code>paths[i]</code> chỉ gồm chữ cái tiếng Anh, chữ số, <code>&#39;/&#39;</code>, <code>&#39;.&#39;</code>, <code>&#39;(&#39;</code>, <code>&#39;)&#39;</code> và <code>&#39; &#39;</code>.</li>
	<li>Có thể giả sử không có file hoặc thư mục nào trùng tên trong cùng một thư mục.</li>
	<li>Có thể giả sử mỗi chuỗi thông tin thư mục đã cho tương ứng với một thư mục riêng biệt. Đường dẫn thư mục và thông tin file được ngăn cách bằng một dấu cách.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong></p>

<ul>
	<li>Giả sử bạn được cung cấp một hệ thống file thực, bạn sẽ tìm kiếm file như thế nào: DFS hay BFS?</li>
	<li>Nếu nội dung file rất lớn (cỡ GB), bạn sẽ điều chỉnh lời giải như thế nào?</li>
	<li>Nếu mỗi lần chỉ có thể đọc 1kb của file, bạn sẽ điều chỉnh lời giải như thế nào?</li>
	<li>Độ phức tạp thời gian của lời giải đã điều chỉnh là bao nhiêu? Phần nào tốn thời gian và bộ nhớ nhất? Có thể tối ưu như thế nào?</li>
	<li>Làm thế nào để đảm bảo các file trùng lặp tìm được không phải là kết quả dương tính giả?</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> File trùng lặp được xác định dựa trên nội dung giống nhau, nhưng mỗi chuỗi path gộp thư mục và `name(content)` vào cùng một chuỗi. So sánh từng cặp sẽ không hiệu quả khi dữ liệu lớn.
>
> Tách đường dẫn thư mục khỏi các token file, rồi tách mỗi token tại dấu ngoặc. Nhóm các path theo nội dung và giữ lại các nhóm có hơn $1$ phần tử.

<!-- thinking:end -->

Ta tạo hash table $d$, trong đó key là nội dung file, còn value là danh sách path của các file có cùng nội dung.

Tiếp theo, duyệt $\textit{paths}$. Với mỗi path, tách thành đường dẫn thư mục và thông tin file. Với từng file, lấy tên và nội dung, rồi thêm đường dẫn file vào danh sách tương ứng trong hash table $d$.

Cuối cùng, trả về các value trong hash table $d$ có nhiều hơn một path.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của $\textit{paths}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findDuplicate(self, paths: List[str]) -> List[List[str]]:
        d = defaultdict(list)
        for p in paths:
            ps = p.split()
            for f in ps[1:]:
                i = f.find('(')
                name, content = f[:i], f[i + 1 : -1]
                d[content].append(ps[0] + '/' + name)
        return [v for v in d.values() if len(v) > 1]
```

#### Java

```java
class Solution {
    public List<List<String>> findDuplicate(String[] paths) {
        Map<String, List<String>> d = new HashMap<>();
        for (String p : paths) {
            String[] ps = p.split(" ");
            for (int i = 1; i < ps.length; ++i) {
                int j = ps[i].indexOf('(');
                String content = ps[i].substring(j + 1, ps[i].length() - 1);
                String name = ps[0] + '/' + ps[i].substring(0, j);
                d.computeIfAbsent(content, k -> new ArrayList<>()).add(name);
            }
        }
        List<List<String>> ans = new ArrayList<>();
        for (var e : d.values()) {
            if (e.size() > 1) {
                ans.add(e);
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
    vector<vector<string>> findDuplicate(vector<string>& paths) {
        unordered_map<string, vector<string>> d;
        for (auto& p : paths) {
            auto ps = split(p, ' ');
            for (int i = 1; i < ps.size(); ++i) {
                int j = ps[i].find('(');
                auto content = ps[i].substr(j + 1, ps[i].size() - j - 2);
                auto name = ps[0] + '/' + ps[i].substr(0, j);
                d[content].push_back(name);
            }
        }
        vector<vector<string>> ans;
        for (auto& [_, e] : d) {
            if (e.size() > 1) {
                ans.push_back(e);
            }
        }
        return ans;
    }

    vector<string> split(string& s, char c) {
        vector<string> res;
        stringstream ss(s);
        string t;
        while (getline(ss, t, c)) {
            res.push_back(t);
        }
        return res;
    }
};
```

#### Go

```go
func findDuplicate(paths []string) [][]string {
	d := map[string][]string{}
	for _, p := range paths {
		ps := strings.Split(p, " ")
		for i := 1; i < len(ps); i++ {
			j := strings.IndexByte(ps[i], '(')
			content := ps[i][j+1 : len(ps[i])-1]
			name := ps[0] + "/" + ps[i][:j]
			d[content] = append(d[content], name)
		}
	}
	ans := [][]string{}
	for _, e := range d {
		if len(e) > 1 {
			ans = append(ans, e)
		}
	}
	return ans
}
```

#### TypeScript

```ts
function findDuplicate(paths: string[]): string[][] {
    const d = new Map<string, string[]>();
    for (const p of paths) {
        const [root, ...fs] = p.split(' ');
        for (const f of fs) {
            const [name, content] = f.split(/\(|\)/g).filter(Boolean);
            const t = d.get(content) ?? [];
            t.push(root + '/' + name);
            d.set(content, t);
        }
    }
    return [...d.values()].filter(e => e.length > 1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
