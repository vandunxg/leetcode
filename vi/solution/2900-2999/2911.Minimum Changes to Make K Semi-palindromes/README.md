---
comments: true
difficulty: Hard
rating: 2607
source: Weekly Contest 368 Q4
tags:
    - Two Pointers
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [2911. Minimum Changes to Make K Semi-palindromes](https://leetcode.com/problems/minimum-changes-to-make-k-semi-palindromes)

[中文文档](/solution/2900-2999/2911.Minimum%20Changes%20to%20Make%20K%20Semi-palindromes/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> và một số nguyên <code>k</code>, hãy chia <code>s</code> thành <code>k</code> <strong><span data-keyword="substring-nonempty">các chuỗi con</span></strong> không rỗng sao cho số lần thay đổi ký tự cần thiết để biến mỗi chuỗi con thành một <strong>chuỗi bán đối xứng</strong>&nbsp;là nhỏ nhất.</p>

<p>Trả về <em><strong>số lần thay đổi ký tự tối thiểu</strong></em> cần thực hiện<em>.</em></p>

<p><strong>Chuỗi bán đối xứng</strong> là một loại chuỗi đặc biệt có thể được chia thành các <strong><span data-keyword="palindrome">chuỗi đối xứng</span></strong> dựa trên một mẫu lặp lại. Để kiểm tra một chuỗi có phải là chuỗi bán đối xứng hay không:</p>

<ol>
	<li>Chọn một ước dương <code>d</code> của độ dài chuỗi. <code>d</code> có thể nhận các giá trị từ <code>1</code> đến nhỏ hơn độ dài chuỗi. Với chuỗi có độ dài <code>1</code>, không có ước hợp lệ theo định nghĩa này, vì ước duy nhất chính là độ dài chuỗi và không được phép chọn.</li>
	<li>Với một ước <code>d</code> đã cho, chia chuỗi thành các nhóm sao cho mỗi nhóm chứa các ký tự ở những vị trí theo một mẫu lặp có độ dài <code>d</code>. Cụ thể, nhóm đầu tiên gồm các ký tự ở vị trí <code>1</code>, <code>1 + d</code>, <code>1 + 2d</code>, v.v.; nhóm thứ hai gồm các ký tự ở vị trí <code>2</code>, <code>2 + d</code>, <code>2 + 2d</code>, v.v.</li>
	<li>Chuỗi được xem là bán đối xứng nếu mỗi nhóm trong số các nhóm này là một chuỗi đối xứng.</li>
</ol>

<p>Xét chuỗi <code>&quot;abcabc&quot;</code>:</p>

<ul>
	<li>Độ dài của <code>&quot;abcabc&quot;</code> là <code>6</code>. Các ước hợp lệ là <code>1</code>, <code>2</code> và <code>3</code>.</li>
	<li>Với <code>d = 1</code>: Toàn bộ chuỗi <code>&quot;abcabc&quot;</code> tạo thành một nhóm. Nhóm này không phải là chuỗi đối xứng.</li>
	<li>Với <code>d = 2</code>:
	<ul>
		<li>Nhóm 1 (các vị trí <code>1, 3, 5</code>): <code>&quot;acb&quot;</code></li>
		<li>Nhóm 2 (các vị trí <code>2, 4, 6</code>): <code>&quot;bac&quot;</code></li>
		<li>Không nhóm nào là chuỗi đối xứng.</li>
	</ul>
	</li>
	<li>Với <code>d = 3</code>:
	<ul>
		<li>Nhóm 1 (các vị trí <code>1, 4</code>): <code>&quot;aa&quot;</code></li>
		<li>Nhóm 2 (các vị trí <code>2, 5</code>): <code>&quot;bb&quot;</code></li>
		<li>Nhóm 3 (các vị trí <code>3, 6</code>): <code>&quot;cc&quot;</code></li>
		<li>Tất cả các nhóm đều là chuỗi đối xứng. Vì vậy, <code>&quot;abcabc&quot;</code> là một chuỗi bán đối xứng.</li>
	</ul>
	</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1: </strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> s = &quot;abcac&quot;, k = 2 </span></p>

<p><strong>Đầu ra: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> 1 </span></p>

<p><strong>Giải thích: </strong> Chia <code>s</code> thành <code>&quot;ab&quot;</code> và <code>&quot;cac&quot;</code>. <code>&quot;cac&quot;</code> đã là chuỗi bán đối xứng. Thay đổi <code>&quot;ab&quot;</code> thành <code>&quot;aa&quot;</code>, khi đó nó trở thành chuỗi bán đối xứng với <code>d = 1</code>.</p>
</div>

<p><strong class="example">Ví dụ 2: </strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> s = &quot;abcdef&quot;, k = 2 </span></p>

<p><strong>Đầu ra: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> 2 </span></p>

<p><strong>Giải thích: </strong> Chia <code>s</code> thành các chuỗi con <code>&quot;abc&quot;</code> và <code>&quot;def&quot;</code>. Mỗi chuỗi cần thay đổi một ký tự để trở thành chuỗi bán đối xứng.</p>
</div>

<p><strong class="example">Ví dụ 3: </strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> s = &quot;aabbaa&quot;, k = 3 </span></p>

<p><strong>Đầu ra: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> 0 </span></p>

<p><strong>Giải thích: </strong> Chia <code>s</code> thành các chuỗi con <code>&quot;aa&quot;</code>, <code>&quot;bb&quot;</code> và <code>&quot;aa&quot;</code>. Tất cả đều đã là chuỗi bán đối xứng.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= s.length &lt;= 200</code></li>
	<li><code>1 &lt;= k &lt;= s.length / 2</code></li>
	<li><code>s</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý + DP

<!-- thinking:start -->

> **Tư duy**
>
> Chia $s$ thành $k$ chuỗi bán đối xứng với số lần thay đổi ít nhất. Vì $n \le 200$, với mỗi chuỗi con ta có thể thử từng ước $d$, đếm số cặp ký tự bán đối xứng khác nhau và lưu chi phí vào $g[i][j]$.
>
> Việc chia đoạn là bài toán DP với $k$ lần cắt quen thuộc: $f[i][j]$ là chi phí nhỏ nhất để chia $i$ ký tự đầu tiên thành $j$ phần, bằng cách duyệt vị trí cắt trước đó $h$. Phần tiền xử lý thuộc lớp $O(n^3)$ kết hợp với DP vẫn đáp ứng được giới hạn.

<!-- thinking:end -->

Tính trước chi phí để biến mỗi chuỗi con thành chuỗi bán đối xứng, sau đó dùng DP hai chiều để chia đoạn.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumChanges(self, s: str, k: int) -> int:
        n = len(s)
        g = [[inf] * (n + 1) for _ in range(n + 1)]
        for i in range(1, n + 1):
            for j in range(i, n + 1):
                m = j - i + 1
                for d in range(1, m):
                    if m % d == 0:
                        cnt = 0
                        for l in range(m):
                            r = (m // d - 1 - l // d) * d + l % d
                            if l >= r:
                                break
                            if s[i - 1 + l] != s[i - 1 + r]:
                                cnt += 1
                        g[i][j] = min(g[i][j], cnt)

        f = [[inf] * (k + 1) for _ in range(n + 1)]
        f[0][0] = 0
        for i in range(1, n + 1):
            for j in range(1, k + 1):
                for h in range(i - 1):
                    f[i][j] = min(f[i][j], f[h][j - 1] + g[h + 1][i])
        return f[n][k]
```

#### Java

```java
class Solution {
    public int minimumChanges(String s, int k) {
        int n = s.length();
        int[][] g = new int[n + 1][n + 1];
        int[][] f = new int[n + 1][k + 1];
        final int inf = 1 << 30;
        for (int i = 0; i <= n; ++i) {
            Arrays.fill(g[i], inf);
            Arrays.fill(f[i], inf);
        }
        for (int i = 1; i <= n; ++i) {
            for (int j = i; j <= n; ++j) {
                int m = j - i + 1;
                for (int d = 1; d < m; ++d) {
                    if (m % d == 0) {
                        int cnt = 0;
                        for (int l = 0; l < m; ++l) {
                            int r = (m / d - 1 - l / d) * d + l % d;
                            if (l >= r) {
                                break;
                            }
                            if (s.charAt(i - 1 + l) != s.charAt(i - 1 + r)) {
                                ++cnt;
                            }
                        }
                        g[i][j] = Math.min(g[i][j], cnt);
                    }
                }
            }
        }
        f[0][0] = 0;
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= k; ++j) {
                for (int h = 0; h < i - 1; ++h) {
                    f[i][j] = Math.min(f[i][j], f[h][j - 1] + g[h + 1][i]);
                }
            }
        }
        return f[n][k];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumChanges(string s, int k) {
        int n = s.size();
        int g[n + 1][n + 1];
        int f[n + 1][k + 1];
        memset(g, 0x3f, sizeof(g));
        memset(f, 0x3f, sizeof(f));
        f[0][0] = 0;
        for (int i = 1; i <= n; ++i) {
            for (int j = i; j <= n; ++j) {
                int m = j - i + 1;
                for (int d = 1; d < m; ++d) {
                    if (m % d == 0) {
                        int cnt = 0;
                        for (int l = 0; l < m; ++l) {
                            int r = (m / d - 1 - l / d) * d + l % d;
                            if (l >= r) {
                                break;
                            }
                            if (s[i - 1 + l] != s[i - 1 + r]) {
                                ++cnt;
                            }
                        }
                        g[i][j] = min(g[i][j], cnt);
                    }
                }
            }
        }
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j <= k; ++j) {
                for (int h = 0; h < i - 1; ++h) {
                    f[i][j] = min(f[i][j], f[h][j - 1] + g[h + 1][i]);
                }
            }
        }
        return f[n][k];
    }
};
```

#### Go

```go
func minimumChanges(s string, k int) int {
	n := len(s)
	g := make([][]int, n+1)
	f := make([][]int, n+1)
	const inf int = 1 << 30
	for i := range g {
		g[i] = make([]int, n+1)
		f[i] = make([]int, k+1)
		for j := range g[i] {
			g[i][j] = inf
		}
		for j := range f[i] {
			f[i][j] = inf
		}
	}
	f[0][0] = 0
	for i := 1; i <= n; i++ {
		for j := i; j <= n; j++ {
			m := j - i + 1
			for d := 1; d < m; d++ {
				if m%d == 0 {
					cnt := 0
					for l := 0; l < m; l++ {
						r := (m/d-1-l/d)*d + l%d
						if l >= r {
							break
						}
						if s[i-1+l] != s[i-1+r] {
							cnt++
						}
					}
					g[i][j] = min(g[i][j], cnt)
				}
			}
		}
	}
	for i := 1; i <= n; i++ {
		for j := 1; j <= k; j++ {
			for h := 0; h < i-1; h++ {
				f[i][j] = min(f[i][j], f[h][j-1]+g[h+1][i])
			}
		}
	}
	return f[n][k]
}
```

#### TypeScript

```ts
function minimumChanges(s: string, k: number): number {
    const n = s.length;
    const g = Array.from({ length: n + 1 }, () => Array.from({ length: n + 1 }, () => Infinity));
    const f = Array.from({ length: n + 1 }, () => Array.from({ length: k + 1 }, () => Infinity));
    f[0][0] = 0;
    for (let i = 1; i <= n; ++i) {
        for (let j = 1; j <= n; ++j) {
            const m = j - i + 1;
            for (let d = 1; d < m; ++d) {
                if (m % d === 0) {
                    let cnt = 0;
                    for (let l = 0; l < m; ++l) {
                        const r = (((m / d) | 0) - 1 - ((l / d) | 0)) * d + (l % d);
                        if (l >= r) {
                            break;
                        }
                        if (s[i - 1 + l] !== s[i - 1 + r]) {
                            ++cnt;
                        }
                    }
                    g[i][j] = Math.min(g[i][j], cnt);
                }
            }
        }
    }
    for (let i = 1; i <= n; ++i) {
        for (let j = 1; j <= k; ++j) {
            for (let h = 0; h < i - 1; ++h) {
                f[i][j] = Math.min(f[i][j], f[h][j - 1] + g[h + 1][i]);
            }
        }
    }
    return f[n][k];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: DP (Tối ưu bộ nhớ)

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 lưu chi phí của mọi chuỗi con trong một bảng $O(n^2)$ và duy trì thêm một chiều cho số lần cắt. Nhiều chuỗi con không bao giờ xuất hiện trong một cách chia tối ưu, vì vậy ta có thể tính memo chi phí khi cần.
>
> Chiều cắt được gộp vào mảng một chiều $dp$ trên lớp $j-1$ trước đó; ta cập nhật từ ít phần đến nhiều phần và từ phải sang trái để không ghi đè các trạng thái đang cần dùng. Bộ nhớ giảm xuống tuyến tính, còn công thức truy hồi không thay đổi.

<!-- thinking:end -->

Tính memo chi phí và gộp DP chia đoạn thành một chiều.

<!-- tabs:start -->

#### Java

```java
class Solution {
    static int inf = 200;
    List<Integer>[] factorLists;
    int n;
    int k;
    char[] ch;
    Integer[][] cost;
    public int minimumChanges(String s, int k) {
        this.k = k;
        n = s.length();
        ch = s.toCharArray();

        factorLists = getFactorLists(n);
        cost = new Integer[n + 1][n + 1];
        return calcDP();
    }
    static List<Integer>[] getFactorLists(int n) {
        List<Integer>[] l = new ArrayList[n + 1];
        for (int i = 1; i <= n; i++) {
            l[i] = new ArrayList<>();
            l[i].add(1);
        }
        for (int factor = 2; factor < n; factor++) {
            for (int num = factor + factor; num <= n; num += factor) {
                l[num].add(factor);
            }
        }
        return l;
    }
    int calcDP() {
        int[] dp = new int[n];
        for (int i = n - k * 2 + 1; i >= 1; i--) {
            dp[i] = getCost(0, i);
        }
        int bound = 0;
        for (int subs = 2; subs <= k; subs++) {
            bound = subs * 2;
            for (int i = n - 1 - k * 2 + subs * 2; i >= bound - 1; i--) {
                dp[i] = inf;
                for (int prev = bound - 3; prev < i - 1; prev++) {
                    dp[i] = Math.min(dp[i], dp[prev] + getCost(prev + 1, i));
                }
            }
        }
        return dp[n - 1];
    }
    int getCost(int l, int r) {
        if (l >= r) {
            return inf;
        }
        if (cost[l][r] != null) {
            return cost[l][r];
        }
        cost[l][r] = inf;
        for (int factor : factorLists[r - l + 1]) {
            cost[l][r] = Math.min(cost[l][r], getStepwiseCost(l, r, factor));
        }
        return cost[l][r];
    }
    int getStepwiseCost(int l, int r, int stepsize) {
        if (l >= r) {
            return 0;
        }
        int left = 0;
        int right = 0;
        int count = 0;
        for (int i = 0; i < stepsize; i++) {
            left = l + i;
            right = r - stepsize + 1 + i;
            while (left + stepsize <= right) {
                if (ch[left] != ch[right]) {
                    count++;
                }
                left += stepsize;
                right -= stepsize;
            }
        }
        return count;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
