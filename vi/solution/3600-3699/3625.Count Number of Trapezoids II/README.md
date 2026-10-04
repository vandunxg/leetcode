---
comments: true
difficulty: Hard
rating: 2643
source: Weekly Contest 459 Q4
tags:
    - Geometry
    - Array
    - Hash Table
    - Math
---

<!-- problem:start -->

# [3625. Count Number of Trapezoids II](https://leetcode.com/problems/count-number-of-trapezoids-ii)

[中文文档](/solution/3600-3699/3625.Count%20Number%20of%20Trapezoids%20II/README.md)

## Mô tả

<!-- description:start -->

<p data-end="189" data-start="146">Bạn được cho một mảng số nguyên 2D <code>points</code>, trong đó <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> biểu diễn tọa độ của điểm thứ <code>i<sup>th</sup></code> trên mặt phẳng tọa độ.</p>

<p data-end="189" data-start="146">Trả về <em data-end="330" data-start="297">số lượng </em><em>hình thang</em> phân biệt có thể tạo thành bằng cách chọn bốn điểm phân biệt bất kỳ từ <code>points</code>.</p>

<p data-end="579" data-start="405"><b>Hình </b><strong>thang</strong> là một tứ giác lồi có <strong data-end="496" data-start="475">ít nhất một cặp</strong> cạnh song song. Hai đường thẳng song song khi và chỉ khi chúng có cùng hệ số góc.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">points = [[-3,2],[3,0],[2,3],[3,2],[2,-3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3600-3699/3625.Count%20Number%20of%20Trapezoids%20II/images/desmos-graph-4.png" style="width: 250px; height: 250px;" /> <img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3600-3699/3625.Count%20Number%20of%20Trapezoids%20II/images/desmos-graph-3.png" style="width: 250px; height: 250px;" /></p>

<p>Có hai cách khác nhau để chọn bốn điểm tạo thành một hình thang:</p>

<ul>
    <li>Các điểm <code>[-3,2], [2,3], [3,2], [2,-3]</code> tạo thành một hình thang.</li>
    <li>Các điểm <code>[2,3], [3,2], [3,0], [2,-3]</code> tạo thành một hình thang khác.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">points = [[0,0],[1,0],[0,1],[2,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3600-3699/3625.Count%20Number%20of%20Trapezoids%20II/images/desmos-graph-5.png" style="width: 250px; height: 250px;" /></p>

<p>Chỉ có thể tạo thành một hình thang duy nhất.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>4 &lt;= points.length &lt;= 500</code></li>
    <li><code>&ndash;1000 &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= 1000</code></li>
    <li>Tất cả các điểm đôi một phân biệt.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Một hình thang tổng quát có hai cặp cạnh song song. Việc liệt kê bốn điểm có độ phức tạp $O(n^4)$; ghép cặp các điểm có độ phức tạp $O(n^2)$ và chấp nhận được với $n\le 500$.
>
> Một đường thẳng được xác định bởi hệ số góc $k$ và hệ số tự do $b$. Các cặp đoạn thẳng có cùng $k$ và $b$ khác nhau tạo thành hình thang; mỗi hình bình hành bị đếm hai lần, một lần cho mỗi cặp cạnh, nên cần được trừ đi.
>
> Một hình bình hành có trung điểm hai đường chéo trùng nhau. $\textit{cnt1}[k][b]$ lưu các đường thẳng đi qua các cặp điểm; $\textit{cnt2}[p][k]$ lưu trung điểm và hệ số góc. Nhân số lượng các $b$ khác nhau với mỗi $k$, sau đó trừ đi tích của các hệ số góc khác nhau có cùng trung điểm.

<!-- thinking:end -->

Ta có thể ghép từng cặp điểm, tính hệ số góc và hệ số tự do của đường thẳng tương ứng với mỗi cặp điểm, lưu chúng bằng hash table, rồi tính tổng số cặp được tạo bởi các đường thẳng có cùng hệ số góc nhưng khác hệ số tự do. Lưu ý rằng với hình bình hành, phép tính trên sẽ đếm mỗi hình hai lần, do đó ta cần trừ chúng đi.

Hai đường chéo của một hình bình hành có cùng trung điểm. Vì vậy, ta cũng ghép từng cặp điểm, tính tọa độ trung điểm và hệ số góc của mỗi cặp điểm, lưu chúng bằng hash table, rồi tính tổng số cặp được tạo bởi các cặp điểm có cùng hệ số góc và cùng tọa độ trung điểm.

