---
comments: true
difficulty: Hard
rating: 2209
source: Weekly Contest 476 Q4
tags:
    - Array
    - Binary Search
    - Prefix Sum
---

<!-- problem:start -->

# [3748. Count Stable Subarrays](https://leetcode.com/problems/count-stable-subarrays)

[中文文档](/solution/3700-3799/3748.Count%20Stable%20Subarrays/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Một <strong><span data-keyword="subarray-nonempty">mảng con</span></strong> của <code>nums</code> được gọi là <strong>ổn định</strong> nếu nó không chứa <strong>nghịch thế</strong>, tức là không tồn tại cặp chỉ số <code>i &lt; j</code> sao cho <code>nums[i] &gt; nums[j]</code>.</p>

<p>Bạn cũng được cho một <strong>mảng số nguyên 2 chiều</strong> <code>queries</code> có độ dài <code>q</code>, trong đó mỗi <code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>]</code> biểu diễn một truy vấn. Với mỗi truy vấn <code>[l<sub>i</sub>, r<sub>i</sub>]</code>, hãy tính số lượng <strong>mảng con ổn định</strong> nằm hoàn toàn trong đoạn <code>nums[l<sub>i</sub>..r<sub>i</sub>]</code>.</p>

<p>Trả về một mảng số nguyên <code>ans</code> có độ dài <code>q</code>, trong đó <code>ans[i]</code> là đáp án của truy vấn thứ <code>i<sup>th</sup></code>.​​​​​​​​​​​​​​</p>

<p><strong>Lưu ý</strong>:</p>

<ul>
    <li>Mảng con gồm một phần tử được xem là ổn định.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,1,2], queries = [[0,1],[1,2],[0,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,3,4]</span></p>

<p><strong>Giải thích:</strong>​​​​​</p>

<ul>
    <li>Với <code>queries[0] = [0, 1]</code>, mảng con là <code>[nums[0], nums[1]] = [3, 1]</code>.

    <ul>
        <li>Các mảng con ổn định là <code>[3]</code> và <code>[1]</code>. Tổng số mảng con ổn định là 2.</li>
    </ul>
    </li>
    <li>Với <code>queries[1] = [1, 2]</code>, mảng con là <code>[nums[1], nums[2]] = [1, 2]</code>.
    <ul>
        <li>Các mảng con ổn định là <code>[1]</code>, <code>[2]</code> và <code>[1, 2]</code>. Tổng số mảng con ổn định là 3.</li>
    </ul>
    </li>
    <li>Với <code>queries[2] = [0, 2]</code>, mảng con là <code>[nums[0], nums[1], nums[2]] = [3, 1, 2]</code>.
    <ul>
        <li>Các mảng con ổn định là <code>[3]</code>, <code>[1]</code>, <code>[2]</code> và <code>[1, 2]</code>. Tổng số mảng con ổn định là 4.</li>
    </ul>
    </li>

</ul>

<p>Vì vậy, <code>ans = [2, 3, 4]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,2], queries = [[0,1],[0,0]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[3,1]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Với <code>queries[0] = [0, 1]</code>, mảng con là <code>[nums[0], nums[1]] = [2, 2]</code>.

    <ul>
        <li>Các mảng con ổn định là <code>[2]</code>, <code>[2]</code> và <code>[2, 2]</code>. Tổng số mảng con ổn định là 3.</li>
    </ul>
    </li>
    <li>Với <code>queries[1] = [0, 0]</code>, mảng con là <code>[nums[0]] = [2]</code>.
    <ul>
        <li>Mảng con ổn định là <code>[2]</code>. Tổng số mảng con ổn định là 1.</li>
    </ul>
    </li>

</ul>

<p>Vì vậy, <code>ans = [3, 1]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
    <li><code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>]</code></li>
    <li><code>0 &lt;= l<sub>i</sub> &lt;= r<sub>i</sub> &lt;= nums.length - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm theo đoạn

<!-- thinking:start -->

> **Tư duy**
>
> Mảng con ổn định chính là các mảng con không giảm, nên mảng được chia thành các đoạn đơn điệu. Nhiều truy vấn cần được trả lời nhanh. Ta lưu vị trí bắt đầu của mỗi đoạn và tổng tiền tố của số mảng con trong từng đoạn: truy vấn nằm trong một đoạn dùng số tam giác; truy vấn trải qua nhiều đoạn dùng tổng tiền tố cho các đoạn đầy đủ và số tam giác cho hai đoạn đầu mút.

<!-- thinking:end -->

Theo mô tả bài toán, một mảng con ổn định được định nghĩa là mảng con không chứa cặp nghịch thế, nghĩa là các phần tử trong mảng con được sắp xếp theo thứ tự không giảm. Do đó, ta có thể chia mảng thành một số đoạn không giảm, sử dụng mảng $\text{seg}$ để lưu vị trí bắt đầu của mỗi đoạn. Đồng thời, ta cần một mảng tổng tiền tố $\text{s}$ để lưu số lượng mảng con ổn định trong mỗi đoạn.

Sau đó, với mỗi truy vấn $[l, r]$, có thể có 3 trường hợp:

1. Đoạn truy vấn $[l, r]$ nằm hoàn toàn trong một đoạn duy nhất. Khi đó, số mảng con ổn định có thể được tính trực tiếp bằng công thức $\frac{(k + 1) \cdot k}{2}$, trong đó $k = r - l + 1$.
2. Đoạn truy vấn $[l, r]$ trải qua nhiều đoạn. Khi đó, ta cần lần lượt tính số mảng con ổn định trong đoạn không đầy đủ bên trái, đoạn không đầy đủ bên phải và các đoạn đầy đủ ở giữa, rồi cộng chúng lại để thu được kết quả cuối cùng.

