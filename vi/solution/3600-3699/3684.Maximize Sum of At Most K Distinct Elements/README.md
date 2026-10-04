---
comments: true
difficulty: Easy
rating: 1298
source: Weekly Contest 467 Q2
tags:
    - Greedy
    - Array
    - Hash Table
    - Sorting
---

<!-- problem:start -->

# [3684. Maximize Sum of At Most K Distinct Elements](https://leetcode.com/problems/maximize-sum-of-at-most-k-distinct-elements)

[中文文档](/solution/3600-3699/3684.Maximize%20Sum%20of%20At%20Most%20K%20Distinct%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>dương</strong> <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Hãy chọn nhiều nhất <code>k</code> phần tử từ <code>nums</code> sao cho tổng của chúng là lớn nhất. Tuy nhiên, các số được chọn phải <strong>khác nhau</strong>.</p>

<p>Trả về một mảng chứa các số đã chọn theo thứ tự <strong>giảm dần nghiêm ngặt</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [84,93,100,77,90], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[100,93,90]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tổng lớn nhất là 283, đạt được khi chọn 93, 100 và 90. Sắp xếp chúng theo thứ tự giảm dần nghiêm ngặt thành <code>[100, 93, 90]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [84,93,100,77,93], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[100,93,84]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tổng lớn nhất là 277, đạt được khi chọn 84, 93 và 100. Sắp xếp chúng theo thứ tự giảm dần nghiêm ngặt thành <code>[100, 93, <span class="example-io">84</span>]</code>. Không thể chọn 93, 100 và 93 vì các số được chọn phải khác nhau.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,1,2,2,2], k = 6</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,1]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tổng lớn nhất là 3, đạt được khi chọn 1 và 2. Sắp xếp chúng theo thứ tự giảm dần nghiêm ngặt thành <code>[2, 1]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Chọn nhiều nhất $k$ giá trị khác nhau và đưa chúng ra theo thứ tự giảm dần nghiêm ngặt. Các giá trị khác nhau lớn nhất là lựa chọn tối ưu.
>
> Sắp xếp, duyệt từ phải sang trái, bỏ qua giá trị trùng với phần tử bên cạnh và thu thập cho đến khi chọn đủ $k$ giá trị.
>
> Thứ tự từ phải sang trái là giảm dần; bỏ qua các giá trị trùng nhau giúp các giá trị được chọn là khác nhau.

<!-- thinking:end -->

Trước tiên, chúng ta sắp xếp mảng $\textit{nums}$, sau đó duyệt từ cuối về đầu để chọn $k$ phần tử khác nhau lớn nhất. Vì yêu cầu kết quả phải theo thứ tự giảm dần nghiêm ngặt, chúng ta bỏ qua các phần tử trùng nhau trong quá trình chọn.

Độ phức tạp thời gian là $O(n \times \log n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Không tính phần không gian dùng cho đáp án, độ phức tạp không gian là $O(\log n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxKDistinct(self, nums: List[int], k: int) -> List[int]:
        nums.sort()
        n = len(nums)
        ans = []
        for i in range(n - 1, -1, -1):
            if i + 1 < n and nums[i] == nums[i + 1]:
                continue
            ans.append(nums[i])
            k -= 1
            if k == 0:
                break
        return ans
```

#### Java

```java
class Solution {
    public int[] maxKDistinct(int[] nums, int k) {
        Arrays.sort(nums);
        int n = nums.length;
        List<Integer> ans = new ArrayList<>();
        for (int i = n - 1; i >= 0; --i) {
            if (i + 1 < n && nums[i] == nums[i + 1]) {
                continue;
            }
            ans.add(nums[i]);
            if (--k == 0) {
                break;
            }
        }
        return ans.stream().mapToInt(x -> x).toArray();
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> maxKDistinct(vector<int>& nums, int k) {
        ranges::sort(nums);
        int n = nums.size();
        vector<int> ans;
        for (int i = n - 1; ~i; --i) {
            if (i + 1 < n && nums[i] == nums[i + 1]) {
                continue;
            }
            ans.push_back(nums[i]);
            if (--k == 0) {
                break;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxKDistinct(nums []int, k int) (ans []int) {
	slices.Sort(nums)
	n := len(nums)
	for i := n - 1; i >= 0; i-- {
		if i+1 < n && nums[i] == nums[i+1] {
			continue
		}
		ans = append(ans, nums[i])
		if k--; k == 0 {
			break
		}
	}
	return
}
```

#### TypeScript

```ts
function maxKDistinct(nums: number[], k: number): number[] {
    nums.sort((a, b) => a - b);
    const ans: number[] = [];
    const n = nums.length;
    for (let i = n - 1; ~i; --i) {
        if (i + 1 < n && nums[i] === nums[i + 1]) {
            continue;
        }
        ans.push(nums[i]);
        if (--k === 0) {
            break;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
