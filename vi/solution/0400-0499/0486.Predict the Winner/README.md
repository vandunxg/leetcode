---
comments: true
difficulty: Medium
tags:
    - Recursion
    - Minimax
    - Array
    - Math
    - Dynamic Programming
    - Game Theory
    - Zero-Sum Game
---

<!-- problem:start -->

# [486. Predict the Winner](https://leetcode.com/problems/predict-the-winner)

[中文文档](/solution/0400-0499/0486.Predict%20the%20Winner/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>. Hai người chơi, người chơi 1 và người chơi 2, sẽ chơi một trò chơi với mảng này.</p>

<p>Hai người chơi lần lượt chọn số, người chơi 1 đi trước. Cả hai bắt đầu với điểm <code>0</code>. Mỗi lượt, người chơi lấy một số ở một trong hai đầu mảng (tức <code>nums[0]</code> hoặc <code>nums[nums.length - 1]</code>), làm mảng ngắn đi <code>1</code> phần tử, rồi cộng số vừa chọn vào điểm của mình. Trò chơi kết thúc khi mảng không còn phần tử.</p>

<p>Trả về <code>true</code> nếu người chơi 1 có thể thắng. Nếu hai người bằng điểm thì người chơi 1 vẫn được tính là thắng, vì vậy cũng trả về <code>true</code>. Có thể giả định cả hai người đều chơi tối ưu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,5,2]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Ban đầu, người chơi 1 có thể chọn 1 hoặc 2.
Nếu chọn 2 (hoặc 1), người chơi 2 có thể chọn 1 (hoặc 2) hoặc 5. Nếu người chơi 2 chọn 5, người chơi 1 sẽ còn lại 1 (hoặc 2).
Vì vậy, điểm cuối cùng của người chơi 1 là 1 + 2 = 3, còn người chơi 2 là 5.
Do đó, người chơi 1 không thể thắng và cần trả về false.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,5,233,7]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Đầu tiên, người chơi 1 chọn 1. Sau đó, người chơi 2 phải chọn 5 hoặc 7. Dù người chơi 2 chọn số nào, người chơi 1 vẫn có thể chọn 233.
Cuối cùng, người chơi 1 đạt điểm cao hơn (234) so với người chơi 2 (12), nên trả về true để biểu thị người chơi 1 có thể thắng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 20</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>7</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có memoization

<!-- thinking:start -->

> **Tư duy**
>
> Người chơi chọn số ở một trong hai đầu; người chơi đầu tiên thắng nếu điểm của họ ít nhất bằng điểm người còn lại. Cây trạng thái có thể lặp lại cùng một đoạn.
>
> $dfs(i,j)$ là chênh lệch điểm tốt nhất trên đoạn $[i,j]$: chọn đầu trái thì được $nums[i]-dfs(i+1,j)$; chọn đầu phải thì tương tự. Người chơi đầu tiên thắng khi và chỉ khi $dfs(0,n-1)\ge 0$.
>
> Có $O(n^2)$ đoạn; memoization đảm bảo mỗi đoạn còn lại được tính một lần theo góc nhìn của người đang đi.

<!-- thinking:end -->

Ta định nghĩa hàm $\textit{dfs}(i, j)$ là chênh lệch điểm lớn nhất giữa người chơi hiện tại và đối thủ khi chỉ còn các số từ vị trí $i$ đến vị trí $j$. Đáp án là $\textit{dfs}(0, n - 1) \geq 0$.

Hàm $\textit{dfs}(i, j)$ được tính như sau:

- Nếu $i > j$, không còn số nào để chọn nên người hiện tại không lấy được điểm nào; chênh lệch bằng $0$, tức $\textit{dfs}(i, j) = 0$.
- Nếu không, người chơi hiện tại có hai lựa chọn. Nếu chọn số ở vị trí $i$, chênh lệch điểm giữa họ và đối thủ là $\textit{nums}[i] - \textit{dfs}(i + 1, j)$. Nếu chọn số ở vị trí $j$, chênh lệch là $\textit{nums}[j] - \textit{dfs}(i, j - 1)$. Người chơi hiện tại chọn phương án tạo ra chênh lệch lớn hơn, nên $\textit{dfs}(i, j) = \max(\textit{nums}[i] - \textit{dfs}(i + 1, j), \textit{nums}[j] - \textit{dfs}(i, j - 1))$.

