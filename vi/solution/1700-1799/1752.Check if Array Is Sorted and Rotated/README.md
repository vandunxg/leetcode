---
comments: true
difficulty: Easy
rating: 1324
source: Weekly Contest 227 Q1
tags:
    - Array
---

<!-- problem:start -->

# [1752. Check if Array Is Sorted and Rotated](https://leetcode.com/problems/check-if-array-is-sorted-and-rotated)

[中文文档](/solution/1700-1799/1752.Check%20if%20Array%20Is%20Sorted%20and%20Rotated/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>nums</code>, trả về <code>true</code><em> nếu ban đầu mảng được sắp xếp theo thứ tự không giảm rồi xoay <strong>một số</strong> vị trí (kể cả không xoay)</em>. Nếu không, trả về <code>false</code>.</p>

<p>Mảng ban đầu có thể chứa các phần tử <strong>trùng nhau</strong>.</p>

<p><strong>Lưu ý:</strong> Xoay mảng <code>A</code> đi <code>x</code> vị trí cho mảng <code>B</code> có cùng độ dài, sao cho <code>B[i] == A[(i+x) % A.length]</code> với mọi chỉ số hợp lệ <code>i</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,4,5,1,2]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> [1,2,3,4,5] là mảng đã sắp xếp ban đầu.
Có thể xoay mảng đi x = 2 vị trí để bắt đầu từ phần tử có giá trị 3: [3,4,5,1,2].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,1,3,4]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không có mảng đã sắp xếp nào có thể tạo thành nums sau khi xoay.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> [1,2,3] là mảng đã sắp xếp ban đầu.
Có thể xoay mảng đi x = 0 vị trí (tức là không xoay) để tạo thành nums.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Single Pass

<!-- thinking:start -->

> **Tư duy**
>
> Mảng không giảm được xoay nhiều nhất một lần có nhiều nhất một điểm giảm trên vòng tròn ($nums[i-1]>nums[i]$, tính cả cặp nối vòng).
>
> Đếm các điểm giảm đó trong một lượt; mảng hợp lệ khi và chỉ khi số điểm giảm không vượt quá $1$.

<!-- thinking:end -->

Để thỏa mãn yêu cầu, trong mảng $\textit{nums}$ có nhiều nhất một phần tử lớn hơn phần tử kế tiếp, tức là $nums[i] \gt nums[i + 1]$. Nếu có nhiều hơn một phần tử như vậy thì không thể thu được mảng $\textit{nums}$ bằng phép xoay.

Lưu ý rằng phần tử sau phần tử cuối của mảng $\textit{nums}$ là phần tử đầu tiên của mảng $\textit{nums}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def check(self, nums: List[int]) -> bool:
        return sum(nums[i - 1] > x for i, x in enumerate(nums)) <= 1
```

#### Java

```java
class Solution {
    public boolean check(int[] nums) {
        int cnt = 0;
        for (int i = 0, n = nums.length; i < n; ++i) {
            if (nums[i] > nums[(i + 1) % n]) {
                ++cnt;
            }
        }
        return cnt <= 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool check(vector<int>& nums) {
        int cnt = 0;
        for (int i = 0, n = nums.size(); i < n; ++i) {
            cnt += nums[i] > (nums[(i + 1) % n]);
        }
        return cnt <= 1;
    }
};
```

#### Go

```go
func check(nums []int) bool {
	cnt := 0
	for i, x := range nums {
		if x > nums[(i+1)%len(nums)] {
			cnt++
		}
	}
	return cnt <= 1
}
```

#### TypeScript

```ts
function check(nums: number[]): boolean {
    const n = nums.length;
    return nums.reduce((cnt, x, i) => cnt + (x > nums[(i + 1) % n] ? 1 : 0), 0) <= 1;
}
```

#### Rust

```rust
impl Solution {
    pub fn check(nums: Vec<i32>) -> bool {
        let n = nums.len();
        let cnt = nums.iter().enumerate().fold(0, |cnt, (i, &x)| {
            cnt + if x > nums[(i + 1) % n] { 1 } else { 0 }
        });
        cnt <= 1
    }
}
```

#### C

```c
bool check(int* nums, int numsSize) {
    int cnt = 0;
    for (int i = 0; i < numsSize; i++) {
        if (nums[i] > nums[(i + 1) % numsSize]) {
            cnt++;
        }
    }
    return cnt <= 1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
