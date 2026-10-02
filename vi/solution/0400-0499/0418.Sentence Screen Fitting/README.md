---
comments: true
difficulty: Medium
tags:
    - Array
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [418. Sentence Screen Fitting 🔒](https://leetcode.com/problems/sentence-screen-fitting)

[中文文档](/solution/0400-0499/0418.Sentence%20Screen%20Fitting/README.md)

## Mô tả

<!-- description:start -->

<p>Cho màn hình có kích thước&nbsp;<code>rows x cols</code> và <code>sentence</code> là danh sách các chuỗi, hãy trả về <em>số lần có thể đặt câu đã cho vừa lên màn hình</em>.</p>

<p>Thứ tự các từ trong câu phải được giữ nguyên và không được tách một từ sang hai dòng. Giữa hai từ liên tiếp trên cùng một dòng phải có đúng một dấu cách.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> sentence = [&quot;hello&quot;,&quot;world&quot;], rows = 2, cols = 8
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
hello---
world---
Ký tự &#39;-&#39; biểu thị một ô trống trên màn hình.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> sentence = [&quot;a&quot;, &quot;bcd&quot;, &quot;e&quot;], rows = 3, cols = 6
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
a-bcd- 
e-a---
bcd-e-
Ký tự &#39;-&#39; biểu thị một ô trống trên màn hình.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> sentence = [&quot;i&quot;,&quot;had&quot;,&quot;apple&quot;,&quot;pie&quot;], rows = 4, cols = 5
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
i-had
apple
pie-i
had--
Ký tự &#39;-&#39; biểu thị một ô trống trên màn hình.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= sentence.length &lt;= 100</code></li>
	<li><code>1 &lt;= sentence[i].length &lt;= 10</code></li>
	<li><code>sentence[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>1 &lt;= rows, cols &lt;= 2 * 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mô phỏng lần lượt từng từ trên mỗi dòng khá rườm rà: số dòng có thể lên tới $10^4$ và câu được lặp lại nhiều lần.
>
> Nối các từ, thêm một dấu cách ở cuối để tạo thành chuỗi tuần hoàn $s$, rồi dùng $\textit{cur}$ lưu số ký tự đã đặt. Mỗi dòng tăng giá trị này thêm $\textit{cols}$: nếu vị trí mới rơi vào dấu cách thì dòng vừa kết thúc đúng ranh giới từ và ta tiến thêm một vị trí; nếu không, lùi về dấu cách trước đó, tức chuyển từ chưa hoàn chỉnh sang dòng tiếp theo.
>
> Số câu hoàn chỉnh được đặt là $\lfloor \textit{cur}/|s| \rfloor$. Dấu cách ở cuối biểu diễn khoảng cách bắt buộc giữa các từ, còn thao tác lùi lại đảm bảo không tách từ.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def wordsTyping(self, sentence: List[str], rows: int, cols: int) -> int:
        s = " ".join(sentence) + " "
        m = len(s)
        cur = 0
        for _ in range(rows):
            cur += cols
            if s[cur % m] == " ":
                cur += 1
            while cur and s[(cur - 1) % m] != " ":
                cur -= 1
        return cur // m
```

#### Java

```java
class Solution {
    public int wordsTyping(String[] sentence, int rows, int cols) {
        String s = String.join(" ", sentence) + " ";
        int m = s.length();
        int cur = 0;
        while (rows-- > 0) {
            cur += cols;
            if (s.charAt(cur % m) == ' ') {
                ++cur;
            } else {
                while (cur > 0 && s.charAt((cur - 1) % m) != ' ') {
                    --cur;
                }
            }
        }
        return cur / m;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int wordsTyping(vector<string>& sentence, int rows, int cols) {
        string s;
        for (auto& t : sentence) {
            s += t;
            s += " ";
        }
        int m = s.size();
        int cur = 0;
        while (rows--) {
            cur += cols;
            if (s[cur % m] == ' ') {
                ++cur;
            } else {
                while (cur && s[(cur - 1) % m] != ' ') {
                    --cur;
                }
            }
        }
        return cur / m;
    }
};
```

#### Go

```go
func wordsTyping(sentence []string, rows int, cols int) int {
	s := strings.Join(sentence, " ") + " "
	m := len(s)
	cur := 0
	for i := 0; i < rows; i++ {
		cur += cols
		if s[cur%m] == ' ' {
			cur++
		} else {
			for cur > 0 && s[(cur-1)%m] != ' ' {
				cur--
			}
		}
	}
	return cur / m
}
```

#### TypeScript

```ts
function wordsTyping(sentence: string[], rows: number, cols: number): number {
    const s = sentence.join(' ') + ' ';
    let cur = 0;
    const m = s.length;
    for (let i = 0; i < rows; ++i) {
        cur += cols;
        if (s[cur % m] === ' ') {
            ++cur;
        } else {
            while (cur > 0 && s[(cur - 1) % m] !== ' ') {
                --cur;
            }
        }
    }
    return Math.floor(cur / m);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
