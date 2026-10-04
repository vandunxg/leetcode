---
comments: true
difficulty: Medium
rating: 1979
source: Weekly Contest 396 Q3
tags:
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [3138. Minimum Length of Anagram Concatenation](https://leetcode.com/problems/minimum-length-of-anagram-concatenation)

[中文文档](/solution/3100-3199/3138.Minimum%20Length%20of%20Anagram%20Concatenation/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một chuỗi <code>s</code>, được biết là phép nối các <strong>anagram</strong> của một chuỗi <code>t</code>.</p>

<p>Trả về độ dài <strong>nhỏ nhất</strong> có thể có của chuỗi <code>t</code>.</p>

<p>Một <strong>anagram</strong> được tạo ra bằng cách sắp xếp lại các chữ cái của một chuỗi. Ví dụ, &quot;aab&quot;, &quot;aba&quot; và &quot;baa&quot; là các anagram của &quot;aab&quot;.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abba&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một chuỗi <code>t</code> có thể là <code>&quot;ba&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;cdef&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một chuỗi <code>t</code> có thể là <code>&quot;cdef&quot;</code>; lưu ý rằng <code>t</code> có thể bằng <code>s</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abcbcacabbaccba&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> $s$ là phép nối của các khối anagram có cùng độ dài; ta cần tìm khối ngắn nhất. Độ dài của nó phải là ước của $n$, nên chỉ có một số ít ứng viên cần xét.
>
> Một khối có độ dài $k$ là hợp lệ khi số lần xuất hiện của mỗi ký tự trong từng đoạn, nhân với $n/k$, khôi phục được số lần xuất hiện trên toàn chuỗi. Ta có thể kiểm tra một ứng viên trong một lượt duyệt $O(n)$.
>
> Đếm toàn bộ chuỗi, sau đó thử từng ước $k$ và kiểm tra từng đoạn. Kết quả đúng đầu tiên là $t$ nhỏ nhất.

<!-- thinking:end -->

Theo mô tả bài toán, độ dài của chuỗi $\textit{t}$ phải là một ước của độ dài chuỗi $\textit{s}$. Ta có thể liệt kê độ dài $k$ của chuỗi $\textit{t}$ từ nhỏ đến lớn, sau đó kiểm tra xem nó có thỏa mãn yêu cầu của bài toán hay không. Nếu có, ta trả về ngay. Như vậy, bài toán được chuyển thành việc kiểm tra xem độ dài $k$ của chuỗi $\textit{t}$ có thỏa mãn yêu cầu hay không.

Trước tiên, ta đếm số lần xuất hiện của mỗi ký tự trong chuỗi $\textit{s}$ và lưu vào một mảng hoặc hash table $\textit{cnt}$.

Tiếp theo, ta định nghĩa hàm $\textit{check}(k)$ để kiểm tra xem độ dài $k$ của chuỗi $\textit{t}$ có thỏa mãn yêu cầu hay không. Ta duyệt chuỗi $\textit{s}$, mỗi lần lấy một chuỗi con có độ dài $k$, rồi đếm số lần xuất hiện của mỗi ký tự. Nếu số lần xuất hiện của bất kỳ ký tự nào nhân với $\frac{n}{k}$ không bằng giá trị tương ứng trong $\textit{cnt}$, ta trả về $\textit{false}$. Nếu tất cả các lần kiểm tra đều thành công khi duyệt hết chuỗi, ta trả về $\textit{true}$.

Độ phức tạp thời gian là $O(n \times A)$, trong đó $n$ là độ dài chuỗi $\textit{s}$, còn $A$ là số ước của $n$. Độ phức tạp không gian là $O(|\Sigma|)$, trong đó $\Sigma$ là tập ký tự, ở đây là tập các chữ cái tiếng Anh viết thường.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minAnagramLength(self, s: str) -> int:
        def check(k: int) -> bool:
            for i in range(0, n, k):
                cnt1 = Counter(s[i : i + k])
                for c, v in cnt.items():
                    if cnt1[c] * (n // k) != v:
                        return False
            return True

        cnt = Counter(s)
        n = len(s)
        for i in range(1, n + 1):
            if n % i == 0 and check(i):
                return i
```

#### Java

```java
class Solution {
    private int n;
    private char[] s;
    private int[] cnt = new int[26];

    public int minAnagramLength(String s) {
        n = s.length();
        this.s = s.toCharArray();
        for (int i = 0; i < n; ++i) {
            ++cnt[this.s[i] - 'a'];
        }
        for (int i = 1;; ++i) {
            if (n % i == 0 && check(i)) {
                return i;
            }
        }
    }

    private boolean check(int k) {
        for (int i = 0; i < n; i += k) {
            int[] cnt1 = new int[26];
            for (int j = i; j < i + k; ++j) {
                ++cnt1[s[j] - 'a'];
            }
            for (int j = 0; j < 26; ++j) {
                if (cnt1[j] * (n / k) != cnt[j]) {
                    return false;
                }
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minAnagramLength(string s) {
        int n = s.size();
        int cnt[26]{};
        for (char c : s) {
            cnt[c - 'a']++;
        }
        auto check = [&](int k) {
            for (int i = 0; i < n; i += k) {
                int cnt1[26]{};
                for (int j = i; j < i + k; ++j) {
                    cnt1[s[j] - 'a']++;
                }
                for (int j = 0; j < 26; ++j) {
                    if (cnt1[j] * (n / k) != cnt[j]) {
                        return false;
                    }
                }
            }
            return true;
        };
        for (int i = 1;; ++i) {
            if (n % i == 0 && check(i)) {
                return i;
            }
        }
    }
};
```

#### Go

```go
func minAnagramLength(s string) int {
	n := len(s)
	cnt := [26]int{}
	for _, c := range s {
		cnt[c-'a']++
	}
	check := func(k int) bool {
		for i := 0; i < n; i += k {
			cnt1 := [26]int{}
			for j := i; j < i+k; j++ {
				cnt1[s[j]-'a']++
			}
			for j, v := range cnt {
				if cnt1[j]*(n/k) != v {
					return false
				}
			}
		}
		return true
	}
	for i := 1; ; i++ {
		if n%i == 0 && check(i) {
			return i
		}
	}
}
```

#### TypeScript

```ts
function minAnagramLength(s: string): number {
    const n = s.length;
    const cnt: Record<string, number> = {};
    for (let i = 0; i < n; i++) {
        cnt[s[i]] = (cnt[s[i]] || 0) + 1;
    }
    const check = (k: number): boolean => {
        for (let i = 0; i < n; i += k) {
            const cnt1: Record<string, number> = {};
            for (let j = i; j < i + k; j++) {
                cnt1[s[j]] = (cnt1[s[j]] || 0) + 1;
            }
            for (const [c, v] of Object.entries(cnt)) {
                if (cnt1[c] * ((n / k) | 0) !== v) {
                    return false;
                }
            }
        }
        return true;
    };
    for (let i = 1; ; ++i) {
        if (n % i === 0 && check(i)) {
            return i;
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
