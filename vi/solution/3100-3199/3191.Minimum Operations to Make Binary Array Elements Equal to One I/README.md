---
comments: true
difficulty: Medium
rating: 1311
source: Biweekly Contest 133 Q2
tags:
    - Bit Manipulation
    - Queue
    - Array
    - Prefix Sum
    - Sliding Window
---

<!-- problem:start -->

# [3191. Minimum Operations to Make Binary Array Elements Equal to One I](https://leetcode.com/problems/minimum-operations-to-make-binary-array-elements-equal-to-one-i)

[中文文档](/solution/3100-3199/3191.Minimum%20Operations%20to%20Make%20Binary%20Array%20Elements%20Equal%20to%20One%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một <span data-keyword="binary-array">mảng nhị phân</span> <code>nums</code>.</p>

<p>Bạn có thể thực hiện thao tác sau trên mảng <strong>bất kỳ</strong> số lần nào (có thể bằng 0):</p>

<ul>
    <li>Chọn <strong>bất kỳ</strong> 3 phần tử <strong>liên tiếp</strong> trong mảng và <strong>lật</strong> <strong>tất cả</strong> chúng.</li>
</ul>

<p><strong>Lật</strong> một phần tử nghĩa là thay đổi giá trị của nó từ 0 thành 1, và từ 1 thành 0.</p>

<p>Trả về số thao tác <strong>nhỏ nhất</strong> cần thực hiện để tất cả phần tử trong <code>nums</code> đều bằng 1. Nếu không thể, trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [0,1,1,1,0,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong><br />
Ta có thể thực hiện các thao tác sau:</p>

<ul>
    <li>Chọn các phần tử tại các chỉ số 0, 1 và 2. Mảng sau đó là <code>nums = [<u><strong>1</strong></u>,<u><strong>0</strong></u>,<u><strong>0</strong></u>,1,0,0]</code>.</li>
    <li>Chọn các phần tử tại các chỉ số 1, 2 và 3. Mảng sau đó là <code>nums = [1,<u><strong>1</strong></u>,<u><strong>1</strong></u>,<strong><u>0</u></strong>,0,0]</code>.</li>
    <li>Chọn các phần tử tại các chỉ số 3, 4 và 5. Mảng sau đó là <code>nums = [1,1,1,<strong><u>1</u></strong>,<u><strong>1</strong></u>,<u><strong>1</strong></u>]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [0,1,1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong><br />
Không thể làm cho tất cả phần tử đều bằng 1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>3 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
    <li><code>0 &lt;= nums[i] &lt;= 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt tuần tự + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác lật một cửa sổ có độ dài $3$. Một số $0$ nếu không được lật ngay thì không thể được bao phủ bởi một cửa sổ nào về sau.
>
> Lật tại mỗi số $0$ còn lại trên các chỉ số $i,i+1,i+2$; nếu $i+2$ vượt quá cuối mảng thì không thể thực hiện.
>
> Duyệt từ trái sang phải, thực hiện XOR trên hai phần tử tiếp theo và đếm số thao tác. Trả về $-1$ nếu vượt quá giới hạn, nếu không trả về số lần đếm.

<!-- thinking:end -->

Ta nhận thấy vị trí đầu tiên trong mảng có giá trị $0$ bắt buộc phải được lật, nếu không nó không thể được chuyển thành $1$. Vì vậy, ta có thể duyệt tuần tự qua mảng, và mỗi khi gặp $0$, ta lật hai phần tử tiếp theo rồi tăng số thao tác lên một.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums: List[int]) -> int:
        ans = 0
        for i, x in enumerate(nums):
            if x == 0:
                if i + 2 >= len(nums):
                    return -1
                nums[i + 1] ^= 1
                nums[i + 2] ^= 1
                ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int minOperations(int[] nums) {
        int ans = 0;
        int n = nums.length;
        for (int i = 0; i < n; ++i) {
            if (nums[i] == 0) {
                if (i + 2 >= n) {
                    return -1;
                }
                nums[i + 1] ^= 1;
                nums[i + 2] ^= 1;
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
    int minOperations(vector<int>& nums) {
        int ans = 0;
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            if (nums[i] == 0) {
                if (i + 2 >= n) {
                    return -1;
                }
                nums[i + 1] ^= 1;
                nums[i + 2] ^= 1;
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minOperations(nums []int) (ans int) {
    for i, x := range nums {
        if x == 0 {
            if i+2 >= len(nums) {
                return -1
            }
            nums[i+1] ^= 1
            nums[i+2] ^= 1
            ans++
        }
    }
    return
}
```

#### TypeScript

```ts
function minOperations(nums: number[]): number {
    const n = nums.length;
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        if (nums[i] === 0) {
            if (i + 2 >= n) {
                return -1;
            }
            nums[i + 1] ^= 1;
            nums[i + 2] ^= 1;
            ++ans;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
