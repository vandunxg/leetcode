---
comments: true
difficulty: Easy
rating: 1295
source: Weekly Contest 358 Q1
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [2815. Max Pair Sum in an Array](https://leetcode.com/problems/max-pair-sum-in-an-array)

[中文文档](/solution/2800-2899/2815.Max%20Pair%20Sum%20in%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>. Hãy tìm <strong>tổng lớn nhất</strong> của một cặp số trong <code>nums</code> sao cho <strong>chữ số lớn nhất</strong> trong cả hai số là như nhau.</p>

<p>Ví dụ, 2373 gồm ba chữ số khác nhau: 2, 3 và 7, trong đó 7 là chữ số lớn nhất.</p>

<p>Trả về <strong>tổng lớn nhất</strong>, hoặc -1 nếu không tồn tại cặp số như vậy.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [112,131,411]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chữ số lớn nhất của mỗi số lần lượt là [2,3,4].</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2536,1613,3366,162]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5902</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chữ số lớn nhất của tất cả các số đều là 6, nên đáp án là <span class="example-io">2536 + 3366 = 5902.</span></p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [51,71,17,24,42]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">88</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chữ số lớn nhất của mỗi số lần lượt là [5,7,7,4,4].</p>

<p>Vì vậy, chỉ có hai cặp khả dĩ: 71 + 17 = 88 và 24 + 42 = 66.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Với $n\le 100$, ta có thể liệt kê mọi cặp và chấp nhận một cặp khi hai số có cùng chữ số lớn nhất trong biểu diễn thập phân. Với ràng buộc này, không cần nhóm các số theo chữ số đó.

<!-- thinking:end -->

Đầu tiên, ta khởi tạo biến đáp án $ans=-1$. Tiếp theo, ta trực tiếp liệt kê mọi cặp $(nums[i], nums[j])$ với $i \lt j$ và tính tổng của chúng là $v=nums[i] + nums[j]$. Nếu $v$ lớn hơn $ans$ và chữ số lớn nhất của $nums[i]$ và $nums[j]$ giống nhau, ta cập nhật $ans$ bằng $v$.

Độ phức tạp thời gian là $O(n^2 \times \log M)$, trong đó $n$ là độ dài của mảng và $M$ là giá trị lớn nhất trong mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSum(self, nums: List[int]) -> int:
        ans = -1
        for i, x in enumerate(nums):
            for y in nums[i + 1 :]:
                v = x + y
                if ans < v and max(str(x)) == max(str(y)):
                    ans = v
        return ans
```

#### Java

```java
class Solution {
    public int maxSum(int[] nums) {
        int ans = -1;
        int n = nums.length;
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                int v = nums[i] + nums[j];
                if (ans < v && f(nums[i]) == f(nums[j])) {
                    ans = v;
                }
            }
        }
        return ans;
    }

    private int f(int x) {
        int y = 0;
        for (; x > 0; x /= 10) {
            y = Math.max(y, x % 10);
        }
        return y;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxSum(vector<int>& nums) {
        int ans = -1;
        int n = nums.size();
        auto f = [](int x) {
            int y = 0;
            for (; x; x /= 10) {
                y = max(y, x % 10);
            }
            return y;
        };
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                int v = nums[i] + nums[j];
                if (ans < v && f(nums[i]) == f(nums[j])) {
                    ans = v;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxSum(nums []int) int {
	ans := -1
	f := func(x int) int {
		y := 0
		for ; x > 0; x /= 10 {
			y = max(y, x%10)
		}
		return y
	}
	for i, x := range nums {
		for _, y := range nums[i+1:] {
			if v := x + y; ans < v && f(x) == f(y) {
				ans = v
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maxSum(nums: number[]): number {
    const n = nums.length;
    let ans = -1;
    const f = (x: number): number => {
        let y = 0;
        for (; x > 0; x = Math.floor(x / 10)) {
            y = Math.max(y, x % 10);
        }
        return y;
    };
    for (let i = 0; i < n; ++i) {
        for (let j = i + 1; j < n; ++j) {
            const v = nums[i] + nums[j];
            if (ans < v && f(nums[i]) === f(nums[j])) {
                ans = v;
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
