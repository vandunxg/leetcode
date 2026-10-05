---
comments: true
difficulty: Hard
rating: 2162
source: Weekly Contest 502 Q4
tags:
    - Array
    - Hash Table
    - Binary Search
    - Suffix Array
    - Hash Function
    - Rolling Hash
---

<!-- problem:start -->

# [3934. Smallest Unique Subarray](https://leetcode.com/problems/smallest-unique-subarray)

[中文文档](/solution/3900-3999/3934.Smallest%20Unique%20Subarray/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Hãy tìm độ dài <strong>nhỏ nhất</strong> của một <span data-keyword="subarray">mảng con</span> <strong>không</strong> <strong>giống hệt</strong> bất kỳ <strong>mảng con</strong> nào khác trong <code>nums</code>.</p>

<p>Trả về một số nguyên biểu thị <strong>độ dài nhỏ nhất có thể</strong> của <strong>mảng con</strong> như vậy.</p>

<p>Hai <strong>mảng con</strong> được xem là giống hệt nhau nếu chúng có cùng độ dài và các phần tử ở các vị trí tương ứng giống nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,3,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Mảng con độ dài 1: <code>[3]</code> &rarr; xuất hiện 3 lần</li>
	<li>Mảng con độ dài 2: <code>[3, 3]</code> &rarr; xuất hiện 2 lần</li>
	<li>Mảng con độ dài 3: <code>[3, 3, 3]</code> &rarr; xuất hiện 1 lần</li>
</ul>

<p>Mảng con <code>[3, 3, 3]</code> là duy nhất, nên độ dài mảng con duy nhất nhỏ nhất là 3.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,1,2,3,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con độ dài 1:</p>

<ul>
	<li><code>[2]</code> &rarr; xuất hiện 2 lần</li>
	<li><code>[1]</code> &rarr; xuất hiện 1 lần</li>
	<li><code>[3]</code> &rarr; xuất hiện 2 lần</li>
</ul>
Mảng con <code>[1]</code> là duy nhất, nên độ dài mảng con duy nhất nhỏ nhất là 1.</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,2,2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con độ dài 1:</p>

<ul>
	<li><code>[1]</code> &rarr; xuất hiện 3 lần</li>
	<li><code>[2]</code> &rarr; xuất hiện 2 lần</li>
</ul>

<p>Các mảng con độ dài 2:</p>

<ul>
	<li><code>[1, 1]</code> &rarr; xuất hiện 1 lần</li>
	<li><code>[1, 2]</code> &rarr; xuất hiện 1 lần</li>
	<li><code>[2, 2]</code> &rarr; xuất hiện 1 lần</li>
	<li><code>[2, 1]</code> &rarr; xuất hiện 1 lần</li>
</ul>
Có ít nhất một mảng con độ dài 2 là duy nhất, nên độ dài mảng con duy nhất nhỏ nhất là 2.</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Rolling Hash + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Nếu tồn tại một mảng con duy nhất có độ dài $L$, thì với mọi độ dài lớn hơn, ta cũng có một mảng con duy nhất bằng cách mở rộng mảng con đó. Vì vậy, độ dài nhỏ nhất của mảng con duy nhất có tính đơn điệu theo $L$ và có thể tìm bằng tìm kiếm nhị phân.
>
> Để kiểm tra một giá trị $\textit{mid}$, ta trượt mọi cửa sổ có độ dài đó và đếm các hash. Rolling hash giúp mỗi lần trượt có độ phức tạp $O(1)$, nên một lần kiểm tra tốn $O(n)$.
>
> Thực hiện $O(\log n)$ lần kiểm tra cho tổng độ phức tạp $O(n\log n)$.

<!-- thinking:end -->

Với $\textit{mid_len} = \frac{\textit{min_len} + \textit{max_len}}{2}$, đối với mỗi mảng con ứng viên có độ dài $\textit{mid_len}$, ta trượt một cửa sổ rolling hash qua tất cả các mảng con có độ dài đó và ghi nhận số lần xuất hiện của từng giá trị hash.

Nếu có giá trị hash xuất hiện đúng một lần, ta tìm được một mảng con duy nhất có độ dài $\textit{mid_len}$. Khi đó, ta có thể giảm $\textit{max_len}$ xuống $\textit{mid_len} - 1$.

Ngược lại, điều đó có nghĩa là không tồn tại mảng con duy nhất có độ dài đó, nên ta phải tăng $\textit{min_len}$ lên $\textit{mid_len} + 1$.

Cách này đúng vì một khi tồn tại mảng con duy nhất có độ dài $l$, mọi mảng con có độ dài $> l$ cũng chắc chắn tồn tại.

Độ phức tạp thời gian là $O(n \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài mảng ban đầu.

Ta thực hiện tổng cộng $O(\log n)$ lần tìm kiếm nhị phân, mỗi lần tốn $O(n)$ với rolling hash.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestUniqueSubarray(self, nums: list[int]) -> int:
        self.nums = nums

        self.base = 19
        self.modulo = 10**9 + 7
        self.powers = [1] * (len(nums) + 1)

        self.hash_values: dict[int, int] = dict()

        min_possible_len = len(nums)  # Base case.

        min_len, max_len = 1, len(nums)  # For binary search usage.

        while min_len <= max_len:
            mid_len = (min_len + max_len) // 2

            if self._check_uniqueness(mid_len):
                if mid_len < min_possible_len:
                    min_possible_len = mid_len
                max_len = mid_len - 1

            else:
                min_len = mid_len + 1

        return min_possible_len

    def _check_uniqueness(self, subarray_len: int) -> bool:
        # Only need to reset leftmost power to 1 before usage.
        self.powers[0] = 1  # Because powers are calculated by bottom-up.

        for idx in range(1, len(self.nums) + 1):
            self.powers[idx] = (self.powers[idx - 1] * self.base) % self.modulo

        current_hash = 0
        for idx in range(subarray_len):
            current_hash *= self.base
            current_hash += self.nums[idx]
            current_hash %= self.modulo

        self.hash_values.clear()  # Clear before usage.
        self.hash_values[current_hash] = 1

        for idx in range(1, len(self.nums) - subarray_len + 1):
            # Window shifts: deduct leftmost num's share from hash value.
            current_hash -= self.powers[subarray_len - 1] * self.nums[idx - 1]

            # Integrate newly added num's share into hash value.
            current_hash *= self.base
            current_hash += self.nums[idx + subarray_len - 1]
            current_hash %= self.modulo

            if current_hash not in self.hash_values.keys():
                self.hash_values.update({current_hash: 0})
            self.hash_values[current_hash] += 1

        return 1 in self.hash_values.values()
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
