---
comments: true
difficulty: Medium
rating: 1658
source: Weekly Contest 447 Q2
tags:
    - Union Find
    - Graph
    - Array
    - Hash Table
    - Binary Search
---

<!-- problem:start -->

# [3532. Path Existence Queries in a Graph I](https://leetcode.com/problems/path-existence-queries-in-a-graph-i)

[中文文档](/solution/3500-3599/3532.Path%20Existence%20Queries%20in%20a%20Graph%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code> biểu thị số nút trong một đồ thị, được đánh số từ 0 đến <code>n - 1</code>.</p>

<p>Bạn cũng được cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>, được sắp xếp theo thứ tự <strong>không giảm</strong>, và một số nguyên <code>maxDiff</code>.</p>

<p>Một cạnh <strong>vô hướng</strong> tồn tại giữa các nút <code>i</code> và <code>j</code> nếu hiệu <strong>tuyệt đối</strong> giữa <code>nums[i]</code> và <code>nums[j]</code> <strong>không vượt quá</strong> <code>maxDiff</code> (tức là <code>|nums[i] - nums[j]| &lt;= maxDiff</code>).</p>

<p>Bạn cũng được cho một mảng số nguyên 2 chiều <code>queries</code>. Với mỗi <code>queries[i] = [u<sub>i</sub>, v<sub>i</sub>]</code>, hãy xác định xem có tồn tại đường đi giữa các nút <code>u<sub>i</sub></code> và <code>v<sub>i</sub></code> hay không.</p>

<p>Trả về một mảng boolean <code>answer</code>, trong đó <code>answer[i]</code> là <code>true</code> nếu tồn tại đường đi giữa <code>u<sub>i</sub></code> và <code>v<sub>i</sub></code> trong truy vấn thứ <code>i<sup>th</sup></code>, và là <code>false</code> nếu ngược lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2, nums = [1,3], maxDiff = 1, queries = [[0,0],[0,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[true,false]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Truy vấn <code>[0,0]</code>: Nút 0 có một đường đi tầm thường đến chính nó.</li>
    <li>Truy vấn <code>[0,1]</code>: Không có cạnh giữa nút 0 và nút 1 vì <code>|nums[0] - nums[1]| = |1 - 3| = 2</code>, lớn hơn <code>maxDiff</code>.</li>
    <li>Do đó, đáp án cuối cùng sau khi xử lý tất cả truy vấn là <code>[true, false]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, nums = [2,5,6,8], maxDiff = 2, queries = [[0,1],[0,2],[1,3],[2,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[false,false,true,true]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Đồ thị thu được là:</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3532.Path%20Existence%20Queries%20in%20a%20Graph%20I/images/screenshot-2025-03-26-at-122249.png" style="width: 300px; height: 170px;" /></p>

<ul>
    <li>Truy vấn <code>[0,1]</code>: Không có cạnh giữa nút 0 và nút 1 vì <code>|nums[0] - nums[1]| = |2 - 5| = 3</code>, lớn hơn <code>maxDiff</code>.</li>
    <li>Truy vấn <code>[0,2]</code>: Không có cạnh giữa nút 0 và nút 2 vì <code>|nums[0] - nums[2]| = |2 - 6| = 4</code>, lớn hơn <code>maxDiff</code>.</li>
    <li>Truy vấn <code>[1,3]</code>: Có một đường đi giữa nút 1 và nút 3 qua nút 2 vì <code>|nums[1] - nums[2]| = |5 - 6| = 1</code> và <code>|nums[2] - nums[3]| = |6 - 8| = 2</code>, cả hai đều không vượt quá <code>maxDiff</code>.</li>
    <li>Truy vấn <code>[2,3]</code>: Có một cạnh giữa nút 2 và nút 3 vì <code>|nums[2] - nums[3]| = |6 - 8| = 2</code>, bằng với <code>maxDiff</code>.</li>
    <li>Do đó, đáp án cuối cùng sau khi xử lý tất cả truy vấn là <code>[false, false, true, true]</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
    <li><code>0 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
    <li><code>nums</code> được sắp xếp theo thứ tự <strong>không giảm</strong>.</li>
    <li><code>0 &lt;= maxDiff &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
    <li><code>queries[i] == [u<sub>i</sub>, v<sub>i</sub>]</code></li>
    <li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt; n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân nhóm

<!-- thinking:start -->

> **Tư duy**
>
> $\textit{nums}$ đã được sắp xếp theo thứ tự không giảm, và các cạnh nối những giá trị cách nhau không quá $\textit{maxDiff}$, nên mỗi thành phần liên thông là một đoạn chỉ số liên tiếp.
>
> Quét từ trái sang phải và bắt đầu một mã nhóm mới khi khoảng cách giữa hai phần tử kề nhau vượt quá ngưỡng. Một truy vấn cho kết quả đúng khi và chỉ khi hai mã nhóm giống nhau.

<!-- thinking:end -->

Theo mô tả bài toán, các chỉ số nút trong cùng một thành phần liên thông phải liên tiếp. Vì vậy, ta có thể dùng một mảng $g$ để ghi lại chỉ số thành phần liên thông của mỗi nút và một biến $\textit{cnt}$ để theo dõi chỉ số của thành phần liên thông hiện tại. Khi duyệt qua mảng $\textit{nums}$, nếu hiệu giữa nút hiện tại và nút trước đó lớn hơn $\textit{maxDiff}$, điều đó cho biết nút hiện tại và nút trước đó không thuộc cùng một thành phần liên thông. Khi đó, ta tăng $\textit{cnt}$. Sau đó, ta gán chỉ số thành phần liên thông của nút hiện tại bằng $\textit{cnt}$.

Cuối cùng, với mỗi truy vấn $(u, v)$, ta chỉ cần kiểm tra xem $g[u]$ và $g[v]$ có bằng nhau hay không. Nếu bằng nhau, điều đó có nghĩa là $u$ và $v$ thuộc cùng một thành phần liên thông, và đáp án cho truy vấn thứ $i$ là $\text{true}$. Ngược lại, đáp án là $\text{false}$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def pathExistenceQueries(
        self, n: int, nums: List[int], maxDiff: int, queries: List[List[int]]
    ) -> List[bool]:
        g = [0] * n
        cnt = 0
        for i in range(1, n):
            if nums[i] - nums[i - 1] > maxDiff:
                cnt += 1
            g[i] = cnt
        return [g[u] == g[v] for u, v in queries]
```

#### Java

```java
class Solution {
    public boolean[] pathExistenceQueries(int n, int[] nums, int maxDiff, int[][] queries) {
        int[] g = new int[n];
        int cnt = 0;
        for (int i = 1; i < n; ++i) {
            if (nums[i] - nums[i - 1] > maxDiff) {
                cnt++;
            }
            g[i] = cnt;
        }

        int m = queries.length;
        boolean[] ans = new boolean[m];
        for (int i = 0; i < m; ++i) {
            int u = queries[i][0];
            int v = queries[i][1];
            ans[i] = g[u] == g[v];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<bool> pathExistenceQueries(int n, vector<int>& nums, int maxDiff, vector<vector<int>>& queries) {
        vector<int> g(n);
        int cnt = 0;
        for (int i = 1; i < n; ++i) {
            if (nums[i] - nums[i - 1] > maxDiff) {
                ++cnt;
            }
            g[i] = cnt;
        }

        vector<bool> ans;
        for (const auto& q : queries) {
            int u = q[0], v = q[1];
            ans.push_back(g[u] == g[v]);
        }
        return ans;
    }
};
```

#### Go

```go
func pathExistenceQueries(n int, nums []int, maxDiff int, queries [][]int) (ans []bool) {
    g := make([]int, n)
    cnt := 0
    for i := 1; i < n; i++ {
        if nums[i]-nums[i-1] > maxDiff {
            cnt++
        }
        g[i] = cnt
    }

    for _, q := range queries {
        u, v := q[0], q[1]
        ans = append(ans, g[u] == g[v])
    }
    return
}
```

#### TypeScript

```ts
function pathExistenceQueries(
    n: number,
    nums: number[],
    maxDiff: number,
    queries: number[][],
): boolean[] {
    const g: number[] = Array(n).fill(0);
    let cnt = 0;

    for (let i = 1; i < n; ++i) {
        if (nums[i] - nums[i - 1] > maxDiff) {
            ++cnt;
        }
        g[i] = cnt;
    }

    return queries.map(([u, v]) => g[u] === g[v]);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
