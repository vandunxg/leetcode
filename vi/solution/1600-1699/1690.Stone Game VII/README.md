---
comments: true
difficulty: Medium
rating: 1951
source: Weekly Contest 219 Q3
tags:
    - Minimax
    - Array
    - Math
    - Dynamic Programming
    - Game Theory
    - Zero-Sum Game
---

<!-- problem:start -->

# [1690. Stone Game VII](https://leetcode.com/problems/stone-game-vii)

[中文文档](/solution/1600-1699/1690.Stone%20Game%20VII/README.md)

## Mô tả

<!-- description:start -->

<p>Alice và Bob lần lượt chơi một trò chơi, trong đó <strong>Alice đi trước</strong>.</p>

<p>Có <code>n</code> viên đá được xếp thành một hàng. Trong lượt của mình, mỗi người chơi có thể <strong>loại bỏ</strong> viên đá ngoài cùng bên trái hoặc bên phải, rồi nhận số điểm bằng <strong>tổng</strong> giá trị của các viên đá còn lại trong hàng. Khi không còn viên đá nào, người có điểm cao hơn sẽ thắng.</p>

<p>Bob nhận ra rằng mình luôn thua trò chơi này (tội nghiệp Bob, lúc nào cũng thua), nên anh ấy quyết định <strong>tối thiểu hóa hiệu điểm</strong>. Mục tiêu của Alice là <strong>tối đa hóa hiệu điểm</strong>.</p>

