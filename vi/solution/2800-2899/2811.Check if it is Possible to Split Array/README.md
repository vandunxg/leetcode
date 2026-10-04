---
comments: true
difficulty: Medium
rating: 1543
source: Weekly Contest 357 Q2
tags:
    - Greedy
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [2811. Check if it is Possible to Split Array](https://leetcode.com/problems/check-if-it-is-possible-to-split-array)

[中文文档](/solution/2800-2899/2811.Check%20if%20it%20is%20Possible%20to%20Split%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <code>nums</code> có độ dài <code>n</code> và một số nguyên <code>m</code>. Hãy xác định xem có thể chia mảng thành <code>n</code> mảng có kích thước 1 bằng cách thực hiện một chuỗi thao tác hay không.</p>

<p>Một mảng được gọi là <strong>good</strong> nếu:</p>

<ul>
	<li>Độ dài mảng bằng <strong>một</strong>, hoặc</li>
	<li>Tổng các phần tử của mảng <strong>lớn hơn hoặc bằng</strong> <code>m</code>.</li>
</ul>

<p>Trong mỗi bước, bạn có thể chọn một mảng hiện có (có thể là kết quả của các bước trước đó) có độ dài <strong>ít nhất hai</strong> và chia nó thành <strong>hai </strong>mảng, nếu cả hai mảng kết quả đều good.</p>

<p>Trả về true nếu có thể chia mảng đã cho thành <code>n</code> mảng, ngược lại trả về false.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2, 2, 1], m = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chia <code>[2, 2, 1]</code> thành <code>[2, 2]</code> và <code>[1]</code>. Mảng <code>[1]</code> có độ dài bằng một, còn tổng các phần tử của mảng <code>[2, 2]</code> bằng <code>4 &gt;= m</code>, nên cả hai đều là mảng good.</li>
	<li>Chia <code>[2, 2]</code> thành <code>[2]</code> và <code>[2]</code>. Cả hai mảng đều có độ dài bằng một, nên đều là mảng good.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2, 1, 3], m = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<p>Thao tác đầu tiên phải là một trong hai thao tác sau:</p>

<ul>
	<li>Chia <code>[2, 1, 3]</code> thành <code>[2, 1]</code> và <code>[3]</code>. Mảng <code>[2, 1]</code> không có độ dài bằng một và cũng không có tổng các phần tử lớn hơn hoặc bằng <code>m</code>.</li>
	<li>Chia <code>[2, 1, 3]</code> thành <code>[2]</code> và <code>[1, 3]</code>. Mảng <code>[1, 3]</code> không có độ dài bằng một và cũng không có tổng các phần tử lớn hơn hoặc bằng <code>m</code>.</li>
</ul>

<p>Vì cả hai thao tác đều không hợp lệ (không chia mảng thành hai mảng good), nên không thể chia <code>nums</code> thành <code>n</code> mảng có kích thước 1.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2, 3, 3, 2, 3], m = 6</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><span class="example-io">Chia <code>[2, 3, 3, 2, 3]</code> thành <code>[2]</code> và <code>[3, 3, 2, 3]</code>.</span></li>
	<li><span class="example-io">Chia <code>[3, 3, 2, 3]</code> thành <code>[3, 3, 2]</code> và <code>[3]</code>.</span></li>
	<li><span class="example-io">Chia <code>[3, 3, 2]</code> thành <code>[3, 3]</code> và <code>[2]</code>.</span></li>
	<li><span class="example-io">Chia <code>[3, 3]</code> thành <code>[3]</code> và <code>[3]</code>.</span></li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
	<li><code>1 &lt;= m &lt;= 200</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lần chia yêu cầu cả hai phía phải có độ dài bằng $1$ hoặc có tổng ít nhất là $m$. Có $O(n^2)$ đoạn, nên có thể sử dụng memoization. Với tổng tiền tố, $dfs(i,j)$ thử mọi vị trí chia $k$, kiểm tra hai phía rồi đệ quy.

