---
comments: true
difficulty: Easy
rating: 1121
source: Weekly Contest 191 Q1
tags:
    - Array
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1464. Maximum Product of Two Elements in an Array](https://leetcode.com/problems/maximum-product-of-two-elements-in-an-array)

[中文文档](/solution/1400-1499/1464.Maximum%20Product%20of%20Two%20Elements%20in%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>.</p>

<p>Chọn hai chỉ số <strong>khác nhau</strong> <code>i</code> và <code>j</code> của mảng đó.</p>

<p>Trả về giá trị <strong>lớn nhất</strong> của <code>(nums[i] - 1) * (nums[j] - 1)</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,4,5,2]
<strong>Đầu ra:</strong> 12
<strong>Giải thích:</strong> Nếu chọn các chỉ số i=1 và j=2 (đánh chỉ số từ 0), ta nhận được giá trị lớn nhất, đó là (nums[1]-1)*(nums[2]-1) = (4-1)*(5-1) = 3*4 = 12.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,5,4,5]
<strong>Đầu ra:</strong> 16
<strong>Giải thích:</strong> Chọn các chỉ số i=1 và j=3 (đánh chỉ số từ 0), ta nhận được giá trị lớn nhất là (5-1)*(5-1) = 16.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,7]
<strong>Đầu ra:</strong> 12
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 500</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10^3</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> $n\le 500$. Liệt kê mọi cặp không thứ tự và giữ lại giá trị lớn nhất của $(a-1)(b-1)$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxProduct(self, nums: List[int]) -> int:
        ans = 0
        for i, a in enumerate(nums):
            for b in nums[i + 1 :]:
                ans = max(ans, (a - 1) * (b - 1))
        return ans
```

#### Java

```java
class Solution {
    public int maxProduct(int[] nums) {
        int ans = 0;
        int n = nums.length;
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                ans = Math.max(ans, (nums[i] - 1) * (nums[j] - 1));
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int ans = 0;
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                ans = max(ans, (nums[i] - 1) * (nums[j] - 1));
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxProduct(nums []int) int {
	ans := 0
	for i, a := range nums {
		for _, b := range nums[i+1:] {
			t := (a - 1) * (b - 1)
			if ans < t {
				ans = t
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maxProduct(nums: number[]): number {
    const n = nums.length;
    for (let i = 0; i < 2; i++) {
        let maxIdx = i;
        for (let j = i + 1; j < n; j++) {
            if (nums[j] > nums[maxIdx]) {
                maxIdx = j;
            }
        }
        [nums[i], nums[maxIdx]] = [nums[maxIdx], nums[i]];
    }
    return (nums[0] - 1) * (nums[1] - 1);
}
```

#### Rust

```rust
impl Solution {
    pub fn max_product(nums: Vec<i32>) -> i32 {
        let mut max = 0;
        let mut submax = 0;
        for &num in nums.iter() {
            if num > max {
                submax = max;
                max = num;
            } else if num > submax {
                submax = num;
            }
        }
        (max - 1) * (submax - 1)
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param Integer[] $nums
     * @return Integer
     */
    function maxProduct($nums) {
        $max = 0;
        $submax = 0;
        for ($i = 0; $i < count($nums); $i++) {
            if ($nums[$i] > $max) {
                $submax = $max;
                $max = $nums[$i];
            } elseif ($nums[$i] > $submax) {
                $submax = $nums[$i];
            }
        }
        return ($max - 1) * ($submax - 1);
    }
}
```

#### C

```c
int maxProduct(int* nums, int numsSize) {
    int max = 0;
    int submax = 0;
    for (int i = 0; i < numsSize; i++) {
        int num = nums[i];
        if (num > max) {
            submax = max;
            max = num;
        } else if (num > submax) {
            submax = num;
        }
    }
    return (max - 1) * (submax - 1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 có độ phức tạp bậc hai. Tích tăng theo hai giá trị, vì vậy hãy sắp xếp rồi lấy hai phần tử cuối cùng.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxProduct(self, nums: List[int]) -> int:
        nums.sort()
        return (nums[-1] - 1) * (nums[-2] - 1)
```

#### Java

```java
class Solution {
    public int maxProduct(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;
        return (nums[n - 1] - 1) * (nums[n - 2] - 1);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        sort(nums.rbegin(), nums.rend());
        return (nums[0] - 1) * (nums[1] - 1);
    }
};
```

#### Go

```go
func maxProduct(nums []int) int {
	sort.Ints(nums)
	n := len(nums)
	return (nums[n-1] - 1) * (nums[n-2] - 1)
}
```

#### TypeScript

```ts
function maxProduct(nums: number[]): number {
    let max = 0;
    let submax = 0;
    for (const num of nums) {
        if (num > max) {
            submax = max;
            max = num;
        } else if (num > submax) {
            submax = num;
        }
    }
    return (max - 1) * (submax - 1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 2 vẫn phải sắp xếp. Chỉ cần duyệt một lần để giữ lại giá trị lớn nhất và lớn thứ hai.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxProduct(self, nums: List[int]) -> int:
        a = b = 0
        for v in nums:
            if v > a:
                a, b = v, a
            elif v > b:
                b = v
        return (a - 1) * (b - 1)
```

#### Java

```java
class Solution {
    public int maxProduct(int[] nums) {
        int a = 0, b = 0;
        for (int v : nums) {
            if (v > a) {
                b = a;
                a = v;
            } else if (v > b) {
                b = v;
            }
        }
        return (a - 1) * (b - 1);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int a = 0, b = 0;
        for (int v : nums) {
            if (v > a) {
                b = a;
                a = v;
            } else if (v > b) {
                b = v;
            }
        }
        return (a - 1) * (b - 1);
    }
};
```

#### Go

```go
func maxProduct(nums []int) int {
	a, b := 0, 0
	for _, v := range nums {
		if v > a {
			b, a = a, v
		} else if v > b {
			b = v
		}
	}
	return (a - 1) * (b - 1)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
