---
comments: true
difficulty: Medium
rating: 2092
source: Weekly Contest 137 Q4
tags:
    - Array
    - Dynamic Programming
    - Knapsack
    - 0-1 Knapsack
---

<!-- problem:start -->

# [1049. Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii)

[中文文档](/solution/1000-1099/1049.Last%20Stone%20Weight%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>stones</code>, trong đó <code>stones[i]</code> là khối lượng của viên đá thứ <code>i<sup>th</sup></code>.</p>

<p>Ta chơi một trò chơi với các viên đá. Ở mỗi lượt, chọn hai viên bất kỳ và đập chúng vào nhau. Giả sử khối lượng của chúng lần lượt là <code>x</code> và <code>y</code>, với <code>x &lt;= y</code>. Kết quả sau khi đập là:</p>

<ul>
	<li>Nếu <code>x == y</code>, cả hai viên đá đều bị phá hủy; còn</li>
	<li>Nếu <code>x != y</code>, viên đá có khối lượng <code>x</code> bị phá hủy, còn viên đá có khối lượng <code>y</code> có khối lượng mới là <code>y - x</code>.</li>
</ul>

<p>Khi trò chơi kết thúc, còn lại <strong>nhiều nhất một</strong> viên đá.</p>

<p>Hãy trả về <em>khối lượng nhỏ nhất có thể của viên đá còn lại</em>. Nếu không còn viên đá nào, trả về <code>0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> stones = [2,7,4,1,8,1]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
Ta có thể đập 2 và 4 để còn lại 2, khi đó mảng trở thành [2,7,1,8,1]. Tiếp theo,
ta đập 7 và 8 để còn lại 1, khi đó mảng trở thành [2,1,1,1]. Tiếp theo,
ta đập 2 và 1 để còn lại 1, khi đó mảng trở thành [1,1,1]. Tiếp theo,
ta đập 1 và 1 để cả hai bị phá hủy, khi đó mảng trở thành [1]. Đây là giá trị tối ưu.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> stones = [31,26,33,21,40]
<strong>Đầu ra:</strong> 5
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= stones.length &lt;= 30</code></li>
	<li><code>1 &lt;= stones[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lần đập tương đương với việc gán dấu trái ngược cho hai viên đá, nên khối lượng còn lại là độ chênh lệch tuyệt đối giữa tổng của hai tập con. $n\le 30$ nhưng tổng khối lượng chỉ vài nghìn, vì vậy có thể dùng bài toán ba lô $0$-$1$ với sức chứa $\lfloor s/2\rfloor$: chọn tổng khối lượng gần một nửa nhất có thể.
>
> $\textit{dp}[i][j]$ là khối lượng lớn nhất không vượt quá $j$ có thể đạt được từ $i$ viên đá đầu tiên, bằng cách chọn hoặc không chọn từng viên.
>
> Đáp án là $s-2\cdot\textit{dp}[n][\lfloor s/2\rfloor]$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def lastStoneWeightII(self, stones: List[int]) -> int:
        s = sum(stones)
        m, n = len(stones), s >> 1
        dp = [[0] * (n + 1) for _ in range(m + 1)]
        for i in range(1, m + 1):
            for j in range(n + 1):
                dp[i][j] = dp[i - 1][j]
                if stones[i - 1] <= j:
                    dp[i][j] = max(
                        dp[i][j], dp[i - 1][j - stones[i - 1]] + stones[i - 1]
                    )
        return s - 2 * dp[-1][-1]
```

#### Java

```java
class Solution {
    public int lastStoneWeightII(int[] stones) {
        int s = 0;
        for (int v : stones) {
            s += v;
        }
        int m = stones.length;
        int n = s >> 1;
        int[][] dp = new int[m + 1][n + 1];
        for (int i = 1; i <= m; ++i) {
            for (int j = 0; j <= n; ++j) {
                dp[i][j] = dp[i - 1][j];
                if (stones[i - 1] <= j) {
                    dp[i][j] = Math.max(dp[i][j], dp[i - 1][j - stones[i - 1]] + stones[i - 1]);
                }
            }
        }
        return s - dp[m][n] * 2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int lastStoneWeightII(vector<int>& stones) {
        int s = accumulate(stones.begin(), stones.end(), 0);
        int m = stones.size(), n = s >> 1;
        vector<vector<int>> dp(m + 1, vector<int>(n + 1));
        for (int i = 1; i <= m; ++i) {
            for (int j = 0; j <= n; ++j) {
                dp[i][j] = dp[i - 1][j];
                if (stones[i - 1] <= j) dp[i][j] = max(dp[i][j], dp[i - 1][j - stones[i - 1]] + stones[i - 1]);
            }
        }
        return s - dp[m][n] * 2;
    }
};
```

#### Go

```go
func lastStoneWeightII(stones []int) int {
	s := 0
	for _, v := range stones {
		s += v
	}
	m, n := len(stones), s>>1
	dp := make([][]int, m+1)
	for i := range dp {
		dp[i] = make([]int, n+1)
	}
	for i := 1; i <= m; i++ {
		for j := 0; j <= n; j++ {
			dp[i][j] = dp[i-1][j]
			if stones[i-1] <= j {
				dp[i][j] = max(dp[i][j], dp[i-1][j-stones[i-1]]+stones[i-1])
			}
		}
	}
	return s - dp[m][n]*2
}
```

#### Rust

```rust
impl Solution {
    #[allow(dead_code)]
    pub fn last_stone_weight_ii(stones: Vec<i32>) -> i32 {
        let n = stones.len();
        let mut sum = 0;

        for e in &stones {
            sum += *e;
        }

        let m = (sum / 2) as usize;
        let mut dp: Vec<Vec<i32>> = vec![vec![0; m + 1]; n + 1];

        // Begin the actual dp process
        for i in 1..=n {
            for j in 1..=m {
                dp[i][j] = if stones[i - 1] > (j as i32) {
                    dp[i - 1][j]
                } else {
                    std::cmp::max(
                        dp[i - 1][j],
                        dp[i - 1][j - (stones[i - 1] as usize)] + stones[i - 1],
                    )
                };
            }
        }

        sum - 2 * dp[n][m]
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} stones
 * @return {number}
 */
var lastStoneWeightII = function (stones) {
    let s = 0;
    for (const v of stones) {
        s += v;
    }
    const m = stones.length;
    const n = s >> 1;
    const dp = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));
    for (let i = 1; i <= m; ++i) {
        for (let j = 0; j <= n; ++j) {
            dp[i][j] = dp[i - 1][j];
            if (stones[i - 1] <= j) {
                dp[i][j] = Math.max(dp[i][j], dp[i - 1][j - stones[i - 1]] + stones[i - 1]);
            }
        }
    }
    return s - dp[m][n] * 2;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động tối ưu

<!-- thinking:start -->

> **Tư duy**
>
> Hàng $i$ chỉ phụ thuộc vào hàng $i-1$. Dùng mảng một chiều và cập nhật từ sức chứa lớn xuống nhỏ sẽ tái sử dụng cùng công thức chuyển trạng thái mà không chọn một viên đá nhiều lần; độ phức tạp không gian giảm còn $O(s)$.

<!-- thinking:end -->

$dp[i][j]$ chỉ phụ thuộc vào hàng trước $dp[i - 1][\cdot]$, nên có thể bỏ chiều thứ nhất và duyệt sức chứa từ lớn xuống nhỏ, giảm độ phức tạp không gian còn $O(\textit{sum})$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def lastStoneWeightII(self, stones: List[int]) -> int:
        s = sum(stones)
        m, n = len(stones), s >> 1
        dp = [0] * (n + 1)
        for v in stones:
            for j in range(n, v - 1, -1):
                dp[j] = max(dp[j], dp[j - v] + v)
        return s - dp[-1] * 2
```

#### Java

```java
class Solution {
    public int lastStoneWeightII(int[] stones) {
        int s = 0;
        for (int v : stones) {
            s += v;
        }
        int m = stones.length;
        int n = s >> 1;
        int[] dp = new int[n + 1];
        for (int v : stones) {
            for (int j = n; j >= v; --j) {
                dp[j] = Math.max(dp[j], dp[j - v] + v);
            }
        }
        return s - dp[n] * 2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int lastStoneWeightII(vector<int>& stones) {
        int s = accumulate(stones.begin(), stones.end(), 0);
        int n = s >> 1;
        vector<int> dp(n + 1);
        for (int& v : stones)
            for (int j = n; j >= v; --j)
                dp[j] = max(dp[j], dp[j - v] + v);
        return s - dp[n] * 2;
    }
};
```

#### Go

```go
func lastStoneWeightII(stones []int) int {
	s := 0
	for _, v := range stones {
		s += v
	}
	n := s >> 1
	dp := make([]int, n+1)
	for _, v := range stones {
		for j := n; j >= v; j-- {
			dp[j] = max(dp[j], dp[j-v]+v)
		}
	}
	return s - dp[n]*2
}
```

#### JavaScript

```js
/**
 * @param {number[]} stones
 * @return {number}
 */
var lastStoneWeightII = function (stones) {
    let s = 0;
    for (const v of stones) {
        s += v;
    }
    const n = s >> 1;
    const dp = Array(n + 1).fill(0);
    for (const v of stones) {
        for (let j = n; j >= v; --j) {
            dp[j] = Math.max(dp[j], dp[j - v] + v);
        }
    }
    return s - dp[n] * 2;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
