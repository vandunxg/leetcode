---
comments: true
difficulty: Medium
rating: 1913
source: Weekly Contest 424 Q3
tags:
    - Array
    - Two Pointers
    - Binary Search
    - Prefix Sum
---

<!-- problem:start -->

# [3356. Zero Array Transformation II](https://leetcode.com/problems/zero-array-transformation-ii)

[中文文档](/solution/3300-3399/3356.Zero%20Array%20Transformation%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code> và một mảng 2D <code>queries</code>, trong đó <code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>, val<sub>i</sub>]</code>.</p>

<p>Mỗi <code>queries[i]</code> biểu diễn thao tác sau trên <code>nums</code>:</p>

<ul>
    <li>Giảm giá trị tại mỗi chỉ số trong đoạn <code>[l<sub>i</sub>, r<sub>i</sub>]</code> của <code>nums</code> đi <strong>nhiều nhất</strong> <code>val<sub>i</sub></code>.</li>
    <li>Lượng giảm<!-- notionvc: b232c9d9-a32d-448c-85b8-b637de593c11 --> có thể được chọn <strong>độc lập</strong> cho từng chỉ số.</li>
</ul>

<p>Một <strong>Zero Array</strong> là một mảng có tất cả phần tử bằng 0.</p>

<p>Trả về giá trị <strong>không âm</strong> <strong>nhỏ nhất</strong> của <code>k</code> sao cho sau khi thực hiện <strong>tuần tự</strong> <code>k</code> query đầu tiên, <code>nums</code> trở thành một <strong>Zero Array</strong>. Nếu không tồn tại <code>k</code> như vậy, trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,0,2], queries = [[0,2,1],[0,2,1],[1,1,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><strong>Với i = 0 (l = 0, r = 2, val = 1):</strong>

    <ul>
        <li>Giảm các giá trị tại các chỉ số <code>[0, 1, 2]</code> lần lượt đi <code>[1, 0, 1]</code>.</li>
        <li>Mảng trở thành <code>[1, 0, 1]</code>.</li>
    </ul>
    </li>
    <li><strong>Với i = 1 (l = 0, r = 2, val = 1):</strong>
    <ul>
        <li>Giảm các giá trị tại các chỉ số <code>[0, 1, 2]</code> lần lượt đi <code>[1, 0, 1]</code>.</li>
        <li>Mảng trở thành <code>[0, 0, 0]</code>, là một Zero Array. Vì vậy, giá trị nhỏ nhất của <code>k</code> là 2.</li>
    </ul>
    </li>

</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,3,2,1], queries = [[1,3,2],[0,2,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li><strong>Với i = 0 (l = 1, r = 3, val = 2):</strong>

    <ul>
        <li>Giảm các giá trị tại các chỉ số <code>[1, 2, 3]</code> lần lượt đi <code>[2, 2, 1]</code>.</li>
        <li>Mảng trở thành <code>[4, 1, 0, 0]</code>.</li>
    </ul>
    </li>
    <li><strong>Với i = 1 (l = 0, r = 2, val<span style="font-size: 13.3333px;"> </span>= 1):</strong>
    <ul>
        <li>Giảm các giá trị tại các chỉ số <code>[0, 1, 2]</code> lần lượt đi <code>[1, 1, 0]</code>.</li>
        <li>Mảng trở thành <code>[3, 0, 0, 0]</code>, không phải là một Zero Array.</li>
    </ul>
    </li>

</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
    <li><code>0 &lt;= nums[i] &lt;= 5 * 10<sup>5</sup></code></li>
    <li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
    <li><code>queries[i].length == 3</code></li>
    <li><code>0 &lt;= l<sub>i</sub> &lt;= r<sub>i</sub> &lt; nums.length</code></li>
    <li><code>1 &lt;= val<sub>i</sub> &lt;= 5</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng hiệu + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Các query hiện có trọng số và phải được lấy theo một prefix. Prefix càng dài thì càng dễ thỏa mãn, nên độ dài khả thi có tính đơn điệu.
>
> Để kiểm tra $k$, ta ghi $k$ đoạn có trọng số đầu tiên vào một mảng hiệu rồi kiểm tra xem mọi chỉ số có được phủ đủ hay không.
>
> Tìm kiếm nhị phân cho độ dài $k$ nhỏ nhất khả thi, hoặc $-1$ nếu ngay cả $m$ query cũng không đủ.

<!-- thinking:end -->

Ta nhận thấy càng sử dụng nhiều query thì càng dễ biến mảng thành một mảng toàn số 0, cho thấy tính đơn điệu. Vì vậy, ta có thể dùng tìm kiếm nhị phân để xét số lượng query và kiểm tra xem mảng có thể trở thành một mảng toàn số 0 sau $k$ query đầu tiên hay không.

Ta xác định biên trái $l$ và biên phải $r$ cho tìm kiếm nhị phân, ban đầu $l = 0$, $r = m + 1$, trong đó $m$ là số lượng query. Ta định nghĩa hàm $\text{check}(k)$ để cho biết liệu mảng có thể trở thành một mảng toàn số 0 sau $k$ query đầu tiên hay không. Ta có thể dùng mảng hiệu để duy trì giá trị của mỗi phần tử.

Xét mảng $d$ có độ dài $n + 1$, được khởi tạo toàn bằng $0$. Với mỗi query $[l, r, val]$ trong $k$ query đầu tiên, ta cộng $val$ vào $d[l]$ và trừ $val$ khỏi $d[r + 1]$.

Sau đó, ta duyệt mảng $d$ trong phạm vi $[0, n - 1]$, đồng thời cộng dồn tổng tiền tố $s$. Nếu $\textit{nums}[i] > s$, điều đó có nghĩa là không thể biến $\textit{nums}$ thành một mảng toàn số 0, nên ta trả về $\textit{false}$.

Trong quá trình tìm kiếm nhị phân, nếu $\text{check}(k)$ trả về $\text{true}$, điều đó có nghĩa là mảng có thể trở thành một mảng toàn số 0, nên ta cập nhật biên phải $r$ thành $k$; ngược lại, ta cập nhật biên trái $l$ thành $k + 1$.

Cuối cùng, ta kiểm tra xem $l > m$ hay không. Nếu có, trả về -1; nếu không, trả về $l$.

Độ phức tạp thời gian là $O((n + m) \times \log m)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ và $m$ lần lượt là độ dài của mảng $\textit{nums}$ và $\textit{queries}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minZeroArray(self, nums: List[int], queries: List[List[int]]) -> int:
        def check(k: int) -> bool:
            d = [0] * (len(nums) + 1)
            for l, r, val in queries[:k]:
                d[l] += val
                d[r + 1] -= val
            s = 0
            for x, y in zip(nums, d):
                s += y
                if x > s:
                    return False
            return True

        m = len(queries)
        l = bisect_left(range(m + 1), True, key=check)
        return -1 if l > m else l
```

#### Java

```java
class Solution {
    private int n;
    private int[] nums;
    private int[][] queries;

    public int minZeroArray(int[] nums, int[][] queries) {
        this.nums = nums;
        this.queries = queries;
        n = nums.length;
        int m = queries.length;
        int l = 0, r = m + 1;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (check(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l > m ? -1 : l;
    }

    private boolean check(int k) {
        int[] d = new int[n + 1];
        for (int i = 0; i < k; ++i) {
            int l = queries[i][0], r = queries[i][1], val = queries[i][2];
            d[l] += val;
            d[r + 1] -= val;
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
    int minZeroArray(vector<int>& nums, vector<vector<int>>& queries) {
        int n = nums.size();
        int d[n + 1];
        int m = queries.size();
        int l = 0, r = m + 1;
        auto check = [&](int k) -> bool {
            memset(d, 0, sizeof(d));
            for (int i = 0; i < k; ++i) {
                int l = queries[i][0], r = queries[i][1], val = queries[i][2];
                d[l] += val;
                d[r + 1] -= val;
            }
            for (int i = 0, s = 0; i < n; ++i) {
                s += d[i];
                if (nums[i] > s) {
                    return false;
                }
            }
            return true;
        };
        while (l < r) {
            int mid = (l + r) >> 1;
            if (check(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l > m ? -1 : l;
    }
};
```

#### Go

```go
func minZeroArray(nums []int, queries [][]int) int {
    n, m := len(nums), len(queries)
    l := sort.Search(m+1, func(k int) bool {
        d := make([]int, n+1)
        for _, q := range queries[:k] {
            l, r, val := q[0], q[1], q[2]
            d[l] += val
            d[r+1] -= val
        }
        s := 0
        for i, x := range nums {
            s += d[i]
            if x > s {
                return false
            }
        }
        return true
    })
    if l > m {
        return -1
    }
    return l
}
```

#### TypeScript

```ts
function minZeroArray(nums: number[], queries: number[][]): number {
    const [n, m] = [nums.length, queries.length];
    const d: number[] = Array(n + 1);
    let [l, r] = [0, m + 1];
    const check = (k: number): boolean => {
        d.fill(0);
        for (let i = 0; i < k; ++i) {
            const [l, r, val] = queries[i];
            d[l] += val;
            d[r + 1] -= val;
        }
        for (let i = 0, s = 0; i < n; ++i) {
            s += d[i];
            if (nums[i] > s) {
                return false;
            }
        }
        return true;
    };
    while (l < r) {
        const mid = (l + r) >> 1;
        if (check(mid)) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return l > m ? -1 : l;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_zero_array(nums: Vec<i32>, queries: Vec<Vec<i32>>) -> i32 {
        let n = nums.len();
        let m = queries.len();
        let mut d: Vec<i64> = vec![0; n + 1];
        let (mut l, mut r) = (0_usize, m + 1);

        let check = |k: usize, d: &mut Vec<i64>| -> bool {
            d.fill(0);
            for i in 0..k {
                let (l, r, val) = (
                    queries[i][0] as usize,
                    queries[i][1] as usize,
                    queries[i][2] as i64,
                );
                d[l] += val;
                d[r + 1] -= val;
            }
            let mut s: i64 = 0;
            for i in 0..n {
                s += d[i];
                if nums[i] as i64 > s {
                    return false;
                }
            }
            true
        };

        while l < r {
            let mid = (l + r) >> 1;
            if check(mid, &mut d) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        if l > m { -1 } else { l as i32 }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
