---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Longest Increasing Subsequence
---

<!-- problem:start -->

# [334. Increasing Triplet Subsequence](https://leetcode.com/problems/increasing-triplet-subsequence)

[中文文档](/solution/0300-0399/0334.Increasing%20Triplet%20Subsequence/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>, hãy trả về <code>true</code><em> nếu tồn tại bộ ba chỉ số </em><code>(i, j, k)</code><em> sao cho </em><code>i &lt; j &lt; k</code><em> và </em><code>nums[i] &lt; nums[j] &lt; nums[k]</code>. Nếu không tồn tại bộ chỉ số nào như vậy, hãy trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,5]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Bất kỳ bộ ba nào thỏa mãn i &lt; j &lt; k đều hợp lệ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,4,3,2,1]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không tồn tại bộ ba nào như vậy.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,1,5,0,4,6]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Một bộ ba hợp lệ là (1, 4, 5), vì nums[1] == 1 &lt; nums[4] == 4 &lt; nums[5] == 6.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 5 * 10<sup>5</sup></code></li>
	<li><code>-2<sup>31</sup> &lt;= nums[i] &lt;= 2<sup>31</sup> - 1</code></li>
</ul>

<p>&nbsp;</p>
<strong>Câu hỏi mở rộng:</strong> Bạn có thể cài đặt lời giải có độ phức tạp thời gian <code>O(n)</code> và độ phức tạp không gian <code>O(1)</code> không?

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần xác định có tồn tại bộ ba chỉ số tăng dần hay không. Thử mọi chỉ số ở giữa tốn $O(n^2)$. Ta chỉ cần lưu một giá trị nhỏ hơn ở bên trái và một giá trị ứng viên cho phần tử thứ hai.
>
> Duy trì $mi<mid$. Nếu gặp giá trị lớn hơn $mid$ thì ta đã tìm được bộ ba; nếu không, cập nhật $mi$ khi giá trị mới nhỏ hơn hoặc bằng nó, còn lại thì cập nhật $mid$. Không cần lưu chỉ số cũ của $mid$: nó vốn đã nằm sau một $mi$ nhỏ hơn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def increasingTriplet(self, nums: List[int]) -> bool:
        mi, mid = inf, inf
        for num in nums:
            if num > mid:
                return True
            if num <= mi:
                mi = num
            else:
                mid = num
        return False
```

#### Java

```java
class Solution {
    public boolean increasingTriplet(int[] nums) {
        int min = Integer.MAX_VALUE, mid = Integer.MAX_VALUE;
        for (int num : nums) {
            if (num > mid) {
                return true;
            }
            if (num <= min) {
                min = num;
            } else {
                mid = num;
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool increasingTriplet(vector<int>& nums) {
        int mi = INT_MAX, mid = INT_MAX;
        for (int num : nums) {
            if (num > mid) return true;
            if (num <= mi)
                mi = num;
            else
                mid = num;
        }
        return false;
    }
};
```

#### Go

```go
func increasingTriplet(nums []int) bool {
	min, mid := math.MaxInt32, math.MaxInt32
	for _, num := range nums {
		if num > mid {
			return true
		}
		if num <= min {
			min = num
		} else {
			mid = num
		}
	}
	return false
}
```

#### TypeScript

```ts
function increasingTriplet(nums: number[]): boolean {
    let n = nums.length;
    if (n < 3) return false;
    let min = nums[0],
        mid = Number.MAX_SAFE_INTEGER;
    for (let num of nums) {
        if (num <= min) {
            min = num;
        } else if (num <= mid) {
            mid = num;
        } else if (num > mid) {
            return true;
        }
    }
    return false;
}
```

#### Rust

```rust
impl Solution {
    pub fn increasing_triplet(nums: Vec<i32>) -> bool {
        let n = nums.len();
        if n < 3 {
            return false;
        }
        let mut min = i32::MAX;
        let mut mid = i32::MAX;
        for num in nums.into_iter() {
            if num <= min {
                min = num;
            } else if num <= mid {
                mid = num;
            } else {
                return true;
            }
        }
        false
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
