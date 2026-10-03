---
comments: true
difficulty: Easy
rating: 1372
source: Weekly Contest 241 Q1
tags:
    - Bit Manipulation
    - Array
    - Math
    - Backtracking
    - Combinatorics
    - Enumeration
---

<!-- problem:start -->

# [1863. Sum of All Subset XOR Totals](https://leetcode.com/problems/sum-of-all-subset-xor-totals)

[中文文档](/solution/1800-1899/1863.Sum%20of%20All%20Subset%20XOR%20Totals/README.md)

## Mô tả

<!-- description:start -->

<p><strong>XOR total</strong> của một mảng được định nghĩa là phép toán bitwise <code>XOR</code> của<strong> tất cả các phần tử</strong> trong mảng, hoặc bằng <code>0</code> nếu mảng<strong> rỗng</strong>.</p>

<ul>
	<li>Ví dụ, <strong>XOR total</strong> của mảng <code>[2,5,6]</code> là <code>2 XOR 5 XOR 6 = 1</code>.</li>
</ul>

<p>Cho một mảng <code>nums</code>, hãy trả về <em><strong>tổng</strong> của mọi <strong>XOR total</strong> của tất cả các <strong>tập con</strong> của </em><code>nums</code>.&nbsp;</p>

<p><strong>Lưu ý:</strong> Các tập con có <strong>cùng</strong> phần tử phải được tính <strong>nhiều lần</strong>.</p>

<p>Mảng <code>a</code> là một <strong>tập con</strong> của mảng <code>b</code> nếu có thể thu được <code>a</code> từ <code>b</code> bằng cách xóa một số phần tử (có thể là không xóa phần tử nào) của <code>b</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3]
<strong>Đầu ra:</strong> 6
<strong>Giải thích: </strong>4 tập con của [1,3] là:
- Tập con rỗng có XOR total bằng 0.
- [1] có XOR total bằng 1.
- [3] có XOR total bằng 3.
- [1,3] có XOR total bằng 1 XOR 3 = 2.
0 + 1 + 3 + 2 = 6
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,1,6]
<strong>Đầu ra:</strong> 28
<strong>Giải thích: </strong>8 tập con của [5,1,6] là:
- Tập con rỗng có XOR total bằng 0.
- [5] có XOR total bằng 5.
- [1] có XOR total bằng 1.
- [6] có XOR total bằng 6.
- [5,1] có XOR total bằng 5 XOR 1 = 4.
- [5,6] có XOR total bằng 5 XOR 6 = 3.
- [1,6] có XOR total bằng 1 XOR 6 = 7.
- [5,1,6] có XOR total bằng 5 XOR 1 XOR 6 = 2.
0 + 5 + 1 + 6 + 4 + 3 + 7 + 2 = 28
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,4,5,6,7,8]
<strong>Đầu ra:</strong> 480
<strong>Giải thích:</strong> Tổng XOR total của mọi tập con là 480.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 12</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 20</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tính tổng XOR total của mọi tập con. Vì $n\le 12$, chỉ có $2^n$ tập con nên có thể liệt kê tất cả.
>
> Với mỗi mask $i\in[0,2^n)$ biểu diễn một tập con, ta XOR các phần tử được chọn rồi cộng kết quả vào tổng. Không cần dùng đệ quy.

<!-- thinking:end -->

Ta có thể dùng liệt kê nhị phân để liệt kê mọi tập con, sau đó tính XOR sum của từng tập con.

Cụ thể, ta liệt kê $i$ trong đoạn $[0, 2^n)$, trong đó $n$ là độ dài của mảng $nums$. Nếu bit thứ $j$ trong biểu diễn nhị phân của $i$ bằng $1$, điều đó có nghĩa phần tử thứ $j$ của $nums$ thuộc tập con hiện tại; nếu bit thứ $j$ bằng $0$, phần tử thứ $j$ của $nums$ không thuộc tập con hiện tại. Dựa vào biểu diễn nhị phân của $i$, ta tính XOR sum của tập con hiện tại rồi cộng vào đáp án.

Độ phức tạp thời gian là $O(n \times 2^n)$, trong đó $n$ là độ dài của mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def subsetXORSum(self, nums: List[int]) -> int:
        ans, n = 0, len(nums)
        for i in range(1 << n):
            s = 0
            for j in range(n):
                if i >> j & 1:
                    s ^= nums[j]
            ans += s
        return ans
