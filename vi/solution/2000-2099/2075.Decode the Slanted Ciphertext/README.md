---
comments: true
difficulty: Medium
rating: 1759
source: Weekly Contest 267 Q3
tags:
    - String
    - Simulation
---

<!-- problem:start -->

# [2075. Decode the Slanted Ciphertext](https://leetcode.com/problems/decode-the-slanted-ciphertext)

[中文文档](/solution/2000-2099/2075.Decode%20the%20Slanted%20Ciphertext/README.md)

## Mô tả

<!-- description:start -->

<p>Một chuỗi <code>originalText</code> được mã hóa bằng <strong>mật mã chuyển vị xiên</strong> thành chuỗi <code>encodedText</code> với sự trợ giúp của một ma trận có <strong>số hàng cố định</strong> là <code>rows</code>.</p>

<p><code>originalText</code> được điền vào ma trận theo thứ tự từ trên cùng bên trái đến dưới cùng bên phải.</p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2075.Decode%20the%20Slanted%20Ciphertext/images/exa11.png" style="width: 300px; height: 185px;" />
<p>Các ô màu xanh được điền trước, tiếp theo là các ô màu đỏ, rồi đến các ô màu vàng, cứ như vậy cho đến khi điền hết <code>originalText</code>. Mũi tên biểu thị thứ tự điền các ô. Tất cả các ô trống được điền bằng <code>&#39; &#39;</code>. Số cột được chọn sao cho cột ngoài cùng bên phải <strong>không bị trống</strong> sau khi điền <code>originalText</code>.</p>

<p>Sau đó, <code>encodedText</code> được tạo thành bằng cách nối tất cả các ký tự của ma trận theo thứ tự từng hàng.</p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2075.Decode%20the%20Slanted%20Ciphertext/images/exa12.png" style="width: 300px; height: 200px;" />
<p>Các ký tự trong các ô màu xanh được nối vào <code>encodedText</code> trước, tiếp theo là các ô màu đỏ, rồi đến các ô màu vàng, và cuối cùng là các ô còn lại. Mũi tên biểu thị thứ tự truy cập các ô.</p>

<p>Ví dụ, nếu <code>originalText = &quot;cipher&quot;</code> và <code>rows = 3</code>, ta mã hóa theo cách sau:</p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2075.Decode%20the%20Slanted%20Ciphertext/images/desc2.png" style="width: 281px; height: 211px;" />
<p>Các mũi tên màu xanh biểu diễn cách đặt <code>originalText</code> vào ma trận, còn các mũi tên màu đỏ biểu diễn thứ tự tạo thành <code>encodedText</code>. Trong ví dụ trên, <code>encodedText = &quot;ch   ie   pr&quot;</code>.</p>

<p>Cho chuỗi đã mã hóa <code>encodedText</code> và số hàng <code>rows</code>, hãy trả về <em>chuỗi ban đầu</em> <code>originalText</code>.</p>

<p><strong>Lưu ý:</strong> <code>originalText</code> <strong>không</strong> có khoảng trắng ở cuối <code>&#39; &#39;</code>. Các test case được tạo sao cho chỉ có duy nhất một <code>originalText</code> khả dĩ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> encodedText = &quot;ch   ie   pr&quot;, rows = 3
<strong>Đầu ra:</strong> &quot;cipher&quot;
<strong>Giải thích:</strong> Đây chính là ví dụ được mô tả trong phần đề bài.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2075.Decode%20the%20Slanted%20Ciphertext/images/exam1.png" style="width: 250px; height: 168px;" />
<pre>
<strong>Đầu vào:</strong> encodedText = &quot;iveo    eed   l te   olc&quot;, rows = 4
<strong>Đầu ra:</strong> &quot;i love leetcode&quot;
<strong>Giải thích:</strong> Hình trên biểu diễn ma trận được dùng để mã hóa originalText.
Các mũi tên màu xanh cho biết cách tìm originalText từ encodedText.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2075.Decode%20the%20Slanted%20Ciphertext/images/eg2.png" style="width: 300px; height: 51px;" />
<pre>
<strong>Đầu vào:</strong> encodedText = &quot;coding&quot;, rows = 1
<strong>Đầu ra:</strong> &quot;coding&quot;
<strong>Giải thích:</strong> Vì chỉ có 1 hàng nên originalText và encodedText giống nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= encodedText.length &lt;= 10<sup>6</sup></code></li>
	<li><code>encodedText</code> chỉ gồm các chữ cái tiếng Anh viết thường và <code>&#39; &#39;</code>.</li>
