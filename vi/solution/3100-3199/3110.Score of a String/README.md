---
comments: true
difficulty: Easy
rating: 1152
source: Biweekly Contest 128 Q1
tags:
    - String
---

<!-- problem:start -->

# [3110. Score of a String](https://leetcode.com/problems/score-of-a-string)

[中文文档](/solution/3100-3199/3110.Score%20of%20a%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code>. <strong>Điểm số</strong> của một chuỗi được định nghĩa là tổng các giá trị tuyệt đối của hiệu giữa giá trị <strong>ASCII</strong> của các ký tự liền kề.</p>

<p>Hãy trả về <strong>điểm số</strong> của<em> </em><code>s</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;hello&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">13</span></p>

<p><strong>Giải thích:</strong></p>

<p>Giá trị <strong>ASCII</strong> của các ký tự trong <code>s</code> là: <code>&#39;h&#39; = 104</code>, <code>&#39;e&#39; = 101</code>, <code>&#39;l&#39; = 108</code>, <code>&#39;o&#39; = 111</code>. Do đó, điểm số của <code>s</code> là <code>|104 - 101| + |101 - 108| + |108 - 108| + |108 - 111| = 3 + 7 + 0 + 3 = 13</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;zaz&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">50</span></p>

<p><strong>Giải thích:</strong></p>

<p>Giá trị <strong>ASCII</strong> của các ký tự trong <code>s</code> là: <code>&#39;z&#39; = 122</code>, <code>&#39;a&#39; = 97</code>. Do đó, điểm số của <code>s</code> là <code>|122 - 97| + |97 - 122| = 25 + 25 = 50</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Điểm số là tổng các hiệu tuyệt đối giữa mã ASCII của các ký tự liền kề, vốn chỉ cần một lượt duyệt tuyến tính.
>
> Với độ dài đã cho, không cần tiền xử lý. Các cặp ký tự liền kề độc lập với nhau.
>
> Chuyển $s$ thành các code point, tính hiệu tuyệt đối giữa các phần tử lân cận rồi cộng lại. Độ phức tạp thời gian là tuyến tính và không gian phụ là hằng số.

<!-- thinking:end -->

Ta duyệt trực tiếp chuỗi $s$, tính tổng các hiệu tuyệt đối giữa mã ASCII của những ký tự liền kề.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def scoreOfString(self, s: str) -> int:
        return sum(abs(a - b) for a, b in pairwise(map(ord, s)))
```

#### Java

```java
class Solution {
    public int scoreOfString(String s) {
        int ans = 0;
        for (int i = 1; i < s.length(); ++i) {
            ans += Math.abs(s.charAt(i - 1) - s.charAt(i));
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int scoreOfString(string s) {
        int ans = 0;
        for (int i = 1; i < s.size(); ++i) {
            ans += abs(s[i] - s[i - 1]);
        }
        return ans;
    }
};
```

#### Go

```go
func scoreOfString(s string) (ans int) {
	for i := 1; i < len(s); i++ {
		ans += abs(int(s[i-1]) - int(s[i]))
	}
	return
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function scoreOfString(s: string): number {
    let ans = 0;
    for (let i = 1; i < s.length; ++i) {
        ans += Math.abs(s.charCodeAt(i) - s.charCodeAt(i - 1));
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn score_of_string(s: String) -> i32 {
        s.as_bytes()
            .windows(2)
            .map(|w| (w[0] as i32 - w[1] as i32).abs())
            .sum()
    }
}
```

#### C#

```cs
public class Solution {
    public int ScoreOfString(string s) {
        int ans = 0;
        for (int i = 1; i < s.Length; ++i) {
            ans += Math.Abs(s[i] - s[i - 1]);
        }
        return ans;
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param String $s
     * @return Integer
     */
    function scoreOfString($s) {
        $ans = 0;
        $n = strlen($s);
        for ($i = 1; $i < $n; ++$i) {
            $ans += abs(ord($s[$i]) - ord($s[$i - 1]));
        }
        return $ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
