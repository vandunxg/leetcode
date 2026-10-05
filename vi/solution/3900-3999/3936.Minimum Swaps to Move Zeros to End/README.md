---
comments: true
difficulty: Easy
rating: 1346
source: Biweekly Contest 183 Q1
tags:
    - Array
    - Two Pointers
---

<!-- problem:start -->

# [3936. Minimum Swaps to Move Zeros to End](https://leetcode.com/problems/minimum-swaps-to-move-zeros-to-end)

[中文文档](/solution/3900-3999/3936.Minimum%20Swaps%20to%20Move%20Zeros%20to%20End/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Trong một thao tác, bạn có thể chọn hai chỉ số <code>i</code> và <code>j</code> <strong>khác nhau</strong> bất kỳ, rồi hoán đổi <code>nums[i]</code> và <code>nums[j]</code>.</p>

<p>Trả về một số nguyên biểu thị số thao tác <strong>ít nhất</strong> cần thực hiện để chuyển tất cả số 0 về cuối mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [0,1,0,3,12]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta thực hiện các thao tác hoán đổi sau:</p>

<ul>
	<li>Hoán đổi <code>nums[0]</code> và <code>nums[3]</code>, thu được <code>nums = [3, 1, 0, 0, 12]</code>.</li>
	<li>Hoán đổi <code>nums[2]</code> và <code>nums[4]</code>, thu được <code>nums = [3, 1, 12, 0, 0]</code>.</li>
</ul>

<p>Vì vậy, đáp án là 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [0,1,0,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta thực hiện thao tác hoán đổi sau:</p>

<ul>
	<li>Hoán đổi <code>nums[0]</code> và <code>nums[3]</code>, thu được <code>nums = [2, 1, 0, 0]</code>.</li>
</ul>

<p>Vì vậy, đáp án là 1.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng đã thỏa mãn điều kiện. Vì vậy, không cần thực hiện thao tác hoán đổi nào.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Vì $n\le 100$, ta có thể mô phỏng việc hoán đổi một số 0 với một số khác 0 ở phía sau. Mục tiêu là đưa mọi số 0 về hậu tố; thứ tự giữa các số 0 hoặc giữa các số khác 0 không quan trọng.
>
> Dùng hai con trỏ từ hai đầu: con trỏ trái tìm số $0$, con trỏ phải tìm số khác 0, và mỗi cặp chưa vượt qua nhau tương ứng với một lần hoán đổi.
>
> Số lần hoán đổi chính là số cặp 0–khác 0 như vậy, có thể tính trong một lần duyệt.

<!-- thinking:end -->

Ta dùng hai con trỏ $i$ và $j$ lần lượt trỏ đến đầu và cuối mảng. Mỗi lần, ta tăng $i$ sang phải cho đến khi tìm thấy số 0, đồng thời giảm $j$ sang trái cho đến khi tìm thấy một số khác 0. Nếu $i < j$, ta hoán đổi hai phần tử và tăng đáp án lên 1. Ta lặp lại quá trình này cho đến khi $i \geq j$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumSwaps(self, nums: list[int]) -> int:
        ans = 0
        n = len(nums)
        i, j = 0, n - 1
        while i < j:
            while i < n and nums[i] != 0:
                i += 1
            while j and nums[j] == 0:
                j -= 1
            if i >= j:
                break
            ans += 1
            i += 1
            j -= 1
        return ans
```

#### Java

```java
class Solution {
    public int minimumSwaps(int[] nums) {
        int ans = 0;
        int n = nums.length;
        for (int i = 0, j = n - 1; i < j; ++i, --j) {
            while (i < n && nums[i] != 0) {
                ++i;
            }

            while (j > 0 && nums[j] == 0) {
                --j;
            }

            if (i >= j) {
                break;
            }

            ++ans;
        }

        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumSwaps(vector<int>& nums) {
        int ans = 0;
        int n = nums.size();
        for (int i = 0, j = n - 1; i < j; ++i, --j) {
            while (i < n && nums[i] != 0) {
                ++i;
            }

            while (j > 0 && nums[j] == 0) {
                --j;
            }

            if (i >= j) {
                break;
            }

            ++ans;
        }

        return ans;
    }
};
```

#### Go

```go
func minimumSwaps(nums []int) int {
	ans := 0
	n := len(nums)

	for i, j := 0, n-1; i < j; i, j = i+1, j-1 {
		for i < n && nums[i] != 0 {
			i++
		}

		for j > 0 && nums[j] == 0 {
			j--
		}

		if i >= j {
			break
		}

		ans++
	}

	return ans
}
```

#### TypeScript

```ts
function minimumSwaps(nums: number[]): number {
    let ans = 0;
    const n = nums.length;

    let i = 0;
    let j = n - 1;

    while (i < j) {
        while (i < n && nums[i] !== 0) {
            ++i;
        }

        while (j > 0 && nums[j] === 0) {
            --j;
        }

        if (i >= j) {
            break;
        }

        ++ans;
        ++i;
        --j;
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
