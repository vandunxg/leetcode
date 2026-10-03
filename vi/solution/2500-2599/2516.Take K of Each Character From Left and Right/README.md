---
comments: true
difficulty: Medium
rating: 1947
source: Weekly Contest 325 Q2
tags:
    - Hash Table
    - String
    - Sliding Window
---

<!-- problem:start -->

# [2516. Take K of Each Character From Left and Right](https://leetcode.com/problems/take-k-of-each-character-from-left-and-right)

[中文文档](/solution/2500-2599/2516.Take%20K%20of%20Each%20Character%20From%20Left%20and%20Right/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> chỉ gồm các ký tự <code>&#39;a&#39;</code>, <code>&#39;b&#39;</code> và <code>&#39;c&#39;</code>, cùng một số nguyên không âm <code>k</code>. Mỗi phút, bạn có thể lấy ký tự ngoài cùng bên <strong>trái</strong> của <code>s</code> hoặc ngoài cùng bên <strong>phải</strong> của <code>s</code>.</p>

<p>Hãy trả về <em>số phút <strong>ít nhất</strong> cần thiết để lấy <strong>ít nhất</strong> </em><code>k</code><em> ký tự của mỗi loại, hoặc trả về </em><code>-1</code><em> nếu không thể lấy </em><code>k</code><em> ký tự của mỗi loại.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aabaaaacaabc&quot;, k = 2
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong>
Lấy ba ký tự từ bên trái của s. Lúc này bạn có hai ký tự &#39;a&#39; và một ký tự &#39;b&#39;.
Lấy năm ký tự từ bên phải của s. Lúc này bạn có bốn ký tự &#39;a&#39;, hai ký tự &#39;b&#39; và hai ký tự &#39;c&#39;.
Cần tổng cộng 3 + 5 = 8 phút.
Có thể chứng minh rằng 8 là số phút ít nhất cần thiết.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;a&quot;, k = 1
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không thể lấy một ký tự &#39;b&#39; hoặc &#39;c&#39;, nên trả về -1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
    <li><code>s</code> chỉ gồm các chữ cái <code>&#39;a&#39;</code>, <code>&#39;b&#39;</code> và <code>&#39;c&#39;</code>.</li>
    <li><code>0 &lt;= k &lt;= s.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sliding Window

<!-- thinking:start -->

> **Tư duy**
>
> Các ký tự chỉ có thể được lấy từ đầu trái hoặc đầu phải hiện tại. Chúng ta cần ít nhất $k$ ký tự của mỗi loại $a,b,c$ và muốn số lần lấy là ít nhất. Việc duyệt mọi độ dài prefix và suffix có độ phức tạp bậc hai khi $n\le 10^5$.
>
> Những gì được lấy là một prefix cộng với một suffix, nên phần còn lại là một cửa sổ ở giữa. Tương đương với việc tìm cửa sổ dài nhất sao cho bên ngoài nó, mỗi chữ cái vẫn xuất hiện ít nhất $k$ lần. Nếu số lượng tổng của các ký tự đã không đủ, bài toán không có đáp án. Ngược lại, ta trượt cửa sổ: mở rộng đầu phải và thu hẹp đầu trái bất cứ khi nào số lượng một ký tự giảm dưới $k$. Cửa sổ hợp lệ dài nhất cho số lần lấy ít nhất.

<!-- thinking:end -->

Đầu tiên, chúng ta dùng một hash table hoặc một mảng có độ dài $3$, ký hiệu là $cnt$, để đếm số lần xuất hiện của mỗi ký tự trong chuỗi $s$. Nếu số lần xuất hiện của bất kỳ ký tự nào nhỏ hơn $k$, không thể lấy đủ, nên trả về $-1$ ngay.

Bài toán yêu cầu xóa ký tự từ hai đầu trái và phải của chuỗi, sao cho số lượng của mỗi ký tự còn lại không nhỏ hơn $k$. Ta có thể xét bài toán ngược: xóa một substring ở giữa với độ dài nào đó, sao cho chuỗi còn lại ở hai bên có số lượng của mỗi ký tự không nhỏ hơn $k$.

Do đó, chúng ta duy trì một sliding window, với hai con trỏ $j$ và $i$ lần lượt biểu diễn biên trái và biên phải của cửa sổ. Chuỗi nằm trong cửa sổ là phần chúng ta muốn xóa. Mỗi lần di chuyển biên phải $i$, ta thêm ký tự tương ứng $s[i]$ vào cửa sổ (tức là xóa một ký tự $s[i]$). Nếu số lượng $cnt[s[i]]$ nhỏ hơn $k$, ta lặp việc di chuyển biên trái $j$ cho đến khi số lượng $cnt[s[i]]$ không còn nhỏ hơn $k$. Khi đó, kích thước cửa sổ là $i - j + 1$, và chúng ta cập nhật kích thước cửa sổ lớn nhất.

Đáp án cuối cùng là độ dài của chuỗi $s$ trừ đi kích thước cửa sổ lớn nhất.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def takeCharacters(self, s: str, k: int) -> int:
        cnt = Counter(s)
        if any(cnt[c] < k for c in "abc"):
            return -1
        mx = j = 0
        for i, c in enumerate(s):
            cnt[c] -= 1
            while cnt[c] < k:
                cnt[s[j]] += 1
                j += 1
            mx = max(mx, i - j + 1)
        return len(s) - mx
```

#### Java

```java
class Solution {
    public int takeCharacters(String s, int k) {
        int[] cnt = new int[3];
        int n = s.length();
        for (int i = 0; i < n; ++i) {
            ++cnt[s.charAt(i) - 'a'];
        }
        for (int x : cnt) {
            if (x < k) {
                return -1;
            }
        }
        int mx = 0, j = 0;
        for (int i = 0; i < n; ++i) {
            int c = s.charAt(i) - 'a';
            --cnt[c];
            while (cnt[c] < k) {
                ++cnt[s.charAt(j++) - 'a'];
            }
            mx = Math.max(mx, i - j + 1);
        }
        return n - mx;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int takeCharacters(string s, int k) {
        int cnt[3]{};
        int n = s.length();
        for (int i = 0; i < n; ++i) {
            ++cnt[s[i] - 'a'];
        }
        for (int x : cnt) {
            if (x < k) {
                return -1;
            }
        }
        int mx = 0, j = 0;
        for (int i = 0; i < n; ++i) {
            int c = s[i] - 'a';
            --cnt[c];
            while (cnt[c] < k) {
                ++cnt[s[j++] - 'a'];
            }
            mx = max(mx, i - j + 1);
        }
        return n - mx;
    }
};
```

#### Go

```go
func takeCharacters(s string, k int) int {
	cnt := [3]int{}
	for _, c := range s {
		cnt[c-'a']++
	}
	for _, x := range cnt {
		if x < k {
			return -1
		}
	}
	mx, j := 0, 0
	for i, c := range s {
		c -= 'a'
		for cnt[c]--; cnt[c] < k; j++ {
			cnt[s[j]-'a']++
		}
		mx = max(mx, i-j+1)
	}
	return len(s) - mx
}
```

#### TypeScript

```ts
function takeCharacters(s: string, k: number): number {
    const idx = (c: string) => c.charCodeAt(0) - 97;
    const cnt: number[] = Array(3).fill(0);
    for (const c of s) {
        ++cnt[idx(c)];
    }
    if (cnt.some(v => v < k)) {
        return -1;
    }
    const n = s.length;
    let [mx, j] = [0, 0];
    for (let i = 0; i < n; ++i) {
        const c = idx(s[i]);
        --cnt[c];
        while (cnt[c] < k) {
            ++cnt[idx(s[j++])];
        }
        mx = Math.max(mx, i - j + 1);
    }
    return n - mx;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn take_characters(s: String, k: i32) -> i32 {
        let mut cnt: HashMap<char, i32> = HashMap::new();
        for c in s.chars() {
            *cnt.entry(c).or_insert(0) += 1;
        }

        if "abc".chars().any(|c| cnt.get(&c).unwrap_or(&0) < &k) {
            return -1;
        }

        let mut mx = 0;
        let mut j = 0;
        let mut cs = s.chars().collect::<Vec<char>>();
        for i in 0..cs.len() {
            let c = cs[i];
            *cnt.get_mut(&c).unwrap() -= 1;
            while cnt.get(&c).unwrap() < &k {
                *cnt.get_mut(&cs[j]).unwrap() += 1;
                j += 1;
            }
            mx = mx.max(i - j + 1);
        }
        (cs.len() as i32) - (mx as i32)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
