---
comments: true
difficulty: Easy
tags:
    - Array
    - Two Pointers
---

<!-- problem:start -->

# [283. Move Zeroes](https://leetcode.com/problems/move-zeroes)

[中文文档](/solution/0200-0299/0283.Move%20Zeroes/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>. Hãy chuyển tất cả số <code>0</code> về cuối mảng, đồng thời giữ nguyên thứ tự tương đối của các phần tử khác 0.</p>

<p><strong>Lưu ý</strong> rằng bạn phải thao tác trực tiếp trên mảng, không tạo bản sao.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> nums = [0,1,0,3,12]
<strong>Đầu ra:</strong> [1,3,12,0,0]
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> nums = [0]
<strong>Đầu ra:</strong> [0]
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>4</sup></code></li>
	<li><code>-2<sup>31</sup> &lt;= nums[i] &lt;= 2<sup>31</sup> - 1</code></li>
</ul>

<p>&nbsp;</p>
<strong>Câu hỏi mở rộng:</strong> Bạn có thể giảm thiểu tổng số thao tác không?

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Các số 0 phải được chuyển về cuối, còn các số khác 0 giữ nguyên thứ tự. Gọi $k$ là vị trí tiếp theo để đặt một số khác 0.
>
> Mỗi số khác 0 được swap với $nums[k]$, sau đó tăng $k$ lên một. Như vậy, đoạn đầu có độ dài $k$ sẽ chứa các số khác 0 theo đúng thứ tự ban đầu.

<!-- thinking:end -->

Ta dùng con trỏ $k$ để ghi lại vị trí cần chèn hiện tại, ban đầu $k = 0$.

Sau đó, ta duyệt mảng $\textit{nums}$. Mỗi khi gặp một số khác 0, ta swap số đó với $\textit{nums}[k]$ rồi tăng $k$ lên 1.

Bằng cách này, ta đảm bảo $k$ phần tử đầu tiên của $\textit{nums}$ đều khác 0 và giữ nguyên thứ tự tương đối như trong mảng ban đầu.

Độ phức tạp thời gian là $O(n)$, với $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        k = 0
        for i, x in enumerate(nums):
            if x:
                nums[k], nums[i] = nums[i], nums[k]
                k += 1
```

#### Java

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int k = 0, n = nums.length;
        for (int i = 0; i < n; ++i) {
            if (nums[i] != 0) {
                int t = nums[i];
                nums[i] = nums[k];
                nums[k++] = t;
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int k = 0, n = nums.size();
        for (int i = 0; i < n; ++i) {
            if (nums[i]) {
                swap(nums[i], nums[k++]);
            }
        }
    }
};
```

#### Go

```go
func moveZeroes(nums []int) {
	k := 0
	for i, x := range nums {
		if x != 0 {
			nums[i], nums[k] = nums[k], nums[i]
			k++
		}
	}
}
```

#### TypeScript

```ts
/**
 Do not return anything, modify nums in-place instead.
 */
function moveZeroes(nums: number[]): void {
    let k = 0;
    for (let i = 0; i < nums.length; ++i) {
        if (nums[i]) {
            [nums[i], nums[k]] = [nums[k], nums[i]];
            ++k;
        }
    }
}
```

#### Rust

```rust
impl Solution {
    pub fn move_zeroes(nums: &mut Vec<i32>) {
        let mut k = 0;
        let n = nums.len();
        for i in 0..n {
            if nums[i] != 0 {
                nums.swap(i, k);
                k += 1;
            }
        }
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
var moveZeroes = function (nums) {
    let k = 0;
    for (let i = 0; i < nums.length; ++i) {
        if (nums[i]) {
            [nums[i], nums[k]] = [nums[k], nums[i]];
            ++k;
        }
    }
};
```

#### C

```c
void moveZeroes(int* nums, int numsSize) {
    int k = 0;
    for (int i = 0; i < numsSize; ++i) {
        if (nums[i] != 0) {
            int t = nums[i];
            nums[i] = nums[k];
            nums[k++] = t;
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
