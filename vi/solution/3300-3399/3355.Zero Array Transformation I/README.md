---
comments: true
difficulty: Medium
rating: 1591
source: Weekly Contest 424 Q2
tags:
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [3355. Zero Array Transformation I](https://leetcode.com/problems/zero-array-transformation-i)

[中文文档](/solution/3300-3399/3355.Zero%20Array%20Transformation%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code> và một mảng 2D <code>queries</code>, trong đó <code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>]</code>.</p>

<p>Với mỗi <code>queries[i]</code>:</p>

<ul>
    <li>Chọn một <span data-keyword="subset">tập con</span> các chỉ số trong phạm vi <code>[l<sub>i</sub>, r<sub>i</sub>]</code> của <code>nums</code>.</li>
    <li>Giảm giá trị tại các chỉ số đã chọn đi 1.</li>
</ul>

<p><strong>Mảng bằng không</strong> là một mảng trong đó tất cả phần tử đều bằng 0.</p>

<p>Trả về <code>true</code> nếu <em>có thể</em> biến đổi <code>nums</code> thành một <strong>mảng bằng không</strong> sau khi xử lý tuần tự tất cả các truy vấn, ngược lại trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,0,1], queries = [[0,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><strong>Với i = 0:</strong>

    <ul>
        <li>Chọn tập con các chỉ số là <code>[0, 2]</code> và giảm giá trị tại các chỉ số này đi 1.</li>
        <li>Mảng trở thành <code>[0, 0, 0]</code>, đây là một mảng bằng không.</li>
    </ul>
    </li>

</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,3,2,1], queries = [[1,3],[0,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><strong>Với i = 0:</strong>

    <ul>
        <li>Chọn tập con các chỉ số là <code>[1, 2, 3]</code> và giảm giá trị tại các chỉ số này đi 1.</li>
        <li>Mảng trở thành <code>[4, 2, 1, 0]</code>.</li>
    </ul>
    </li>
    <li><strong>Với i = 1:</strong>
    <ul>
        <li>Chọn tập con các chỉ số là <code>[0, 1, 2]</code> và giảm giá trị tại các chỉ số này đi 1.</li>
        <li>Mảng trở thành <code>[3, 1, 0, 0]</code>, không phải là một mảng bằng không.</li>
    </ul>
    </li>

</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
    <li><code>0 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
    <li><code>queries[i].length == 2</code></li>
    <li><code>0 &lt;= l<sub>i</sub> &lt;= r<sub>i</sub> &lt; nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng hiệu

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi truy vấn bao phủ $[l,r]$ một lần; ta cần kiểm tra liệu các truy vấn có thể giảm $\textit{nums}$ về 0 hay không. Với $n,m \le 10^5$, ta không thể áp dụng từng truy vấn một.
>
> Việc bao phủ là một phép cộng trên đoạn: cộng $+1$ tại $l$ và trừ $-1$ tại $r+1$. Tổng tiền tố chính là số lần bao phủ của mỗi chỉ số.
>
> Nếu $\textit{nums}[i]$ lớn hơn số lần bao phủ đó, mảng không thể trở thành mảng bằng không.

<!-- thinking:end -->

Ta có thể dùng mảng hiệu để giải bài toán này.

Xây dựng một mảng $d$ có độ dài $n + 1$, với tất cả giá trị ban đầu bằng $0$. Với mỗi truy vấn $[l, r]$, ta cộng $1$ vào $d[l]$ và trừ $1$ khỏi $d[r + 1]$.

Sau đó, duyệt mảng $d$ trong phạm vi $[0, n - 1]$, đồng thời cộng dồn tổng tiền tố $s$. Nếu $\textit{nums}[i] > s$, nghĩa là $\textit{nums}$ không thể được biến đổi thành một mảng bằng không, nên trả về $\textit{false}$.

Sau khi duyệt xong, trả về $\textit{true}$.

Độ phức tạp thời gian là $O(n + m)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $\textit{nums}$ và $m$ là số lượng truy vấn.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isZeroArray(self, nums: List[int], queries: List[List[int]]) -> bool:
        d = [0] * (len(nums) + 1)
        for l, r in queries:
            d[l] += 1
            d[r + 1] -= 1
        s = 0
        for x, y in zip(nums, d):
            s += y
            if x > s:
                return False
        return True
```

#### Java

```java
class Solution {
    public boolean isZeroArray(int[] nums, int[][] queries) {
        int n = nums.length;
        int[] d = new int[n + 1];
        for (var q : queries) {
            int l = q[0], r = q[1];
            ++d[l];
            --d[r + 1];
        }
        for (int i = 0, s = 0; i < n; ++i) {
            s += d[i];
            if (nums[i] > s) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isZeroArray(vector<int>& nums, vector<vector<int>>& queries) {
        int n = nums.size();
        int d[n + 1];
        memset(d, 0, sizeof(d));
        for (const auto& q : queries) {
            int l = q[0], r = q[1];
            ++d[l];
            --d[r + 1];
        }
        for (int i = 0, s = 0; i < n; ++i) {
            s += d[i];
            if (nums[i] > s) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func isZeroArray(nums []int, queries [][]int) bool {
    d := make([]int, len(nums)+1)
    for _, q := range queries {
        l, r := q[0], q[1]
        d[l]++
        d[r+1]--
    }
    s := 0
    for i, x := range nums {
        s += d[i]
        if x > s {
            return false
        }
    }
    return true
}
```

#### TypeScript

```ts
function isZeroArray(nums: number[], queries: number[][]): boolean {
    const n = nums.length;
    const d: number[] = Array(n + 1).fill(0);
    for (const [l, r] of queries) {
        ++d[l];
        --d[r + 1];
    }
    for (let i = 0, s = 0; i < n; ++i) {
        s += d[i];
        if (nums[i] > s) {
            return false;
        }
    }
    return true;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
