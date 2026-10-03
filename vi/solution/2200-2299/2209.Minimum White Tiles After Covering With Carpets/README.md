---
comments: true
difficulty: Hard
rating: 2105
source: Biweekly Contest 74 Q4
tags:
    - String
    - Dynamic Programming
    - Prefix Sum
---

<!-- problem:start -->

# [2209. Minimum White Tiles After Covering With Carpets](https://leetcode.com/problems/minimum-white-tiles-after-covering-with-carpets)

[中文文档](/solution/2200-2299/2209.Minimum%20White%20Tiles%20After%20Covering%20With%20Carpets/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi nhị phân <strong>đánh chỉ số từ 0</strong> <code>floor</code>, biểu diễn màu của các ô trên sàn:</p>

<ul>
	<li><code>floor[i] = &#39;0&#39;</code> nghĩa là ô thứ <code>i<sup>th</sup></code> của sàn có màu <strong>đen</strong>.</li>
	<li>Ngược lại, <code>floor[i] = &#39;1&#39;</code> nghĩa là ô thứ <code>i<sup>th</sup></code> của sàn có màu <strong>trắng</strong>.</li>
</ul>

<p>Bạn cũng được cho <code>numCarpets</code> và <code>carpetLen</code>. Bạn có <code>numCarpets</code> tấm thảm <strong>đen</strong>, mỗi tấm dài <code>carpetLen</code> ô. Hãy phủ các ô bằng những tấm thảm đã cho sao cho số ô <strong>trắng</strong> vẫn còn nhìn thấy là <strong>nhỏ nhất</strong>. Các tấm thảm có thể chồng lên nhau.</p>

<p>Trả về <em>số ô trắng <strong>nhỏ nhất</strong> vẫn còn nhìn thấy</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2209.Minimum%20White%20Tiles%20After%20Covering%20With%20Carpets/images/ex1-1.png" style="width: 400px; height: 73px;" />
<pre>
<strong>Đầu vào:</strong> floor = &quot;10110101&quot;, numCarpets = 2, carpetLen = 2
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Hình trên minh họa một cách phủ các ô bằng những tấm thảm sao cho chỉ còn 2 ô trắng nhìn thấy.
Không có cách phủ nào khác có thể khiến số ô trắng nhìn thấy còn ít hơn 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2209.Minimum%20White%20Tiles%20After%20Covering%20With%20Carpets/images/ex2.png" style="width: 353px; height: 123px;" />
<pre>
<strong>Đầu vào:</strong> floor = &quot;11111&quot;, numCarpets = 2, carpetLen = 3
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
Hình trên minh họa một cách phủ các ô bằng những tấm thảm sao cho không còn ô trắng nào nhìn thấy.
Lưu ý rằng các tấm thảm có thể chồng lên nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= carpetLen &lt;= floor.length &lt;= 1000</code></li>
	<li><code>floor[i]</code> chỉ có thể là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code>.</li>
	<li><code>1 &lt;= numCarpets &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Ta có $m$ tấm thảm, mỗi tấm dài $L$, và muốn số ô trắng không được phủ là ít nhất. Việc liệt kê các vị trí đặt thảm có độ phức tạp cấp số mũ; với $n, m \le 10^3$ thì cách này không phù hợp. Ta có thể đưa ra quyết định từ trái sang phải.
>
> Gọi $\textit{dfs}(i, j)$ là số ô trắng không được phủ ít nhất tính từ chỉ số $i$ khi còn $j$ tấm thảm. Ta bỏ qua ô màu đen. Khi không còn tấm thảm nào, phần còn lại là hiệu tổng tiền tố $s[n]-s[i]$. Với một ô trắng, ta có thể để nguyên ô đó ($1 + \textit{dfs}(i+1, j)$) hoặc phủ nó ($\textit{dfs}(i+L, j-1)$).
>
> Có $O(nm)$ trạng thái và mỗi trạng thái chỉ cần thời gian hằng số; dùng ghi nhớ sẽ cho kết quả $\textit{dfs}(0, m)$.

<!-- thinking:end -->

Ta xây dựng hàm $\textit{dfs}(i, j)$ biểu diễn số ô trắng nhỏ nhất không được phủ, bắt đầu từ chỉ số $i$ và sử dụng $j$ tấm thảm. Đáp án là $\textit{dfs}(0, \textit{numCarpets})$.

Với chỉ số $i$, ta xét các trường hợp sau:

- Nếu $i \ge n$, nghĩa là tất cả các ô đã được xét, trả về $0$;
- Nếu $\textit{floor}[i] = 0$, ta không cần dùng thảm mà chỉ cần bỏ qua ô này, tức là $\textit{dfs}(i, j) = \textit{dfs}(i + 1, j)$;
- Nếu $j = 0$, ta có thể dùng mảng tổng tiền tố $s$ để tính trực tiếp số ô trắng còn lại chưa được phủ, tức là $\textit{dfs}(i, j) = s[n] - s[i]$;
- Nếu $\textit{floor}[i] = 1$, ta có thể chọn dùng hoặc không dùng một tấm thảm, rồi lấy giá trị nhỏ hơn trong hai trường hợp, tức là $\textit{dfs}(i, j) = \min(\textit{dfs}(i + 1, j), \textit{dfs}(i + \textit{carpetLen}, j - 1))$.

Ta sử dụng tìm kiếm có ghi nhớ để giải bài toán này.

Độ phức tạp thời gian là $O(n \times m)$, độ phức tạp không gian là $O(n \times m)$. Trong đó, $n$ và $m$ lần lượt là độ dài chuỗi $\textit{floor}$ và giá trị của $\textit{numCarpets}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumWhiteTiles(self, floor: str, numCarpets: int, carpetLen: int) -> int:
        @cache
        def dfs(i: int, j: int) -> int:
            if i >= n:
                return 0
            if floor[i] == "0":
                return dfs(i + 1, j)
            if j == 0:
                return s[-1] - s[i]
            return min(1 + dfs(i + 1, j), dfs(i + carpetLen, j - 1))

        n = len(floor)
        s = [0] * (n + 1)
        for i, c in enumerate(floor):
            s[i + 1] = s[i] + int(c == "1")
        ans = dfs(0, numCarpets)
        dfs.cache_clear()
        return ans
```

#### Java

```java
class Solution {
    private Integer[][] f;
    private int[] s;
    private int n;
    private int k;

    public int minimumWhiteTiles(String floor, int numCarpets, int carpetLen) {
        n = floor.length();
        f = new Integer[n][numCarpets + 1];
        s = new int[n + 1];
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + (floor.charAt(i) == '1' ? 1 : 0);
        }
        k = carpetLen;
        return dfs(0, numCarpets);
    }

    private int dfs(int i, int j) {
        if (i >= n) {
            return 0;
        }
        if (j == 0) {
            return s[n] - s[i];
        }
        if (f[i][j] != null) {
            return f[i][j];
        }
        if (s[i + 1] == s[i]) {
            return dfs(i + 1, j);
        }
        int ans = Math.min(1 + dfs(i + 1, j), dfs(i + k, j - 1));
        f[i][j] = ans;
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumWhiteTiles(string floor, int numCarpets, int carpetLen) {
        int n = floor.size();
        vector<vector<int>> f(n, vector<int>(numCarpets + 1, -1));
        vector<int> s(n + 1);
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + (floor[i] == '1');
        }
        auto dfs = [&](this auto&& dfs, int i, int j) -> int {
            if (i >= n) {
                return 0;
            }
            if (j == 0) {
                return s[n] - s[i];
            }
            if (f[i][j] != -1) {
                return f[i][j];
            }
            if (s[i + 1] == s[i]) {
                return dfs(i + 1, j);
            }
            int ans = min(1 + dfs(i + 1, j), dfs(i + carpetLen, j - 1));
            f[i][j] = ans;
            return ans;
        };
        return dfs(0, numCarpets);
    }
};
```

#### Go

```go
func minimumWhiteTiles(floor string, numCarpets int, carpetLen int) int {
	n := len(floor)
	f := make([][]int, n)
	for i := range f {
		f[i] = make([]int, numCarpets+1)
		for j := range f[i] {
			f[i][j] = -1
		}
	}
	s := make([]int, n+1)
	for i, c := range floor {
		s[i+1] = s[i] + int(c-'0')
	}
	var dfs func(i, j int) int
	dfs = func(i, j int) int {
		if i >= n {
			return 0
		}
		if j == 0 {
			return s[n] - s[i]
		}
		if f[i][j] != -1 {
			return f[i][j]
		}
		if s[i+1] == s[i] {
			return dfs(i+1, j)
		}
		ans := min(1+dfs(i+1, j), dfs(i+carpetLen, j-1))
		f[i][j] = ans
		return ans
	}
	return dfs(0, numCarpets)
}
```

#### TypeScript

```ts
function minimumWhiteTiles(floor: string, numCarpets: number, carpetLen: number): number {
    const n = floor.length;
    const f: number[][] = Array.from({ length: n }, () => Array(numCarpets + 1).fill(-1));
    const s: number[] = Array(n + 1).fill(0);
    for (let i = 0; i < n; ++i) {
        s[i + 1] = s[i] + (floor[i] === '1' ? 1 : 0);
    }
    const dfs = (i: number, j: number): number => {
        if (i >= n) {
            return 0;
        }
        if (j === 0) {
            return s[n] - s[i];
        }
        if (f[i][j] !== -1) {
            return f[i][j];
        }
        if (s[i + 1] === s[i]) {
            return dfs(i + 1, j);
        }
        const ans = Math.min(1 + dfs(i + 1, j), dfs(i + carpetLen, j - 1));
        f[i][j] = ans;
        return ans;
    };
    return dfs(0, numCarpets);
}
```

#### Rust

```rust
impl Solution {
    pub fn minimum_white_tiles(floor: String, num_carpets: i32, carpet_len: i32) -> i32 {
        let n = floor.len();
        let a: Vec<u8> = floor.bytes().collect();
        let m = num_carpets as usize;
        let k = carpet_len as usize;

        let mut s = vec![0i32; n + 1];
        for i in 0..n {
            s[i + 1] = s[i] + if a[i] == b'1' { 1 } else { 0 };
        }

        let mut f = vec![vec![-1; m + 1]; n];

        fn dfs(
            i: usize,
            j: usize,
            n: usize,
            k: usize,
            s: &Vec<i32>,
            f: &mut Vec<Vec<i32>>,
            a: &Vec<u8>,
        ) -> i32 {
            if i >= n {
                return 0;
            }
            if j == 0 {
                return s[n] - s[i];
            }
            if f[i][j] != -1 {
                return f[i][j];
            }

            if s[i + 1] == s[i] {
                let v = dfs(i + 1, j, n, k, s, f, a);
                f[i][j] = v;
                return v;
            }

            let t1 = 1 + dfs(i + 1, j, n, k, s, f, a);
            let ni = i + k;
            let t2 = dfs(ni, j - 1, n, k, s, f, a);

            let t = t1.min(t2);
            f[i][j] = t;
            t
        }

        dfs(0, m, n, k, &s, &mut f, &a)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
