---
comments: true
difficulty: Hard
rating: 2081
source: Biweekly Contest 38 Q4
tags:
    - Array
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [1639. Number of Ways to Form a Target String Given a Dictionary](https://leetcode.com/problems/number-of-ways-to-form-a-target-string-given-a-dictionary)

[中文文档](/solution/1600-1699/1639.Number%20of%20Ways%20to%20Form%20a%20Target%20String%20Given%20a%20Dictionary/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một danh sách các chuỗi của<strong>same length</strong> <code>words</code>và một chuỗi<code>target</code>.</p>

<p>Nhiệm vụ của bạn là hình thành<code>target</code>sử dụng cái đã cho<code>words</code>theo các quy tắc sau:</p>

<ul>
	<li><code>target</code>nên được hình thành từ trái sang phải.</li>
	<li>Để hình thành<code>i<sup>th</sup></code>tính cách (<strong>0-indexed</strong>) của<code>target</code>, bạn có thể chọn<code>k<sup>th</sup></code>nhân vật của<code>j<sup>th</sup></code>xâu chuỗi vào<code>words</code>nếu như<code>target[i] = words[j][k]</code>.</li>
	<li>Một khi bạn sử dụng<code>k<sup>th</sup></code>nhân vật của<code>j<sup>th</sup></code>chuỗi của<code>words</code>, Bạn<strong>can no longer</strong>sử dụng<code>x<sup>th</sup></code>ký tự của bất kỳ chuỗi nào trong<code>words</code>Ở đâu<code>x &lt;= k</code>. Nói cách khác, tất cả các ký tự ở bên trái hoặc ở chỉ mục<code>k</code>trở nên không thể sử dụng được cho mọi chuỗi.</li>
	<li>Lặp lại quá trình cho đến khi bạn tạo thành chuỗi<code>target</code>.</li>
</ul>

<p><strong>Notice</strong>mà bạn có thể sử dụng<strong>multiple characters</strong>từ<strong>same string</strong>TRONG<code>words</code>miễn là đáp ứng được các điều kiện trên.</p>

<p>Trở lại<em>the number of ways to form <code>target</code> from <code>words</code></em>. Vì câu trả lời có thể quá lớn nên hãy trả lại<strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> words = [&quot;acca&quot;,&quot;bbbb&quot;,&quot;caca&quot;], target = &quot;aba&quot;
<strong>Output:</strong> 6
<strong>Explanation:</strong> There are 6 ways to form target.
&quot;aba&quot; -&gt; index 0 (&quot;<u>a</u>cca&quot;), index 1 (&quot;b<u>b</u>bb&quot;), index 3 (&quot;cac<u>a</u>&quot;)
&quot;aba&quot; -&gt; index 0 (&quot;<u>a</u>cca&quot;), index 2 (&quot;bb<u>b</u>b&quot;), index 3 (&quot;cac<u>a</u>&quot;)
&quot;aba&quot; -&gt; index 0 (&quot;<u>a</u>cca&quot;), index 1 (&quot;b<u>b</u>bb&quot;), index 3 (&quot;acc<u>a</u>&quot;)
&quot;aba&quot; -&gt; index 0 (&quot;<u>a</u>cca&quot;), index 2 (&quot;bb<u>b</u>b&quot;), index 3 (&quot;acc<u>a</u>&quot;)
&quot;aba&quot; -&gt; index 1 (&quot;c<u>a</u>ca&quot;), index 2 (&quot;bb<u>b</u>b&quot;), index 3 (&quot;acc<u>a</u>&quot;)
&quot;aba&quot; -&gt; index 1 (&quot;c<u>a</u>ca&quot;), index 2 (&quot;bb<u>b</u>b&quot;), index 3 (&quot;cac<u>a</u>&quot;)
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> words = [&quot;abba&quot;,&quot;baab&quot;], target = &quot;bab&quot;
<strong>Output:</strong> 4
<strong>Explanation:</strong> There are 4 ways to form target.
&quot;bab&quot; -&gt; index 0 (&quot;<u>b</u>aab&quot;), index 1 (&quot;b<u>a</u>ab&quot;), index 2 (&quot;ab<u>b</u>a&quot;)
&quot;bab&quot; -&gt; index 0 (&quot;<u>b</u>aab&quot;), index 1 (&quot;b<u>a</u>ab&quot;), index 3 (&quot;baa<u>b</u>&quot;)
&quot;bab&quot; -&gt; index 0 (&quot;<u>b</u>aab&quot;), index 2 (&quot;ba<u>a</u>b&quot;), index 3 (&quot;baa<u>b</u>&quot;)
&quot;bab&quot; -&gt; index 1 (&quot;a<u>b</u>ba&quot;), index 2 (&quot;ba<u>a</u>b&quot;), index 3 (&quot;baa<u>b</u>&quot;)
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 1000</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 1000</code></li>
	<li>Tất cả các chuỗi trong<code>words</code>có cùng độ dài.</li>
	<li><code>1 &lt;= target.length &lt;= 1000</code></li>
	<li><code>words[i]</code>Và<code>target</code>chỉ chứa các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Preprocessing + Memory Search

<!-- thinking:start -->

> **Suy nghĩ**
>
> Các từ có cùng độ dài, các chữ cái trong cùng một cột có thể hoán đổi cho nhau và các cột phải được đưa từ trái sang phải. Quay lại các từ lặp lại công việc.
>
> Đếm chữ cái$c$trong cột$j$BẰNG$\textit{cnt}[j][c]$. Trạng thái là “khớp$\textit{target}[i:]$bắt đầu từ cột$j$”.
>
> Đã ghi nhớ$dfs(i,j)$bỏ qua cột$j$hoặc sử dụng nó cho$\textit{target}[i]$nhân với số đếm. Hoàn thành$i$sản lượng$1$; hết cột mang lại kết quả$0$.

<!-- thinking:end -->

Chúng tôi nhận thấy rằng độ dài của mỗi chuỗi trong mảng chuỗi$words$giống nhau nên hãy nhớ$n$, thì chúng ta có thể xử lý trước một mảng hai chiều$cnt$, Ở đâu$cnt[j][c]$đại diện cho mảng chuỗi$words$Số lượng ký tự$c$trong$j$-vị trí thứ của.

Tiếp theo, chúng ta thiết kế một hàm$dfs(i, j)$, đại diện cho số lượng sơ đồ xây dựng$target[i,..]$và vị trí ký tự hiện được chọn từ$words$là$j$. Vậy thì câu trả lời là$dfs(0, 0)$.

Logic tính toán của hàm$dfs(i, j)$như sau:

- Nếu như$i \geq m$, điều đó có nghĩa là tất cả các ký tự trong$target$đã được chọn thì số phương án là$1$.
- Nếu như$j \geq n$, điều đó có nghĩa là tất cả các ký tự trong$words$đã được chọn thì số phương án là$0$.
- Ngược lại chúng ta có thể chọn không chọn ký tự trong$j$-vị trí thứ của$words$, thì số phương án là$dfs(i, j + 1)$; hoặc chúng ta chọn nhân vật trong$j$-vị trí thứ của$words$, thì số phương án là$dfs(i + 1, j + 1) \times cnt[j][target[i] - 'a']$.

Cuối cùng, chúng tôi trở lại$dfs(0, 0)$. Lưu ý rằng câu trả lời được thực hiện theo phép toán modulo.

Độ phức tạp về thời gian là$O(m \times n)$và độ phức tạp của không gian là$O(m \times n)$. Ở đâu$m$là độ dài của chuỗi$target$, Và$n$là độ dài của mỗi chuỗi trong mảng chuỗi$words$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numWays(self, words: List[str], target: str) -> int:
        @cache
        def dfs(i: int, j: int) -> int:
            if i >= m:
                return 1
            if j >= n:
                return 0
            ans = dfs(i + 1, j + 1) * cnt[j][ord(target[i]) - ord('a')]
            ans = (ans + dfs(i, j + 1)) % mod
            return ans

        m, n = len(target), len(words[0])
        cnt = [[0] * 26 for _ in range(n)]
        for w in words:
            for j, c in enumerate(w):
                cnt[j][ord(c) - ord('a')] += 1
        mod = 10**9 + 7
        return dfs(0, 0)
```

#### Java

```java
class Solution {
    private int m;
    private int n;
    private String target;
    private Integer[][] f;
    private int[][] cnt;
    private final int mod = (int) 1e9 + 7;

    public int numWays(String[] words, String target) {
        m = target.length();
        n = words[0].length();
        f = new Integer[m][n];
        this.target = target;
        cnt = new int[n][26];
        for (var w : words) {
            for (int j = 0; j < n; ++j) {
                cnt[j][w.charAt(j) - 'a']++;
            }
        }
        return dfs(0, 0);
    }

    private int dfs(int i, int j) {
        if (i >= m) {
            return 1;
        }
        if (j >= n) {
            return 0;
        }
        if (f[i][j] != null) {
            return f[i][j];
        }
        long ans = dfs(i, j + 1);
        ans += 1L * dfs(i + 1, j + 1) * cnt[j][target.charAt(i) - 'a'];
        ans %= mod;
        return f[i][j] = (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numWays(vector<string>& words, string target) {
        const int mod = 1e9 + 7;
        int m = target.size(), n = words[0].size();
        vector<vector<int>> cnt(n, vector<int>(26));
        for (auto& w : words) {
            for (int j = 0; j < n; ++j) {
                ++cnt[j][w[j] - 'a'];
            }
        }
        int f[m][n];
        memset(f, -1, sizeof(f));
        function<int(int, int)> dfs = [&](int i, int j) -> int {
            if (i >= m) {
                return 1;
            }
            if (j >= n) {
                return 0;
            }
            if (f[i][j] != -1) {
                return f[i][j];
            }
            int ans = dfs(i, j + 1);
            ans = (ans + 1LL * dfs(i + 1, j + 1) * cnt[j][target[i] - 'a']) % mod;
            return f[i][j] = ans;
        };
        return dfs(0, 0);
    }
};
```

#### Go

```go
func numWays(words []string, target string) int {
	m, n := len(target), len(words[0])
	f := make([][]int, m)
	cnt := make([][26]int, n)
	for _, w := range words {
		for j, c := range w {
			cnt[j][c-'a']++
		}
	}
	for i := range f {
		f[i] = make([]int, n)
		for j := range f[i] {
			f[i][j] = -1
		}
	}
	const mod = 1e9 + 7
	var dfs func(i, j int) int
	dfs = func(i, j int) int {
		if i >= m {
			return 1
		}
		if j >= n {
			return 0
		}
		if f[i][j] != -1 {
			return f[i][j]
		}
		ans := dfs(i, j+1)
		ans = (ans + dfs(i+1, j+1)*cnt[j][target[i]-'a']) % mod
		f[i][j] = ans
		return ans
	}
	return dfs(0, 0)
}
```

#### TypeScript

```ts
function numWays(words: string[], target: string): number {
    const m = target.length;
    const n = words[0].length;
    const f = new Array(m + 1).fill(0).map(() => new Array(n + 1).fill(0));
    const mod = 1e9 + 7;
    for (let j = 0; j <= n; ++j) {
        f[0][j] = 1;
    }
    const cnt = new Array(n).fill(0).map(() => new Array(26).fill(0));
    for (const w of words) {
        for (let j = 0; j < n; ++j) {
            ++cnt[j][w.charCodeAt(j) - 97];
        }
    }
    for (let i = 1; i <= m; ++i) {
        for (let j = 1; j <= n; ++j) {
            f[i][j] = f[i][j - 1] + f[i - 1][j - 1] * cnt[j - 1][target.charCodeAt(i - 1) - 97];
            f[i][j] %= mod;
        }
    }
    return f[m][n];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Preprocessing + Dynamic Programming

<!-- thinking:start -->

> **Suy nghĩ**
>
> Đệ quy trong Giải pháp 1 trở thành một bảng rõ ràng và loại bỏ ngăn xếp cuộc gọi.$f[i][j]$là cách để xây dựng cái đầu tiên$i$nhân vật của$\textit{target}$từ đầu tiên$j$cột.
>
> Bỏ qua cột$j$BẰNG$f[i][j-1]$, hoặc coi nó như$f[i-1][j-1]\times \textit{cnt}[j-1][\textit{target}[i-1]]$, với$f[0][\cdot]=1$.

<!-- thinking:end -->

Tương tự như Giải pháp 1, trước tiên chúng ta có thể xử lý trước mảng hai chiều$cnt$, Ở đâu$cnt[j][c]$đại diện cho số lượng ký tự$c$trong$j$-vị trí thứ của mảng chuỗi$words$.

Tiếp theo, chúng tôi xác định$f[i][j]$đại diện cho số cách để xây dựng cái đầu tiên$i$nhân vật của$target$và hiện đang chọn các ký tự từ đầu tiên$j$ký tự của mỗi từ trong$words$. Vậy thì câu trả lời là$f[m][n]$. Ban đầu$f[0][j] = 1$, Ở đâu$0 \leq j \leq n$.

Coi như$f[i][j]$, Ở đâu$i \gt 0$, $j \gt 0$. Chúng ta có thể chọn không chọn ký tự trong$j$-vị trí thứ của$words$, trong trường hợp đó số cách là$f[i][j - 1]$; hoặc chúng ta chọn nhân vật trong$j$-vị trí thứ của$words$, trong trường hợp đó số cách là$f[i - 1][j - 1] \times cnt[j - 1][target[i - 1] - 'a']$. Cuối cùng, chúng ta cộng số cách trong hai trường hợp này, đó là giá trị của$f[i][j]$.

Cuối cùng, chúng tôi trở lại$f[m][n]$. Lưu ý hoạt động mod của câu trả lời.

Độ phức tạp về thời gian là$O(m \times n)$và độ phức tạp của không gian là$O(m \times n)$. Ở đâu$m$là độ dài của chuỗi$target$, Và$n$là độ dài của mỗi chuỗi trong mảng chuỗi$words$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numWays(self, words: List[str], target: str) -> int:
        m, n = len(target), len(words[0])
        cnt = [[0] * 26 for _ in range(n)]
        for w in words:
            for j, c in enumerate(w):
                cnt[j][ord(c) - ord('a')] += 1
        mod = 10**9 + 7
        f = [[0] * (n + 1) for _ in range(m + 1)]
        f[0] = [1] * (n + 1)
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                f[i][j] = (
                    f[i][j - 1]
                    + f[i - 1][j - 1] * cnt[j - 1][ord(target[i - 1]) - ord('a')]
                )
                f[i][j] %= mod
        return f[m][n]
```

#### Java

```java
class Solution {
    public int numWays(String[] words, String target) {
        int m = target.length();
        int n = words[0].length();
        final int mod = (int) 1e9 + 7;
        long[][] f = new long[m + 1][n + 1];
        Arrays.fill(f[0], 1);
        int[][] cnt = new int[n][26];
        for (var w : words) {
            for (int j = 0; j < n; ++j) {
                cnt[j][w.charAt(j) - 'a']++;
            }
        }
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                f[i][j] = f[i][j - 1] + f[i - 1][j - 1] * cnt[j - 1][target.charAt(i - 1) - 'a'];
                f[i][j] %= mod;
            }
        }
        return (int) f[m][n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numWays(vector<string>& words, string target) {
        int m = target.size(), n = words[0].size();
        const int mod = 1e9 + 7;
        long long f[m + 1][n + 1];
        memset(f, 0, sizeof(f));
        fill(f[0], f[0] + n + 1, 1);
        vector<vector<int>> cnt(n, vector<int>(26));
        for (auto& w : words) {
            for (int j = 0; j < n; ++j) {
                ++cnt[j][w[j] - 'a'];
            }
        }
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                f[i][j] = f[i][j - 1] + f[i - 1][j - 1] * cnt[j - 1][target[i - 1] - 'a'];
                f[i][j] %= mod;
            }
        }
        return f[m][n];
    }
};
```

#### Go

```go
func numWays(words []string, target string) int {
	const mod = 1e9 + 7
	m, n := len(target), len(words[0])
	f := make([][]int, m+1)
	for i := range f {
		f[i] = make([]int, n+1)
	}
	for j := range f[0] {
		f[0][j] = 1
	}
	cnt := make([][26]int, n)
	for _, w := range words {
		for j, c := range w {
			cnt[j][c-'a']++
		}
	}
	for i := 1; i <= m; i++ {
		for j := 1; j <= n; j++ {
			f[i][j] = f[i][j-1] + f[i-1][j-1]*cnt[j-1][target[i-1]-'a']
			f[i][j] %= mod
		}
	}
	return f[m][n]
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
