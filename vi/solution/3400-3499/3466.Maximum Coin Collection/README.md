---
comments: true
difficulty: Medium
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3466. Maximum Coin Collection 🔒](https://leetcode.com/problems/maximum-coin-collection)

[中文文档](/solution/3400-3499/3466.Maximum%20Coin%20Collection/README.md)

## Mô tả

<!-- description:start -->

<p>Mario lái xe trên một đường cao tốc có hai làn, mỗi dặm có coin. Cho hai mảng số nguyên <code>lane1</code> và <code>lane2</code>, trong đó giá trị tại chỉ số <code>i<sup>th</sup></code> biểu thị số coin Mario <em>nhận được hoặc mất đi</em> ở dặm thứ <code>i<sup>th</sup></code> của làn đó.</p>

<ul>
	<li>Nếu Mario đang ở làn 1 tại dặm <code>i</code> và <code>lane1[i] &gt; 0</code>, Mario nhận được <code>lane1[i]</code> coin.</li>
	<li>Nếu Mario đang ở làn 1 tại dặm <code>i</code> và <code>lane1[i] &lt; 0</code>, Mario trả phí và mất <code>abs(lane1[i])</code> coin.</li>
	<li>Các quy tắc tương tự cũng áp dụng cho <code>lane2</code>.</li>
</ul>

<p>Mario có thể đi vào đường cao tốc ở bất kỳ vị trí nào và rời đi bất cứ lúc nào sau khi đã đi <strong>ít nhất</strong> một dặm. Mario luôn đi vào đường cao tốc ở làn 1 nhưng có thể đổi làn <strong>nhiều nhất</strong> 2 lần.</p>

<p><strong>Đổi làn</strong> là khi Mario chuyển từ làn 1 sang làn 2 hoặc ngược lại.</p>

<p>Trả về số coin <strong>lớn nhất</strong> Mario có thể kiếm được sau khi thực hiện <strong>nhiều nhất 2 lần đổi làn</strong>.</p>

<p><strong>Lưu ý:</strong> Mario có thể đổi làn ngay sau khi đi vào đường cao tốc hoặc ngay trước khi rời đi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">lane1 = [1,-2,-10,3], lane2 = [-5,10,0,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">14</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Mario đi dặm đầu tiên ở làn 1.</li>
	<li>Sau đó, Mario chuyển sang làn 2 và đi hai dặm.</li>
	<li>Mario chuyển lại sang làn 1 trong dặm cuối cùng.</li>
</ul>

<p>Mario thu được <code>1 + 10 + 0 + 3 = 14</code> coin.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">lane1 = [1,-1,-1,-1], lane2 = [0,3,4,-5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Mario bắt đầu ở dặm 0 tại làn 1 và đi một dặm.</li>
	<li>Sau đó, Mario chuyển sang làn 2 và đi thêm hai dặm. Mario rời đường cao tốc trước dặm 3.</li>
</ul>

<p>Mario thu được <code>1 + 3 + 4 = 8</code> coin.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">lane1 = [-5,-4,-3], lane2 = [-1,2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Mario đi vào tại dặm 1 và ngay lập tức chuyển sang làn 2. Mario ở làn này trong suốt quãng đường.</li>
</ul>

<p>Mario thu được tổng cộng <code>2 + 3 = 5</code> coin.</p>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">lane1 = [-3,-3,-3], lane2 = [9,-2,4]</span></p>

<p><strong>Đầu ra:</strong> 11</p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Mario bắt đầu ở đầu đường cao tốc và ngay lập tức chuyển sang làn 2. Mario ở làn này trong suốt quãng đường.</li>
</ul>

<p>Mario thu được tổng cộng <code>9 + (-2) + 4 = 11</code> coin.</p>
</div>

<p><strong class="example">Ví dụ 5:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">lane1 = [-10], lane2 = [-2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Vì Mario phải đi trên đường cao tốc ít nhất một dặm, Mario chỉ đi một dặm ở làn 2.</li>
</ul>

<p>Mario thu được tổng cộng -2 coin.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= lane1.length == lane2.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= lane1[i], lane2[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có memoization

<!-- thinking:start -->

> **Tư duy**
>
> Có hai làn, được đổi làn nhiều nhất hai lần, có thể bắt đầu ở bất kỳ đâu và rời đi bất cứ lúc nào. Với $n\le 10^5$, không thể liệt kê tất cả các đường đi.
>
> Một state lưu chỉ số, làn hiện tại và số lần đổi làn còn lại. Chỉ số bắt đầu được duyệt ở bên ngoài, nên “chưa đi vào” không cần là một state.
>
> $\textit{dfs}(i,j,k)$ có thể dừng tại đây, tiếp tục đi trên cùng làn, hoặc dùng một lần đổi làn (sau khi đi một bước hoặc ngay tại vị trí hiện tại). Đáp án là giá trị lớn nhất của $\textit{dfs}(i,0,2)$ trên mọi vị trí bắt đầu.

<!-- thinking:end -->

Ta xây dựng hàm $\textit{dfs}(i, j, k)$, biểu diễn số coin lớn nhất Mario có thể thu thập khi bắt đầu từ vị trí $i$, hiện đang ở làn $j$, với $k$ lần đổi làn còn lại. Đáp án là giá trị lớn nhất của $\textit{dfs}(i, 0, 2)$ với mọi $i$.

Hàm $\textit{dfs}(i, j, k)$ được tính như sau:

- Nếu $i \geq n$, nghĩa là Mario đã đi đến cuối, ta trả về 0;
- Nếu không đổi làn, Mario có thể đi 1 dặm rồi rời đi, hoặc tiếp tục lái xe, lấy giá trị lớn hơn trong hai trường hợp, tức là $\max(x, \textit{dfs}(i + 1, j, k) + x)$;
- Nếu có thể đổi làn, có hai lựa chọn: đi 1 dặm rồi đổi làn, hoặc đổi làn ngay lập tức, lấy giá trị lớn hơn trong hai trường hợp, tức là $\max(\textit{dfs}(i + 1, j \oplus 1, k - 1) + x, \textit{dfs}(i, j \oplus 1, k - 1))$.
- Trong đó, $x$ biểu diễn số coin tại vị trí hiện tại.

Để tránh tính toán lặp lại, ta dùng tìm kiếm có memoization để lưu kết quả đã được tính.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của các làn.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxCoins(self, lane1: List[int], lane2: List[int]) -> int:
        @cache
        def dfs(i: int, j: int, k: int) -> int:
            if i >= n:
                return 0
            x = lane1[i] if j == 0 else lane2[i]
            ans = max(x, dfs(i + 1, j, k) + x)
            if k > 0:
                ans = max(ans, dfs(i + 1, j ^ 1, k - 1) + x)
                ans = max(ans, dfs(i, j ^ 1, k - 1))
            return ans

        n = len(lane1)
        ans = -inf
        for i in range(n):
            ans = max(ans, dfs(i, 0, 2))
        return ans
```

#### Java

```java
class Solution {
    private int n;
    private int[] lane1;
    private int[] lane2;
    private Long[][][] f;

    public long maxCoins(int[] lane1, int[] lane2) {
        n = lane1.length;
        this.lane1 = lane1;
        this.lane2 = lane2;
        f = new Long[n][2][3];
        long ans = Long.MIN_VALUE;
        for (int i = 0; i < n; ++i) {
            ans = Math.max(ans, dfs(i, 0, 2));
        }
        return ans;
    }

    private long dfs(int i, int j, int k) {
        if (i >= n) {
            return 0;
        }
        if (f[i][j][k] != null) {
            return f[i][j][k];
        }
        int x = j == 0 ? lane1[i] : lane2[i];
        long ans = Math.max(x, dfs(i + 1, j, k) + x);
        if (k > 0) {
            ans = Math.max(ans, dfs(i + 1, j ^ 1, k - 1) + x);
            ans = Math.max(ans, dfs(i, j ^ 1, k - 1));
        }
        return f[i][j][k] = ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxCoins(vector<int>& lane1, vector<int>& lane2) {
        int n = lane1.size();
        long long ans = -1e18;
        vector<vector<vector<long long>>> f(n, vector<vector<long long>>(2, vector<long long>(3, -1e18)));
        auto dfs = [&](this auto&& dfs, int i, int j, int k) -> long long {
            if (i >= n) {
                return 0LL;
            }
            if (f[i][j][k] != -1e18) {
                return f[i][j][k];
            }
            int x = j == 0 ? lane1[i] : lane2[i];
            long long ans = max((long long) x, dfs(i + 1, j, k) + x);
            if (k > 0) {
                ans = max(ans, dfs(i + 1, j ^ 1, k - 1) + x);
                ans = max(ans, dfs(i, j ^ 1, k - 1));
            }
            return f[i][j][k] = ans;
        };
        for (int i = 0; i < n; ++i) {
            ans = max(ans, dfs(i, 0, 2));
        }
        return ans;
    }
};
```

#### Go

```go
func maxCoins(lane1 []int, lane2 []int) int64 {
	n := len(lane1)
	f := make([][2][3]int64, n)
	for i := range f {
		for j := range f[i] {
			for k := range f[i][j] {
				f[i][j][k] = -1
			}
		}
	}
	var dfs func(int, int, int) int64
	dfs = func(i, j, k int) int64 {
		if i >= n {
			return 0
		}
		if f[i][j][k] != -1 {
			return f[i][j][k]
		}
		x := int64(lane1[i])
		if j == 1 {
			x = int64(lane2[i])
		}
		ans := max(x, dfs(i+1, j, k)+x)
		if k > 0 {
			ans = max(ans, dfs(i+1, j^1, k-1)+x)
			ans = max(ans, dfs(i, j^1, k-1))
		}
		f[i][j][k] = ans
		return ans
	}
	ans := int64(-1e18)
	for i := range lane1 {
		ans = max(ans, dfs(i, 0, 2))
	}
	return ans
}
```

#### TypeScript

```ts
function maxCoins(lane1: number[], lane2: number[]): number {
    const n = lane1.length;
    const NEG_INF = -1e18;
    const f: number[][][] = Array.from({ length: n }, () =>
        Array.from({ length: 2 }, () => Array(3).fill(NEG_INF)),
    );
    const dfs = (dfs: Function, i: number, j: number, k: number): number => {
        if (i >= n) {
            return 0;
        }
        if (f[i][j][k] !== NEG_INF) {
            return f[i][j][k];
        }
        const x = j === 0 ? lane1[i] : lane2[i];
        let ans = Math.max(x, dfs(dfs, i + 1, j, k) + x);
        if (k > 0) {
            ans = Math.max(ans, dfs(dfs, i + 1, j ^ 1, k - 1) + x);
            ans = Math.max(ans, dfs(dfs, i, j ^ 1, k - 1));
        }
        f[i][j][k] = ans;
        return ans;
    };
    let ans = NEG_INF;
    for (let i = 0; i < n; ++i) {
        ans = Math.max(ans, dfs(dfs, i, 0, 2));
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
