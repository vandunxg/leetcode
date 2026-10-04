---
comments: true
difficulty: Medium
rating: 1721
source: Biweekly Contest 111 Q3
tags:
    - Array
    - Binary Search
    - Dynamic Programming
---

<!-- problem:start -->

# [2826. Sorting Three Groups](https://leetcode.com/problems/sorting-three-groups)

[中文文档](/solution/2800-2899/2826.Sorting%20Three%20Groups/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>. Mỗi phần tử trong <code>nums</code> là 1, 2 hoặc 3. Trong mỗi thao tác, bạn có thể xóa một phần tử khỏi <code>nums</code>. Hãy trả về số <strong>thao tác tối thiểu</strong> để <code>nums</code> trở thành <strong>không giảm</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,1,3,2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một cách tối ưu là xóa <code>nums[0]</code>, <code>nums[2]</code> và <code>nums[3]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,3,2,1,3,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một cách tối ưu là xóa <code>nums[1]</code> và <code>nums[2]</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,2,2,2,3,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>nums</code> đã là dãy không giảm.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 3</code></li>
</ul>

<p>&nbsp;</p>
<strong>Câu hỏi mở rộng:</strong> Bạn có thể tìm ra thuật toán có độ phức tạp thời gian <code>O(n)</code> không?

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Một mảng đẹp là mảng không giảm trên $\{1,2,3\}$. Khi gán giá trị cuối cùng cho từng vị trí, ta phải giữ dãy không giảm. $f[j]$ là chi phí nhỏ nhất để tiền tố hiện tại kết thúc bằng $j+1$, chuyển từ một trạng thái trước đó có chỉ số nhiều nhất $j$; chỉ cần giữ lại ba trạng thái.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là số thao tác ít nhất để biến đổi $i$ phần tử đầu tiên thành một mảng đẹp, trong đó phần tử thứ $i$ được đổi thành $j+1$. Đáp án là $\min(f[n][0], f[n][1], f[n][2])$.

Ta có thể liệt kê mọi trường hợp phần tử thứ $i$ được đổi thành $j+1$, sau đó lấy giá trị nhỏ nhất. Ở đây, ta có thể sử dụng mảng cuộn để tối ưu độ phức tạp không gian.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumOperations(self, nums: List[int]) -> int:
        f = [0] * 3
        for x in nums:
            g = [0] * 3
            if x == 1:
                g[0] = f[0]
                g[1] = min(f[:2]) + 1
                g[2] = min(f) + 1
            elif x == 2:
                g[0] = f[0] + 1
                g[1] = min(f[:2])
                g[2] = min(f) + 1
            else:
                g[0] = f[0] + 1
                g[1] = min(f[:2]) + 1
                g[2] = min(f)
            f = g
        return min(f)
```

#### Java

```java
class Solution {
    public int minimumOperations(List<Integer> nums) {
        int[] f = new int[3];
        for (int x : nums) {
            int[] g = new int[3];
            if (x == 1) {
                g[0] = f[0];
                g[1] = Math.min(f[0], f[1]) + 1;
                g[2] = Math.min(f[0], Math.min(f[1], f[2])) + 1;
            } else if (x == 2) {
                g[0] = f[0] + 1;
                g[1] = Math.min(f[0], f[1]);
                g[2] = Math.min(f[0], Math.min(f[1], f[2])) + 1;
            } else {
                g[0] = f[0] + 1;
                g[1] = Math.min(f[0], f[1]) + 1;
                g[2] = Math.min(f[0], Math.min(f[1], f[2]));
            }
            f = g;
        }
        return Math.min(f[0], Math.min(f[1], f[2]));
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumOperations(vector<int>& nums) {
        vector<int> f(3);
        for (int x : nums) {
            vector<int> g(3);
            if (x == 1) {
                g[0] = f[0];
                g[1] = min(f[0], f[1]) + 1;
                g[2] = min({f[0], f[1], f[2]}) + 1;
            } else if (x == 2) {
                g[0] = f[0] + 1;
                g[1] = min(f[0], f[1]);
                g[2] = min(f[0], min(f[1], f[2])) + 1;
            } else {
                g[0] = f[0] + 1;
                g[1] = min(f[0], f[1]) + 1;
                g[2] = min(f[0], min(f[1], f[2]));
            }
            f = move(g);
        }
        return min({f[0], f[1], f[2]});
    }
};
```

#### Go

```go
func minimumOperations(nums []int) int {
	f := make([]int, 3)
	for _, x := range nums {
		g := make([]int, 3)
		if x == 1 {
			g[0] = f[0]
			g[1] = min(f[0], f[1]) + 1
			g[2] = min(f[0], min(f[1], f[2])) + 1
		} else if x == 2 {
			g[0] = f[0] + 1
			g[1] = min(f[0], f[1])
			g[2] = min(f[0], min(f[1], f[2])) + 1
		} else {
			g[0] = f[0] + 1
			g[1] = min(f[0], f[1]) + 1
			g[2] = min(f[0], min(f[1], f[2]))
		}
		f = g
	}
	return min(f[0], min(f[1], f[2]))
}
```

#### TypeScript

```ts
function minimumOperations(nums: number[]): number {
    let f: number[] = new Array(3).fill(0);
    for (const x of nums) {
        const g: number[] = new Array(3).fill(0);
        if (x === 1) {
            g[0] = f[0];
            g[1] = Math.min(f[0], f[1]) + 1;
            g[2] = Math.min(f[0], Math.min(f[1], f[2])) + 1;
        } else if (x === 2) {
            g[0] = f[0] + 1;
            g[1] = Math.min(f[0], f[1]);
            g[2] = Math.min(f[0], Math.min(f[1], f[2])) + 1;
        } else {
            g[0] = f[0] + 1;
            g[1] = Math.min(f[0], f[1]) + 1;
            g[2] = Math.min(f[0], Math.min(f[1], f[2]));
        }
        f = g;
    }
    return Math.min(...f);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
