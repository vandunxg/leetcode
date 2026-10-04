---
comments: true
difficulty: Hard
rating: 2693
source: Weekly Contest 435 Q4
tags:
    - String
    - Enumeration
    - Prefix Sum
    - Sliding Window
---

<!-- problem:start -->

# [3445. Maximum Difference Between Even and Odd Frequency II](https://leetcode.com/problems/maximum-difference-between-even-and-odd-frequency-ii)

[中文文档](/solution/3400-3499/3445.Maximum%20Difference%20Between%20Even%20and%20Odd%20Frequency%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> và một số nguyên <code>k</code>. Nhiệm vụ của bạn là tìm <strong>độ chênh lệch lớn nhất</strong> giữa tần suất xuất hiện của <strong>hai</strong> ký tự, <code>freq[a] - freq[b]</code>, trong một <span data-keyword="substring">chuỗi con</span> <code>subs</code> của <code>s</code>, sao cho:</p>

<ul>
	<li><code>subs</code> có độ dài <strong>ít nhất</strong> <code>k</code>.</li>
	<li>Ký tự <code>a</code> xuất hiện với <em>số lần lẻ</em> trong <code>subs</code>.</li>
	<li>Ký tự <code>b</code> xuất hiện với <strong>số lần chẵn</strong> và <em>khác 0</em> trong <code>subs</code>.</li>
</ul>

<p>Trả về <strong>độ chênh lệch lớn nhất</strong>.</p>

<p><strong>Lưu ý</strong> rằng <code>subs</code> có thể chứa nhiều hơn 2 ký tự <strong>phân biệt</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;12233&quot;, k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Với chuỗi con <code>&quot;12233&quot;</code>, tần suất xuất hiện của <code>&#39;1&#39;</code> là 1 và tần suất xuất hiện của <code>&#39;3&#39;</code> là 2. Độ chênh lệch là <code>1 - 2 = -1</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;1122211&quot;, k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Với chuỗi con <code>&quot;11222&quot;</code>, tần suất xuất hiện của <code>&#39;2&#39;</code> là 3 và tần suất xuất hiện của <code>&#39;1&#39;</code> là 2. Độ chênh lệch là <code>3 - 2 = 1</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;110&quot;, k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= s.length &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ số từ <code>&#39;0&#39;</code> đến <code>&#39;4&#39;</code>.</li>
	<li>Dữ liệu đầu vào được tạo sao cho có ít nhất một chuỗi con chứa một ký tự xuất hiện với số lần chẵn và một ký tự xuất hiện với số lần lẻ.</li>
	<li><code>1 &lt;= k &lt;= s.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê cặp ký tự + Cửa sổ trượt + Nén trạng thái prefix

<!-- thinking:start -->

> **Tư duy**
>
> Khác với phần I, ta cần tối đa hóa hiệu giữa số lần xuất hiện lẻ của $a$ và số lần xuất hiện chẵn của $b$ trên các chuỗi con có độ dài ít nhất $k$. Có năm ký tự, nhưng có $O(n^2)$ chuỗi con.
>
> $f_a-f_b$ là hiệu của các số đếm prefix. Các ràng buộc về tính chẵn lẻ có thể được nén thành hai bit, vì vậy với mỗi đầu trái của cửa sổ trượt, ta chỉ cần truy vấn giá trị nhỏ nhất của $\textit{preA}-\textit{preB}$.
>
> Với mỗi cặp $(a,b)$, ta tăng $r$, thu hẹp $l$ khi độ dài và số lần xuất hiện của $b$ cho phép, lưu hiệu prefix tốt nhất cho mỗi tính chẵn lẻ trong $t[2][2]$, rồi kết hợp nó với $\textit{curA}-\textit{curB}$.

<!-- thinking:end -->

Ta cần tìm một chuỗi con $\textit{subs}$ của chuỗi $s$ thỏa mãn các điều kiện sau:

- Độ dài của $\textit{subs}$ ít nhất là $k$.
- Số lần xuất hiện của ký tự $a$ trong $\textit{subs}$ là số lẻ.
- Số lần xuất hiện của ký tự $b$ trong $\textit{subs}$ là số chẵn.
- Tối đa hóa độ chênh lệch tần suất $f_a - f_b$, trong đó $f_a$ và $f_b$ lần lượt là số lần xuất hiện của $a$ và $b$ trong $\textit{subs}$.

Các ký tự trong $s$ nằm trong khoảng từ '0' đến '4', nên có 5 ký tự có thể chọn. Ta có thể liệt kê mọi cặp ký tự khác nhau $(a, b)$, tổng cộng nhiều nhất là $5 \times 4 = 20$ cặp. Ta định nghĩa:

- Ký tự $a$ là ký tự mục tiêu có tần suất lẻ.
- Ký tự $b$ là ký tự mục tiêu có tần suất chẵn.

Ta dùng cửa sổ trượt để duy trì biên trái và biên phải của chuỗi con, với các biến:

- $l$ biểu thị vị trí ngay trước biên trái, nên cửa sổ là $[l+1, r]$;
- $r$ là biên phải, duyệt qua toàn bộ chuỗi;
- $\textit{curA}$ và $\textit{curB}$ lần lượt là số lần xuất hiện của $a$ và $b$ trong cửa sổ hiện tại;
- $\textit{preA}$ và $\textit{preB}$ lần lượt là số lần xuất hiện tích lũy của $a$ và $b$ trước biên trái $l$.

Ta dùng một mảng 2 chiều $t[2][2]$ để ghi lại giá trị nhỏ nhất của $\textit{preA} - \textit{preB}$ cho mỗi tổ hợp tính chẵn lẻ có thể có ở đầu trái của cửa sổ, trong đó $t[i][j]$ nghĩa là $\textit{preA} \bmod 2 = i$ và $\textit{preB} \bmod 2 = j$.

Mỗi khi dịch $r$ sang phải, nếu độ dài cửa sổ thỏa mãn $r - l \ge k$ và $\textit{curB} - \textit{preB} \ge 2$, ta thử dịch biên trái $l$ để thu hẹp cửa sổ, đồng thời cập nhật $t[\textit{preA} \bmod 2][\textit{preB} \bmod 2]$ tương ứng.

Sau đó, ta thử cập nhật đáp án:

$$
\textit{ans} = \max(\textit{ans},\ \textit{curA} - \textit{curB} - t[(\textit{curA} \bmod 2) \oplus 1][\textit{curB} \bmod 2])
$$

Bằng cách này, mỗi khi $r$ dịch sang phải, ta có thể tính độ chênh lệch tần suất lớn nhất cho cửa sổ hiện tại.

Độ phức tạp thời gian là $O(n \times |\Sigma|^2)$, trong đó $n$ là độ dài của $s$ và $|\Sigma|$ là kích thước bảng chữ cái (5 trong bài toán này). Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxDifference(self, S: str, k: int) -> int:
        s = list(map(int, S))
        ans = -inf
        for a in range(5):
            for b in range(5):
                if a == b:
                    continue
                curA = curB = 0
                preA = preB = 0
                t = [[inf, inf], [inf, inf]]
                l = -1
                for r, x in enumerate(s):
                    curA += x == a
                    curB += x == b
                    while r - l >= k and curB - preB >= 2:
                        t[preA & 1][preB & 1] = min(t[preA & 1][preB & 1], preA - preB)
                        l += 1
                        preA += s[l] == a
                        preB += s[l] == b
                    ans = max(ans, curA - curB - t[curA & 1 ^ 1][curB & 1])
        return ans
```

#### Java

```java
class Solution {
    public int maxDifference(String S, int k) {
        char[] s = S.toCharArray();
        int n = s.length;
        final int inf = Integer.MAX_VALUE / 2;
        int ans = -inf;
        for (int a = 0; a < 5; ++a) {
            for (int b = 0; b < 5; ++b) {
                if (a == b) {
                    continue;
                }
                int curA = 0, curB = 0;
                int preA = 0, preB = 0;
                int[][] t = {{inf, inf}, {inf, inf}};
                for (int l = -1, r = 0; r < n; ++r) {
                    curA += s[r] == '0' + a ? 1 : 0;
                    curB += s[r] == '0' + b ? 1 : 0;
                    while (r - l >= k && curB - preB >= 2) {
                        t[preA & 1][preB & 1] = Math.min(t[preA & 1][preB & 1], preA - preB);
                        ++l;
                        preA += s[l] == '0' + a ? 1 : 0;
                        preB += s[l] == '0' + b ? 1 : 0;
                    }
                    ans = Math.max(ans, curA - curB - t[curA & 1 ^ 1][curB & 1]);
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
    int maxDifference(string s, int k) {
        const int n = s.size();
        const int inf = INT_MAX / 2;
        int ans = -inf;

        for (int a = 0; a < 5; ++a) {
            for (int b = 0; b < 5; ++b) {
                if (a == b) {
                    continue;
                }

                int curA = 0, curB = 0;
                int preA = 0, preB = 0;
                int t[2][2] = {{inf, inf}, {inf, inf}};
                int l = -1;

                for (int r = 0; r < n; ++r) {
                    curA += (s[r] == '0' + a);
                    curB += (s[r] == '0' + b);
                    while (r - l >= k && curB - preB >= 2) {
                        t[preA & 1][preB & 1] = min(t[preA & 1][preB & 1], preA - preB);
                        ++l;
                        preA += (s[l] == '0' + a);
                        preB += (s[l] == '0' + b);
                    }
                    ans = max(ans, curA - curB - t[(curA & 1) ^ 1][curB & 1]);
                }
            }
        }

        return ans;
    }
};
```

#### Go

```go
func maxDifference(s string, k int) int {
	n := len(s)
	inf := math.MaxInt32 / 2
	ans := -inf

	for a := 0; a < 5; a++ {
		for b := 0; b < 5; b++ {
			if a == b {
				continue
			}
			curA, curB := 0, 0
			preA, preB := 0, 0
			t := [2][2]int{{inf, inf}, {inf, inf}}
			l := -1

			for r := 0; r < n; r++ {
				if s[r] == byte('0'+a) {
					curA++
				}
				if s[r] == byte('0'+b) {
					curB++
				}

				for r-l >= k && curB-preB >= 2 {
					t[preA&1][preB&1] = min(t[preA&1][preB&1], preA-preB)
					l++
					if s[l] == byte('0'+a) {
						preA++
					}
					if s[l] == byte('0'+b) {
						preB++
					}
				}

				ans = max(ans, curA-curB-t[curA&1^1][curB&1])
			}
		}
	}

	return ans
}
```

#### TypeScript

```ts
function maxDifference(S: string, k: number): number {
    const s = S.split('').map(Number);
    let ans = -Infinity;
    for (let a = 0; a < 5; a++) {
        for (let b = 0; b < 5; b++) {
            if (a === b) {
                continue;
            }
            let [curA, curB, preA, preB] = [0, 0, 0, 0];
            const t: number[][] = [
                [Infinity, Infinity],
                [Infinity, Infinity],
            ];
            let l = -1;
            for (let r = 0; r < s.length; r++) {
                const x = s[r];
                curA += x === a ? 1 : 0;
                curB += x === b ? 1 : 0;
                while (r - l >= k && curB - preB >= 2) {
                    t[preA & 1][preB & 1] = Math.min(t[preA & 1][preB & 1], preA - preB);
                    l++;
                    preA += s[l] === a ? 1 : 0;
                    preB += s[l] === b ? 1 : 0;
                }
                ans = Math.max(ans, curA - curB - t[(curA & 1) ^ 1][curB & 1]);
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
use std::cmp::{max, min};
use std::i32::{MAX, MIN};

impl Solution {
    pub fn max_difference(S: String, k: i32) -> i32 {
        let s: Vec<usize> = S.chars().map(|c| c.to_digit(10).unwrap() as usize).collect();
        let k = k as usize;
        let mut ans = MIN;

        for a in 0..5 {
            for b in 0..5 {
                if a == b {
                    continue;
                }

                let mut curA = 0;
                let mut curB = 0;
                let mut preA = 0;
                let mut preB = 0;
                let mut t = [[MAX; 2]; 2];
                let mut l: isize = -1;

                for (r, &x) in s.iter().enumerate() {
                    curA += (x == a) as i32;
                    curB += (x == b) as i32;

                    while (r as isize - l) as usize >= k && curB - preB >= 2 {
                        let i = (preA & 1) as usize;
                        let j = (preB & 1) as usize;
                        t[i][j] = min(t[i][j], preA - preB);
                        l += 1;
                        if l >= 0 {
                            preA += (s[l as usize] == a) as i32;
                            preB += (s[l as usize] == b) as i32;
                        }
                    }

                    let i = (curA & 1 ^ 1) as usize;
                    let j = (curB & 1) as usize;
                    if t[i][j] != MAX {
                        ans = max(ans, curA - curB - t[i][j]);
                    }
                }
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
