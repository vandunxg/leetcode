---
comments: true
difficulty: Medium
tags:
    - Minimax
    - Array
    - Math
    - Dynamic Programming
    - Game Theory
    - Zero-Sum Game
---

<!-- problem:start -->

# [877. Stone Game](https://leetcode.com/problems/stone-game)

[中文文档](/solution/0800-0899/0877.Stone%20Game/README.md)

## Mô tả

<!-- description:start -->

<p>Alice và Bob chơi trò chơi với các đống đá. Có một số lượng đống <strong>chẵn</strong> được xếp thành một hàng, mỗi đống có số viên đá nguyên <strong>dương</strong> là <code>piles[i]</code>.</p>

<p>Mục tiêu là kết thúc trò chơi với nhiều đá nhất. <strong>Tổng</strong> số viên đá trong tất cả các đống là số <strong>lẻ</strong>, nên không thể hòa.</p>

<p>Alice và Bob thay phiên nhau, <strong>Alice đi trước</strong>. Mỗi lượt, người chơi lấy toàn bộ đống đá ở <strong>đầu</strong> hoặc <strong>cuối</strong> hàng. Trò chơi tiếp tục cho đến khi không còn đống nào; khi đó người có <strong>nhiều đá hơn sẽ thắng</strong>.</p>

<p>Giả sử Alice và Bob đều chơi tối ưu, hãy trả về <code>true</code><em> nếu Alice thắng, hoặc </em><code>false</code><em> nếu Bob thắng</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> piles = [5,3,4,5]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> 
Alice đi trước và chỉ có thể lấy đống 5 đầu tiên hoặc đống 5 cuối cùng.
Giả sử cô ấy lấy đống 5 đầu tiên, hàng còn lại là [3, 4, 5].
Nếu Bob lấy đống 3, hàng còn [4, 5], Alice lấy đống 5 và thắng với 10 điểm.
Nếu Bob lấy đống 5 cuối cùng, hàng còn [3, 4], Alice lấy đống 4 và thắng với 9 điểm.
Điều này cho thấy lấy đống 5 đầu tiên là nước đi giúp Alice thắng, nên ta trả về true.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> piles = [3,7,2,3]
<strong>Đầu ra:</strong> true
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= piles.length &lt;= 500</code></li>
	<li><code>piles.length</code> là số <strong>chẵn</strong>.</li>
	<li><code>1 &lt;= piles[i] &lt;= 500</code></li>
	<li><code>sum(piles[i])</code> là số <strong>lẻ</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có memoization

<!-- thinking:start -->

> **Tư duy**
>
> Người chơi lấy đá ở một trong hai đầu của dãy đống đá trong trò chơi zero-sum; ta cần xác định Alice có đạt điểm cao hơn không. Với tối đa $500$ đống, trạng thái là chênh lệch điểm trên đoạn $[i,j]$.
>
> $dfs(i,j)$ là giá trị lớn hơn giữa $piles[i]-dfs(i+1,j)$ và $piles[j]-dfs(i,j-1)$. Chênh lệch điểm dương đồng nghĩa với chiến thắng. Alice luôn thắng với các ràng buộc đã cho, nhưng code vẫn tính DP trên các đoạn.

<!-- thinking:end -->

Ta định nghĩa hàm $dfs(i, j)$ là chênh lệch số đá lớn nhất giữa người chơi hiện tại và đối thủ khi xét các đống từ đống thứ $i$ đến đống thứ $j$. Khi đó, đáp án là $dfs(0, n - 1) \gt 0$.

Hàm $dfs(i, j)$ được tính như sau:

- Nếu $i \gt j$, không còn đá, nên người chơi hiện tại không thể lấy thêm và chênh lệch bằng $0$, tức là $dfs(i, j) = 0$.
- Nếu không, người chơi hiện tại có hai lựa chọn: nếu lấy đống thứ $i$, chênh lệch giữa người chơi hiện tại và đối thủ là $piles[i] - dfs(i + 1, j)$; nếu lấy đống thứ $j$, chênh lệch là $piles[j] - dfs(i, j - 1)$. Người chơi hiện tại chọn phương án có chênh lệch lớn hơn, do đó $dfs(i, j) = \max(piles[i] - dfs(i + 1, j), piles[j] - dfs(i, j - 1))$.

