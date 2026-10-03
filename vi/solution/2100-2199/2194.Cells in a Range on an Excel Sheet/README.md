---
comments: true
difficulty: Easy
rating: 1253
source: Weekly Contest 283 Q1
tags:
    - String
---

<!-- problem:start -->

# [2194. Cells in a Range on an Excel Sheet](https://leetcode.com/problems/cells-in-a-range-on-an-excel-sheet)

[中文文档](/solution/2100-2199/2194.Cells%20in%20a%20Range%20on%20an%20Excel%20Sheet/README.md)

## Mô tả

<!-- description:start -->

<p>Một ô <code>(r, c)</code> trong trang tính Excel được biểu diễn dưới dạng chuỗi <code>&quot;&lt;col&gt;&lt;row&gt;&quot;</code>, trong đó:</p>

<ul>
	<li><code>&lt;col&gt;</code> biểu thị số cột <code>c</code> của ô. Cột này được biểu diễn bằng <strong>các chữ cái</strong>.

    <ul>
    <li>Ví dụ, cột <code>1<sup>st</sup></code> được biểu diễn bằng <code>&#39;A&#39;</code>, cột <code>2<sup>nd</sup></code> bằng <code>&#39;B&#39;</code>, cột <code>3<sup>rd</sup></code> bằng <code>&#39;C&#39;</code>, và tiếp tục như vậy.</li>
    </ul>
    </li>
        <li><code>&lt;row&gt;</code> là số hàng <code>r</code> của ô. Hàng <code>r<sup>th</sup></code> được biểu diễn bằng <strong>số nguyên</strong> <code>r</code>.</li>

</ul>

<p>Bạn được cho một chuỗi <code>s</code>&nbsp;theo định dạng <code>&quot;&lt;col1&gt;&lt;row1&gt;:&lt;col2&gt;&lt;row2&gt;&quot;</code>, trong đó <code>&lt;col1&gt;</code> biểu thị cột <code>c1</code>, <code>&lt;row1&gt;</code> biểu thị hàng <code>r1</code>, <code>&lt;col2&gt;</code> biểu thị cột <code>c2</code> và <code>&lt;row2&gt;</code> biểu thị hàng <code>r2</code>, sao cho <code>r1 &lt;= r2</code> và <code>c1 &lt;= c2</code>.</p>

<p>Trả về <em><strong>danh sách các ô</strong></em> <code>(x, y)</code> <em>thỏa mãn</em> <code>r1 &lt;= x &lt;= r2</code> <em>và</em> <code>c1 &lt;= y &lt;= c2</code>. Các ô phải được biểu diễn dưới dạng <strong>chuỗi</strong> theo định dạng đã nêu ở trên và được sắp xếp theo thứ tự <strong>không giảm</strong>, trước tiên theo cột rồi đến hàng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2194.Cells%20in%20a%20Range%20on%20an%20Excel%20Sheet/images/ex1drawio.png" style="width: 250px; height: 160px;" />
<pre>
<strong>Đầu vào:</strong> s = &quot;K1:L2&quot;
<strong>Đầu ra:</strong> [&quot;K1&quot;,&quot;K2&quot;,&quot;L1&quot;,&quot;L2&quot;]
<strong>Giải thích:</strong>
Sơ đồ trên cho thấy các ô cần có trong danh sách.
Các mũi tên màu đỏ biểu thị thứ tự cần trình bày các ô.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2194.Cells%20in%20a%20Range%20on%20an%20Excel%20Sheet/images/exam2drawio.png" style="width: 500px; height: 50px;" />
<pre>
<strong>Đầu vào:</strong> s = &quot;A1:F1&quot;
<strong>Đầu ra:</strong> [&quot;A1&quot;,&quot;B1&quot;,&quot;C1&quot;,&quot;D1&quot;,&quot;E1&quot;,&quot;F1&quot;]
<strong>Giải thích:</strong>
Sơ đồ trên cho thấy các ô cần có trong danh sách.
Mũi tên màu đỏ biểu thị thứ tự cần trình bày các ô.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>s.length == 5</code></li>
	<li><code>&#39;A&#39; &lt;= s[0] &lt;= s[3] &lt;= &#39;Z&#39;</code></li>
	<li><code>&#39;1&#39; &lt;= s[1] &lt;= s[4] &lt;= &#39;9&#39;</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết hoa, chữ số và <code>&#39;:&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Một phạm vi như `A1:F2` cần liệt kê các ô theo thứ tự cột trước, rồi đến hàng. Cả hai khoảng đều rất nhỏ, nên chỉ cần hai vòng lặp lồng nhau.
>
> Duyệt các ký tự cột ở vòng lặp ngoài và các số hàng ở vòng lặp trong, rồi nối chúng để tạo thành từng ô.
>
> Cột bắt đầu và cột kết thúc lần lượt là chữ cái đầu tiên và cuối cùng của $s$; các hàng là hai chữ số.

<!-- thinking:end -->

Ta duyệt trực tiếp tất cả các ô trong phạm vi và thêm chúng vào mảng kết quả.

Độ phức tạp thời gian là $O(m \times n)$ và độ phức tạp không gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là phạm vi của các hàng và cột.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def cellsInRange(self, s: str) -> List[str]:
        return [
            chr(i) + str(j)
            for i in range(ord(s[0]), ord(s[-2]) + 1)
            for j in range(int(s[1]), int(s[-1]) + 1)
        ]
```

#### Java

```java
class Solution {
    public List<String> cellsInRange(String s) {
        List<String> ans = new ArrayList<>();
        for (char i = s.charAt(0); i <= s.charAt(3); ++i) {
            for (char j = s.charAt(1); j <= s.charAt(4); ++j) {
                ans.add(i + "" + j);
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
    vector<string> cellsInRange(string s) {
        vector<string> ans;
        for (char i = s[0]; i <= s[3]; ++i) {
            for (char j = s[1]; j <= s[4]; ++j) {
                ans.push_back({i, j});
            }
        }
        return ans;
    }
};
```

#### Go

```go
func cellsInRange(s string) (ans []string) {
	for i := s[0]; i <= s[3]; i++ {
		for j := s[1]; j <= s[4]; j++ {
			ans = append(ans, string(i)+string(j))
		}
	}
	return
}
```

#### TypeScript

```ts
function cellsInRange(s: string): string[] {
    const ans: string[] = [];
    for (let i = s.charCodeAt(0); i <= s.charCodeAt(3); ++i) {
        for (let j = s.charCodeAt(1); j <= s.charCodeAt(4); ++j) {
            ans.push(String.fromCharCode(i) + String.fromCharCode(j));
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
