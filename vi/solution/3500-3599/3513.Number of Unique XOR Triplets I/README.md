---
comments: true
difficulty: Medium
rating: 1663
source: Biweekly Contest 154 Q2
tags:
    - Bit Manipulation
    - Array
    - Math
---

<!-- problem:start -->

# [3513. Number of Unique XOR Triplets I](https://leetcode.com/problems/number-of-unique-xor-triplets-i)

[中文文档](/solution/3500-3599/3513.Number%20of%20Unique%20XOR%20Triplets%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>, trong đó <code>nums</code> là một <strong><span data-keyword="permutation">hoán vị</span></strong> của các số trong phạm vi <code>[1, n]</code>.</p>

<p>Một <strong>bộ ba XOR</strong> được định nghĩa là XOR của ba phần tử <code>nums[i] XOR nums[j] XOR nums[k]</code> trong đó <code>i &lt;= j &lt;= k</code>.</p>

<p>Hãy trả về số lượng giá trị bộ ba XOR <strong>khác nhau</strong> từ tất cả các bộ ba <code>(i, j, k)</code> có thể có.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các giá trị bộ ba XOR có thể có là:</p>

<ul>
	<li><code>(0, 0, 0) &rarr; 1 XOR 1 XOR 1 = 1</code></li>
	<li><code>(0, 0, 1) &rarr; 1 XOR 1 XOR 2 = 2</code></li>
	<li><code>(0, 1, 1) &rarr; 1 XOR 2 XOR 2 = 1</code></li>
	<li><code>(1, 1, 1) &rarr; 2 XOR 2 XOR 2 = 2</code></li>
</ul>

<p>Các giá trị XOR khác nhau là <code>{1, 2}</code>, nên kết quả là 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,1,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các giá trị bộ ba XOR có thể có bao gồm:</p>

<ul>
	<li><code>(0, 0, 0) &rarr; 3 XOR 3 XOR 3 = 3</code></li>
	<li><code>(0, 0, 1) &rarr; 3 XOR 3 XOR 1 = 1</code></li>
	<li><code>(0, 0, 2) &rarr; 3 XOR 3 XOR 2 = 2</code></li>
	<li><code>(0, 1, 2) &rarr; 3 XOR 1 XOR 2 = 0</code></li>
</ul>

<p>Các giá trị XOR khác nhau là <code>{0, 1, 2, 3}</code>, nên kết quả là 4.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= n</code></li>
	<li><code>nums</code> là một hoán vị của các số nguyên từ <code>1</code> đến <code>n</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bit Manipulation

<!-- thinking:start -->

> **Tư duy**
>
> $\textit{nums}$ là một hoán vị của $[1,n]$ và các chỉ số có thể lặp lại, nên tập hợp các XOR của bộ ba chỉ phụ thuộc vào $n$. Việc liệt kê tất cả bộ ba có độ phức tạp bậc ba không phù hợp với giới hạn dữ liệu.
>
> Với $n \le 2$, đáp án bằng $n$. Với $n \ge 3$, các XOR có thể có lấp đầy đoạn $[0, 2^{\lfloor \log_2 n \rfloor + 1} - 1]$, có giá trị là $1 \ll \textit{bitLength}(n)$.

<!-- thinking:end -->

Vì $\textit{nums}$ là một hoán vị của $[1, n]$, các giá trị có sẵn được cố định là $\{1, 2, \ldots, n\}$. Do các chỉ số thỏa mãn $i \le j \le k$, cùng một chỉ số có thể được chọn nhiều lần, nên một bộ ba XOR tương đương với việc chọn ba số (có lặp) từ tập hợp này rồi lấy XOR của chúng.

Khi $n \le 2$, việc liệt kê cho thấy các đáp án lần lượt là $1$ ($n = 1$) và $2$ ($n = 2$), tức là đáp án bằng $n$.

Khi $n \ge 3$, có thể chứng minh rằng mọi kết quả XOR có thể có tạo thành chính xác đoạn $[0, 2^{k} - 1]$, trong đó $2^{k}$ là lũy thừa của $2$ nhỏ nhất và lớn hơn nghiêm ngặt $n$. Giá trị này cũng bằng $2^{\lfloor \log_2 n \rfloor + 1}$, có thể tính bằng hàm xác định độ dài bit của từng ngôn ngữ:

$$
\textit{ans} = 1 \ll \textit{bitLength}(n)
$$

Ví dụ, khi $n = 3$, $\textit{bitLength}(3) = 2$, nên đáp án là $4$, khớp với tập ví dụ $\{0, 1, 2, 3\}$.

Độ phức tạp thời gian là $O(1)$, và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def uniqueXorTriplets(self, nums: List[int]) -> int:
        n = len(nums)
        return n if n <= 2 else 1 << n.bit_length()
```

#### Java

```java
class Solution {
    public int uniqueXorTriplets(int[] nums) {
        int n = nums.length;
        return n <= 2 ? n : 1 << (32 - Integer.numberOfLeadingZeros(n));
    }
}
```

#### C++

```cpp
class Solution {
public:
    int uniqueXorTriplets(vector<int>& nums) {
        size_t n = nums.size();
        return n <= 2 ? n : 1 << bit_width(n);
    }
};
```

#### Go

```go
func uniqueXorTriplets(nums []int) int {
	n := len(nums)
	if n <= 2 {
		return n
	}
	return 1 << bits.Len(uint(n))
}
```

#### TypeScript

```ts
function uniqueXorTriplets(nums: number[]): number {
    const n = nums.length;
    if (n <= 2) {
        return n;
    }
    return 1 << (32 - Math.clz32(n));
}
```

#### Rust

```rust
impl Solution {
    pub fn unique_xor_triplets(nums: Vec<i32>) -> i32 {
        let length = nums.len() as i32;
        if length < 3 {
            return length;
        }
        1 << (length.ilog2() + 1)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
