---
comments: true
difficulty: Easy
rating: 1235
source: Weekly Contest 278 Q1
tags:
    - Array
    - Hash Table
    - Sorting
    - Simulation
---

<!-- problem:start -->

# [2154. Keep Multiplying Found Values by Two](https://leetcode.com/problems/keep-multiplying-found-values-by-two)

[中文文档](/solution/2100-2199/2154.Keep%20Multiplying%20Found%20Values%20by%20Two/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>. Bạn cũng được cho một số nguyên <code>original</code>, là số đầu tiên cần tìm trong <code>nums</code>.</p>

<p>Sau đó, bạn thực hiện các bước sau:</p>

<ol>
	<li>Nếu tìm thấy <code>original</code> trong <code>nums</code>, <strong>nhân</strong> nó với hai (tức là đặt <code>original = 2 * original</code>).</li>
	<li>Nếu không, <strong>dừng</strong> quá trình.</li>
	<li><strong>Lặp lại</strong> quá trình này với số mới, miễn là bạn vẫn tìm thấy số đó.</li>
</ol>

<p>Trả về <em><strong>giá trị cuối cùng</strong> của </em><code>original</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,3,6,1,12], original = 3
<strong>Đầu ra:</strong> 24
<strong>Giải thích:</strong>
- Tìm thấy 3 trong nums. Nhân 3 với 2 để được 6.
- Tìm thấy 6 trong nums. Nhân 6 với 2 để được 12.
- Tìm thấy 12 trong nums. Nhân 12 với 2 để được 24.
- Không tìm thấy 24 trong nums. Vì vậy, trả về 24.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,7,9], original = 4
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong>
- Không tìm thấy 4 trong nums. Vì vậy, trả về 4.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i], original &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Trong khi $\textit{original}$ còn xuất hiện trong mảng, ta thay nó bằng hai lần giá trị hiện tại. Giá trị tăng nhanh, nên chỉ có một vài lần nhân được thực hiện. Hash set giúp mỗi lần kiểm tra sự tồn tại có độ phức tạp kỳ vọng là $O(1)$.
>
> Đưa $\textit{nums}$ vào một set và dịch trái $\textit{original}$ cho đến khi nó không còn xuất hiện.
>
> Giá trị cuối cùng đó chính là đáp án.

<!-- thinking:end -->

Ta sử dụng một hash table $\textit{s}$ để lưu tất cả các số trong mảng $\textit{nums}$.

Tiếp theo, bắt đầu từ $\textit{original}$, nếu $\textit{original}$ thuộc $\textit{s}$, ta nhân $\textit{original}$ với $2$ cho đến khi $\textit{original}$ không còn thuộc $\textit{s}$ nữa, rồi trả về $\textit{original}$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findFinalValue(self, nums: List[int], original: int) -> int:
        s = set(nums)
        while original in s:
            original <<= 1
        return original
```

#### Java

```java
class Solution {

    public int findFinalValue(int[] nums, int original) {
        Set<Integer> s = new HashSet<>();
        for (int num : nums) {
            s.add(num);
        }
        while (s.contains(original)) {
            original <<= 1;
        }
        return original;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findFinalValue(vector<int>& nums, int original) {
        unordered_set<int> s(nums.begin(), nums.end());
        while (s.contains(original)) {
            original <<= 1;
        }
        return original;
    }
};
```

#### Go

```go
func findFinalValue(nums []int, original int) int {
	s := map[int]bool{}
	for _, x := range nums {
		s[x] = true
	}
	for s[original] {
		original <<= 1
	}
	return original
}
```

#### TypeScript

```ts
function findFinalValue(nums: number[], original: number): number {
    const s: Set<number> = new Set([...nums]);
    while (s.has(original)) {
        original <<= 1;
    }
    return original;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_final_value(nums: Vec<i32>, original: i32) -> i32 {
        use std::collections::HashSet;
        let s: HashSet<i32> = nums.into_iter().collect();
        let mut original = original;
        while s.contains(&original) {
            original <<= 1;
        }
        original
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