<!-- thinking:end -->

Trước hết, ta tiền xử lý để tạo mảng tổng tiền tố $s$, trong đó $s[i]$ biểu diễn tổng của $i$ phần tử đầu tiên trong mảng $nums$.

Tiếp theo, ta thiết kế hàm $dfs(i, j)$, biểu diễn việc có cách nào chia đoạn chỉ số $[i, j]$ của mảng $nums$ sao cho thỏa mãn các điều kiện hay không. Nếu có, trả về `true`, ngược lại trả về `false`.

Quá trình tính toán của hàm $dfs(i, j)$ như sau:

Nếu $i = j$, thì đoạn chỉ có một phần tử và không cần chia, trả về `true`;

Ngược lại, ta liệt kê vị trí chia $k$, với $k \in [i, j]$. Nếu thỏa mãn các điều kiện sau, đoạn có thể được chia thành hai mảng con $nums[i,.. k]$ và $nums[k + 1,.. j]$:

- Mảng con $nums[i,..k]$ chỉ có một phần tử, hoặc tổng các phần tử của mảng con $nums[i,..k]$ lớn hơn hoặc bằng $m$;
- Mảng con $nums[k + 1,..j]$ chỉ có một phần tử, hoặc tổng các phần tử của mảng con $nums[k + 1,..j]$ lớn hơn hoặc bằng $m$;
- Cả $dfs(i, k)$ và $dfs(k + 1, j)$ đều trả về `true`.

Để tránh tính toán lặp lại, ta sử dụng phương pháp tìm kiếm có ghi nhớ và dùng mảng hai chiều $f$ để lưu tất cả giá trị trả về của $dfs(i, j)$, trong đó $f[i][j]$ biểu diễn giá trị trả về của $dfs(i, j)$.

Độ phức tạp thời gian là $O(n^3)$, độ phức tạp không gian là $O(n^2)$, trong đó $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canSplitArray(self, nums: List[int], m: int) -> bool:
        @cache
        def dfs(i: int, j: int) -> bool:
            if i == j:
                return True
            for k in range(i, j):
                a = k == i or s[k + 1] - s[i] >= m
                b = k == j - 1 or s[j + 1] - s[k + 1] >= m
                if a and b and dfs(i, k) and dfs(k + 1, j):
                    return True
            return False

        s = list(accumulate(nums, initial=0))
        return dfs(0, len(nums) - 1)
```

#### Java

```java
class Solution {
    private Boolean[][] f;
    private int[] s;
    private int m;

    public boolean canSplitArray(List<Integer> nums, int m) {
        int n = nums.size();
        f = new Boolean[n][n];
        s = new int[n + 1];
        for (int i = 1; i <= n; ++i) {
            s[i] = s[i - 1] + nums.get(i - 1);
        }
        this.m = m;
        return dfs(0, n - 1);
    }

