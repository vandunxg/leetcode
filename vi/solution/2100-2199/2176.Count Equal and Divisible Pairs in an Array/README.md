---
comments: true
difficulty: Easy
rating: 1215
source: Biweekly Contest 72 Q1
tags:
    - Array
---

<!-- problem:start -->

# [2176. Count Equal and Divisible Pairs in an Array](https://leetcode.com/problems/count-equal-and-divisible-pairs-in-an-array)

[中文文档](/solution/2100-2199/2176.Count%20Equal%20and%20Divisible%20Pairs%20in%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

Cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums</code> với độ dài <code>n</code> và một số nguyên <code>k</code>, hãy trả về <em><strong>số lượng cặp</strong></em> <code>(i, j)</code> <em>thỏa mãn</em> <code>0 &lt;= i &lt; j &lt; n</code>, <em>sao cho</em> <code>nums[i] == nums[j]</code> <em>và</em> <code>(i * j)</code> <em>chia hết cho</em> <code>k</code>.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,1,2,2,2,1,3], k = 2
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong>
Có 4 cặp thỏa mãn tất cả các yêu cầu:
- nums[0] == nums[6], và 0 * 6 == 0, chia hết cho 2.
- nums[2] == nums[3], và 2 * 3 == 6, chia hết cho 2.
- nums[2] == nums[4], và 2 * 4 == 8, chia hết cho 2.
- nums[3] == nums[4], và 3 * 4 == 12, chia hết cho 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4], k = 1
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Vì không có giá trị nào trong nums lặp lại, không có cặp (i,j) nào thỏa mãn tất cả các yêu cầu.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i], k &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các cặp $i<j$ có giá trị bằng nhau và $i\cdot j$ chia hết cho $k$. Vì $n\le 100$, ta có thể dùng hai vòng lặp lồng nhau.
>
> Cố định $j$ rồi duyệt qua các $i$ đứng trước nó, kiểm tra điều kiện bằng nhau và tích theo modulo.
>
> Không cần thêm cấu trúc dữ liệu để lưu chỉ số.

<!-- thinking:end -->

Trước tiên, chúng ta duyệt chỉ số $j$ trong đoạn $[0, n)$, sau đó duyệt chỉ số $i$ trong đoạn $[0, j)$. Ta đếm số cặp thỏa mãn $\textit{nums}[i] = \textit{nums}[j]$ và $(i \times j) \bmod k = 0$.

Độ phức tạp thời gian là $O(n^2)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countPairs(self, nums: List[int], k: int) -> int:
        ans = 0
        for j, y in enumerate(nums):
            for i, x in enumerate(nums[:j]):
                ans += int(x == y and i * j % k == 0)
        return ans
```

#### Java

```java
class Solution {
    public int countPairs(int[] nums, int k) {
        int ans = 0;
        for (int j = 1; j < nums.length; ++j) {
            for (int i = 0; i < j; ++i) {
                ans += nums[i] == nums[j] && (i * j % k) == 0 ? 1 : 0;
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
    int countPairs(vector<int>& nums, int k) {
        int ans = 0;
        for (int j = 1; j < nums.size(); ++j) {
            for (int i = 0; i < j; ++i) {
                ans += nums[i] == nums[j] && (i * j % k) == 0;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countPairs(nums []int, k int) (ans int) {
	for j, y := range nums {
		for i, x := range nums[:j] {
			if x == y && (i*j%k) == 0 {
				ans++
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function countPairs(nums: number[], k: number): number {
    let ans = 0;
    for (let j = 1; j < nums.length; ++j) {
        for (let i = 0; i < j; ++i) {
            if (nums[i] === nums[j] && (i * j) % k === 0) {
                ++ans;
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_pairs(nums: Vec<i32>, k: i32) -> i32 {
        let mut ans = 0;
        for j in 1..nums.len() {
            for (i, &x) in nums[..j].iter().enumerate() {
                if x == nums[j] && (i * j) as i32 % k == 0 {
                    ans += 1;
                }
            }
        }
        ans
    }
}
```

#### C

```c
int countPairs(int* nums, int numsSize, int k) {
    int ans = 0;
    for (int j = 1; j < numsSize; ++j) {
        for (int i = 0; i < j; ++i) {
            ans += (nums[i] == nums[j] && (i * j % k) == 0);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
