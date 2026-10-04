---
comments: true
difficulty: Medium
rating: 1579
source: Weekly Contest 459 Q2
tags:
    - Geometry
    - Array
    - Hash Table
    - Math
---

<!-- problem:start -->

# [3623. Count Number of Trapezoids I](https://leetcode.com/problems/count-number-of-trapezoids-i)

[中文文档](/solution/3600-3699/3623.Count%20Number%20of%20Trapezoids%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên 2D <code>points</code>, trong đó <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> biểu diễn tọa độ của điểm thứ <code>i<sup>th</sup></code> trên mặt phẳng Cartesian.</p>

<p data-end="579" data-start="405">Một <strong>hình thang</strong> <strong>ngang</strong> là một tứ giác lồi có <strong data-end="496" data-start="475">ít nhất một cặp</strong> cạnh nằm ngang (tức là song song với trục x). Hai đường thẳng song song khi và chỉ khi chúng có cùng hệ số góc.</p>

<p data-end="579" data-start="405">Hãy trả về <em data-end="330" data-start="297"> số lượng </em><strong><em>hình thang</em> <em>ngang</em></strong> phân biệt có thể tạo thành bằng cách chọn bốn điểm phân biệt bất kỳ từ <code>points</code>.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">points = [[1,0],[2,0],[3,0],[2,2],[3,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3600-3699/3623.Count%20Number%20of%20Trapezoids%20I/images/desmos-graph-6.png" style="width: 250px; height: 250px;" /> <img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3600-3699/3623.Count%20Number%20of%20Trapezoids%20I/images/desmos-graph-7.png" style="width: 250px; height: 250px;" /> <img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3600-3699/3623.Count%20Number%20of%20Trapezoids%20I/images/desmos-graph-8.png" style="width: 250px; height: 250px;" /></p>

<p>Có ba cách phân biệt để chọn bốn điểm tạo thành một hình thang ngang:</p>

<ul>
    <li data-end="247" data-start="193">Chọn các điểm <code data-end="213" data-start="206">[1,0]</code>, <code data-end="222" data-start="215">[2,0]</code>, <code data-end="231" data-start="224">[3,2]</code> và <code data-end="244" data-start="237">[2,2]</code>.</li>
    <li data-end="305" data-start="251">Chọn các điểm <code data-end="271" data-start="264">[2,0]</code>, <code data-end="280" data-start="273">[3,0]</code>, <code data-end="289" data-start="282">[3,2]</code> và <code data-end="302" data-start="295">[2,2]</code>.</li>
    <li data-end="361" data-start="309">Chọn các điểm <code data-end="329" data-start="322">[1,0]</code>, <code data-end="338" data-start="331">[3,0]</code>, <code data-end="347" data-start="340">[3,2]</code> và <code data-end="360" data-start="353">[2,2]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">points = [[0,0],[1,0],[0,1],[2,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3600-3699/3623.Count%20Number%20of%20Trapezoids%20I/images/desmos-graph-5.png" style="width: 250px; height: 250px;" /></p>

<p>Chỉ có thể tạo thành một hình thang ngang duy nhất.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>4 &lt;= points.length &lt;= 10<sup>5</sup></code></li>
    <li><code>&ndash;10<sup>8</sup> &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= 10<sup>8</sup></code></li>
    <li>Tất cả các điểm đôi một phân biệt.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ các hình thang có hai cạnh nằm ngang được tính, nên một cạnh ngang là một cặp điểm có cùng $y$. Việc liệt kê bốn điểm không phù hợp với $n\le 10^5$.
>
> Nhóm các điểm theo $y$; một nhóm có $v$ điểm đóng góp $\binom{v}{2}$ cạnh ngang. Hai cạnh như vậy ở hai giá trị $y$ khác nhau xác định một hình thang.
>
> Duyệt các nhóm, gọi $s$ là số cạnh ngang đã gặp, cộng $s\cdot t$ với nhóm hiện tại có $t$ cạnh, sau đó cộng $t$ vào $s$. Lấy modulo $10^9+7$.

<!-- thinking:end -->

Theo mô tả bài toán, các cạnh ngang có cùng tọa độ $y$. Vì vậy, ta có thể nhóm các điểm theo tọa độ $y$ và đếm số điểm với mỗi tọa độ $y$.

Ta dùng một hash table $\textit{cnt}$ để lưu số điểm tương ứng với mỗi tọa độ $y$. Với mỗi tọa độ $y$, ký hiệu là $y_i$, giả sử số điểm tương ứng là $v$, số cách chọn hai điểm trong các điểm này làm một cạnh ngang là $\binom{v}{2} = \frac{v(v-1)}{2}$, ký hiệu là $t$.

Ta dùng biến $s$ để lưu tổng số cạnh ngang của tất cả các tọa độ $y$ trước đó. Sau đó, ta nhân số cạnh ngang $t$ của tọa độ $y$ hiện tại với tổng $s$ số cạnh ngang của tất cả các tọa độ $y$ trước đó để tính số hình thang có tọa độ $y$ hiện tại là một cặp cạnh ngang, rồi cộng kết quả vào đáp án. Cuối cùng, ta cộng số cạnh ngang $t$ của tọa độ $y$ hiện tại vào $s$ để dùng cho các phép tính tiếp theo.

Lưu ý rằng vì đáp án có thể rất lớn, ta cần lấy modulo $10^9 + 7$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số điểm.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countTrapezoids(self, points: List[List[int]]) -> int:
        mod = 10**9 + 7
        cnt = Counter(p[1] for p in points)
        ans = s = 0
        for v in cnt.values():
            t = v * (v - 1) // 2
            ans = (ans + s * t) % mod
            s += t
        return ans
```

#### Java

```java
class Solution {
    public int countTrapezoids(int[][] points) {
        final int mod = (int) 1e9 + 7;
        Map<Integer, Integer> cnt = new HashMap<>();
        for (var p : points) {
            cnt.merge(p[1], 1, Integer::sum);
        }
        long ans = 0, s = 0;
        for (int v : cnt.values()) {
            long t = 1L * v * (v - 1) / 2;
            ans = (ans + s * t) % mod;
            s += t;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countTrapezoids(vector<vector<int>>& points) {
        const int mod = 1e9 + 7;
        unordered_map<int, int> cnt;
        for (auto& p : points) {
            cnt[p[1]]++;
        }
        long long ans = 0, s = 0;
        for (auto& [_, v] : cnt) {
            long long t = 1LL * v * (v - 1) / 2;
            ans = (ans + s * t) % mod;
            s += t;
        }
        return (int) ans;
    }
};
```

#### Go

```go
func countTrapezoids(points [][]int) int {
    const mod = 1_000_000_007
    cnt := make(map[int]int)
    for _, p := range points {
        cnt[p[1]]++
    }

    var ans, s int64
    for _, v := range cnt {
        t := int64(v) * int64(v-1) / 2
        ans = (ans + s*t) % mod
        s += t
    }
    return int(ans)
}
```

#### TypeScript

```ts
function countTrapezoids(points: number[][]): number {
    const mod = 1_000_000_007;
    const cnt = new Map<number, number>();

    for (const p of points) {
        cnt.set(p[1], (cnt.get(p[1]) ?? 0) + 1);
    }

    let ans = 0;
    let s = 0;
    for (const v of cnt.values()) {
        const t = (v * (v - 1)) / 2;
        const mul = BigInt(s) * BigInt(t);
        ans = Number((BigInt(ans) + mul) % BigInt(mod));
        s += t;
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
