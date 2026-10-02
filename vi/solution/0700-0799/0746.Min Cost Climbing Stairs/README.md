---
comments: true
difficulty: Easy
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [746. Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs)

[中文文档](/solution/0700-0799/0746.Min%20Cost%20Climbing%20Stairs/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>cost</code>, trong đó <code>cost[i]</code> là chi phí của bậc thứ <code>i<sup>th</sup></code> trên cầu thang. Sau khi trả chi phí, bạn có thể bước lên một hoặc hai bậc.</p>

<p>Bạn có thể bắt đầu ở bậc có chỉ số <code>0</code> hoặc bậc có chỉ số <code>1</code>.</p>

<p>Trả về <em>chi phí nhỏ nhất để lên đến đỉnh cầu thang</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> cost = [10,<u>15</u>,20]
<strong>Đầu ra:</strong> 15
<strong>Giải thích:</strong> Bạn sẽ bắt đầu ở chỉ số 1.
- Trả 15 rồi bước lên hai bậc để tới đỉnh.
Tổng chi phí là 15.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> cost = [<u>1</u>,100,<u>1</u>,1,<u>1</u>,100,<u>1</u>,<u>1</u>,100,<u>1</u>]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Bạn sẽ bắt đầu ở chỉ số 0.
- Trả 1 rồi bước lên hai bậc để tới chỉ số 2.
- Trả 1 rồi bước lên hai bậc để tới chỉ số 4.
- Trả 1 rồi bước lên hai bậc để tới chỉ số 6.
- Trả 1 rồi bước lên một bậc để tới chỉ số 7.
- Trả 1 rồi bước lên hai bậc để tới chỉ số 9.
- Trả 1 rồi bước lên một bậc để tới đỉnh.
Tổng chi phí là 6.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= cost.length &lt;= 1000</code></li>
	<li><code>0 &lt;= cost[i] &lt;= 999</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Bắt đầu ở chỉ số $0$ hoặc $1$, trả chi phí của bậc hiện tại rồi bước lên một hoặc hai bậc. Vì $n\le 1000$, đệ quy thông thường sẽ tính lặp lại các bậc.
>
> Chi phí tính từ bậc $i$ chỉ phụ thuộc vào $i+1$ và $i+2$; vượt quá đỉnh thì chi phí bằng $0$. Memoization giúp tính mỗi chỉ số đúng một lần.
>
> Đáp án là $\min(dfs(0), dfs(1))$. Độ phức tạp thời gian và không gian đều là $O(n)$.

<!-- thinking:end -->

Ta định nghĩa hàm $\textit{dfs}(i)$ biểu thị chi phí nhỏ nhất để leo cầu thang khi bắt đầu từ bậc thứ $i$. Vì vậy, đáp án là $\min(\textit{dfs}(0), \textit{dfs}(1))$.

Hàm $\textit{dfs}(i)$ hoạt động như sau:

- Nếu $i \ge \textit{len(cost)}$, vị trí hiện tại đã vượt quá đỉnh cầu thang nên không cần leo tiếp; trả về $0$;
- Nếu không, ta có thể trả $\textit{cost}[i]$ để bước lên $1$ bậc rồi gọi đệ quy $\textit{dfs}(i + 1)$; hoặc trả cùng chi phí đó để bước lên $2$ bậc rồi gọi đệ quy $\textit{dfs}(i + 2)$;
- Trả về chi phí nhỏ hơn trong hai lựa chọn.

Để tránh tính lặp, ta dùng memoization và lưu kết quả đã tính trong mảng hoặc hash table.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài mảng $\textit{cost}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        @cache
        def dfs(i: int) -> int:
            if i >= len(cost):
                return 0
            return cost[i] + min(dfs(i + 1), dfs(i + 2))

        return min(dfs(0), dfs(1))
```

#### Java

```java
class Solution {
    private Integer[] f;
    private int[] cost;

    public int minCostClimbingStairs(int[] cost) {
        this.cost = cost;
        f = new Integer[cost.length];
        return Math.min(dfs(0), dfs(1));
    }

    private int dfs(int i) {
        if (i >= cost.length) {
            return 0;
        }
        if (f[i] == null) {
            f[i] = cost[i] + Math.min(dfs(i + 1), dfs(i + 2));
        }
        return f[i];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        int n = cost.size();
        int f[n];
        memset(f, -1, sizeof(f));
        auto dfs = [&](this auto&& dfs, int i) -> int {
            if (i >= n) {
                return 0;
            }
            if (f[i] < 0) {
                f[i] = cost[i] + min(dfs(i + 1), dfs(i + 2));
            }
            return f[i];
        };
        return min(dfs(0), dfs(1));
    }
};
```

#### Go

```go
func minCostClimbingStairs(cost []int) int {
	n := len(cost)
	f := make([]int, n)
	for i := range f {
		f[i] = -1
	}
	var dfs func(int) int
	dfs = func(i int) int {
		if i >= n {
			return 0
		}
		if f[i] < 0 {
			f[i] = cost[i] + min(dfs(i+1), dfs(i+2))
		}
		return f[i]
	}
	return min(dfs(0), dfs(1))
}
```

#### TypeScript

```ts
function minCostClimbingStairs(cost: number[]): number {
    const n = cost.length;
    const f: number[] = Array(n).fill(-1);
    const dfs = (i: number): number => {
        if (i >= n) {
            return 0;
        }
        if (f[i] < 0) {
            f[i] = cost[i] + Math.min(dfs(i + 1), dfs(i + 2));
        }
        return f[i];
    };
    return Math.min(dfs(0), dfs(1));
}
```

#### Rust

```rust
impl Solution {
    pub fn min_cost_climbing_stairs(cost: Vec<i32>) -> i32 {
        let n = cost.len();
        let mut f = vec![-1; n];

        fn dfs(i: usize, cost: &Vec<i32>, f: &mut Vec<i32>, n: usize) -> i32 {
            if i >= n {
                return 0;
            }
            if f[i] < 0 {
                let next1 = dfs(i + 1, cost, f, n);
                let next2 = dfs(i + 2, cost, f, n);
                f[i] = cost[i] + next1.min(next2);
            }
            f[i]
        }

        dfs(0, &cost, &mut f, n).min(dfs(1, &cost, &mut f, n))
    }
}
```

#### JavaScript

```js
function minCostClimbingStairs(cost) {
    const n = cost.length;
    const f = Array(n).fill(-1);
    const dfs = i => {
        if (i >= n) {
            return 0;
        }
        if (f[i] < 0) {
            f[i] = cost[i] + Math.min(dfs(i + 1), dfs(i + 2));
        }
        return f[i];
    };
    return Math.min(dfs(0), dfs(1));
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Dynamic Programming

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 đã có độ phức tạp tuyến tính nhưng dùng đệ quy. Ta có thể dùng cùng công thức để tính tiến từ đầu.
>
> $f[i]$ là chi phí nhỏ nhất để tới chỉ số $i$, đi từ $i-1$ hoặc $i-2$ và trả chi phí của bậc xuất phát. $f[n]$ tương ứng với đỉnh cầu thang.

<!-- thinking:end -->

Ta định nghĩa $f[i]$ là chi phí nhỏ nhất để tới bậc thứ $i$. Ban đầu, $f[0] = f[1] = 0$, và đáp án là $f[n]$.

Khi $i \ge 2$, ta có thể tới bậc thứ $i$ từ bậc $(i - 1)$ bằng một bước hoặc từ bậc $(i - 2)$ bằng hai bước. Do đó, công thức chuyển trạng thái là:

$$
f[i] = \min(f[i - 1] + \textit{cost}[i - 1], f[i - 2] + \textit{cost}[i - 2])
$$

Đáp án cuối cùng là $f[n]$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài mảng $\textit{cost}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        n = len(cost)
        f = [0] * (n + 1)
        for i in range(2, n + 1):
            f[i] = min(f[i - 2] + cost[i - 2], f[i - 1] + cost[i - 1])
        return f[n]
```

#### Java

```java
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        int n = cost.length;
        int[] f = new int[n + 1];
        for (int i = 2; i <= n; ++i) {
            f[i] = Math.min(f[i - 2] + cost[i - 2], f[i - 1] + cost[i - 1]);
        }
        return f[n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        int n = cost.size();
        vector<int> f(n + 1);
        for (int i = 2; i <= n; ++i) {
            f[i] = min(f[i - 2] + cost[i - 2], f[i - 1] + cost[i - 1]);
        }
        return f[n];
    }
};
```

#### Go

```go
func minCostClimbingStairs(cost []int) int {
	n := len(cost)
	f := make([]int, n+1)
	for i := 2; i <= n; i++ {
		f[i] = min(f[i-1]+cost[i-1], f[i-2]+cost[i-2])
	}
	return f[n]
}
```

#### TypeScript

```ts
function minCostClimbingStairs(cost: number[]): number {
    const n = cost.length;
    const f: number[] = Array(n + 1).fill(0);
    for (let i = 2; i <= n; ++i) {
        f[i] = Math.min(f[i - 1] + cost[i - 1], f[i - 2] + cost[i - 2]);
    }
    return f[n];
}
```

#### Rust

```rust
impl Solution {
    pub fn min_cost_climbing_stairs(cost: Vec<i32>) -> i32 {
        let n = cost.len();
        let mut f = vec![0; n + 1];
        for i in 2..=n {
            f[i] = std::cmp::min(f[i - 2] + cost[i - 2], f[i - 1] + cost[i - 1]);
        }
        f[n]
    }
}
```

#### JavaScript

```js
function minCostClimbingStairs(cost) {
    const n = cost.length;
    const f = Array(n + 1).fill(0);
    for (let i = 2; i <= n; ++i) {
        f[i] = Math.min(f[i - 1] + cost[i - 1], f[i - 2] + cost[i - 2]);
    }
    return f[n];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3: Dynamic Programming (Tối ưu bộ nhớ)

<!-- thinking:start -->

> **Tư duy**
>
> $f[i]$ chỉ cần hai giá trị trước đó, nên chỉ cần hai biến vô hướng. Độ phức tạp không gian là $O(1)$.

<!-- thinking:end -->

Ta nhận thấy công thức chuyển trạng thái của $f[i]$ chỉ phụ thuộc vào $f[i - 1]$ và $f[i - 2]$. Vì vậy, có thể dùng hai biến $f$ và $g$ để lần lượt lưu giá trị của $f[i - 2]$ và $f[i - 1]$, giảm độ phức tạp không gian xuống $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        f = g = 0
        for i in range(2, len(cost) + 1):
            f, g = g, min(f + cost[i - 2], g + cost[i - 1])
        return g
```

#### Java

```java
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        int f = 0, g = 0;
        for (int i = 2; i <= cost.length; ++i) {
            int gg = Math.min(f + cost[i - 2], g + cost[i - 1]);
            f = g;
            g = gg;
        }
        return g;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        int f = 0, g = 0;
        for (int i = 2; i <= cost.size(); ++i) {
            int gg = min(f + cost[i - 2], g + cost[i - 1]);
            f = g;
            g = gg;
        }
        return g;
    }
};
```

#### Go

```go
func minCostClimbingStairs(cost []int) int {
	var f, g int
	for i := 2; i <= n; i++ {
		f, g = g, min(f+cost[i-2], g+cost[i-1])
	}
	return g
}
```

#### TypeScript

```ts
function minCostClimbingStairs(cost: number[]): number {
    let [f, g] = [0, 0];
    for (let i = 1; i < cost.length; ++i) {
        [f, g] = [g, Math.min(f + cost[i - 1], g + cost[i])];
    }
    return g;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_cost_climbing_stairs(cost: Vec<i32>) -> i32 {
        let (mut f, mut g) = (0, 0);
        for i in 2..=cost.len() {
            let gg = std::cmp::min(f + cost[i - 2], g + cost[i - 1]);
            f = g;
            g = gg;
        }
        g
    }
}
```

#### JavaScript

```js
function minCostClimbingStairs(cost) {
    let [f, g] = [0, 0];
    for (let i = 1; i < cost.length; ++i) {
        [f, g] = [g, Math.min(f + cost[i - 1], g + cost[i])];
    }
    return g;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
