---
comments: true
difficulty: Easy
tags:
    - Two Pointers
    - String
---

<!-- problem:start -->

# [408. Valid Word Abbreviation 🔒](https://leetcode.com/problems/valid-word-abbreviation)

[中文文档](/solution/0400-0499/0408.Valid%20Word%20Abbreviation/README.md)

## Mô tả

<!-- description:start -->

<p>Có thể <strong>viết tắt</strong> một chuỗi bằng cách thay thế một số chuỗi con <strong>không liền kề</strong>, <strong>không rỗng</strong> bằng độ dài tương ứng. Các độ dài này <strong>không được</strong> có số 0 ở đầu.</p>

<p>Ví dụ, chuỗi <code>&quot;substitution&quot;</code> có thể được viết tắt thành (nhưng không giới hạn ở):</p>

<ul>
	<li><code>&quot;s10n&quot;</code> (<code>&quot;s <u>ubstitutio</u> n&quot;</code>)</li>
	<li><code>&quot;sub4u4&quot;</code> (<code>&quot;sub <u>stit</u> u <u>tion</u>&quot;</code>)</li>
	<li><code>&quot;12&quot;</code> (<code>&quot;<u>substitution</u>&quot;</code>)</li>
	<li><code>&quot;su3i1u2on&quot;</code> (<code>&quot;su <u>bst</u> i <u>t</u> u <u>ti</u> on&quot;</code>)</li>
	<li><code>&quot;substitution&quot;</code> (không thay thế chuỗi con nào)</li>
</ul>

<p>Các cách viết tắt sau đây <strong>không hợp lệ</strong>:</p>

<ul>
	<li><code>&quot;s55n&quot;</code> (<code>&quot;s <u>ubsti</u> <u>tutio</u> n&quot;</code>, hai chuỗi con bị thay thế nằm liền kề nhau)</li>
	<li><code>&quot;s010n&quot;</code> (có số 0 ở đầu)</li>
	<li><code>&quot;s0ubstitution&quot;</code> (thay thế một chuỗi con rỗng)</li>
</ul>

<p>Cho chuỗi <code>word</code> và cách viết tắt <code>abbr</code>. Hãy xác định <em>chuỗi có <strong>khớp</strong> với cách viết tắt đã cho hay không</em>.</p>

<p><strong>Chuỗi con</strong> là một dãy ký tự <strong>không rỗng</strong> liên tiếp trong một chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;internationalization&quot;, abbr = &quot;i12iz4n&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Có thể viết tắt từ &quot;internationalization&quot; thành &quot;i12iz4n&quot; (&quot;i <u>nternational</u> iz <u>atio</u> n&quot;).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;apple&quot;, abbr = &quot;a2e&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không thể viết tắt từ &quot;apple&quot; thành &quot;a2e&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= word.length &lt;= 20</code></li>
	<li><code>word</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>1 &lt;= abbr.length &lt;= 10</code></li>
	<li><code>abbr</code> chỉ gồm các chữ cái tiếng Anh viết thường và chữ số.</li>
	<li>Tất cả số nguyên trong <code>abbr</code> đều nằm trong phạm vi của số nguyên 32-bit.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Cách viết tắt xen kẽ các chữ cái và độ dài cần bỏ qua; số biểu thị độ dài không được có số 0 ở đầu. Không cần tạo chuỗi mới bằng cách khai triển cách viết tắt vì chuỗi đó có thể rất dài.
>
> Dùng hai con trỏ duyệt $\textit{abbr}$: các chữ số được ghép thành số ký tự cần bỏ qua (nếu có số 0 ở đầu thì từ chối); khi gặp chữ cái, trước tiên áp dụng bước bỏ qua rồi so sánh ký tự. Cuối cùng, con trỏ trong word cộng với số ký tự còn cần bỏ qua phải đúng bằng độ dài chuỗi.
>
> Thực hiện bỏ qua và so sánh trong cùng một lượt để kiểm tra đồng thời trường hợp tràn số, không khớp và còn dư độ dài chưa dùng.

<!-- thinking:end -->

Có thể mô phỏng trực tiếp quá trình so khớp ký tự và thay thế.

Gọi độ dài của chuỗi $word$ và $abbr$ lần lượt là $m$ và $n$. Dùng hai con trỏ $i$ và $j$ lần lượt trỏ đến vị trí hiện tại trong $word$ và $abbr$, cùng biến số nguyên $x$ để lưu số đang được đọc trong $abbr$.

