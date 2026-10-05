---
comments: true
difficulty: Medium
rating: 1618
source: Biweekly Contest 176 Q3
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3840. House Robber V](https://leetcode.com/problems/house-robber-v)

[中文文档](/solution/3800-3899/3840.House%20Robber%20V/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn là một tên trộm chuyên nghiệp đang lên kế hoạch cướp các ngôi nhà dọc theo một con phố. Mỗi ngôi nhà có một số tiền nhất định được cất giữ và được bảo vệ bởi một hệ thống an ninh có mã màu.</p>

<p>Bạn được cho hai mảng số nguyên <code>nums</code> và <code>colors</code>, cả hai đều có độ dài <code>n</code>, trong đó <code>nums[i]</code> là số tiền trong ngôi nhà thứ <code>i<sup>th</sup></code> và <code>colors[i]</code> là mã màu của ngôi nhà đó.</p>

<p>Bạn <strong>không thể cướp hai</strong> ngôi nhà liền kề nếu chúng có <strong>cùng màu</strong>.</p>

<p>Trả về số tiền <strong>lớn nhất</strong> mà bạn có thể cướp.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,4,3,5], colors = [1,1,2,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">9</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn các ngôi nhà <code>i = 1</code> có <code>nums[1] = 4</code> và <code>i = 3</code> có <code>nums[3] = 5</code> vì chúng không liền kề.</li>
	<li>Do đó, tổng số tiền bị cướp là <code>4 + 5 = 9</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,1,2,4], colors = [2,3,2,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn các ngôi nhà <code>i = 0</code> có <code>nums[0] = 3</code>, <code>i = 1</code> có <code>nums[1] = 1</code> và <code>i = 3</code> có <code>nums[3] = 4</code>.</li>
	<li>Lựa chọn này hợp lệ vì các ngôi nhà <code>i = 0</code> và <code>i = 1</code> có màu khác nhau, còn ngôi nhà <code>i = 3</code> không liền kề với <code>i = 1</code>.</li>
	<li>Do đó, tổng số tiền bị cướp là <code>3 + 1 + 4 = 8</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [10,1,3,9], colors = [1,1,1,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">22</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn các ngôi nhà <code>i = 0</code> có <code>nums[0] = 10</code>, <code>i = 2</code> có <code>nums[2] = 3</code> và <code>i = 3</code> có <code>nums[3] = 9</code>.</li>
	<li>Lựa chọn này hợp lệ vì các ngôi nhà <code>i = 0</code> và <code>i = 2</code> không liền kề, còn các ngôi nhà <code>i = 2</code> và <code>i = 3</code> có màu khác nhau.</li>
	<li>Do đó, tổng số tiền bị cướp là <code>10 + 3 + 9 = 22</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length == colors.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i], colors[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Không thể cướp đồng thời hai ngôi nhà liền kề có cùng màu; các màu khác nhau thì không bị hạn chế như vậy. Vì $n \le 10^5$, cần dùng DP tuyến tính.
>
> Với hai ngôi nhà cùng màu, chuyển trạng thái cướp hoặc bỏ qua thông thường vẫn được giữ nguyên; với hai màu khác nhau, ngôi nhà hiện tại có thể được chọn ngay sau một ngôi nhà đã bị cướp.
>
> Gọi $f,g$ lần lượt là số tiền tốt nhất khi bỏ qua hoặc cướp ngôi nhà trước đó. Nếu cùng màu, $g$ chỉ có thể lấy từ $f$ cũ. Nếu khác màu, $g$ có thể lấy từ $\max(f,g)$.
>
> Chỉ cần hai biến cuộn; đáp án là giá trị lớn hơn trong hai biến.

<!-- thinking:end -->

Ta định nghĩa hai biến $f$ và $g$, trong đó $f$ biểu thị số tiền lớn nhất khi không cướp ngôi nhà hiện tại, còn $g$ biểu thị số tiền lớn nhất khi cướp ngôi nhà hiện tại. Ban đầu, $f = 0$ và $g = nums[0]$. Đáp án là $\max(f, g)$.

Tiếp theo, ta duyệt từ ngôi nhà thứ hai:

- Nếu ngôi nhà hiện tại có cùng màu với ngôi nhà trước đó, thì cập nhật $f$ thành $\max(f, g)$ và $g$ thành $f + nums[i]$.
- Nếu ngôi nhà hiện tại có màu khác với ngôi nhà trước đó, thì cập nhật $f$ thành $\max(f, g)$ và $g$ thành $\max(f, g) + nums[i]$.

Cuối cùng, trả về $\max(f, g)$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số ngôi nhà. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def rob(self, nums: List[int], colors: List[int]) -> int:
        n = len(nums)
        f, g = 0, nums[0]
        for i in range(1, n):
            if colors[i - 1] == colors[i]:
                f, g = max(f, g), f + nums[i]
            else:
                f, g = max(f, g), max(f, g) + nums[i]
        return max(f, g)
```

#### Java

```java
class Solution {
    public long rob(int[] nums, int[] colors) {
        int n = nums.length;
        long f = 0, g = nums[0];
        for (int i = 1; i < n; i++) {
            if (colors[i - 1] == colors[i]) {
                long gg = f + nums[i];
                f = Math.max(f, g);
                g = gg;
            } else {
                long gg = Math.max(f, g) + nums[i];
                f = Math.max(f, g);
                g = gg;
            }
        }
        return Math.max(f, g);
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long rob(vector<int>& nums, vector<int>& colors) {
        int n = nums.size();
        long long f = 0, g = nums[0];
        for (int i = 1; i < n; i++) {
            if (colors[i - 1] == colors[i]) {
                long long gg = f + nums[i];
                f = max(f, g);
                g = gg;
            } else {
                long long gg = max(f, g) + nums[i];
                f = max(f, g);
                g = gg;
            }
        }
        return max(f, g);
    }
};
```

#### Go

```go
func rob(nums []int, colors []int) int64 {
	n := len(nums)
	var f int64 = 0
	var g int64 = int64(nums[0])

	for i := 1; i < n; i++ {
		if colors[i-1] == colors[i] {
			f, g = max(f, g), f+int64(nums[i])
		} else {
			f, g = max(f, g), max(f, g)+int64(nums[i])
		}
	}

	return max(f, g)
}
```

#### TypeScript

```ts
function rob(nums: number[], colors: number[]): number {
    const n = nums.length;
    let f = 0;
    let g = nums[0];

    for (let i = 1; i < n; i++) {
        if (colors[i - 1] === colors[i]) {
            [f, g] = [Math.max(f, g), f + nums[i]];
        } else {
            [f, g] = [Math.max(f, g), Math.max(f, g) + nums[i]];
        }
    }

    return Math.max(f, g);
}
```

#### Rust

```rust
impl Solution {
    pub fn rob(nums: Vec<i32>, colors: Vec<i32>) -> i64 {
        let n = nums.len();
        let mut f: i64 = 0;
        let mut g: i64 = nums[0] as i64;

        for i in 1..n {
            if colors[i - 1] == colors[i] {
                let gg = f + nums[i] as i64;
                f = f.max(g);
                g = gg;
            } else {
                let gg = f.max(g) + nums[i] as i64;
                f = f.max(g);
                g = gg;
            }
        }

        f.max(g)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
