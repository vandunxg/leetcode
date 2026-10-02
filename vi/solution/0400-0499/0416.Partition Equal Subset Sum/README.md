---
comments: true
difficulty: Medium
tags:
    - Array
    - Dynamic Programming
    - Knapsack
    - 0-1 Knapsack
---

<!-- problem:start -->

# [416. Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum)

[中文文档](/solution/0400-0499/0416.Partition%20Equal%20Subset%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>, trả về <code>true</code> <em>nếu có thể chia mảng thành hai tập con sao cho tổng các phần tử ở hai tập bằng nhau; nếu không thì trả về </em><code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,5,11,5]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Có thể chia mảng thành [1, 5, 5] và [11].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,5]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không thể chia mảng thành hai tập con có tổng bằng nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 200</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Để chia thành hai tập có tổng bằng nhau, cần có một tập con có tổng bằng một nửa tổng toàn mảng. Nếu tổng là số lẻ thì không thể chia được. Với $n\le 200$, duyệt mọi tập con sẽ quá tốn kém.
>
> Đây là bài toán knapsack 0-1 với sức chứa $m=s/2$: $f[i][j]$ cho biết có thể tạo tổng $j$ từ $i$ số đầu tiên hay không, bằng cách chọn hoặc bỏ qua $x$. Bảng $n\times m$ phù hợp với giới hạn đề bài.
>
> Trước tiên, loại trường hợp tổng lẻ. Tập rỗng cho ta $f[0][0]=\textit{true}$, làm trạng thái ban đầu cho công thức truy hồi.

<!-- thinking:end -->

Trước hết, tính tổng $s$ của mảng. Nếu tổng lẻ thì không thể chia thành hai tập con có tổng bằng nhau, nên trả về `false`. Nếu tổng chẵn, đặt tổng cần đạt của một tập con là $m = \frac{s}{2}$. Khi đó, bài toán trở thành: có tồn tại tập con nào có tổng phần tử bằng $m$ hay không?

Đặt $f[i][j]$ là trạng thái cho biết có thể chọn một số phần tử trong $i$ số đầu tiên sao cho tổng của chúng đúng bằng $j$ hay không. Ban đầu, $f[0][0] = true$ và các trạng thái còn lại $f[i][j] = false$. Đáp án là $f[n][m]$.

Xét $f[i][j]$: nếu chọn số thứ $i$ là $x$ thì $f[i][j] = f[i - 1][j - x]$. Nếu không chọn số thứ $i$, thì $f[i][j] = f[i - 1][j]$. Do đó, công thức chuyển trạng thái là:

$$
f[i][j] = f[i - 1][j] \textit{ or } f[i - 1][j - x] \textit{ if } j \geq x
$$

Đáp án cuối cùng là $f[n][m]$.

Độ phức tạp thời gian và không gian đều là $O(m \times n)$, trong đó $m$ bằng một nửa tổng các phần tử trong mảng, còn $n$ là độ dài mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        m, mod = divmod(sum(nums), 2)
        if mod:
            return False
        n = len(nums)
        f = [[False] * (m + 1) for _ in range(n + 1)]
        f[0][0] = True
        for i, x in enumerate(nums, 1):
            for j in range(m + 1):
                f[i][j] = f[i - 1][j] or (j >= x and f[i - 1][j - x])
        return f[n][m]
