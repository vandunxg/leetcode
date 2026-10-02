---
comments: true
difficulty: Easy
tags:
    - Array
    - Matrix
---

<!-- problem:start -->

# [422. Valid Word Square 🔒](https://leetcode.com/problems/valid-word-square)

[中文文档](/solution/0400-0499/0422.Valid%20Word%20Square/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng chuỗi <code>words</code>, trả về <code>true</code> <em>nếu các chuỗi tạo thành một <strong>word square</strong> hợp lệ</em>.</p>

<p>Một dãy chuỗi tạo thành <strong>word square</strong> hợp lệ nếu hàng và cột thứ <code>k<sup>th</sup></code> tạo thành cùng một chuỗi, với <code>0 &lt;= k &lt; max(numRows, numColumns)</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0422.Valid%20Word%20Square/images/validsq1-grid.jpg" style="width: 333px; height: 333px;" />
<pre>
<strong>Đầu vào:</strong> words = [&quot;abcd&quot;,&quot;bnrt&quot;,&quot;crmy&quot;,&quot;dtye&quot;]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong>
Hàng thứ 1 và cột thứ 1 đều tạo thành &quot;abcd&quot;.
Hàng thứ 2 và cột thứ 2 đều tạo thành &quot;bnrt&quot;.
Hàng thứ 3 và cột thứ 3 đều tạo thành &quot;crmy&quot;.
Hàng thứ 4 và cột thứ 4 đều tạo thành &quot;dtye&quot;.
Vì vậy, đây là một word square hợp lệ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0422.Valid%20Word%20Square/images/validsq2-grid.jpg" style="width: 333px; height: 333px;" />
<pre>
<strong>Đầu vào:</strong> words = [&quot;abcd&quot;,&quot;bnrt&quot;,&quot;crm&quot;,&quot;dt&quot;]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong>
Hàng thứ 1 và cột thứ 1 đều tạo thành &quot;abcd&quot;.
Hàng thứ 2 và cột thứ 2 đều tạo thành &quot;bnrt&quot;.
Hàng thứ 3 và cột thứ 3 đều tạo thành &quot;crm&quot;.
Hàng thứ 4 và cột thứ 4 đều tạo thành &quot;dt&quot;.
Vì vậy, đây là một word square hợp lệ.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0422.Valid%20Word%20Square/images/validsq3-grid.jpg" style="width: 333px; height: 333px;" />
<pre>
<strong>Đầu vào:</strong> words = [&quot;ball&quot;,&quot;area&quot;,&quot;read&quot;,&quot;lady&quot;]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong>
Hàng thứ 3 tạo thành &quot;read&quot;, còn cột thứ 3 tạo thành &quot;lead&quot;.
Vì vậy, đây KHÔNG phải word square hợp lệ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 500</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 500</code></li>
	<li><code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Kiểm tra lặp

<!-- thinking:start -->

> **Tư duy**
>
> Word square cần thỏa mãn $words[i][j]=words[j][i]$. Các hàng có thể dài ngắn khác nhau, nên nếu dựng ma trận chuyển vị trước thì sẽ gặp các cột bị thiếu.
>
> Kiểm tra từng ký tự với $words[j][i]$; nếu chỉ số vượt phạm vi hoặc ký tự không khớp thì trả về false.
>
> Nếu không có $words[j][i]$ thì điều kiện không thỏa: cột tương ứng không tạo thành cùng một từ.

<!-- thinking:end -->

Ta nhận thấy nếu $words[i][j] \neq words[j][i]$, có thể trả về `false` ngay.

Vì vậy, ta chỉ cần duyệt từng hàng và kiểm tra điều kiện $words[i][j] = words[j][i]$. Lưu ý rằng nếu chỉ số vượt phạm vi, ta cũng trả về `false` ngay.

Độ phức tạp thời gian là $O(n^2)$, trong đó $n$ là số chuỗi trong `words`. Độ phức tạp không gian là $O(1)`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def validWordSquare(self, words: List[str]) -> bool:
        m = len(words)
        for i, w in enumerate(words):
            for j, c in enumerate(w):
                if j >= m or i >= len(words[j]) or c != words[j][i]:
                    return False
        return True
```

#### Java

```java
class Solution {
    public boolean validWordSquare(List<String> words) {
        int m = words.size();
        for (int i = 0; i < m; ++i) {
            int n = words.get(i).length();
            for (int j = 0; j < n; ++j) {
                if (j >= m || i >= words.get(j).length()) {
                    return false;
                }
                if (words.get(i).charAt(j) != words.get(j).charAt(i)) {
                    return false;
                }
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool validWordSquare(vector<string>& words) {
        int m = words.size();
        for (int i = 0; i < m; ++i) {
            int n = words[i].size();
            for (int j = 0; j < n; ++j) {
                if (j >= m || i >= words[j].size() || words[i][j] != words[j][i]) {
                    return false;
                }
            }
        }
        return true;
    }
};
```

#### Go

```go
func validWordSquare(words []string) bool {
	m := len(words)
	for i, w := range words {
		for j := range w {
			if j >= m || i >= len(words[j]) || w[j] != words[j][i] {
				return false
			}
		}
	}
	return true
}
```

#### TypeScript

```ts
function validWordSquare(words: string[]): boolean {
    const m = words.length;
    for (let i = 0; i < m; ++i) {
        const n = words[i].length;
        for (let j = 0; j < n; ++j) {
            if (j >= m || i >= words[j].length || words[i][j] !== words[j][i]) {
                return false;
            }
        }
    }
    return true;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
