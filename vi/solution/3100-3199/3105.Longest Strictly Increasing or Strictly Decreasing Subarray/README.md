---
comments: true
difficulty: Easy
rating: 1217
source: Weekly Contest 392 Q1
tags:
    - Array
---

<!-- problem:start -->

# [3105. Longest Strictly Increasing or Strictly Decreasing Subarray](https://leetcode.com/problems/longest-strictly-increasing-or-strictly-decreasing-subarray)

[中文文档](/solution/3100-3199/3105.Longest%20Strictly%20Increasing%20or%20Strictly%20Decreasing%20Subarray/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>. Hãy trả về <em>độ dài của <strong>mảng con</strong> <span data-keyword="subarray-nonempty">dài nhất</span> của </em><code>nums</code><em>, trong đó mảng con là <strong><span data-keyword="strictly-increasing-array">tăng nghiêm ngặt</span></strong> hoặc <strong><span data-keyword="strictly-decreasing-array">giảm nghiêm ngặt</span></strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,4,3,3,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con tăng nghiêm ngặt của <code>nums</code> là <code>[1]</code>, <code>[2]</code>, <code>[3]</code>, <code>[3]</code>, <code>[4]</code> và <code>[1,4]</code>.</p>

<p>Các mảng con giảm nghiêm ngặt của <code>nums</code> là <code>[1]</code>, <code>[2]</code>, <code>[3]</code>, <code>[3]</code>, <code>[4]</code>, <code>[3,2]</code> và <code>[4,3]</code>.</p>

<p>Do đó, ta trả về <code>2</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,3,3,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con tăng nghiêm ngặt của <code>nums</code> là <code>[3]</code>, <code>[3]</code>, <code>[3]</code> và <code>[3]</code>.</p>

<p>Các mảng con giảm nghiêm ngặt của <code>nums</code> là <code>[3]</code>, <code>[3]</code>, <code>[3]</code> và <code>[3]</code>.</p>

<p>Do đó, ta trả về <code>1</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con tăng nghiêm ngặt của <code>nums</code> là <code>[3]</code>, <code>[2]</code> và <code>[1]</code>.</p>

<p>Các mảng con giảm nghiêm ngặt của <code>nums</code> là <code>[3]</code>, <code>[2]</code>, <code>[1]</code>, <code>[3,2]</code>, <code>[2,1]</code> và <code>[3,2,1]</code>.</p>

<p>Do đó, ta trả về <code>3</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 50</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai lượt duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Đáp án là độ dài của đoạn liên tiếp dài nhất tăng nghiêm ngặt hoặc giảm nghiêm ngặt. Việc theo dõi cả hai hướng trong một lượt duyệt đòi hỏi phải reset cẩn thận tại các điểm đổi hướng và rất dễ bị sai.
>
> Mảng đủ ngắn để thực hiện hai lần duyệt tuyến tính. Các đoạn tăng và giảm độc lập với nhau, nên ta có thể đo riêng rồi so sánh chúng.
>
> Duyệt một lần để tìm độ dài đoạn tăng nghiêm ngặt và một lần để tìm độ dài đoạn giảm nghiêm ngặt, reset về $1$ khi tính đơn điệu bị phá vỡ. Giá trị lớn hơn trong hai độ dài lớn nhất là đáp án.

<!-- thinking:end -->

Trước tiên, ta duyệt một lượt để tìm độ dài của mảng con tăng nghiêm ngặt dài nhất và cập nhật đáp án. Sau đó, ta thực hiện một lượt duyệt khác để tìm độ dài của mảng con giảm nghiêm ngặt dài nhất và tiếp tục cập nhật đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestMonotonicSubarray(self, nums: List[int]) -> int:
        ans = t = 1
        for i, x in enumerate(nums[1:]):
            if nums[i] < x:
                t += 1
                ans = max(ans, t)
            else:
                t = 1
        t = 1
        for i, x in enumerate(nums[1:]):
            if nums[i] > x:
                t += 1
                ans = max(ans, t)
            else:
                t = 1
        return ans
```

#### Java

```java
class Solution {
    public int longestMonotonicSubarray(int[] nums) {
        int ans = 1;
        for (int i = 1, t = 1; i < nums.length; ++i) {
            if (nums[i - 1] < nums[i]) {
                ans = Math.max(ans, ++t);
            } else {
                t = 1;
            }
        }
        for (int i = 1, t = 1; i < nums.length; ++i) {
            if (nums[i - 1] > nums[i]) {
                ans = Math.max(ans, ++t);
            } else {
                t = 1;
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
    int longestMonotonicSubarray(vector<int>& nums) {
        int ans = 1;
        for (int i = 1, t = 1; i < nums.size(); ++i) {
            if (nums[i - 1] < nums[i]) {
                ans = max(ans, ++t);
            } else {
                t = 1;
            }
        }
        for (int i = 1, t = 1; i < nums.size(); ++i) {
            if (nums[i - 1] > nums[i]) {
                ans = max(ans, ++t);
            } else {
                t = 1;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func longestMonotonicSubarray(nums []int) int {
	ans := 1
	t := 1
	for i, x := range nums[1:] {
		if nums[i] < x {
			t++
			ans = max(ans, t)
		} else {
			t = 1
		}
	}
	t = 1
	for i, x := range nums[1:] {
		if nums[i] > x {
			t++
			ans = max(ans, t)
		} else {
			t = 1
		}
	}
	return ans
}
```

#### TypeScript

```ts
function longestMonotonicSubarray(nums: number[]): number {
    const n = nums.length;
    let ans = 1;

    for (let i = 1, t1 = 1, t2 = 1; i < n; i++) {
        t1 = nums[i] > nums[i - 1] ? t1 + 1 : 1;
        t2 = nums[i] < nums[i - 1] ? t2 + 1 : 1;
        ans = Math.max(ans, t1, t2);
    }

    return ans;
}
```

#### JavaScript

```js
function longestMonotonicSubarray(nums) {
    const n = nums.length;
    let ans = 1;

    for (let i = 1, t1 = 1, t2 = 1; i < n; i++) {
        t1 = nums[i] > nums[i - 1] ? t1 + 1 : 1;
        t2 = nums[i] < nums[i - 1] ? t2 + 1 : 1;
        ans = Math.max(ans, t1, t2);
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
