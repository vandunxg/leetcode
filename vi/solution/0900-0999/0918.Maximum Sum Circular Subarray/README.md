---
comments: true
difficulty: Medium
tags:
    - Queue
    - Array
    - Divide and Conquer
    - Dynamic Programming
    - Monotonic Queue
---

<!-- problem:start -->

# [918. Maximum Sum Circular Subarray](https://leetcode.com/problems/maximum-sum-circular-subarray)

[中文文档](/solution/0900-0999/0918.Maximum%20Sum%20Circular%20Subarray/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <strong>mảng số nguyên vòng tròn</strong> <code>nums</code> có độ dài <code>n</code>, hãy trả về <em>tổng lớn nhất có thể của một <strong>mảng con</strong> không rỗng trong </em><code>nums</code>.</p>

<p><strong>Mảng vòng tròn</strong> nghĩa là phần cuối mảng nối với phần đầu. Cụ thể, phần tử tiếp theo của <code>nums[i]</code> là <code>nums[(i + 1) % n]</code>, còn phần tử đứng trước <code>nums[i]</code> là <code>nums[(i - 1 + n) % n]</code>.</p>

<p>Mỗi phần tử trong bộ đệm cố định <code>nums</code> chỉ được xuất hiện nhiều nhất một lần trong <strong>mảng con</strong>. Cụ thể, với mảng con <code>nums[i], nums[i + 1], ..., nums[j]</code>, không tồn tại <code>i &lt;= k1</code>, <code>k2 &lt;= j</code> sao cho <code>k1 % n == k2 % n</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,-2,3,-2]
<strong>Output:</strong> 3
<strong>Giải thích:</strong> Mảng con [3] có tổng lớn nhất là 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [5,-3,5]
<strong>Output:</strong> 10
<strong>Giải thích:</strong> Mảng con [5,5] có tổng lớn nhất là 5 + 5 = 10.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> nums = [-3,-2,-3]
<strong>Output:</strong> -2
<strong>Giải thích:</strong> Mảng con [-2] có tổng lớn nhất là -2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>1 &lt;= n &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>-3 * 10<sup>4</sup> &lt;= nums[i] &lt;= 3 * 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duy trì Prefix Maximum

<!-- thinking:start -->

> **Tư duy**
>
> Mảng con có tổng lớn nhất trong mảng vòng tròn hoặc là một đoạn Kadane thông thường, hoặc bằng tổng toàn mảng trừ đi đoạn có tổng nhỏ nhất nằm ở giữa, tạo thành phần bù vòng qua hai đầu. Duyệt một lần, duy trì prefix minimum và prefix maximum để tính cả hai trường hợp, rồi trả về giá trị lớn hơn giữa $ans$ và $s-smi$.

<!-- thinking:end -->

Tổng lớn nhất của mảng con vòng tròn có thể thuộc một trong hai trường hợp:

- Trường hợp 1: Mảng con có tổng lớn nhất không đi qua phần nối vòng; đây là bài toán tìm tổng mảng con thông thường lớn nhất;
- Trường hợp 2: Mảng con có tổng lớn nhất đi qua phần nối vòng; ta có thể chuyển bài toán thành tổng toàn mảng trừ đi tổng mảng con nhỏ nhất.

Vì vậy, ta duy trì các biến sau:

- Prefix sum nhỏ nhất $pmi$, ban đầu bằng $0$;
- Prefix sum lớn nhất $pmx$, ban đầu bằng $-\infty$;
- Prefix sum $s$, ban đầu bằng $0$;
- Tổng mảng con nhỏ nhất $smi$, ban đầu bằng $\infty$;
- Đáp án $ans$, ban đầu bằng $-\infty$.

Tiếp theo, ta chỉ cần duyệt mảng $nums$. Với phần tử hiện tại $x$, thực hiện các cập nhật sau:

- Cập nhật prefix sum $s = s + x$;
- Cập nhật đáp án $ans = \max(ans, s - pmi)$, tương ứng với Trường hợp 1 (lấy prefix sum $s$ trừ prefix sum nhỏ nhất $pmi$ để thu được tổng mảng con lớn nhất);
- Cập nhật $smi = \min(smi, s - pmx)$, là tổng mảng con nhỏ nhất cho Trường hợp 2;
- Cập nhật $pmi = \min(pmi, s)$, là prefix sum nhỏ nhất;
- Cập nhật $pmx = \max(pmx, s)$, là prefix sum lớn nhất.

Sau khi duyệt xong, ta trả về giá trị lớn hơn giữa $ans$ và $s - smi$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSubarraySumCircular(self, nums: List[int]) -> int:
        pmi, pmx = 0, -inf
        ans, s, smi = -inf, 0, inf
        for x in nums:
            s += x
            ans = max(ans, s - pmi)
            smi = min(smi, s - pmx)
            pmi = min(pmi, s)
            pmx = max(pmx, s)
        return max(ans, s - smi)
```

#### Java

```java
class Solution {
    public int maxSubarraySumCircular(int[] nums) {
        final int inf = 1 << 30;
        int pmi = 0, pmx = -inf;
        int ans = -inf, s = 0, smi = inf;
        for (int x : nums) {
            s += x;
            ans = Math.max(ans, s - pmi);
            smi = Math.min(smi, s - pmx);
            pmi = Math.min(pmi, s);
            pmx = Math.max(pmx, s);
        }
        return Math.max(ans, s - smi);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxSubarraySumCircular(vector<int>& nums) {
        const int inf = 1 << 30;
        int pmi = 0, pmx = -inf;
        int ans = -inf, s = 0, smi = inf;
        for (int x : nums) {
            s += x;
            ans = max(ans, s - pmi);
            smi = min(smi, s - pmx);
            pmi = min(pmi, s);
            pmx = max(pmx, s);
        }
        return max(ans, s - smi);
    }
};
```

#### Go

```go
func maxSubarraySumCircular(nums []int) int {
	const inf = 1 << 30
	pmi, pmx := 0, -inf
	ans, s, smi := -inf, 0, inf
	for _, x := range nums {
		s += x
		ans = max(ans, s-pmi)
		smi = min(smi, s-pmx)
		pmi = min(pmi, s)
		pmx = max(pmx, s)
	}
	return max(ans, s-smi)
}
```

#### TypeScript

```ts
function maxSubarraySumCircular(nums: number[]): number {
    let [pmi, pmx] = [0, -Infinity];
    let [ans, s, smi] = [-Infinity, 0, Infinity];
    for (const x of nums) {
        s += x;
        ans = Math.max(ans, s - pmi);
        smi = Math.min(smi, s - pmx);
        pmi = Math.min(pmi, s);
        pmx = Math.max(pmx, s);
    }
    return Math.max(ans, s - smi);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
