---
comments: true
difficulty: Hard
rating: 2092
source: Weekly Contest 315 Q4
tags:
    - Queue
    - Array
    - Sliding Window
    - Monotonic Queue
---

<!-- problem:start -->

# [2444. Count Subarrays With Fixed Bounds](https://leetcode.com/problems/count-subarrays-with-fixed-bounds)

[中文文档](/solution/2400-2499/2444.Count%20Subarrays%20With%20Fixed%20Bounds/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và hai số nguyên <code>minK</code> và <code>maxK</code>.</p>

<p>Một <strong>mảng con có biên cố định</strong> của <code>nums</code> là mảng con thỏa mãn các điều kiện sau:</p>

<ul>
	<li>Giá trị <strong>nhỏ nhất</strong> trong mảng con bằng <code>minK</code>.</li>
	<li>Giá trị <strong>lớn nhất</strong> trong mảng con bằng <code>maxK</code>.</li>
</ul>

<p>Hãy trả về <em><strong>số lượng</strong> mảng con có biên cố định</em>.</p>

<p><strong>Mảng con</strong> là một phần <strong>liên tiếp</strong> của mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,5,2,7,5], minK = 1, maxK = 5
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Các mảng con có biên cố định là [1,3,5] và [1,3,5,2].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,1,1], minK = 1, maxK = 1
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> Mọi mảng con của nums đều là mảng con có biên cố định. Có 10 mảng con có thể tạo thành.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i], minK, maxK &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê đầu mút phải

<!-- thinking:start -->

> **Tư duy**
>
> Với $n\le 10^5$, một mảng con có biên cố định phải nằm trong $[\textit{minK},\textit{maxK}]$ và chứa cả hai giá trị biên. Với đầu mút phải $i$, đầu mút trái phải nằm sau chỉ số gần nhất nằm ngoài khoảng và không lớn hơn chỉ số gần nhất trong hai lần xuất hiện gần nhất của $\textit{minK}$ và $\textit{maxK}$.
>
> Ta lưu ba chỉ số đó là $k,j_1,j_2$ và cộng thêm $\max(0,\min(j_1,j_2)-k)$. Mỗi đầu mút phải chỉ cần $O(1)$ phép tính.

<!-- thinking:end -->

Theo mô tả bài toán, ta biết rằng mọi phần tử của một mảng con bị giới hạn đều nằm trong khoảng $[\textit{minK}, \textit{maxK}]$, giá trị nhỏ nhất phải là $\textit{minK}$, còn giá trị lớn nhất phải là $\textit{maxK}$.

Ta duyệt qua mảng $\textit{nums}$ và đếm số mảng con bị giới hạn có $\textit{nums}[i]$ làm đầu mút phải. Sau đó, ta cộng tất cả các số đếm lại.

Chi tiết triển khai như sau:

1. Duy trì chỉ số $k$ của phần tử gần nhất không nằm trong khoảng $[\textit{minK}, \textit{maxK}]$, khởi tạo bằng $-1$. Đầu mút trái của phần tử hiện tại $\textit{nums}[i]$ phải lớn hơn $k$.
2. Duy trì chỉ số gần nhất $j_1$ mà giá trị bằng $\textit{minK}$ và chỉ số gần nhất $j_2$ mà giá trị bằng $\textit{maxK}$, cả hai đều khởi tạo bằng $-1$. Đầu mút trái của phần tử hiện tại $\textit{nums}[i]$ phải nhỏ hơn hoặc bằng $\min(j_1, j_2)$.
3. Dựa trên các điều kiện trên, số mảng con bị giới hạn có phần tử hiện tại làm đầu mút phải là $\max\bigl(0,\ \min(j_1, j_2) - k\bigr)$. Cộng dồn tất cả các số này để thu được kết quả.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSubarrays(self, nums: List[int], minK: int, maxK: int) -> int:
        j1 = j2 = k = -1
        ans = 0
        for i, v in enumerate(nums):
            if v < minK or v > maxK:
                k = i
            if v == minK:
                j1 = i
            if v == maxK:
                j2 = i
            ans += max(0, min(j1, j2) - k)
        return ans
```

#### Java

```java
class Solution {
    public long countSubarrays(int[] nums, int minK, int maxK) {
        long ans = 0;
        int j1 = -1, j2 = -1, k = -1;
        for (int i = 0; i < nums.length; ++i) {
            if (nums[i] < minK || nums[i] > maxK) {
                k = i;
            }
            if (nums[i] == minK) {
                j1 = i;
            }
            if (nums[i] == maxK) {
                j2 = i;
            }
            ans += Math.max(0, Math.min(j1, j2) - k);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long countSubarrays(vector<int>& nums, int minK, int maxK) {
        long long ans = 0;
        int j1 = -1, j2 = -1, k = -1;
        for (int i = 0; i < static_cast<int>(nums.size()); ++i) {
            if (nums[i] < minK || nums[i] > maxK) {
                k = i;
            }
            if (nums[i] == minK) {
                j1 = i;
            }
            if (nums[i] == maxK) {
                j2 = i;
            }
            ans += max(0, min(j1, j2) - k);
        }
        return ans;
    }
};
```

#### Go

```go
func countSubarrays(nums []int, minK int, maxK int) int64 {
	ans := 0
	j1, j2, k := -1, -1, -1
	for i, v := range nums {
		if v < minK || v > maxK {
			k = i
		}
		if v == minK {
			j1 = i
		}
		if v == maxK {
			j2 = i
		}
		ans += max(0, min(j1, j2)-k)
	}
	return int64(ans)
}
```

#### TypeScript

```ts
function countSubarrays(nums: number[], minK: number, maxK: number): number {
    let ans = 0;
    let [j1, j2, k] = [-1, -1, -1];
    for (let i = 0; i < nums.length; ++i) {
        if (nums[i] < minK || nums[i] > maxK) k = i;
        if (nums[i] === minK) j1 = i;
        if (nums[i] === maxK) j2 = i;
        ans += Math.max(0, Math.min(j1, j2) - k);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_subarrays(nums: Vec<i32>, min_k: i32, max_k: i32) -> i64 {
        let mut ans: i64 = 0;
        let mut j1: i64 = -1;
        let mut j2: i64 = -1;
        let mut k: i64 = -1;
        for (i, &v) in nums.iter().enumerate() {
            let i = i as i64;
            if v < min_k || v > max_k {
                k = i;
            }
            if v == min_k {
                j1 = i;
            }
            if v == max_k {
                j2 = i;
            }
            let m = j1.min(j2);
            if m > k {
                ans += m - k;
            }
        }
        ans
    }
}
```

#### C

```c
long long countSubarrays(int* nums, int numsSize, int minK, int maxK) {
    long long ans = 0;
    int j1 = -1, j2 = -1, k = -1;
    for (int i = 0; i < numsSize; ++i) {
        if (nums[i] < minK || nums[i] > maxK) k = i;
        if (nums[i] == minK) j1 = i;
        if (nums[i] == maxK) j2 = i;
        int m = j1 < j2 ? j1 : j2;
        if (m > k) ans += (long long) (m - k);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