    private boolean dfs(int i, int j) {
        if (i == j) {
            return true;
        }
        if (f[i][j] != null) {
            return f[i][j];
        }
        for (int k = i; k < j; ++k) {
            boolean a = k == i || s[k + 1] - s[i] >= m;
            boolean b = k == j - 1 || s[j + 1] - s[k + 1] >= m;
            if (a && b && dfs(i, k) && dfs(k + 1, j)) {
                return f[i][j] = true;
            }
        }
        return f[i][j] = false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canSplitArray(vector<int>& nums, int m) {
        int n = nums.size();
        vector<int> s(n + 1);
        for (int i = 1; i <= n; ++i) {
            s[i] = s[i - 1] + nums[i - 1];
        }
        int f[n][n];
        memset(f, -1, sizeof f);
        function<bool(int, int)> dfs = [&](int i, int j) {
            if (i == j) {
                return true;
            }
            if (f[i][j] != -1) {
                return f[i][j] == 1;
            }
            for (int k = i; k < j; ++k) {
                bool a = k == i || s[k + 1] - s[i] >= m;
                bool b = k == j - 1 || s[j + 1] - s[k + 1] >= m;
                if (a && b && dfs(i, k) && dfs(k + 1, j)) {
                    f[i][j] = 1;
                    return true;
                }
            }
            f[i][j] = 0;
            return false;
        };
        return dfs(0, n - 1);
    }
};
```

#### Go

```go
func canSplitArray(nums []int, m int) bool {
	n := len(nums)
	f := make([][]int, n)
	s := make([]int, n+1)
	for i, x := range nums {
		s[i+1] = s[i] + x
	}
	for i := range f {
		f[i] = make([]int, n)
	}
	var dfs func(i, j int) bool
	dfs = func(i, j int) bool {
		if i == j {
			return true
		}
		if f[i][j] != 0 {
			return f[i][j] == 1
		}
		for k := i; k < j; k++ {
			a := k == i || s[k+1]-s[i] >= m
			b := k == j-1 || s[j+1]-s[k+1] >= m
			if a && b && dfs(i, k) && dfs(k+1, j) {
				f[i][j] = 1
				return true
			}
		}
		f[i][j] = -1
		return false
	}
	return dfs(0, n-1)
}
```

#### TypeScript

```ts
function canSplitArray(nums: number[], m: number): boolean {
    const n = nums.length;
    const s: number[] = new Array(n + 1).fill(0);
    for (let i = 1; i <= n; ++i) {
        s[i] = s[i - 1] + nums[i - 1];
    }
    const f: number[][] = Array(n)
        .fill(0)
        .map(() => Array(n).fill(-1));
    const dfs = (i: number, j: number): boolean => {
        if (i === j) {
            return true;
        }
        if (f[i][j] !== -1) {
            return f[i][j] === 1;
        }
        for (let k = i; k < j; ++k) {
            const a = k === i || s[k + 1] - s[i] >= m;
            const b = k === j - 1 || s[j + 1] - s[k + 1] >= m;
            if (a && b && dfs(i, k) && dfs(k + 1, j)) {
                f[i][j] = 1;
                return true;
            }
        }
        f[i][j] = 0;
        return false;
    };
    return dfs(0, n - 1);
}
```

#### Rust

```rust
impl Solution {
    pub fn can_split_array(nums: Vec<i32>, m: i32) -> bool {
        let n = nums.len();
        if n <= 2 {
            return true;
        }
        for i in 1..n {
            if nums[i - 1] + nums[i] >= m {
                return true;
            }
        }
        false
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Suy luận nhanh

<!-- thinking:start -->

> **Tư duy**
>
> Memoization vẫn phải liệt kê các vị trí chia. Vì các giá trị không âm, sau nhiều lần chia, cuối cùng sẽ còn lại một đoạn có độ dài $2$, và các đoạn dài hơn chỉ có tổng lớn hơn. Với $n>2$, điều kiện cần và đủ là tồn tại một cặp phần tử kề nhau có tổng ít nhất là $m$; nếu $n\le 2$ thì không cần chia.

<!-- thinking:end -->

Bất kể thực hiện thao tác như thế nào, cuối cùng sẽ luôn còn lại một mảng con có `length == 2`. Vì các phần tử không âm, khi thao tác chia tiếp diễn, độ dài và tổng của mảng con sẽ dần giảm. Tổng của các mảng con `length > 2` khác phải lớn hơn tổng của mảng con này. Do đó, ta chỉ cần xét xem có mảng con `length == 2` nào có tổng lớn hơn hoặc bằng `m` hay không.

> 📢 Lưu ý rằng khi `nums.length <= 2`, không cần thực hiện thao tác nào.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### TypeScript

```ts
function canSplitArray(nums: number[], m: number): boolean {
    const n = nums.length;
    if (n <= 2) {
        return true;
    }
    for (let i = 1; i < n; i++) {
        if (nums[i - 1] + nums[i] >= m) {
            return true;
        }
    }
    return false;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
