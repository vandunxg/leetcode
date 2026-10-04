---
comments: true
difficulty: Hard
rating: 2507
source: Weekly Contest 447 Q4
tags:
    - Greedy
    - Bit Manipulation
    - Graph
    - Array
    - Two Pointers
    - Binary Search
    - Dynamic Programming
    - Sorting
---

<!-- problem:start -->

# [3534. Path Existence Queries in a Graph II](https://leetcode.com/problems/path-existence-queries-in-a-graph-ii)

[中文文档](/solution/3500-3599/3534.Path%20Existence%20Queries%20in%20a%20Graph%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code> biểu thị số lượng node trong một đồ thị, được đánh số từ 0 đến <code>n - 1</code>.</p>

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code> và một số nguyên <code>maxDiff</code>.</p>

<p>Có một cạnh <strong>vô hướng</strong> giữa các node <code>i</code> và <code>j</code> nếu hiệu <strong>tuyệt đối</strong> giữa <code>nums[i]</code> và <code>nums[j]</code> <strong>không vượt quá</strong> <code>maxDiff</code> (tức là <code>|nums[i] - nums[j]| &lt;= maxDiff</code>).</p>

<p>Đồng thời, cho một mảng số nguyên 2D <code>queries</code>. Với mỗi <code>queries[i] = [u<sub>i</sub>, v<sub>i</sub>]</code>, hãy tìm khoảng cách <strong>nhỏ nhất</strong> giữa các node <code>u<sub>i</sub></code> và <code>v<sub>i</sub></code><sub>.</sub> Nếu không tồn tại đường đi giữa hai node, trả về -1 cho truy vấn đó.</p>

<p>Trả về một mảng <code>answer</code>, trong đó <code>answer[i]</code> là kết quả của truy vấn thứ <code>i<sup>th</sup></code>.</p>

<p><strong>Lưu ý:</strong> Các cạnh giữa các node là không có trọng số.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, nums = [1,8,3,4,2], maxDiff = 3, queries = [[0,3],[2,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,1]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Đồ thị thu được là:</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3534.Path%20Existence%20Queries%20in%20a%20Graph%20II/images/4149example1drawio.png" style="width: 281px; height: 161px;" /></p>

<table>
    <tbody>
        <tr>
            <th>Truy vấn</th>
            <th>Đường đi ngắn nhất</th>
            <th>Khoảng cách nhỏ nhất</th>
        </tr>
        <tr>
            <td>[0, 3]</td>
            <td>0 &rarr; 3</td>
            <td>1</td>
        </tr>
        <tr>
            <td>[2, 4]</td>
            <td>2 &rarr; 4</td>
            <td>1</td>
        </tr>
    </tbody>
</table>

<p>Vì vậy, kết quả là <code>[1, 1]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, nums = [5,3,1,9,10], maxDiff = 2, queries = [[0,1],[0,2],[2,3],[4,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,2,-1,1]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Đồ thị thu được là:</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3534.Path%20Existence%20Queries%20in%20a%20Graph%20II/images/4149example2drawio.png" style="width: 281px; height: 121px;" /></p>
</div>

<table>
    <tbody>
        <tr>
            <th>Truy vấn</th>
            <th>Đường đi ngắn nhất</th>
            <th>Khoảng cách nhỏ nhất</th>
        </tr>
        <tr>
            <td>[0, 1]</td>
            <td>0 &rarr; 1</td>
            <td>1</td>
        </tr>
        <tr>
            <td>[0, 2]</td>
            <td>0 &rarr; 1 &rarr; 2</td>
            <td>2</td>
        </tr>
        <tr>
            <td>[2, 3]</td>
            <td>Không có</td>
            <td>-1</td>
        </tr>
        <tr>
            <td>[4, 3]</td>
            <td>3 &rarr; 4</td>
            <td>1</td>
        </tr>
    </tbody>
</table>

<p>Vì vậy, kết quả là <code>[1, 2, -1, 1]</code>.</p>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, nums = [3,6,1], maxDiff = 1, queries = [[0,0],[0,1],[1,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,-1,-1]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có cạnh nào giữa hai node bất kỳ vì:</p>

<ul>
    <li>Node 0 và 1: <code>|nums[0] - nums[1]| = |3 - 6| = 3 &gt; 1</code></li>
    <li>Node 0 và 2: <code>|nums[0] - nums[2]| = |3 - 1| = 2 &gt; 1</code></li>
    <li>Node 1 và 2: <code>|nums[1] - nums[2]| = |6 - 1| = 5 &gt; 1</code></li>
</ul>

<p>Vì vậy, không node nào có thể đi đến node khác, và kết quả là <code>[0, -1, -1]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
    <li><code>0 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
    <li><code>0 &lt;= maxDiff &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
    <li><code>queries[i] == [u<sub>i</sub>, v<sub>i</sub>]</code></li>
    <li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt; n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Binary Lifting

<!-- thinking:start -->

> **Tư duy**
>
> Bài trước chỉ yêu cầu kiểm tra tính liên thông. Ở đây, chúng ta cần độ dài đường đi ngắn nhất cho nhiều truy vấn. Sau khi sắp xếp theo giá trị, mỗi bước từ một node có giá trị nhỏ hơn có thể nhảy đến node xa nhất trong phạm vi $\textit{maxDiff}$; cách nhảy tham lam này tạo ra một đường đi ngắn nhất.
>
> Hai con trỏ dùng để tính bước nhảy một lần; binary lifting xây dựng $f[i][k]$. Căn chỉnh node có giá trị nhỏ hơn, cộng các bước nhảy, rồi trả về $-1$ nếu vẫn không thể đến đích.

<!-- thinking:end -->

Nhận xét then chốt: tồn tại một cạnh giữa hai node nếu hiệu tuyệt đối giữa các giá trị của chúng không vượt quá `maxDiff`. Sau khi sắp xếp các node theo giá trị, việc liên tục nhảy từ node có giá trị nhỏ hơn đến node có giá trị lớn nhất có thể đi tới ở mỗi bước sẽ cho đường đi ngắn nhất.

Các bước tiền xử lý như sau:

1. Sắp xếp các cặp `(nums[i], i)` theo giá trị;
2. Sử dụng hai con trỏ: với mỗi vị trí đã sắp xếp `l`, tìm vị trí ngoài cùng bên phải `r` sao cho `pairs[r].first - pairs[l].first <= maxDiff`. Gán `f[i][0] = j`, nghĩa là từ node `i`, một bước nhảy sẽ đến node `j`, node có giá trị lớn nhất trong phạm vi `maxDiff`;
3. Xây dựng bảng binary lifting `f[i][k]`, biểu diễn node đạt được sau $2^k$ bước nhảy từ node `i`.

Với mỗi truy vấn, giả sử `nums[u] <= nums[v]`:

- Nếu `u == v`, đáp án là $0$;
- Nếu `nums[u] == nums[v]`, đáp án là $1$;
- Nếu không, sử dụng binary lifting để tìm số bước nhảy nhỏ nhất sao cho giá trị của node đạt được ít nhất bằng `nums[v]`; nếu không thể đi đến đó, trả về $-1$, ngược lại đáp án là $d + 1$.

Độ phức tạp thời gian là $O(n \log n + (n + q) \log n)$ và độ phức tạp không gian là $O(n \log n)$, trong đó $n$ là số lượng node và $q$ là số lượng truy vấn.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def pathExistenceQueries(
        self, n: int, nums: List[int], maxDiff: int, queries: List[List[int]]
    ) -> List[int]:
        pairs = sorted((x, i) for i, x in enumerate(nums))
        m = 20
        f = [[0] * m for _ in range(n)]
        r = n - 1
        for l in range(n - 1, -1, -1):
            while pairs[r][0] - pairs[l][0] > maxDiff:
                r -= 1
            i, j = pairs[l][1], pairs[r][1]
            f[i][0] = j
            for k in range(1, m):
                f[i][k] = f[f[i][k - 1]][k - 1]

        ans = []
        for i, j in queries:
            if nums[i] > nums[j]:
                i, j = j, i
            if i == j:
                ans.append(0)
                continue
            if nums[i] == nums[j]:
                ans.append(1)
                continue
            d = 0
            for k in range(m - 1, -1, -1):
                if nums[f[i][k]] < nums[j]:
                    d |= 1 << k
                    i = f[i][k]
            if nums[f[i][0]] < nums[j]:
                ans.append(-1)
            else:
                ans.append(d + 1)
        return ans
```

#### Java

```java
class Solution {
    public int[] pathExistenceQueries(int n, int[] nums, int maxDiff, int[][] queries) {
        int[][] pairs = new int[n][2];
        for (int i = 0; i < n; i++) {
            pairs[i][0] = nums[i];
            pairs[i][1] = i;
        }
        Arrays.sort(pairs, (a, b) -> a[0] - b[0]);

        int m = 20;
        int[][] f = new int[n][m];
        int r = n - 1;
        for (int l = n - 1; l >= 0; l--) {
            while (pairs[r][0] - pairs[l][0] > maxDiff) {
                r--;
            }
            int i = pairs[l][1], j = pairs[r][1];
            f[i][0] = j;
            for (int k = 1; k < m; k++) {
                f[i][k] = f[f[i][k - 1]][k - 1];
            }
        }

        int[] ans = new int[queries.length];
        for (int t = 0; t < queries.length; t++) {
            int i = queries[t][0], j = queries[t][1];
            if (nums[i] > nums[j]) {
                int tmp = i;
                i = j;
                j = tmp;
            }
            if (i == j) {
                ans[t] = 0;
                continue;
            }
            if (nums[i] == nums[j]) {
                ans[t] = 1;
                continue;
            }
            int d = 0;
            for (int k = m - 1; k >= 0; k--) {
                if (nums[f[i][k]] < nums[j]) {
                    d |= 1 << k;
                    i = f[i][k];
                }
            }
            if (nums[f[i][0]] < nums[j]) {
                ans[t] = -1;
            } else {
                ans[t] = d + 1;
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
    vector<int> pathExistenceQueries(int n, vector<int>& nums, int maxDiff, vector<vector<int>>& queries) {
        vector<pair<int, int>> pairs;
        for (int i = 0; i < n; i++) {
            pairs.emplace_back(nums[i], i);
        }
        sort(pairs.begin(), pairs.end());

        int m = 20;
        vector<vector<int>> f(n, vector<int>(m));
        int r = n - 1;
        for (int l = n - 1; l >= 0; l--) {
            while (pairs[r].first - pairs[l].first > maxDiff) {
                r--;
            }
            int i = pairs[l].second, j = pairs[r].second;
            f[i][0] = j;
            for (int k = 1; k < m; k++) {
                f[i][k] = f[f[i][k - 1]][k - 1];
            }
        }

        vector<int> ans;
        for (auto& q : queries) {
            int i = q[0], j = q[1];
            if (nums[i] > nums[j]) {
                swap(i, j);
            }
            if (i == j) {
                ans.push_back(0);
                continue;
            }
            if (nums[i] == nums[j]) {
                ans.push_back(1);
                continue;
            }
            int d = 0;
            for (int k = m - 1; k >= 0; k--) {
                if (nums[f[i][k]] < nums[j]) {
                    d |= 1 << k;
                    i = f[i][k];
                }
            }
            if (nums[f[i][0]] < nums[j]) {
                ans.push_back(-1);
            } else {
                ans.push_back(d + 1);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func pathExistenceQueries(n int, nums []int, maxDiff int, queries [][]int) []int {
    pairs := make([][2]int, n)
    for i, x := range nums {
        pairs[i] = [2]int{x, i}
    }
    sort.Slice(pairs, func(i, j int) bool {
        return pairs[i][0] < pairs[j][0]
    })

    m := 20
    f := make([][]int, n)
    for i := range f {
        f[i] = make([]int, m)
    }

    r := n - 1
    for l := n - 1; l >= 0; l-- {
        for pairs[r][0]-pairs[l][0] > maxDiff {
            r--
        }
        i, j := pairs[l][1], pairs[r][1]
        f[i][0] = j
        for k := 1; k < m; k++ {
            f[i][k] = f[f[i][k-1]][k-1]
        }
    }

    ans := make([]int, 0, len(queries))
    for _, q := range queries {
        i, j := q[0], q[1]
        if nums[i] > nums[j] {
            i, j = j, i
        }
        if i == j {
            ans = append(ans, 0)
            continue
        }
        if nums[i] == nums[j] {
            ans = append(ans, 1)
            continue
        }
        d := 0
        for k := m - 1; k >= 0; k-- {
            if nums[f[i][k]] < nums[j] {
                d |= 1 << k
                i = f[i][k]
            }
        }
        if nums[f[i][0]] < nums[j] {
            ans = append(ans, -1)
        } else {
            ans = append(ans, d+1)
        }
    }
    return ans
}
```

#### TypeScript

```ts
function pathExistenceQueries(
    n: number,
    nums: number[],
    maxDiff: number,
    queries: number[][],
): number[] {
    const pairs: number[][] = [];
    for (let i = 0; i < n; i++) {
        pairs.push([nums[i], i]);
    }
    pairs.sort((a, b) => a[0] - b[0]);

    const m = 20;
    const f = Array.from({ length: n }, () => Array(m).fill(0));

    let r = n - 1;
    for (let l = n - 1; l >= 0; l--) {
        while (pairs[r][0] - pairs[l][0] > maxDiff) {
            r--;
        }
        let i = pairs[l][1],
            j = pairs[r][1];
        f[i][0] = j;
        for (let k = 1; k < m; k++) {
            f[i][k] = f[f[i][k - 1]][k - 1];
        }
    }

    const ans: number[] = [];
    for (const q of queries) {
        let i = q[0],
            j = q[1];
        if (nums[i] > nums[j]) {
            [i, j] = [j, i];
        }
        if (i === j) {
            ans.push(0);
            continue;
        }
        if (nums[i] === nums[j]) {
            ans.push(1);
            continue;
        }
        let d = 0;
        for (let k = m - 1; k >= 0; k--) {
            if (nums[f[i][k]] < nums[j]) {
                d |= 1 << k;
                i = f[i][k];
            }
        }
        if (nums[f[i][0]] < nums[j]) {
            ans.push(-1);
        } else {
            ans.push(d + 1);
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn path_existence_queries(
        n: i32,
        nums: Vec<i32>,
        max_diff: i32,
        queries: Vec<Vec<i32>>,
    ) -> Vec<i32> {
        let n = n as usize;
        let mut pairs = Vec::with_capacity(n);
        for (i, &x) in nums.iter().enumerate() {
            pairs.push((x, i));
        }
        pairs.sort_unstable();

        let m = 20;
        let mut f = vec![vec![0; m]; n];

        let mut r = n - 1;
        for l in (0..n).rev() {
            while pairs[r].0 - pairs[l].0 > max_diff {
                r -= 1;
            }
            let (i, j) = (pairs[l].1, pairs[r].1);
            f[i][0] = j;
            for k in 1..m {
                f[i][k] = f[f[i][k - 1]][k - 1];
            }
        }

        let mut ans = Vec::with_capacity(queries.len());
        for q in queries {
            let (mut i, mut j) = (q[0] as usize, q[1] as usize);
            if nums[i] > nums[j] {
                std::mem::swap(&mut i, &mut j);
            }
            if i == j {
                ans.push(0);
                continue;
            }
            if nums[i] == nums[j] {
                ans.push(1);
                continue;
            }
            let mut d = 0;
            for k in (0..m).rev() {
                if nums[f[i][k]] < nums[j] {
                    d |= 1 << k;
                    i = f[i][k];
                }
            }
            if nums[f[i][0]] < nums[j] {
                ans.push(-1);
            } else {
                ans.push(d + 1);
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
