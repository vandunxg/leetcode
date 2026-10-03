---
comments: true
difficulty: Easy
rating: 1235
source: Weekly Contest 235 Q1
tags:
    - Array
    - String
---

<!-- problem:start -->

# [1816. Truncate Sentence](https://leetcode.com/problems/truncate-sentence)

[中文文档](/solution/1800-1899/1816.Truncate%20Sentence/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Câu</strong> là một danh sách các từ được ngăn cách bằng đúng một dấu cách, không có dấu cách ở đầu hoặc cuối. Mỗi từ <strong>chỉ</strong> gồm các chữ cái tiếng Anh viết hoa và viết thường (không có dấu câu).</p>

<ul>
	<li>Ví dụ, <code>&quot;Hello World&quot;</code>, <code>&quot;HELLO&quot;</code> và <code>&quot;hello world hello world&quot;</code> đều là các câu.</li>
</ul>

<p>Cho một câu <code>s</code>​​​​​​ và một số nguyên <code>k</code>​​​​​​. Bạn cần <strong>cắt ngắn</strong> <code>s</code>​​​​​​ sao cho nó chỉ chứa <code>k</code>​​​​​​ từ <strong>đầu tiên</strong>. Trả về <code>s</code>​​​​<em>​​ sau khi <strong>cắt ngắn</strong> nó.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;Hello how are you Contestant&quot;, k = 4
<strong>Đầu ra:</strong> &quot;Hello how are you&quot;
<strong>Giải thích:</strong>
Các từ trong s là [&quot;Hello&quot;, &quot;how&quot;, &quot;are&quot;, &quot;you&quot;, &quot;Contestant&quot;].
4 từ đầu tiên là [&quot;Hello&quot;, &quot;how&quot;, &quot;are&quot;, &quot;you&quot;].
Vì vậy, ta trả về &quot;Hello how are you&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;What is the solution to this problem&quot;, k = 4
<strong>Đầu ra:</strong> &quot;What is the solution&quot;
<strong>Giải thích:</strong>
Các từ trong s là [&quot;What&quot;, &quot;is&quot; &quot;the&quot;, &quot;solution&quot;, &quot;to&quot;, &quot;this&quot;, &quot;problem&quot;].
4 từ đầu tiên là [&quot;What&quot;, &quot;is&quot;, &quot;the&quot;, &quot;solution&quot;].
Vì vậy, ta trả về &quot;What is the solution&quot;.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;chopper is not a tanuki&quot;, k = 5
<strong>Đầu ra:</strong> &quot;chopper is not a tanuki&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 500</code></li>
	<li><code>k</code> nằm trong khoảng <code>[1, the number of words in s]</code>.</li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường, viết hoa và dấu cách.</li>
	<li>Các từ trong <code>s</code> được ngăn cách bằng đúng một dấu cách.</li>
	<li>Không có dấu cách ở đầu hoặc cuối.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tách chuỗi

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần giữ lại $k$ từ đầu tiên của một câu. Tách theo dấu cách rồi ghép lại là cách trực tiếp, nhưng cần một danh sách từ trung gian.
>
> Hàm $\textit{split}$ của ngôn ngữ đã biến chuỗi thành các token theo khoảng trắng; lấy $k$ token đầu tiên và ghép bằng dấu cách đúng với mô tả bài toán.

<!-- thinking:end -->

Tách câu theo dấu cách, sau đó ghép lại $k$ từ đầu tiên.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def truncateSentence(self, s: str, k: int) -> str:
        return ' '.join(s.split()[:k])
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 cấp phát một mảng trung gian. Nếu chỉ cần vị trí cắt, ta quét từ trái sang phải và giảm $k$ sau mỗi dấu cách; khi $k$ về $0$, chỉ số hiện tại là dấu cách sau từ thứ $k$. Nếu kết thúc khi $k>0$, câu có ít hơn $k$ từ nên ta giữ nguyên. Phần không gian phụ giảm còn $O(1)$.

<!-- thinking:end -->

Ta duyệt chuỗi $s$ từ đầu. Với ký tự hiện tại $s[i]$, nếu đó là dấu cách, ta giảm $k$. Khi $k$ bằng $0$, nghĩa là ta đã lấy đủ $k$ từ, nên trả về chuỗi con $s[0..i)$.

Sau khi duyệt xong, ta trả về $s$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$. Không tính phần không gian của kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def truncateSentence(self, s: str, k: int) -> str:
        for i, c in enumerate(s):
            k -= c == ' '
            if k == 0:
                return s[:i]
        return s
```

#### Java

```java
class Solution {
    public String truncateSentence(String s, int k) {
        for (int i = 0; i < s.length(); ++i) {
            if (s.charAt(i) == ' ' && (--k) == 0) {
                return s.substring(0, i);
            }
        }
        return s;
    }
}
```

#### C++

```cpp
class Solution {
public:
    string truncateSentence(string s, int k) {
        for (int i = 0; i < s.size(); ++i) {
            if (s[i] == ' ' && (--k) == 0) {
                return s.substr(0, i);
            }
        }
        return s;
    }
};
```

#### Go

```go
func truncateSentence(s string, k int) string {
	for i, c := range s {
		if c == ' ' {
			k--
		}
		if k == 0 {
			return s[:i]
		}
	}
	return s
}
```

#### TypeScript

```ts
function truncateSentence(s: string, k: number): string {
    for (let i = 0; i < s.length; ++i) {
        if (s[i] === ' ' && --k === 0) {
            return s.slice(0, i);
        }
    }
    return s;
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @param {number} k
 * @return {string}
 */
var truncateSentence = function (s, k) {
    for (let i = 0; i < s.length; ++i) {
        if (s[i] === ' ' && --k === 0) {
            return s.slice(0, i);
        }
    }
    return s;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