Cụ thể, ta sử dụng hai hash table $\textit{cnt1}$ và $\textit{cnt2}$ để lưu các thông tin sau:

- $\textit{cnt1}$ lưu số lần xuất hiện của hệ số góc $k$ và hệ số tự do $b$, với khóa là hệ số góc $k$ và giá trị là một hash table khác lưu số lần xuất hiện của hệ số tự do $b$;
- $\textit{cnt2}$ lưu số lần xuất hiện của tọa độ trung điểm và hệ số góc $k$ của các cặp điểm, với khóa là tọa độ trung điểm $p$ của cặp điểm và giá trị là một hash table khác lưu số lần xuất hiện của hệ số góc $k$.

Với một cặp điểm $(x_1, y_1)$ và $(x_2, y_2)$, ta đặt $dx = x_2 - x_1$ và $dy = y_2 - y_1$. Nếu $dx = 0$, điều đó có nghĩa là hai điểm nằm trên cùng một đường thẳng đứng, khi đó ta đặt hệ số góc $k = +\infty$ và hệ số tự do $b = x_1$; nếu không, hệ số góc $k = \frac{dy}{dx}$ và hệ số tự do $b = \frac{y_1 \cdot dx - x_1 \cdot dy}{dx}$. Tọa độ trung điểm $p$ của cặp điểm có thể được biểu diễn là $p = (x_1 + x_2 + 2000) \cdot 4000 + (y_1 + y_2 + 2000)$, trong đó cộng thêm offset để tránh số âm.

Tiếp theo, ta duyệt qua tất cả các cặp điểm, tính hệ số góc $k$, hệ số tự do $b$ và tọa độ trung điểm $p$ tương ứng, rồi cập nhật các hash table $\textit{cnt1}$ và $\textit{cnt2}$.

Sau đó, ta duyệt qua hash table $\textit{cnt1}$. Với mỗi hệ số góc $k$, ta tính tổng số tổ hợp từng cặp của số lần xuất hiện của hệ số tự do $b$ và cộng vào đáp án. Cuối cùng, ta duyệt qua hash table $\textit{cnt2}$. Với mỗi tọa độ trung điểm $p$, ta tính tổng số tổ hợp từng cặp của số lần xuất hiện của hệ số góc $k$ và trừ khỏi đáp án.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là số điểm.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countTrapezoids(self, points: List[List[int]]) -> int:
        n = len(points)

        # cnt1: k -> (b -> count)
        cnt1: dict[float, dict[float, int]] = defaultdict(lambda: defaultdict(int))
        # cnt2: p -> (k -> count)
        cnt2: dict[int, dict[float, int]] = defaultdict(lambda: defaultdict(int))

        for i in range(n):
            x1, y1 = points[i]
            for j in range(i):
                x2, y2 = points[j]
                dx, dy = x2 - x1, y2 - y1

                if dx == 0:
                    k = 1e9
                    b = x1
                else:
                    k = dy / dx
                    b = (y1 * dx - x1 * dy) / dx

                cnt1[k][b] += 1

                p = (x1 + x2 + 2000) * 4000 + (y1 + y2 + 2000)
                cnt2[p][k] += 1

        ans = 0

        for e in cnt1.values():
            s = 0
            for t in e.values():
                ans += s * t
                s += t

        for e in cnt2.values():
            s = 0
            for t in e.values():
                ans -= s * t
                s += t

        return ans
