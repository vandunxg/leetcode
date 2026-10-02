---
comments: true
difficulty: Hard
tags:
    - Two Pointers
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [466. Count The Repetitions](https://leetcode.com/problems/count-the-repetitions)

[中文文档](/solution/0400-0499/0466.Count%20The%20Repetitions/README.md)

## Mô tả

<!-- description:start -->

<p>Ta định nghĩa <code>str = [s, n]</code> là chuỗi <code>str</code> được tạo bằng cách nối chuỗi <code>s</code> với nhau <code>n</code> lần.</p>

<ul>
	<li>Ví dụ, <code>str == [&quot;abc&quot;, 3] ==&quot;abcabcabc&quot;</code>.</li>
</ul>

<p>Ta nói có thể tạo chuỗi <code>s1</code> từ chuỗi <code>s2</code> nếu xóa một số ký tự khỏi <code>s2</code> để thu được <code>s1</code>.</p>

<ul>
	<li>Ví dụ, theo định nghĩa này có thể tạo <code>s1 = &quot;abc&quot;</code> từ <code>s2 = &quot;ab<strong><u>dbe</u></strong>c&quot;</code> bằng cách xóa các ký tự vừa in đậm vừa gạch chân.</li>
</ul>

<p>Cho hai chuỗi <code>s1</code>, <code>s2</code> và hai số nguyên <code>n1</code>, <code>n2</code>. Ta có hai chuỗi <code>str1 = [s1, n1]</code> và <code>str2 = [s2, n2]</code>.</p>

<p>Hãy trả về <em>số nguyên lớn nhất </em><code>m</code><em> sao cho có thể tạo </em><code>str = [str2, m]</code><em> từ </em><code>str1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> s1 = "acb", n1 = 4, s2 = "ab", n2 = 2
<strong>Đầu ra:</strong> 2
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> s1 = "acb", n1 = 1, s2 = "acb", n2 = 1
<strong>Đầu ra:</strong> 1
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s1.length, s2.length &lt;= 100</code></li>
	<li><code>s1</code> và <code>s2</code> chỉ gồm chữ cái tiếng Anh viết thường.</li>
	<li><code>1 &lt;= n1, n2 &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý + duyệt lặp

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm có bao nhiêu bản sao của $n_2\cdot s_2$ nằm trong $n_1\cdot s_1$. Nếu duyệt trực tiếp chuỗi đã nối, khi $n_1$ lớn ta sẽ lặp lại nhiều lần cùng một offset trong $s_2$.
>
> Tiền xử lý: với mỗi chỉ số $i$ trong $s_2$, duyệt một lượt qua $s_1$ để biết đã khớp được bao nhiêu bản $s_2$ và chỉ số tiếp theo là gì. Sau đó tra bảng này $n_1$ lần rồi chia cho $n_2$.
>
> $s_2$ có độ dài nhỏ, nên trạng thái duy nhất cần theo dõi là offset bắt đầu; xử lý một bản $s_1$ trở thành một transition $O(1)$.

<!-- thinking:end -->

Ta tiền xử lý chuỗi $s_2$: với mỗi vị trí bắt đầu $i$, tính vị trí tiếp theo $j$ và số lần khớp trọn $s_2$ sau khi xử lý một bản $s_1$. Cụ thể, $d[i] = (cnt, j)$, trong đó $cnt$ là số bản $s_2$ đã khớp và $j$ là vị trí tiếp theo trong $s_2$.

Tiếp theo, khởi tạo $j=0$ rồi lặp $n1$ lần. Mỗi lượt, cộng $d[j][0]$ vào đáp án rồi cập nhật $j=d[j][1]$.

Đáp án cuối cùng bằng số bản $s_2$ có thể khớp trong $n1$ bản $s_1$, chia cho $n2$.

Độ phức tạp thời gian là $O(m \times n + n_1)$ và độ phức tạp không gian là $O(n)$, trong đó $m$ và $n$ lần lượt là độ dài của $s_1$ và $s_2$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getMaxRepetitions(self, s1: str, n1: int, s2: str, n2: int) -> int:
        n = len(s2)
        d = {}
        for i in range(n):
            cnt = 0
            j = i
            for c in s1:
                if c == s2[j]:
                    j += 1
                if j == n:
                    cnt += 1
                    j = 0
            d[i] = (cnt, j)

        ans = 0
        j = 0
        for _ in range(n1):
            cnt, j = d[j]
            ans += cnt
        return ans // n2
```

#### Java

```java
class Solution {
    public int getMaxRepetitions(String s1, int n1, String s2, int n2) {
        int m = s1.length(), n = s2.length();
        int[][] d = new int[n][0];
        for (int i = 0; i < n; ++i) {
            int j = i;
            int cnt = 0;
            for (int k = 0; k < m; ++k) {
                if (s1.charAt(k) == s2.charAt(j)) {
                    if (++j == n) {
                        j = 0;
                        ++cnt;
                    }
                }
            }
            d[i] = new int[] {cnt, j};
        }
        int ans = 0;
        for (int j = 0; n1 > 0; --n1) {
            ans += d[j][0];
            j = d[j][1];
        }
        return ans / n2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int getMaxRepetitions(string s1, int n1, string s2, int n2) {
        int m = s1.size(), n = s2.size();
        vector<pair<int, int>> d;
        for (int i = 0; i < n; ++i) {
            int j = i;
            int cnt = 0;
            for (int k = 0; k < m; ++k) {
                if (s1[k] == s2[j]) {
                    if (++j == n) {
                        ++cnt;
                        j = 0;
                    }
                }
            }
            d.emplace_back(cnt, j);
        }
        int ans = 0;
        for (int j = 0; n1; --n1) {
            ans += d[j].first;
            j = d[j].second;
        }
        return ans / n2;
    }
};
```

#### Go

```go
func getMaxRepetitions(s1 string, n1 int, s2 string, n2 int) (ans int) {
	n := len(s2)
	d := make([][2]int, n)
	for i := 0; i < n; i++ {
		j := i
		cnt := 0
		for k := range s1 {
			if s1[k] == s2[j] {
				j++
				if j == n {
					cnt++
					j = 0
				}
			}
		}
		d[i] = [2]int{cnt, j}
	}
	for j := 0; n1 > 0; n1-- {
		ans += d[j][0]
		j = d[j][1]
	}
	ans /= n2
	return
}
```

#### TypeScript

```ts
function getMaxRepetitions(s1: string, n1: number, s2: string, n2: number): number {
    const n = s2.length;
    const d: number[][] = new Array(n).fill(0).map(() => new Array(2).fill(0));
    for (let i = 0; i < n; ++i) {
        let j = i;
        let cnt = 0;
        for (const c of s1) {
            if (c === s2[j]) {
                if (++j === n) {
                    j = 0;
                    ++cnt;
                }
            }
        }
        d[i] = [cnt, j];
    }
    let ans = 0;
    for (let j = 0; n1 > 0; --n1) {
        ans += d[j][0];
        j = d[j][1];
    }
    return Math.floor(ans / n2);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
