---
comments: true
difficulty: Easy
rating: 1314
source: Biweekly Contest 71 Q1
tags:
    - Greedy
    - Math
    - Sorting
---

<!-- problem:start -->

# [2160. Minimum Sum of Four Digit Number After Splitting Digits](https://leetcode.com/problems/minimum-sum-of-four-digit-number-after-splitting-digits)

[中文文档](/solution/2100-2199/2160.Minimum%20Sum%20of%20Four%20Digit%20Number%20After%20Splitting%20Digits/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <strong>dương</strong> <code>num</code> gồm đúng bốn chữ số. Hãy tách <code>num</code> thành hai số nguyên mới <code>new1</code> và <code>new2</code> bằng cách sử dụng các <strong>chữ số</strong> có trong <code>num</code>. <strong>Các số 0 ở đầu</strong> được phép xuất hiện trong <code>new1</code> và <code>new2</code>, và phải sử dụng <strong>tất cả</strong> các chữ số có trong <code>num</code>.</p>

<ul>
	<li>Ví dụ, với <code>num = 2932</code>, ta có các chữ số: hai chữ số <code>2</code>, một chữ số <code>9</code> và một chữ số <code>3</code>. Một số cặp <code>[new1, new2]</code> có thể tạo thành là <code>[22, 93]</code>, <code>[23, 92]</code>, <code>[223, 9]</code> và <code>[2, 329]</code>.</li>
</ul>

<p>Hãy trả về <em>tổng <strong>nhỏ nhất</strong> có thể có của </em><code>new1</code><em> và </em><code>new2</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 2932
<strong>Đầu ra:</strong> 52
<strong>Giải thích:</strong> Một số cặp có thể tạo thành là [29, 23], [223, 9], ...
Tổng nhỏ nhất có thể đạt được với cặp [29, 23]: 29 + 23 = 52.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 4009
<strong>Đầu ra:</strong> 13
<strong>Giải thích:</strong> Một số cặp có thể tạo thành là [0, 49], [490, 0], ...
Tổng nhỏ nhất có thể đạt được với cặp [4, 9]: 4 + 9 = 13.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1000 &lt;= num &lt;= 9999</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tách bốn chữ số thành hai số nguyên sao cho tổng nhỏ nhất. Các số 0 ở đầu được phép xuất hiện. Có thể thử tất cả các cách phân chia, nhưng đáp án tối ưu sử dụng hai chữ số nhỏ nhất làm chữ số hàng chục.
>
> Trích xuất và sắp xếp bốn chữ số; chữ số hàng chục là hai chữ số nhỏ nhất, còn chữ số hàng đơn vị là hai chữ số còn lại.
>
> Tổng là $10(a+b)+c+d$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumSum(self, num: int) -> int:
        nums = []
        while num:
            nums.append(num % 10)
            num //= 10
        nums.sort()
        return 10 * (nums[0] + nums[1]) + nums[2] + nums[3]
```

#### Java

```java
class Solution {
    public int minimumSum(int num) {
        int[] nums = new int[4];
        for (int i = 0; num != 0; ++i) {
            nums[i] = num % 10;
            num /= 10;
        }
        Arrays.sort(nums);
        return 10 * (nums[0] + nums[1]) + nums[2] + nums[3];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumSum(int num) {
        vector<int> nums;
        while (num) {
            nums.push_back(num % 10);
            num /= 10;
        }
        sort(nums.begin(), nums.end());
        return 10 * (nums[0] + nums[1]) + nums[2] + nums[3];
    }
};
```

#### Go

```go
func minimumSum(num int) int {
	var nums []int
	for num > 0 {
		nums = append(nums, num%10)
		num /= 10
	}
	sort.Ints(nums)
	return 10*(nums[0]+nums[1]) + nums[2] + nums[3]
}
```

#### TypeScript

```ts
function minimumSum(num: number): number {
    const nums = new Array(4).fill(0);
    for (let i = 0; i < 4; i++) {
        nums[i] = num % 10;
        num = Math.floor(num / 10);
    }
    nums.sort((a, b) => a - b);
    return 10 * (nums[0] + nums[1]) + nums[2] + nums[3];
}
```

#### Rust

```rust
impl Solution {
    pub fn minimum_sum(mut num: i32) -> i32 {
        let mut nums = [0; 4];
        for i in 0..4 {
            nums[i] = num % 10;
            num /= 10;
        }
        nums.sort();
        10 * (nums[0] + nums[1]) + nums[2] + nums[3]
    }
}
```

#### C

```c
int cmp(const void* a, const void* b) {
    return *(int*) a - *(int*) b;
}

int minimumSum(int num) {
    int nums[4] = {0};
    for (int i = 0; i < 4; i++) {
        nums[i] = num % 10;
        num /= 10;
    }
    qsort(nums, 4, sizeof(int), cmp);
    return 10 * (nums[0] + nums[1]) + nums[2] + nums[3];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
