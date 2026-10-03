---
comments: true
difficulty: Easy
rating: 1222
source: Weekly Contest 328 Q1
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [2535. Difference Between Element Sum and Digit Sum of an Array](https://leetcode.com/problems/difference-between-element-sum-and-digit-sum-of-an-array)

[中文文档](/solution/2500-2599/2535.Difference%20Between%20Element%20Sum%20and%20Digit%20Sum%20of%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên dương <code>nums</code>.</p>

<ul>
	<li><strong>Tổng các phần tử</strong> là tổng của tất cả phần tử trong <code>nums</code>.</li>
	<li><strong>Tổng các chữ số</strong> là tổng của tất cả chữ số (kể cả chữ số trùng nhau) xuất hiện trong <code>nums</code>.</li>
</ul>

<p>Hãy trả về <em>hiệu <strong>tuyệt đối</strong> giữa <strong>tổng các phần tử</strong> và <strong>tổng các chữ số</strong> của </em><code>nums</code>.</p>

<p><strong>Lưu ý</strong> rằng hiệu tuyệt đối giữa hai số nguyên <code>x</code> và <code>y</code> được định nghĩa là <code>|x - y|</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,15,6,3]
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong>
Tổng các phần tử của nums là 1 + 15 + 6 + 3 = 25.
Tổng các chữ số của nums là 1 + 1 + 5 + 6 + 3 = 16.
Hiệu tuyệt đối giữa tổng các phần tử và tổng các chữ số là |25 - 16| = 9.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
Tổng các phần tử của nums là 1 + 2 + 3 + 4 = 10.
Tổng các chữ số của nums là 1 + 2 + 3 + 4 = 10.
Hiệu tuyệt đối giữa tổng các phần tử và tổng các chữ số là |10 - 10| = 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 2000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 2000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Hiệu tuyệt đối giữa tổng các số và tổng tất cả chữ số của chúng. Chỉ cần tách từng giá trị là đủ.
>
> Cộng từng giá trị vào $x$ và các chữ số của nó vào $y$. Tổng các phần tử không nhỏ hơn tổng các chữ số, nên trả về $x-y$.

<!-- thinking:end -->

Chúng ta duyệt qua mảng $\textit{nums}$, tính tổng các phần tử $x$ và tổng các chữ số $y$, rồi cuối cùng trả về $|x - y|$. Vì $x$ luôn lớn hơn hoặc bằng $y$, ta có thể trực tiếp trả về $x - y$.

Độ phức tạp thời gian là $O(n \times \log_{10} M)$, trong đó $n$ và $M$ lần lượt là độ dài mảng $\textit{nums}$ và giá trị lớn nhất của các phần tử trong mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def differenceOfSum(self, nums: List[int]) -> int:
        x = y = 0
        for v in nums:
            x += v
            while v:
                y += v % 10
                v //= 10
        return x - y
```

#### Java

```java
class Solution {
    public int differenceOfSum(int[] nums) {
        int x = 0, y = 0;
        for (int v : nums) {
            x += v;
            for (; v > 0; v /= 10) {
                y += v % 10;
            }
        }
        return x - y;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int differenceOfSum(vector<int>& nums) {
        int x = 0, y = 0;
        for (int v : nums) {
            x += v;
            for (; v; v /= 10) {
                y += v % 10;
            }
        }
        return x - y;
    }
};
```

#### Go

```go
func differenceOfSum(nums []int) int {
	var x, y int
	for _, v := range nums {
		x += v
		for ; v > 0; v /= 10 {
			y += v % 10
		}
	}
	return x - y
}
```

#### TypeScript

```ts
function differenceOfSum(nums: number[]): number {
    let [x, y] = [0, 0];
    for (let v of nums) {
        x += v;
        for (; v; v = Math.floor(v / 10)) {
            y += v % 10;
        }
    }
    return x - y;
}
```

#### Rust

```rust
impl Solution {
    pub fn difference_of_sum(nums: Vec<i32>) -> i32 {
        let mut x = 0;
        let mut y = 0;

        for &v in &nums {
            x += v;
            let mut num = v;
            while num > 0 {
                y += num % 10;
                num /= 10;
            }
        }

        x - y
    }
}
```

#### C

```c
int differenceOfSum(int* nums, int numsSize) {
    int x = 0, y = 0;
    for (int i = 0; i < numsSize; i++) {
        int v = nums[i];
        x += v;
        while (v > 0) {
            y += v % 10;
            v /= 10;
        }
    }
    return x - y;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
