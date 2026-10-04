---
comments: true
difficulty: Medium
rating: 1699
source: Weekly Contest 441 Q2
tags:
    - Array
    - Hash Table
    - Binary Search
---

<!-- problem:start -->

# [3488. Closest Equal Element Queries](https://leetcode.com/problems/closest-equal-element-queries)

[中文文档](/solution/3400-3499/3488.Closest%20Equal%20Element%20Queries/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <strong>vòng</strong> <code>nums</code> và một mảng <code>queries</code>.</p>

<p>Với mỗi truy vấn <code>i</code>, bạn cần tìm:</p>

<ul>
    <li><strong>Khoảng cách nhỏ nhất</strong> giữa phần tử tại chỉ số <code>queries[i]</code> và <strong>bất kỳ</strong> chỉ số <code>j</code> nào khác trong mảng <strong>vòng</strong>, sao cho <code>nums[j] == nums[queries[i]]</code>. Nếu không tồn tại chỉ số như vậy, kết quả của truy vấn đó là -1.</li>
</ul>

<p>Trả về một mảng <code>answer</code> có kích thước <strong>bằng</strong> <code>queries</code>, trong đó <code>answer[i]</code> là kết quả của truy vấn <code>i</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,3,1,4,1,3,2], queries = [0,3,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,-1,3]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Truy vấn 0: Phần tử tại <code>queries[0] = 0</code> là <code>nums[0] = 1</code>. Chỉ số gần nhất có cùng giá trị là 2, và khoảng cách giữa chúng là 2.</li>
    <li>Truy vấn 1: Phần tử tại <code>queries[1] = 3</code> là <code>nums[3] = 4</code>. Không có chỉ số nào khác chứa 4, nên kết quả là -1.</li>
    <li>Truy vấn 2: Phần tử tại <code>queries[2] = 5</code> là <code>nums[5] = 3</code>. Chỉ số gần nhất có cùng giá trị là 1, và khoảng cách giữa chúng là 3 (theo đường đi vòng: <code>5 -&gt; 6 -&gt; 0 -&gt; 1</code>).</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4], queries = [0,1,2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[-1,-1,-1,-1]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mỗi giá trị trong <code>nums</code> là duy nhất, nên không có chỉ số nào có cùng giá trị với phần tử được truy vấn. Vì vậy, kết quả của tất cả truy vấn đều là -1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= queries.length &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
    <li><code>0 &lt;= queries[i] &lt; nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng vòng + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Trên một mảng vòng, ta cần tìm khoảng cách từ mỗi chỉ số được truy vấn đến giá trị bằng nó gần nhất. Vì $n,q\le 10^5$, không thể duyệt toàn bộ mảng cho từng truy vấn.
>
> Nối thêm một bản sao của mảng sẽ mở mảng vòng thành bài toán tìm các lần xuất hiện gần nhất trên một đường thẳng.
>
> Hai lượt duyệt xuôi và ngược lưu lần xuất hiện gần nhất của cùng giá trị vào $\textit{d}$. Với chỉ số $i$, ta lấy $\min(\textit{d}[i],\textit{d}[i+n])$; nếu khoảng cách $\ge n$ thì giá trị đó chỉ xuất hiện một lần và đáp án là $-1$.

<!-- thinking:end -->

Theo mô tả bài toán, ta cần tìm khoảng cách nhỏ nhất giữa mỗi phần tử trong mảng với phần tử giống nó trước đó, cũng như khoảng cách nhỏ nhất đến phần tử giống nó tiếp theo. Vì mảng có tính chất vòng, ta cần xét đặc điểm này. Ta có thể mở rộng mảng thành một mảng có độ dài gấp đôi, sau đó dùng hai hash table $\textit{left}$ và $\textit{right}$ để lần lượt ghi nhận vị trí xuất hiện gần nhất trước đó và gần nhất tiếp theo của mỗi phần tử. Ta tính khoảng cách nhỏ nhất giữa phần tử tại mỗi vị trí và một phần tử giống nó, rồi lưu vào mảng $\textit{d}$. Cuối cùng, ta duyệt các truy vấn; với mỗi truy vấn $i$, ta lấy giá trị nhỏ hơn giữa $\textit{d}[i]$ và $\textit{d}[i+n]$. Nếu giá trị này lớn hơn hoặc bằng $n$, nghĩa là không có phần tử nào giống phần tử được truy vấn, nên ta trả về $-1$; ngược lại, ta trả về giá trị đó.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def solveQueries(self, nums: List[int], queries: List[int]) -> List[int]:
        n = len(nums)
        m = n << 1
        d = [m] * m
        left = {}
        for i in range(m):
            x = nums[i % n]
            if x in left:
                d[i] = min(d[i], i - left[x])
            left[x] = i
        right = {}
        for i in range(m - 1, -1, -1):
            x = nums[i % n]
            if x in right:
                d[i] = min(d[i], right[x] - i)
            right[x] = i
        for i in range(n):
            d[i] = min(d[i], d[i + n])
        return [-1 if d[i] >= n else d[i] for i in queries]
```

#### Java

```java
class Solution {
    public List<Integer> solveQueries(int[] nums, int[] queries) {
        int n = nums.length;
        int m = n * 2;
        int[] d = new int[m];
        Arrays.fill(d, m);

        Map<Integer, Integer> left = new HashMap<>();
        for (int i = 0; i < m; i++) {
            int x = nums[i % n];
            if (left.containsKey(x)) {
                d[i] = Math.min(d[i], i - left.get(x));
            }
            left.put(x, i);
        }

        Map<Integer, Integer> right = new HashMap<>();
        for (int i = m - 1; i >= 0; i--) {
            int x = nums[i % n];
            if (right.containsKey(x)) {
                d[i] = Math.min(d[i], right.get(x) - i);
            }
            right.put(x, i);
        }

        for (int i = 0; i < n; i++) {
            d[i] = Math.min(d[i], d[i + n]);
        }

        List<Integer> ans = new ArrayList<>();
        for (int query : queries) {
            ans.add(d[query] >= n ? -1 : d[query]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> solveQueries(vector<int>& nums, vector<int>& queries) {
        int n = nums.size();
        int m = n * 2;
        vector<int> d(m, m);

        unordered_map<int, int> left;
        for (int i = 0; i < m; i++) {
            int x = nums[i % n];
            if (left.count(x)) {
                d[i] = min(d[i], i - left[x]);
            }
            left[x] = i;
        }

        unordered_map<int, int> right;
        for (int i = m - 1; i >= 0; i--) {
            int x = nums[i % n];
            if (right.count(x)) {
                d[i] = min(d[i], right[x] - i);
            }
            right[x] = i;
        }

        for (int i = 0; i < n; i++) {
            d[i] = min(d[i], d[i + n]);
        }

        vector<int> ans;
        for (int query : queries) {
            ans.push_back(d[query] >= n ? -1 : d[query]);
        }
        return ans;
    }
};
```

#### Go

```go
func solveQueries(nums []int, queries []int) []int {
    n := len(nums)
    m := n * 2
    d := make([]int, m)
    for i := range d {
        d[i] = m
    }

    left := make(map[int]int)
    for i := 0; i < m; i++ {
        x := nums[i%n]
        if idx, exists := left[x]; exists {
            d[i] = min(d[i], i-idx)
        }
        left[x] = i
    }

    right := make(map[int]int)
    for i := m - 1; i >= 0; i-- {
        x := nums[i%n]
        if idx, exists := right[x]; exists {
            d[i] = min(d[i], idx-i)
        }
        right[x] = i
    }

    for i := 0; i < n; i++ {
        d[i] = min(d[i], d[i+n])
    }

    ans := make([]int, len(queries))
    for i, query := range queries {
        if d[query] >= n {
            ans[i] = -1
        } else {
            ans[i] = d[query]
        }
    }
    return ans
}
```

#### TypeScript

```ts
function solveQueries(nums: number[], queries: number[]): number[] {
    const n = nums.length;
    const m = n * 2;
    const d: number[] = Array(m).fill(m);

    const left = new Map<number, number>();
    for (let i = 0; i < m; i++) {
        const x = nums[i % n];
        if (left.has(x)) {
            d[i] = Math.min(d[i], i - left.get(x)!);
        }
        left.set(x, i);
    }

    const right = new Map<number, number>();
    for (let i = m - 1; i >= 0; i--) {
        const x = nums[i % n];
        if (right.has(x)) {
            d[i] = Math.min(d[i], right.get(x)! - i);
        }
        right.set(x, i);
    }

    for (let i = 0; i < n; i++) {
        d[i] = Math.min(d[i], d[i + n]);
    }

    return queries.map(query => (d[query] >= n ? -1 : d[query]));
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn solve_queries(nums: Vec<i32>, queries: Vec<i32>) -> Vec<i32> {
        let n = nums.len();
        let m = n * 2;
        let mut d = vec![m as i32; m];
        let mut left = HashMap::new();

        for i in 0..m {
            let x = nums[i % n];
            if let Some(&l) = left.get(&x) {
                d[i] = d[i].min((i - l) as i32);
            }
            left.insert(x, i);
        }

        let mut right = HashMap::new();

        for i in (0..m).rev() {
            let x = nums[i % n];
            if let Some(&r) = right.get(&x) {
                d[i] = d[i].min((r - i) as i32);
            }
            right.insert(x, i);
        }

        for i in 0..n {
            d[i] = d[i].min(d[i + n]);
        }

        queries.iter().map(|&query| {
            if d[query as usize] >= n as i32 {
                -1
            } else {
                d[query as usize]
            }
        }).collect()
    }
}
```

#### C#

```cs
public class Solution {
    public IList<int> SolveQueries(int[] nums, int[] queries) {
        int n = nums.Length;
        int m = n * 2;
        int[] d = new int[m];
        Array.Fill(d, m);

        Dictionary<int, int> left = new Dictionary<int, int>();
        for (int i = 0; i < m; i++) {
            int x = nums[i % n];
            if (left.ContainsKey(x)) {
                d[i] = Math.Min(d[i], i - left[x]);
            }
            left[x] = i;
        }

        Dictionary<int, int> right = new Dictionary<int, int>();
        for (int i = m - 1; i >= 0; i--) {
            int x = nums[i % n];
            if (right.ContainsKey(x)) {
                d[i] = Math.Min(d[i], right[x] - i);
            }
            right[x] = i;
        }

        for (int i = 0; i < n; i++) {
            d[i] = Math.Min(d[i], d[i + n]);
        }

        List<int> ans = new List<int>();
        foreach (int query in queries) {
            ans.Add(d[query] >= n ? -1 : d[query]);
        }

        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
