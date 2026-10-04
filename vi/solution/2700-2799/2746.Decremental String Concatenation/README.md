---
comments: true
difficulty: Medium
rating: 2126
source: Biweekly Contest 107 Q3
tags:
    - Array
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [2746. Decremental String Concatenation](https://leetcode.com/problems/decremental-string-concatenation)

[中文文档](/solution/2700-2799/2746.Decremental%20String%20Concatenation/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>words</code> gồm <code>n</code> chuỗi, được đánh chỉ số từ <strong>0</strong>.</p>

<p>Ta định nghĩa phép <strong>join</strong> <code>join(x, y)</code> giữa hai chuỗi <code>x</code> và <code>y</code> là nối chúng thành <code>xy</code>. Tuy nhiên, nếu ký tự cuối của <code>x</code> bằng ký tự đầu của <code>y</code>, một trong hai ký tự đó sẽ bị <strong>xóa</strong>.</p>

<p>Ví dụ, <code>join(&quot;ab&quot;, &quot;ba&quot;) = &quot;aba&quot;</code> và <code>join(&quot;ab&quot;, &quot;cde&quot;) = &quot;abcde&quot;</code>.</p>

<p>Bạn cần thực hiện <code>n - 1</code> phép <strong>join</strong>. Đặt <code>str<sub>0</sub> = words[0]</code>. Bắt đầu từ <code>i = 1</code> đến <code>i = n - 1</code>, với phép toán thứ <code>i<sup>th</sup></code>, bạn có thể thực hiện một trong hai cách sau:</p>

<ul>
	<li>Đặt <code>str<sub>i</sub> = join(str<sub>i - 1</sub>, words[i])</code></li>
	<li>Đặt <code>str<sub>i</sub> = join(words[i], str<sub>i - 1</sub>)</code></li>
</ul>

<p>Nhiệm vụ của bạn là <strong>tối thiểu hóa</strong> độ dài của <code>str<sub>n - 1</sub></code>.</p>

<p>Trả về <em>một số nguyên biểu thị độ dài nhỏ nhất có thể của</em> <code>str<sub>n - 1</sub></code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;aa&quot;,&quot;ab&quot;,&quot;bc&quot;]
<strong>Đầu ra:</strong> 4
<strong>Giải thích: </strong>Trong ví dụ này, ta có thể thực hiện các phép join theo thứ tự sau để tối thiểu hóa độ dài của str<sub>2</sub>:
str<sub>0</sub> = &quot;aa&quot;
str<sub>1</sub> = join(str<sub>0</sub>, &quot;ab&quot;) = &quot;aab&quot;
str<sub>2</sub> = join(str<sub>1</sub>, &quot;bc&quot;) = &quot;aabc&quot;
Có thể chứng minh rằng độ dài nhỏ nhất có thể của str<sub>2</sub> là 4.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;ab&quot;,&quot;b&quot;]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Trong ví dụ này, str<sub>0</sub> = &quot;ab&quot;, có hai cách để tạo str<sub>1</sub>:
join(str<sub>0</sub>, &quot;b&quot;) = &quot;ab&quot; hoặc join(&quot;b&quot;, str<sub>0</sub>) = &quot;bab&quot;.
Chuỗi đầu tiên có độ dài nhỏ nhất là &quot;ab&quot;. Vì vậy, đáp án là 2.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;aaa&quot;,&quot;c&quot;,&quot;aba&quot;]
<strong>Đầu ra:</strong> 6
<strong>Giải thích: </strong>Trong ví dụ này, ta có thể thực hiện các phép join theo thứ tự sau để tối thiểu hóa độ dài của str<sub>2</sub>:
str<sub>0</sub> = &quot;aaa&quot;
str<sub>1</sub> = join(str<sub>0</sub>, &quot;c&quot;) = &quot;aaac&quot;
str<sub>2</sub> = join(&quot;aba&quot;, str<sub>1</sub>) = &quot;abaaac&quot;
Có thể chứng minh rằng độ dài nhỏ nhất có thể của str<sub>2</sub> là 6.
</pre>

<div class="notranslate" style="all: initial;">&nbsp;</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 1000</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 50</code></li>
	<li>Mỗi ký tự trong <code>words[i]</code> là một chữ cái tiếng Anh viết thường</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Các từ phải lần lượt được thêm vào đầu hoặc cuối; hai ký tự ở vị trí tiếp giáp sẽ gộp thành một nếu giống nhau, và ta muốn độ dài cuối cùng nhỏ nhất. Với mỗi từ có hai lựa chọn và $n\le 1000$, tìm kiếm trực tiếp là không khả thi.
>
> Chi phí trong tương lai chỉ phụ thuộc vào ký tự đầu và cuối hiện tại. $dfs(i,a,b)$ là phần độ dài tăng thêm khi xử lý từ $i$ với hai đầu là $a,b$: khi thêm vào cuối, ta so sánh $s[0]$ với $b$; khi thêm vào đầu, ta so sánh $s[-1]$ với $a$. Ghi nhớ kết quả cho ta $O(n\cdot 26^2)$ trạng thái.

<!-- thinking:end -->

Ta nhận thấy khi nối các chuỗi, ký tự đầu và ký tự cuối của chuỗi sẽ ảnh hưởng đến độ dài chuỗi sau khi nối. Vì vậy, ta xây dựng hàm $dfs(i, a, b)$, biểu thị độ dài nhỏ nhất của chuỗi sau khi nối bắt đầu từ chuỗi thứ $i$, trong đó ký tự đầu của chuỗi đã nối trước đó là $a$ và ký tự cuối là $b$.

