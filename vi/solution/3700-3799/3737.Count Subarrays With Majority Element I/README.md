---
comments: true
difficulty: Medium
rating: 1422
source: Biweekly Contest 169 Q2
tags:
    - Segment Tree
    - Array
    - Hash Table
    - Divide and Conquer
    - Counting
    - Prefix Sum
    - Merge Sort
---

<!-- problem:start -->

# [3737. Count Subarrays With Majority Element I](https://leetcode.com/problems/count-subarrays-with-majority-element-i)

[中文文档](/solution/3700-3799/3737.Count%20Subarrays%20With%20Majority%20Element%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>target</code>.</p>

<p>Trả về số lượng <strong><span data-keyword="subarray-nonempty">mảng con</span></strong> của <code>nums</code> mà trong đó <code>target</code> là <strong>phần tử chiếm đa số</strong>.</p>

<p><strong>Phần tử chiếm đa số</strong> của một mảng con là phần tử xuất hiện <strong>nhiều hơn một cách nghiêm ngặt</strong> <strong>một nửa</strong> số lần trong mảng con đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,2,3], target = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con hợp lệ có <code>target = 2</code> là phần tử chiếm đa số:</p>

<ul>
	<li><code>nums[1..1] = [2]</code></li>
	<li><code>nums[2..2] = [2]</code></li>
	<li><code>nums[1..2] = [2,2]</code></li>
	<li><code>nums[0..2] = [1,2,2]</code></li>
	<li><code>nums[1..3] = [2,2,3]</code></li>
</ul>

<p>Vậy có 5 mảng con như vậy.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,1,1], target = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">10</span></p>

<p><strong>Giải thích: </strong></p>

<p><strong>​​​​​​​</strong>Cả 10 mảng con đều có 1 là phần tử chiếm đa số.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3], target = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>target = 4</code> hoàn toàn không xuất hiện trong <code>nums</code>. Do đó, không thể có mảng con nào mà 4 là phần tử chiếm đa số. Vì vậy, đáp án là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>​​​​​​​9</sup></code></li>
	<li><code>1 &lt;= target &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Phần tử chiếm đa số nghĩa là $\textit{target}$ xuất hiện nhiều hơn một nửa độ dài mảng con. Với giới hạn đề bài, ta có thể liệt kê mọi mảng con: cố định đầu trái, duyệt đầu phải đồng thời đếm số lần xuất hiện của $\textit{target}$, rồi kiểm tra điều kiện $2\cdot\textit{cnt}>\textit{len}$.

<!-- thinking:end -->

Ta có thể liệt kê tất cả các mảng con và duy trì một bộ đếm $\textit{cnt}$ để ghi nhận số lần $\textit{target}$ xuất hiện trong mảng con, sau đó xác định liệu $\textit{target}$ có phải là phần tử chiếm đa số của mảng con đó hay không.

Cụ thể, ta liệt kê vị trí bắt đầu $i$ của mảng con trong phạm vi $[0, n-1]$, sau đó liệt kê vị trí kết thúc $j$ trong phạm vi $[i, n-1]$. Với mỗi mảng con $nums[i..j]$, ta cập nhật bộ đếm $\textit{cnt}$. Nếu $\textit{cnt} \times 2 > j - i + 1$, điều đó có nghĩa $\textit{target}$ là phần tử chiếm đa số của mảng con này, và ta tăng đáp án lên $1$.

Độ phức tạp thời gian là $O(n^2)$, còn độ phức tạp không gian là $O(1)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countMajoritySubarrays(self, nums: List[int], target: int) -> int:
        n = len(nums)
        ans = 0
        for i in range(n):
            cnt = 0
            for j in range(i, n):
                cnt += int(nums[j] == target)
                if cnt * 2 > j - i + 1:
                    ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int countMajoritySubarrays(int[] nums, int target) {
        int n = nums.length;
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int cnt = 0;
            for (int j = i; j < n; ++j) {
                cnt += nums[j] == target ? 1 : 0;
                if (cnt * 2 > j - i + 1) {
                    ++ans;
                }
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
    int countMajoritySubarrays(vector<int>& nums, int target) {
        int n = nums.size();
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int cnt = 0;
            for (int j = i; j < n; ++j) {
                cnt += nums[j] == target;
                if (cnt * 2 > j - i + 1) {
                    ++ans;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countMajoritySubarrays(nums []int, target int) (ans int) {
	n := len(nums)
	for i := range nums {
		cnt := 0
		for j := i; j < n; j++ {
			if nums[j] == target {
				cnt++
			}
			if k := j - i + 1; cnt*2 > k {
				ans++
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function countMajoritySubarrays(nums: number[], target: number): number {
    const n = nums.length;
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        let cnt: number = 0;
        for (let j = i; j < n; ++j) {
            const k = j - i + 1;
            cnt += nums[j] == target ? 1 : 0;
            if (cnt * 2 > k) {
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
    pub fn count_majority_subarrays(nums: Vec<i32>, target: i32) -> i32 {
        let n = nums.len();
        let mut ans = 0;

        for i in 0..n {
            let mut cnt = 0;
            for j in i..n {
                let k = (j - i + 1) as i32;
                if nums[j] == target {
                    cnt += 1;
                }
                if cnt * 2 > k {
                    ans += 1;
                }
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