```

#### Java

```java
class Solution {
    public int subsetXORSum(int[] nums) {
        int n = nums.length;
        int ans = 0;
        for (int i = 0; i < 1 << n; ++i) {
            int s = 0;
            for (int j = 0; j < n; ++j) {
                if ((i >> j & 1) == 1) {
                    s ^= nums[j];
                }
            }
            ans += s;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int subsetXORSum(vector<int>& nums) {
        int n = nums.size();
        int ans = 0;
        for (int i = 0; i < 1 << n; ++i) {
            int s = 0;
            for (int j = 0; j < n; ++j) {
                if (i >> j & 1) {
                    s ^= nums[j];
                }
            }
            ans += s;
        }
        return ans;
    }
};
```

#### Go

```go
func subsetXORSum(nums []int) (ans int) {
	n := len(nums)
	for i := 0; i < 1<<n; i++ {
		s := 0
		for j, x := range nums {
			if i>>j&1 == 1 {
				s ^= x
			}
		}
		ans += s
	}
	return
}
```

#### TypeScript

```ts
function subsetXORSum(nums: number[]): number {
    let ans = 0;
    const n = nums.length;
    for (let i = 0; i < 1 << n; ++i) {
        let s = 0;
        for (let j = 0; j < n; ++j) {
            if ((i >> j) & 1) {
                s ^= nums[j];
            }
        }
        ans += s;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn subset_xor_sum(nums: Vec<i32>) -> i32 {
        let n = nums.len();
        let mut ans = 0;

        for i in 0..(1 << n) {
            let mut s = 0;
            for j in 0..n {
                if ((i >> j) & 1) == 1 {
                    s ^= nums[j];
                }
            }
            ans += s;
        }

        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var subsetXORSum = function (nums) {
    let ans = 0;
    const n = nums.length;
    for (let i = 0; i < 1 << n; ++i) {
        let s = 0;
        for (let j = 0; j < n; ++j) {
            if ((i >> j) & 1) {
                s ^= nums[j];
            }
        }
        ans += s;
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: DFS (Depth-First Search)

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 kiểm tra từng bit của một mask. DFS có thể thay thế bằng cách bỏ qua hoặc XOR thêm $nums[i]$, rồi cộng XOR đang có tại các node lá. Độ phức tạp không đổi, nhưng cách chọn hoặc không chọn trực tiếp hơn.

<!-- thinking:end -->

Ta cũng có thể dùng tìm kiếm theo chiều sâu để liệt kê mọi tập con, sau đó tính XOR sum của từng tập con.

Ta xây dựng hàm $dfs(i, s)$, trong đó $i$ biểu diễn việc đang xét đến phần tử thứ $i$ của mảng $nums$, còn $s$ biểu diễn XOR sum của tập con hiện tại. Ban đầu, $i=0$, $s=0$. Ở mỗi bước tìm kiếm, ta có hai lựa chọn:

- Thêm phần tử thứ $i$ của $nums$ vào tập con hiện tại, tức là $dfs(i+1, s \oplus nums[i])$;
- Không thêm phần tử thứ $i$ của $nums$ vào tập con hiện tại, tức là $dfs(i+1, s)$.

Khi đã xét hết các phần tử của mảng $nums$, tức là $i=n$, XOR sum của tập con hiện tại là $s$, và ta có thể cộng nó vào đáp án.

Độ phức tạp thời gian là $O(2^n)$, độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def subsetXORSum(self, nums: List[int]) -> int:
        def dfs(i: int, s: int):
            nonlocal ans
            if i >= len(nums):
                ans += s
                return
            dfs(i + 1, s)
            dfs(i + 1, s ^ nums[i])

        ans = 0
        dfs(0, 0)
        return ans
```

#### Java

```java
class Solution {
    private int ans;
    private int[] nums;

    public int subsetXORSum(int[] nums) {
        this.nums = nums;
        dfs(0, 0);
        return ans;
    }

    private void dfs(int i, int s) {
        if (i >= nums.length) {
            ans += s;
            return;
        }
        dfs(i + 1, s);
        dfs(i + 1, s ^ nums[i]);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int subsetXORSum(vector<int>& nums) {
        int n = nums.size();
        int ans = 0;
        auto dfs = [&](this auto&& dfs, int i, int s) {
            if (i >= n) {
                ans += s;
                return;
            }
            dfs(i + 1, s);
            dfs(i + 1, s ^ nums[i]);
        };
        dfs(0, 0);
        return ans;
    }
};
```

#### Go

```go
func subsetXORSum(nums []int) (ans int) {
	n := len(nums)
	var dfs func(int, int)
	dfs = func(i, s int) {
		if i >= n {
			ans += s
			return
		}
		dfs(i+1, s)
		dfs(i+1, s^nums[i])
	}
	dfs(0, 0)
	return
}
```

#### TypeScript

```ts
function subsetXORSum(nums: number[]): number {
    let ans = 0;
    const n = nums.length;
    const dfs = (i: number, s: number) => {
        if (i >= n) {
            ans += s;
            return;
        }
        dfs(i + 1, s);
        dfs(i + 1, s ^ nums[i]);
    };
    dfs(0, 0);
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn subset_xor_sum(nums: Vec<i32>) -> i32 {
        fn dfs(i: usize, s: i32, nums: &[i32], ans: &mut i32) {
            if i == nums.len() {
                *ans += s;
                return;
            }
            dfs(i + 1, s, nums, ans);
            dfs(i + 1, s ^ nums[i], nums, ans);
        }

        let mut ans = 0;
        dfs(0, 0, &nums, &mut ans);
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var subsetXORSum = function (nums) {
    let ans = 0;
    const n = nums.length;
    const dfs = (i, s) => {
        if (i >= n) {
            ans += s;
            return;
        }
        dfs(i + 1, s);
        dfs(i + 1, s ^ nums[i]);
    };
    dfs(0, 0);
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
