---
comments: true
difficulty: Medium
rating: 1541
source: Weekly Contest 166 Q3
tags:
    - Array
    - Binary Search
---

<!-- problem:start -->

# [1283. Find the Smallest Divisor Given a Threshold](https://leetcode.com/problems/find-the-smallest-divisor-given-a-threshold)

[中文文档](/solution/1200-1299/1283.Find%20the%20Smallest%20Divisor%20Given%20a%20Threshold/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> và số nguyên <code>threshold</code>. Chọn số nguyên dương <code>divisor</code>, chia từng phần tử trong mảng cho số này rồi cộng các kết quả. Hãy tìm <code>divisor</code> <strong>nhỏ nhất</strong> sao cho tổng vừa tính không vượt quá <code>threshold</code>.</p>

<p>Mỗi kết quả phép chia được làm tròn lên thành số nguyên nhỏ nhất không nhỏ hơn kết quả đó. (Ví dụ: <code>7/3 = 3</code> và <code>10/2 = 5</code>).</p>

<p>Các test case được tạo sao cho luôn tồn tại đáp án.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,5,9], threshold = 6
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Nếu divisor bằng 1, tổng là 17 (1+2+5+9). 
Nếu divisor bằng 4, tổng là 7 (1+1+2+3); còn nếu divisor bằng 5, tổng là 5 (1+1+1+2). 
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [44,22,33,11,1], threshold = 5
<strong>Đầu ra:</strong> 44
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
	<li><code>nums.length &lt;= threshold &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Divisor càng lớn thì tổng các kết quả làm tròn lên càng nhỏ, nên điều kiện này có tính đơn điệu. Vì $n \le 5\times 10^4$, không thể thử mọi divisor. Ta tìm kiếm nhị phân divisor nhỏ nhất $v$ trong $[1,\max nums]$ sao cho $\sum \lceil nums_i/v \rceil \le threshold$. Mỗi lần kiểm tra cần duyệt mảng một lượt.

<!-- thinking:end -->

Nhận thấy nếu với divisor $v$, tổng các kết quả chia từng số trong $nums$ cho $v$ không vượt quá $threshold$, thì mọi giá trị lớn hơn $v$ cũng thỏa điều kiện. Điều kiện có tính đơn điệu, nên ta có thể dùng tìm kiếm nhị phân để tìm $v$ nhỏ nhất thỏa mãn.

Ta đặt cận trái của tìm kiếm nhị phân là $l=1$ và cận phải là $r=\max(nums)$. Mỗi lần, lấy $mid=(l+r)/2$ rồi tính tổng $s$ của các kết quả khi chia từng số trong $nums$ cho $mid$. Nếu $s$ không vượt quá $threshold$, thì $mid$ thỏa điều kiện và ta cập nhật $r=mid$; ngược lại, cập nhật $l=mid+1$.

Cuối cùng, trả về $l$.

Độ phức tạp thời gian là $O(n \times \log M)$, trong đó $n$ là độ dài mảng $nums$ và $M$ là giá trị lớn nhất trong mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestDivisor(self, nums: List[int], threshold: int) -> int:
        def f(v: int) -> bool:
            v += 1
            return sum((x + v - 1) // v for x in nums) <= threshold

        return bisect_left(range(max(nums)), True, key=f) + 1
```

#### Java

```java
class Solution {
    public int smallestDivisor(int[] nums, int threshold) {
        int l = 1, r = 1000000;
        while (l < r) {
            int mid = (l + r) >> 1;
            int s = 0;
            for (int x : nums) {
                s += (x + mid - 1) / mid;
            }
            if (s <= threshold) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int smallestDivisor(vector<int>& nums, int threshold) {
        int l = 1;
        int r = *max_element(nums.begin(), nums.end());
        while (l < r) {
            int mid = (l + r) >> 1;
            int s = 0;
            for (int x : nums) {
                s += (x + mid - 1) / mid;
            }
            if (s <= threshold) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
};
```

#### Go

```go
func smallestDivisor(nums []int, threshold int) int {
	return sort.Search(slices.Max(nums), func(v int) bool {
		v++
		s := 0
		for _, x := range nums {
			s += (x + v - 1) / v
		}
		return s <= threshold
	}) + 1
}
```

#### TypeScript

```ts
function smallestDivisor(nums: number[], threshold: number): number {
    let l = 1;
    let r = Math.max(...nums);
    while (l < r) {
        const mid = (l + r) >> 1;
        const s = nums.reduce((acc, x) => acc + Math.ceil(x / mid), 0);
        if (s <= threshold) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return l;
}
```

#### Rust

```rust
impl Solution {
    pub fn smallest_divisor(nums: Vec<i32>, threshold: i32) -> i32 {
        let mut l = 1;
        let mut r = *nums.iter().max().unwrap();
        while l < r {
            let mid = (l + r) / 2;
            let s: i32 = nums.iter().map(|&x| (x + mid - 1) / mid).sum();
            if s <= threshold {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        l
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @param {number} threshold
 * @return {number}
 */
var smallestDivisor = function (nums, threshold) {
    let l = 1;
    let r = Math.max(...nums);
    while (l < r) {
        const mid = (l + r) >> 1;
        const s = nums.reduce((acc, x) => acc + Math.ceil(x / mid), 0);
        if (s <= threshold) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return l;
};
```

#### C#

```cs
public class Solution {
    public int SmallestDivisor(int[] nums, int threshold) {
        int l = 1;
        int r = nums.Max();
        while (l < r) {
            int mid = (l + r) >> 1;
            int s = 0;
            foreach (int x in nums) {
                s += (x + mid - 1) / mid;
            }
            if (s <= threshold) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
