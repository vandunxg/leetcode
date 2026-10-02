---
comments: true
difficulty: Hard
rating: 2288
source: Weekly Contest 204 Q4
tags:
    - Tree
    - Union Find
    - Binary Search Tree
    - Memoization
    - Array
    - Math
    - Divide and Conquer
    - Dynamic Programming
    - Binary Tree
    - Combinatorics
    - Fermat's Little Theorem
---

<!-- problem:start -->

# [1569. Number of Ways to Reorder Array to Get Same BST](https://leetcode.com/problems/number-of-ways-to-reorder-array-to-get-same-bst)

[中文文档](/solution/1500-1599/1569.Number%20of%20Ways%20to%20Reorder%20Array%20to%20Get%20Same%20BST/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>nums</code> biểu diễn một hoán vị các số nguyên từ <code>1</code> đến <code>n</code>. Ta xây dựng cây tìm kiếm nhị phân (BST) bằng cách lần lượt chèn các phần tử của <code>nums</code> vào một BST ban đầu rỗng. Hãy tìm số cách khác nhau để sắp xếp lại <code>nums</code> sao cho BST tạo được giống hệt BST từ mảng <code>nums</code> ban đầu.</p>

<ul>
	<li>Ví dụ, với <code>nums = [2,1,3]</code>, ta có 2 làm root, 1 là con trái và 3 là con phải. Mảng <code>[2,3,1]</code> cũng tạo ra BST tương tự, nhưng <code>[3,2,1]</code> tạo ra BST khác.</li>
</ul>

<p>Trả về <em>số cách sắp xếp lại</em> <code>nums</code> <em>để BST tạo thành giống hệt BST ban đầu từ</em> <code>nums</code>.</p>

<p>Vì đáp án có thể rất lớn, <strong>trả về phần dư khi chia cho </strong><code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1569.Number%20of%20Ways%20to%20Reorder%20Array%20to%20Get%20Same%20BST/images/1569_ex0.png" style="width: 160px; height: 160px;" />
<pre>
<strong>Input:</strong> nums = [2,1,3]
<strong>Output:</strong> 1
<strong>Giải thích:</strong> Có thể sắp xếp nums thành [2,3,1] để tạo ra BST tương tự. Không có cách sắp xếp nào khác tạo ra BST tương tự.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1569.Number%20of%20Ways%20to%20Reorder%20Array%20to%20Get%20Same%20BST/images/1569_ex.png" style="width: 270px; height: 270px;" />
<pre>
<strong>Input:</strong> nums = [3,4,5,1,2]
<strong>Output:</strong> 5
<strong>Giải thích:</strong> 5 mảng sau sẽ tạo ra BST tương tự: 
[3,1,2,4,5]
[3,1,4,2,5]
[3,1,4,5,2]
[3,4,1,2,5]
[3,4,1,5,2]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1569.Number%20of%20Ways%20to%20Reorder%20Array%20to%20Get%20Same%20BST/images/1569_ex2.png" style="width: 205px; height: 205px;" />
<pre>
<strong>Input:</strong> nums = [1,2,3]
<strong>Output:</strong> 0
<strong>Giải thích:</strong> Không có thứ tự nào khác của nums tạo ra BST tương tự.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= nums.length</code></li>
	<li>All integers in <code>nums</code> are <strong>distinct</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm tổ hợp + Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các cách sắp xếp lại tạo ra BST giống $nums$, không tính thứ tự ban đầu. Vì $n\le 1000$, không thể liệt kê mọi hoán vị. Root phải là giá trị đầu tiên; các tập con trái và phải được xác định bởi phép so sánh, còn thứ tự bên trong được xử lý đệ quy.
>
> Hai phía có thể xen kẽ tự do, đóng góp $C_{m+n}^{m}$ lần tích số cách của mỗi phía. Sau khi tính trước các hệ số nhị thức, ta đệ quy rồi trừ một để loại mảng ban đầu.

<!-- thinking:end -->

Ta thiết kế hàm $dfs(nums)$ để tính số cách của cây tìm kiếm nhị phân có các node là $nums$. Đáp án là $dfs(nums)-1$, vì $dfs(nums)$ tính cả cách sắp xếp ban đầu, trong khi đề bài chỉ yêu cầu các cách sau khi sắp xếp lại, nên cần trừ đi một.

Tiếp theo, hãy xem cách tính $dfs(nums)$.

Với mảng $nums$, phần tử đầu tiên là root, nên các phần tử của cây con trái nhỏ hơn root và các phần tử của cây con phải lớn hơn root. Ta chia mảng thành ba phần: root, các phần tử cây con trái ký hiệu là $left$, và các phần tử cây con phải ký hiệu là $right$. Nếu cây con trái có $m$ phần tử và cây con phải có $n$ phần tử, số cách tương ứng là $dfs(left)$ và $dfs(right)$. Chọn $m$ vị trí trong $m + n$ vị trí của mảng $nums$ để đặt các phần tử cây con trái, các vị trí còn lại dành cho cây con phải, nhờ đó BST sau khi sắp xếp lại giống BST ban đầu. Vì vậy, ta tính $dfs(nums)$ dựa trên $nums$ và $left$. Các phần tử còn lại của $nums$ tạo thành $right$ với $n$ phần tử; toàn bộ quá trình vẫn dựa trên $nums$:

$$
dfs(nums) = C_{m+n}^m \times dfs(left) \times dfs(right)
$$

where $C_{m+n}^m$ represents the number of schemes to select $m$ positions from $m + n$ positions, which we can get through preprocessing.

Lưu ý phép modulo của đáp án: giá trị $dfs(nums)$ có thể rất lớn, nên cần lấy modulo ở mỗi bước tính toán và cuối cùng lấy modulo của toàn bộ kết quả.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là độ dài mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numOfWays(self, nums: List[int]) -> int:
        def dfs(nums):
            if len(nums) < 2:
                return 1
            left = [x for x in nums if x < nums[0]]
            right = [x for x in nums if x > nums[0]]
            m, n = len(left), len(right)
            a, b = dfs(left), dfs(right)
            return (((c[m + n][m] * a) % mod) * b) % mod

        n = len(nums)
        mod = 10**9 + 7
        c = [[0] * n for _ in range(n)]
        c[0][0] = 1
        for i in range(1, n):
            c[i][0] = 1
            for j in range(1, i + 1):
                c[i][j] = (c[i - 1][j] + c[i - 1][j - 1]) % mod
        return (dfs(nums) - 1 + mod) % mod
```

#### Java

```java
class Solution {
    private int[][] c;
    private final int mod = (int) 1e9 + 7;

    public int numOfWays(int[] nums) {
        int n = nums.length;
        c = new int[n][n];
        c[0][0] = 1;
        for (int i = 1; i < n; ++i) {
            c[i][0] = 1;
            for (int j = 1; j <= i; ++j) {
                c[i][j] = (c[i - 1][j] + c[i - 1][j - 1]) % mod;
            }
        }
        List<Integer> list = new ArrayList<>();
        for (int x : nums) {
            list.add(x);
        }
        return (dfs(list) - 1 + mod) % mod;
    }

    private int dfs(List<Integer> nums) {
        if (nums.size() < 2) {
            return 1;
        }
        List<Integer> left = new ArrayList<>();
        List<Integer> right = new ArrayList<>();
        for (int x : nums) {
            if (x < nums.get(0)) {
                left.add(x);
            } else if (x > nums.get(0)) {
                right.add(x);
            }
        }
        int m = left.size(), n = right.size();
        int a = dfs(left), b = dfs(right);
        return (int) ((long) a * b % mod * c[m + n][n] % mod);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numOfWays(vector<int>& nums) {
        int n = nums.size();
        const int mod = 1e9 + 7;
        int c[n][n];
        memset(c, 0, sizeof(c));
        c[0][0] = 1;
        for (int i = 1; i < n; ++i) {
            c[i][0] = 1;
            for (int j = 1; j <= i; ++j) {
                c[i][j] = (c[i - 1][j] + c[i - 1][j - 1]) % mod;
            }
        }
        function<int(vector<int>)> dfs = [&](vector<int> nums) -> int {
            if (nums.size() < 2) {
                return 1;
            }
            vector<int> left, right;
            for (int& x : nums) {
                if (x < nums[0]) {
                    left.push_back(x);
                } else if (x > nums[0]) {
                    right.push_back(x);
                }
            }
            int m = left.size(), n = right.size();
            int a = dfs(left), b = dfs(right);
            return c[m + n][m] * 1ll * a % mod * b % mod;
        };
        return (dfs(nums) - 1 + mod) % mod;
    }
};
```

#### Go

```go
func numOfWays(nums []int) int {
	n := len(nums)
	const mod = 1e9 + 7
	c := make([][]int, n)
	for i := range c {
		c[i] = make([]int, n)
	}
	c[0][0] = 1
	for i := 1; i < n; i++ {
		c[i][0] = 1
		for j := 1; j <= i; j++ {
			c[i][j] = (c[i-1][j] + c[i-1][j-1]) % mod
		}
	}
	var dfs func(nums []int) int
	dfs = func(nums []int) int {
		if len(nums) < 2 {
			return 1
		}
		var left, right []int
		for _, x := range nums[1:] {
			if x < nums[0] {
				left = append(left, x)
			} else {
				right = append(right, x)
			}
		}
		m, n := len(left), len(right)
		a, b := dfs(left), dfs(right)
		return c[m+n][m] * a % mod * b % mod
	}
	return (dfs(nums) - 1 + mod) % mod
}
```

#### TypeScript

```ts
function numOfWays(nums: number[]): number {
    const n = nums.length;
    const mod = 1e9 + 7;
    const c = new Array(n).fill(0).map(() => new Array(n).fill(0));
    c[0][0] = 1;
    for (let i = 1; i < n; ++i) {
        c[i][0] = 1;
        for (let j = 1; j <= i; ++j) {
            c[i][j] = (c[i - 1][j - 1] + c[i - 1][j]) % mod;
        }
    }
    const dfs = (nums: number[]): number => {
        if (nums.length < 2) {
            return 1;
        }
        const left: number[] = [];
        const right: number[] = [];
        for (let i = 1; i < nums.length; ++i) {
            if (nums[i] < nums[0]) {
                left.push(nums[i]);
            } else {
                right.push(nums[i]);
            }
        }
        const m = left.length;
        const n = right.length;
        const a = dfs(left);
        const b = dfs(right);
        return Number((BigInt(c[m + n][m]) * BigInt(a) * BigInt(b)) % BigInt(mod));
    };
    return (dfs(nums) - 1 + mod) % mod;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
