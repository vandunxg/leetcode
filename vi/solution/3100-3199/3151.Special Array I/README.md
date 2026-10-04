---
comments: true
difficulty: Easy
rating: 1152
source: Weekly Contest 398 Q1
tags:
    - Array
---

<!-- problem:start -->

# [3151. Special Array I](https://leetcode.com/problems/special-array-i)

[中文文档](/solution/3100-3199/3151.Special%20Array%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Một mảng được gọi là <strong>đặc biệt</strong> nếu <em>tính chẵn lẻ</em> của mọi cặp phần tử liền kề là khác nhau. Nói cách khác, một phần tử trong mỗi cặp <strong>phải</strong> là số chẵn và phần tử còn lại <strong>phải</strong> là số lẻ.</p>

<p>Bạn được cho một mảng số nguyên <code>nums</code>. Trả về <code>true</code> nếu <code>nums</code> là mảng <strong>đặc biệt</strong>, ngược lại trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chỉ có một phần tử. Vì vậy, đáp án là <code>true</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,1,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có đúng hai cặp: <code>(2,1)</code> và <code>(1,4)</code>, cả hai đều chứa các số có tính chẵn lẻ khác nhau. Vì vậy, đáp án là <code>true</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,3,1,6]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>nums[1]</code> và <code>nums[2]</code> đều là số lẻ. Vì vậy, đáp án là <code>false</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lần

<!-- thinking:start -->

> **Tư duy**
>
> Một mảng đặc biệt cần các phần tử liền kề có tính chẵn lẻ trái ngược nhau. Chỉ cần một cặp có cùng tính chẵn lẻ là mảng không thỏa mãn.
>
> Chỉ cần duyệt qua các phần tử liền kề một lần với độ dài mảng đã cho.
>
> Trả về việc mọi cặp phần tử liền kề có phần dư khác nhau khi chia cho $2$ hay không.

<!-- thinking:end -->

Ta duyệt mảng từ trái sang phải. Với mỗi cặp phần tử liền kề, nếu chúng có cùng tính chẵn lẻ thì mảng không phải là mảng đặc biệt và trả về `false`; ngược lại, mảng là mảng đặc biệt và trả về `true`.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isArraySpecial(self, nums: List[int]) -> bool:
        return all(a % 2 != b % 2 for a, b in pairwise(nums))
```

#### Java

```java
class Solution {
    public boolean isArraySpecial(int[] nums) {
        for (int i = 1; i < nums.length; ++i) {
            if (nums[i] % 2 == nums[i - 1] % 2) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isArraySpecial(vector<int>& nums) {
        for (int i = 1; i < nums.size(); ++i) {
            if (nums[i] % 2 == nums[i - 1] % 2) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func isArraySpecial(nums []int) bool {
	for i, x := range nums[1:] {
		if x%2 == nums[i]%2 {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function isArraySpecial(nums: number[]): boolean {
    for (let i = 1; i < nums.length; ++i) {
        if (nums[i] % 2 === nums[i - 1] % 2) {
            return false;
        }
    }
    return true;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
