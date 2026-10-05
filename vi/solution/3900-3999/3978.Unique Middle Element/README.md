---
comments: true
difficulty: Easy
rating: 1180
source: Biweekly Contest 186 Q1
tags:
    - Array
    - Counting
---

<!-- problem:start -->

# [3978. Unique Middle Element](https://leetcode.com/problems/unique-middle-element)

[中文文档](/solution/3900-3999/3978.Unique%20Middle%20Element/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có độ dài lẻ <code>n</code>.</p>

<p>Trả về <code>true</code> nếu phần tử ở giữa của <code>nums</code> xuất hiện <strong>chính xác</strong> một lần trong mảng. Nếu không, trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p>Phần tử ở giữa của <code>nums</code> là 2 và nó chỉ xuất hiện đúng một lần.</p>

<p>Vì vậy, đáp án là <code>true</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<p>Phần tử ở giữa của <code>nums</code> là 2 và nó xuất hiện hai lần.</p>

<p>Vì vậy, đáp án là <code>false</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 100</code></li>
	<li><code>n</code> là số lẻ.</li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Độ dài mảng là số lẻ nên chỉ số ở giữa là duy nhất. Giá trị ở giữa là duy nhất trong toàn mảng khi $\textit{count}$ trả về $1$.
>
> Vì $n\le 100$, chỉ cần đếm một lần theo thứ tự tuyến tính.

<!-- thinking:end -->

Ta lấy phần tử ở chỉ số giữa của mảng và đếm số lần nó xuất hiện. Nếu số lần là $1$, trả về $\textit{true}$; ngược lại trả về $\textit{false}$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isMiddleElementUnique(self, nums: list[int]) -> bool:
        return nums.count(nums[len(nums) // 2]) == 1
```

#### Java

```java
class Solution {
    public boolean isMiddleElementUnique(int[] nums) {
        int cnt = 0;
        for (int x : nums) {
            if (x == nums[nums.length / 2]) {
                ++cnt;
            }
        }
        return cnt == 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isMiddleElementUnique(vector<int>& nums) {
        int n = nums.size();
        int cnt = 0;
        for (int x : nums) {
            if (x == nums[n / 2]) {
                ++cnt;
            }
        }
        return cnt == 1;
    }
};
```

#### Go

```go
func isMiddleElementUnique(nums []int) bool {
	cnt := 0
	for _, x := range nums {
		if x == nums[len(nums)/2] {
			cnt++
		}
	}
	return cnt == 1
}
```

#### TypeScript

```ts
function isMiddleElementUnique(nums: number[]): boolean {
    let cnt: number = 0;
    for (const x of nums) {
        if (x === nums[nums.length >> 1]) {
            ++cnt;
        }
    }
    return cnt === 1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
