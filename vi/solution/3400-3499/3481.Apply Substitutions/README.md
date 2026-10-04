---
comments: true
difficulty: Medium
tags:
    - Depth-First Search
    - Breadth-First Search
    - Graph
    - Topological Sort
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [3481. Apply Substitutions 🔒](https://leetcode.com/problems/apply-substitutions)

[中文文档](/solution/3400-3499/3481.Apply%20Substitutions/README.md)

## Mô tả

<!-- description:start -->

<p data-end="384" data-start="34">Bạn được cung cấp một mapping <code>replacements</code> và một chuỗi <code>text</code> có thể chứa các <strong>placeholder</strong> theo định dạng <code data-end="139" data-start="132">%var%</code>, trong đó mỗi <code>var</code> tương ứng với một key trong mapping <code>replacements</code>. Mỗi giá trị thay thế có thể tự chứa <strong>một hoặc nhiều</strong> <strong>placeholder</strong> như vậy. Mỗi <strong>placeholder</strong> được thay bằng giá trị tương ứng với key thay thế của nó.</p>

<p data-end="353" data-start="34">Trả về chuỗi <code>text</code> sau khi thay thế hoàn toàn, <strong>không</strong> còn chứa bất kỳ <strong>placeholder</strong> nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">replacements = [[&quot;A&quot;,&quot;abc&quot;],[&quot;B&quot;,&quot;def&quot;]], text = &quot;%A%_%B%&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;abc_def&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul data-end="238" data-start="71">
    <li data-end="138" data-start="71">Mapping liên kết <code data-end="101" data-start="96">&quot;A&quot;</code> với <code data-end="114" data-start="107">&quot;abc&quot;</code> và <code data-end="124" data-start="119">&quot;B&quot;</code> với <code data-end="137" data-start="130">&quot;def&quot;</code>.</li>
    <li data-end="203" data-start="139">Thay <code data-end="154" data-start="149">%A%</code> bằng <code data-end="167" data-start="160">&quot;abc&quot;</code> và <code data-end="177" data-start="172">%B%</code> bằng <code data-end="190" data-start="183">&quot;def&quot;</code> trong chuỗi.</li>
    <li data-end="238" data-start="204">Chuỗi cuối cùng trở thành <code data-end="237" data-start="226">&quot;abc_def&quot;</code>.</li>
 </ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">replacements = [[&quot;A&quot;,&quot;bce&quot;],[&quot;B&quot;,&quot;ace&quot;],[&quot;C&quot;,&quot;abc%B%&quot;]], text = &quot;%A%_%B%_%C%&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;bce_ace_abcace&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul data-end="541" data-is-last-node="" data-is-only-node="" data-start="255">
    <li data-end="346" data-start="255">Mapping liên kết <code data-end="285" data-start="280">&quot;A&quot;</code> với <code data-end="298" data-start="291">&quot;bce&quot;</code>, <code data-end="305" data-start="300">&quot;B&quot;</code> với <code data-end="318" data-start="311">&quot;ace&quot;</code> và <code data-end="329" data-start="324">&quot;C&quot;</code> với <code data-end="345" data-start="335">&quot;abc%B%&quot;</code>.</li>
    <li data-end="411" data-start="347">Thay <code data-end="362" data-start="357">%A%</code> bằng <code data-end="375" data-start="368">&quot;bce&quot;</code> và <code data-end="385" data-start="380">%B%</code> bằng <code data-end="398" data-start="391">&quot;ace&quot;</code> trong chuỗi.</li>
    <li data-end="496" data-start="412">Sau đó, với <code data-end="429" data-start="424">%C%</code>, thay <code data-end="447" data-start="442">%B%</code> trong <code data-end="461" data-start="451">&quot;abc%B%&quot;</code> bằng <code data-end="474" data-start="467">&quot;ace&quot;</code> để nhận được <code data-end="495" data-start="485">&quot;abcace&quot;</code>.</li>
    <li data-end="541" data-is-last-node="" data-start="497">Chuỗi cuối cùng trở thành <code data-end="540" data-start="522">&quot;bce_ace_abcace&quot;</code>.</li>
 </ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li data-end="1432" data-start="1398"><code>1 &lt;= replacements.length &lt;= 10</code></li>
    <li data-end="1683" data-start="1433">Mỗi phần tử của <code data-end="1465" data-start="1451">replacements</code> là một danh sách gồm hai phần tử <code data-end="1502" data-start="1488">[key, value]</code>, trong đó:
    <ul data-end="1683" data-start="1513">
        <li data-end="1558" data-start="1513"><code data-end="1520" data-start="1515">key</code> là một chữ cái tiếng Anh viết hoa.</li>
        <li data-end="1683" data-start="1561"><code data-end="1570" data-start="1563">value</code> là một chuỗi không rỗng dài tối đa 8 ký tự, có thể chứa không hoặc nhiều placeholder theo định dạng <code data-end="1682" data-start="1673">%&lt;key&gt;%</code>.</li>
    </ul>
    </li>
    <li data-end="726" data-start="688">Tất cả key thay thế là duy nhất.</li>
    <li data-end="1875" data-start="1723">Chuỗi <code>text</code> được tạo bằng cách nối ngẫu nhiên tất cả placeholder của các key (theo định dạng <code data-end="1808" data-start="1799">%&lt;key&gt;%</code>) trong mapping replacements, ngăn cách bởi dấu gạch dưới.</li>
    <li data-end="1942" data-start="1876"><code>text.length == 4 * replacements.length - 1</code></li>
    <li data-end="2052" data-start="1943">Mọi placeholder trong <code>text</code> hoặc trong bất kỳ giá trị thay thế nào đều tương ứng với một key trong mapping <code>replacements</code>.</li>
    <li data-end="2265" data-start="2205">Không có phụ thuộc chu kỳ giữa các key thay thế.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm + Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Các placeholder $\%key\%$ được mở rộng thành giá trị tương ứng, nhưng bản thân giá trị đó có thể vẫn chứa placeholder. Vì không có chu kỳ và có nhiều nhất $10$ key, dùng đệ quy là đủ.
>
> Thay thế một cấp sẽ để lại các dấu phần trăm lồng nhau. Ta xây dựng một hash map, rồi tìm cặp dấu $\%$ tiếp theo.
>
> $\textit{dfs}$ mở rộng hoàn toàn $d[key]$ trước, nối phần bên trái, rồi tiếp tục với phần bên phải. Mọi placeholder đều được định nghĩa.

<!-- thinking:end -->

Ta dùng một hash table $\textit{d}$ để lưu mapping thay thế, sau đó định nghĩa hàm $\textit{dfs}$ để đệ quy thay thế các placeholder trong chuỗi.

Logic thực thi của hàm $\textit{dfs}$ như sau:

1. Tìm vị trí bắt đầu $i$ của placeholder đầu tiên trong chuỗi $\textit{s}$. Nếu không tìm thấy, trả về $\textit{s}$;
2. Tìm vị trí kết thúc $j$ của placeholder đầu tiên trong chuỗi $\textit{s}$. Nếu không tìm thấy, trả về $\textit{s}$;
3. Trích xuất key của placeholder, sau đó đệ quy thay thế giá trị của placeholder $d[key]$;
4. Trả về chuỗi đã thay thế.

Trong hàm chính, ta gọi hàm $\textit{dfs}$ với chuỗi $\textit{text}$ và trả về kết quả.

Độ phức tạp thời gian là $O(m + n \times L)$, độ phức tạp không gian là $O(m + n \times L)$. Trong đó $m$ là độ dài của mapping thay thế, còn $n$ và $L$ lần lượt là độ dài của chuỗi text và độ dài trung bình của các placeholder.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def applySubstitutions(self, replacements: List[List[str]], text: str) -> str:
        def dfs(s: str) -> str:
            i = s.find("%")
            if i == -1:
                return s
            j = s.find("%", i + 1)
            if j == -1:
                return s
            key = s[i + 1 : j]
            replacement = dfs(d[key])
            return s[:i] + replacement + dfs(s[j + 1 :])

        d = {s: t for s, t in replacements}
        return dfs(text)
```

#### Java

```java
class Solution {
    private final Map<String, String> d = new HashMap<>();

    public String applySubstitutions(List<List<String>> replacements, String text) {
        for (List<String> e : replacements) {
            d.put(e.get(0), e.get(1));
        }
        return dfs(text);
    }

    private String dfs(String s) {
        int i = s.indexOf("%");
        if (i == -1) {
            return s;
        }
        int j = s.indexOf("%", i + 1);
        if (j == -1) {
            return s;
        }
        String key = s.substring(i + 1, j);
        String replacement = dfs(d.getOrDefault(key, ""));
        return s.substring(0, i) + replacement + dfs(s.substring(j + 1));
    }
}
```

#### C++

```cpp
class Solution {
public:
    string applySubstitutions(vector<vector<string>>& replacements, string text) {
        unordered_map<string, string> d;
        for (const auto& e : replacements) {
            d[e[0]] = e[1];
        }
        auto dfs = [&](this auto&& dfs, const string& s) -> string {
            size_t i = s.find('%');
            if (i == string::npos) {
                return s;
            }
            size_t j = s.find('%', i + 1);
            if (j == string::npos) {
                return s;
            }
            string key = s.substr(i + 1, j - i - 1);
            string replacement = dfs(d[key]);
            return s.substr(0, i) + replacement + dfs(s.substr(j + 1));
        };
        return dfs(text);
    }
};
```

#### Go

```go
func applySubstitutions(replacements [][]string, text string) string {
    d := make(map[string]string)
    for _, e := range replacements {
        d[e[0]] = e[1]
    }
    var dfs func(string) string
    dfs = func(s string) string {
        i := strings.Index(s, "%")
        if i == -1 {
            return s
        }
        j := strings.Index(s[i+1:], "%")
        if j == -1 {
            return s
        }
        j += i + 1
        key := s[i+1 : j]
        replacement := dfs(d[key])
        return s[:i] + replacement + dfs(s[j+1:])
    }

    return dfs(text)
}
```

#### TypeScript

```ts
function applySubstitutions(replacements: string[][], text: string): string {
    const d: Record<string, string> = {};
    for (const [key, value] of replacements) {
        d[key] = value;
    }

    const dfs = (s: string): string => {
        const i = s.indexOf('%');
        if (i === -1) {
            return s;
        }
        const j = s.indexOf('%', i + 1);
        if (j === -1) {
            return s;
        }
        const key = s.slice(i + 1, j);
        const replacement = dfs(d[key] ?? '');
        return s.slice(0, i) + replacement + dfs(s.slice(j + 1));
    };

    return dfs(text);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