```

#### Java

```java
class Solution {
    public boolean canPartition(int[] nums) {
        // int s = Arrays.stream(nums).sum();
        int s = 0;
        for (int x : nums) {
            s += x;
        }
        if (s % 2 == 1) {
            return false;
        }
        int n = nums.length;
        int m = s >> 1;
        boolean[][] f = new boolean[n + 1][m + 1];
        f[0][0] = true;
        for (int i = 1; i <= n; ++i) {
            int x = nums[i - 1];
            for (int j = 0; j <= m; ++j) {
                f[i][j] = f[i - 1][j] || (j >= x && f[i - 1][j - x]);
            }
        }
        return f[n][m];
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int s = accumulate(nums.begin(), nums.end(), 0);
        if (s % 2 == 1) {
            return false;
        }
        int n = nums.size();
        int m = s >> 1;
        bool f[n + 1][m + 1];
        memset(f, false, sizeof(f));
        f[0][0] = true;
        for (int i = 1; i <= n; ++i) {
            int x = nums[i - 1];
            for (int j = 0; j <= m; ++j) {
                f[i][j] = f[i - 1][j] || (j >= x && f[i - 1][j - x]);
            }
        }
        return f[n][m];
    }
};
```

#### Go

```go
func canPartition(nums []int) bool {
	s := 0
	for _, x := range nums {
		s += x
	}
	if s%2 == 1 {
		return false
	}
	n, m := len(nums), s>>1
	f := make([][]bool, n+1)
	for i := range f {
		f[i] = make([]bool, m+1)
	}
	f[0][0] = true
	for i := 1; i <= n; i++ {
		x := nums[i-1]
		for j := 0; j <= m; j++ {
			f[i][j] = f[i-1][j] || (j >= x && f[i-1][j-x])
		}
	}
	return f[n][m]
}
```

#### TypeScript

```ts
function canPartition(nums: number[]): boolean {
    const s = nums.reduce((a, b) => a + b, 0);
    if (s % 2 === 1) {
        return false;
    }
    const n = nums.length;
    const m = s >> 1;
    const f: boolean[][] = Array.from({ length: n + 1 }, () => Array(m + 1).fill(false));
    f[0][0] = true;
    for (let i = 1; i <= n; ++i) {
        const x = nums[i - 1];
        for (let j = 0; j <= m; ++j) {
            f[i][j] = f[i - 1][j] || (j >= x && f[i - 1][j - x]);
        }
    }
    return f[n][m];
}
```

#### Rust

```rust
impl Solution {
    pub fn can_partition(nums: Vec<i32>) -> bool {
        let s: i32 = nums.iter().sum();
        if s % 2 != 0 {
            return false;
        }
        let m = (s / 2) as usize;
        let n = nums.len();
        let mut f = vec![vec![false; m + 1]; n + 1];
        f[0][0] = true;

        for i in 1..=n {
            let x = nums[i - 1] as usize;
            for j in 0..=m {
                f[i][j] = f[i - 1][j] || (j >= x && f[i - 1][j - x]);
            }
        }

        f[n][m]
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {boolean}
 */
var canPartition = function (nums) {
    const s = nums.reduce((a, b) => a + b, 0);
    if (s % 2 === 1) {
        return false;
    }
    const n = nums.length;
    const m = s >> 1;
    const f = Array.from({ length: n + 1 }, () => Array(m + 1).fill(false));
    f[0][0] = true;
    for (let i = 1; i <= n; ++i) {
        const x = nums[i - 1];
        for (let j = 0; j <= m; ++j) {
            f[i][j] = f[i - 1][j] || (j >= x && f[i - 1][j - x]);
        }
    }
    return f[n][m];
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động (Tối ưu bộ nhớ)

<!-- thinking:start -->

> **Tư duy**
>
> Hàng $i$ chỉ phụ thuộc vào hàng $i-1$, và mỗi phần tử chỉ được dùng một lần. Vì vậy, cập nhật $j$ theo thứ tự giảm dần và dùng mảng một chiều. Độ phức tạp không gian giảm từ $O(nm)$ xuống $O(m)$.

<!-- thinking:end -->

Ta nhận thấy trong lời giải 1, $f[i][j]$ chỉ phụ thuộc vào $f[i - 1][\cdot]$. Vì vậy, có thể nén mảng hai chiều thành mảng một chiều.

Độ phức tạp thời gian là $O(n \times m)$ và độ phức tạp không gian là $O(m)$, trong đó $n$ là độ dài mảng, còn $m$ bằng một nửa tổng các phần tử trong mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        m, mod = divmod(sum(nums), 2)
        if mod:
            return False
        f = [True] + [False] * m
        for x in nums:
            for j in range(m, x - 1, -1):
                f[j] = f[j] or f[j - x]
        return f[m]
```

#### Java

```java
class Solution {
    public boolean canPartition(int[] nums) {
        // int s = Arrays.stream(nums).sum();
        int s = 0;
        for (int x : nums) {
            s += x;
        }
        if (s % 2 == 1) {
            return false;
        }
        int m = s >> 1;
        boolean[] f = new boolean[m + 1];
        f[0] = true;
        for (int x : nums) {
            for (int j = m; j >= x; --j) {
                f[j] |= f[j - x];
            }
        }
        return f[m];
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int s = accumulate(nums.begin(), nums.end(), 0);
        if (s % 2 == 1) {
            return false;
        }
        int m = s >> 1;
        bool f[m + 1];
        memset(f, false, sizeof(f));
        f[0] = true;
        for (int& x : nums) {
            for (int j = m; j >= x; --j) {
                f[j] |= f[j - x];
            }
        }
        return f[m];
    }
};
```

#### Go

```go
func canPartition(nums []int) bool {
	s := 0
	for _, x := range nums {
		s += x
	}
	if s%2 == 1 {
		return false
	}
	m := s >> 1
	f := make([]bool, m+1)
	f[0] = true
	for _, x := range nums {
		for j := m; j >= x; j-- {
			f[j] = f[j] || f[j-x]
		}
	}
	return f[m]
}
```

#### TypeScript

```ts
function canPartition(nums: number[]): boolean {
    const s = nums.reduce((a, b) => a + b, 0);
    if (s % 2 === 1) {
        return false;
    }
    const m = s >> 1;
    const f: boolean[] = Array(m + 1).fill(false);
    f[0] = true;
    for (const x of nums) {
        for (let j = m; j >= x; --j) {
            f[j] = f[j] || f[j - x];
        }
    }
    return f[m];
}
```

#### Rust

```rust
impl Solution {
    pub fn can_partition(nums: Vec<i32>) -> bool {
        let s: i32 = nums.iter().sum();
        if s % 2 != 0 {
            return false;
        }
        let m = (s / 2) as usize;
        let mut f = vec![false; m + 1];
        f[0] = true;

        for x in nums {
            let x = x as usize;
            for j in (x..=m).rev() {
                f[j] = f[j] || f[j - x];
            }
        }

        f[m]
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {boolean}
 */
var canPartition = function (nums) {
    const s = nums.reduce((a, b) => a + b, 0);
    if (s % 2 === 1) {
        return false;
    }
    const m = s >> 1;
    const f = Array(m + 1).fill(false);
    f[0] = true;
    for (const x of nums) {
        for (let j = m; j >= x; --j) {
            f[j] = f[j] || f[j - x];
        }
    }
    return f[m];
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
