---
comments: true
difficulty: Hard
tags:
    - Greedy
    - Array
    - Math
    - Two Pointers
    - String
    - Game Theory
---

<!-- problem:start -->

# [2868. The Wording Game 🔒](https://leetcode.com/problems/the-wording-game)

[中文文档](/solution/2800-2899/2868.The%20Wording%20Game/README.md)

## Mô tả

<!-- description:start -->

<p>Alice và Bob lần lượt có một mảng chuỗi <code>a</code> và <code>b</code> đã được sắp xếp theo <strong>thứ tự từ điển</strong>.</p>

<p>Họ chơi một trò chơi với từ theo các luật sau:</p>

<ul>
	<li>Trong mỗi lượt, người chơi hiện tại phải chọn một từ trong danh sách của mình sao cho từ mới <strong>lớn hơn kề</strong> từ cuối cùng đã được chọn; sau đó đến lượt người chơi còn lại.</li>
	<li>Nếu không thể chọn từ nào trong lượt của mình, người chơi đó thua.</li>
</ul>

<p>Alice bắt đầu trò chơi bằng cách chọn từ <strong>theo thứ tự từ điển </strong><strong>nhỏ nhất </strong> của mình.</p>

<p>Cho <code>a</code> và <code>b</code>, hãy trả về <code>true</code> <em>nếu Alice có thể chiến thắng khi biết cả hai người chơi đều chơi tối ưu, và</em> <code>false</code> <em>nếu ngược lại.</em></p>

<p>Một từ <code>w</code> được gọi là <strong>lớn hơn kề</strong> một từ <code>z</code> nếu thỏa mãn các điều kiện sau:</p>

<ul>
	<li><code>w</code> <strong>lớn hơn theo thứ tự từ điển</strong> <code>z</code>.</li>
	<li>Nếu <code>w<sub>1</sub></code> là chữ cái đầu tiên của <code>w</code> và <code>z<sub>1</sub></code> là chữ cái đầu tiên của <code>z</code>, thì <code>w<sub>1</sub></code> phải <strong>bằng</strong> <code>z<sub>1</sub></code> hoặc là <strong>chữ cái đứng ngay sau</strong> <code>z<sub>1</sub></code> trong bảng chữ cái.</li>
	<li>Ví dụ, từ <code>&quot;care&quot;</code> lớn hơn kề <code>&quot;book&quot;</code> và <code>&quot;car&quot;</code>, nhưng không lớn hơn kề <code>&quot;ant&quot;</code> hoặc <code>&quot;cook&quot;</code>.</li>
</ul>

