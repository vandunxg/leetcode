---
comments: true
difficulty: Medium
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [377. Combination Sum IV](https://leetcode.com/problems/combination-sum-iv)

[中文文档](/solution/0300-0399/0377.Combination%20Sum%20IV/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng các số nguyên <code>nums</code> <strong>đôi một khác nhau</strong> và số nguyên đích <code>target</code>, hãy trả về <em>số tổ hợp có thể tạo thành</em>&nbsp;<code>target</code>.</p>

<p>Các test case được tạo sao cho đáp án có thể biểu diễn bằng số nguyên <strong>32-bit</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3], target = 4
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong>
Các cách tạo thành target là:
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)
Lưu ý rằng các dãy khác nhau được tính là những tổ hợp khác nhau.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [9], target = 3
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 200</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 1000</code></li>
	<li>Mọi phần tử trong <code>nums</code> đều <strong>khác nhau</strong>.</li>
	<li><code>1 &lt;= target &lt;= 1000</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Nếu mảng đã cho được phép chứa số âm thì sao? Điều đó làm thay đổi bài toán như thế nào? Cần thêm điều kiện giới hạn nào vào đề bài để cho phép số âm?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Đếm số cách có thứ tự để đạt tổng $target$. Nếu vòng lặp ngoài duyệt từng phần tử, ta sẽ bỏ sót các hoán vị. Thay vào đó, duyệt tổng trước rồi xét phần tử cuối cùng.
>
> $f[i]$ là số hoán vị có tổng bằng $i$, với $f[0]=1$. Với mỗi $i$ và mỗi $x\le i$, cộng $f[i-x]$ vào kết quả. Cách làm tương tự bài 322, nhưng đếm số cách thay vì tìm số lượng xu ít nhất.

<!-- thinking:end -->

Đặt $f[i]$ là số cách tạo thành tổng $i$. Ban đầu, $f[0] = 1$ và các giá trị còn lại $f[i] = 0$. Đáp án cuối cùng là $f[target]$.

Để tính $f[i]$, ta có thể duyệt từng phần tử $x$ trong mảng. Nếu $i \ge x$, thì cập nhật $f[i] = f[i] + f[i - x]$.

Cuối cùng, trả về $f[target]$.

Độ phức tạp thời gian là $O(n \times target)$ và độ phức tạp không gian là $O(target)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def combinationSum4(self, nums: List[int], target: int) -> int:
        f = [1] + [0] * target
        for i in range(1, target + 1):
            for x in nums:
                if i >= x:
                    f[i] += f[i - x]
        return f[target]
```

#### Java

```java
class Solution {
    public int combinationSum4(int[] nums, int target) {
        int[] f = new int[target + 1];
        f[0] = 1;
        for (int i = 1; i <= target; ++i) {
            for (int x : nums) {
                if (i >= x) {
                    f[i] += f[i - x];
                }
            }
        }
        return f[target];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int combinationSum4(vector<int>& nums, int target) {
        int f[target + 1];
        memset(f, 0, sizeof(f));
        f[0] = 1;
        for (int i = 1; i <= target; ++i) {
            for (int x : nums) {
                if (i >= x && f[i - x] < INT_MAX - f[i]) {
                    f[i] += f[i - x];
                }
            }
        }
        return f[target];
    }
};
```

#### Go

```go
func combinationSum4(nums []int, target int) int {
	f := make([]int, target+1)
	f[0] = 1
	for i := 1; i <= target; i++ {
		for _, x := range nums {
			if i >= x {
				f[i] += f[i-x]
			}
		}
	}
	return f[target]
}
```

#### TypeScript

```ts
function combinationSum4(nums: number[], target: number): number {
    const f: number[] = Array(target + 1).fill(0);
    f[0] = 1;
    for (let i = 1; i <= target; ++i) {
        for (const x of nums) {
            if (i >= x) {
                f[i] += f[i - x];
            }
        }
    }
    return f[target];
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var combinationSum4 = function (nums, target) {
    const f = Array(target + 1).fill(0);
    f[0] = 1;
    for (let i = 1; i <= target; ++i) {
        for (const x of nums) {
            if (i >= x) {
                f[i] += f[i - x];
            }
        }
    }
    return f[target];
};
```

#### C#

```cs
public class Solution {
    public int CombinationSum4(int[] nums, int target) {
        int[] f = new int[target + 1];
        f[0] = 1;
        for (int i = 1; i <= target; ++i) {
            foreach (int x in nums) {
                if (i >= x) {
                    f[i] += f[i - x];
                }
            }
        }
        return f[target];
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
