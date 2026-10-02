---
comments: true
difficulty: Easy
tags:
    - Array
    - String
---

<!-- problem:start -->

# [806. Number of Lines To Write String](https://leetcode.com/problems/number-of-lines-to-write-string)

[中文文档](/solution/0800-0899/0806.Number%20of%20Lines%20To%20Write%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> gồm các chữ cái tiếng Anh viết thường và mảng <code>widths</code> cho biết <strong>độ rộng tính bằng pixel</strong> của từng chữ cái tiếng Anh viết thường. Cụ thể, <code>widths[0]</code> là độ rộng của <code>&#39;a&#39;</code>, <code>widths[1]</code> là độ rộng của <code>&#39;b&#39;</code>, và cứ tiếp tục như vậy.</p>

<p>Bạn cần viết <code>s</code> thành nhiều dòng, trong đó <strong>mỗi dòng không dài quá </strong><code>100</code><strong> pixel</strong>. Bắt đầu từ đầu chuỗi <code>s</code>, viết nhiều chữ cái nhất có thể trên dòng đầu tiên sao cho tổng độ rộng không vượt quá <code>100</code> pixel. Sau đó, tiếp tục từ vị trí đã dừng trong <code>s</code> và viết nhiều chữ cái nhất có thể trên dòng thứ hai. Lặp lại quá trình này cho đến khi viết hết <code>s</code>.</p>

<p>Trả về <em>mảng </em><code>result</code><em> có độ dài 2, trong đó:</em></p>

<ul>
	<li><code>result[0]</code><em> là tổng số dòng.</em></li>
	<li><code>result[1]</code><em> là độ rộng tính bằng pixel của dòng cuối cùng.</em></li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> widths = [10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10], s = &quot;abcdefghijklmnopqrstuvwxyz&quot;
<strong>Đầu ra:</strong> [3,60]
<strong>Giải thích:</strong> Bạn có thể viết s như sau:
abcdefghij  // rộng 100 pixel
klmnopqrst  // rộng 100 pixel
uvwxyz      // rộng 60 pixel
Tổng cộng có 3 dòng, dòng cuối rộng 60 pixel.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> widths = [4,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10,10], s = &quot;bbbcccdddaaa&quot;
<strong>Đầu ra:</strong> [2,4]
<strong>Giải thích:</strong> Bạn có thể viết s như sau:
bbbcccdddaa  // rộng 98 pixel
a            // rộng 4 pixel
Tổng cộng có 2 dòng, dòng cuối rộng 4 pixel.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>widths.length == 26</code></li>
	<li><code>2 &lt;= widths[i] &lt;= 10</code></li>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>s</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Độ rộng của mỗi chữ cái đã biết và mỗi dòng chứa tối đa $100$ pixel. Chuỗi đủ ngắn để mô phỏng bằng một lượt duyệt.
>
> Nếu chữ cái hiện tại không vừa dòng, bắt đầu dòng mới với độ rộng của chữ cái đó. Trả về số dòng và độ rộng đã dùng ở dòng cuối.

<!-- thinking:end -->

Ta định nghĩa hai biến `lines` và `last`, lần lượt biểu diễn số dòng và độ rộng của dòng cuối. Ban đầu, `lines = 1` và `last = 0`.

Ta duyệt chuỗi $s$. Với mỗi ký tự $c$, ta tính độ rộng $w$ của nó. Nếu $last + w \leq 100$, ta cộng $w$ vào `last`. Nếu không, ta tăng `lines` thêm một và đặt lại `last` thành $w$.

Cuối cùng, ta trả về mảng gồm `lines` và `last`.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfLines(self, widths: List[int], s: str) -> List[int]:
        lines, last = 1, 0
        for w in map(lambda c: widths[ord(c) - ord("a")], s):
            if last + w <= 100:
                last += w
            else:
                lines += 1
                last = w
        return [lines, last]
```

#### Java

```java
class Solution {
    public int[] numberOfLines(int[] widths, String s) {
        int lines = 1, last = 0;
        for (int i = 0; i < s.length(); ++i) {
            int w = widths[s.charAt(i) - 'a'];
            if (last + w <= 100) {
                last += w;
            } else {
                ++lines;
                last = w;
            }
        }
        return new int[] {lines, last};
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> numberOfLines(vector<int>& widths, string s) {
        int lines = 1, last = 0;
        for (char c : s) {
            int w = widths[c - 'a'];
            if (last + w <= 100) {
                last += w;
            } else {
                ++lines;
                last = w;
            }
        }
        return {lines, last};
    }
};
```

#### Go

```go
func numberOfLines(widths []int, s string) []int {
	lines, last := 1, 0
	for _, c := range s {
		w := widths[c-'a']
		if last+w <= 100 {
			last += w
		} else {
			lines++
			last = w
		}
	}
	return []int{lines, last}
}
```

#### TypeScript

```ts
function numberOfLines(widths: number[], s: string): number[] {
    let [lines, last] = [1, 0];
    for (const c of s) {
        const w = widths[c.charCodeAt(0) - 'a'.charCodeAt(0)];
        if (last + w <= 100) {
            last += w;
        } else {
            ++lines;
            last = w;
        }
    }
    return [lines, last];
}
```

#### Rust

```rust
impl Solution {
    pub fn number_of_lines(widths: Vec<i32>, s: String) -> Vec<i32> {
        let mut lines = 1;
        let mut last = 0;

        for c in s.chars() {
            let idx = ((c as u8) - b'a') as usize;
            let w = widths[idx];
            if last + w <= 100 {
                last += w;
            } else {
                lines += 1;
                last = w;
            }
        }

        vec![lines, last]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
