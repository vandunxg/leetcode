---
comments: true
difficulty: Easy
rating: 1348
source: Weekly Contest 339 Q1
tags:
    - String
---

<!-- problem:start -->

# [2609. Find the Longest Balanced Substring of a Binary String](https://leetcode.com/problems/find-the-longest-balanced-substring-of-a-binary-string)

[中文文档](/solution/2600-2699/2609.Find%20the%20Longest%20Balanced%20Substring%20of%20a%20Binary%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi nhị phân <code>s</code> chỉ gồm các số 0 và 1.</p>

<p>Một chuỗi con của <code>s</code> được gọi là cân bằng nếu <strong>tất cả số 0 đều đứng trước các số 1</strong> và số lượng số 0 bằng số lượng số 1 trong chuỗi con. Lưu ý rằng chuỗi con rỗng cũng được xem là chuỗi con cân bằng.</p>

<p>Hãy trả về <em>độ dài của chuỗi con cân bằng dài nhất của </em><code>s</code>.</p>

<p><b>Chuỗi con</b> là một dãy ký tự liên tiếp trong một chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;01000111&quot;
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Chuỗi con cân bằng dài nhất là &quot;000111&quot;, có độ dài 6.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;00111&quot;
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Chuỗi con cân bằng dài nhất là &quot;0011&quot;, có độ dài 4.&nbsp;
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;111&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có chuỗi con cân bằng nào ngoài chuỗi con rỗng, nên đáp án là 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 50</code></li>
	<li><code>&#39;0&#39; &lt;= s[i] &lt;= &#39;1&#39;</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Vét cạn

<!-- thinking:start -->

> **Tư duy**
>
> Một chuỗi con cân bằng gồm các số 0 đứng trước cùng số lượng số 1. Với $n \le 50$, ta có thể liệt kê $O(n^2)$ chuỗi con và quét từng chuỗi trong $O(n)$.
>
> Với mỗi đoạn, ta đếm số lượng số 1, yêu cầu mọi số 0 phải đứng trước số 1 đầu tiên, đồng thời yêu cầu số lượng số 1 đúng bằng một nửa độ dài.

<!-- thinking:end -->

Vì miền giá trị của $n$ nhỏ, ta có thể liệt kê tất cả chuỗi con $s[i..j]$ để kiểm tra xem đó có phải là chuỗi cân bằng hay không. Nếu đúng, ta cập nhật đáp án.

Độ phức tạp thời gian là $O(n^3)$, còn độ phức tạp không gian là $O(1)$. Trong đó $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findTheLongestBalancedSubstring(self, s: str) -> int:
        def check(i, j):
            cnt = 0
            for k in range(i, j + 1):
                if s[k] == '1':
                    cnt += 1
                elif cnt:
                    return False
            return cnt * 2 == (j - i + 1)

        n = len(s)
        ans = 0
        for i in range(n):
            for j in range(i + 1, n):
                if check(i, j):
                    ans = max(ans, j - i + 1)
        return ans
```

#### Java

```java
class Solution {
    public int findTheLongestBalancedSubstring(String s) {
        int n = s.length();
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                if (check(s, i, j)) {
                    ans = Math.max(ans, j - i + 1);
                }
            }
        }
        return ans;
    }

    private boolean check(String s, int i, int j) {
        int cnt = 0;
        for (int k = i; k <= j; ++k) {
            if (s.charAt(k) == '1') {
                ++cnt;
            } else if (cnt > 0) {
                return false;
            }
        }
        return cnt * 2 == j - i + 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findTheLongestBalancedSubstring(string s) {
        int n = s.size();
        int ans = 0;
        auto check = [&](int i, int j) -> bool {
            int cnt = 0;
            for (int k = i; k <= j; ++k) {
                if (s[k] == '1') {
                    ++cnt;
                } else if (cnt) {
                    return false;
                }
            }
            return cnt * 2 == j - i + 1;
        };
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                if (check(i, j)) {
                    ans = max(ans, j - i + 1);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findTheLongestBalancedSubstring(s string) (ans int) {
	n := len(s)
	check := func(i, j int) bool {
		cnt := 0
		for k := i; k <= j; k++ {
			if s[k] == '1' {
				cnt++
			} else if cnt > 0 {
				return false
			}
		}
		return cnt*2 == j-i+1
	}
	for i := 0; i < n; i++ {
		for j := i + 1; j < n; j++ {
			if check(i, j) {
				ans = max(ans, j-i+1)
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function findTheLongestBalancedSubstring(s: string): number {
    const n = s.length;
    let ans = 0;
    const check = (i: number, j: number): boolean => {
        let cnt = 0;
        for (let k = i; k <= j; ++k) {
            if (s[k] === '1') {
                ++cnt;
            } else if (cnt > 0) {
                return false;
            }
        }
        return cnt * 2 === j - i + 1;
    };
    for (let i = 0; i < n; ++i) {
        for (let j = i + 1; j < n; j += 2) {
            if (check(i, j)) {
                ans = Math.max(ans, j - i + 1);
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_the_longest_balanced_substring(s: String) -> i32 {
        let check = |i: usize, j: usize| -> bool {
            let mut cnt = 0;

            for k in i..=j {
                if s.as_bytes()[k] == b'1' {
                    cnt += 1;
                } else if cnt > 0 {
                    return false;
                }
            }

            cnt * 2 == j - i + 1
        };

        let mut ans = 0;
        let n = s.len();
        for i in 0..n - 1 {
            for j in (i + 1..n).rev() {
                if j - i + 1 < ans {
                    break;
                }

                if check(i, j) {
                    ans = std::cmp::max(ans, j - i + 1);
                    break;
                }
            }
        }

        ans as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Tối ưu hóa việc liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 quét mọi đoạn trong thời gian bậc ba. Một chuỗi hợp lệ chỉ gồm một đoạn các số 0 nối tiếp theo sau bởi một đoạn các số 1, vì vậy ta chỉ cần lưu độ dài của hai đoạn này.
>
> Khi gặp số 0 sau các số 1, ta bắt đầu một cặp đoạn mới; khi gặp số 1, ta cập nhật đáp án bằng $2\times\min(\textit{zero},\textit{one})$. Chỉ cần một lượt duyệt.

<!-- thinking:end -->

Ta dùng các biến $zero$ và $one$ để ghi nhận số lượng số $0$ và $1$ liên tiếp.

Duyệt chuỗi $s$, với ký tự hiện tại $c$:

- Nếu ký tự hiện tại là `'0'`, ta kiểm tra xem $one$ có lớn hơn $0$ hay không. Nếu có, ta đặt lại $zero$ và $one$ về $0$, sau đó cộng $1$ vào $zero$.
- Nếu ký tự hiện tại là `'1'`, ta cộng $1$ vào $one$, rồi cập nhật đáp án thành $ans = max(ans, 2 \times min(one, zero))$.

Sau khi duyệt xong, ta sẽ nhận được độ dài của chuỗi con cân bằng dài nhất.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(1)$. Trong đó $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findTheLongestBalancedSubstring(self, s: str) -> int:
        ans = zero = one = 0
        for c in s:
            if c == '0':
                if one:
                    zero = one = 0
                zero += 1
            else:
                one += 1
                ans = max(ans, 2 * min(one, zero))
        return ans
```

#### Java

```java
class Solution {
    public int findTheLongestBalancedSubstring(String s) {
        int zero = 0, one = 0;
        int ans = 0, n = s.length();
        for (int i = 0; i < n; ++i) {
            if (s.charAt(i) == '0') {
                if (one > 0) {
                    zero = 0;
                    one = 0;
                }
                ++zero;
            } else {
                ans = Math.max(ans, 2 * Math.min(zero, ++one));
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
    int findTheLongestBalancedSubstring(string s) {
        int zero = 0, one = 0;
        int ans = 0;
        for (char& c : s) {
            if (c == '0') {
                if (one > 0) {
                    zero = 0;
                    one = 0;
                }
                ++zero;
            } else {
                ans = max(ans, 2 * min(zero, ++one));
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findTheLongestBalancedSubstring(s string) (ans int) {
	zero, one := 0, 0
	for _, c := range s {
		if c == '0' {
			if one > 0 {
				zero, one = 0, 0
			}
			zero++
		} else {
			one++
			ans = max(ans, 2*min(zero, one))
		}
	}
	return
}
```

#### TypeScript

```ts
function findTheLongestBalancedSubstring(s: string): number {
    let zero = 0;
    let one = 0;
    let ans = 0;
    for (const c of s) {
        if (c === '0') {
            if (one > 0) {
                zero = 0;
                one = 0;
            }
            ++zero;
        } else {
            ans = Math.max(ans, 2 * Math.min(zero, ++one));
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_the_longest_balanced_substring(s: String) -> i32 {
        let mut zero = 0;
        let mut one = 0;
        let mut ans = 0;

        for &c in s.as_bytes().iter() {
            if c == b'0' {
                if one > 0 {
                    zero = 0;
                    one = 0;
                }
                zero += 1;
            } else {
                one += 1;
                ans = std::cmp::max(ans, std::cmp::min(zero, one) * 2);
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
