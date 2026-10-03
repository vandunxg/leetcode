---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Hash Table
    - String
---

<!-- problem:start -->

# [1794. Count Pairs of Equal Substrings With Minimum Difference 🔒](https://leetcode.com/problems/count-pairs-of-equal-substrings-with-minimum-difference)

[中文文档](/solution/1700-1799/1794.Count%20Pairs%20of%20Equal%20Substrings%20With%20Minimum%20Difference/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>firstString</code> và <code>secondString</code> được đánh chỉ số từ <strong>0</strong> và chỉ gồm chữ cái tiếng Anh viết thường. Hãy đếm số bộ bốn chỉ số <code>(i,j,a,b)</code> thỏa mãn:</p>

<ul>
	<li><code>0 &lt;= i &lt;= j &lt; firstString.length</code></li>
	<li><code>0 &lt;= a &lt;= b &lt; secondString.length</code></li>
	<li>Chuỗi con của <code>firstString</code> bắt đầu tại ký tự thứ <code>i<sup>th</sup></code> và kết thúc tại ký tự thứ <code>j<sup>th</sup></code> (bao gồm cả hai) <strong>bằng</strong> chuỗi con của <code>secondString</code> bắt đầu tại ký tự thứ <code>a<sup>th</sup></code> và kết thúc tại ký tự thứ <code>b<sup>th</sup></code> (bao gồm cả hai).</li>
	<li><code>j - a</code> là giá trị <strong>nhỏ nhất</strong> trong tất cả các bộ bốn thỏa mãn điều kiện trước.</li>
</ul>

<p>Trả về <em><strong>số lượng</strong> bộ bốn như vậy</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> firstString = &quot;abcd&quot;, secondString = &quot;bccda&quot;
<strong>Output:</strong> 1
<strong>Giải thích:</strong> Bộ bốn (0,0,4,4) là bộ duy nhất thỏa mãn mọi điều kiện và tối thiểu hóa j - a.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> firstString = &quot;ab&quot;, secondString = &quot;cd&quot;
<strong>Output:</strong> 0
<strong>Giải thích:</strong> Không có bộ bốn nào thỏa mãn mọi điều kiện.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= firstString.length, secondString.length &lt;= 2 * 10<sup>5</sup></code></li>
	<li>Cả hai chuỗi chỉ gồm chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Bảng băm

<!-- thinking:start -->

> **Tư duy**
>
> Một bộ bốn hợp lệ cần hai chuỗi con bằng nhau và $j-a$ nhỏ nhất. Điều kiện bằng nhau quy về một ký tự: ghép lần xuất hiện bên trái nhất trong $firstString$ với lần xuất hiện bên phải nhất của cùng ký tự trong $secondString$.
>
> Ánh xạ mỗi ký tự của $secondString$ tới chỉ số cuối cùng của nó. Duyệt $firstString$ và cập nhật giá trị nhỏ nhất toàn cục của $i-\textit{last}[c]$ cùng số lần đạt giá trị đó.

<!-- thinking:end -->

Thực chất, bài toán yêu cầu tìm chỉ số nhỏ nhất $i$ và chỉ số lớn nhất $j$ sao cho $firstString[i]$ bằng $secondString[j]$, đồng thời giá trị của $i - j$ là nhỏ nhất trong mọi cặp chỉ số thỏa mãn điều kiện.

Vì vậy, trước hết ta dùng hash table $last$ để ghi chỉ số xuất hiện cuối cùng của mỗi ký tự trong $secondString$. Sau đó duyệt $firstString$. Với mỗi ký tự $c$, nếu $c$ xuất hiện trong $secondString$, ta tính $i - last[c]$. Nếu giá trị $i - last[c]$ này nhỏ hơn giá trị nhỏ nhất hiện tại, cập nhật giá trị nhỏ nhất và đặt đáp án bằng 1. Nếu giá trị $i - last[c]$ bằng giá trị nhỏ nhất hiện tại, tăng đáp án thêm 1.

Độ phức tạp thời gian là $O(m + n)$, và độ phức tạp không gian là $O(C)$. Ở đây, $m$ và $n$ lần lượt là độ dài của $firstString$ và $secondString$, còn $C$ là kích thước tập ký tự. Trong bài này, $C = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countQuadruples(self, firstString: str, secondString: str) -> int:
        last = {c: i for i, c in enumerate(secondString)}
        ans, mi = 0, inf
        for i, c in enumerate(firstString):
            if c in last:
                t = i - last[c]
                if mi > t:
                    mi = t
                    ans = 1
                elif mi == t:
                    ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int countQuadruples(String firstString, String secondString) {
        int[] last = new int[26];
        for (int i = 0; i < secondString.length(); ++i) {
            last[secondString.charAt(i) - 'a'] = i + 1;
        }
        int ans = 0, mi = 1 << 30;
        for (int i = 0; i < firstString.length(); ++i) {
            int j = last[firstString.charAt(i) - 'a'];
            if (j > 0) {
                int t = i - j;
                if (mi > t) {
                    mi = t;
                    ans = 1;
                } else if (mi == t) {
                    ++ans;
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
    int countQuadruples(string firstString, string secondString) {
        int last[26] = {0};
        for (int i = 0; i < secondString.size(); ++i) {
            last[secondString[i] - 'a'] = i + 1;
        }
        int ans = 0, mi = 1 << 30;
        for (int i = 0; i < firstString.size(); ++i) {
            int j = last[firstString[i] - 'a'];
            if (j) {
                int t = i - j;
                if (mi > t) {
                    mi = t;
                    ans = 1;
                } else if (mi == t) {
                    ++ans;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countQuadruples(firstString string, secondString string) (ans int) {
	last := [26]int{}
	for i, c := range secondString {
		last[c-'a'] = i + 1
	}
	mi := 1 << 30
	for i, c := range firstString {
		j := last[c-'a']
		if j > 0 {
			t := i - j
			if mi > t {
				mi = t
				ans = 1
			} else if mi == t {
				ans++
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function countQuadruples(firstString: string, secondString: string): number {
    const last: number[] = new Array(26).fill(0);
    for (let i = 0; i < secondString.length; ++i) {
        last[secondString.charCodeAt(i) - 97] = i + 1;
    }
    let [ans, mi] = [0, Infinity];
    for (let i = 0; i < firstString.length; ++i) {
        const j = last[firstString.charCodeAt(i) - 97];
        if (j) {
            const t = i - j;
            if (mi > t) {
                mi = t;
                ans = 1;
            } else if (mi === t) {
                ++ans;
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
