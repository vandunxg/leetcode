---
comments: true
difficulty: Easy
rating: 1301
source: Weekly Contest 289 Q1
tags:
    - String
    - Simulation
---

<!-- problem:start -->

# [2243. Calculate Digit Sum of a String](https://leetcode.com/problems/calculate-digit-sum-of-a-string)

[中文文档](/solution/2200-2299/2243.Calculate%20Digit%20Sum%20of%20a%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> chỉ gồm các chữ số và một số nguyên <code>k</code>.</p>

<p>Một <strong>vòng</strong> có thể được thực hiện nếu độ dài của <code>s</code> lớn hơn <code>k</code>. Trong một vòng, thực hiện các bước sau:</p>

<ol>
	<li><strong>Chia</strong> <code>s</code> thành các <strong>nhóm liên tiếp</strong> có kích thước <code>k</code>, sao cho <code>k</code> ký tự đầu tiên thuộc nhóm đầu tiên, <code>k</code> ký tự tiếp theo thuộc nhóm thứ hai, và cứ tiếp tục như vậy. <strong>Lưu ý</strong> rằng nhóm cuối cùng có thể nhỏ hơn <code>k</code>.</li>
	<li><strong>Thay thế</strong> mỗi nhóm của <code>s</code> bằng một chuỗi biểu diễn tổng của tất cả chữ số trong nhóm đó. Ví dụ, <code>&quot;346&quot;</code> được thay bằng <code>&quot;13&quot;</code> vì <code>3 + 4 + 6 = 13</code>.</li>
	<li><strong>Gộp</strong> các nhóm liên tiếp lại để tạo thành một chuỗi mới. Nếu độ dài chuỗi lớn hơn <code>k</code>, lặp lại từ bước <code>1</code>.</li>
</ol>

<p>Trả về <code>s</code> <em>sau khi hoàn thành tất cả các vòng</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;11111222223&quot;, k = 3
<strong>Đầu ra:</strong> &quot;135&quot;
<strong>Giải thích:</strong>
- Ở vòng đầu tiên, ta chia s thành các nhóm có kích thước 3: &quot;111&quot;, &quot;112&quot;, &quot;222&quot; và &quot;23&quot;.
  ​​​​​Sau đó, ta tính tổng các chữ số của từng nhóm: 1 + 1 + 1 = 3, 1 + 1 + 2 = 4, 2 + 2 + 2 = 6 và 2 + 3 = 5.
&nbsp; Vì vậy, sau vòng đầu tiên, s trở thành &quot;3&quot; + &quot;4&quot; + &quot;6&quot; + &quot;5&quot; = &quot;3465&quot;.
- Ở vòng thứ hai, ta chia s thành &quot;346&quot; và &quot;5&quot;.
&nbsp; Sau đó, ta tính tổng các chữ số của từng nhóm: 3 + 4 + 6 = 13, 5 = 5.
&nbsp; Vì vậy, sau vòng thứ hai, s trở thành &quot;13&quot; + &quot;5&quot; = &quot;135&quot;.
Hiện tại, s.length &lt;= k, nên ta trả về &quot;135&quot; làm đáp án.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;00000000&quot;, k = 3
<strong>Đầu ra:</strong> &quot;000&quot;
<strong>Giải thích:</strong>
Ta chia s thành &quot;000&quot;, &quot;000&quot; và &quot;00&quot;.
Sau đó, ta tính tổng các chữ số của từng nhóm: 0 + 0 + 0 = 0, 0 + 0 + 0 = 0 và 0 + 0 = 0.
s trở thành &quot;0&quot; + &quot;0&quot; + &quot;0&quot; = &quot;000&quot;, có độ dài bằng k, nên ta trả về &quot;000&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>2 &lt;= k &lt;= 100</code></li>
	<li><code>s</code> chỉ gồm các chữ số.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Lặp lại việc thay mỗi khối gồm $k$ chữ số bằng tổng các chữ số trong khối đó cho đến khi chuỗi có độ dài không quá $k$. Vì $|s| \le 100$, mô phỏng các vòng là đủ.
>
> Khi độ dài vượt quá $k$, ta lấy các lát với bước nhảy $k$, tính tổng từng lát rồi nối các biểu diễn thập phân của chúng. Mỗi tổng có nhiều nhất ba chữ số, nên chuỗi sẽ ngắn dần.

<!-- thinking:end -->

Theo đề bài, ta có thể mô phỏng các thao tác được mô tả cho đến khi độ dài chuỗi nhỏ hơn hoặc bằng $k$. Cuối cùng, trả về chuỗi đó.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def digitSum(self, s: str, k: int) -> str:
        while len(s) > k:
            t = []
            n = len(s)
            for i in range(0, n, k):
                x = 0
                for j in range(i, min(i + k, n)):
                    x += int(s[j])
                t.append(str(x))
            s = "".join(t)
        return s
```

#### Java

```java
class Solution {
    public String digitSum(String s, int k) {
        while (s.length() > k) {
            int n = s.length();
            StringBuilder t = new StringBuilder();
            for (int i = 0; i < n; i += k) {
                int x = 0;
                for (int j = i; j < Math.min(i + k, n); ++j) {
                    x += s.charAt(j) - '0';
                }
                t.append(x);
            }
            s = t.toString();
        }
        return s;
    }
}
```

#### C++

```cpp
class Solution {
public:
    string digitSum(string s, int k) {
        while (s.size() > k) {
            string t;
            int n = s.size();
            for (int i = 0; i < n; i += k) {
                int x = 0;
                for (int j = i; j < min(i + k, n); ++j) {
                    x += s[j] - '0';
                }
                t += to_string(x);
            }
            s = t;
        }
        return s;
    }
};
```

#### Go

```go
func digitSum(s string, k int) string {
	for len(s) > k {
		t := &strings.Builder{}
		n := len(s)
		for i := 0; i < n; i += k {
			x := 0
			for j := i; j < i+k && j < n; j++ {
				x += int(s[j] - '0')
			}
			t.WriteString(strconv.Itoa(x))
		}
		s = t.String()
	}
	return s
}
```

#### TypeScript

```ts
function digitSum(s: string, k: number): string {
    while (s.length > k) {
        const t: number[] = [];
        for (let i = 0; i < s.length; i += k) {
            const x = s
                .slice(i, i + k)
                .split('')
                .reduce((a, b) => a + +b, 0);
            t.push(x);
        }
        s = t.join('');
    }
    return s;
}
```

#### Rust

```rust
impl Solution {
    pub fn digit_sum(s: String, k: i32) -> String {
        let mut s = s;
        let k = k as usize;
        while s.len() > k {
            let mut t = Vec::new();
            for chunk in s.as_bytes().chunks(k) {
                let sum: i32 = chunk.iter().map(|&c| (c - b'0') as i32).sum();
                t.push(sum.to_string());
            }
            s = t.join("");
        }
        s
    }
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @param {number} k
 * @return {string}
 */
var digitSum = function (s, k) {
    while (s.length > k) {
        const t = [];
        for (let i = 0; i < s.length; i += k) {
            const x = s
                .slice(i, i + k)
                .split('')
                .reduce((a, b) => a + +b, 0);
            t.push(x);
        }
        s = t.join('');
    }
    return s;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
