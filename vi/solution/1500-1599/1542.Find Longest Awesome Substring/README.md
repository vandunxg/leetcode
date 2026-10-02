---
comments: true
difficulty: Hard
rating: 2221
source: Biweekly Contest 32 Q4
tags:
    - Bit Manipulation
    - Hash Table
    - String
---

<!-- problem:start -->

# [1542. Find Longest Awesome Substring](https://leetcode.com/problems/find-longest-awesome-substring)

[中文文档](/solution/1500-1599/1542.Find%20Longest%20Awesome%20Substring/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code>. Một chuỗi con <strong>đặc biệt</strong> là chuỗi con khác rỗng của <code>s</code> mà ta có thể hoán đổi các ký tự để biến thành palindrome.</p>

<p>Trả về <em>độ dài của <strong>chuỗi con đặc biệt</strong> dài nhất của</em> <code>s</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;3242415&quot;
<strong>Output:</strong> 5
<strong>Explanation:</strong> &quot;24241&quot; là chuỗi con đặc biệt dài nhất; có thể tạo palindrome &quot;24142&quot; bằng một số lần hoán đổi.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;12345678&quot;
<strong>Output:</strong> 1
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;213123&quot;
<strong>Output:</strong> 6
<strong>Explanation:</strong> &quot;213123&quot; là chuỗi con đặc biệt dài nhất; có thể tạo palindrome &quot;231132&quot; bằng một số lần hoán đổi.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> consists only of digits.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nén trạng thái + Prefix Sum

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi con đặc biệt có thể sắp xếp lại thành palindrome, nên nhiều nhất chỉ một chữ số xuất hiện lẻ lần. Vì $n\le 10^5$, không thể liệt kê mọi chuỗi con. Mười chữ số vừa đủ biểu diễn bằng parity mask 10 bit.
>
> Prefix mask $st$ lưu parity của từng chữ số. $s[j+1..i]$ là chuỗi con đặc biệt khi $st_i$ và $st_j$ khác nhau nhiều nhất một bit. Một map lưu chỉ số đầu tiên của mỗi mask: cùng mask tạo đoạn có mọi chữ số xuất hiện chẵn lần, còn lật một bit tạo đúng một chữ số xuất hiện lẻ lần. Giữ lại đoạn dài nhất.

<!-- thinking:end -->

Theo đề bài, các ký tự trong "chuỗi con đặc biệt" có thể được hoán đổi để tạo palindrome. Vì vậy, nhiều nhất một chữ số trong "chuỗi con đặc biệt" xuất hiện số lần lẻ, còn các chữ số khác xuất hiện số lần chẵn.

Ta dùng số nguyên $st$ biểu diễn parity của các chữ số trong prefix hiện tại. Bit thứ $i$ của $st$ biểu diễn parity của chữ số $i$: bằng $1$ nếu chữ số $i$ xuất hiện lẻ lần và bằng $0$ nếu xuất hiện chẵn lần.

Nếu chuỗi con $s[j,..i]$ là "chuỗi con đặc biệt", trạng thái $st$ của prefix $s[0,..i]$ và trạng thái $st'$ của prefix $s[0,..j-1]$ khác nhau nhiều nhất một bit. Bởi nếu các bit khác nhau thì parity khác nhau, và parity khác nhau nghĩa là chữ số đó xuất hiện lẻ lần trong chuỗi con $s[j,..i]$.

Vì vậy, ta dùng hash table hoặc mảng để ghi nhận lần xuất hiện đầu tiên của mọi trạng thái $st$. Nếu trạng thái $st$ của prefix hiện tại đã có trong hash table, mọi bit của nó và trạng thái $st'$ của prefix $s[0,..j-1]$ đều giống nhau, tức $s[j,..i]$ là "chuỗi con đặc biệt", nên cập nhật đáp án lớn nhất. Hoặc ta duyệt từng bit, lật bit thứ $i$ của trạng thái $st$, tức $st \oplus 2^i$, rồi kiểm tra nó trong hash table. Nếu có, chỉ bit thứ $i$ khác nhau, nên $s[j,..i]$ là "chuỗi con đặc biệt" và ta cập nhật đáp án. Trạng thái $st' \oplus 2^i$ tương ứng cũng được kiểm tra; các trạng thái $st$, $st$ và $st$ được lưu lại để dùng cho các chỉ số $i$ và $i$ tiếp theo trong prefix $s[0,..j-1]$. Ta cũng kiểm tra trạng thái $st \oplus 2^i$ tương ứng.

Cuối cùng, trả về đáp án.

Độ phức tạp thời gian là $O(n \times C)$ và độ phức tạp không gian là $O(2^C)$, trong đó $n$ là độ dài chuỗi $s$ và $C$ là số loại chữ số.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestAwesome(self, s: str) -> int:
        st = 0
        d = {0: -1}
        ans = 1
        for i, c in enumerate(s):
            v = int(c)
            st ^= 1 << v
            if st in d:
                ans = max(ans, i - d[st])
            else:
                d[st] = i
            for v in range(10):
                if st ^ (1 << v) in d:
                    ans = max(ans, i - d[st ^ (1 << v)])
        return ans
```

#### Java

```java
class Solution {
    public int longestAwesome(String s) {
        int[] d = new int[1024];
        int st = 0, ans = 1;
        Arrays.fill(d, -1);
        d[0] = 0;
        for (int i = 1; i <= s.length(); ++i) {
            int v = s.charAt(i - 1) - '0';
            st ^= 1 << v;
            if (d[st] >= 0) {
                ans = Math.max(ans, i - d[st]);
            } else {
                d[st] = i;
            }
            for (v = 0; v < 10; ++v) {
                if (d[st ^ (1 << v)] >= 0) {
                    ans = Math.max(ans, i - d[st ^ (1 << v)]);
                }
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
    int longestAwesome(string s) {
        vector<int> d(1024, -1);
        d[0] = 0;
        int st = 0, ans = 1;
        for (int i = 1; i <= s.size(); ++i) {
            int v = s[i - 1] - '0';
            st ^= 1 << v;
            if (~d[st]) {
                ans = max(ans, i - d[st]);
            } else {
                d[st] = i;
            }
            for (v = 0; v < 10; ++v) {
                if (~d[st ^ (1 << v)]) {
                    ans = max(ans, i - d[st ^ (1 << v)]);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func longestAwesome(s string) int {
	d := [1024]int{}
	d[0] = 1
	st, ans := 0, 1
	for i, c := range s {
		i += 2
		st ^= 1 << (c - '0')
		if d[st] > 0 {
			ans = max(ans, i-d[st])
		} else {
			d[st] = i
		}
		for v := 0; v < 10; v++ {
			if d[st^(1<<v)] > 0 {
				ans = max(ans, i-d[st^(1<<v)])
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function longestAwesome(s: string): number {
    const d: number[] = Array(1024).fill(-1);
    let [st, ans] = [0, 1];
    d[0] = 0;

    for (let i = 1; i <= s.length; ++i) {
        const v = s.charCodeAt(i - 1) - '0'.charCodeAt(0);
        st ^= 1 << v;

        if (d[st] >= 0) {
            ans = Math.max(ans, i - d[st]);
        } else {
            d[st] = i;
        }

        for (let v = 0; v < 10; ++v) {
            if (d[st ^ (1 << v)] >= 0) {
                ans = Math.max(ans, i - d[st ^ (1 << v)]);
            }
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
