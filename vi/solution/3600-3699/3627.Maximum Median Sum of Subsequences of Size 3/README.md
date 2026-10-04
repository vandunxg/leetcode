---
comments: true
difficulty: Medium
rating: 1262
source: Weekly Contest 460 Q1
---

<!-- problem:start -->

# [3627. Maximum Median Sum of Subsequences of Size 3](https://leetcode.com/problems/maximum-median-sum-of-subsequences-of-size-3)

[中文文档](/solution/3600-3699/3627.Maximum%20Median%20Sum%20of%20Subsequences%20of%20Size%203/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài chia hết cho 3.</p>

<p>Bạn muốn làm cho mảng rỗng qua nhiều bước. Ở mỗi bước, bạn có thể chọn bất kỳ ba phần tử nào trong mảng, tính <strong>trung vị</strong> của chúng, rồi xóa các phần tử đã chọn khỏi mảng.</p>

<p><strong>Trung vị</strong> của một dãy có độ dài lẻ được định nghĩa là phần tử ở giữa dãy khi dãy được sắp xếp theo thứ tự không giảm.</p>

<p>Trả về <strong>tổng lớn nhất</strong> có thể đạt được của các trung vị được tính từ những phần tử đã chọn.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,1,3,2,1,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Ở bước đầu tiên, chọn các phần tử tại các chỉ số 2, 4 và 5, có trung vị là 3. Sau khi xóa các phần tử này, <code>nums</code> trở thành <code>[2, 1, 2]</code>.</li>
    <li>Ở bước thứ hai, chọn các phần tử tại các chỉ số 0, 1 và 2, có trung vị là 2. Sau khi xóa các phần tử này, <code>nums</code> trở thành rỗng.</li>
</ul>

<p>Do đó, tổng các trung vị là <code>3 + 2 = 5</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,10,10,10,10]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">20</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Ở bước đầu tiên, chọn các phần tử tại các chỉ số 0, 2 và 3, có trung vị là 10. Sau khi xóa các phần tử này, <code>nums</code> trở thành <code>[1, 10, 10]</code>.</li>
    <li>Ở bước thứ hai, chọn các phần tử tại các chỉ số 0, 1 và 2, có trung vị là 10. Sau khi xóa các phần tử này, <code>nums</code> trở thành rỗng.</li>
</ul>

<p>Do đó, tổng các trung vị là <code>10 + 10 = 20</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 5 * 10<sup>5</sup></code></li>
    <li><code>nums.length % 3 == 0</code></li>
    <li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi bộ ba đóng góp trung vị của nó. Để tối đa hóa tổng các trung vị, ta cần biến các giá trị lớn thành trung vị và ghép chúng với các phần tử nhỏ hơn làm phần tử bổ sung. Với $n\le 5\times 10^5$, ta không thể thử tất cả các cách chia.
>
> Sau khi sắp xếp, $n/3$ giá trị nhỏ nhất chỉ có thể làm phần tử bổ sung. Trong $2n/3$ giá trị còn lại, ta lấy các phần tử nhỏ hơn cách nhau 2 vị trí làm trung vị: tính tổng từ chỉ số $n/3$ với bước nhảy $2$.
>
> Khi đó, mỗi bộ ba có một phần tử ghép cặp nhỏ hơn, còn các trung vị là nửa lớn hơn của phần còn lại.

<!-- thinking:end -->

Để tối đa hóa tổng các trung vị, ta cần chọn các phần tử lớn hơn làm trung vị bất cứ khi nào có thể. Vì mỗi thao tác chỉ có thể chọn ba phần tử, ta có thể sắp xếp mảng rồi bắt đầu từ chỉ số $n / 3$, chọn các phần tử cách nhau một vị trí cho đến cuối mảng. Cách này đảm bảo ta chọn được các trung vị lớn nhất có thể.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(\log n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumMedianSum(self, nums: List[int]) -> int:
        nums.sort()
        return sum(nums[len(nums) // 3 :: 2])
```

#### Java

```java
class Solution {
    public long maximumMedianSum(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;
        long ans = 0;
        for (int i = n / 3; i < n; i += 2) {
            ans += nums[i];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maximumMedianSum(vector<int>& nums) {
        ranges::sort(nums);
        int n = nums.size();
        long long ans = 0;
        for (int i = n / 3; i < n; i += 2) {
            ans += nums[i];
        }
        return ans;
    }
};
```

#### Go

```go
func maximumMedianSum(nums []int) (ans int64) {
    sort.Ints(nums)
    n := len(nums)
    for i := n / 3; i < n; i += 2 {
        ans += int64(nums[i])
    }
    return
}
```

#### TypeScript

```ts
function maximumMedianSum(nums: number[]): number {
    nums.sort((a, b) => a - b);
    const n = nums.length;
    let ans = 0;
    for (let i = n / 3; i < n; i += 2) {
        ans += nums[i];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
