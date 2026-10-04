---
comments: true
difficulty: Medium
rating: 1524
source: Biweekly Contest 121 Q2
tags:
    - Bit Manipulation
    - Array
---

<!-- problem:start -->

# [2997. Minimum Number of Operations to Make Array XOR Equal to K](https://leetcode.com/problems/minimum-number-of-operations-to-make-array-xor-equal-to-k)

[中文文档](/solution/2900-2999/2997.Minimum%20Number%20of%20Operations%20to%20Make%20Array%20XOR%20Equal%20to%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums</code> và một số nguyên dương <code>k</code>.</p>

<p>Bạn có thể thực hiện thao tác sau trên mảng <strong>bất kỳ</strong> số lần:</p>

<ul>
	<li>Chọn <strong>bất kỳ</strong> phần tử nào của mảng và <strong>đảo</strong> một bit trong biểu diễn <strong>nhị phân</strong> của nó. Đảo một bit nghĩa là chuyển <code>0</code> thành <code>1</code> hoặc ngược lại.</li>
</ul>

<p>Trả về <em>số thao tác <strong>ít nhất</strong> cần thực hiện để tính phép </em><code>XOR</code><em> theo bit của <strong>tất cả</strong> phần tử trong mảng cuối cùng bằng </em><code>k</code>.</p>

<p><strong>Lưu ý</strong> rằng bạn có thể đảo các bit 0 ở đầu trong biểu diễn nhị phân của các phần tử. Ví dụ, với số <code>(101)<sub>2</sub></code>, bạn có thể đảo bit thứ tư và nhận được <code>(1101)<sub>2</sub></code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,1,3,4], k = 1
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta có thể thực hiện các thao tác sau:
- Chọn phần tử 2, có giá trị 3 == (011)<sub>2</sub>, đảo bit đầu tiên và nhận được (010)<sub>2</sub> == 2. nums trở thành [2,1,2,4].
- Chọn phần tử 0, có giá trị 2 == (010)<sub>2</sub>, đảo bit thứ ba và nhận được (110)<sub>2</sub> = 6. nums trở thành [6,1,2,4].
XOR của các phần tử trong mảng cuối cùng là (6 XOR 1 XOR 2 XOR 4) == 1 == k.
Có thể chứng minh rằng không thể làm cho XOR bằng k với ít hơn 2 thao tác.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,0,2,0], k = 0
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> XOR của các phần tử trong mảng là (2 XOR 0 XOR 2 XOR 0) == 0 == k. Vì vậy, không cần thực hiện thao tác nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
	<li><code>0 &lt;= k &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thao tác trên bit

<!-- thinking:start -->

> **Tư duy**
>
> Đảo một bit của một phần tử cũng làm đảo bit đó trong XOR tổng. Số lần đảo ít nhất là khoảng cách Hamming giữa $\bigoplus nums$ và $k$.
>
> Thực hiện XOR với $k$ rồi đếm các bit được đặt. Vì $n \le 10^5$, chỉ cần một lần reduce.

<!-- thinking:end -->

Ta có thể thực hiện phép XOR theo bit trên tất cả phần tử trong mảng $nums$. Số bit khác nhau giữa kết quả và biểu diễn nhị phân của $k$ chính là số thao tác ít nhất.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums: List[int], k: int) -> int:
        return reduce(xor, nums, k).bit_count()
```

#### Java

```java
class Solution {
    public int minOperations(int[] nums, int k) {
        for (int x : nums) {
            k ^= x;
        }
        return Integer.bitCount(k);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minOperations(vector<int>& nums, int k) {
        for (int x : nums) {
            k ^= x;
        }
        return __builtin_popcount(k);
    }
};
```

#### Go

```go
func minOperations(nums []int, k int) (ans int) {
	for _, x := range nums {
		k ^= x
	}
	return bits.OnesCount(uint(k))
}
```

#### TypeScript

```ts
function minOperations(nums: number[], k: number): number {
    for (const x of nums) {
        k ^= x;
    }
    return bitCount(k);
}

function bitCount(i: number): number {
    i = i - ((i >>> 1) & 0x55555555);
    i = (i & 0x33333333) + ((i >>> 2) & 0x33333333);
    i = (i + (i >>> 4)) & 0x0f0f0f0f;
    i = i + (i >>> 8);
    i = i + (i >>> 16);
    return i & 0x3f;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
