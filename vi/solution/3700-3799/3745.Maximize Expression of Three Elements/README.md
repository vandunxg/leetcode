---
comments: true
difficulty: Easy
rating: 1218
source: Weekly Contest 476 Q1
tags:
    - Greedy
    - Array
    - Enumeration
    - Sorting
---

<!-- problem:start -->

# [3745. Maximize Expression of Three Elements](https://leetcode.com/problems/maximize-expression-of-three-elements)

[中文文档](/solution/3700-3799/3745.Maximize%20Expression%20of%20Three%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>.</p>

<p>Chọn ba phần tử <code>a</code>, <code>b</code> và <code>c</code> từ <code>nums</code> tại các chỉ số <strong>khác nhau</strong> sao cho giá trị của biểu thức <code>a + b - c</code> là lớn nhất.</p>

<p>Trả về một số nguyên biểu thị <strong>giá trị lớn nhất có thể đạt được</strong> của biểu thức này.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,4,2,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể chọn <code>a = 4</code>, <code>b = 5</code> và <code>c = 1</code>. Giá trị của biểu thức là <code>4 + 5 - 1 = 8</code>, đây là giá trị lớn nhất có thể đạt được.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [-2,0,5,-2,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">11</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể chọn <code>a = 5</code>, <code>b = 4</code> và <code>c = -2</code>. Giá trị của biểu thức là <code>5 + 4 - (-2) = 11</code>, đây là giá trị lớn nhất có thể đạt được.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= nums.length &lt;= 100</code></li>
	<li><code>-100 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm giá trị lớn nhất, lớn thứ hai và nhỏ nhất

<!-- thinking:start -->

> **Tư duy**
>
> Để $a+b-c$ đạt giá trị lớn nhất, ta chọn hai giá trị lớn nhất làm $a,b$ và giá trị nhỏ nhất làm $c$. Chỉ cần duyệt qua mảng một lần để theo dõi giá trị lớn nhất, lớn thứ hai và nhỏ nhất; không cần sắp xếp.

<!-- thinking:end -->

Theo đề bài, ta cần chọn ba phần tử $a$, $b$ và $c$ tại các chỉ số khác nhau sao cho giá trị của biểu thức $a + b - c$ là lớn nhất.

Ta chỉ cần duyệt qua mảng để tìm hai phần tử lớn nhất $a$ và $b$, cùng phần tử nhỏ nhất $c$. Sau đó, ta có thể tính giá trị của biểu thức.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximizeExpressionOfThree(self, nums: List[int]) -> int:
        a = b = -inf
        c = inf
        for x in nums:
            if x < c:
                c = x
            if x >= a:
                a, b = x, a
            elif x > b:
                b = x
        return a + b - c
```

#### Java

```java
class Solution {
    public int maximizeExpressionOfThree(int[] nums) {
        final int inf = 1 << 30;
        int a = -inf, b = -inf, c = inf;
        for (int x : nums) {
            if (x < c) {
                c = x;
            }
            if (x >= a) {
                b = a;
                a = x;
            } else if (x > b) {
                b = x;
            }
        }
        return a + b - c;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximizeExpressionOfThree(vector<int>& nums) {
        const int inf = 1 << 30;
        int a = -inf, b = -inf, c = inf;
        for (int x : nums) {
            if (x < c) {
                c = x;
            }
            if (x >= a) {
                b = a;
                a = x;
            } else if (x > b) {
                b = x;
            }
        }
        return a + b - c;
    }
};
```

#### Go

```go
func maximizeExpressionOfThree(nums []int) int {
    const inf = 1 << 30
    a, b, c := -inf, -inf, inf
    for _, x := range nums {
        if x < c {
            c = x
        }
        if x >= a {
            b = a
            a = x
        } else if x > b {
            b = x
        }
    }
    return a + b - c
}
```

#### TypeScript

```ts
function maximizeExpressionOfThree(nums: number[]): number {
    const inf = 1 << 30;
    let [a, b, c] = [-inf, -inf, inf];

    for (const x of nums) {
        if (x < c) {
            c = x;
        }
        if (x >= a) {
            b = a;
            a = x;
        } else if (x > b) {
            b = x;
        }
    }
    return a + b - c;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