```

#### Java

```java
class Solution {
    public int countTrapezoids(int[][] points) {
        int n = points.length;
        Map<Double, Map<Double, Integer>> cnt1 = new HashMap<>(n * n);
        Map<Integer, Map<Double, Integer>> cnt2 = new HashMap<>(n * n);

        for (int i = 0; i < n; ++i) {
            int x1 = points[i][0], y1 = points[i][1];
            for (int j = 0; j < i; ++j) {
                int x2 = points[j][0], y2 = points[j][1];
                int dx = x2 - x1, dy = y2 - y1;
                double k = dx == 0 ? Double.MAX_VALUE : 1.0 * dy / dx;
                double b = dx == 0 ? x1 : 1.0 * (y1 * dx - x1 * dy) / dx;
                if (k == -0.0) {
                    k = 0.0;
                }
                if (b == -0.0) {
                    b = 0.0;
                }
                cnt1.computeIfAbsent(k, _ -> new HashMap<>()).merge(b, 1, Integer::sum);
                int p = (x1 + x2 + 2000) * 4000 + (y1 + y2 + 2000);
                cnt2.computeIfAbsent(p, _ -> new HashMap<>()).merge(k, 1, Integer::sum);
            }
        }

        int ans = 0;
        for (var e : cnt1.values()) {
            int s = 0;
            for (int t : e.values()) {
                ans += s * t;
                s += t;
            }
        }
        for (var e : cnt2.values()) {
            int s = 0;
            for (int t : e.values()) {
                ans -= s * t;
                s += t;
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
    int countTrapezoids(vector<vector<int>>& points) {
        int n = points.size();
        unordered_map<double, unordered_map<double, int>> cnt1;
        unordered_map<int, unordered_map<double, int>> cnt2;

        cnt1.reserve(n * n);
        cnt2.reserve(n * n);

        for (int i = 0; i < n; ++i) {
            int x1 = points[i][0], y1 = points[i][1];
            for (int j = 0; j < i; ++j) {
                int x2 = points[j][0], y2 = points[j][1];
                int dx = x2 - x1, dy = y2 - y1;
                double k = (dx == 0 ? 1e9 : 1.0 * dy / dx);
                double b = (dx == 0 ? x1 : 1.0 * (1LL * y1 * dx - 1LL * x1 * dy) / dx);

                cnt1[k][b] += 1;
                int p = (x1 + x2 + 2000) * 4000 + (y1 + y2 + 2000);
                cnt2[p][k] += 1;
            }
        }

        int ans = 0;
        for (auto& [_, e] : cnt1) {
            int s = 0;
            for (auto& [_, t] : e) {
                ans += s * t;
                s += t;
            }
        }
        for (auto& [_, e] : cnt2) {
            int s = 0;
            for (auto& [_, t] : e) {
                ans -= s * t;
                s += t;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countTrapezoids(points [][]int) int {
    n := len(points)
    cnt1 := make(map[float64]map[float64]int, n*n)
    cnt2 := make(map[int]map[float64]int, n*n)

    for i := 0; i < n; i++ {
        x1, y1 := points[i][0], points[i][1]
        for j := 0; j < i; j++ {
            x2, y2 := points[j][0], points[j][1]
            dx, dy := x2-x1, y2-y1

            var k, b float64
            if dx == 0 {
                k = 1e9
                b = float64(x1)
            } else {
                k = float64(dy) / float64(dx)
                b = float64(int64(y1)*int64(dx)-int64(x1)*int64(dy)) / float64(dx)
            }

            if cnt1[k] == nil {
                cnt1[k] = make(map[float64]int)
            }
            cnt1[k][b]++

            p := (x1+x2+2000)*4000 + (y1 + y2 + 2000)
            if cnt2[p] == nil {
                cnt2[p] = make(map[float64]int)
            }
            cnt2[p][k]++
        }
    }

    ans := 0
    for _, e := range cnt1 {
        s := 0
        for _, t := range e {
            ans += s * t
            s += t
        }
    }
    for _, e := range cnt2 {
        s := 0
        for _, t := range e {
            ans -= s * t
            s += t
        }
    }
    return ans
}
```

#### TypeScript

```ts
function countTrapezoids(points: number[][]): number {
    const n = points.length;

    const cnt1: Map<number, Map<number, number>> = new Map();
    const cnt2: Map<number, Map<number, number>> = new Map();

    for (let i = 0; i < n; i++) {
        const [x1, y1] = points[i];
        for (let j = 0; j < i; j++) {
            const [x2, y2] = points[j];
            const [dx, dy] = [x2 - x1, y2 - y1];

            const k = dx === 0 ? 1e9 : dy / dx;
            const b = dx === 0 ? x1 : (y1 * dx - x1 * dy) / dx;

            if (!cnt1.has(k)) {
                cnt1.set(k, new Map());
            }
            const mapB = cnt1.get(k)!;
            mapB.set(b, (mapB.get(b) || 0) + 1);

            const p = (x1 + x2 + 2000) * 4000 + (y1 + y2 + 2000);

            if (!cnt2.has(p)) {
                cnt2.set(p, new Map());
            }
            const mapK = cnt2.get(p)!;
            mapK.set(k, (mapK.get(k) || 0) + 1);
        }
    }

    let ans = 0;
    for (const e of cnt1.values()) {
        let s = 0;
        for (const t of e.values()) {
            ans += s * t;
            s += t;
        }
    }
    for (const e of cnt2.values()) {
        let s = 0;
        for (const t of e.values()) {
            ans -= s * t;
            s += t;
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
