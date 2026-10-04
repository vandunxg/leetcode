---
comments: true
difficulty: Easy
rating: 1200
source: Weekly Contest 450 Q1
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [3550. Smallest Index With Digit Sum Equal to Index](https://leetcode.com/problems/smallest-index-with-digit-sum-equal-to-index)

[中文文档](/solution/3500-3599/3550.Smallest%20Index%20With%20Digit%20Sum%20Equal%20to%20Index/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Hãy trả về chỉ số <strong>nhỏ nhất</strong> <code>i</code> sao cho tổng các chữ số của <code>nums[i]</code> bằng <code>i</code>.</p>

<p>Nếu không tồn tại chỉ số như vậy, hãy trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,3,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Với <code>nums[2] = 2</code>, tổng các chữ số là 2, bằng với chỉ số <code>i = 2</code>. Vì vậy, kết quả là 2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,10,11]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Với <code>nums[1] = 10</code>, tổng các chữ số là <code>1 + 0 = 1</code>, bằng với chỉ số <code>i = 1</code>.</li>
    <li>Với <code>nums[2] = 11</code>, tổng các chữ số là <code>1 + 1 = 2</code>, bằng với chỉ số <code>i = 2</code>.</li>
    <li>Vì chỉ số 1 là nhỏ nhất, kết quả là 1.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Không có chỉ số nào thỏa mãn điều kiện, nên kết quả là -1.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 100</code></li>
    <li><code>0 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt + Tổng chữ số

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm chỉ số nhỏ nhất $i$ có tổng các chữ số bằng $i$. Duyệt từ $0$ sẽ cho chỉ số nhỏ nhất.
>
> Ta tính tổng các chữ số bằng cách chia cho $10$; nếu không có chỉ số nào khớp, trả về $-1$. Chỉ cần duyệt tuyến tính là đủ.

<!-- thinking:end -->

Ta bắt đầu từ chỉ số $i = 0$ và duyệt qua từng phần tử $x$ trong mảng, tính tổng các chữ số $s$ của $x$. Nếu $s = i$, ta trả về chỉ số $i$. Nếu duyệt hết mảng mà không tìm thấy chỉ số phù hợp, ta trả về -1.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng. Độ phức tạp không gian là $O(1)$ vì chỉ sử dụng thêm một lượng bộ nhớ hằng số.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestIndex(self, nums: List[int]) -> int:
        for i, x in enumerate(nums):
            s = 0
            while x:
                s += x % 10
                x //= 10
            if s == i:
                return i
        return -1
```

#### Java

```java
class Solution {
    public int smallestIndex(int[] nums) {
        for (int i = 0; i < nums.length; ++i) {
            int s = 0;
            while (nums[i] != 0) {
                s += nums[i] % 10;
                nums[i] /= 10;
            }
            if (s == i) {
                return i;
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int smallestIndex(vector<int>& nums) {
        for (int i = 0; i < nums.size(); ++i) {
            int s = 0;
            while (nums[i]) {
                s += nums[i] % 10;
                nums[i] /= 10;
            }
            if (s == i) {
                return i;
            }
        }
        return -1;
    }
};
```

#### Go

```go
func smallestIndex(nums []int) int {
    for i, x := range nums {
        s := 0
        for ; x > 0; x /= 10 {
            s += x % 10
        }
        if s == i {
            return i
        }
    }
    return -1
}
```

#### TypeScript

```ts
function smallestIndex(nums: number[]): number {
    for (let i = 0; i < nums.length; ++i) {
        let s = 0;
        for (; nums[i] > 0; nums[i] = Math.floor(nums[i] / 10)) {
            s += nums[i] % 10;
        }
        if (s === i) {
            return i;
        }
    }
    return -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
