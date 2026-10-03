---
comments: true
difficulty: Easy
rating: 1364
source: Biweekly Contest 59 Q1
tags:
    - Greedy
    - String
---

<!-- problem:start -->

# [1974. Minimum Time to Type Word Using Special Typewriter](https://leetcode.com/problems/minimum-time-to-type-word-using-special-typewriter)

[中文文档](/solution/1900-1999/1974.Minimum%20Time%20to%20Type%20Word%20Using%20Special%20Typewriter/README.md)

## Mô tả

<!-- description:start -->

<p>Có một máy đánh chữ đặc biệt với các chữ cái tiếng Anh viết thường từ <code>&#39;a&#39;</code> đến <code>&#39;z&#39;</code> được sắp xếp thành một <strong>vòng tròn</strong> cùng một <strong>con trỏ</strong>. Một ký tự <strong>chỉ</strong> có thể được gõ khi con trỏ đang chỉ vào ký tự đó. <strong>Ban đầu</strong>, con trỏ chỉ vào ký tự <code>&#39;a&#39;</code>.</p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1974.Minimum%20Time%20to%20Type%20Word%20Using%20Special%20Typewriter/images/chart.jpg" style="width: 530px; height: 410px;" />
<p>Mỗi giây, bạn có thể thực hiện một trong các thao tác sau:</p>

<ul>
	<li>Di chuyển con trỏ một ký tự theo <strong>ngược chiều kim đồng hồ</strong> hoặc <strong>theo chiều kim đồng hồ</strong>.</li>
	<li>Gõ ký tự mà con trỏ <strong>hiện đang</strong> chỉ vào.</li>
</ul>

<p>Cho một chuỗi <code>word</code>, hãy trả về số giây <strong>nhỏ nhất</strong> cần để gõ hết các ký tự trong <code>word</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;abc&quot;
<strong>Đầu ra:</strong> 5
<strong>Giải thích:
</strong>Các ký tự được gõ như sau:
- Gõ ký tự &#39;a&#39; trong 1 giây vì ban đầu con trỏ đang ở &#39;a&#39;.
- Di chuyển con trỏ theo chiều kim đồng hồ đến &#39;b&#39; trong 1 giây.
- Gõ ký tự &#39;b&#39; trong 1 giây.
- Di chuyển con trỏ theo chiều kim đồng hồ đến &#39;c&#39; trong 1 giây.
- Gõ ký tự &#39;c&#39; trong 1 giây.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;bza&quot;
<strong>Đầu ra:</strong> 7
<strong>Giải thích:
</strong>Các ký tự được gõ như sau:
- Di chuyển con trỏ theo chiều kim đồng hồ đến &#39;b&#39; trong 1 giây.
- Gõ ký tự &#39;b&#39; trong 1 giây.
- Di chuyển con trỏ ngược chiều kim đồng hồ đến &#39;z&#39; trong 2 giây.
- Gõ ký tự &#39;z&#39; trong 1 giây.
- Di chuyển con trỏ theo chiều kim đồng hồ đến &#39;a&#39; trong 1 giây.
- Gõ ký tự &#39;a&#39; trong 1 giây.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;zjpc&quot;
<strong>Đầu ra:</strong> 34
<strong>Giải thích:</strong>
Các ký tự được gõ như sau:
- Di chuyển con trỏ ngược chiều kim đồng hồ đến &#39;z&#39; trong 1 giây.
- Gõ ký tự &#39;z&#39; trong 1 giây.
- Di chuyển con trỏ theo chiều kim đồng hồ đến &#39;j&#39; trong 10 giây.
- Gõ ký tự &#39;j&#39; trong 1 giây.
- Di chuyển con trỏ theo chiều kim đồng hồ đến &#39;p&#39; trong 6 giây.
- Gõ ký tự &#39;p&#39; trong 1 giây.
- Di chuyển con trỏ ngược chiều kim đồng hồ đến &#39;c&#39; trong 13 giây.
- Gõ ký tự &#39;c&#39; trong 1 giây.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= word.length &lt;= 100</code></li>
	<li><code>word</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Con trỏ di chuyển trên bảng chữ cái dạng vòng tròn và mỗi ký tự vẫn tốn một giây để gõ. Vì vậy, luôn chọn cung ngắn hơn.
>
> Từ `'a'`, cộng thêm $\min(|c-a|,26-|c-a|)$ giữa từng cặp ký tự liên tiếp, sau đó cộng thêm độ dài của chuỗi.

<!-- thinking:end -->

Ta khởi tạo biến đáp án $\textit{ans}$ bằng độ dài của chuỗi, vì cần ít nhất $\textit{ans}$ giây để gõ chuỗi.

Tiếp theo, ta duyệt qua chuỗi. Với mỗi ký tự, ta tính khoảng cách nhỏ nhất giữa ký tự hiện tại và ký tự trước đó, rồi cộng khoảng cách này vào đáp án. Sau đó, ta cập nhật ký tự hiện tại thành ký tự trước đó và tiếp tục duyệt.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minTimeToType(self, word: str) -> int:
        ans, a = len(word), ord("a")
        for c in map(ord, word):
            d = abs(c - a)
            ans += min(d, 26 - d)
            a = c
        return ans
```

#### Java

```java
class Solution {
    public int minTimeToType(String word) {
        int ans = word.length();
        char a = 'a';
        for (char c : word.toCharArray()) {
            int d = Math.abs(a - c);
            ans += Math.min(d, 26 - d);
            a = c;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minTimeToType(string word) {
        int ans = word.length();
        char a = 'a';
        for (char c : word) {
            int d = abs(a - c);
            ans += min(d, 26 - d);
            a = c;
        }
        return ans;
    }
};
```

#### Go

```go
func minTimeToType(word string) int {
	ans := len(word)
	a := rune('a')
	for _, c := range word {
		d := int(max(a-c, c-a))
		ans += min(d, 26-d)
		a = c
	}
	return ans
}
```

#### TypeScript

```ts
function minTimeToType(word: string): number {
    let a = 'a'.charCodeAt(0);
    let ans = word.length;
    for (const c of word) {
        const d = Math.abs(c.charCodeAt(0) - a);
        ans += Math.min(d, 26 - d);
        a = c.charCodeAt(0);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
