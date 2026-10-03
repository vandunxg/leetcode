---
comments: true
difficulty: Medium
rating: 1315
source: Biweekly Contest 83 Q2
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [2348. Number of Zero-Filled Subarrays](https://leetcode.com/problems/number-of-zero-filled-subarrays)

[中文文档](/solution/2300-2399/2348.Number%20of%20Zero-Filled%20Subarrays/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>, hãy trả về <em>số lượng <strong>mảng con</strong> chỉ gồm </em><code>0</code>.</p>

<p><strong>Mảng con</strong> là một dãy phần tử liên tiếp, không rỗng trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,0,0,2,0,0,4]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong>
Có 4 lần xuất hiện của [0] dưới dạng mảng con.
Có 2 lần xuất hiện của [0,0] dưới dạng mảng con.
Không có mảng con nào có kích thước lớn hơn 2 chỉ gồm 0. Vì vậy, ta trả về 6.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,0,0,2,0,0]
<strong>Đầu ra:</strong> 9
<strong>Giải thích:
</strong>Có 5 lần xuất hiện của [0] dưới dạng mảng con.
Có 3 lần xuất hiện của [0,0] dưới dạng mảng con.
Có 1 lần xuất hiện của [0,0,0] dưới dạng mảng con.
Không có mảng con nào có kích thước lớn hơn 3 chỉ gồm 0. Vì vậy, ta trả về 9.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,10,2019]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có mảng con nào chỉ gồm 0. Vì vậy, ta trả về 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt và đếm

<!-- thinking:start -->

> **Tư duy**
>
> Các mảng con chỉ gồm 0 nằm trong những đoạn liên tiếp toàn số 0. Vì $n \le 10^5$, ta đếm theo từng đoạn thay vì theo các điểm đầu cuối.
>
> Duy trì độ dài đoạn hiện tại trong biến $cnt$. Gặp số 0 thì tăng $cnt$ và cộng giá trị này vào đáp án (các mảng con kết thúc tại đây); gặp số khác 0 thì đặt lại $cnt$.

<!-- thinking:end -->

Ta duyệt mảng $\textit{nums}$ và dùng biến $\textit{cnt}$ để ghi nhận số lượng số $0$ liên tiếp hiện tại. Với phần tử hiện tại $x$, nếu $x$ bằng $0$ thì tăng $\textit{cnt}$ lên $1$. Khi đó, số lượng mảng con chỉ gồm 0 kết thúc tại $x$ chính là $\textit{cnt}$, và ta cộng giá trị này vào đáp án. Ngược lại, ta đặt $\textit{cnt}$ về $0$.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

Bài toán tương tự:

- [413. Các mảng con cấp số cộng](https://github.com/doocs/leetcode/blob/main/solution/0400-0499/0413.Arithmetic%20Slices/README_EN.md)
- [1513. Số lượng chuỗi chỉ gồm số 1](https://github.com/doocs/leetcode/blob/main/solution/1500-1599/1513.Number%20of%20Substrings%20With%20Only%201s/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def zeroFilledSubarray(self, nums: List[int]) -> int:
        ans = cnt = 0
        for x in nums:
            if x == 0:
                cnt += 1
                ans += cnt
            else:
                cnt = 0
        return ans
```

#### Java

```java
class Solution {
    public long zeroFilledSubarray(int[] nums) {
        long ans = 0;
        int cnt = 0;
        for (int x : nums) {
            if (x == 0) {
                ans += ++cnt;
            } else {
                cnt = 0;
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
    long long zeroFilledSubarray(vector<int>& nums) {
        long long ans = 0;
        int cnt = 0;
        for (int x : nums) {
            if (x == 0) {
                ans += ++cnt;
            } else {
                cnt = 0;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func zeroFilledSubarray(nums []int) (ans int64) {
	cnt := 0
	for _, x := range nums {
		if x == 0 {
			cnt++
			ans += int64(cnt)
		} else {
			cnt = 0
		}
	}
	return
}
```

#### TypeScript

```ts
function zeroFilledSubarray(nums: number[]): number {
    let [ans, cnt] = [0, 0];
    for (const x of nums) {
        if (!x) {
            ans += ++cnt;
        } else {
            cnt = 0;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn zero_filled_subarray(nums: Vec<i32>) -> i64 {
        let mut ans: i64 = 0;
        let mut cnt: i64 = 0;
        for x in nums {
            if x == 0 {
                cnt += 1;
                ans += cnt;
            } else {
                cnt = 0;
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
