---
comments: true
difficulty: Medium
rating: 1883
source: Weekly Contest 439 Q2
tags:
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [3472. Longest Palindromic Subsequence After at Most K Operations](https://leetcode.com/problems/longest-palindromic-subsequence-after-at-most-k-operations)

[中文文档](/solution/3400-3499/3472.Longest%20Palindromic%20Subsequence%20After%20at%20Most%20K%20Operations/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> và một số nguyên <code>k</code>.</p>

<p>Trong một thao tác, bạn có thể thay thế ký tự ở bất kỳ vị trí nào bằng chữ cái liền sau hoặc liền trước trong bảng chữ cái (quay vòng sao cho <code>&#39;a&#39;</code> đứng sau <code>&#39;z&#39;</code>). Ví dụ, thay <code>&#39;a&#39;</code> bằng chữ cái liền sau sẽ được <code>&#39;b&#39;</code>, còn thay <code>&#39;a&#39;</code> bằng chữ cái liền trước sẽ được <code>&#39;z&#39;</code>. Tương tự, thay <code>&#39;z&#39;</code> bằng chữ cái liền sau sẽ được <code>&#39;a&#39;</code>, còn thay <code>&#39;z&#39;</code> bằng chữ cái liền trước sẽ được <code>&#39;y&#39;</code>.</p>

<p>Trả về độ dài của <strong><span data-keyword="palindrome-string">phân dãy đối xứng</span> <span data-keyword="subsequence-string-nonempty">dài nhất</span></strong> của <code>s</code> có thể thu được sau khi thực hiện <strong>nhiều nhất</strong> <code>k</code> thao tác.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abced&quot;, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Thay <code>s[1]</code> bằng chữ cái liền sau, khi đó <code>s</code> trở thành <code>&quot;acced&quot;</code>.</li>
	<li>Thay <code>s[4]</code> bằng chữ cái liền trước, khi đó <code>s</code> trở thành <code>&quot;accec&quot;</code>.</li>
</ul>

<p>Phân dãy <code>&quot;ccc&quot;</code> tạo thành một palindrome có độ dài 3, đây là độ dài lớn nhất.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;</span>aaazzz<span class="example-io">&quot;, k = 4</span></p>

<p><strong>Đầu ra:</strong> 6</p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Thay <code>s[0]</code> bằng chữ cái liền trước, khi đó <code>s</code> trở thành <code>&quot;zaazzz&quot;</code>.</li>
	<li>Thay <code>s[4]</code> bằng chữ cái liền sau, khi đó <code>s</code> trở thành <code>&quot;zaazaz&quot;</code>.</li>
	<li>Thay <code>s[3]</code> bằng chữ cái liền sau, khi đó <code>s</code> trở thành <code>&quot;zaaaaz&quot;</code>.</li>
</ul>

<p>Toàn bộ chuỗi tạo thành một palindrome có độ dài 6.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 200</code></li>
	<li><code>1 &lt;= k &lt;= 200</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác đưa một chữ cái sang chữ cái liền kề trên vòng tròn, với nhiều nhất $k$ thao tác; ta cần tìm phân dãy đối xứng dài nhất. Ghép hai đầu của phân dãy phải trả chi phí bằng khoảng cách giữa chúng trên bảng chữ cái vòng.
>
> Đây là bài toán LPS kinh điển có thêm một chiều ngân sách còn lại. Trạng thái $(i,j,k)$ có kích thước $n^2(k+1)$ và phù hợp với việc ghi nhớ kết quả.
>
> Các chuyển trạng thái là bỏ qua ký tự bên trái hoặc bên phải, hoặc dùng $t=\min(|s_i-s_j|,26-|s_i-s_j|)$ để ghép chúng. Các đoạn rỗng và đoạn chỉ có một ký tự là các trường hợp cơ sở.

<!-- thinking:end -->

Ta xây dựng hàm $\textit{dfs}(i, j, k)$, biểu diễn độ dài của phân dãy đối xứng dài nhất có thể thu được trong đoạn con $s[i..j]$ với nhiều nhất $k$ thao tác. Đáp án là $\textit{dfs}(0, n - 1, k)$.

Quá trình tính hàm $\textit{dfs}(i, j, k)$ như sau:

- Nếu $i > j$, trả về $0$;
- Nếu $i = j$, trả về $1$;
- Ngược lại, ta có thể bỏ qua $s[i]$ hoặc $s[j]$ và lần lượt tính $\textit{dfs}(i + 1, j, k)$ và $\textit{dfs}(i, j - 1, k)$; hoặc thay đổi $s[i]$ và $s[j]$ thành cùng một ký tự rồi tính $\textit{dfs}(i + 1, j - 1, k - t) + 2$, trong đó $t$ là độ chênh lệch mã ASCII giữa $s[i]$ và $s[j]$.
- Trả về giá trị lớn nhất trong ba trường hợp trên.

Để tránh tính toán lặp lại, ta sử dụng tìm kiếm có ghi nhớ.

Độ phức tạp thời gian là $O(n^2 \times k)$, và độ phức tạp không gian là $O(n^2 \times k)$. Trong đó, $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestPalindromicSubsequence(self, s: str, k: int) -> int:
        @cache
        def dfs(i: int, j: int, k: int) -> int:
            if i > j:
                return 0
            if i == j:
                return 1
            res = max(dfs(i + 1, j, k), dfs(i, j - 1, k))
            d = abs(s[i] - s[j])
            t = min(d, 26 - d)
            if t <= k:
                res = max(res, dfs(i + 1, j - 1, k - t) + 2)
            return res

        s = list(map(ord, s))
        n = len(s)
        ans = dfs(0, n - 1, k)
        dfs.cache_clear()
        return ans
```

#### Java

```java
class Solution {
    private char[] s;
    private Integer[][][] f;

    public int longestPalindromicSubsequence(String s, int k) {
        this.s = s.toCharArray();
        int n = s.length();
        f = new Integer[n][n][k + 1];
        return dfs(0, n - 1, k);
    }

    private int dfs(int i, int j, int k) {
        if (i > j) {
            return 0;
        }
        if (i == j) {
            return 1;
        }
        if (f[i][j][k] != null) {
            return f[i][j][k];
        }
        int res = Math.max(dfs(i + 1, j, k), dfs(i, j - 1, k));
        int d = Math.abs(s[i] - s[j]);
        int t = Math.min(d, 26 - d);
        if (t <= k) {
            res = Math.max(res, 2 + dfs(i + 1, j - 1, k - t));
        }
        f[i][j][k] = res;
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestPalindromicSubsequence(string s, int k) {
        int n = s.size();
        vector f(n, vector(n, vector<int>(k + 1, -1)));
        auto dfs = [&](this auto&& dfs, int i, int j, int k) -> int {
            if (i > j) {
                return 0;
            }
            if (i == j) {
                return 1;
            }
            if (f[i][j][k] != -1) {
                return f[i][j][k];
            }
            int res = max(dfs(i + 1, j, k), dfs(i, j - 1, k));
            int d = abs(s[i] - s[j]);
            int t = min(d, 26 - d);
            if (t <= k) {
                res = max(res, 2 + dfs(i + 1, j - 1, k - t));
            }
            return f[i][j][k] = res;
        };
        return dfs(0, n - 1, k);
    }
};
```

#### Go

```go
func longestPalindromicSubsequence(s string, k int) int {
	n := len(s)
	f := make([][][]int, n)
	for i := range f {
		f[i] = make([][]int, n)
		for j := range f[i] {
			f[i][j] = make([]int, k+1)
			for l := range f[i][j] {
				f[i][j][l] = -1
			}
		}
	}
	var dfs func(int, int, int) int
	dfs = func(i, j, k int) int {
		if i > j {
			return 0
		}
		if i == j {
			return 1
		}
		if f[i][j][k] != -1 {
			return f[i][j][k]
		}
		res := max(dfs(i+1, j, k), dfs(i, j-1, k))
		d := abs(int(s[i]) - int(s[j]))
		t := min(d, 26-d)
		if t <= k {
			res = max(res, 2+dfs(i+1, j-1, k-t))
		}
		f[i][j][k] = res
		return res
	}
	return dfs(0, n-1, k)
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function longestPalindromicSubsequence(s: string, k: number): number {
    const n = s.length;
    const sCodes = s.split('').map(c => c.charCodeAt(0));
    const f: number[][][] = Array.from({ length: n }, () =>
        Array.from({ length: n }, () => Array(k + 1).fill(-1)),
    );

    function dfs(i: number, j: number, k: number): number {
        if (i > j) {
            return 0;
        }
        if (i === j) {
            return 1;
        }

        if (f[i][j][k] !== -1) {
            return f[i][j][k];
        }

        let res = Math.max(dfs(i + 1, j, k), dfs(i, j - 1, k));
        const d = Math.abs(sCodes[i] - sCodes[j]);
        const t = Math.min(d, 26 - d);
        if (t <= k) {
            res = Math.max(res, 2 + dfs(i + 1, j - 1, k - t));
        }
        return (f[i][j][k] = res);
    }

    return dfs(0, n - 1, k);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
