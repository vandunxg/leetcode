---
comments: true
difficulty: Easy
rating: 1461
source: Biweekly Contest 55 Q1
tags:
    - Array
---

<!-- problem:start -->

# [1909. Remove One Element to Make the Array Strictly Increasing](https://leetcode.com/problems/remove-one-element-to-make-the-array-strictly-increasing)

[中文文档](/solution/1900-1999/1909.Remove%20One%20Element%20to%20Make%20the%20Array%20Strictly%20Increasing/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> <strong>đánh chỉ số từ 0</strong>, hãy trả về <code>true</code> <em>nếu có thể làm cho mảng trở thành <strong>tăng nghiêm ngặt</strong> sau khi xóa <strong>đúng một</strong> phần tử, hoặc </em><code>false</code><em> nếu không thể. Nếu mảng vốn đã tăng nghiêm ngặt, hãy trả về </em><code>true</code>.</p>

<p>Mảng <code>nums</code> được gọi là <strong>tăng nghiêm ngặt</strong> nếu <code>nums[i - 1] &lt; nums[i]</code> với mọi chỉ số <code>(1 &lt;= i &lt; nums.length).</code></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,<u>10</u>,5,7]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Xóa 10 ở chỉ số 2 khỏi nums, ta được [1,2,5,7].
[1,2,5,7] tăng nghiêm ngặt, nên trả về true.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,3,1,2]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong>
[3,1,2] là kết quả khi xóa phần tử ở chỉ số 0.
[2,1,2] là kết quả khi xóa phần tử ở chỉ số 1.
[2,3,2] là kết quả khi xóa phần tử ở chỉ số 2.
[2,3,1] là kết quả khi xóa phần tử ở chỉ số 3.
Không có mảng kết quả nào tăng nghiêm ngặt, nên trả về false.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,1]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Xóa bất kỳ phần tử nào cũng cho kết quả [1,1].
[1,1] không tăng nghiêm ngặt, nên trả về false.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Thử xóa từng phần tử rồi kiểm tra lại thứ tự sẽ tốn $O(n^2)$. Vì chỉ được phép xóa nhiều nhất một phần tử, nên chỉ có thể xuất hiện nhiều nhất một điểm giảm.
>
> Duyệt đến $i$ đầu tiên sao cho $\textit{nums}[i]\ge \textit{nums}[i+1]$. Chỉ cần kiểm tra việc xóa $i$ hoặc xóa $i+1$.
>
> Mỗi lần kiểm tra, ta duyệt mảng một lần và bỏ qua chỉ số cần xóa. Nếu không có điểm giảm, việc xóa một trong hai phần tử vẫn cho kết quả hợp lệ.

<!-- thinking:end -->

Ta có thể duyệt mảng để tìm vị trí đầu tiên $i$ mà điều kiện $\textit{nums}[i] < \textit{nums}[i+1]$ không đúng. Sau đó, kiểm tra xem mảng có tăng nghiêm ngặt sau khi xóa $i$ hoặc $i+1$ hay không. Nếu có, trả về $\textit{true}$; ngược lại, trả về $\textit{false}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canBeIncreasing(self, nums: List[int]) -> bool:
        def check(k: int) -> bool:
            pre = -inf
            for i, x in enumerate(nums):
                if i == k:
                    continue
                if pre >= x:
                    return False
                pre = x
            return True

        i = 0
        while i + 1 < len(nums) and nums[i] < nums[i + 1]:
            i += 1
        return check(i) or check(i + 1)
```

#### Java

```java
class Solution {
    public boolean canBeIncreasing(int[] nums) {
        int i = 0;
        while (i + 1 < nums.length && nums[i] < nums[i + 1]) {
            ++i;
        }
        return check(nums, i) || check(nums, i + 1);
    }

    private boolean check(int[] nums, int k) {
        int pre = 0;
        for (int i = 0; i < nums.length; ++i) {
            if (i == k) {
                continue;
            }
            if (pre >= nums[i]) {
                return false;
            }
            pre = nums[i];
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canBeIncreasing(vector<int>& nums) {
        int n = nums.size();
        auto check = [&](int k) -> bool {
            int pre = 0;
            for (int i = 0; i < n; ++i) {
                if (i == k) {
                    continue;
                }
                if (pre >= nums[i]) {
                    return false;
                }
                pre = nums[i];
            }
            return true;
        };
        int i = 0;
        while (i + 1 < n && nums[i] < nums[i + 1]) {
            ++i;
        }
        return check(i) || check(i + 1);
    }
};
```

#### Go

```go
func canBeIncreasing(nums []int) bool {
	check := func(k int) bool {
		pre := 0
		for i, x := range nums {
			if i == k {
				continue
			}
			if pre >= x {
				return false
			}
			pre = x
		}
		return true
	}
	i := 0
	for i+1 < len(nums) && nums[i] < nums[i+1] {
		i++
	}
	return check(i) || check(i+1)
}
```

#### TypeScript

```ts
function canBeIncreasing(nums: number[]): boolean {
    const n = nums.length;
    const check = (k: number): boolean => {
        let pre = 0;
        for (let i = 0; i < n; ++i) {
            if (i === k) {
                continue;
            }
            if (pre >= nums[i]) {
                return false;
            }
            pre = nums[i];
        }
        return true;
    };
    let i = 0;
    while (i + 1 < n && nums[i] < nums[i + 1]) {
        ++i;
    }
    return check(i) || check(i + 1);
}
```

#### Rust

```rust
impl Solution {
    pub fn can_be_increasing(nums: Vec<i32>) -> bool {
        let check = |k: usize| -> bool {
            let mut pre = 0;
            for (i, &x) in nums.iter().enumerate() {
                if i == k {
                    continue;
                }
                if pre >= x {
                    return false;
                }
                pre = x;
            }
            true
        };

        let mut i = 0;
        while i + 1 < nums.len() && nums[i] < nums[i + 1] {
            i += 1;
        }
        check(i) || check(i + 1)
    }
}
```

#### C#

```cs
public class Solution {
    public bool CanBeIncreasing(int[] nums) {
        int n = nums.Length;
        bool check(int k) {
            int pre = 0;
            for (int i = 0; i < n; ++i) {
                if (i == k) {
                    continue;
                }
                if (pre >= nums[i]) {
                    return false;
                }
                pre = nums[i];
            }
            return true;
        }
        int i = 0;
        while (i + 1 < n && nums[i] < nums[i + 1]) {
            ++i;
        }
        return check(i) || check(i + 1);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