Quy trình thực hiện của hàm $dfs(i, a, b)$ như sau:

- Nếu $i = n$, nghĩa là tất cả các chuỗi đã được nối, trả về $0$;
- Ngược lại, ta xét việc nối chuỗi thứ $i$ vào cuối hoặc vào đầu của chuỗi đã nối, thu được độ dài $x$ và $y$ của chuỗi sau khi nối, khi đó $dfs(i, a, b) = \min(x, y) + |words[i]|$.

Để tránh tính toán lặp lại, ta sử dụng phương pháp tìm kiếm có ghi nhớ. Cụ thể, ta dùng một mảng ba chiều $f$ để lưu tất cả giá trị trả về của $dfs(i, a, b)$. Khi cần tính $dfs(i, a, b)$, nếu $f[i][a][b]$ đã được tính, ta trả về ngay $f[i][a][b]$; nếu chưa, ta tính giá trị của $dfs(i, a, b)$ theo công thức truy hồi trên rồi lưu vào $f[i][a][b]$.

Trong hàm chính, ta trực tiếp trả về $|words[0]| + dfs(1, words[0][0], words[0][|words[0]| - 1])$.

Độ phức tạp thời gian là $O(n \times C^2)$ và độ phức tạp không gian là $O(n \times C^2)$, trong đó $C$ biểu thị độ dài lớn nhất của chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimizeConcatenatedLength(self, words: List[str]) -> int:
        @cache
        def dfs(i: int, a: str, b: str) -> int:
            if i >= len(words):
                return 0
            s = words[i]
            x = dfs(i + 1, a, s[-1]) - int(s[0] == b)
            y = dfs(i + 1, s[0], b) - int(s[-1] == a)
            return len(s) + min(x, y)

        return len(words[0]) + dfs(1, words[0][0], words[0][-1])
```

#### Java

```java
class Solution {
    private Integer[][][] f;
    private String[] words;
    private int n;

    public int minimizeConcatenatedLength(String[] words) {
        n = words.length;
        this.words = words;
        f = new Integer[n][26][26];
        return words[0].length()
            + dfs(1, words[0].charAt(0) - 'a', words[0].charAt(words[0].length() - 1) - 'a');
    }

    private int dfs(int i, int a, int b) {
        if (i >= n) {
            return 0;
        }
        if (f[i][a][b] != null) {
            return f[i][a][b];
        }
        String s = words[i];
        int m = s.length();
        int x = dfs(i + 1, a, s.charAt(m - 1) - 'a') - (s.charAt(0) - 'a' == b ? 1 : 0);
        int y = dfs(i + 1, s.charAt(0) - 'a', b) - (s.charAt(m - 1) - 'a' == a ? 1 : 0);
        return f[i][a][b] = m + Math.min(x, y);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimizeConcatenatedLength(vector<string>& words) {
        int n = words.size();
        int f[n][26][26];
        memset(f, 0, sizeof(f));
        function<int(int, int, int)> dfs = [&](int i, int a, int b) {
            if (i >= n) {
                return 0;
            }
            if (f[i][a][b]) {
                return f[i][a][b];
            }
            auto s = words[i];
            int m = s.size();
            int x = dfs(i + 1, a, s[m - 1] - 'a') - (s[0] - 'a' == b);
            int y = dfs(i + 1, s[0] - 'a', b) - (s[m - 1] - 'a' == a);
            return f[i][a][b] = m + min(x, y);
        };
        return words[0].size() + dfs(1, words[0].front() - 'a', words[0].back() - 'a');
    }
};
```

#### Go

```go
func minimizeConcatenatedLength(words []string) int {
	n := len(words)
	f := make([][26][26]int, n)
	var dfs func(i, a, b int) int
	dfs = func(i, a, b int) int {
		if i >= n {
			return 0
		}
		if f[i][a][b] > 0 {
			return f[i][a][b]
		}
		s := words[i]
		m := len(s)
		x := dfs(i+1, a, int(s[m-1]-'a'))
		y := dfs(i+1, int(s[0]-'a'), b)
		if int(s[0]-'a') == b {
			x--
		}
		if int(s[m-1]-'a') == a {
			y--
		}
		f[i][a][b] = m + min(x, y)
		return f[i][a][b]
	}
	return len(words[0]) + dfs(1, int(words[0][0]-'a'), int(words[0][len(words[0])-1]-'a'))
}
```

#### TypeScript

```ts
function minimizeConcatenatedLength(words: string[]): number {
    const n = words.length;
    const f: number[][][] = Array(n)
        .fill(0)
        .map(() =>
            Array(26)
                .fill(0)
                .map(() => Array(26).fill(0)),
        );
    const dfs = (i: number, a: number, b: number): number => {
        if (i >= n) {
            return 0;
        }
        if (f[i][a][b] > 0) {
            return f[i][a][b];
        }
        const s = words[i];
        const m = s.length;
        const x =
            dfs(i + 1, a, s[m - 1].charCodeAt(0) - 97) - (s[0].charCodeAt(0) - 97 === b ? 1 : 0);
        const y =
            dfs(i + 1, s[0].charCodeAt(0) - 97, b) - (s[m - 1].charCodeAt(0) - 97 === a ? 1 : 0);
        return (f[i][a][b] = Math.min(x + m, y + m));
    };
    return (
        words[0].length +
        dfs(1, words[0][0].charCodeAt(0) - 97, words[0][words[0].length - 1].charCodeAt(0) - 97)
    );
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
