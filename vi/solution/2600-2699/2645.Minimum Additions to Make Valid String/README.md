---
comments: true
difficulty: Medium
rating: 1477
source: Weekly Contest 341 Q3
tags:
    - Stack
    - Greedy
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [2645. Minimum Additions to Make Valid String](https://leetcode.com/problems/minimum-additions-to-make-valid-string)

[中文文档](/solution/2600-2699/2645.Minimum%20Additions%20to%20Make%20Valid%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>word</code>, bạn có thể chèn các chữ cái &quot;a&quot;, &quot;b&quot; hoặc &quot;c&quot; vào bất kỳ vị trí nào và với số lần tùy ý. Hãy trả về <em>số chữ cái ít nhất cần chèn để <code>word</code> trở thành một chuỗi <strong>hợp lệ</strong>.</em></p>

<p>Một chuỗi được gọi là <strong>hợp lệ </strong> nếu có thể tạo thành bằng cách nối chuỗi &quot;abc&quot; một số lần.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;b&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Chèn chữ cái &quot;a&quot; ngay trước &quot;b&quot; và chữ cái &quot;c&quot; ngay sau &quot;b&quot; để thu được chuỗi hợp lệ &quot;<strong>a</strong>b<strong>c</strong>&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;aaa&quot;
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Chèn các chữ cái &quot;b&quot; và &quot;c&quot; ngay sau mỗi &quot;a&quot; để thu được chuỗi hợp lệ &quot;a<strong>bc</strong>a<strong>bc</strong>a<strong>bc</strong>&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;abc&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> word đã hợp lệ. Không cần thay đổi gì.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= word.length &lt;= 50</code></li>
	<li><code>word</code> chỉ gồm các chữ cái &quot;a&quot;, &quot;b&quot; và &quot;c&quot;.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Mục tiêu là các phép nối của `abc`, và ta chỉ được phép chèn. Có thể dùng DP trên các điểm chia với $n \le 50$, nhưng việc đối chiếu có thể tiến hành theo chiến lược tham lam.
>
> Duyệt theo mẫu lặp `abc`: mỗi lần không khớp tương ứng với một lần chèn, còn mỗi lần khớp sẽ tiêu thụ một ký tự của $word$. Sau khi duyệt xong, bổ sung phần hậu tố còn thiếu nếu chữ cái cuối cùng không phải là `c`.

<!-- thinking:end -->

Ta định nghĩa chuỗi $s$ là `"abc"`, đồng thời dùng các con trỏ $i$ và $j$ lần lượt trỏ đến $s$ và $word$.

Nếu $word[j] \neq s[i]$, ta cần chèn $s[i]$ và tăng đáp án lên $1$; ngược lại, điều đó có nghĩa là $word[j]$ khớp với $s[i]$, nên ta dịch $j$ sang phải một bước.

Sau đó, ta dịch $i$ sang phải một bước, tức là $i = (i + 1) \bmod 3$. Ta tiếp tục các thao tác trên cho đến khi $j$ đi đến cuối chuỗi $word$.

Cuối cùng, ta kiểm tra xem ký tự cuối cùng của $word$ có phải là `'b'` hoặc `'a'` hay không. Nếu có, ta cần chèn `'c'` hoặc `'bc'`, đồng thời tăng đáp án lên $1$ hoặc $2$ rồi trả về kết quả.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $word$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def addMinimum(self, word: str) -> int:
        s = 'abc'
        ans, n = 0, len(word)
        i = j = 0
        while j < n:
            if word[j] != s[i]:
                ans += 1
            else:
                j += 1
            i = (i + 1) % 3
        if word[-1] != 'c':
            ans += 1 if word[-1] == 'b' else 2
        return ans
```

#### Java

```java
class Solution {
    public int addMinimum(String word) {
        String s = "abc";
        int ans = 0, n = word.length();
        for (int i = 0, j = 0; j < n; i = (i + 1) % 3) {
            if (word.charAt(j) != s.charAt(i)) {
                ++ans;
            } else {
                ++j;
            }
        }
        if (word.charAt(n - 1) != 'c') {
            ans += word.charAt(n - 1) == 'b' ? 1 : 2;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int addMinimum(string word) {
        string s = "abc";
        int ans = 0, n = word.size();
        for (int i = 0, j = 0; j < n; i = (i + 1) % 3) {
            if (word[j] != s[i]) {
                ++ans;
            } else {
                ++j;
            }
        }
        if (word[n - 1] != 'c') {
            ans += word[n - 1] == 'b' ? 1 : 2;
        }
        return ans;
    }
};
```

#### Go

```go
func addMinimum(word string) (ans int) {
	s := "abc"
	n := len(word)
	for i, j := 0, 0; j < n; i = (i + 1) % 3 {
		if word[j] != s[i] {
			ans++
		} else {
			j++
		}
	}
	if word[n-1] == 'b' {
		ans++
	} else if word[n-1] == 'a' {
		ans += 2
	}
	return
}
```

#### TypeScript

```ts
function addMinimum(word: string): number {
    const s: string = 'abc';
    let ans: number = 0;
    const n: number = word.length;
    for (let i = 0, j = 0; j < n; i = (i + 1) % 3) {
        if (word[j] !== s[i]) {
            ++ans;
        } else {
            ++j;
        }
    }
    if (word[n - 1] === 'b') {
        ++ans;
    } else if (word[n - 1] === 'a') {
        ans += 2;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
