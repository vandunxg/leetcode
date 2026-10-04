---
comments: true
difficulty: Hard
rating: 2533
source: Weekly Contest 349 Q4
tags:
    - Stack
    - Binary Indexed Tree
    - Segment Tree
    - Array
    - Binary Search
    - Sorting
    - Monotonic Stack
---

<!-- problem:start -->

# [2736. Maximum Sum Queries](https://leetcode.com/problems/maximum-sum-queries)

[中文文档](/solution/2700-2799/2736.Maximum%20Sum%20Queries/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>nums1</code> và <code>nums2</code> được đánh chỉ số từ <strong>0</strong>, cùng có độ dài <code>n</code>, và một <strong>mảng 2 chiều được đánh chỉ số từ 1</strong> <code>queries</code>, trong đó <code>queries[i] = [x<sub>i</sub>, y<sub>i</sub>]</code>.</p>

<p>Với truy vấn thứ <code>i<sup>th</sup></code>, hãy tìm <strong>giá trị lớn nhất</strong> của <code>nums1[j] + nums2[j]</code> trong tất cả các chỉ số <code>j</code> <code>(0 &lt;= j &lt; n)</code> sao cho <code>nums1[j] &gt;= x<sub>i</sub></code> và <code>nums2[j] &gt;= y<sub>i</sub></code>, hoặc <strong>-1</strong> nếu không có <code>j</code> nào thỏa mãn các ràng buộc.</p>

<p>Trả về <em>một mảng </em><code>answer</code><em>, trong đó </em><code>answer[i]</code><em> là đáp án của truy vấn thứ </em><code>i<sup>th</sup></code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [4,3,1,2], nums2 = [2,4,9,5], queries = [[4,1],[1,3],[2,5]]
<strong>Đầu ra:</strong> [6,10,7]
<strong>Giải thích:</strong>
Với truy vấn thứ 1 <code node="[object Object]">x<sub>i</sub> = 4</code>&nbsp;và&nbsp;<code node="[object Object]">y<sub>i</sub> = 1</code>, ta có thể chọn chỉ số&nbsp;<code node="[object Object]">j = 0</code>&nbsp;vì&nbsp;<code node="[object Object]">nums1[j] &gt;= 4</code>&nbsp;và&nbsp;<code node="[object Object]">nums2[j] &gt;= 1</code>. Tổng&nbsp;<code node="[object Object]">nums1[j] + nums2[j]</code>&nbsp;là 6, và có thể chứng minh rằng 6 là giá trị lớn nhất có thể đạt được.

Với truy vấn thứ 2 <code node="[object Object]">x<sub>i</sub> = 1</code>&nbsp;và&nbsp;<code node="[object Object]">y<sub>i</sub> = 3</code>, ta có thể chọn chỉ số&nbsp;<code node="[object Object]">j = 2</code>&nbsp;vì&nbsp;<code node="[object Object]">nums1[j] &gt;= 1</code>&nbsp;và&nbsp;<code node="[object Object]">nums2[j] &gt;= 3</code>. Tổng&nbsp;<code node="[object Object]">nums1[j] + nums2[j]</code>&nbsp;là 10, và có thể chứng minh rằng 10 là giá trị lớn nhất có thể đạt được.

Với truy vấn thứ 3 <code node="[object Object]">x<sub>i</sub> = 2</code>&nbsp;và&nbsp;<code node="[object Object]">y<sub>i</sub> = 5</code>, ta có thể chọn chỉ số&nbsp;<code node="[object Object]">j = 3</code>&nbsp;vì&nbsp;<code node="[object Object]">nums1[j] &gt;= 2</code>&nbsp;và&nbsp;<code node="[object Object]">nums2[j] &gt;= 5</code>. Tổng&nbsp;<code node="[object Object]">nums1[j] + nums2[j]</code>&nbsp;là 7, và có thể chứng minh rằng 7 là giá trị lớn nhất có thể đạt được.

Do đó, ta trả về&nbsp;<code node="[object Object]">[6,10,7]</code>.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [3,2,5], nums2 = [2,3,4], queries = [[4,4],[3,2],[1,1]]
<strong>Đầu ra:</strong> [9,9,9]
<strong>Giải thích:</strong> Trong ví dụ này, ta có thể sử dụng chỉ số&nbsp;<code node="[object Object]">j = 2</code>&nbsp;cho tất cả truy vấn vì nó thỏa mãn các ràng buộc của từng truy vấn.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [2,1], nums2 = [2,3], queries = [[3,3]]
<strong>Đầu ra:</strong> [-1]
<strong>Giải thích:</strong> Có một truy vấn trong ví dụ này với <code node="[object Object]">x<sub>i</sub></code> = 3 và <code node="[object Object]">y<sub>i</sub></code> = 3. Với mọi chỉ số, j, hoặc nums1[j] &lt; <code node="[object Object]">x<sub>i</sub></code> hoặc nums2[j] &lt; <code node="[object Object]">y<sub>i</sub></code>. Vì vậy, không có đáp án.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>nums1.length == nums2.length</code>&nbsp;</li>
	<li><code>n ==&nbsp;nums1.length&nbsp;</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums1[i], nums2[i] &lt;= 10<sup>9</sup>&nbsp;</code></li>
	<li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code>queries[i].length ==&nbsp;2</code></li>
	<li><code>x<sub>i</sub>&nbsp;== queries[i][1]</code></li>
	<li><code>y<sub>i</sub> == queries[i][2]</code></li>
	<li><code>1 &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Binary Indexed Tree

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi truy vấn yêu cầu tìm giá trị lớn nhất của $nums1+nums2$ trong các điểm có $nums1\ge x$ và $nums2\ge y$. Hai mảng có độ dài $10^5$, nên không thể duyệt mọi điểm cho từng truy vấn.
>
> Đây là bài toán tìm cực đại theo quan hệ trội hai chiều. Ta xử lý các điểm và truy vấn theo thứ tự giảm dần của tọa độ thứ nhất, để khi $(x,y)$ được xử lý, mọi điểm có $nums1\ge x$ đã được thêm vào. Binary Indexed Tree lưu giá trị lớn nhất của các tổng trên $nums2$ đã rời rạc hóa và đảo chiều, nhờ đó truy vấn được các giá trị $nums2\ge y$.

<!-- thinking:end -->

Bài toán này thuộc nhóm các bài toán về thứ tự bộ phận hai chiều.

Bài toán thứ tự bộ phận hai chiều được định nghĩa như sau: cho một số cặp điểm $(a_1, b_1)$, $(a_2, b_2)$, ..., $(a_n, b_n)$ và một quan hệ thứ tự bộ phận được xác định. Với một điểm $(a_i, b_i)$, ta cần tìm số lượng hoặc giá trị lớn nhất của các cặp điểm $(a_j, b_j)$ thỏa mãn quan hệ thứ tự bộ phận. Cụ thể:

$$
\left(a_{j}, b_{j}\right) \prec\left(a_{i}, b_{i}\right) \stackrel{\text { def }}{=} a_{j} \lesseqgtr a_{i} \text { and } b_{j} \lesseqgtr b_{i}
$$

Cách giải tổng quát cho các bài toán thứ tự bộ phận hai chiều là sắp xếp một chiều, rồi dùng một cấu trúc dữ liệu để xử lý chiều còn lại (cấu trúc dữ liệu này thường là Binary Indexed Tree).

Với bài toán này, ta có thể tạo một mảng $nums$, trong đó $nums[i]=(nums_1[i], nums_2[i])$, sau đó sắp xếp $nums$ theo thứ tự giảm dần của $nums_1$. Ta cũng sắp xếp các truy vấn $queries$ theo thứ tự giảm dần của $x$.

Tiếp theo, ta duyệt từng truy vấn $queries[i] = (x, y)$. Với truy vấn hiện tại, ta lần lượt thêm giá trị $nums_2$ của mọi phần tử trong $nums$ lớn hơn hoặc bằng $x$ vào Binary Indexed Tree. Binary Indexed Tree duy trì giá trị lớn nhất của $nums_1 + nums_2$ trên khoảng $nums_2$ đã được rời rạc hóa. Vì vậy, ta chỉ cần truy vấn giá trị lớn nhất tương ứng với khoảng lớn hơn hoặc bằng $y$ đã rời rạc hóa trong Binary Indexed Tree. Lưu ý rằng vì Binary Indexed Tree duy trì giá trị lớn nhất trên prefix, trong phần cài đặt ta thêm $nums_2$ theo thứ tự ngược lại.

Độ phức tạp thời gian là $O((n + m) \times \log n + m \times \log m)$, và độ phức tạp không gian là $O(n + m)$. Trong đó, $n$ là độ dài của mảng $nums$, còn $m$ là độ dài của mảng $queries$.

Bài toán tương tự:

- [2940. Find Building Where Alice and Bob Can Meet](https://github.com/doocs/leetcode/blob/main/solution/2900-2999/2940.Find%20Building%20Where%20Alice%20and%20Bob%20Can%20Meet/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class BinaryIndexedTree:
    __slots__ = ["n", "c"]

    def __init__(self, n: int):
        self.n = n
        self.c = [-1] * (n + 1)

    def update(self, x: int, v: int):
        while x <= self.n:
            self.c[x] = max(self.c[x], v)
            x += x & -x

    def query(self, x: int) -> int:
        mx = -1
        while x:
            mx = max(mx, self.c[x])
            x -= x & -x
        return mx


class Solution:
    def maximumSumQueries(
        self, nums1: List[int], nums2: List[int], queries: List[List[int]]
    ) -> List[int]:
        nums = sorted(zip(nums1, nums2), key=lambda x: -x[0])
        nums2.sort()
        n, m = len(nums1), len(queries)
        ans = [-1] * m
        j = 0
        tree = BinaryIndexedTree(n)
        for i in sorted(range(m), key=lambda i: -queries[i][0]):
            x, y = queries[i]
            while j < n and nums[j][0] >= x:
                k = n - bisect_left(nums2, nums[j][1])
                tree.update(k, nums[j][0] + nums[j][1])
                j += 1
            k = n - bisect_left(nums2, y)
            ans[i] = tree.query(k)
        return ans
```

#### Java

```java
class BinaryIndexedTree {
    private int n;
    private int[] c;

    public BinaryIndexedTree(int n) {
        this.n = n;
        c = new int[n + 1];
        Arrays.fill(c, -1);
    }

    public void update(int x, int v) {
        while (x <= n) {
            c[x] = Math.max(c[x], v);
            x += x & -x;
        }
    }

    public int query(int x) {
        int mx = -1;
        while (x > 0) {
            mx = Math.max(mx, c[x]);
            x -= x & -x;
        }
        return mx;
    }
}

class Solution {
    public int[] maximumSumQueries(int[] nums1, int[] nums2, int[][] queries) {
        int n = nums1.length;
        int[][] nums = new int[n][0];
        for (int i = 0; i < n; ++i) {
            nums[i] = new int[] {nums1[i], nums2[i]};
        }
        Arrays.sort(nums, (a, b) -> b[0] - a[0]);
        Arrays.sort(nums2);
        int m = queries.length;
        Integer[] idx = new Integer[m];
        for (int i = 0; i < m; ++i) {
            idx[i] = i;
        }
        Arrays.sort(idx, (i, j) -> queries[j][0] - queries[i][0]);
        int[] ans = new int[m];
        int j = 0;
        BinaryIndexedTree tree = new BinaryIndexedTree(n);
        for (int i : idx) {
            int x = queries[i][0], y = queries[i][1];
            for (; j < n && nums[j][0] >= x; ++j) {
                int k = n - Arrays.binarySearch(nums2, nums[j][1]);
                tree.update(k, nums[j][0] + nums[j][1]);
            }
            int p = Arrays.binarySearch(nums2, y);
            int k = p >= 0 ? n - p : n + p + 1;
            ans[i] = tree.query(k);
        }
        return ans;
    }
}
```

#### C++

```cpp
class BinaryIndexedTree {
private:
    int n;
    vector<int> c;

public:
    BinaryIndexedTree(int n) {
        this->n = n;
        c.resize(n + 1, -1);
    }

    void update(int x, int v) {
        while (x <= n) {
            c[x] = max(c[x], v);
            x += x & -x;
        }
    }

    int query(int x) {
        int mx = -1;
        while (x > 0) {
            mx = max(mx, c[x]);
            x -= x & -x;
        }
        return mx;
    }
};

class Solution {
public:
    vector<int> maximumSumQueries(vector<int>& nums1, vector<int>& nums2, vector<vector<int>>& queries) {
        vector<pair<int, int>> nums;
        int n = nums1.size(), m = queries.size();
        for (int i = 0; i < n; ++i) {
            nums.emplace_back(-nums1[i], nums2[i]);
        }
        sort(nums.begin(), nums.end());
        sort(nums2.begin(), nums2.end());
        vector<int> idx(m);
        iota(idx.begin(), idx.end(), 0);
        sort(idx.begin(), idx.end(), [&](int i, int j) { return queries[j][0] < queries[i][0]; });
        vector<int> ans(m);
        int j = 0;
        BinaryIndexedTree tree(n);
        for (int i : idx) {
            int x = queries[i][0], y = queries[i][1];
            for (; j < n && -nums[j].first >= x; ++j) {
                int k = nums2.end() - lower_bound(nums2.begin(), nums2.end(), nums[j].second);
                tree.update(k, -nums[j].first + nums[j].second);
            }
            int k = nums2.end() - lower_bound(nums2.begin(), nums2.end(), y);
            ans[i] = tree.query(k);
        }
        return ans;
    }
};
```

#### Go

```go
type BinaryIndexedTree struct {
    n int
    c []int
}

func NewBinaryIndexedTree(n int) BinaryIndexedTree {
    c := make([]int, n+1)
    for i := range c {
        c[i] = -1
    }
    return BinaryIndexedTree{n: n, c: c}
}

func (bit *BinaryIndexedTree) update(x, v int) {
    for x <= bit.n {
        bit.c[x] = max(bit.c[x], v)
        x += x & -x
    }
}

func (bit *BinaryIndexedTree) query(x int) int {
    mx := -1
    for x > 0 {
        mx = max(mx, bit.c[x])
        x -= x & -x
    }
    return mx
}

func maximumSumQueries(nums1 []int, nums2 []int, queries [][]int) []int {
    n, m := len(nums1), len(queries)
    nums := make([][2]int, n)
    for i := range nums {
        nums[i] = [2]int{nums1[i], nums2[i]}
    }
    sort.Slice(nums, func(i, j int) bool { return nums[j][0] < nums[i][0] })
    sort.Ints(nums2)
    idx := make([]int, m)
    for i := range idx {
        idx[i] = i
    }
    sort.Slice(idx, func(i, j int) bool { return queries[idx[j]][0] < queries[idx[i]][0] })
    tree := NewBinaryIndexedTree(n)
    ans := make([]int, m)
    j := 0
    for _, i := range idx {
        x, y := queries[i][0], queries[i][1]
        for ; j < n && nums[j][0] >= x; j++ {
            k := n - sort.SearchInts(nums2, nums[j][1])
            tree.update(k, nums[j][0]+nums[j][1])
        }
        k := n - sort.SearchInts(nums2, y)
        ans[i] = tree.query(k)
    }
    return ans
}
```

#### TypeScript

```ts
class BinaryIndexedTree {
    private n: number;
    private c: number[];

    constructor(n: number) {
        this.n = n;
        this.c = Array(n + 1).fill(-1);
    }

    update(x: number, v: number): void {
        while (x <= this.n) {
            this.c[x] = Math.max(this.c[x], v);
            x += x & -x;
        }
    }

    query(x: number): number {
        let mx = -1;
        while (x > 0) {
            mx = Math.max(mx, this.c[x]);
            x -= x & -x;
        }
        return mx;
    }
}

function maximumSumQueries(nums1: number[], nums2: number[], queries: number[][]): number[] {
    const n = nums1.length;
    const m = queries.length;
    const nums: [number, number][] = [];
    for (let i = 0; i < n; ++i) {
        nums.push([nums1[i], nums2[i]]);
    }
    nums.sort((a, b) => b[0] - a[0]);
    nums2.sort((a, b) => a - b);
    const idx: number[] = Array(m)
        .fill(0)
        .map((_, i) => i);
    idx.sort((i, j) => queries[j][0] - queries[i][0]);
    const ans: number[] = Array(m).fill(0);
    let j = 0;
    const search = (x: number) => {
        let [l, r] = [0, n];
        while (l < r) {
            const mid = (l + r) >> 1;
            if (nums2[mid] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    };
    const tree = new BinaryIndexedTree(n);
    for (const i of idx) {
        const [x, y] = queries[i];
        for (; j < n && nums[j][0] >= x; ++j) {
            const k = n - search(nums[j][1]);
            tree.update(k, nums[j][0] + nums[j][1]);
        }
        const k = n - search(y);
        ans[i] = tree.query(k);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Sắp xếp + Ngăn xếp đơn điệu + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Binary Indexed Tree cần rời rạc hóa và đảo chiều chỉ số. Sau khi loại bỏ các điểm theo thứ tự giảm dần của $nums1$, một ngăn xếp đơn điệu giữ các ứng viên có $nums2$ tăng dần và tổng giảm dần, đồng thời loại bỏ điểm có $nums2$ nhỏ hơn nhưng không tốt hơn. Với mỗi truy vấn, ta dùng tìm kiếm nhị phân để tìm phần tử đầu tiên trong stack có $nums2\ge y$.

<!-- thinking:end -->

Ta xử lý các truy vấn theo thứ tự giảm dần của ngưỡng $x$ tương ứng.
Đồng thời, ta cũng sắp xếp các cặp số theo thứ tự giảm dần của $\textit{nums1}[i]$.

Với mỗi $query_j$, tất cả các cặp số thỏa mãn $\textit{nums1}[i] \geq x_j$ được thêm vào một ngăn xếp đơn điệu.

Ngăn xếp này có thứ tự tăng dần theo $\textit{nums2}[i]$ nhưng giảm dần theo $\textit{nums1}[i] + \textit{nums2}[i]$, đảm bảo rằng mọi ứng viên có $\textit{nums2}[i]$ lớn hơn
đều có $\textit{nums1}[i] + \textit{nums2}[i]$ nhỏ hơn, vì vậy chỉ giữ lại các ứng viên hiệu quả.

Với mỗi $query_j$, tìm kiếm nhị phân xác định phần tử đầu tiên trong stack có $\textit{nums2}[i] \geq y_j$. Giá trị $\textit{nums1}[i] + \textit{nums2}[i]$ tương ứng là đáp án.

### Độ phức tạp thời gian và không gian

Trong đó, $n$ là độ dài của mảng $nums2$, còn $m$ là độ dài của mảng $queries$.

- Độ phức tạp thời gian: $O((n + m) \times \log n + m \times \log m)$.
- Độ phức tạp không gian: $O(n + m)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumSumQueries(
        self, nums1: list[int], nums2: list[int], queries: list[list[int]]
    ) -> list[int]:
        max_values = [-1] * len(queries)

        queries = [(query[0], query[1], idx) for idx, query in enumerate(queries)]
        # Process queries by descending x threshold and y threshold.
        queries.sort(key=lambda x: (-x[0], -x[1]))

        tuples: list[tuple[int, int]] = []  # Format: (num 1, num 2).
        for num_1, num_2 in zip(nums1, nums2):
            tuples.append((num_1, num_2))

        # Process queries by descending num 1 and num 2.
        # Sort by ascending num 1 and num 2 to pop from the back.
        tuples.sort(key=lambda x: (x[0], x[1]))

        stack: list[tuple[int, int]] = []  # Format: (num 2, sum).

        for query_1, query_2, query_idx in queries:
            while tuples and tuples[-1][0] >= query_1:  # Tuple's num 1 >= x threshold.
                num_1, num_2 = tuples.pop(-1)
                nums_sum = num_1 + num_2

                while stack and stack[-1][0] < num_2 and stack[-1][1] <= nums_sum:
                    stack.pop(-1)  # Stack top isn't better than popped tuple.

                insertion_idx = bisect_left(stack, (num_2, nums_sum))

                if insertion_idx == len(stack):
                    stack.insert(insertion_idx, (num_2, nums_sum))

                elif stack[insertion_idx][1] < nums_sum:
                    stack.insert(insertion_idx, (num_2, nums_sum))

            search_idx = bisect_left(stack, (query_2, 0))
            if search_idx < len(stack):
                max_values[query_idx] = stack[search_idx][1]

        return max_values
```

#### C++

```cpp
class Solution {
public:
    vector<int> maximumSumQueries(vector<int>& nums1, vector<int>& nums2, vector<vector<int>>& queries) {
        vector<int> maxValues(queries.size(), -1);

        vector<vector<int>> queriesIndices;
        for (int idx = 0; idx < queries.size(); idx++)
            queriesIndices.push_back({queries[idx][0], queries[idx][1], idx});

        // Process queries by descending x threshold and y threshold.
        // Sort ascendingly and later pop from the back.
        sort(queriesIndices.begin(), queriesIndices.end());

        vector<pair<int, int>> numsPairs; // Format: {num 1, num 2}.
        for (int idx = 0; idx < nums2.size(); idx++)
            numsPairs.push_back({nums1[idx], nums2[idx]});

        // Process queries by descending num 1 and num 2.
        // Sort by ascending num 1 and num 2 to pop from the back.
        sort(numsPairs.begin(), numsPairs.end());

        deque<pair<int, int>> stack; // Format: {num 2, sum}.

        while (!queriesIndices.empty()) {
            int queryOne = queriesIndices.back()[0];
            int queryTwo = queriesIndices.back()[1];
            int queryIdx = queriesIndices.back()[2];
            queriesIndices.pop_back();

            // Pair's num 1 >= x threshold.
            while (!numsPairs.empty() && numsPairs.back().first >= queryOne) {
                auto [numOne, numTwo] = numsPairs.back();
                numsPairs.pop_back();
                int numsSum = numOne + numTwo;

                while (!stack.empty() and stack.back().first < numTwo and stack.back().second <= numsSum)
                    stack.pop_back(); // Stack top isn't better than popped pair.

                pair<int, int> targetPair = {numTwo, numsSum};
                int insertion_idx = lower_bound(stack.begin(), stack.end(), targetPair) - stack.begin();

                if (insertion_idx == stack.size())
                    stack.insert(stack.begin() + insertion_idx, targetPair);

                else if (stack[insertion_idx].second < numsSum)
                    stack.insert(stack.begin() + insertion_idx, targetPair);
            }

            pair<int, int> queryNumTwoPair = {queryTwo, 0};

            int search_idx = lower_bound(stack.begin(), stack.end(), queryNumTwoPair) - stack.begin();
            if (search_idx < stack.size())
                maxValues[queryIdx] = stack[search_idx].second;
        }

        return maxValues;
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
