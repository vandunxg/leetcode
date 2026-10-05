---
comments: true
difficulty: Easy
rating: 1216
source: Weekly Contest 485 Q1
tags:
    - String
    - Simulation
---

<!-- problem:start -->

# [3813. Vowel-Consonant Score](https://leetcode.com/problems/vowel-consonant-score)

[中文文档](/solution/3800-3899/3813.Vowel-Consonant%20Score/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> gồm các chữ cái tiếng Anh viết thường, dấu cách và chữ số.</p>

<p>Gọi <code>v</code> là số lượng nguyên âm trong <code>s</code> và <code>c</code> là số lượng phụ âm trong <code>s</code>.</p>

<p>Một nguyên âm là một trong các chữ cái <code>&#39;a&#39;</code>, <code>&#39;e&#39;</code>, <code>&#39;i&#39;</code>, <code>&#39;o&#39;</code> hoặc <code>&#39;u&#39;</code>, còn mọi chữ cái khác trong bảng chữ cái tiếng Anh được xem là phụ âm.</p>

<p><strong>Điểm số</strong> của chuỗi <code>s</code> được định nghĩa như sau:</p>

<ul>
	<li>Nếu <code>c &gt; 0</code>, <code>score = floor(v / c)</code>, trong đó floor là phép <strong>làm tròn xuống</strong> đến số nguyên gần nhất.</li>
	<li>Nếu không, <code>score = 0</code>.</li>
</ul>

<p>Trả về một số nguyên biểu thị điểm số của chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;cooear&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi <code>s = &quot;cooear&quot;</code> chứa <code>v = 4</code> nguyên âm <code>(&#39;o&#39;, &#39;o&#39;, &#39;e&#39;, &#39;a&#39;)</code> và <code>c = 2</code> phụ âm <code>(&#39;c&#39;, &#39;r&#39;)</code>.</p>

<p>Điểm số là <code>floor(v / c) = floor(4 / 2) = 2</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;axeyizou&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi <code>s = &quot;axeyizou&quot;</code> chứa <code>v = 5</code> nguyên âm <code>(&#39;a&#39;, &#39;e&#39;, &#39;i&#39;, &#39;o&#39;, &#39;u&#39;)</code> và <code>c = 3</code> phụ âm <code>(&#39;x&#39;, &#39;y&#39;, &#39;z&#39;)</code>.</p>

<p>Điểm số là <code>floor(v / c) = floor(5 / 3) = 1</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;au 123&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi <code>s = &quot;au 123&quot;</code> không có phụ âm nào <code>(c = 0)</code>, nên điểm số là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường, dấu cách và chữ số.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Điểm số chỉ phụ thuộc vào số lượng nguyên âm $v$ và phụ âm $c$, không phụ thuộc vào vị trí. Với $|s| \le 100$, ta có thể duyệt chuỗi một lần.
>
> Dấu cách và chữ số không phải là nguyên âm hay phụ âm, nên không được tính vào $c$.
>
> Ta chỉ đếm các chữ cái: tăng $c$ với mỗi chữ cái, tăng $v$ nếu chữ cái đó thuộc $\texttt{aeiou}$, sau đó trừ $v$ khỏi $c$ để thu được số phụ âm thực sự.
>
> Điểm số là $0$ khi $c=0$, ngược lại là $\lfloor v/c \rfloor$.

<!-- thinking:end -->

Ta duyệt chuỗi để đếm số lượng nguyên âm và phụ âm, lần lượt ký hiệu là $v$ và $c$. Cuối cùng, ta tính điểm số theo mô tả của đề bài.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def vowelConsonantScore(self, s: str) -> int:
        v = c = 0
        for ch in s:
            if ch.isalpha():
                c += 1
                if ch in "aeiou":
                    v += 1
        c -= v
        return 0 if c == 0 else v // c
```

#### Java

```java
class Solution {
    public int vowelConsonantScore(String s) {
        int v = 0, c = 0;
        for (int i = 0; i < s.length(); i++) {
            char ch = s.charAt(i);
            if (Character.isLetter(ch)) {
                c++;
                if ("aeiou".indexOf(ch) != -1) {
                    v++;
                }
            }
        }
        c -= v;
        return c == 0 ? 0 : v / c;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int vowelConsonantScore(string s) {
        int v = 0, c = 0;
        for (char ch : s) {
            if (isalpha(ch)) {
                c++;
                if (string("aeiou").find(ch) != string::npos) {
                    v++;
                }
            }
        }
        c -= v;
        return c == 0 ? 0 : v / c;
    }
};
```

#### Go

```go
func vowelConsonantScore(s string) int {
	v, c := 0, 0
	for _, ch := range s {
		if unicode.IsLetter(ch) {
			c++
			if strings.ContainsRune("aeiou", ch) {
				v++
			}
		}
	}
	c -= v
	if c == 0 {
		return 0
	}
	return v / c
}
```

#### TypeScript

```ts
function vowelConsonantScore(s: string): number {
    let [v, c] = [0, 0];
    for (const ch of s) {
        if (/[a-zA-Z]/.test(ch)) {
            c++;
            if ('aeiou'.includes(ch)) {
                v++;
            }
        }
    }
    c -= v;
    return c === 0 ? 0 : Math.floor(v / c);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
