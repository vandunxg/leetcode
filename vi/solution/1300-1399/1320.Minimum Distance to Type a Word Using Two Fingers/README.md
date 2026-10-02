---
comments: true
difficulty: Hard
rating: 2027
source: Weekly Contest 171 Q4
tags:
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [1320. Minimum Distance to Type a Word Using Two Fingers](https://leetcode.com/problems/minimum-distance-to-type-a-word-using-two-fingers)

[中文文档](/solution/1300-1399/1320.Minimum%20Distance%20to%20Type%20a%20Word%20Using%20Two%20Fingers/README.md)

## Mô tả

<!-- description:start -->

<img height="172" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1320.Minimum%20Distance%20to%20Type%20a%20Word%20Using%20Two%20Fingers/images/image.png" width="284" />
<p>Bạn có bố cục bàn phím như hình trên trong mặt phẳng <strong>X-Y</strong>, mỗi chữ cái tiếng Anh viết hoa nằm tại một tọa độ.</p>

<ul>
	<li>Ví dụ, chữ cái <code>&#39;A&#39;</code> nằm tại tọa độ <code>(0, 0)</code>, <code>&#39;B&#39;</code> nằm tại <code>(0, 1)</code>, <code>&#39;P&#39;</code> nằm tại <code>(2, 3)</code> và <code>&#39;Z&#39;</code> nằm tại <code>(4, 1)</code>.</li>
</ul>

<p>Cho chuỗi <code>word</code>, hãy trả về <em>tổng <strong>khoảng cách</strong> nhỏ nhất để gõ chuỗi bằng hai ngón tay</em>.</p>

<p><strong>Khoảng cách</strong> giữa hai tọa độ <code>(x<sub>1</sub>, y<sub>1</sub>)</code> và <code>(x<sub>2</sub>, y<sub>2</sub>)</code> là <code>|x<sub>1</sub> - x<sub>2</sub>| + |y<sub>1</sub> - y<sub>2</sub>|</code>.</p>

<p><strong>Lưu ý</strong> rằng vị trí ban đầu của hai ngón tay được xem là miễn phí, không tính vào tổng khoảng cách. Hai ngón tay cũng không nhất thiết phải bắt đầu ở chữ cái đầu tiên hoặc hai chữ cái đầu tiên.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;CAKE&quot;
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Một cách gõ &quot;CAKE&quot; tối ưu bằng hai ngón tay là: 
Ngón tay 1 đặt tại chữ &#39;C&#39; -&gt; chi phí = 0 
Ngón tay 1 đặt tại chữ &#39;A&#39; -&gt; chi phí = khoảng cách từ chữ &#39;C&#39; đến chữ &#39;A&#39; = 2 
Ngón tay 2 đặt tại chữ &#39;K&#39; -&gt; chi phí = 0 
Ngón tay 2 đặt tại chữ &#39;E&#39; -&gt; chi phí = khoảng cách từ chữ &#39;K&#39; đến chữ &#39;E&#39; = 1 
Tổng khoảng cách = 3
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;HAPPY&quot;
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Một cách gõ &quot;HAPPY&quot; tối ưu bằng hai ngón tay là:
Ngón tay 1 đặt tại chữ &#39;H&#39; -&gt; chi phí = 0
Ngón tay 1 đặt tại chữ &#39;A&#39; -&gt; chi phí = khoảng cách từ chữ &#39;H&#39; đến chữ &#39;A&#39; = 2
Ngón tay 2 đặt tại chữ &#39;P&#39; -&gt; chi phí = 0
Ngón tay 2 đặt tại chữ &#39;P&#39; -&gt; chi phí = khoảng cách từ chữ &#39;P&#39; đến chữ &#39;P&#39; = 0
Ngón tay 1 đặt tại chữ &#39;Y&#39; -&gt; chi phí = khoảng cách từ chữ &#39;A&#39; đến chữ &#39;Y&#39; = 4
Tổng khoảng cách = 6
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= word.length &lt;= 300</code></li>
	<li><code>word</code> chỉ gồm các chữ cái tiếng Anh viết hoa.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Dynamic Programming

<!-- thinking:start -->

> **Tư duy**
>
> Hai ngón tay gõ $\textit{word}$ trên bàn phím $6\times 5$; mỗi lần di chuyển tốn khoảng cách Manhattan, còn chữ cái đầu tiên được gõ miễn phí. Với $n \le 300$, thử mọi cách dùng ngón tay sẽ có độ phức tạp tăng theo hàm mũ. Sau khi gõ chữ thứ $i$, ta chỉ cần biết vị trí hiện tại của hai ngón tay.
>
> Gọi $f[i][j][k]$ là chi phí nhỏ nhất sau khi gõ $\textit{word}[i]$, với hai ngón tay lần lượt ở vị trí $j$ và $k$. Chữ đầu tiên được một trong hai ngón tay gõ với chi phí $0$. Ở mỗi bước sau, ta di chuyển đúng một ngón tay đến chữ tiếp theo và cộng khoảng cách trên bàn phím. Đáp án là giá trị nhỏ nhất ở lớp cuối.

<!-- thinking:end -->

Ta định nghĩa $f[i][j][k]$ là khoảng cách nhỏ nhất sau khi gõ đến $\textit{word}[i]$, với ngón tay 1 ở vị trí $j$ và ngón tay 2 ở vị trí $k$. Các vị trí $j$ và $k$ là số tương ứng với các chữ cái, nằm trong khoảng $[0,..25]$. Ban đầu, $f[i][j][k] = \infty$.

Ta định nghĩa hàm $\textit{dist}(a, b)$ là khoảng cách giữa hai vị trí $a$ và $b$, tức $\textit{dist}(a, b) = |\frac{a}{6} - \frac{b}{6}| + |a \bmod 6 - b \bmod 6|$.

Tiếp theo, xét việc gõ $\textit{word}[0]$, tức trường hợp chỉ có một chữ cái. Có hai lựa chọn:

- Ngón tay 1 ở vị trí của $\textit{word}[0]$, còn ngón tay 2 có thể ở vị trí bất kỳ. Khi đó, $f[0][\textit{word}[0]][k] = 0$, với $k \in [0,..25]$.
- Ngón tay 2 ở vị trí của $\textit{word}[0]$, còn ngón tay 1 có thể ở vị trí bất kỳ. Khi đó, $f[0][k][\textit{word}[0]] = 0$, với $k \in [0,..25]$.

Sau đó, xét lần lượt các ký tự $\textit{word}[1,..n-1]$. Gọi vị trí của chữ cái trước và chữ cái hiện tại lần lượt là $a$ và $b$. Ta xét các trường hợp sau:

Nếu ngón tay 1 hiện ở vị trí $b$, ta duyệt vị trí $j$ của ngón tay 2. Nếu vị trí trước đó $a$ cũng là vị trí của ngón tay 1 thì $f[i][b][j] = \min(f[i][b][j], f[i-1][a][j] + \textit{dist}(a, b))$. Nếu ngón tay 2 đang ở vị trí trước đó $a$, tức $j = a$, ta duyệt vị trí $k$ trước đó của ngón tay 1. Khi đó, $f[i][b][j] = \min(f[i][b][j], f[i-1][k][a] + \textit{dist}(k, b))$.

Tương tự, nếu ngón tay 2 hiện ở vị trí $b$, ta duyệt vị trí $j$ của ngón tay 1. Nếu vị trí trước đó $a$ cũng là vị trí của ngón tay 2 thì $f[i][j][b] = \min(f[i][j][b], f[i-1][j][a] + \textit{dist}(a, b))$. Nếu ngón tay 1 đang ở vị trí trước đó $a$, tức $j = a$, ta duyệt vị trí $k$ trước đó của ngón tay 2. Khi đó, $f[i][j][b] = \min(f[i][j][b], f[i-1][a][k] + \textit{dist}(k, b))$.

Cuối cùng, ta duyệt các vị trí của hai ngón tay sau khi gõ xong ký tự cuối và lấy giá trị nhỏ nhất làm đáp án.

Độ phức tạp thời gian và không gian đều là $O(n \times |\Sigma|^2)$. Trong đó, $n$ là độ dài chuỗi $\textit{word}$, còn $|\Sigma|$ là kích thước bảng chữ cái, bằng $26$ trong bài này.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumDistance(self, word: str) -> int:
        def dist(a: int, b: int) -> int:
            x1, y1 = divmod(a, 6)
            x2, y2 = divmod(b, 6)
            return abs(x1 - x2) + abs(y1 - y2)

        n = len(word)
        f = [[[inf] * 26 for _ in range(26)] for _ in range(n)]
        for j in range(26):
            f[0][ord(word[0]) - ord('A')][j] = 0
            f[0][j][ord(word[0]) - ord('A')] = 0
        for i in range(1, n):
            a, b = ord(word[i - 1]) - ord('A'), ord(word[i]) - ord('A')
            d = dist(a, b)
            for j in range(26):
                f[i][b][j] = min(f[i][b][j], f[i - 1][a][j] + d)
                f[i][j][b] = min(f[i][j][b], f[i - 1][j][a] + d)
                if j == a:
                    for k in range(26):
                        t = dist(k, b)
                        f[i][b][j] = min(f[i][b][j], f[i - 1][k][a] + t)
                        f[i][j][b] = min(f[i][j][b], f[i - 1][a][k] + t)
        a = min(f[n - 1][ord(word[-1]) - ord('A')])
        b = min(f[n - 1][j][ord(word[-1]) - ord('A')] for j in range(26))
        return int(min(a, b))
```

#### Java

```java
class Solution {
    public int minimumDistance(String word) {
        int n = word.length();
        final int inf = 1 << 30;
        int[][][] f = new int[n][26][26];
        for (int[][] g : f) {
            for (int[] h : g) {
                Arrays.fill(h, inf);
            }
        }
        for (int j = 0; j < 26; ++j) {
            f[0][word.charAt(0) - 'A'][j] = 0;
            f[0][j][word.charAt(0) - 'A'] = 0;
        }
        for (int i = 1; i < n; ++i) {
            int a = word.charAt(i - 1) - 'A';
            int b = word.charAt(i) - 'A';
            int d = dist(a, b);
            for (int j = 0; j < 26; ++j) {
                f[i][b][j] = Math.min(f[i][b][j], f[i - 1][a][j] + d);
                f[i][j][b] = Math.min(f[i][j][b], f[i - 1][j][a] + d);
                if (j == a) {
                    for (int k = 0; k < 26; ++k) {
                        int t = dist(k, b);
                        f[i][b][j] = Math.min(f[i][b][j], f[i - 1][k][a] + t);
                        f[i][j][b] = Math.min(f[i][j][b], f[i - 1][a][k] + t);
                    }
                }
            }
        }
        int ans = inf;
        for (int j = 0; j < 26; ++j) {
            ans = Math.min(ans, f[n - 1][j][word.charAt(n - 1) - 'A']);
            ans = Math.min(ans, f[n - 1][word.charAt(n - 1) - 'A'][j]);
        }
        return ans;
    }

    private int dist(int a, int b) {
        int x1 = a / 6, y1 = a % 6;
        int x2 = b / 6, y2 = b % 6;
        return Math.abs(x1 - x2) + Math.abs(y1 - y2);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumDistance(string word) {
        int n = word.size();
        const int inf = 1 << 30;
        vector<vector<vector<int>>> f(n, vector<vector<int>>(26, vector<int>(26, inf)));
        for (int j = 0; j < 26; ++j) {
            f[0][word[0] - 'A'][j] = 0;
            f[0][j][word[0] - 'A'] = 0;
        }
        for (int i = 1; i < n; ++i) {
            int a = word[i - 1] - 'A';
            int b = word[i] - 'A';
            int d = dist(a, b);
            for (int j = 0; j < 26; ++j) {
                f[i][b][j] = min(f[i][b][j], f[i - 1][a][j] + d);
                f[i][j][b] = min(f[i][j][b], f[i - 1][j][a] + d);
                if (j == a) {
                    for (int k = 0; k < 26; ++k) {
                        int t = dist(k, b);
                        f[i][b][j] = min(f[i][b][j], f[i - 1][k][a] + t);
                        f[i][j][b] = min(f[i][j][b], f[i - 1][a][k] + t);
                    }
                }
            }
        }
        int ans = inf;
        for (int j = 0; j < 26; ++j) {
            ans = min(ans, f[n - 1][word[n - 1] - 'A'][j]);
            ans = min(ans, f[n - 1][j][word[n - 1] - 'A']);
        }
        return ans;
    }

    int dist(int a, int b) {
        int x1 = a / 6, y1 = a % 6;
        int x2 = b / 6, y2 = b % 6;
        return abs(x1 - x2) + abs(y1 - y2);
    }
};
```

#### Go

```go
func minimumDistance(word string) int {
	n := len(word)
	f := make([][26][26]int, n)
	const inf = 1 << 30
	for i := range f {
		for j := range f[i] {
			for k := range f[i][j] {
				f[i][j][k] = inf
			}
		}
	}
	for j := range f[0] {
		f[0][word[0]-'A'][j] = 0
		f[0][j][word[0]-'A'] = 0
	}
	dist := func(a, b int) int {
		x1, y1 := a/6, a%6
		x2, y2 := b/6, b%6
		return abs(x1-x2) + abs(y1-y2)
	}
	for i := 1; i < n; i++ {
		a, b := int(word[i-1]-'A'), int(word[i]-'A')
		d := dist(a, b)
		for j := 0; j < 26; j++ {
			f[i][b][j] = min(f[i][b][j], f[i-1][a][j]+d)
			f[i][j][b] = min(f[i][j][b], f[i-1][j][a]+d)
			if j == a {
				for k := 0; k < 26; k++ {
					t := dist(k, b)
					f[i][b][j] = min(f[i][b][j], f[i-1][k][a]+t)
					f[i][j][b] = min(f[i][j][b], f[i-1][a][k]+t)
				}
			}
		}
	}
	ans := inf
	for j := 0; j < 26; j++ {
		ans = min(ans, f[n-1][word[n-1]-'A'][j])
		ans = min(ans, f[n-1][j][word[n-1]-'A'])
	}
	return ans
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
