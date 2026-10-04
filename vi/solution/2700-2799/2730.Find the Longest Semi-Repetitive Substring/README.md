---
comments: true
difficulty: Medium
rating: 1501
source: Biweekly Contest 106 Q2
tags:
    - String
    - Sliding Window
---

<!-- problem:start -->

# [2730. Find the Longest Semi-Repetitive Substring](https://leetcode.com/problems/find-the-longest-semi-repetitive-substring)

[中文文档](/solution/2700-2799/2730.Find%20the%20Longest%20Semi-Repetitive%20Substring/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi chữ số <code>s</code> chỉ gồm các chữ số từ 0 đến 9.</p>

<p>Một chuỗi được gọi là <strong>bán lặp</strong> nếu có <strong>nhiều nhất</strong> một cặp chữ số giống nhau đứng liền kề. Ví dụ, <code>&quot;0010&quot;</code>, <code>&quot;002020&quot;</code>, <code>&quot;0123&quot;</code>, <code>&quot;2002&quot;</code> và <code>&quot;54944&quot;</code> là các chuỗi bán lặp, trong khi <code>&quot;00101022&quot;</code> (các cặp chữ số giống nhau đứng liền kề là 00 và 22) và <code>&quot;1101234883&quot;</code> (các cặp chữ số giống nhau đứng liền kề là 11 và 88) thì không.</p>

<p>Trả về độ dài của <strong><span data-keyword="substring-nonempty">substring</span> bán lặp dài nhất</strong> của <code>s</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;52233&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Substring bán lặp dài nhất là &quot;5223&quot;. Nếu chọn toàn bộ chuỗi &quot;52233&quot; thì có hai cặp chữ số giống nhau đứng liền kề là 22 và 33, nhưng chỉ được phép có nhiều nhất một cặp.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;5494&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>s</code> là một chuỗi bán lặp.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;1111111&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Substring bán lặp dài nhất là &quot;11&quot;. Nếu chọn substring &quot;111&quot; thì có hai cặp chữ số giống nhau đứng liền kề, nhưng chỉ được phép có nhiều nhất một cặp.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 50</code></li>
	<li><code>&#39;0&#39; &lt;= s[i] &lt;= &#39;9&#39;</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Một chuỗi bán lặp có nhiều nhất một cặp ký tự giống nhau đứng liền kề; ta cần tìm độ dài lớn nhất của chuỗi đó. Việc kiểm tra mọi cặp đầu mút là không khả thi khi $n\le 10^5$.
>
> Di chuyển đầu phải và đếm số cặp ký tự giống nhau đứng liền kề. Khi số lượng vượt quá $1$, tăng đầu trái cho đến khi cửa sổ trở nên hợp lệ. Độ dài lớn nhất của cửa sổ như vậy chính là đáp án.

<!-- thinking:end -->

Ta dùng hai con trỏ để duy trì một đoạn $s[j..i]$ có nhiều nhất một cặp ký tự liền kề bằng nhau, ban đầu $j = 0$, $i = 1$. Khởi tạo đáp án $ans = 1$.

Ta dùng $cnt$ để ghi nhận số cặp ký tự liền kề bằng nhau trong đoạn. Nếu $cnt > 1$, ta cần di chuyển con trỏ trái $j$ cho đến khi $cnt \le 1$. Sau mỗi lần, ta cập nhật đáp án bằng $ans = \max(ans, i - j + 1)$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestSemiRepetitiveSubstring(self, s: str) -> int:
        ans, n = 1, len(s)
        cnt = j = 0
        for i in range(1, n):
            cnt += s[i] == s[i - 1]
            while cnt > 1:
                cnt -= s[j] == s[j + 1]
                j += 1
            ans = max(ans, i - j + 1)
        return ans
```

#### Java

```java
class Solution {
    public int longestSemiRepetitiveSubstring(String s) {
        int ans = 1, n = s.length();
        for (int i = 1, j = 0, cnt = 0; i < n; ++i) {
            cnt += s.charAt(i) == s.charAt(i - 1) ? 1 : 0;
            for (; cnt > 1; ++j) {
                cnt -= s.charAt(j) == s.charAt(j + 1) ? 1 : 0;
            }
            ans = Math.max(ans, i - j + 1);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestSemiRepetitiveSubstring(string s) {
        int ans = 1, n = s.size();
        for (int i = 1, j = 0, cnt = 0; i < n; ++i) {
            cnt += s[i] == s[i - 1] ? 1 : 0;
            for (; cnt > 1; ++j) {
                cnt -= s[j] == s[j + 1] ? 1 : 0;
            }
            ans = max(ans, i - j + 1);
        }
        return ans;
    }
};
```

#### Go

```go
func longestSemiRepetitiveSubstring(s string) (ans int) {
	ans = 1
	for i, j, cnt := 1, 0, 0; i < len(s); i++ {
		if s[i] == s[i-1] {
			cnt++
		}
		for ; cnt > 1; j++ {
			if s[j] == s[j+1] {
				cnt--
			}
		}
		ans = max(ans, i-j+1)
	}
	return
}
```

#### TypeScript

```ts
function longestSemiRepetitiveSubstring(s: string): number {
    const n = s.length;
    let ans = 1;
    for (let i = 1, j = 0, cnt = 0; i < n; ++i) {
        cnt += s[i] === s[i - 1] ? 1 : 0;
        for (; cnt > 1; ++j) {
            cnt -= s[j] === s[j + 1] ? 1 : 0;
        }
        ans = Math.max(ans, i - j + 1);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Hai con trỏ (Tối ưu hóa)

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 có thể phải thu nhỏ cửa sổ nhiều lần và theo dõi giá trị lớn nhất trong quá trình đó. Vì chỉ cần độ dài, mỗi lần vi phạm ta chỉ cần di chuyển đầu trái một lần. Cửa sổ không bao giờ thu nhỏ, và $n-l$ là độ dài hợp lệ lớn nhất.

<!-- thinking:end -->

Vì bài toán chỉ yêu cầu tìm độ dài của substring bán lặp dài nhất, mỗi khi số lượng ký tự giống nhau đứng liền kề trong đoạn vượt quá $1$, ta có thể di chuyển con trỏ trái $l$ một lần, trong khi con trỏ phải $r$ tiếp tục di chuyển sang phải. Điều này đảm bảo độ dài của substring không giảm.

Cuối cùng, đáp án là $n - l$, trong đó $n$ là độ dài chuỗi.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestSemiRepetitiveSubstring(self, s: str) -> int:
        n = len(s)
        cnt = l = 0
        for i in range(1, n):
            cnt += s[i] == s[i - 1]
            if cnt > 1:
                cnt -= s[l] == s[l + 1]
                l += 1
        return n - l
```

#### Java

```java
class Solution {
    public int longestSemiRepetitiveSubstring(String s) {
        int n = s.length();
        int cnt = 0, l = 0;
        for (int i = 1; i < n; ++i) {
            cnt += s.charAt(i) == s.charAt(i - 1) ? 1 : 0;
            if (cnt > 1) {
                cnt -= s.charAt(l) == s.charAt(++l) ? 1 : 0;
            }
        }
        return n - l;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestSemiRepetitiveSubstring(string s) {
        int n = s.length();
        int cnt = 0, l = 0;
        for (int i = 1; i < n; ++i) {
            cnt += s[i] == s[i - 1] ? 1 : 0;
            if (cnt > 1) {
                cnt -= s[l] == s[++l] ? 1 : 0;
            }
        }
        return n - l;
    }
};
```

#### Go

```go
func longestSemiRepetitiveSubstring(s string) (ans int) {
	cnt, l := 0, 0
	for i, c := range s[1:] {
		if byte(c) == s[i] {
			cnt++
		}
		if cnt > 1 {
			if s[l] == s[l+1] {
				cnt--
			}
			l++
		}
	}
	return len(s) - l
}
```

#### TypeScript

```ts
function longestSemiRepetitiveSubstring(s: string): number {
    const n = s.length;
    let [cnt, l] = [0, 0];
    for (let i = 1; i < n; ++i) {
        cnt += s[i] === s[i - 1] ? 1 : 0;
        if (cnt > 1) {
            cnt -= s[l] === s[l + 1] ? 1 : 0;
            ++l;
        }
    }
    return n - l;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
