---
comments: true
difficulty: Easy
rating: 1194
source: Weekly Contest 403 Q1
tags:
    - Array
    - Two Pointers
    - Sorting
---

<!-- problem:start -->

# [3194. Minimum Average of Smallest and Largest Elements](https://leetcode.com/problems/minimum-average-of-smallest-and-largest-elements)

[中文文档](/solution/3100-3199/3194.Minimum%20Average%20of%20Smallest%20and%20Largest%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có một mảng số thực <code>averages</code> ban đầu rỗng. Cho một mảng số nguyên <code>nums</code> gồm <code>n</code> phần tử, trong đó <code>n</code> là số chẵn.</p>

<p>Bạn lặp lại quy trình sau <code>n / 2</code> lần:</p>

<ul>
    <li>Xóa phần tử <strong>nhỏ nhất</strong>, <code>minElement</code>, và phần tử <strong>lớn nhất</strong>, <code>maxElement</code>, khỏi <code>nums</code>.</li>
    <li>Thêm <code>(minElement + maxElement) / 2</code> vào <code>averages</code>.</li>
</ul>

<p>Trả về phần tử <strong>nhỏ nhất</strong> trong <code>averages</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [7,8,3,4,15,13,4,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5.5</span></p>

<p><strong>Giải thích:</strong></p>

<table>
    <tbody>
        <tr>
            <th>bước</th>
            <th>nums</th>
            <th>averages</th>
        </tr>
        <tr>
            <td>0</td>
            <td>[7,8,3,4,15,13,4,1]</td>
            <td>[]</td>
        </tr>
        <tr>
            <td>1</td>
            <td>[7,8,3,4,13,4]</td>
            <td>[8]</td>
        </tr>
        <tr>
            <td>2</td>
            <td>[7,8,4,4]</td>
            <td>[8,8]</td>
        </tr>
        <tr>
            <td>3</td>
            <td>[7,4]</td>
            <td>[8,8,6]</td>
        </tr>
        <tr>
            <td>4</td>
            <td>[]</td>
            <td>[8,8,6,5.5]</td>
        </tr>
    </tbody>
</table>
Phần tử nhỏ nhất của averages là 5.5, nên được trả về.</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,9,8,3,10,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5.5</span></p>

<p><strong>Giải thích:</strong></p>

<table>
    <tbody>
        <tr>
            <th>bước</th>
            <th>nums</th>
            <th>averages</th>
        </tr>
        <tr>
            <td>0</td>
            <td><span class="example-io">[1,9,8,3,10,5]</span></td>
            <td>[]</td>
        </tr>
        <tr>
            <td>1</td>
            <td><span class="example-io">[9,8,3,5]</span></td>
            <td>[5.5]</td>
        </tr>
        <tr>
            <td>2</td>
            <td><span class="example-io">[8,5]</span></td>
            <td>[5.5,6]</td>
        </tr>
        <tr>
            <td>3</td>
            <td>[]</td>
            <td>[5.5,6,6.5]</td>
        </tr>
    </tbody>
</table>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,7,8,9]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5.0</span></p>

<p><strong>Giải thích:</strong></p>

<table>
    <tbody>
        <tr>
            <th>bước</th>
            <th>nums</th>
            <th>averages</th>
        </tr>
        <tr>
            <td>0</td>
            <td><span class="example-io">[1,2,3,7,8,9]</span></td>
            <td>[]</td>
        </tr>
        <tr>
            <td>1</td>
            <td><span class="example-io">[2,3,7,8]</span></td>
            <td>[5]</td>
        </tr>
        <tr>
            <td>2</td>
            <td><span class="example-io">[3,7]</span></td>
            <td>[5,5]</td>
        </tr>
        <tr>
            <td>3</td>
            <td><span class="example-io">[]</span></td>
            <td>[5,5,5]</td>
        </tr>
    </tbody>
</table>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= n == nums.length &lt;= 50</code></li>
    <li><code>n</code> là số chẵn.</li>
    <li><code>1 &lt;= nums[i] &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi bước lấy trung bình của phần tử nhỏ nhất và lớn nhất hiện tại; đáp án là giá trị nhỏ nhất trong các giá trị trung bình đó. Có thể mô phỏng bằng một multiset, nhưng sau khi sắp xếp, các phần tử ở hai đầu đã tạo thành các cặp.
>
> Phần tử nhỏ thứ $i$ được ghép với phần tử lớn thứ $i$, lấy trung bình bằng $(nums[i]+nums[n-1-i])/2$.
>
> Lấy giá trị nhỏ nhất trong các tổng đó với $i=0..n/2-1$, rồi chia cho hai.

<!-- thinking:end -->

Trước hết, ta sắp xếp mảng $\textit{nums}$. Sau đó, ta lần lượt lấy các phần tử ở hai đầu mảng, tính tổng của hai phần tử và lấy giá trị nhỏ nhất. Cuối cùng, ta trả về giá trị nhỏ nhất chia cho 2 làm đáp án.

Độ phức tạp thời gian là $O(n \log n)$, còn độ phức tạp không gian là $O(\log n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumAverage(self, nums: List[int]) -> float:
        nums.sort()
        n = len(nums)
        return min(nums[i] + nums[-i - 1] for i in range(n // 2)) / 2
```

#### Java

```java
class Solution {
    public double minimumAverage(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;
        int ans = 1 << 30;
        for (int i = 0; i < n / 2; ++i) {
            ans = Math.min(ans, nums[i] + nums[n - i - 1]);
        }
        return ans / 2.0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    double minimumAverage(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int ans = 1 << 30, n = nums.size();
        for (int i = 0; i < n; ++i) {
            ans = min(ans, nums[i] + nums[n - i - 1]);
        }
        return ans / 2.0;
    }
};
```

#### Go

```go
func minimumAverage(nums []int) float64 {
    sort.Ints(nums)
    n := len(nums)
    ans := 1 << 30
    for i, x := range nums[:n/2] {
        ans = min(ans, x+nums[n-i-1])
    }
    return float64(ans) / 2
}
```

#### TypeScript

```ts
function minimumAverage(nums: number[]): number {
    nums.sort((a, b) => a - b);
    const n = nums.length;
    let ans = Infinity;
    for (let i = 0; i * 2 < n; ++i) {
        ans = Math.min(ans, nums[i] + nums[n - 1 - i]);
    }
    return ans / 2;
}
```

#### Rust

```rust
impl Solution {
    pub fn minimum_average(mut nums: Vec<i32>) -> f64 {
        nums.sort();
        let n = nums.len();
        let ans = (0..n / 2).map(|i| nums[i] + nums[n - i - 1]).min().unwrap();
        ans as f64 / 2.0
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