Cuối cùng, ta chỉ cần kiểm tra $dfs(0, n - 1) \gt 0$.

Để tránh tính toán lặp, ta dùng memoization: lưu các giá trị $dfs(i, j)$ vào mảng $f$ để khi hàm được gọi lại, có thể lấy kết quả trực tiếp từ $f$ thay vì tính lại.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là số đống đá.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def stoneGame(self, piles: List[int]) -> bool:
        @cache
        def dfs(i: int, j: int) -> int:
            if i > j:
                return 0
            return max(piles[i] - dfs(i + 1, j), piles[j] - dfs(i, j - 1))

        return dfs(0, len(piles) - 1) > 0
```

#### Java

```java
class Solution {
    private int[] piles;
    private int[][] f;

    public boolean stoneGame(int[] piles) {
        this.piles = piles;
        int n = piles.length;
        f = new int[n][n];
        return dfs(0, n - 1) > 0;
    }

    private int dfs(int i, int j) {
        if (i > j) {
            return 0;
        }
        if (f[i][j] != 0) {
            return f[i][j];
        }
        return f[i][j] = Math.max(piles[i] - dfs(i + 1, j), piles[j] - dfs(i, j - 1));
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool stoneGame(vector<int>& piles) {
        int n = piles.size();
        int f[n][n];
        memset(f, 0, sizeof(f));
        auto dfs = [&](this auto&& dfs, int i, int j) -> int {
            if (i > j) {
                return 0;
            }
            if (f[i][j]) {
                return f[i][j];
            }
            return f[i][j] = max(piles[i] - dfs(i + 1, j), piles[j] - dfs(i, j - 1));
        };
        return dfs(0, n - 1) > 0;
    }
};
```

#### Go

```go
func stoneGame(piles []int) bool {
	n := len(piles)
	f := make([][]int, n)
	for i := range f {
		f[i] = make([]int, n)
	}
	var dfs func(i, j int) int
	dfs = func(i, j int) int {
		if i > j {
			return 0
		}
		if f[i][j] == 0 {
			f[i][j] = max(piles[i]-dfs(i+1, j), piles[j]-dfs(i, j-1))
		}
		return f[i][j]
	}
	return dfs(0, n-1) > 0
}
```

#### TypeScript

```ts
function stoneGame(piles: number[]): boolean {
    const n = piles.length;
    const f: number[][] = new Array(n).fill(0).map(() => new Array(n).fill(0));
    const dfs = (i: number, j: number): number => {
        if (i > j) {
            return 0;
        }
        if (f[i][j] === 0) {
            f[i][j] = Math.max(piles[i] - dfs(i + 1, j), piles[j] - dfs(i, j - 1));
        }
        return f[i][j];
    };
    return dfs(0, n - 1) > 0;
}
```

#### Rust

```rust
impl Solution {
    pub fn stone_game(piles: Vec<i32>) -> bool {
        let n = piles.len();
        let mut f = vec![vec![0; n]; n];

        fn dfs(i: usize, j: usize, piles: &Vec<i32>, f: &mut Vec<Vec<i32>>) -> i32 {
            if i == j {
                return piles[i];
            }
            if f[i][j] != 0 {
                return f[i][j];
            }

            let res = (piles[i] - dfs(i + 1, j, piles, f)).max(piles[j] - dfs(i, j - 1, piles, f));

            f[i][j] = res;
            res
        }

        dfs(0, n - 1, &piles, &mut f) > 0
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Có thể chuyển công thức truy hồi có memoization thành cách tính bảng mà không cần đệ quy. $f[i][j]$ là chênh lệch điểm trên $[i,j]$, được điền từ đường chéo có độ dài $1$ ra ngoài.
>
> Công thức chuyển trạng thái giống Lời giải 1. Đáp án dựa vào việc $f[0][n-1]$ có dương hay không.

<!-- thinking:end -->

Ta cũng có thể dùng quy hoạch động. Định nghĩa $f[i][j]$ là chênh lệch số đá tối đa mà người chơi hiện tại có thể lấy được so với đối thủ từ các đống $piles[i..j]$. Đáp án cuối cùng là $f[0][n - 1] \gt 0$.

Ban đầu, $f[i][i] = piles[i]$, vì khi chỉ còn một đống, người chơi hiện tại chỉ có thể lấy đống đó và chênh lệch là $piles[i]$.

Với $f[i][j]$ khi $i \lt j$, có hai trường hợp:

- Nếu người chơi hiện tại lấy đống $piles[i]$, các đống còn lại là $piles[i + 1..j]$ và đến lượt đối thủ, nên $f[i][j] = piles[i] - f[i + 1][j]$.
- Nếu người chơi hiện tại lấy đống $piles[j]$, các đống còn lại là $piles[i..j - 1]$ và đến lượt đối thủ, nên $f[i][j] = piles[j] - f[i][j - 1]$.

Vì vậy, phương trình chuyển trạng thái cuối cùng là $f[i][j] = \max(piles[i] - f[i + 1][j], piles[j] - f[i][j - 1])$.

Cuối cùng, ta chỉ cần kiểm tra $f[0][n - 1] \gt 0$.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là số đống đá.

Các bài toán tương tự:

- [486. Predict the Winner](https://github.com/doocs/leetcode/blob/main/solution/0400-0499/0486.Predict%20the%20Winner/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def stoneGame(self, piles: List[int]) -> bool:
        n = len(piles)
        f = [[0] * n for _ in range(n)]
        for i, x in enumerate(piles):
            f[i][i] = x
        for i in range(n - 2, -1, -1):
            for j in range(i + 1, n):
                f[i][j] = max(piles[i] - f[i + 1][j], piles[j] - f[i][j - 1])
        return f[0][n - 1] > 0
```

#### Java

```java
class Solution {
    public boolean stoneGame(int[] piles) {
        int n = piles.length;
        int[][] f = new int[n][n];
        for (int i = 0; i < n; ++i) {
            f[i][i] = piles[i];
        }
        for (int i = n - 2; i >= 0; --i) {
            for (int j = i + 1; j < n; ++j) {
                f[i][j] = Math.max(piles[i] - f[i + 1][j], piles[j] - f[i][j - 1]);
            }
        }
        return f[0][n - 1] > 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool stoneGame(vector<int>& piles) {
        int n = piles.size();
        int f[n][n];
        memset(f, 0, sizeof(f));
        for (int i = 0; i < n; ++i) {
            f[i][i] = piles[i];
        }
        for (int i = n - 2; ~i; --i) {
            for (int j = i + 1; j < n; ++j) {
                f[i][j] = max(piles[i] - f[i + 1][j], piles[j] - f[i][j - 1]);
            }
        }
        return f[0][n - 1] > 0;
    }
};
```

#### Go

```go
func stoneGame(piles []int) bool {
	n := len(piles)
	f := make([][]int, n)
	for i, x := range piles {
		f[i] = make([]int, n)
		f[i][i] = x
	}
	for i := n - 2; i >= 0; i-- {
		for j := i + 1; j < n; j++ {
			f[i][j] = max(piles[i]-f[i+1][j], piles[j]-f[i][j-1])
		}
	}
	return f[0][n-1] > 0
}
```

#### TypeScript

```ts
function stoneGame(piles: number[]): boolean {
    const n = piles.length;
    const f: number[][] = new Array(n).fill(0).map(() => new Array(n).fill(0));
    for (let i = 0; i < n; ++i) {
        f[i][i] = piles[i];
    }
    for (let i = n - 2; i >= 0; --i) {
        for (let j = i + 1; j < n; ++j) {
            f[i][j] = Math.max(piles[i] - f[i + 1][j], piles[j] - f[i][j - 1]);
        }
    }
    return f[0][n - 1] > 0;
}
```

#### Rust

```rust
impl Solution {
    pub fn stone_game(piles: Vec<i32>) -> bool {
        let n = piles.len();
        let mut f = vec![vec![0; n]; n];

        for i in 0..n {
            f[i][i] = piles[i];
        }

        for i in (0..n - 1).rev() {
            for j in i + 1..n {
                f[i][j] = (piles[i] - f[i + 1][j]).max(piles[j] - f[i][j - 1]);
            }
        }

        f[0][n - 1] > 0
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
