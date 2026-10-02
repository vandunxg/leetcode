---
comments: true
difficulty: Medium
tags:
    - Bit Manipulation
    - Array
    - Math
---

<!-- problem:start -->

# [477. Total Hamming Distance](https://leetcode.com/problems/total-hamming-distance)

[中文文档](/solution/0400-0499/0477.Total%20Hamming%20Distance/README.md)

## Mô tả

<!-- description:start -->

<p><a href="https://en.wikipedia.org/wiki/Hamming_distance" target="_blank">Hamming distance</a> giữa hai số nguyên là số vị trí mà các bit tương ứng khác nhau.</p>

<p>Cho mảng số nguyên <code>nums</code>, hãy trả về <em>tổng <strong>Hamming distance</strong> giữa mọi cặp số nguyên trong</em> <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,14,2]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Ở dạng nhị phân, 4 là 0100, 14 là 1110 và 2 là 0010 (ở đây chỉ
hiển thị bốn bit cần xét).
Đáp án là:
HammingDistance(4, 14) + HammingDistance(4, 2) + HammingDistance(14, 2) = 2 + 2 + 2 = 6.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,14,4]
<strong>Đầu ra:</strong> 4
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li>Đáp án của đầu vào đã cho sẽ nằm trong phạm vi số nguyên <strong>32-bit</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Tính tổng Hamming distance của mọi cặp. XOR từng cặp sẽ tốn $O(n^2)$. Đóng góp của mỗi bit có thể tính độc lập.
>
> Xét bit $i$: nếu có $a$ số mang bit $1$ thì có $n-a$ số mang bit $0$, nên bit này đóng góp $a(n-a)$. Cộng đóng góp của cả $32$ bit.
>
> Khi tách theo từng bit, mỗi cặp khác nhau ở bit đó được tính đúng một lần; không cần liệt kê các cặp.

<!-- thinking:end -->

Ta xét lần lượt các bit trong khoảng $[0, 31]$. Với bit $i$ đang xét, đếm số phần tử có bit thứ $i$ bằng $1$, gọi số lượng này là $a$. Khi đó, số phần tử có bit thứ $i$ bằng $0$ là $b = n - a$, trong đó $n$ là độ dài mảng. Vì vậy, tổng Hamming distance ở bit thứ $i$ là $a \times b$. Cộng đóng góp của tất cả các bit để có đáp án.

Độ phức tạp thời gian là $O(n \times \log M)$, trong đó $n$ là độ dài mảng và $M$ là giá trị lớn nhất trong mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def totalHammingDistance(self, nums: List[int]) -> int:
        ans, n = 0, len(nums)
        for i in range(32):
            a = sum(x >> i & 1 for x in nums)
            b = n - a
            ans += a * b
        return ans
```

#### Java

```java
class Solution {
    public int totalHammingDistance(int[] nums) {
        int ans = 0, n = nums.length;
        for (int i = 0; i < 32; ++i) {
            int a = 0;
            for (int x : nums) {
                a += (x >> i & 1);
            }
            int b = n - a;
            ans += a * b;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int totalHammingDistance(vector<int>& nums) {
        int ans = 0, n = nums.size();
        for (int i = 0; i < 32; ++i) {
            int a = 0;
            for (int x : nums) {
                a += x >> i & 1;
            }
            int b = n - a;
            ans += a * b;
        }
        return ans;
    }
};
```

#### Go

```go
func totalHammingDistance(nums []int) (ans int) {
	for i := 0; i < 32; i++ {
		a := 0
		for _, x := range nums {
			a += x >> i & 1
		}
		b := len(nums) - a
		ans += a * b
	}
	return
}
```

#### TypeScript

```ts
function totalHammingDistance(nums: number[]): number {
    let ans = 0;
    for (let i = 0; i < 32; ++i) {
        const a = nums.filter(x => (x >> i) & 1).length;
        const b = nums.length - a;
        ans += a * b;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn total_hamming_distance(nums: Vec<i32>) -> i32 {
        let mut ans = 0;
        for i in 0..32 {
            let mut a = 0;
            for &x in nums.iter() {
                a += (x >> i) & 1;
            }
            let b = (nums.len() as i32) - a;
            ans += a * b;
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
