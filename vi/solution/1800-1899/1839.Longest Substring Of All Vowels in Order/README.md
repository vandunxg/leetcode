---
comments: true
difficulty: Medium
rating: 1580
source: Weekly Contest 238 Q3
tags:
    - String
    - Sliding Window
---

<!-- problem:start -->

# [1839. Longest Substring Of All Vowels in Order](https://leetcode.com/problems/longest-substring-of-all-vowels-in-order)

[中文文档](/solution/1800-1899/1839.Longest%20Substring%20Of%20All%20Vowels%20in%20Order/README.md)

## Mô tả

<!-- description:start -->

<p>Một chuỗi được xem là <strong>đẹp</strong> nếu thỏa mãn các điều kiện sau:</p>

<ul>
	<li>Mỗi nguyên âm trong 5 nguyên âm tiếng Anh (<code>&#39;a&#39;</code>, <code>&#39;e&#39;</code>, <code>&#39;i&#39;</code>, <code>&#39;o&#39;</code>, <code>&#39;u&#39;</code>) phải xuất hiện <strong>ít nhất một lần</strong> trong chuỗi.</li>
	<li>Các chữ cái phải được sắp xếp theo <strong>thứ tự bảng chữ cái</strong> (tức là mọi <code>&#39;a&#39;</code> đứng trước <code>&#39;e&#39;</code>, mọi <code>&#39;e&#39;</code> đứng trước <code>&#39;i&#39;</code>, v.v.).</li>
</ul>

<p>Ví dụ, các chuỗi <code>&quot;aeiou&quot;</code> và <code>&quot;aaaaaaeiiiioou&quot;</code> được xem là <strong>đẹp</strong>, nhưng <code>&quot;uaeio&quot;</code>, <code>&quot;aeoiu&quot;</code> và <code>&quot;aaaeeeooo&quot;</code> thì <strong>không đẹp</strong>.</p>

<p>Cho một chuỗi <code>word</code> chỉ gồm các nguyên âm tiếng Anh, hãy trả về <em><strong>độ dài chuỗi con đẹp dài nhất</strong> của </em><code>word</code><em>. Nếu không tồn tại chuỗi con như vậy, trả về </em><code>0</code>.</p>

<p><strong>Chuỗi con</strong> là một dãy ký tự liên tiếp trong chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;aeiaaio<u>aaaaeiiiiouuu</u>ooaauuaeiu&quot;
<strong>Đầu ra:</strong> 13
<b>Giải thích:</b> Chuỗi con đẹp dài nhất trong word là &quot;aaaaeiiiiouuu&quot; có độ dài 13.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;aeeeiiiioooauuu<u>aeiou</u>&quot;
<strong>Đầu ra:</strong> 5
<b>Giải thích:</b> Chuỗi con đẹp dài nhất trong word là &quot;aeiou&quot; có độ dài 5.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;a&quot;
<strong>Đầu ra:</strong> 0
<b>Giải thích:</b> Không có chuỗi con đẹp nào, nên trả về 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= word.length &lt;= 5 * 10<sup>5</sup></code></li>
	<li><code>word</code> gồm các ký tự <code>&#39;a&#39;</code>, <code>&#39;e&#39;</code>, <code>&#39;i&#39;</code>, <code>&#39;o&#39;</code> và <code>&#39;u&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Một chuỗi con đẹp phải chứa $a,e,i,o,u$ theo đúng thứ tự, mỗi nguyên âm ít nhất một lần và không được quay lui. Kiểm tra mọi đoạn có độ phức tạp $O(n^2)$, quá chậm với $n\le 10^5$.
>
> Nén các đoạn liên tiếp có cùng chữ cái thành các cặp $(\textit{letter},\textit{length})$. Một chuỗi đẹp chính xác là năm đoạn liên tiếp tạo thành $\textit{aeiou}$; tổng độ dài của chúng là một ứng viên. Chỉ cần duyệt một lần qua các đoạn đã nén.

<!-- thinking:end -->

Trước tiên, ta biến đổi chuỗi `word`. Ví dụ, với `word="aaaeiouu"`, ta có thể biến đổi thành các phần tử dữ liệu `('a', 3)`, `('e', 1)`, `('i', 1)`, `('o', 1)`, `('u', 2)` và lưu chúng trong mảng `arr`. Phần tử đầu tiên của mỗi phần tử dữ liệu biểu diễn một nguyên âm, còn phần tử thứ hai biểu diễn số lần nguyên âm đó xuất hiện liên tiếp. Có thể thực hiện phép biến đổi này bằng hai con trỏ.

Tiếp theo, ta duyệt mảng `arr`, mỗi lần lấy $5$ phần tử dữ liệu kề nhau và kiểm tra xem các nguyên âm trong đó lần lượt có phải là `'a'`, `'e'`, `'i'`, `'o'`, `'u'` hay không. Nếu đúng, ta tính tổng số lần các nguyên âm xuất hiện trong $5$ phần tử dữ liệu này, đó là độ dài của chuỗi con đẹp hiện tại, rồi cập nhật đáp án lớn nhất.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi `word`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestBeautifulSubstring(self, word: str) -> int:
        arr = []
        n = len(word)
        i = 0
        while i < n:
            j = i
            while j < n and word[j] == word[i]:
                j += 1
            arr.append((word[i], j - i))
            i = j
        ans = 0
        for i in range(len(arr) - 4):
            a, b, c, d, e = arr[i : i + 5]
            if a[0] + b[0] + c[0] + d[0] + e[0] == "aeiou":
                ans = max(ans, a[1] + b[1] + c[1] + d[1] + e[1])
        return ans
```

#### Java

```java
class Solution {
    public int longestBeautifulSubstring(String word) {
        int n = word.length();
        List<Node> arr = new ArrayList<>();
        for (int i = 0; i < n;) {
            int j = i;
            while (j < n && word.charAt(j) == word.charAt(i)) {
                ++j;
            }
            arr.add(new Node(word.charAt(i), j - i));
            i = j;
        }
        int ans = 0;
        for (int i = 0; i < arr.size() - 4; ++i) {
            Node a = arr.get(i), b = arr.get(i + 1), c = arr.get(i + 2), d = arr.get(i + 3),
                 e = arr.get(i + 4);
            if (a.c == 'a' && b.c == 'e' && c.c == 'i' && d.c == 'o' && e.c == 'u') {
                ans = Math.max(ans, a.v + b.v + c.v + d.v + e.v);
            }
        }
        return ans;
    }
}

class Node {
    char c;
    int v;

    Node(char c, int v) {
        this.c = c;
        this.v = v;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestBeautifulSubstring(string word) {
        vector<pair<char, int>> arr;
        int n = word.size();
        for (int i = 0; i < n;) {
            int j = i;
            while (j < n && word[j] == word[i]) ++j;
            arr.push_back({word[i], j - i});
            i = j;
        }
        int ans = 0;
        for (int i = 0; i < (int) arr.size() - 4; ++i) {
            auto& [a, v1] = arr[i];
            auto& [b, v2] = arr[i + 1];
            auto& [c, v3] = arr[i + 2];
            auto& [d, v4] = arr[i + 3];
            auto& [e, v5] = arr[i + 4];
            if (a == 'a' && b == 'e' && c == 'i' && d == 'o' && e == 'u') {
                ans = max(ans, v1 + v2 + v3 + v4 + v5);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func longestBeautifulSubstring(word string) (ans int) {
	arr := []pair{}
	n := len(word)
	for i := 0; i < n; {
		j := i
		for j < n && word[j] == word[i] {
			j++
		}
		arr = append(arr, pair{word[i], j - i})
		i = j
	}
	for i := 0; i < len(arr)-4; i++ {
		a, b, c, d, e := arr[i], arr[i+1], arr[i+2], arr[i+3], arr[i+4]
		if a.c == 'a' && b.c == 'e' && c.c == 'i' && d.c == 'o' && e.c == 'u' {
			ans = max(ans, a.v+b.v+c.v+d.v+e.v)
		}
	}
	return
}

type pair struct {
	c byte
	v int
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