<p>Cho một mảng số nguyên <code>stones</code>, trong đó <code>stones[i]</code> biểu diễn giá trị của viên đá thứ <code>i<sup>th</sup></code> <strong>tính từ trái sang</strong>, hãy trả về <em><strong>hiệu điểm</strong> của Alice và Bob nếu cả hai đều chơi <strong>tối ưu</strong>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> stones = [5,3,1,4,2]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong>
- Alice removes 2 and gets 5 + 3 + 1 + 4 = 13 points. Alice = 13, Bob = 0, stones = [5,3,1,4].
- Bob removes 5 and gets 3 + 1 + 4 = 8 points. Alice = 13, Bob = 8, stones = [3,1,4].
- Alice removes 3 and gets 1 + 4 = 5 points. Alice = 18, Bob = 8, stones = [1,4].
- Bob removes 1 and gets 4 points. Alice = 18, Bob = 12, stones = [4].
- Alice removes 4 and gets 0 points. Alice = 18, Bob = 12, stones = [].
Hiệu điểm là 18 - 12 = 6.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> stones = [7,90,5,1,100,10,10,2]
<strong>Đầu ra:</strong> 122</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == stones.length</code></li>
	<li><code>2 &lt;= n &lt;= 1000</code></li>
	<li><code>1 &lt;= stones[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lượt loại bỏ một đầu và ghi điểm bằng tổng các giá trị còn lại. Hiệu điểm giữa người chơi thứ nhất và thứ hai trên một đoạn là một bài toán trò chơi có ghi nhớ: $dfs(i,j)$ là lợi thế của người chơi hiện tại.
>
> Mảng tổng tiền tố $s$ cho biết số điểm sau khi bỏ đầu trái hoặc đầu phải; ta chọn giá trị lớn hơn trong $s[j+1]-s[i+1]-dfs(i+1,j)$ và $s[j]-s[i]-dfs(i,j-1)$.

<!-- thinking:end -->

Trước hết, ta tiền xử lý để có mảng tổng tiền tố $s$, trong đó $s[i]$ biểu diễn tổng của $i$ viên đá đầu tiên.

Tiếp theo, ta định nghĩa hàm $dfs(i, j)$ biểu diễn hiệu điểm giữa người chơi thứ nhất và thứ hai khi các viên đá còn lại là $stones[i], stones[i + 1], \dots, stones[j]$. Đáp án là $dfs(0, n - 1)$.

Quá trình tính hàm $dfs(i, j)$ như sau:

- Nếu $i > j$, nghĩa là hiện tại không còn viên đá nào, nên trả về $0$;
- Ngược lại, người chơi thứ nhất có hai lựa chọn: loại bỏ $stones[i]$ hoặc $stones[j]$, sau đó tính hiệu điểm, lần lượt là $a = s[j + 1] - s[i + 1] - dfs(i + 1, j)$ và $b = s[j] - s[i] - dfs(i, j - 1)$. Ta lấy giá trị lớn hơn làm giá trị trả về của $dfs(i, j)$.

Trong quá trình này, ta sử dụng tìm kiếm có ghi nhớ, tức là dùng một mảng $f$ để lưu giá trị trả về của hàm $dfs(i, j)$ nhằm tránh tính toán lặp lại.

Độ phức tạp thời gian là $O(n^2)$, và độ phức tạp không gian là $O(n^2)$. Ở đây, $n$ là số lượng viên đá.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def stoneGameVII(self, stones: List[int]) -> int:
        @cache
        def dfs(i: int, j: int) -> int:
            if i > j:
                return 0
            a = s[j + 1] - s[i + 1] - dfs(i + 1, j)
            b = s[j] - s[i] - dfs(i, j - 1)
            return max(a, b)

        s = list(accumulate(stones, initial=0))
        ans = dfs(0, len(stones) - 1)
        dfs.cache_clear()
        return ans
```

#### Java

```java
class Solution {
    private int[] s;
    private Integer[][] f;

    public int stoneGameVII(int[] stones) {
        int n = stones.length;
        s = new int[n + 1];
        f = new Integer[n][n];
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + stones[i];
        }
        return dfs(0, n - 1);
    }

    private int dfs(int i, int j) {
        if (i > j) {
            return 0;
        }
        if (f[i][j] != null) {
            return f[i][j];
        }
        int a = s[j + 1] - s[i + 1] - dfs(i + 1, j);
        int b = s[j] - s[i] - dfs(i, j - 1);
        return f[i][j] = Math.max(a, b);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int stoneGameVII(vector<int>& stones) {
        int n = stones.size();
        vector<vector<int>> f(n, vector<int>(n));
        vector<int> s(n + 1);
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + stones[i];
        }
        auto dfs = [&](this auto&& dfs, int i, int j) -> int {
            if (i > j) {
                return 0;
            }
            if (f[i][j]) {
                return f[i][j];
            }
            int a = s[j + 1] - s[i + 1] - dfs(i + 1, j);
            int b = s[j] - s[i] - dfs(i, j - 1);
            return f[i][j] = max(a, b);
        };
        return dfs(0, n - 1);
    }
};
```

#### Go

```go
func stoneGameVII(stones []int) int {
	n := len(stones)
	s := make([]int, n+1)
	f := make([][]int, n)
	for i, x := range stones {
		s[i+1] = s[i] + x
		f[i] = make([]int, n)
	}
	var dfs func(int, int) int
	dfs = func(i, j int) int {
		if i > j {
			return 0
		}
		if f[i][j] != 0 {
			return f[i][j]
		}
		a := s[j+1] - s[i+1] - dfs(i+1, j)
		b := s[j] - s[i] - dfs(i, j-1)
		f[i][j] = max(a, b)
		return f[i][j]
	}
	return dfs(0, n-1)
}
```

#### TypeScript

```ts
function stoneGameVII(stones: number[]): number {
    const n = stones.length;
    const s: number[] = Array(n + 1).fill(0);
    for (let i = 0; i < n; ++i) {
        s[i + 1] = s[i] + stones[i];
    }
    const f: number[][] = Array.from({ length: n }, () => Array(n).fill(0));
    const dfs = (i: number, j: number): number => {
        if (i > j) {
            return 0;
        }
        if (f[i][j]) {
            return f[i][j];
        }
        const a = s[j + 1] - s[i + 1] - dfs(i + 1, j);
        const b = s[j] - s[i] - dfs(i, j - 1);
        return (f[i][j] = Math.max(a, b));
    };
    return dfs(0, n - 1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Đệ quy trở thành quy hoạch động trên đoạn. $f[i][j]$ giữ nguyên ý nghĩa và cần các đoạn ngắn hơn được tính trước: duyệt $i$ giảm dần và $j$ tăng dần. Đáp án là $f[0][n-1]$.

<!-- thinking:end -->

Ta có thể chuyển tìm kiếm có ghi nhớ trong Lời giải 1 thành quy hoạch động. Ta định nghĩa $f[i][j]$ là hiệu điểm giữa người chơi thứ nhất và thứ hai khi các viên đá còn lại là $stones[i], stones[i + 1], \dots, stones[j]$. Do đó, đáp án là $f[0][n - 1]$.

Công thức chuyển trạng thái như sau:

$$
f[i][j] = \max(s[j + 1] - s[i + 1] - f[i + 1][j], s[j] - s[i] - f[i][j - 1])
$$

Khi tính $f[i][j]$, ta cần bảo đảm $f[i + 1][j]$ và $f[i][j - 1]$ đã được tính. Do đó, ta cần duyệt $i$ theo thứ tự giảm dần và $j$ theo thứ tự tăng dần.

Cuối cùng, đáp án là $f[0][n - 1]$.

Độ phức tạp thời gian là $O(n^2)$, và độ phức tạp không gian là $O(n^2)$. Ở đây, $n$ là số lượng viên đá.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def stoneGameVII(self, stones: List[int]) -> int:
        s = list(accumulate(stones, initial=0))
        n = len(stones)
        f = [[0] * n for _ in range(n)]
        for i in range(n - 2, -1, -1):
            for j in range(i + 1, n):
                a = s[j + 1] - s[i + 1] - f[i + 1][j]
                b = s[j] - s[i] - f[i][j - 1]
                f[i][j] = max(a, b)
        return f[0][-1]
```

#### Java

```java
class Solution {
    public int stoneGameVII(int[] stones) {
        int n = stones.length;
        int[] s = new int[n + 1];
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + stones[i];
        }
        int[][] f = new int[n][n];
        for (int i = n - 2; i >= 0; --i) {
            for (int j = i + 1; j < n; ++j) {
                int a = s[j + 1] - s[i + 1] - f[i + 1][j];
                int b = s[j] - s[i] - f[i][j - 1];
                f[i][j] = Math.max(a, b);
            }
        }
        return f[0][n - 1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int stoneGameVII(vector<int>& stones) {
        int n = stones.size();
        vector<int> s(n + 1);
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + stones[i];
        }
        vector<vector<int>> f(n, vector<int>(n));
        for (int i = n - 2; i >= 0; --i) {
            for (int j = i + 1; j < n; ++j) {
                int a = s[j + 1] - s[i + 1] - f[i + 1][j];
                int b = s[j] - s[i] - f[i][j - 1];
                f[i][j] = max(a, b);
            }
        }
        return f[0][n - 1];
    }
};
```

#### Go

```go
func stoneGameVII(stones []int) int {
	n := len(stones)
	s := make([]int, n+1)
	for i, x := range stones {
		s[i+1] = s[i] + x
	}
	f := make([][]int, n)
	for i := range f {
		f[i] = make([]int, n)
	}
	for i := n - 2; i >= 0; i-- {
		for j := i + 1; j < n; j++ {
			f[i][j] = max(s[j+1]-s[i+1]-f[i+1][j], s[j]-s[i]-f[i][j-1])
		}
	}
	return f[0][n-1]
}
```

#### TypeScript

```ts
function stoneGameVII(stones: number[]): number {
    const n = stones.length;
    const s: number[] = Array(n + 1).fill(0);
    for (let i = 0; i < n; ++i) {
        s[i + 1] = s[i] + stones[i];
    }
    const f: number[][] = Array.from({ length: n }, () => Array(n).fill(0));
    for (let i = n - 2; ~i; --i) {
        for (let j = i + 1; j < n; ++j) {
            f[i][j] = Math.max(s[j + 1] - s[i + 1] - f[i + 1][j], s[j] - s[i] - f[i][j - 1]);
        }
    }
    return f[0][n - 1];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
