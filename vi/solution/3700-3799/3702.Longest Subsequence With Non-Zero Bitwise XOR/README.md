---
comments: true
difficulty: Medium
rating: 1489
source: Weekly Contest 470 Q2
tags:
    - Bit Manipulation
    - Array
---

<!-- problem:start -->

# [3702. Longest Subsequence With Non-Zero Bitwise XOR](https://leetcode.com/problems/longest-subsequence-with-non-zero-bitwise-xor)

[中文文档](/solution/3700-3799/3702.Longest%20Subsequence%20With%20Non-Zero%20Bitwise%20XOR/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>.</p>

<p>Hãy trả về độ dài của <strong>dài nhất <span data-keyword="subsequence-array-nonempty">dãy con</span></strong> trong <code>nums</code> sao cho XOR bit của <strong>dãy con</strong> là <strong>khác 0</strong>. Nếu không tồn tại <strong>dãy con</strong> nào như vậy, trả về 0.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một dãy con dài nhất là <code>[2, 3]</code>. XOR bit được tính là <code>2 XOR 3 = 1</code>, khác 0.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Dãy con dài nhất là <code>[2, 3, 4]</code>. XOR bit được tính là <code>2 XOR 3 XOR 4 = 5</code>, khác 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bài toán mẹo

<!-- thinking:start -->

> **Tư duy**
>
> $n\le 10^5$ khiến việc liệt kê tất cả các dãy con trở nên bất khả thi. XOR của một dãy con chính là XOR của toàn bộ mảng sau khi xóa đi một số phần tử. Nếu XOR tổng đã khác $0$, toàn bộ mảng là lựa chọn tối ưu. Nếu mọi phần tử đều là $0$, không tồn tại XOR khác 0. Ngược lại, XOR tổng bằng 0 nhưng vẫn còn một giá trị khác 0, nên chỉ cần xóa một phần tử khác 0 để XOR trở thành khác 0. Chỉ cần một lần duyệt để tính XOR tổng và đếm số lượng số 0 là có thể quyết định ba trường hợp.

<!-- thinking:end -->

Nếu XOR bit của tất cả phần tử trong mảng khác 0, thì toàn bộ mảng chính là dãy con dài nhất cần tìm, với độ dài bằng độ dài mảng.

Nếu tất cả phần tử trong mảng đều bằng 0, thì không tồn tại dãy con nào có XOR bit khác 0, nên ta trả về $0$.

Trong các trường hợp còn lại, ta có thể xóa $1$ phần tử khác 0 khỏi mảng để XOR bit của các phần tử còn lại khác 0. Độ dài của dãy con dài nhất bằng độ dài mảng trừ 1.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestSubsequence(self, nums: List[int]) -> int:
        n = len(nums)
        xor = cnt0 = 0
        for x in nums:
            xor ^= x
            cnt0 += int(x == 0)
        if xor:
            return n
        if cnt0 == n:
            return 0
        return n - 1
```

#### Java

```java
class Solution {
    public int longestSubsequence(int[] nums) {
        int xor = 0, cnt0 = 0;
        int n = nums.length;
        for (int x : nums) {
            xor ^= x;
            cnt0 += x == 0 ? 1 : 0;
        }
        if (xor != 0) {
            return n;
        }
        return cnt0 == n ? 0 : n - 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestSubsequence(vector<int>& nums) {
        int xor_ = 0, cnt0 = 0;
        int n = nums.size();
        for (int x : nums) {
            xor_ ^= x;
            cnt0 += x == 0 ? 1 : 0;
        }
        if (xor_ != 0) {
            return n;
        }
        return cnt0 == n ? 0 : n - 1;
    }
};
```

#### Go

```go
func longestSubsequence(nums []int) int {
	var xor, cnt0 int
	for _, x := range nums {
		xor ^= x
		if x == 0 {
			cnt0++
		}
	}
	n := len(nums)
	if xor != 0 {
		return n
	}
	if cnt0 == n {
		return 0
	}
	return n - 1
}
```

#### TypeScript

```ts
function longestSubsequence(nums: number[]): number {
    let [xor, cnt0] = [0, 0];
    for (const x of nums) {
        xor ^= x;
        cnt0 += x === 0 ? 1 : 0;
    }
    const n = nums.length;
    if (xor) {
        return n;
    }
    if (cnt0 === n) {
        return 0;
    }
    return n - 1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
