---
comments: true
difficulty: Medium
rating: 1185
source: Weekly Contest 458 Q1
tags:
    - String
    - Simulation
---

<!-- problem:start -->

# [3612. Process String with Special Operations I](https://leetcode.com/problems/process-string-with-special-operations-i)

[中文文档](/solution/3600-3699/3612.Process%20String%20with%20Special%20Operations%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một chuỗi <code>s</code> gồm các chữ cái tiếng Anh viết thường và các ký tự đặc biệt: <code>*</code>, <code>#</code> và <code>%</code>.</p>

<p>Hãy xây dựng một chuỗi mới <code>result</code> bằng cách xử lý <code>s</code> từ trái sang phải theo các quy tắc sau:</p>

<ul>
    <li>Nếu ký tự là một chữ cái tiếng Anh viết <strong>thường</strong>, thêm ký tự đó vào <code>result</code>.</li>
    <li><code>&#39;*&#39;</code> <strong>xóa</strong> ký tự cuối cùng khỏi <code>result</code>, nếu ký tự đó tồn tại.</li>
    <li><code>&#39;#&#39;</code> <strong>nhân đôi</strong> <code>result</code> hiện tại và <strong>nối</strong> bản sao vào chính nó.</li>
    <li><code>&#39;%&#39;</code> <strong>đảo ngược</strong> <code>result</code> hiện tại.</li>
</ul>

<p>Trả về chuỗi cuối cùng <code>result</code> sau khi xử lý tất cả các ký tự trong <code>s</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;a#b%*&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;ba&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
    <thead>
        <tr>
            <th style="border: 1px solid black;"><code>i</code></th>
            <th style="border: 1px solid black;"><code>s[i]</code></th>
            <th style="border: 1px solid black;">Thao tác</th>
            <th style="border: 1px solid black;"><code>result</code> hiện tại</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="border: 1px solid black;">0</td>
            <td style="border: 1px solid black;"><code>&#39;a&#39;</code></td>
            <td style="border: 1px solid black;">Thêm <code>&#39;a&#39;</code></td>
            <td style="border: 1px solid black;"><code>&quot;a&quot;</code></td>
        </tr>
        <tr>
            <td style="border: 1px solid black;">1</td>
            <td style="border: 1px solid black;"><code>&#39;#&#39;</code></td>
            <td style="border: 1px solid black;">Nhân đôi <code>result</code></td>
            <td style="border: 1px solid black;"><code>&quot;aa&quot;</code></td>
        </tr>
        <tr>
            <td style="border: 1px solid black;">2</td>
            <td style="border: 1px solid black;"><code>&#39;b&#39;</code></td>
            <td style="border: 1px solid black;">Thêm <code>&#39;b&#39;</code></td>
            <td style="border: 1px solid black;"><code>&quot;aab&quot;</code></td>
        </tr>
        <tr>
            <td style="border: 1px solid black;">3</td>
            <td style="border: 1px solid black;"><code>&#39;%&#39;</code></td>
            <td style="border: 1px solid black;">Đảo ngược <code>result</code></td>
            <td style="border: 1px solid black;"><code>&quot;baa&quot;</code></td>
        </tr>
        <tr>
            <td style="border: 1px solid black;">4</td>
            <td style="border: 1px solid black;"><code>&#39;*&#39;</code></td>
            <td style="border: 1px solid black;">Xóa ký tự cuối cùng</td>
            <td style="border: 1px solid black;"><code>&quot;ba&quot;</code></td>
        </tr>
    </tbody>
</table>

<p>Vậy chuỗi <code>result</code> cuối cùng là <code>&quot;ba&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;z*#&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
    <thead>
        <tr>
            <th style="border: 1px solid black;"><code>i</code></th>
            <th style="border: 1px solid black;"><code>s[i]</code></th>
            <th style="border: 1px solid black;">Thao tác</th>
            <th style="border: 1px solid black;"><code>result</code> hiện tại</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="border: 1px solid black;">0</td>
            <td style="border: 1px solid black;"><code>&#39;z&#39;</code></td>
            <td style="border: 1px solid black;">Thêm <code>&#39;z&#39;</code></td>
            <td style="border: 1px solid black;"><code>&quot;z&quot;</code></td>
        </tr>
        <tr>
            <td style="border: 1px solid black;">1</td>
            <td style="border: 1px solid black;"><code>&#39;*&#39;</code></td>
            <td style="border: 1px solid black;">Xóa ký tự cuối cùng</td>
            <td style="border: 1px solid black;"><code>&quot;&quot;</code></td>
        </tr>
        <tr>
            <td style="border: 1px solid black;">2</td>
            <td style="border: 1px solid black;"><code>&#39;#&#39;</code></td>
            <td style="border: 1px solid black;">Nhân đôi chuỗi</td>
            <td style="border: 1px solid black;"><code>&quot;&quot;</code></td>
        </tr>
    </tbody>
</table>

<p>Vậy chuỗi <code>result</code> cuối cùng là <code>&quot;&quot;</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= s.length &lt;= 20</code></li>
    <li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường và các ký tự đặc biệt <code>*</code>, <code>#</code> và <code>%</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Các thao tác đều tác động lên kết quả hiện tại: thêm ký tự, lấy ra phần tử cuối khi gặp `*`, nhân đôi khi gặp `#`, và đảo ngược khi gặp `%`. Vì $n$ nhỏ nên ta có thể mô phỏng bằng một list.
>
> `#` có thể nhân đôi độ dài, dẫn đến độ phức tạp theo cấp số mũ trong trường hợp xấu nhất, nhưng đây là chuỗi cần tạo ra theo yêu cầu. Bỏ qua `*` khi kết quả đang rỗng để không lấy phần tử vượt quá đầu list.
>
> Duyệt các ký tự theo thứ tự và ghép lại ở cuối. Một list đáp ứng được thao tác xóa phần tử cuối, sao chép toàn bộ và đảo ngược tại chỗ.

<!-- thinking:end -->

Ta có thể mô phỏng trực tiếp các thao tác được mô tả trong đề bài. Ta dùng một list $\text{result}$ để lưu chuỗi kết quả hiện tại. Với mỗi ký tự trong chuỗi đầu vào $s$, ta thực hiện thao tác tương ứng dựa trên loại ký tự:

- Nếu ký tự là một chữ cái tiếng Anh viết thường, thêm ký tự đó vào $\text{result}$.
- Nếu ký tự là `*`, xóa ký tự cuối cùng trong $\text{result}$ (nếu tồn tại).
- Nếu ký tự là `#`, sao chép $\text{result}$ rồi nối bản sao vào chính nó.
- Nếu ký tự là `%`, đảo ngược $\text{result}$.

Cuối cùng, ta chuyển $\text{result}$ thành chuỗi và trả về.

Độ phức tạp thời gian là $O(2^n)$, trong đó $n$ là độ dài của chuỗi $s$. Trong trường hợp xấu nhất, thao tác `#` có thể làm độ dài của $\text{result}$ tăng gấp đôi sau mỗi lần, dẫn đến độ phức tạp thời gian theo cấp số mũ. Không tính phần bộ nhớ dùng cho đáp án, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def processStr(self, s: str) -> str:
        result = []
        for c in s:
            if c.isalpha():
                result.append(c)
            elif c == "*" and result:
                result.pop()
            elif c == "#":
                result.extend(result)
            elif c == "%":
                result.reverse()
        return "".join(result)
```

#### Java

```java
class Solution {
    public String processStr(String s) {
        StringBuilder result = new StringBuilder();
        for (char c : s.toCharArray()) {
            if (Character.isLetter(c)) {
                result.append(c);
            } else if (c == '*') {
                result.setLength(Math.max(0, result.length() - 1));
            } else if (c == '#') {
                result.append(result);
            } else if (c == '%') {
                result.reverse();
            }
        }
        return result.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string processStr(string s) {
        string result;
        for (char c : s) {
            if (isalpha(c)) {
                result += c;
            } else if (c == '*') {
                if (!result.empty()) {
                    result.pop_back();
                }
            } else if (c == '#') {
                result += result;
            } else if (c == '%') {
                ranges::reverse(result);
            }
        }
        return result;
    }
};
```

#### Go

```go
func processStr(s string) string {
    var result []rune
    for _, c := range s {
        if unicode.IsLetter(c) {
            result = append(result, c)
        } else if c == '*' {
            if len(result) > 0 {
                result = result[:len(result)-1]
            }
        } else if c == '#' {
            result = append(result, result...)
        } else if c == '%' {
            slices.Reverse(result)
        }
    }
    return string(result)
}
```

#### TypeScript

```ts
function processStr(s: string): string {
    const result: string[] = [];
    for (const c of s) {
        if (/[a-zA-Z]/.test(c)) {
            result.push(c);
        } else if (c === '*') {
            if (result.length > 0) {
                result.pop();
            }
        } else if (c === '#') {
            result.push(...result);
        } else if (c === '%') {
            result.reverse();
        }
    }
    return result.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
