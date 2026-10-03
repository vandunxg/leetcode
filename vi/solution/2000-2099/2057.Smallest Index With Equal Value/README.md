---
comments: true
difficulty: Easy
rating: 1167
source: Weekly Contest 265 Q1
tags:
    - Array
---

<!-- problem:start -->

# [2057. Smallest Index With Equal Value](https://leetcode.com/problems/smallest-index-with-equal-value)

[中文文档](/solution/2000-2099/2057.Smallest%20Index%20With%20Equal%20Value/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> được <strong>đánh chỉ số từ 0</strong>, hãy trả về <em><strong>chỉ số nhỏ nhất</strong> </em><code>i</code><em> của </em><code>nums</code><em> sao cho </em><code>i mod 10 == nums[i]</code><em>, hoặc </em><code>-1</code><em> nếu không tồn tại chỉ số như vậy</em>.</p>

<p><code>x mod y</code> biểu thị <strong>phần dư</strong> khi chia <code>x</code> cho <code>y</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,1,2]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
i=0: 0 mod 10 = 0 == nums[0].
i=1: 1 mod 10 = 1 == nums[1].
i=2: 2 mod 10 = 2 == nums[2].
Tất cả các chỉ số đều thỏa mãn i mod 10 == nums[i], nên ta trả về chỉ số nhỏ nhất là 0.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,3,2,1]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
i=0: 0 mod 10 = 0 != nums[0].
i=1: 1 mod 10 = 1 != nums[1].
i=2: 2 mod 10 = 2 == nums[2].
i=3: 3 mod 10 = 3 != nums[3].
2 là chỉ số duy nhất thỏa mãn i mod 10 == nums[i].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,5,6,7,8,9,0]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không có chỉ số nào thỏa mãn i mod 10 == nums[i].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 9</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Với $n \le 100$, ta cần tìm chỉ số nhỏ nhất $i$ sao cho $i \bmod 10 = nums[i]$, hoặc trả về $-1$.
>
> Chỉ cần duyệt mảng một lần từ trái sang phải.

<!-- thinking:end -->

Ta duyệt trực tiếp qua mảng. Với mỗi chỉ số $i$, ta kiểm tra xem nó có thỏa mãn $i \bmod 10 = \textit{nums}[i]$ hay không. Nếu có, ta trả về ngay chỉ số hiện tại $i$.

Nếu duyệt hết mảng mà không tìm thấy chỉ số thỏa mãn, ta trả về $-1$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestEqual(self, nums: List[int]) -> int:
        for i, x in enumerate(nums):
            if i % 10 == x:
                return i
        return -1
```

#### Java

```java
class Solution {
    public int smallestEqual(int[] nums) {
        for (int i = 0; i < nums.length; ++i) {
            if (i % 10 == nums[i]) {
                return i;
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int smallestEqual(vector<int>& nums) {
        for (int i = 0; i < nums.size(); ++i) {
            if (i % 10 == nums[i]) {
                return i;
            }
        }
        return -1;
    }
};
```

#### Go

```go
func smallestEqual(nums []int) int {
	for i, x := range nums {
		if i%10 == x {
			return i
		}
	}
	return -1
}
```

#### TypeScript

```ts
function smallestEqual(nums: number[]): number {
    for (let i = 0; i < nums.length; ++i) {
        if (i % 10 === nums[i]) {
            return i;
        }
    }
    return -1;
}
```

#### Rust

```rust
impl Solution {
    pub fn smallest_equal(nums: Vec<i32>) -> i32 {
        for (i, &x) in nums.iter().enumerate() {
            if i % 10 == x as usize {
                return i as i32;
            }
        }
        -1
    }
}
```

#### Cangjie

```cj
class Solution {
    func smallestEqual(nums: Array<Int64>): Int64 {
        for (i in 0..nums.size) {
            if (i % 10 == nums[i]) {
                return i
            }
        }
        -1
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