Độ phức tạp thời gian là $O((n + q) \log n)$, trong đó $n$ là độ dài mảng và $q$ là số truy vấn. Độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countStableSubarrays(
        self, nums: List[int], queries: List[List[int]]
    ) -> List[int]:
        s = [0]
        l, n = 0, len(nums)
        seg = []
        for r, x in enumerate(nums):
            if r == n - 1 or x > nums[r + 1]:
                seg.append(l)
                k = r - l + 1
                s.append(s[-1] + (1 + k) * k // 2)
                l = r + 1
        ans = []
        for l, r in queries:
            i = bisect_right(seg, l)
            j = bisect_right(seg, r) - 1
            if i > j:
                k = r - l + 1
                ans.append((1 + k) * k // 2)
            else:
                a = seg[i] - l
                b = r - seg[j] + 1
                ans.append((1 + a) * a // 2 + s[j] - s[i] + (1 + b) * b // 2)
        return ans
```

#### Java

```java
class Solution {
    public long[] countStableSubarrays(int[] nums, int[][] queries) {
        List<Integer> seg = new ArrayList<>();
        List<Long> s = new ArrayList<>();
        s.add(0L);

        int l = 0;
        int n = nums.length;
        for (int r = 0; r < n; r++) {
            if (r == n - 1 || nums[r] > nums[r + 1]) {
                seg.add(l);
                int k = r - l + 1;
                s.add(s.getLast() + (long) k * (k + 1) / 2);
                l = r + 1;
            }
        }

        long[] ans = new long[queries.length];
        for (int q = 0; q < queries.length; q++) {
            int left = queries[q][0];
            int right = queries[q][1];

            int i = upperBound(seg, left);
            int j = upperBound(seg, right) - 1;

            if (i > j) {
                int k = right - left + 1;
                ans[q] = (long) k * (k + 1) / 2;
            } else {
                int a = seg.get(i) - left;
                int b = right - seg.get(j) + 1;
                ans[q] = (long) a * (a + 1) / 2 + s.get(j) - s.get(i) + (long) b * (b + 1) / 2;
            }
        }
        return ans;
    }

    private int upperBound(List<Integer> list, int target) {
        int l = 0, r = list.size();
        while (l < r) {
            int mid = (l + r) >> 1;
            if (list.get(mid) > target) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<long long> countStableSubarrays(vector<int>& nums, vector<vector<int>>& queries) {
        int n = nums.size();
        vector<int> seg;
        vector<long long> s = {0};

        int l = 0;
        for (int r = 0; r < n; ++r) {
            if (r == n - 1 || nums[r] > nums[r + 1]) {
                seg.push_back(l);
                long long k = r - l + 1;
                s.push_back(s.back() + k * (k + 1) / 2);
                l = r + 1;
            }
        }

        vector<long long> ans;
        for (auto& q : queries) {
            int left = q[0], right = q[1];

            int i = upper_bound(seg.begin(), seg.end(), left) - seg.begin();
            int j = upper_bound(seg.begin(), seg.end(), right) - seg.begin() - 1;

            if (i > j) {
                long long k = right - left + 1;
                ans.push_back(k * (k + 1) / 2);
            } else {
                long long a = seg[i] - left;
                long long b = right - seg[j] + 1;
                ans.push_back(a * (a + 1) / 2 + s[j] - s[i] + b * (b + 1) / 2);
            }
        }

        return ans;
    }
};
```

#### Go

```go
func countStableSubarrays(nums []int, queries [][]int) []int64 {
    n := len(nums)
    seg := []int{}
    s := []int64{0}

    l := 0
    for r := 0; r < n; r++ {
        if r == n-1 || nums[r] > nums[r+1] {
            seg = append(seg, l)
            k := int64(r - l + 1)
            s = append(s, s[len(s)-1]+k*(k+1)/2)
            l = r + 1
        }
    }

    ans := make([]int64, len(queries))
    for idx, q := range queries {
        left, right := q[0], q[1]

        i := sort.SearchInts(seg, left+1)
        j := sort.SearchInts(seg, right+1) - 1

        if i > j {
            k := int64(right - left + 1)
            ans[idx] = k * (k + 1) / 2
        } else {
            a := int64(seg[i] - left)
            b := int64(right - seg[j] + 1)
            ans[idx] = a*(a+1)/2 + s[j] - s[i] + b*(b+1)/2
        }
    }

    return ans
}
```

#### TypeScript

```ts
function countStableSubarrays(nums: number[], queries: number[][]): number[] {
    const n = nums.length;
    const seg: number[] = [];
    const s: number[] = [0];

    let l = 0;
    for (let r = 0; r < n; r++) {
        if (r === n - 1 || nums[r] > nums[r + 1]) {
            seg.push(l);
            const k = r - l + 1;
            s.push(s[s.length - 1] + (k * (k + 1)) / 2);
            l = r + 1;
        }
    }

    const ans: number[] = [];
    for (const [left, right] of queries) {
        const i = _.sortedIndex(seg, left + 1);
        const j = _.sortedIndex(seg, right + 1) - 1;

        if (i > j) {
            const k = right - left + 1;
            ans.push((k * (k + 1)) / 2);
        } else {
            const a = seg[i] - left;
            const b = right - seg[j] + 1;
            ans.push((a * (a + 1)) / 2 + s[j] - s[i] + (b * (b + 1)) / 2);
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