Cuối cùng, chỉ cần kiểm tra $\textit{dfs}(0, n - 1) \geq 0$.

Để tránh tính toán lặp, ta dùng memoization. Mảng $f$ lưu các giá trị của $\textit{dfs}(i, j)$. Khi hàm được gọi lại, ta lấy kết quả trực tiếp từ $f$ thay vì tính lại.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là độ dài mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def predictTheWinner(self, nums: List[int]) -> bool:
        @cache
        def dfs(i: int, j: int) -> int:
            if i > j:
                return 0
            return max(nums[i] - dfs(i + 1, j), nums[j] - dfs(i, j - 1))

        return dfs(0, len(nums) - 1) >= 0
```

#### Java

```java
class Solution {
    private int[] nums;
    private int[][] f;

    public boolean predictTheWinner(int[] nums) {
        this.nums = nums;
        int n = nums.length;
        f = new int[n][n];
        return dfs(0, n - 1) >= 0;
    }

    private int dfs(int i, int j) {
        if (i > j) {
            return 0;
        }
        if (f[i][j] != 0) {
            return f[i][j];
        }
        return f[i][j] = Math.max(nums[i] - dfs(i + 1, j), nums[j] - dfs(i, j - 1));
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool predictTheWinner(vector<int>& nums) {
        int n = nums.size();
        vector<vector<int>> f(n, vector<int>(n));
        auto dfs = [&](this auto&& dfs, int i, int j) -> int {
            if (i > j) {
                return 0;
            }
            if (f[i][j]) {
                return f[i][j];
            }
            return f[i][j] = max(nums[i] - dfs(i + 1, j), nums[j] - dfs(i, j - 1));
        };
        return dfs(0, n - 1) >= 0;
    }
};
```

#### Go

```go
func predictTheWinner(nums []int) bool {
	n := len(nums)
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
			f[i][j] = max(nums[i]-dfs(i+1, j), nums[j]-dfs(i, j-1))
		}
		return f[i][j]
	}
	return dfs(0, n-1) >= 0
}
```

#### TypeScript

```ts
function predictTheWinner(nums: number[]): boolean {
    const n = nums.length;
    const f: number[][] = Array.from({ length: n }, () => Array(n).fill(0));
    const dfs = (i: number, j: number): number => {
        if (i > j) {
            return 0;
        }
        if (f[i][j] === 0) {
            f[i][j] = Math.max(nums[i] - dfs(i + 1, j), nums[j] - dfs(i, j - 1));
        }
        return f[i][j];
    };
    return dfs(0, n - 1) >= 0;
}
```

#### Rust

```rust
impl Solution {
    pub fn predict_the_winner(nums: Vec<i32>) -> bool {
        let n = nums.len();
        let mut f = vec![vec![0; n]; n];
        Self::dfs(&nums, &mut f, 0, n - 1) >= 0
    }

