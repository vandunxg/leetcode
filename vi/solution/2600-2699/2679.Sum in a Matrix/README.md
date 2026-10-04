---
comments: true
difficulty: Medium
rating: 1333
source: Biweekly Contest 104 Q2
tags:
    - Array
    - Matrix
    - Sorting
    - Simulation
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2679. Sum in a Matrix](https://leetcode.com/problems/sum-in-a-matrix)

[中文文档](/solution/2600-2699/2679.Sum%20in%20a%20Matrix/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên 2D <code>nums</code> được đánh chỉ số từ <strong>0</strong>. Ban đầu, điểm số của bạn là <code>0</code>. Thực hiện các thao tác sau cho đến khi ma trận trở thành rỗng:</p>

<ol>
	<li>Từ mỗi hàng của ma trận, chọn số lớn nhất và xóa nó. Nếu có nhiều số bằng nhau thì chọn số nào cũng được.</li>
	<li>Tìm số lớn nhất trong tất cả các số đã xóa ở bước 1. Cộng số đó vào <strong>điểm số</strong>.</li>
</ol>

<p>Trả về <em><strong>điểm số</strong> cuối cùng.</em></p>
<p>&nbsp;</p>
<p><strong>Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [[7,2,1],[6,4,2],[6,5,3],[3,2,1]]
<strong>Đầu ra:</strong> 15
<strong>Giải thích:</strong> Trong thao tác đầu tiên, ta xóa 7, 6, 6 và 3. Sau đó, ta cộng 7 vào điểm số. Tiếp theo, ta xóa 2, 4, 5 và 2. Ta cộng 5 vào điểm số. Cuối cùng, ta xóa 1, 2, 3 và 1. Ta cộng 3 vào điểm số. Do đó, điểm số cuối cùng là 7 + 5 + 3 = 15.
</pre>

<p><strong>Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [[1]]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Ta xóa 1 và cộng nó vào đáp án. Ta trả về 1.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 300</code></li>
	<li><code>1 &lt;= nums[i].length &lt;= 500</code></li>
	<li><code>0 &lt;= nums[i][j] &lt;= 10<sup>3</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi vòng xóa một phần tử lớn nhất hiện tại của từng hàng rồi cộng số lớn nhất trong các phần tử đó. Việc quét tuyến tính lặp lại là không cần thiết. Sắp xếp từng hàng sẽ đưa các phần tử lớn thứ $k$ của các hàng vào cùng một cột; tổng các giá trị lớn nhất của mỗi cột chính là điểm số.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def matrixSum(self, nums: List[List[int]]) -> int:
        for row in nums:
            row.sort()
        return sum(map(max, zip(*nums)))
```

#### Java

```java
class Solution {
    public int matrixSum(int[][] nums) {
        for (var row : nums) {
            Arrays.sort(row);
        }
        int ans = 0;
        for (int j = 0; j < nums[0].length; ++j) {
            int mx = 0;
            for (var row : nums) {
                mx = Math.max(mx, row[j]);
            }
            ans += mx;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int matrixSum(vector<vector<int>>& nums) {
        for (auto& row : nums) {
            sort(row.begin(), row.end());
        }
        int ans = 0;
        for (int j = 0; j < nums[0].size(); ++j) {
            int mx = 0;
            for (auto& row : nums) {
                mx = max(mx, row[j]);
            }
            ans += mx;
        }
        return ans;
    }
};
```

#### Go

```go
func matrixSum(nums [][]int) (ans int) {
	for _, row := range nums {
		sort.Ints(row)
	}
	for i := 0; i < len(nums[0]); i++ {
		mx := 0
		for _, row := range nums {
			mx = max(mx, row[i])
		}
		ans += mx
	}
	return
}
```

#### TypeScript

```ts
function matrixSum(nums: number[][]): number {
    for (const row of nums) {
        row.sort((a, b) => a - b);
    }
    let ans = 0;
    for (let j = 0; j < nums[0].length; ++j) {
        let mx = 0;
        for (const row of nums) {
            mx = Math.max(mx, row[j]);
        }
        ans += mx;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn matrix_sum(mut nums: Vec<Vec<i32>>) -> i32 {
        for row in &mut nums {
            row.sort();
        }
        (0..nums[0].len())
            .map(|col| nums.iter().map(|row| row[col]).max().unwrap())
            .sum()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
