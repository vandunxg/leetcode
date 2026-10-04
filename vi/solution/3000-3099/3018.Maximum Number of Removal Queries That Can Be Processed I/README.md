---
comments: true
difficulty: Hard
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3018. Maximum Number of Removal Queries That Can Be Processed I 🔒](https://leetcode.com/problems/maximum-number-of-removal-queries-that-can-be-processed-i)

[中文文档](/solution/3000-3099/3018.Maximum%20Number%20of%20Removal%20Queries%20That%20Can%20Be%20Processed%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>nums</code> được đánh chỉ số từ <strong>0</strong> và một mảng <code>queries</code> được đánh chỉ số từ <strong>0</strong>.</p>

<p>Bạn có thể thực hiện thao tác sau ở đầu <strong>nhiều nhất một lần</strong>:</p>

<ul>
	<li>Thay <code>nums</code> bằng một <span data-keyword="subsequence-array">dãy con</span> của <code>nums</code>.</li>
</ul>

<p>Ta bắt đầu xử lý các truy vấn theo thứ tự đã cho; với mỗi truy vấn, ta thực hiện như sau:</p>

<ul>
	<li>Nếu phần tử đầu <strong>và</strong> phần tử cuối của <code>nums</code> đều <strong>nhỏ hơn</strong> <code>queries[i]</code>, quá trình xử lý truy vấn sẽ <strong>kết thúc</strong>.</li>
	<li>Nếu phần tử đầu <strong>hoặc</strong> phần tử cuối của <code>nums</code> <strong>lớn hơn hoặc bằng</strong> <code>queries[i]</code>, ta chọn một trong hai phần tử đó và <strong>xóa</strong> phần tử đã chọn khỏi <code>nums</code>.</li>
</ul>

<p>Hãy trả về <em><strong>số lượng lớn nhất</strong> truy vấn có thể xử lý bằng cách thực hiện thao tác một cách tối ưu.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,5], queries = [1,2,3,4,6]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Ta không thực hiện thao tác nào và xử lý các truy vấn như sau:
1- Ta chọn và xóa nums[0] vì 1 &lt;= 1, khi đó nums trở thành [2,3,4,5].
2- Ta chọn và xóa nums[0] vì 2 &lt;= 2, khi đó nums trở thành [3,4,5].
3- Ta chọn và xóa nums[0] vì 3 &lt;= 3, khi đó nums trở thành [4,5].
4- Ta chọn và xóa nums[0] vì 4 &lt;= 4, khi đó nums trở thành [5].
5- Ta không thể chọn phần tử nào từ nums vì chúng không lớn hơn hoặc bằng 5.
Vì vậy, đáp án là 4.
Có thể chứng minh rằng ta không thể xử lý nhiều hơn 4 truy vấn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,3,2], queries = [2,2,3]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta không thực hiện thao tác nào và xử lý các truy vấn như sau:
1- Ta chọn và xóa nums[0] vì 2 &lt;= 2, khi đó nums trở thành [3,2].
2- Ta chọn và xóa nums[1] vì 2 &lt;= 2, khi đó nums trở thành [3].
3- Ta chọn và xóa nums[0] vì 3 &lt;= 3, khi đó nums trở thành [].
Vì vậy, đáp án là 3.
Có thể chứng minh rằng ta không thể xử lý nhiều hơn 3 truy vấn.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,4,3], queries = [4,3,2]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Đầu tiên, ta thay nums bằng dãy con [4,3] của nums.
Sau đó, ta có thể xử lý các truy vấn như sau:
1- Ta chọn và xóa nums[0] vì 4 &lt;= 4, khi đó nums trở thành [3].
2- Ta chọn và xóa nums[0] vì 3 &lt;= 3, khi đó nums trở thành [].
3- Ta không thể xử lý thêm truy vấn nào vì nums đã rỗng.
Vì vậy, đáp án là 2.
Có thể chứng minh rằng ta không thể xử lý nhiều hơn 2 truy vấn.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= queries.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i], queries[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lần xóa lấy một phần tử ở đầu hoặc cuối mảng còn lại, và $n,m \le 1000$. Các lựa chọn greedy bên trái/phải không nhất thiết tối ưu.
>
> Phần còn lại luôn là một đoạn liên tiếp $[i,j]$, và số truy vấn đã xử lý là một hàm của đoạn đó. Có $O(n^2)$ trạng thái.
>
> $f[i][j]$ là số truy vấn đã xử lý khi $[i,j]$ vẫn còn. Giá trị này được xây dựng bằng cách xóa $i-1$ hoặc $j+1$; trường hợp còn lại một phần tử cuối cùng có thể xử lý thêm một truy vấn.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là số lượng truy vấn lớn nhất có thể xử lý khi các phần tử trong đoạn $[i, j]$ chưa bị xóa.

Xét $f[i][j]$:

- Nếu $i > 0$, giá trị của $f[i][j]$ có thể được suy ra từ $f[i - 1][j]$. Nếu $nums[i - 1] \ge queries[f[i - 1][j]]$, ta có thể chọn xóa $nums[i - 1]$. Do đó, ta có $f[i][j] = f[i - 1][j] + (nums[i - 1] \ge queries[f[i - 1][j]])$.
- Nếu $j + 1 < n$, giá trị của $f[i][j]$ có thể được suy ra từ $f[i][j + 1]$. Nếu $nums[j + 1] \ge queries[f[i][j + 1]]$, ta có thể chọn xóa $nums[j + 1]$. Do đó, ta có $f[i][j] = f[i][j + 1] + (nums[j + 1] \ge queries[f[i][j + 1]])$.
- Nếu $f[i][j] = m$, ta có thể trả về trực tiếp $m$.

Đáp án cuối cùng là $\max\limits_{0 \le i < n} f[i][i] + (nums[i] \ge queries[f[i][i]])$.

Độ phức tạp thời gian là $O(n^2)$, và độ phức tạp không gian là $O(n^2)$. Ở đây, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumProcessableQueries(self, nums: List[int], queries: List[int]) -> int:
        n = len(nums)
        f = [[0] * n for _ in range(n)]
        m = len(queries)
        for i in range(n):
            for j in range(n - 1, i - 1, -1):
                if i:
                    f[i][j] = max(
                        f[i][j], f[i - 1][j] + (nums[i - 1] >= queries[f[i - 1][j]])
                    )
                if j + 1 < n:
                    f[i][j] = max(
                        f[i][j], f[i][j + 1] + (nums[j + 1] >= queries[f[i][j + 1]])
                    )
                if f[i][j] == m:
                    return m
        return max(f[i][i] + (nums[i] >= queries[f[i][i]]) for i in range(n))
```

#### Java

```java
class Solution {
    public int maximumProcessableQueries(int[] nums, int[] queries) {
        int n = nums.length;
        int[][] f = new int[n][n];
        int m = queries.length;
        for (int i = 0; i < n; ++i) {
            for (int j = n - 1; j >= i; --j) {
                if (i > 0) {
                    f[i][j] = Math.max(
                        f[i][j], f[i - 1][j] + (nums[i - 1] >= queries[f[i - 1][j]] ? 1 : 0));
                }
                if (j + 1 < n) {
                    f[i][j] = Math.max(
                        f[i][j], f[i][j + 1] + (nums[j + 1] >= queries[f[i][j + 1]] ? 1 : 0));
                }
                if (f[i][j] == m) {
                    return m;
                }
            }
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            ans = Math.max(ans, f[i][i] + (nums[i] >= queries[f[i][i]] ? 1 : 0));
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumProcessableQueries(vector<int>& nums, vector<int>& queries) {
        int n = nums.size();
        int f[n][n];
        memset(f, 0, sizeof(f));
        int m = queries.size();
        for (int i = 0; i < n; ++i) {
            for (int j = n - 1; j >= i; --j) {
                if (i > 0) {
                    f[i][j] = max(f[i][j], f[i - 1][j] + (nums[i - 1] >= queries[f[i - 1][j]] ? 1 : 0));
                }
                if (j + 1 < n) {
                    f[i][j] = max(f[i][j], f[i][j + 1] + (nums[j + 1] >= queries[f[i][j + 1]] ? 1 : 0));
                }
                if (f[i][j] == m) {
                    return m;
                }
            }
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            ans = max(ans, f[i][i] + (nums[i] >= queries[f[i][i]] ? 1 : 0));
        }
        return ans;
    }
};
```

#### Go

```go
func maximumProcessableQueries(nums []int, queries []int) (ans int) {
	n := len(nums)
	f := make([][]int, n)
	for i := range f {
		f[i] = make([]int, n)
	}
	m := len(queries)
	for i := 0; i < n; i++ {
		for j := n - 1; j >= i; j-- {
			if i > 0 {
				t := 0
				if nums[i-1] >= queries[f[i-1][j]] {
					t = 1
				}
				f[i][j] = max(f[i][j], f[i-1][j]+t)
			}
			if j+1 < n {
				t := 0
				if nums[j+1] >= queries[f[i][j+1]] {
					t = 1
				}
				f[i][j] = max(f[i][j], f[i][j+1]+t)
			}
			if f[i][j] == m {
				return m
			}
		}
	}
	for i := 0; i < n; i++ {
		t := 0
		if nums[i] >= queries[f[i][i]] {
			t = 1
		}
		ans = max(ans, f[i][i]+t)
	}
	return
}
```

#### TypeScript

```ts
function maximumProcessableQueries(nums: number[], queries: number[]): number {
    const n = nums.length;
    const f: number[][] = Array.from({ length: n }, () => Array.from({ length: n }, () => 0));
    const m = queries.length;
    for (let i = 0; i < n; ++i) {
        for (let j = n - 1; j >= i; --j) {
            if (i > 0) {
                f[i][j] = Math.max(
                    f[i][j],
                    f[i - 1][j] + (nums[i - 1] >= queries[f[i - 1][j]] ? 1 : 0),
                );
            }
            if (j + 1 < n) {
                f[i][j] = Math.max(
                    f[i][j],
                    f[i][j + 1] + (nums[j + 1] >= queries[f[i][j + 1]] ? 1 : 0),
                );
            }
            if (f[i][j] == m) {
                return m;
            }
        }
    }
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        ans = Math.max(ans, f[i][i] + (nums[i] >= queries[f[i][i]] ? 1 : 0));
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn maximum_processable_queries(nums: Vec<i32>, queries: Vec<i32>) -> i32 {
        let n = nums.len();
        let m = queries.len();
        let mut f = vec![vec![0; n]; n];

        for i in 0..n {
            for j in (i..n).rev() {
                if i > 0 {
                    let idx = f[i - 1][j] as usize;
                    if idx < m {
                        f[i][j] = f[i][j].max(f[i - 1][j] + if nums[i - 1] >= queries[idx] { 1 } else { 0 });
                    }
                }
                if j + 1 < n {
                    let idx = f[i][j + 1] as usize;
                    if idx < m {
                        f[i][j] = f[i][j].max(f[i][j + 1] + if nums[j + 1] >= queries[idx] { 1 } else { 0 });
                    }
                }
                if f[i][j] as usize == m {
                    return m as i32;
                }
            }
        }

        let mut ans = 0;
        for i in 0..n {
            let idx = f[i][i] as usize;
            if idx < m {
                ans = ans.max(f[i][i] + if nums[i] >= queries[idx] { 1 } else { 0 });
            } else {
                ans = ans.max(f[i][i]);
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
