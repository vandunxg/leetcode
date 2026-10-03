---
comments: true
difficulty: Hard
rating: 2515
source: Biweekly Contest 78 Q4
tags:
    - Hash Table
    - String
    - Dynamic Programming
    - Enumeration
---

<!-- problem:start -->

# [2272. Substring With Largest Variance](https://leetcode.com/problems/substring-with-largest-variance)

[中文文档](/solution/2200-2299/2272.Substring%20With%20Largest%20Variance/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Độ lệch</strong> của một chuỗi được định nghĩa là hiệu lớn nhất giữa số lần xuất hiện của <strong>bất kỳ</strong> <code>2</code> ký tự nào trong chuỗi. Lưu ý rằng hai ký tự này có thể giống hoặc khác nhau.</p>

<p>Cho một chuỗi <code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường, hãy trả về <em><strong>độ lệch lớn nhất</strong> có thể đạt được trong mọi <strong>chuỗi con</strong> của</em> <code>s</code>.</p>

<p><strong>Chuỗi con</strong> là một dãy ký tự liên tiếp trong một chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aababbb&quot;
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Tất cả các độ lệch có thể cùng với chuỗi con tương ứng được liệt kê dưới đây:
- Độ lệch 0 với các chuỗi con &quot;a&quot;, &quot;aa&quot;, &quot;ab&quot;, &quot;abab&quot;, &quot;aababb&quot;, &quot;ba&quot;, &quot;b&quot;, &quot;bb&quot; và &quot;bbb&quot;.
- Độ lệch 1 với các chuỗi con &quot;aab&quot;, &quot;aba&quot;, &quot;abb&quot;, &quot;aabab&quot;, &quot;ababb&quot;, &quot;aababbb&quot; và &quot;bab&quot;.
- Độ lệch 2 với các chuỗi con &quot;aaba&quot;, &quot;ababbb&quot;, &quot;abbb&quot; và &quot;babb&quot;.
- Độ lệch 3 với chuỗi con &quot;babbb&quot;.
Vì độ lệch lớn nhất có thể là 3, ta trả về giá trị này.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcde&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
Không có chữ cái nào xuất hiện nhiều hơn một lần trong s, nên độ lệch của mọi chuỗi con đều bằng 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>4</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê + Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Độ lệch của một chuỗi con là khoảng cách giữa tần suất lớn nhất và nhỏ nhất của các chữ cái trong chuỗi. Với $n \le 10^4$, việc liệt kê mọi chuỗi con là không khả thi. Chỉ có $26$ chữ cái, nên độ lệch phụ thuộc vào một cặp $(a,b)$: coi $a$ là $+1$, $b$ là $-1$ rồi tìm maximum subarray có chứa cả hai.
>
> $f[0]$ là một đoạn cuối chỉ chứa $a$; $f[1]$ là hiệu lớn nhất của một đoạn đã chứa cả hai ký tự. Mỗi $a$ làm tăng cả hai giá trị; mỗi $b$ đặt $f[1]=\max(f[1],f[0])-1$ và xóa $f[0]$. $f[1]$ bắt đầu từ $-\infty$ để ta không bao giờ tính một đoạn không có $b$.

<!-- thinking:end -->

Vì tập ký tự chỉ chứa các chữ cái viết thường, ta có thể xét lần lượt ký tự xuất hiện nhiều nhất $a$ và ký tự xuất hiện ít nhất $b$. Với một chuỗi con, hiệu số lần xuất hiện của hai ký tự này chính là độ lệch của chuỗi con.

Cụ thể, ta dùng hai vòng lặp để liệt kê $a$ và $b$. Ta dùng $f[0]$ để ghi nhận số lần xuất hiện liên tiếp của ký tự $a$ kết thúc tại ký tự hiện tại, và $f[1]$ để ghi nhận độ lệch của chuỗi con kết thúc tại ký tự hiện tại và chứa cả $a$ lẫn $b$. Ta duyệt để tìm giá trị lớn nhất của $f[1]$.

Công thức chuyển trạng thái như sau:

1. Nếu ký tự hiện tại là $a$, tăng cả $f[0]$ và $f[1]$ lên $1$;
2. Nếu ký tự hiện tại là $b$, ta có $f[1] = \max(f[1] - 1, f[0] - 1)$, và $f[0] = 0$;
3. Nếu không phải hai trường hợp trên thì không cần xét.

Lưu ý rằng việc khởi tạo $f[1]$ bằng một giá trị âm rất nhỏ đảm bảo việc cập nhật đáp án là hợp lệ.

Độ phức tạp thời gian là $O(n \times |\Sigma|^2)$, trong đó $n$ là độ dài chuỗi và $|\Sigma|$ là kích thước của tập ký tự. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestVariance(self, s: str) -> int:
        ans = 0
        for a, b in permutations(ascii_lowercase, 2):
            if a == b:
                continue
            f = [0, -inf]
            for c in s:
                if c == a:
                    f[0], f[1] = f[0] + 1, f[1] + 1
                elif c == b:
                    f[1] = max(f[1] - 1, f[0] - 1)
                    f[0] = 0
                if ans < f[1]:
                    ans = f[1]
        return ans
```

#### Java

```java
class Solution {
    public int largestVariance(String s) {
        int n = s.length();
        int ans = 0;
        for (char a = 'a'; a <= 'z'; ++a) {
            for (char b = 'a'; b <= 'z'; ++b) {
                if (a == b) {
                    continue;
                }
                int[] f = new int[] {0, -n};
                for (int i = 0; i < n; ++i) {
                    if (s.charAt(i) == a) {
                        f[0]++;
                        f[1]++;
                    } else if (s.charAt(i) == b) {
                        f[1] = Math.max(f[0] - 1, f[1] - 1);
                        f[0] = 0;
                    }
                    ans = Math.max(ans, f[1]);
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
    int largestVariance(string s) {
        int n = s.size();
        int ans = 0;
        for (char a = 'a'; a <= 'z'; ++a) {
            for (char b = 'a'; b <= 'z'; ++b) {
                if (a == b) {
                    continue;
                }
                int f[2] = {0, -n};
                for (char c : s) {
                    if (c == a) {
                        f[0]++;
                        f[1]++;
                    } else if (c == b) {
                        f[1] = max(f[1] - 1, f[0] - 1);
                        f[0] = 0;
                    }
                    ans = max(ans, f[1]);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func largestVariance(s string) int {
	ans, n := 0, len(s)
	for a := 'a'; a <= 'z'; a++ {
		for b := 'a'; b <= 'z'; b++ {
			if a == b {
				continue
			}
			f := [2]int{0, -n}
			for _, c := range s {
				if c == a {
					f[0]++
					f[1]++
				} else if c == b {
					f[1] = max(f[1]-1, f[0]-1)
					f[0] = 0
				}
				ans = max(ans, f[1])
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function largestVariance(s: string): number {
    const n: number = s.length;
    let ans: number = 0;
    for (let a = 97; a <= 122; ++a) {
        for (let b = 97; b <= 122; ++b) {
            if (a === b) {
                continue;
            }
            const f: number[] = [0, -n];
            for (let i = 0; i < n; ++i) {
                if (s.charCodeAt(i) === a) {
                    f[0]++;
                    f[1]++;
                } else if (s.charCodeAt(i) === b) {
                    f[1] = Math.max(f[0] - 1, f[1] - 1);
                    f[0] = 0;
                }
                ans = Math.max(ans, f[1]);
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