    fn dfs(nums: &Vec<i32>, f: &mut Vec<Vec<i32>>, i: usize, j: usize) -> i32 {
        if i == j {
            return nums[i] as i32;
        }
        if f[i][j] != 0 {
            return f[i][j];
        }
        f[i][j] = std::cmp::max(
            nums[i] - Self::dfs(nums, f, i + 1, j),
            nums[j] - Self::dfs(nums, f, i, j - 1)
        );
        f[i][j]
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
> Ta dùng cùng công thức truy hồi để điền bảng: bắt đầu từ đường chéo gồm các đoạn độ dài $1$ của $f[i][j]$, sau đó mở rộng bằng cách giảm $i$ và tăng $j$. Không cần call stack, độ phức tạp tiệm cận vẫn như cũ.

<!-- thinking:end -->

Ta cũng có thể dùng quy hoạch động. Định nghĩa $f[i][j]$ là chênh lệch điểm lớn nhất mà người chơi hiện tại có thể đạt được trên đoạn $\textit{nums}[i..j]$. Đáp án cuối cùng là $f[0][n - 1] \geq 0$.

Ban đầu, $f[i][i] = \textit{nums}[i]$, vì khi chỉ còn một số, người chơi hiện tại chỉ có thể lấy số đó và chênh lệch điểm bằng $\textit{nums}[i]$.

Xét $f[i][j]$ với $i < j$, có hai trường hợp:

- Nếu người chơi hiện tại chọn $\textit{nums}[i]$, các số còn lại là $\textit{nums}[i + 1..j]$ và đến lượt đối thủ. Khi đó, $f[i][j] = \textit{nums}[i] - f[i + 1][j]$.
- Nếu người chơi hiện tại chọn $\textit{nums}[j]$, các số còn lại là $\textit{nums}[i..j - 1]$ và đến lượt đối thủ. Khi đó, $f[i][j] = \textit{nums}[j] - f[i][j - 1]$.

Vì vậy, công thức chuyển trạng thái là $f[i][j] = \max(\textit{nums}[i] - f[i + 1][j], \textit{nums}[j] - f[i][j - 1])$.

Cuối cùng, chỉ cần kiểm tra $f[0][n - 1] \geq 0$.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là độ dài mảng $\textit{nums}$.

Bài toán tương tự:

- [877. Stone Game](https://github.com/doocs/leetcode/blob/main/solution/0800-0899/0877.Stone%20Game/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def predictTheWinner(self, nums: List[int]) -> bool:
        n = len(nums)
        f = [[0] * n for _ in range(n)]
        for i, x in enumerate(nums):
            f[i][i] = x
        for i in range(n - 2, -1, -1):
            for j in range(i + 1, n):
                f[i][j] = max(nums[i] - f[i + 1][j], nums[j] - f[i][j - 1])
        return f[0][n - 1] >= 0
```

#### Java

```java
class Solution {
    public boolean predictTheWinner(int[] nums) {
        int n = nums.length;
        int[][] f = new int[n][n];
        for (int i = 0; i < n; ++i) {
            f[i][i] = nums[i];
        }
        for (int i = n - 2; i >= 0; --i) {
            for (int j = i + 1; j < n; ++j) {
                f[i][j] = Math.max(nums[i] - f[i + 1][j], nums[j] - f[i][j - 1]);
            }
        }
        return f[0][n - 1] >= 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool predictTheWinner(vector<int>& nums) {
        int n = nums.size();
        int f[n][n];
        memset(f, 0, sizeof(f));
        for (int i = 0; i < n; ++i) {
            f[i][i] = nums[i];
        }
        for (int i = n - 2; ~i; --i) {
            for (int j = i + 1; j < n; ++j) {
                f[i][j] = max(nums[i] - f[i + 1][j], nums[j] - f[i][j - 1]);
            }
        }
        return f[0][n - 1] >= 0;
    }
};
```

#### Go

```go
func predictTheWinner(nums []int) bool {
	n := len(nums)
	f := make([][]int, n)
	for i, x := range nums {
		f[i] = make([]int, n)
		f[i][i] = x
	}
	for i := n - 2; i >= 0; i-- {
		for j := i + 1; j < n; j++ {
			f[i][j] = max(nums[i]-f[i+1][j], nums[j]-f[i][j-1])
		}
	}
	return f[0][n-1] >= 0
}
```

#### TypeScript

```ts
function predictTheWinner(nums: number[]): boolean {
    const n = nums.length;
    const f: number[][] = Array.from({ length: n }, () => Array(n).fill(0));
    for (let i = 0; i < n; ++i) {
        f[i][i] = nums[i];
    }
    for (let i = n - 2; i >= 0; --i) {
        for (let j = i + 1; j < n; ++j) {
            f[i][j] = Math.max(nums[i] - f[i + 1][j], nums[j] - f[i][j - 1]);
        }
    }
    return f[0][n - 1] >= 0;
}
```

#### Rust

```rust
impl Solution {
    pub fn predict_the_winner(nums: Vec<i32>) -> bool {
        let n = nums.len();
        let mut f = vec![vec![0; n]; n];

        for i in 0..n {
            f[i][i] = nums[i];
        }

        for i in (0..n - 1).rev() {
            for j in i + 1..n {
                f[i][j] = std::cmp::max(nums[i] - f[i + 1][j], nums[j] - f[i][j - 1]);
            }
        }

        f[0][n - 1] >= 0
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
