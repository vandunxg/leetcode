---
comments: true
difficulty: Medium
tags:
    - Array
    - Two Pointers
    - Simulation
---

<!-- problem:start -->

# [3400. Maximum Number of Matching Indices After Right Shifts 🔒](https://leetcode.com/problems/maximum-number-of-matching-indices-after-right-shifts)

[中文文档](/solution/3400-3499/3400.Maximum%20Number%20of%20Matching%20Indices%20After%20Right%20Shifts/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>nums1</code> và <code>nums2</code> có cùng độ dài.</p>

<p>Chỉ số <code>i</code> được gọi là <strong>khớp</strong> nếu <code>nums1[i] == nums2[i]</code>.</p>

<p>Hãy trả về số lượng <strong>lớn nhất</strong> các chỉ số <strong>khớp</strong> sau khi thực hiện một số lần <strong>dịch phải</strong> trên <code>nums1</code>.</p>

<p>Một lần <strong>dịch phải</strong> được định nghĩa là dịch phần tử tại chỉ số <code>i</code> đến chỉ số <code>(i + 1) % n</code>, với mọi chỉ số.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [3,1,2,3,1,2], nums2 = [1,2,3,1,2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Nếu dịch phải <code>nums1</code> 2 lần, ta được <code>[1, 2, 3, 1, 2, 3]</code>. Mọi chỉ số đều khớp, nên kết quả là 6.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [1,4,2,5,3,1], nums2 = [2,3,1,2,4,6]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Nếu dịch phải <code>nums1</code> 3 lần, ta được <code>[5, 3, 1, 1, 4, 2]</code>. Các chỉ số 1, 2 và 4 khớp, nên kết quả là 3.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>nums1.length == nums2.length</code></li>
    <li><code>1 &lt;= nums1.length, nums2.length &lt;= 3000</code></li>
    <li><code>1 &lt;= nums1[i], nums2[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm số lượng vị trí khớp lớn nhất sau một số lần dịch phải vòng trên $\textit{nums1}$. Việc tạo một bản sao đã xoay cho từng độ lệch không làm giảm số phép so sánh.
>
> Với $n \le 3000$, ta có thể liệt kê toàn bộ $n$ độ lệch và so sánh từng phần tử trong $O(n^2)$, phù hợp với giới hạn đề bài. Việc khớp chỉ phụ thuộc vào độ lệch tương đối, nên không cần xoay mảng một cách tường minh.
>
> Sau $k$ lần dịch phải, giá trị ban đầu ở vị trí $(i+k)\bmod n$ sẽ đến vị trí $i$. Vì vậy, ta liệt kê $k$, so sánh $\textit{nums1}[(i+k)\bmod n]$ với $\textit{nums2}[i]$, rồi lưu lại số lượng lớn nhất.

<!-- thinking:end -->

Ta có thể liệt kê số lần dịch phải $k$, trong đó $0 \leq k < n$. Với mỗi $k$, ta tính số chỉ số khớp giữa mảng $\textit{nums1}$ sau khi dịch phải $k$ lần và $\textit{nums2}$. Giá trị lớn nhất chính là đáp án.

Độ phức tạp thời gian là $O(n^2)$, trong đó $n$ là độ dài của mảng $\textit{nums1}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumMatchingIndices(self, nums1: List[int], nums2: List[int]) -> int:
        n = len(nums1)
        ans = 0
        for k in range(n):
            t = sum(nums1[(i + k) % n] == x for i, x in enumerate(nums2))
            ans = max(ans, t)
        return ans
```

#### Java

```java
class Solution {
    public int maximumMatchingIndices(int[] nums1, int[] nums2) {
        int n = nums1.length;
        int ans = 0;
        for (int k = 0; k < n; ++k) {
            int t = 0;
            for (int i = 0; i < n; ++i) {
                if (nums1[(i + k) % n] == nums2[i]) {
                    ++t;
                }
            }
            ans = Math.max(ans, t);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumMatchingIndices(vector<int>& nums1, vector<int>& nums2) {
        int n = nums1.size();
        int ans = 0;
        for (int k = 0; k < n; ++k) {
            int t = 0;
            for (int i = 0; i < n; ++i) {
                if (nums1[(i + k) % n] == nums2[i]) {
                    ++t;
                }
            }
            ans = max(ans, t);
        }
        return ans;
    }
};
```

#### Go

```go
func maximumMatchingIndices(nums1 []int, nums2 []int) (ans int) {
    n := len(nums1)
    for k := range nums1 {
        t := 0
        for i, x := range nums2 {
            if nums1[(i+k)%n] == x {
                t++
            }
        }
        ans = max(ans, t)
    }
    return
}
```

#### TypeScript

```ts
function maximumMatchingIndices(nums1: number[], nums2: number[]): number {
    const n = nums1.length;
    let ans: number = 0;
    for (let k = 0; k < n; ++k) {
        let t: number = 0;
        for (let i = 0; i < n; ++i) {
            if (nums1[(i + k) % n] === nums2[i]) {
                ++t;
            }
        }
        ans = Math.max(ans, t);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
