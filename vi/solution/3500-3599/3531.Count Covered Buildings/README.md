---
comments: true
difficulty: Medium
rating: 1518
source: Weekly Contest 447 Q1
tags:
    - Array
    - Hash Table
    - Sorting
---

<!-- problem:start -->

# [3531. Count Covered Buildings](https://leetcode.com/problems/count-covered-buildings)

[中文文档](/solution/3500-3599/3531.Count%20Covered%20Buildings/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên dương <code>n</code>, biểu thị một thành phố có kích thước <code>n x n</code>. Bạn cũng được cho một lưới 2D <code>buildings</code>, trong đó <code>buildings[i] = [x, y]</code> biểu thị một tòa nhà <strong>duy nhất</strong> nằm tại tọa độ <code>[x, y]</code>.</p>

<p>Một tòa nhà được <strong>bao quanh</strong> nếu có ít nhất một tòa nhà ở cả <strong>bốn</strong> hướng: bên trái, bên phải, phía trên và phía dưới.</p>

<p>Trả về số lượng tòa nhà <strong>được bao quanh</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3531.Count%20Covered%20Buildings/images/telegram-cloud-photo-size-5-6212982906394101085-m.jpg" style="width: 200px; height: 204px;" /></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, buildings = [[1,2],[2,2],[3,2],[2,1],[2,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Chỉ có tòa nhà <code>[2,2]</code> được bao quanh vì có ít nhất một tòa nhà:

    <ul>
         <li>phía trên (<code>[1,2]</code>)</li>
         <li>phía dưới (<code>[3,2]</code>)</li>
         <li>bên trái (<code>[2,1]</code>)</li>
         <li>bên phải (<code>[2,3]</code>)</li>
    </ul>
    </li>
    <li>Do đó, số lượng tòa nhà được bao quanh là 1.</li>

</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3531.Count%20Covered%20Buildings/images/telegram-cloud-photo-size-5-6212982906394101086-m.jpg" style="width: 200px; height: 204px;" /></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, buildings = [[1,1],[1,2],[2,1],[2,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Không có tòa nhà nào có ít nhất một tòa nhà ở cả bốn hướng.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3531.Count%20Covered%20Buildings/images/telegram-cloud-photo-size-5-6248862251436067566-x.jpg" style="width: 202px; height: 205px;" /></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, buildings = [[1,3],[3,2],[3,3],[3,5],[5,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Chỉ có tòa nhà <code>[3,3]</code> được bao quanh vì có ít nhất một tòa nhà:

    <ul>
         <li>phía trên (<code>[1,3]</code>)</li>
         <li>phía dưới (<code>[5,3]</code>)</li>
         <li>bên trái (<code>[3,2]</code>)</li>
         <li>bên phải (<code>[3,5]</code>)</li>
    </ul>
    </li>
    <li>Do đó, số lượng tòa nhà được bao quanh là 1.</li>

</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= buildings.length &lt;= 10<sup>5</sup> </code></li>
    <li><code>buildings[i] = [x, y]</code></li>
    <li><code>1 &lt;= x, y &lt;= n</code></li>
    <li>Tất cả tọa độ của <code>buildings</code> là <strong>duy nhất</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Một tòa nhà được bao quanh khi có các tòa nhà khác ở bên trái và bên phải trên cùng một hàng, cũng như phía trên và phía dưới trên cùng một cột. Việc duyệt toàn bộ hàng và cột cho từng tòa nhà sẽ chậm.
>
> Nhóm theo $x$ và theo $y$, sau đó sắp xếp từng nhóm. $(x,y)$ được bao quanh khi và chỉ khi $y$ nằm giữa hai đầu mút của cột (không kể các đầu mút) và $x$ nằm giữa hai đầu mút của hàng (không kể các đầu mút).

<!-- thinking:end -->

Ta có thể nhóm các tòa nhà theo tọa độ x và tọa độ y, rồi lưu chúng vào hai hash table $\text{g1}$ và $\text{g2}$. Trong đó, $\text{g1[x]}$ biểu diễn tất cả tọa độ y của các tòa nhà có tọa độ x là $x$, còn $\text{g2[y]}$ biểu diễn tất cả tọa độ x của các tòa nhà có tọa độ y là $y$. Sau đó, ta sắp xếp các danh sách này.

Tiếp theo, ta duyệt qua tất cả tòa nhà. Với tòa nhà hiện tại $(x, y)$, ta lấy danh sách tọa độ y tương ứng $l_1$ từ $\text{g1}$ và danh sách tọa độ x $l_2$ từ $\text{g2}$. Ta kiểm tra các điều kiện để xác định tòa nhà có được bao quanh hay không. Một tòa nhà được bao quanh khi $l_2[0] < x < l_2[-1]$ và $l_1[0] < y < l_1[-1]$. Nếu đúng, ta tăng đáp án lên một.

Sau khi duyệt xong, ta trả về đáp án cuối cùng.

Độ phức tạp là $O(n \times \log n)$, còn độ phức tạp không gian là $O(n)$, trong đó $n$ là số lượng tòa nhà.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countCoveredBuildings(self, n: int, buildings: List[List[int]]) -> int:
        g1 = defaultdict(list)
        g2 = defaultdict(list)
        for x, y in buildings:
            g1[x].append(y)
            g2[y].append(x)
        for x in g1:
            g1[x].sort()
        for y in g2:
            g2[y].sort()
        ans = 0
        for x, y in buildings:
            l1 = g1[x]
            l2 = g2[y]
            if l2[0] < x < l2[-1] and l1[0] < y < l1[-1]:
                ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int countCoveredBuildings(int n, int[][] buildings) {
        Map<Integer, List<Integer>> g1 = new HashMap<>();
        Map<Integer, List<Integer>> g2 = new HashMap<>();

        for (int[] building : buildings) {
            int x = building[0], y = building[1];
            g1.computeIfAbsent(x, k -> new ArrayList<>()).add(y);
            g2.computeIfAbsent(y, k -> new ArrayList<>()).add(x);
        }

        for (var e : g1.entrySet()) {
            Collections.sort(e.getValue());
        }
        for (var e : g2.entrySet()) {
            Collections.sort(e.getValue());
        }

        int ans = 0;

        for (int[] building : buildings) {
            int x = building[0], y = building[1];
            List<Integer> l1 = g1.get(x);
            List<Integer> l2 = g2.get(y);

            if (l2.get(0) < x && x < l2.get(l2.size() - 1) && l1.get(0) < y
                && y < l1.get(l1.size() - 1)) {
                ans++;
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
    int countCoveredBuildings(int n, vector<vector<int>>& buildings) {
        unordered_map<int, vector<int>> g1;
        unordered_map<int, vector<int>> g2;

        for (const auto& building : buildings) {
            int x = building[0], y = building[1];
            g1[x].push_back(y);
            g2[y].push_back(x);
        }

        for (auto& e : g1) {
            sort(e.second.begin(), e.second.end());
        }
        for (auto& e : g2) {
            sort(e.second.begin(), e.second.end());
        }

        int ans = 0;

        for (const auto& building : buildings) {
            int x = building[0], y = building[1];
            const vector<int>& l1 = g1[x];
            const vector<int>& l2 = g2[y];

            if (l2[0] < x && x < l2[l2.size() - 1] && l1[0] < y && y < l1[l1.size() - 1]) {
                ans++;
            }
        }

        return ans;
    }
};
```

#### Go

```go
func countCoveredBuildings(n int, buildings [][]int) (ans int) {
    g1 := make(map[int][]int)
    g2 := make(map[int][]int)

    for _, building := range buildings {
        x, y := building[0], building[1]
        g1[x] = append(g1[x], y)
        g2[y] = append(g2[y], x)
    }

    for _, list := range g1 {
        sort.Ints(list)
    }
    for _, list := range g2 {
        sort.Ints(list)
    }

    for _, building := range buildings {
        x, y := building[0], building[1]
        l1 := g1[x]
        l2 := g2[y]

        if l2[0] < x && x < l2[len(l2)-1] && l1[0] < y && y < l1[len(l1)-1] {
            ans++
        }
    }
    return
}
```

#### TypeScript

```ts
function countCoveredBuildings(n: number, buildings: number[][]): number {
    const g1: Map<number, number[]> = new Map();
    const g2: Map<number, number[]> = new Map();

    for (const [x, y] of buildings) {
        if (!g1.has(x)) g1.set(x, []);
        g1.get(x)?.push(y);

        if (!g2.has(y)) g2.set(y, []);
        g2.get(y)?.push(x);
    }

    for (const list of g1.values()) {
        list.sort((a, b) => a - b);
    }
    for (const list of g2.values()) {
        list.sort((a, b) => a - b);
    }

    let ans = 0;

    for (const [x, y] of buildings) {
        const l1 = g1.get(x)!;
        const l2 = g2.get(y)!;

        if (l2[0] < x && x < l2[l2.length - 1] && l1[0] < y && y < l1[l1.length - 1]) {
            ans++;
        }
    }

    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn count_covered_buildings(_n: i32, buildings: Vec<Vec<i32>>) -> i32 {
        let mut g1: HashMap<i32, Vec<i32>> = HashMap::new();
        let mut g2: HashMap<i32, Vec<i32>> = HashMap::new();

        for b in &buildings {
            let x = b[0];
            let y = b[1];
            g1.entry(x).or_insert(Vec::new()).push(y);
            g2.entry(y).or_insert(Vec::new()).push(x);
        }

        for v in g1.values_mut() {
            v.sort();
        }
        for v in g2.values_mut() {
            v.sort();
        }

        let mut ans: i32 = 0;

        for b in &buildings {
            let x = b[0];
            let y = b[1];

            let l1 = g1.get(&x).unwrap();
            let l2 = g2.get(&y).unwrap();

            if l2[0] < x && x < l2[l2.len() - 1] && l1[0] < y && y < l1[l1.len() - 1] {
                ans += 1;
            }
        }

        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int CountCoveredBuildings(int n, int[][] buildings) {
        var g1 = new Dictionary<int, List<int>>();
        var g2 = new Dictionary<int, List<int>>();

        foreach (var b in buildings) {
            int x = b[0], y = b[1];

            if (!g1.ContainsKey(x)) {
                g1[x] = new List<int>();
            }
            g1[x].Add(y);

            if (!g2.ContainsKey(y)) {
                g2[y] = new List<int>();
            }
            g2[y].Add(x);
        }

        foreach (var kv in g1) {
            kv.Value.Sort();
        }
        foreach (var kv in g2) {
            kv.Value.Sort();
        }

        int ans = 0;

        foreach (var b in buildings) {
            int x = b[0], y = b[1];
            var l1 = g1[x];
            var l2 = g2[y];

            if (l2[0] < x && x < l2[l2.Count - 1] &&
                l1[0] < y && y < l1[l1.Count - 1]) {
                ans++;
            }
        }

        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
