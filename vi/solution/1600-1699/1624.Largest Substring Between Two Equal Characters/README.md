---
comments: true
difficulty: Easy
rating: 1281
source: Weekly Contest 211 Q1
tags:
    - Hash Table
    - String
---

<!-- problem:start -->

# [1624. Largest Substring Between Two Equal Characters](https://leetcode.com/problems/largest-substring-between-two-equal-characters)

[中文文档](/solution/1600-1699/1624.Largest%20Substring%20Between%20Two%20Equal%20Characters/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code>, trả về <em>độ dài chuỗi con dài nhất nằm giữa hai ký tự giống nhau, không tính hai ký tự đó.</em> Nếu không có chuỗi con như vậy, trả về <code>-1</code>.</p>

<p><strong>Chuỗi con</strong> là một dãy ký tự liên tiếp trong chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;aa&quot;
<strong>Output:</strong> 0
<strong>Explanation:</strong> Chuỗi con tối ưu ở đây là chuỗi rỗng nằm giữa hai chữ <code>&#39;a&#39;s</code>.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;abca&quot;
<strong>Output:</strong> 2
<strong>Explanation:</strong> Chuỗi con tối ưu ở đây là &quot;bc&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;cbzxy&quot;
<strong>Output:</strong> -1
<strong>Explanation:</strong> Không có ký tự nào xuất hiện hai lần trong s.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 300</code></li>
	<li><code>s</code> contains only lowercase English letters.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng

<!-- thinking:start -->

> **Tư duy**
>
> Độ dài giữa hai chữ cái giống nhau là khoảng cách giữa lần xuất hiện đầu tiên của chữ đó và một lần xuất hiện sau. Chuỗi ngắn, nhưng chỉ cần lưu chỉ số đầu tiên của mỗi chữ cái là đã có lời giải tuyến tính.
>
> Khi gặp lại một ký tự, cập nhật đáp án bằng $i - d[j] - 1$ và không ghi đè chỉ số đầu tiên, để khoảng cách luôn lớn nhất.
>
> Vì $s$ chỉ chứa chữ cái thường, một mảng độ dài $26$ là đủ; nếu không có ký tự nào xuất hiện hai lần, đáp án vẫn là $-1$.

<!-- thinking:end -->

Vì $s$ chỉ chứa chữ cái tiếng Anh viết thường, ta có thể dùng mảng $d$ độ dài $26$ để lưu chỉ số đầu tiên của mỗi ký tự, ban đầu điền $-1$.

Duyệt $s$. Với ký tự $c$ tại chỉ số $i$, gọi $j$ là độ lệch của $c$ so với `a`. Nếu $d[j] = -1$, đây là lần đầu gặp $c$, nên đặt $d[j] = i$; ngược lại, cập nhật đáp án bằng $i - d[j] - 1$, tức là $ans = \max(ans, i - d[j] - 1)$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(C)$, trong đó $n$ là độ dài của $s$ và $C = 26$ là kích thước bảng chữ cái.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxLengthBetweenEqualCharacters(self, s: str) -> int:
        d = [-1] * 26
        ans = -1
        for i, c in enumerate(s):
            j = ord(c) - ord("a")
            if d[j] == -1:
                d[j] = i
            else:
                ans = max(ans, i - d[j] - 1)
        return ans
```

#### Java

```java
class Solution {
    public int maxLengthBetweenEqualCharacters(String s) {
        int[] d = new int[26];
        Arrays.fill(d, -1);
        int ans = -1;
        for (int i = 0; i < s.length(); ++i) {
            int j = s.charAt(i) - 'a';
            if (d[j] == -1) {
                d[j] = i;
            } else {
                ans = Math.max(ans, i - d[j] - 1);
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxLengthBetweenEqualCharacters(string s) {
        vector<int> d(26, -1);
        int ans = -1;
        for (int i = 0; i < s.size(); ++i) {
            int j = s[i] - 'a';
            if (d[j] == -1) {
                d[j] = i;
            } else {
                ans = max(ans, i - d[j] - 1);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxLengthBetweenEqualCharacters(s string) int {
	d := make([]int, 26)
	for i := range d {
		d[i] = -1
	}
	ans := -1
	for i := range s {
		j := int(s[i] - 'a')
		if d[j] == -1 {
			d[j] = i
		} else {
			ans = max(ans, i-d[j]-1)
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maxLengthBetweenEqualCharacters(s: string): number {
    const d = Array(26).fill(-1);
    let ans = -1;
    for (let i = 0; i < s.length; ++i) {
        const j = s.charCodeAt(i) - 97;
        if (d[j] === -1) {
            d[j] = i;
        } else {
            ans = Math.max(ans, i - d[j] - 1);
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_length_between_equal_characters(s: String) -> i32 {
        let s = s.as_bytes();
        let mut d = [-1; 26];
        let mut ans = -1;
        for i in 0..s.len() {
            let j = (s[i] - b'a') as usize;
            if d[j] == -1 {
                d[j] = i as i32;
            } else {
                ans = ans.max(i as i32 - d[j] - 1);
            }
        }
        ans
    }
}
```

#### C

```c
#define max(a, b) (((a) > (b)) ? (a) : (b))

int maxLengthBetweenEqualCharacters(char* s) {
    int d[26];
    memset(d, -1, sizeof(d));
    int ans = -1;
    for (int i = 0; s[i]; ++i) {
        int j = s[i] - 'a';
        if (d[j] == -1) {
            d[j] = i;
        } else {
            ans = max(ans, i - d[j] - 1);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
