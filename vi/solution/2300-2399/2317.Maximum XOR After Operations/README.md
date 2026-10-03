---
comments: true
difficulty: Medium
rating: 1678
source: Biweekly Contest 81 Q3
tags:
    - Bit Manipulation
    - Array
    - Math
---

<!-- problem:start -->

# [2317. Maximum XOR After Operations](https://leetcode.com/problems/maximum-xor-after-operations)

[中文文档](/solution/2300-2399/2317.Maximum%20XOR%20After%20Operations/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>0</strong>. Trong một thao tác, chọn <strong>bất kỳ</strong> số nguyên không âm <code>x</code> và một chỉ số <code>i</code>, sau đó <strong>cập nhật</strong> <code>nums[i]</code> thành <code>nums[i] AND (nums[i] XOR x)</code>.</p>

<p>Lưu ý rằng <code>AND</code> là phép AND bit và <code>XOR</code> là phép XOR bit.</p>

<p>Trả về <em><strong>giá trị XOR bit lớn nhất</strong> có thể có của tất cả các phần tử trong </em><code>nums</code><em> sau khi thực hiện thao tác <strong>một số lần bất kỳ</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,2,4,6]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Áp dụng thao tác với x = 4 và i = 3, num[3] = 6 AND (6 XOR 4) = 6 AND 2 = 2.
Bây giờ, nums = [3, 2, 4, 2] và XOR bit của tất cả các phần tử = 3 XOR 2 XOR 4 XOR 2 = 7.
Có thể chứng minh rằng 7 là XOR bit lớn nhất có thể đạt được.
Lưu ý rằng có thể sử dụng các thao tác khác để đạt được XOR bit bằng 7.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,9,2]
<strong>Đầu ra:</strong> 11
<strong>Giải thích:</strong> Không thực hiện thao tác nào.
XOR bit của tất cả các phần tử = 1 XOR 2 XOR 3 XOR 9 XOR 2 = 11.
Có thể chứng minh rằng 11 là XOR bit lớn nhất có thể đạt được.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>8</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Một thao tác có thể chuyển một số bit $1$ của $nums[i]$ thành $0$, nhưng không thể tạo ra bit $1$. Với $n \le 10^5$, ta cần một nhận xét có thể xử lý tuyến tính.
>
> XOR lớn nhất có một bit được bật khi và chỉ khi đã có một phần tử chứa bit đó (ta có thể giữ lại đúng một phần tử). Vì vậy, đáp án là phép OR bit của toàn bộ mảng.

<!-- thinking:end -->

Trong một thao tác, ta có thể cập nhật $\textit{nums}[i]$ thành $\textit{nums}[i] \text{ AND } (\textit{nums}[i] \text{ XOR } x)$. Vì $x$ có thể là bất kỳ số nguyên không âm nào, kết quả của $\textit{nums}[i] \oplus x$ có thể là bất kỳ giá trị nào. Bằng cách thực hiện phép AND bit với $\textit{nums}[i]$, ta có thể chuyển một số bit $1$ trong biểu diễn nhị phân của $\textit{nums}[i]$ thành $0$.

Bài toán yêu cầu tìm tổng XOR bit lớn nhất của tất cả các phần tử trong $\textit{nums}$. Đối với một bit nhị phân, chỉ cần có một phần tử trong $\textit{nums}$ có bit tương ứng bằng $1$ thì đóng góp của bit này vào tổng XOR bit lớn nhất sẽ là $1$. Vì vậy, đáp án là kết quả của phép OR bit trên tất cả các phần tử trong $\textit{nums}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumXOR(self, nums: List[int]) -> int:
        return reduce(or_, nums)
```

#### Java

```java
class Solution {
    public int maximumXOR(int[] nums) {
        int ans = 0;
        for (int x : nums) {
            ans |= x;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumXOR(vector<int>& nums) {
        int ans = 0;
        for (int& x : nums) {
            ans |= x;
        }
        return ans;
    }
};
```

#### Go

```go
func maximumXOR(nums []int) (ans int) {
	for _, x := range nums {
		ans |= x
	}
	return
}
```

#### TypeScript

```ts
function maximumXOR(nums: number[]): number {
    let ans = 0;
    for (const x of nums) {
        ans |= x;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
