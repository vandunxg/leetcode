---
comments: true
difficulty: Easy
rating: 1218
source: Weekly Contest 363 Q1
tags:
    - Bit Manipulation
    - Array
---

<!-- problem:start -->

# [2859. Sum of Values at Indices With K Set Bits](https://leetcode.com/problems/sum-of-values-at-indices-with-k-set-bits)

[中文文档](/solution/2800-2899/2859.Sum%20of%20Values%20at%20Indices%20With%20K%20Set%20Bits/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Trả về <em>một số nguyên biểu thị <strong>tổng</strong> các phần tử trong </em><code>nums</code><em> mà <strong>các chỉ số tương ứng</strong> có <strong>chính xác</strong> </em><code>k</code><em> bit 1 trong biểu diễn nhị phân.</em></p>

<p>Các <strong>bit 1</strong> trong một số nguyên là các chữ số <code>1</code> xuất hiện khi số đó được viết ở dạng nhị phân.</p>

<ul>
	<li>Ví dụ, biểu diễn nhị phân của <code>21</code> là <code>10101</code>, có <code>3</code> bit 1.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,10,1,5,2], k = 1
<strong>Đầu ra:</strong> 13
<strong>Giải thích:</strong> Biểu diễn nhị phân của các chỉ số là:
0 = 000<sub>2</sub>
1 = 001<sub>2</sub>
2 = 010<sub>2</sub>
3 = 011<sub>2</sub>
4 = 100<sub>2
</sub>Các chỉ số 1, 2 và 4 có k = 1 bit 1 trong biểu diễn nhị phân.
Do đó, đáp án là nums[1] + nums[2] + nums[4] = 13.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,3,2,1], k = 2
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Biểu diễn nhị phân của các chỉ số là:
0 = 00<sub>2</sub>
1 = 01<sub>2</sub>
2 = 10<sub>2</sub>
3 = 11<sub>2
</sub>Chỉ có chỉ số 3 có k = 2 bit 1 trong biểu diễn nhị phân.
Do đó, đáp án là nums[3] = 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= k &lt;= 10</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> $n$ đủ nhỏ để chúng ta có thể kiểm tra `bit_count` của từng chỉ số với $k$ và cộng các giá trị tương ứng.

<!-- thinking:end -->

Chúng ta duyệt trực tiếp từng chỉ số $i$ và kiểm tra xem số lượng $1$s trong biểu diễn nhị phân của nó có bằng $k$ hay không. Nếu có, chúng ta cộng phần tử tương ứng vào đáp án $ans$.

Sau khi kết thúc quá trình duyệt, chúng ta trả về đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$, trong đó $n$ là độ dài của mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumIndicesWithKSetBits(self, nums: List[int], k: int) -> int:
        return sum(x for i, x in enumerate(nums) if i.bit_count() == k)
```

#### Java

```java
class Solution {
    public int sumIndicesWithKSetBits(List<Integer> nums, int k) {
        int ans = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (Integer.bitCount(i) == k) {
                ans += nums.get(i);
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
    int sumIndicesWithKSetBits(vector<int>& nums, int k) {
        int ans = 0;
        for (int i = 0; i < nums.size(); ++i) {
            if (__builtin_popcount(i) == k) {
                ans += nums[i];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func sumIndicesWithKSetBits(nums []int, k int) (ans int) {
	for i, x := range nums {
		if bits.OnesCount(uint(i)) == k {
			ans += x
		}
	}
	return
}
```

#### TypeScript

```ts
function sumIndicesWithKSetBits(nums: number[], k: number): number {
    let ans = 0;
    for (let i = 0; i < nums.length; ++i) {
        if (bitCount(i) === k) {
            ans += nums[i];
        }
    }
    return ans;
}

function bitCount(n: number): number {
    let count = 0;
    while (n) {
        n &= n - 1;
        count++;
    }
    return count;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