<p>Một chuỗi <code>s</code> được gọi là <b>theo thứ tự từ điển </b><strong>lớn hơn</strong> một chuỗi <code>t</code> nếu tại vị trí đầu tiên mà <code>s</code> và <code>t</code> khác nhau, chuỗi <code>s</code> có một chữ cái xuất hiện sau chữ cái tương ứng trong <code>t</code> trong bảng chữ cái. Nếu <code>min(s.length, t.length)</code> ký tự đầu tiên không khác nhau, thì chuỗi dài hơn sẽ lớn hơn theo thứ tự từ điển.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> a = [&quot;avokado&quot;,&quot;dabar&quot;], b = [&quot;brazil&quot;]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Alice phải bắt đầu trò chơi bằng từ &quot;avokado&quot; vì đây là từ nhỏ nhất của cô ấy, sau đó Bob chọn từ duy nhất của mình là &quot;brazil&quot;, vì chữ cái đầu tiên của nó, &#39;b&#39;, đứng ngay sau chữ cái đầu tiên, &#39;a&#39;, của từ Alice.
Alice không thể chọn từ nào vì chữ cái đầu tiên của từ duy nhất còn lại không bằng &#39;b&#39; hoặc chữ cái đứng ngay sau &#39;b&#39;, là &#39;c&#39;.
Vì vậy, Alice thua và trò chơi kết thúc.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> a = [&quot;ananas&quot;,&quot;atlas&quot;,&quot;banana&quot;], b = [&quot;albatros&quot;,&quot;cikla&quot;,&quot;nogomet&quot;]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Alice phải bắt đầu trò chơi bằng từ &quot;ananas&quot;.
Bob không thể chọn từ nào vì từ duy nhất của anh ấy bắt đầu bằng chữ cái &#39;a&#39; hoặc &#39;b&#39; là &quot;albatros&quot;, nhưng từ này nhỏ hơn từ của Alice.
Vì vậy, Alice thắng và trò chơi kết thúc.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> a = [&quot;hrvatska&quot;,&quot;zastava&quot;], b = [&quot;bijeli&quot;,&quot;galeb&quot;]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Alice phải bắt đầu trò chơi bằng từ &quot;hrvatska&quot;.
Bob không thể chọn từ nào vì chữ cái đầu tiên của cả hai từ của anh ấy đều nhỏ hơn chữ cái đầu tiên của từ Alice, là &#39;h&#39;.
Vì vậy, Alice thắng và trò chơi kết thúc.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= a.length, b.length &lt;= 10<sup>5</sup></code></li>
	<li><code>a[i]</code> và <code>b[i]</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
	<li><code>a</code> và <code>b</code> đã được <strong>sắp xếp theo thứ tự từ điển</strong>.</li>
	<li>Tất cả các từ trong <code>a</code> và <code>b</code> kết hợp lại đều <strong>khác nhau</strong>.</li>
	<li>Tổng độ dài của tất cả các từ trong <code>a</code> và <code>b</code> không vượt quá <code>10<sup>6</sup></code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Cả hai danh sách từ đều đã được sắp xếp. Một lượt chọn hợp lệ là chọn một từ có cùng chữ cái đầu và lớn hơn chuỗi hiện tại, hoặc có chữ cái đầu lớn hơn đúng một chữ cái. Mỗi người chơi luôn chọn từ hợp lệ đầu tiên còn lại, vì vậy ta có thể dùng hai con trỏ để mô phỏng trò chơi mà không cần tìm kiếm.

<!-- thinking:end -->

Ta dùng $k$ để ghi nhận lượt của ai, trong đó $k=0$ nghĩa là đến lượt Alice, còn $k=1$ nghĩa là đến lượt Bob. Ta dùng $i$ để ghi nhận chỉ số của Alice, $j$ để ghi nhận chỉ số của Bob và $w$ để ghi nhận từ hiện tại. Ban đầu, ta đặt $i=1$, $j=0$ và $w=a[0]$.

Ta lặp lại các bước sau:

Nếu $k=1$, ta kiểm tra xem $j$ có bằng độ dài của $b$ hay không. Nếu có, Alice thắng và ta trả về $true$. Ngược lại, ta kiểm tra xem chữ cái đầu tiên của $b[j]$ có bằng chữ cái đầu tiên của $w$ hay không. Nếu có, ta kiểm tra xem $b[j]$ có lớn hơn $w$ hay chữ cái đầu tiên của $b[j]$ có lớn hơn chữ cái đầu tiên của $w$ đúng một chữ cái hay không. Nếu một trong hai điều kiện đúng, Bob có thể chọn từ thứ $j$. Ta đặt $w=b[j]$ và chuyển lượt của $k$. Nếu không, Bob không thể chọn từ thứ $j$, nên ta tăng $j$.

Nếu $k=0$, ta kiểm tra xem $i$ có bằng độ dài của $a$ hay không. Nếu có, Bob thắng và ta trả về $false$. Ngược lại, ta kiểm tra xem chữ cái đầu tiên của $a[i]$ có bằng chữ cái đầu tiên của $w$ hay không. Nếu có, ta kiểm tra xem $a[i]$ có lớn hơn $w$ hay chữ cái đầu tiên của $a[i]$ có lớn hơn chữ cái đầu tiên của $w$ đúng một chữ cái hay không. Nếu một trong hai điều kiện đúng, Alice có thể chọn từ thứ $i$. Ta đặt $w=a[i]$ và chuyển lượt của $k$. Nếu không, Alice không thể chọn từ thứ $i$, nên ta tăng $i$.