<li><code>encodedText</code> là một mã hóa hợp lệ của một <code>originalText</code> <strong>không</strong> có khoảng trắng ở cuối.</li>
	<li><code>1 &lt;= rows &lt;= 1000</code></li>
	<li>Các test case được tạo sao cho chỉ có <strong>duy nhất một</strong> <code>originalText</code> khả dĩ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Văn bản gốc được ghi theo hàng rồi đọc theo các đường chéo. Số cột được xác định từ độ dài và `rows`, vì vậy ta mô phỏng lại các đường chéo đó. Mỗi ký tự được duyệt đúng một lần với $n \le 10^6$.
>
> Bắt đầu từ từng cột trên hàng đầu tiên, đi xuống dưới và sang phải, sau đó `rstrip` các khoảng trắng ở cuối theo đề bài.

<!-- thinking:end -->

Trước hết, ta tính số cột của ma trận $cols = \textit{len}(encodedText) / rows$. Sau đó, theo đúng quy tắc được mô tả trong đề bài, ta bắt đầu duyệt ma trận từ góc trên bên trái và thêm các ký tự vào kết quả.

Cuối cùng, ta trả về kết quả, đồng thời loại bỏ mọi khoảng trắng ở cuối.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi $encodedText$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def decodeCiphertext(self, encodedText: str, rows: int) -> str:
        ans = []
        cols = len(encodedText) // rows
        for j in range(cols):
            x, y = 0, j
            while x < rows and y < cols:
                ans.append(encodedText[x * cols + y])
                x, y = x + 1, y + 1
        return ''.join(ans).rstrip()
```

#### Java

```java
class Solution {
    public String decodeCiphertext(String encodedText, int rows) {
        StringBuilder ans = new StringBuilder();
        int cols = encodedText.length() / rows;
        for (int j = 0; j < cols; ++j) {
            for (int x = 0, y = j; x < rows && y < cols; ++x, ++y) {
                ans.append(encodedText.charAt(x * cols + y));
            }
        }
        while (ans.length() > 0 && ans.charAt(ans.length() - 1) == ' ') {
            ans.deleteCharAt(ans.length() - 1);
        }
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string decodeCiphertext(string encodedText, int rows) {
        string ans;
        int cols = encodedText.size() / rows;
        for (int j = 0; j < cols; ++j) {
            for (int x = 0, y = j; x < rows && y < cols; ++x, ++y) {
                ans += encodedText[x * cols + y];
            }
        }
        while (ans.size() && ans.back() == ' ') {
            ans.pop_back();
        }
        return ans;
    }
};
```

#### Go

```go
func decodeCiphertext(encodedText string, rows int) string {
	ans := []byte{}
	cols := len(encodedText) / rows
	for j := 0; j < cols; j++ {
		for x, y := 0, j; x < rows && y < cols; x, y = x+1, y+1 {
			ans = append(ans, encodedText[x*cols+y])
		}
	}
	for len(ans) > 0 && ans[len(ans)-1] == ' ' {
		ans = ans[:len(ans)-1]
	}
	return string(ans)
}
```

#### TypeScript

```ts
function decodeCiphertext(encodedText: string, rows: number): string {
    const cols = Math.ceil(encodedText.length / rows);
    const ans: string[] = [];
    for (let k = 0; k <= cols; k++) {
        for (let i = 0, j = k; i < rows && j < cols; i++, j++) {
            ans.push(encodedText[i * cols + j]);
        }
    }
    return ans.join('').trimEnd();
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
