---
comments: true
difficulty: Medium
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3647. Maximum Weight in Two Bags 🔒](https://leetcode.com/problems/maximum-weight-in-two-bags)

[中文文档](/solution/3600-3699/3647.Maximum%20Weight%20in%20Two%20Bags/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>weights</code> và hai số nguyên <code>w1</code> và <code>w2</code> lần lượt biểu thị sức chứa <strong>tối đa</strong> của hai túi.</p>

<p>Mỗi vật có thể được đặt vào <strong>nhiều nhất</strong> một túi sao cho:</p>

<ul>
	<li>Túi 1 chứa tổng trọng lượng <strong>không vượt quá</strong> <code>w1</code>.</li>
	<li>Túi 2 chứa tổng trọng lượng <strong>không vượt quá</strong> <code>w2</code>.</li>
</ul>

<p>Trả về tổng trọng lượng <strong>lớn nhất</strong> có thể đặt vào hai túi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">weights = [1,4,3,2], w1 = 5, w2 = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">9</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Túi 1: Đặt <code>weights[2] = 3</code> và <code>weights[3] = 2</code> vì <code>3 + 2 = 5 &lt;= w1</code></li>
	<li>Túi 2: Đặt <code>weights[1] = 4</code> vì <code>4 &lt;= w2</code></li>
	<li>Tổng trọng lượng: <code>5 + 4 = 9</code></li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">weights = [3,6,4,8], w1 = 9, w2 = 7</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">15</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Túi 1: Đặt <code>weights[3] = 8</code> vì <code>8 &lt;= w1</code></li>
	<li>Túi 2: Đặt <code>weights[0] = 3</code> và <code>weights[2] = 4</code> vì <code>3 + 4 = 7 &lt;= w2</code></li>
	<li>Tổng trọng lượng: <code>8 + 7 = 15</code></li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">weights = [5,7], w1 = 2, w2 = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có trọng lượng nào vừa với túi, vì vậy đáp án là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= weights.length &lt;= 100</code></li>
	<li><code>1 &lt;= weights[i] &lt;= 100</code></li>
	<li><code>1 &lt;= w1, w2 &lt;= 300</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi vật có thể được đặt vào túi có sức chứa $w_1$, túi có sức chứa $w_2$, hoặc không đặt vào túi nào. Đây là bài toán ba lô $0$-$1$ hai chiều. Vì $n\le 100$ và $w\le 300$, ta có thể dùng mảng $f[j][k]$ đã nén.
>
> $f[j][k]$ là tổng trọng lượng lớn nhất khi sức chứa còn lại lần lượt là $j$ và $k$. Ta duyệt các sức chứa theo thứ tự giảm dần để mỗi vật chỉ được sử dụng nhiều nhất một lần.
>
> Với trọng lượng $x$, ta xét $f[j-x][k]+x$ và $f[j][k-x]+x$. Đáp án là $f[w_1][w_2]$.

<!-- thinking:end -->

Ta định nghĩa $f[i][j][k]$ biểu thị tổng trọng lượng lớn nhất khi đặt $i$ vật đầu tiên vào hai túi, trong đó túi 1 có sức chứa tối đa là $j$ và túi 2 có sức chứa tối đa là $k$. Ban đầu, $f[0][j][k] = 0$, nghĩa là chưa có vật nào được đặt vào các túi.

Công thức chuyển trạng thái là:

$$
f[i][j][k] = \max(f[i-1][j][k], f[i-1][j-w_i][k], f[i-1][j][k-w_i]) \quad (w_i \leq j \text{ or } w_i \leq k)
$$

trong đó $w_i$ là trọng lượng của vật thứ $i$.

Đáp án cuối cùng là $f[n][w1][w2]$, trong đó $n$ là số lượng vật.

Ta nhận thấy công thức chuyển trạng thái chỉ phụ thuộc vào trạng thái của lớp trước đó, nên có thể nén mảng quy hoạch động ba chiều thành mảng quy hoạch động hai chiều. Khi duyệt $j$ và $k$, ta duyệt theo thứ tự ngược.

Độ phức tạp thời gian là $O(n \times w1 \times w2)$, độ phức tạp không gian là $O(w1 \times w2)$. Trong đó $n$ là độ dài của mảng $\textit{weights}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxWeight(self, weights: List[int], w1: int, w2: int) -> int:
        f = [[0] * (w2 + 1) for _ in range(w1 + 1)]
        max = lambda a, b: a if a > b else b
        for x in weights:
            for j in range(w1, -1, -1):
                for k in range(w2, -1, -1):
                    if x <= j:
                        f[j][k] = max(f[j][k], f[j - x][k] + x)
                    if x <= k:
                        f[j][k] = max(f[j][k], f[j][k - x] + x)
        return f[w1][w2]
```

#### Java

```java
class Solution {
    public int maxWeight(int[] weights, int w1, int w2) {
        int[][] f = new int[w1 + 1][w2 + 1];
        for (int x : weights) {
            for (int j = w1; j >= 0; --j) {
                for (int k = w2; k >= 0; --k) {
                    if (x <= j) {
                        f[j][k] = Math.max(f[j][k], f[j - x][k] + x);
                    }
                    if (x <= k) {
                        f[j][k] = Math.max(f[j][k], f[j][k - x] + x);
                    }
                }
            }
        }
        return f[w1][w2];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxWeight(vector<int>& weights, int w1, int w2) {
        vector<vector<int>> f(w1 + 1, vector<int>(w2 + 1));
        for (int x : weights) {
            for (int j = w1; j >= 0; --j) {
                for (int k = w2; k >= 0; --k) {
                    if (x <= j) {
                        f[j][k] = max(f[j][k], f[j - x][k] + x);
                    }
                    if (x <= k) {
                        f[j][k] = max(f[j][k], f[j][k - x] + x);
                    }
                }
            }
        }
        return f[w1][w2];
    }
};
```

#### Go

```go
func maxWeight(weights []int, w1 int, w2 int) int {
	f := make([][]int, w1+1)
	for i := range f {
		f[i] = make([]int, w2+1)
	}
	for _, x := range weights {
		for j := w1; j >= 0; j-- {
			for k := w2; k >= 0; k-- {
				if x <= j {
					f[j][k] = max(f[j][k], f[j-x][k]+x)
				}
				if x <= k {
					f[j][k] = max(f[j][k], f[j][k-x]+x)
				}
			}
		}
	}
	return f[w1][w2]
}
```

#### TypeScript

```ts
function maxWeight(weights: number[], w1: number, w2: number): number {
    const f: number[][] = Array.from({ length: w1 + 1 }, () => Array(w2 + 1).fill(0));
    for (const x of weights) {
        for (let j = w1; j >= 0; j--) {
            for (let k = w2; k >= 0; k--) {
                if (x <= j) {
                    f[j][k] = Math.max(f[j][k], f[j - x][k] + x);
                }
                if (x <= k) {
                    f[j][k] = Math.max(f[j][k], f[j][k - x] + x);
                }
            }
        }
    }
    return f[w1][w2];
}
```

#### Rust

```rust
impl Solution {
    pub fn max_weight(weights: Vec<i32>, w1: i32, w2: i32) -> i32 {
        let w1 = w1 as usize;
        let w2 = w2 as usize;
        let mut f = vec![vec![0; w2 + 1]; w1 + 1];
        for &x in &weights {
            let x = x as usize;
            for j in (0..=w1).rev() {
                for k in (0..=w2).rev() {
                    if x <= j {
                        f[j][k] = f[j][k].max(f[j - x][k] + x as i32);
                    }
                    if x <= k {
                        f[j][k] = f[j][k].max(f[j][k - x] + x as i32);
                    }
                }
            }
        }
        f[w1][w2]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
