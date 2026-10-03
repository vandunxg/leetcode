---
comments: true
difficulty: Medium
rating: 1416
source: Weekly Contest 296 Q2
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [2294. Partition Array Such That Maximum Difference Is K](https://leetcode.com/problems/partition-array-such-that-maximum-difference-is-k)

[中文文档](/solution/2200-2299/2294.Partition%20Array%20Such%20That%20Maximum%20Difference%20Is%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>. Bạn có thể chia <code>nums</code> thành một hoặc nhiều <strong>dãy con</strong> sao cho mỗi phần tử trong <code>nums</code> xuất hiện <strong>chính xác</strong> trong một dãy con.</p>

<p>Hãy trả về <em><strong>số lượng nhỏ nhất</strong> các dãy con cần thiết sao cho hiệu giữa giá trị lớn nhất và nhỏ nhất trong mỗi dãy con <strong>không vượt quá</strong> </em><code>k</code><em>.</em></p>

<p>Một <strong>dãy con</strong> là một dãy có thể thu được từ một dãy khác bằng cách xóa một số phần tử hoặc không xóa phần tử nào mà không thay đổi thứ tự của các phần tử còn lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,6,1,2,5], k = 2
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Ta có thể chia nums thành hai dãy con [3,1,2] và [6,5].
Hiệu giữa giá trị lớn nhất và nhỏ nhất trong dãy con đầu tiên là 3 - 1 = 2.
Hiệu giữa giá trị lớn nhất và nhỏ nhất trong dãy con thứ hai là 6 - 5 = 1.
Vì đã tạo ra hai dãy con, ta trả về 2. Có thể chứng minh rằng 2 là số lượng dãy con nhỏ nhất cần thiết.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3], k = 1
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Ta có thể chia nums thành hai dãy con [1,2] và [3].
Hiệu giữa giá trị lớn nhất và nhỏ nhất trong dãy con đầu tiên là 2 - 1 = 1.
Hiệu giữa giá trị lớn nhất và nhỏ nhất trong dãy con thứ hai là 3 - 3 = 0.
Vì đã tạo ra hai dãy con, ta trả về 2. Lưu ý rằng một lời giải tối ưu khác là chia thành hai dãy con [1] và [2,3].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,2,4,5], k = 0
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Ta có thể chia nums thành ba dãy con [2,2], [4] và [5].
Hiệu giữa giá trị lớn nhất và nhỏ nhất trong dãy con đầu tiên là 2 - 2 = 0.
Hiệu giữa giá trị lớn nhất và nhỏ nhất trong dãy con thứ hai là 4 - 4 = 0.
Hiệu giữa giá trị lớn nhất và nhỏ nhất trong dãy con thứ ba là 5 - 5 = 0.
Vì đã tạo ra ba dãy con, ta trả về 3. Có thể chứng minh rằng 3 là số lượng dãy con nhỏ nhất cần thiết.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= k &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Ta chia thành các dãy con chứ không phải các mảng con; một nhóm hợp lệ khi $\max-\min\le k$. Vì $n \le 10^5$, trước tiên ta sắp xếp mảng để các giá trị gần nhau được xếp cùng nhóm. Một nhóm bắt đầu từ giá trị nhỏ nhất hiện tại $a$ và nhóm mới bắt đầu khi $b-a>k$.
>
> Sau khi sắp xếp, chỉ cần quét một lần, bắt đầu với một nhóm, là dùng ít nhóm nhất có thể vì mỗi nhóm được mở rộng hết mức có thể.

<!-- thinking:end -->

Đề bài yêu cầu chia thành các dãy con chứ không phải các mảng con, nên các phần tử trong một dãy con có thể không liên tiếp. Ta có thể sắp xếp mảng $\textit{nums}$. Giả sử phần tử đầu tiên của dãy con hiện tại là $a$, khi đó hiệu giữa giá trị lớn nhất và nhỏ nhất trong dãy con sẽ không vượt quá $k$. Vì vậy, ta có thể duyệt qua mảng $\textit{nums}$. Nếu hiệu giữa phần tử hiện tại $b$ và $a$ lớn hơn $k$, ta cập nhật $a$ thành $b$ và tăng số lượng dãy con lên 1. Sau khi duyệt xong, ta nhận được số lượng dãy con nhỏ nhất; lưu ý rằng số lượng dãy con ban đầu là $1$.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(\log n)$. Trong đó, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def partitionArray(self, nums: List[int], k: int) -> int:
        nums.sort()
        ans, a = 1, nums[0]
        for b in nums:
            if b - a > k:
                a = b
                ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int partitionArray(int[] nums, int k) {
        Arrays.sort(nums);
        int ans = 1, a = nums[0];
        for (int b : nums) {
            if (b - a > k) {
                a = b;
                ++ans;
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
    int partitionArray(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        int ans = 1, a = nums[0];
        for (int& b : nums) {
            if (b - a > k) {
                a = b;
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func partitionArray(nums []int, k int) int {
	sort.Ints(nums)
	ans, a := 1, nums[0]
	for _, b := range nums {
		if b-a > k {
			a = b
			ans++
		}
	}
	return ans
}
```

#### TypeScript

```ts
function partitionArray(nums: number[], k: number): number {
    nums.sort((a, b) => a - b);
    let ans = 1;
    let a = nums[0];
    for (const b of nums) {
        if (b - a > k) {
            a = b;
            ++ans;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn partition_array(mut nums: Vec<i32>, k: i32) -> i32 {
        nums.sort();
        let mut ans = 1;
        let mut a = nums[0];

        for &b in nums.iter() {
            if b - a > k {
                a = b;
                ans += 1;
            }
        }

        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int PartitionArray(int[] nums, int k) {
        Array.Sort(nums);
        int ans = 1;
        int a = nums[0];

        foreach (int b in nums) {
            if (b - a > k) {
                a = b;
                ans++;
            }
        }

        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