Duyệt để so khớp từng ký tự của chuỗi $word$ và $abbr$:

Nếu ký tự $abbr[j]$ mà con trỏ $j$ đang trỏ tới là chữ số, và $abbr[j]$ là `'0'` trong khi $x$ bằng $0$, thì số trong $abbr$ có số 0 ở đầu nên cách viết tắt không hợp lệ; trả về `false`. Nếu không, cập nhật $x$ thành $x \times 10 + abbr[j] - '0'$.

Nếu ký tự $abbr[j]$ không phải chữ số, trước tiên tăng con trỏ $i$ thêm $x$ vị trí rồi đặt lại $x$ thành $0$. Nếu lúc này $i \geq m$ hoặc $word[i] \neq abbr[j]$, hai chuỗi không khớp nên trả về `false`; nếu không, tăng $i$ thêm $1$.

Sau đó tăng con trỏ $j$ thêm $1$ rồi lặp lại các bước trên cho đến khi $i$ vượt quá độ dài chuỗi $word$ hoặc $j$ vượt quá độ dài chuỗi $abbr$.

Cuối cùng, nếu $i + x$ bằng $m$ và $j$ bằng $n$, chuỗi $word$ khớp với cách viết tắt $abbr$ nên trả về `true`; nếu không thì trả về `false`.

Độ phức tạp thời gian là $O(m + n)$, trong đó $m$ và $n$ lần lượt là độ dài của chuỗi $word$ và $abbr$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def validWordAbbreviation(self, word: str, abbr: str) -> bool:
        m, n = len(word), len(abbr)
        i = j = x = 0
        while i < m and j < n:
            if abbr[j].isdigit():
                if abbr[j] == "0" and x == 0:
                    return False
                x = x * 10 + int(abbr[j])
            else:
                i += x
                x = 0
                if i >= m or word[i] != abbr[j]:
                    return False
                i += 1
            j += 1
        return i + x == m and j == n
```

#### Java

```java
class Solution {
    public boolean validWordAbbreviation(String word, String abbr) {
        int m = word.length(), n = abbr.length();
        int i = 0, j = 0, x = 0;
        for (; i < m && j < n; ++j) {
            char c = abbr.charAt(j);
            if (Character.isDigit(c)) {
                if (c == '0' && x == 0) {
                    return false;
                }
                x = x * 10 + (c - '0');
            } else {
                i += x;
                x = 0;
                if (i >= m || word.charAt(i) != c) {
                    return false;
                }
                ++i;
            }
        }
        return i + x == m && j == n;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool validWordAbbreviation(string word, string abbr) {
        int m = word.size(), n = abbr.size();
        int i = 0, j = 0, x = 0;
        for (; i < m && j < n; ++j) {
            if (isdigit(abbr[j])) {
                if (abbr[j] == '0' && x == 0) {
                    return false;
                }
                x = x * 10 + (abbr[j] - '0');
            } else {
                i += x;
                x = 0;
                if (i >= m || word[i] != abbr[j]) {
                    return false;
                }
                ++i;
            }
        }
        return i + x == m && j == n;
    }
};
```

#### Go

```go
func validWordAbbreviation(word string, abbr string) bool {
	m, n := len(word), len(abbr)
	i, j, x := 0, 0, 0
	for ; i < m && j < n; j++ {
		if abbr[j] >= '0' && abbr[j] <= '9' {
			if x == 0 && abbr[j] == '0' {
				return false
			}
			x = x*10 + int(abbr[j]-'0')
		} else {
			i += x
			x = 0
			if i >= m || word[i] != abbr[j] {
				return false
			}
			i++
		}
	}
	return i+x == m && j == n
}
```

#### TypeScript

```ts
function validWordAbbreviation(word: string, abbr: string): boolean {
    const [m, n] = [word.length, abbr.length];
    let [i, j, x] = [0, 0, 0];
    for (; i < m && j < n; ++j) {
        if (abbr[j] >= '0' && abbr[j] <= '9') {
            if (abbr[j] === '0' && x === 0) {
                return false;
            }
            x = x * 10 + Number(abbr[j]);
        } else {
            i += x;
            x = 0;
            if (i >= m || word[i++] !== abbr[j]) {
                return false;
            }
        }
    }
    return i + x === m && j === n;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
