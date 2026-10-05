---
comments: true
difficulty: Medium
tags:
    - Trie
    - String
---

<!-- problem:start -->

# [3758. Convert Number Words to Digits 🔒](https://leetcode.com/problems/convert-number-words-to-digits)

[Tài liệu tiếng Trung](/solution/3700-3799/3758.Convert%20Number%20Words%20to%20Digits/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường. <code>s</code> có thể chứa các từ tiếng Anh hợp lệ <strong>ghép liền nhau</strong> biểu diễn các chữ số từ 0 đến 9, không có khoảng trắng.</p>

<p>Nhiệm vụ của bạn là <strong>trích xuất</strong> từng từ số hợp lệ <strong>theo đúng thứ tự</strong> và chuyển nó thành chữ số tương ứng, tạo thành một chuỗi chữ số.</p>

<p>Phân tích <code>s</code> từ trái sang phải. Tại mỗi vị trí:</p>

<ul>
	<li>Nếu một từ số hợp lệ bắt đầu tại vị trí hiện tại, thêm chữ số tương ứng vào kết quả và tiến lên một đoạn bằng độ dài của từ đó.</li>
	<li>Nếu không, bỏ qua <strong>đúng</strong> một ký tự và tiếp tục phân tích.</li>
</ul>

<p>Trả về chuỗi chữ số thu được. Nếu không tìm thấy từ số nào, trả về chuỗi rỗng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;onefourthree&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;143&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Khi phân tích từ trái sang phải, trích xuất các từ số hợp lệ &quot;one&quot;, &quot;four&quot;, &quot;three&quot;.</li>
	<li>Chúng tương ứng với các chữ số 1, 4, 3. Vì vậy, kết quả cuối cùng là <code>&quot;143&quot;</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;ninexsix&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;96&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chuỗi con <code>&quot;nine&quot;</code> là một từ số hợp lệ và tương ứng với 9.</li>
	<li>Ký tự <code>&quot;x&quot;</code> không khớp với bất kỳ tiền tố từ số hợp lệ nào nên bị bỏ qua.</li>
	<li>Sau đó, chuỗi con <code>&quot;six&quot;</code> là một từ số hợp lệ và tương ứng với 6, nên kết quả cuối cùng là <code>&quot;96&quot;</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;zeero&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Không có chuỗi con nào tạo thành một từ số hợp lệ trong quá trình phân tích từ trái sang phải.</li>
	<li>Tất cả ký tự đều bị bỏ qua và các phần chưa hoàn chỉnh bị bỏ qua, nên kết quả là chuỗi rỗng.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;tw&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Không có chuỗi con nào tạo thành một từ số hợp lệ trong quá trình phân tích từ trái sang phải.</li>
	<li>Tất cả ký tự đều bị bỏ qua và các phần chưa hoàn chỉnh bị bỏ qua, nên kết quả là chuỗi rỗng.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ có mười từ số và tất cả đều ngắn. Khi quét từ trái sang phải, nếu vị trí hiện tại khớp với một từ, ta thêm chữ số tương ứng rồi bỏ qua số ký tự bằng độ dài của từ đó; nếu không, ta tiến lên một ký tự. Với $n\le 10^5$, thử mười từ tại mỗi vị trí là đủ.

<!-- thinking:end -->

Trước hết, chúng ta thiết lập mối quan hệ ánh xạ giữa các từ số và chữ số tương ứng, được lưu trong mảng $d$, trong đó $d[i]$ là từ tương ứng với chữ số $i$.

Sau đó, chúng ta duyệt chuỗi $s$ từ trái sang phải. Với mỗi vị trí $i$, chúng ta lần lượt xét các từ số $d[j]$ và kiểm tra xem chuỗi con bắt đầu tại vị trí $i$ có khớp với $d[j]$ hay không. Nếu tìm thấy khớp, chúng ta thêm chữ số $j$ vào kết quả và tiến vị trí $i$ lên $|d[j]|$ vị trí. Nếu không, chúng ta tiến vị trí $i$ lên 1 vị trí.

Chúng ta lặp lại quá trình này cho đến khi duyệt hết chuỗi $s$. Cuối cùng, chúng ta nối các chữ số trong kết quả thành một chuỗi và trả về chuỗi đó.

Độ phức tạp thời gian là $O(n \times |d|)$ và độ phức tạp không gian là $O(|d|)$, trong đó $n$ là độ dài của chuỗi $s$ và $|d|$ là số lượng từ biểu diễn chữ số.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def convertNumber(self, s: str) -> str:
        d = [
            "zero",
            "one",
            "two",
            "three",
            "four",
            "five",
            "six",
            "seven",
            "eight",
            "nine",
        ]
        i, n = 0, len(s)
        ans = []
        while i < n:
            for j, t in enumerate(d):
                m = len(t)
                if i + m <= n and s[i : i + m] == t:
                    ans.append(str(j))
                    i += m - 1
                    break
            i += 1
        return "".join(ans)
```

#### Java

```java
class Solution {
    public String convertNumber(String s) {
        String[] d
            = {"zero", "one", "two", "three", "four", "five", "six", "seven", "eight", "nine"};
        int n = s.length();
        StringBuilder ans = new StringBuilder();
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < d.length; ++j) {
                String t = d[j];
                int m = t.length();
                if (i + m <= n && s.substring(i, i + m).equals(t)) {
                    ans.append(j);
                    i += m - 1;
                    break;
                }
            }
        }
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string convertNumber(string s) {
        vector<string> d = {"zero", "one", "two", "three", "four", "five", "six", "seven", "eight", "nine"};
        int n = s.length();
        string ans;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < d.size(); ++j) {
                string t = d[j];
                int m = t.length();
                if (i + m <= n && s.substr(i, m) == t) {
                    ans += to_string(j);
                    i += m - 1;
                    break;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func convertNumber(s string) string {
	d := []string{"zero", "one", "two", "three", "four", "five", "six", "seven", "eight", "nine"}
	n := len(s)
	var ans strings.Builder
	for i := 0; i < n; i++ {
		for j, t := range d {
			m := len(t)
			if i+m <= n && s[i:i+m] == t {
				ans.WriteString(strconv.Itoa(j))
				i += m - 1
				break
			}
		}
	}
	return ans.String()
}
```

#### TypeScript

```ts
function convertNumber(s: string): string {
    const d: string[] = [
        'zero',
        'one',
        'two',
        'three',
        'four',
        'five',
        'six',
        'seven',
        'eight',
        'nine',
    ];
    const n = s.length;
    const ans: string[] = [];
    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < d.length; ++j) {
            const t = d[j];
            const m = t.length;
            if (i + m <= n && s.substring(i, i + m) === t) {
                ans.push(j.toString());
                i += m - 1;
                break;
            }
        }
    }
    return ans.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
