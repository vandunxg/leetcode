---
comments: true
difficulty: Hard
rating: 2492
source: Weekly Contest 453 Q4
tags:
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [3579. Minimum Steps to Convert String with Operations](https://leetcode.com/problems/minimum-steps-to-convert-string-with-operations)

[中文文档](/solution/3500-3599/3579.Minimum%20Steps%20to%20Convert%20String%20with%20Operations/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai chuỗi <code>word1</code> và <code>word2</code> có cùng độ dài. Bạn cần biến đổi <code>word1</code> thành <code>word2</code>.</p>

<p>Để làm điều này, hãy chia <code>word1</code> thành một hoặc nhiều <strong>các <span data-keyword="substring-nonempty">chuỗi con</span> liên tiếp</strong>. Với mỗi chuỗi con <code>substr</code>, bạn có thể thực hiện các thao tác sau:</p>

<ol>
	<li>
	<p><strong>Thay thế:</strong> Thay ký tự tại một chỉ số bất kỳ của <code>substr</code> bằng một chữ cái tiếng Anh thường khác.</p>
	</li>
	<li>
	<p><strong>Hoán đổi:</strong> Hoán đổi hai ký tự bất kỳ trong <code>substr</code>.</p>
	</li>
	<li>
	<p><strong>Đảo chuỗi con:</strong> Đảo ngược <code>substr</code>.</p>
	</li>
</ol>

<p>Mỗi thao tác trên được tính là <strong>một</strong> thao tác, và mỗi ký tự của mỗi chuỗi con chỉ có thể được sử dụng nhiều nhất một lần cho mỗi loại thao tác (tức là không chỉ số nào được tham gia vào nhiều hơn một lần thay thế, một lần hoán đổi hoặc một lần đảo).</p>

<p>Trả về <strong>số thao tác nhỏ nhất</strong> cần thiết để biến đổi <code>word1</code> thành <code>word2</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word1 = &quot;abcdf&quot;, word2 = &quot;dacbe&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chia <code>word1</code> thành <code>&quot;ab&quot;</code>, <code>&quot;c&quot;</code> và <code>&quot;df&quot;</code>. Các thao tác là:</p>

<ul>
	<li>Với chuỗi con <code>&quot;ab&quot;</code>,

    <ul>
    <li>Thực hiện thao tác loại 3 trên <code>&quot;ab&quot; -&gt; &quot;ba&quot;</code>.</li>
    <li>Thực hiện thao tác loại 1 trên <code>&quot;ba&quot; -&gt; &quot;da&quot;</code>.</li>
    </ul>
    </li>
    <li>Với chuỗi con <code>&quot;c&quot;</code>, không thực hiện thao tác nào.</li>
    <li>Với chuỗi con <code>&quot;df&quot;</code>,
    <ul>
    <li>Thực hiện thao tác loại 1 trên <code>&quot;df&quot; -&gt; &quot;bf&quot;</code>.</li>
    <li>Thực hiện thao tác loại 1 trên <code>&quot;bf&quot; -&gt; &quot;be&quot;</code>.</li>
    </ul>
    </li>

</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word1 = &quot;abceded&quot;, word2 = &quot;baecfef&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chia <code>word1</code> thành <code>&quot;ab&quot;</code>, <code>&quot;ce&quot;</code> và <code>&quot;ded&quot;</code>. Các thao tác là:</p>

<ul>
	<li>Với chuỗi con <code>&quot;ab&quot;</code>,

    <ul>
    <li>Thực hiện thao tác loại 2 trên <code>&quot;ab&quot; -&gt; &quot;ba&quot;</code>.</li>
    </ul>
    </li>
    <li>Với chuỗi con <code>&quot;ce&quot;</code>,
    <ul>
    <li>Thực hiện thao tác loại 2 trên <code>&quot;ce&quot; -&gt; &quot;ec&quot;</code>.</li>
    </ul>
    </li>
    <li>Với chuỗi con <code>&quot;ded&quot;</code>,
    <ul>
    <li>Thực hiện thao tác loại 1 trên <code>&quot;ded&quot; -&gt; &quot;fed&quot;</code>.</li>
    <li>Thực hiện thao tác loại 1 trên <code>&quot;fed&quot; -&gt; &quot;fef&quot;</code>.</li>
    </ul>
    </li>

</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word1 = &quot;abcdef&quot;, word2 = &quot;fedabc&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chia <code>word1</code> thành <code>&quot;abcdef&quot;</code>. Các thao tác là:</p>

<ul>
	<li>Với chuỗi con <code>&quot;abcdef&quot;</code>,

    <ul>
    <li>Thực hiện thao tác loại 3 trên <code>&quot;abcdef&quot; -&gt; &quot;fedcba&quot;</code>.</li>
    <li>Thực hiện thao tác loại 2 trên <code>&quot;fedcba&quot; -&gt; &quot;fedabc&quot;</code>.</li>
    </ul>
    </li>

</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= word1.length == word2.length &lt;= 100</code></li>
	<li><code>word1</code> và <code>word2</code> chỉ gồm các chữ cái tiếng Anh thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Việc biến đổi được chia thành các đoạn; mỗi đoạn có thể được đảo toàn bộ, sau đó chỉnh sửa bằng các phép thay thế hoặc hoán đổi theo cặp. $f[i]$ là kết quả nhỏ nhất đối với tiền tố có độ dài $i$; vị trí cắt trước đó là một $j$ nào đó.
>
> $\textit{calc}(l,r,\textit{rev})$ đếm số phép thay thế trên một đoạn có hoặc không đảo: một cặp $(a,b)$ có thể triệt tiêu một cặp $(b,a)$ xuất hiện sau đó bằng một phép hoán đổi. $f[i]$ lấy giá trị nhỏ hơn giữa “không reverse” và “thực hiện một phép reverse rồi gọi calc”.

<!-- thinking:end -->

Ta định nghĩa $f[i]$ là số thao tác nhỏ nhất cần thiết để biến đổi $i$ ký tự đầu tiên của $\textit{word1}$ thành $i$ ký tự đầu tiên của $\textit{word2}$. Đáp án là $f[n]$, trong đó $n$ là độ dài của cả $\textit{word1}$ và $\textit{word2}$.

Ta có thể tính $f[i]$ bằng cách duyệt qua mọi vị trí chia có thể. Với mỗi vị trí chia $j$, ta cần tính số thao tác nhỏ nhất cần thiết để biến đổi $\textit{word1}[j:i]$ thành $\textit{word2}[j:i]$.

Ta có thể dùng hàm hỗ trợ $\text{calc}(l, r, \text{rev})$ để tính số thao tác nhỏ nhất cần thiết nhằm biến đổi $\textit{word1}[l:r]$ thành $\textit{word2}[l:r]$, trong đó $\text{rev}$ cho biết có đảo chuỗi con hay không. Vì kết quả của việc thực hiện các thao tác khác trước hay sau phép đảo là như nhau, ta chỉ cần xét hai trường hợp: không đảo, và đảo một lần trước khi thực hiện các thao tác khác. Do đó, $f[i] = \min_{j < i} (f[j] + \min(\text{calc}(j, i-1, \text{false}), 1 + \text{calc}(j, i-1, \text{true})))$.

Tiếp theo, ta cần cài đặt hàm $\text{calc}(l, r, \text{rev})$. Ta dùng một mảng hai chiều $cnt$ để ghi lại trạng thái ghép cặp của các ký tự giữa $\textit{word1}$ và $\textit{word2}$. Với mỗi cặp ký tự $(a, b)$, nếu $a \neq b$, ta kiểm tra xem $cnt[b][a] > 0$ hay không. Nếu có, ta có thể ghép cặp chúng và giảm một thao tác; nếu không, ta cần thêm một thao tác và tăng $cnt[a][b]$ lên $1$.

Độ phức tạp thời gian là $O(n^3 + |\Sigma|^2)$ và độ phức tạp không gian là $O(n + |\Sigma|^2)$, trong đó $n$ là độ dài chuỗi và $|\Sigma|$ là kích thước của bộ ký tự (bằng $26$ trong bài này).

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, word1: str, word2: str) -> int:
        def calc(l: int, r: int, rev: bool) -> int:
            cnt = Counter()
            res = 0
            for i in range(l, r + 1):
                j = r - (i - l) if rev else i
                a, b = word1[j], word2[i]
                if a != b:
                    if cnt[(b, a)] > 0:
                        cnt[(b, a)] -= 1
                    else:
                        cnt[(a, b)] += 1
                        res += 1
            return res

        n = len(word1)
        f = [inf] * (n + 1)
        f[0] = 0
        for i in range(1, n + 1):
            for j in range(i):
                t = min(calc(j, i - 1, False), 1 + calc(j, i - 1, True))
                f[i] = min(f[i], f[j] + t)
        return f[n]
```

#### Java

```java
class Solution {
    public int minOperations(String word1, String word2) {
        int n = word1.length();
        int[] f = new int[n + 1];
        Arrays.fill(f, Integer.MAX_VALUE);
        f[0] = 0;
        for (int i = 1; i <= n; i++) {
            for (int j = 0; j < i; j++) {
                int a = calc(word1, word2, j, i - 1, false);
                int b = 1 + calc(word1, word2, j, i - 1, true);
                int t = Math.min(a, b);
                f[i] = Math.min(f[i], f[j] + t);
            }
        }
        return f[n];
    }

    private int calc(String word1, String word2, int l, int r, boolean rev) {
        int[][] cnt = new int[26][26];
        int res = 0;
        for (int i = l; i <= r; i++) {
            int j = rev ? r - (i - l) : i;
            int a = word1.charAt(j) - 'a';
            int b = word2.charAt(i) - 'a';
            if (a != b) {
                if (cnt[b][a] > 0) {
                    cnt[b][a]--;
                } else {
                    cnt[a][b]++;
                    res++;
                }
            }
        }
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minOperations(string word1, string word2) {
        int n = word1.length();
        vector<int> f(n + 1, INT_MAX);
        f[0] = 0;

        for (int i = 1; i <= n; ++i) {
            for (int j = 0; j < i; ++j) {
                int a = calc(word1, word2, j, i - 1, false);
                int b = 1 + calc(word1, word2, j, i - 1, true);
                int t = min(a, b);
                f[i] = min(f[i], f[j] + t);
            }
        }

        return f[n];
    }

private:
    int calc(const string& word1, const string& word2, int l, int r, bool rev) {
        int cnt[26][26] = {0};
        int res = 0;

        for (int i = l; i <= r; ++i) {
            int j = rev ? r - (i - l) : i;
            int a = word1[j] - 'a';
            int b = word2[i] - 'a';

            if (a != b) {
                if (cnt[b][a] > 0) {
                    cnt[b][a]--;
                } else {
                    cnt[a][b]++;
                    res++;
                }
            }
        }

        return res;
    }
};
```

#### Go

```go
func minOperations(word1 string, word2 string) int {
	n := len(word1)
	f := make([]int, n+1)
	for i := range f {
		f[i] = math.MaxInt32
	}
	f[0] = 0

	calc := func(l, r int, rev bool) int {
		var cnt [26][26]int
		res := 0

		for i := l; i <= r; i++ {
			j := i
			if rev {
				j = r - (i - l)
			}
			a := word1[j] - 'a'
			b := word2[i] - 'a'

			if a != b {
				if cnt[b][a] > 0 {
					cnt[b][a]--
				} else {
					cnt[a][b]++
					res++
				}
			}
		}

		return res
	}

	for i := 1; i <= n; i++ {
		for j := 0; j < i; j++ {
			a := calc(j, i-1, false)
			b := 1 + calc(j, i-1, true)
			t := min(a, b)
			f[i] = min(f[i], f[j]+t)
		}
	}

	return f[n]
}
```

#### TypeScript

```ts
function minOperations(word1: string, word2: string): number {
    const n = word1.length;
    const f = Array(n + 1).fill(Number.MAX_SAFE_INTEGER);
    f[0] = 0;

    function calc(l: number, r: number, rev: boolean): number {
        const cnt: number[][] = Array.from({ length: 26 }, () => Array(26).fill(0));
        let res = 0;

        for (let i = l; i <= r; i++) {
            const j = rev ? r - (i - l) : i;
            const a = word1.charCodeAt(j) - 97;
            const b = word2.charCodeAt(i) - 97;

            if (a !== b) {
                if (cnt[b][a] > 0) {
                    cnt[b][a]--;
                } else {
                    cnt[a][b]++;
                    res++;
                }
            }
        }

        return res;
    }

    for (let i = 1; i <= n; i++) {
        for (let j = 0; j < i; j++) {
            const a = calc(j, i - 1, false);
            const b = 1 + calc(j, i - 1, true);
            const t = Math.min(a, b);
            f[i] = Math.min(f[i], f[j] + t);
        }
    }

    return f[n];
}
```

#### Rust

```rust
impl Solution {
    pub fn min_operations(word1: String, word2: String) -> i32 {
        let n = word1.len();
        let word1 = word1.as_bytes();
        let word2 = word2.as_bytes();
        let mut f = vec![i32::MAX; n + 1];
        f[0] = 0;

        for i in 1..=n {
            for j in 0..i {
                let a = Self::calc(word1, word2, j, i - 1, false);
                let b = 1 + Self::calc(word1, word2, j, i - 1, true);
                let t = a.min(b);
                f[i] = f[i].min(f[j] + t);
            }
        }

        f[n]
    }

    fn calc(word1: &[u8], word2: &[u8], l: usize, r: usize, rev: bool) -> i32 {
        let mut cnt = [[0i32; 26]; 26];
        let mut res = 0;

        for i in l..=r {
            let j = if rev { r - (i - l) } else { i };
            let a = (word1[j] - b'a') as usize;
            let b = (word2[i] - b'a') as usize;

            if a != b {
                if cnt[b][a] > 0 {
                    cnt[b][a] -= 1;
                } else {
                    cnt[a][b] += 1;
                    res += 1;
                }
            }
        }

        res
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