Độ phức tạp thời gian là $O(m+n)$, trong đó $m$ và $n$ lần lượt là độ dài của các mảng $a$ và $b$. Ta chỉ cần duyệt qua mỗi mảng một lần. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canAliceWin(self, a: List[str], b: List[str]) -> bool:
        i, j, k = 1, 0, 1
        w = a[0]
        while 1:
            if k:
                if j == len(b):
                    return True
                if (b[j][0] == w[0] and b[j] > w) or ord(b[j][0]) - ord(w[0]) == 1:
                    w = b[j]
                    k ^= 1
                j += 1
            else:
                if i == len(a):
                    return False
                if (a[i][0] == w[0] and a[i] > w) or ord(a[i][0]) - ord(w[0]) == 1:
                    w = a[i]
                    k ^= 1
                i += 1
```

#### Java

```java
class Solution {
    public boolean canAliceWin(String[] a, String[] b) {
        int i = 1, j = 0;
        boolean k = true;
        String w = a[0];
        while (true) {
            if (k) {
                if (j == b.length) {
                    return true;
                }
                if ((b[j].charAt(0) == w.charAt(0) && w.compareTo(b[j]) < 0)
                    || b[j].charAt(0) - w.charAt(0) == 1) {
                    w = b[j];
                    k = !k;
                }
                ++j;
            } else {
                if (i == a.length) {
                    return false;
                }
                if ((a[i].charAt(0) == w.charAt(0) && w.compareTo(a[i]) < 0)
                    || a[i].charAt(0) - w.charAt(0) == 1) {
                    w = a[i];
                    k = !k;
                }
                ++i;
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canAliceWin(vector<string>& a, vector<string>& b) {
        int i = 1, j = 0, k = 1;
        string w = a[0];
        while (1) {
            if (k) {
                if (j == b.size()) {
                    return true;
                }
                if ((b[j][0] == w[0] && w < b[j]) || b[j][0] - w[0] == 1) {
                    w = b[j];
                    k ^= 1;
                }
                ++j;
            } else {
                if (i == a.size()) {
                    return false;
                }
                if ((a[i][0] == w[0] && w < a[i]) || a[i][0] - w[0] == 1) {
                    w = a[i];
                    k ^= 1;
                }
                ++i;
            }
        }
    }
};
```

#### Go

```go
func canAliceWin(a []string, b []string) bool {
	i, j, k := 1, 0, 1
	w := a[0]
	for {
		if k&1 == 1 {
			if j == len(b) {
				return true
			}
			if (b[j][0] == w[0] && w < b[j]) || b[j][0]-w[0] == 1 {
				w = b[j]
				k ^= 1
			}
			j++
		} else {
			if i == len(a) {
				return false
			}
			if (a[i][0] == w[0] && w < a[i]) || a[i][0]-w[0] == 1 {
				w = a[i]
				k ^= 1
			}
			i++
		}
	}
}
```

#### TypeScript

```ts
function canAliceWin(a: string[], b: string[]): boolean {
    let [i, j, k] = [1, 0, 1];
    let w = a[0];
    while (1) {
        if (k) {
            if (j === b.length) {
                return true;
            }
            if ((b[j][0] === w[0] && w < b[j]) || b[j].charCodeAt(0) - w.charCodeAt(0) === 1) {
                w = b[j];
                k ^= 1;
            }
            ++j;
        } else {
            if (i === a.length) {
                return false;
            }
            if ((a[i][0] === w[0] && w < a[i]) || a[i].charCodeAt(0) - w.charCodeAt(0) === 1) {
                w = a[i];
                k ^= 1;
            }
            ++i;
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
