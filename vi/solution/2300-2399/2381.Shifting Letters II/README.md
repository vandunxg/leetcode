---
comments: true
difficulty: Medium
rating: 1793
source: Biweekly Contest 85 Q3
tags:
    - Array
    - String
    - Prefix Sum
---

<!-- problem:start -->

# [2381. Shifting Letters II](https://leetcode.com/problems/shifting-letters-ii)

[中文文档](/solution/2300-2399/2381.Shifting%20Letters%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> gồm các chữ cái tiếng Anh viết thường và một mảng số nguyên 2 chiều <code>shifts</code>, trong đó <code>shifts[i] = [start<sub>i</sub>, end<sub>i</sub>, direction<sub>i</sub>]</code>. Với mỗi <code>i</code>, hãy <strong>dịch chuyển</strong> các ký tự trong <code>s</code> từ chỉ số <code>start<sub>i</sub></code> đến chỉ số <code>end<sub>i</sub></code> (<strong>bao gồm cả hai đầu mút</strong>) theo chiều tiến nếu <code>direction<sub>i</sub> = 1</code>, hoặc theo chiều lùi nếu <code>direction<sub>i</sub> = 0</code>.</p>

<p>Dịch chuyển một ký tự theo chiều <strong>tiến</strong> nghĩa là thay thế nó bằng chữ cái <strong>tiếp theo</strong> trong bảng chữ cái (quay vòng để <code>&#39;z&#39;</code> trở thành <code>&#39;a&#39;</code>). Tương tự, dịch chuyển một ký tự theo chiều <strong>lùi</strong> nghĩa là thay thế nó bằng chữ cái <strong>trước đó</strong> trong bảng chữ cái (quay vòng để <code>&#39;a&#39;</code> trở thành <code>&#39;z&#39;</code>).</p>

<p>Trả về <em>chuỗi cuối cùng sau khi áp dụng tất cả các phép dịch chuyển trên cho </em><code>s</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abc&quot;, shifts = [[0,1,0],[1,2,1],[0,2,1]]
<strong>Đầu ra:</strong> &quot;ace&quot;
<strong>Giải thích:</strong> Đầu tiên, dịch chuyển các ký tự từ chỉ số 0 đến chỉ số 1 theo chiều lùi. Khi đó s = &quot;zac&quot;.
Tiếp theo, dịch chuyển các ký tự từ chỉ số 1 đến chỉ số 2 theo chiều tiến. Khi đó s = &quot;zbd&quot;.
Cuối cùng, dịch chuyển các ký tự từ chỉ số 0 đến chỉ số 2 theo chiều tiến. Khi đó s = &quot;ace&quot;.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;dztz&quot;, shifts = [[0,0,0],[1,1,1]]
<strong>Đầu ra:</strong> &quot;catz&quot;
<strong>Giải thích:</strong> Đầu tiên, dịch chuyển ký tự từ chỉ số 0 đến chỉ số 0 theo chiều lùi. Khi đó s = &quot;cztz&quot;.
Cuối cùng, dịch chuyển ký tự từ chỉ số 1 đến chỉ số 1 theo chiều tiến. Khi đó s = &quot;catz&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length, shifts.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>shifts[i].length == 3</code></li>
	<li><code>0 &lt;= start<sub>i</sub> &lt;= end<sub>i</sub> &lt; s.length</code></li>
	<li><code>0 &lt;= direction<sub>i</sub> &lt;= 1</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Có nhiều phép dịch chuyển đoạn với giá trị $\pm 1$ và quay vòng trong bảng chữ cái. Cả $n$ và số phép toán đều có thể đạt $5 \times 10^4$, vì vậy ta không thể viết lại một đoạn trong mỗi lần.
>
> Mảng hiệu cộng hướng dịch chuyển tại $l$ và trừ nó tại $r+1$. Giá trị prefix là độ dịch chuyển ròng; lấy modulo $26$ rồi ghi lại chữ cái tương ứng.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def shiftingLetters(self, s: str, shifts: List[List[int]]) -> str:
        n = len(s)
        d = [0] * (n + 1)
        for i, j, v in shifts:
            if v == 0:
                v = -1
            d[i] += v
            d[j + 1] -= v
        for i in range(1, n + 1):
            d[i] += d[i - 1]
        return ''.join(
            chr(ord('a') + (ord(s[i]) - ord('a') + d[i] + 26) % 26) for i in range(n)
        )
```

#### Java

```java
class Solution {
    public String shiftingLetters(String s, int[][] shifts) {
        int n = s.length();
        int[] d = new int[n + 1];
        for (int[] e : shifts) {
            if (e[2] == 0) {
                e[2]--;
            }
            d[e[0]] += e[2];
            d[e[1] + 1] -= e[2];
        }
        for (int i = 1; i <= n; ++i) {
            d[i] += d[i - 1];
        }
        StringBuilder ans = new StringBuilder();
        for (int i = 0; i < n; ++i) {
            int j = (s.charAt(i) - 'a' + d[i] % 26 + 26) % 26;
            ans.append((char) ('a' + j));
        }
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string shiftingLetters(string s, vector<vector<int>>& shifts) {
        int n = s.size();
        vector<int> d(n + 1);
        for (auto& e : shifts) {
            if (e[2] == 0) {
                e[2]--;
            }
            d[e[0]] += e[2];
            d[e[1] + 1] -= e[2];
        }
        for (int i = 1; i <= n; ++i) {
            d[i] += d[i - 1];
        }
        string ans;
        for (int i = 0; i < n; ++i) {
            int j = (s[i] - 'a' + d[i] % 26 + 26) % 26;
            ans += ('a' + j);
        }
        return ans;
    }
};
```

#### Go

```go
func shiftingLetters(s string, shifts [][]int) string {
	n := len(s)
	d := make([]int, n+1)
	for _, e := range shifts {
		if e[2] == 0 {
			e[2]--
		}
		d[e[0]] += e[2]
		d[e[1]+1] -= e[2]
	}
	for i := 1; i <= n; i++ {
		d[i] += d[i-1]
	}
	ans := []byte{}
	for i, c := range s {
		j := (int(c-'a') + d[i]%26 + 26) % 26
		ans = append(ans, byte('a'+j))
	}
	return string(ans)
}
```

#### TypeScript

```ts
function shiftingLetters(s: string, shifts: number[][]): string {
    const n: number = s.length;
    const d: number[] = new Array(n + 1).fill(0);

    for (let [i, j, v] of shifts) {
        if (v === 0) {
            v--;
        }
        d[i] += v;
        d[j + 1] -= v;
    }

    for (let i = 1; i <= n; ++i) {
        d[i] += d[i - 1];
    }

    let ans: string = '';
    for (let i = 0; i < n; ++i) {
        const j = (s.charCodeAt(i) - 'a'.charCodeAt(0) + (d[i] % 26) + 26) % 26;
        ans += String.fromCharCode('a'.charCodeAt(0) + j);
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
