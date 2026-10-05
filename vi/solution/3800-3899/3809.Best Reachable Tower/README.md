---
comments: true
difficulty: Medium
rating: 1358
source: Biweekly Contest 174 Q1
tags:
    - Array
---

<!-- problem:start -->

# [3809. Best Reachable Tower](https://leetcode.com/problems/best-reachable-tower)

[中文文档](/solution/3800-3899/3809.Best%20Reachable%20Tower/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên 2 chiều <code>towers</code>, trong đó <code>towers[i] = [x<sub>i</sub>, y<sub>i</sub>, q<sub>i</sub>]</code> biểu diễn tọa độ <code>(x<sub>i</sub>, y<sub>i</sub>)</code> và hệ số chất lượng <code>q<sub>i</sub></code> của tòa tháp thứ <code>i<sup>th</sup></code>.</p>

<p>Bạn cũng được cho một mảng số nguyên <code>center = [cx, cy​​​​​​​]</code> biểu diễn vị trí của bạn và một số nguyên <code>radius</code>.</p>

<p>Một tòa tháp <strong>có thể tiếp cận</strong> nếu có <strong>khoảng cách Manhattan</strong> từ nó đến <code>center</code> <strong>nhỏ hơn hoặc bằng</strong> <code>radius</code>.</p>

<p>Trong số tất cả các tòa tháp có thể tiếp cận:</p>

<ul>
	<li>Trả về tọa độ của tòa tháp có hệ số chất lượng <strong>lớn nhất</strong>.</li>
	<li>Nếu có nhiều tòa tháp cùng chất lượng, trả về tòa tháp có tọa độ <strong>nhỏ nhất theo thứ tự từ điển</strong>. Nếu không có tòa tháp nào có thể tiếp cận, trả về <code>[-1, -1]</code>.</li>
</ul>
<strong>Khoảng cách Manhattan</strong> giữa hai ô <code>(x<sub>i</sub>, y<sub>i</sub>)</code> và <code>(x<sub>j</sub>, y<sub>j</sub>)</code> là <code>|x<sub>i</sub> - x<sub>j</sub>| + |y<sub>i</sub> - y<sub>j</sub>|</code>.

<p>Tọa độ <code>[x<sub>i</sub>, y<sub>i</sub>]</code> được gọi là <strong>nhỏ hơn theo thứ tự từ điển</strong> tọa độ <code>[x<sub>j</sub>, y<sub>j</sub>]</code> nếu <code>x<sub>i</sub> &lt; x<sub>j</sub></code>, hoặc <code>x<sub>i</sub> == x<sub>j</sub></code> và <code>y<sub>i</sub> &lt; y<sub>j</sub></code>.</p>

<p><code>|x|</code> biểu thị <strong>giá trị</strong> <strong>tuyệt đối</strong> của <code>x</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">towers = [[1,2,5], [2,1,7], [3,1,9]], center = [1,1], radius = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[3,1]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tòa tháp <code>[1, 2, 5]</code>: Khoảng cách Manhattan = <code>|1 - 1| + |2 - 1| = 1</code>, có thể tiếp cận.</li>
	<li>Tòa tháp <code>[2, 1, 7]</code>: Khoảng cách Manhattan = <code>|2 - 1| + |1 - 1| = 1</code>, có thể tiếp cận.</li>
	<li>Tòa tháp <code>[3, 1, 9]</code>: Khoảng cách Manhattan = <code>|3 - 1| + |1 - 1| = 2</code>, có thể tiếp cận.</li>
</ul>

<p>Tất cả các tòa tháp đều có thể tiếp cận. Hệ số chất lượng lớn nhất là 9, thuộc về tòa tháp <code>[3, 1]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">towers = [[1,3,4], [2,2,4], [4,4,7]], center = [0,0], radius = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,3]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tòa tháp <code>[1, 3, 4]</code>: Khoảng cách Manhattan = <code>|1 - 0| + |3 - 0| = 4</code>, có thể tiếp cận.</li>
	<li>Tòa tháp <code>[2, 2, 4]</code>: Khoảng cách Manhattan = <code>|2 - 0| + |2 - 0| = 4</code>, có thể tiếp cận.</li>
	<li>Tòa tháp <code>[4, 4, 7]</code>: Khoảng cách Manhattan = <code>|4 - 0| + |4 - 0| = 8</code>, không thể tiếp cận.</li>
</ul>

<p>Trong các tòa tháp có thể tiếp cận, hệ số chất lượng lớn nhất là 4. Cả <code>[1, 3]</code> và <code>[2, 2]</code> đều có cùng chất lượng, nên tọa độ nhỏ hơn theo thứ tự từ điển là <code>[1, 3]</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">towers = [[5,6,8], [0,3,5]], center = [1,2], radius = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[-1,-1]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tòa tháp <code>[5, 6, 8]</code>: Khoảng cách Manhattan = <code>|5 - 1| + |6 - 2| = 8</code>, không thể tiếp cận.</li>
	<li>Tòa tháp <code>[0, 3, 5]</code>: Khoảng cách Manhattan = <code>|0 - 1| + |3 - 2| = 2</code>, không thể tiếp cận.</li>
</ul>

<p>Không có tòa tháp nào có thể tiếp cận trong bán kính đã cho, nên trả về <code>[-1, -1]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= towers.length &lt;= 10<sup>5</sup></code></li>
	<li><code>towers[i] = [x<sub>i</sub>, y<sub>i</sub>, q<sub>i</sub>]</code></li>
	<li><code>center = [cx, cy]</code></li>
	<li><code>0 &lt;= x<sub>i</sub>, y<sub>i</sub>, q<sub>i</sub>, cx, cy &lt;= 10<sup>5</sup></code>​​​​​​​</li>
	<li><code>0 &lt;= radius &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lượt

<!-- thinking:start -->

> **Tư duy**
>
> Trong các tòa tháp có khoảng cách Manhattan không vượt quá $\textit{radius}$, ta chọn tòa tháp có chất lượng lớn nhất, nếu hòa thì chọn tọa độ nhỏ nhất theo thứ tự từ điển. Với $n \le 10^5$, không cần dùng spatial index.
>
> Khoảng cách của mỗi tòa tháp đến tâm là độc lập và chỉ cần được so sánh với đáp án tốt nhất hiện tại.
>
> Một lượt duyệt duy nhất sẽ bỏ qua các tòa tháp nằm ngoài bán kính, còn với các tòa tháp còn lại thì cập nhật chỉ số tốt nhất theo chất lượng, sau đó theo tọa độ.
>
> Nếu không chọn được tòa tháp nào, trả về $[-1,-1]$; nếu không, trả về tọa độ của tòa tháp đó.

<!-- thinking:end -->

Ta định nghĩa biến $\textit{idx}$ để ghi nhận chỉ số của tòa tháp tốt nhất hiện tại, ban đầu $\textit{idx} = -1$. Sau đó, ta duyệt qua từng tòa tháp và tính khoảng cách Manhattan $\textit{dist}$ giữa nó với $\textit{center}$:

$$
\textit{dist} = |x_i - cx| + |y_i - cy|
$$

Nếu $\textit{dist} > \textit{radius}$, tòa tháp không thể tiếp cận được, nên ta bỏ qua. Ngược lại, ta so sánh hệ số chất lượng $q$ của tòa tháp hiện tại với tòa tháp tốt nhất:

- Nếu $\textit{idx} = -1$, nghĩa là chưa tìm thấy tòa tháp nào có thể tiếp cận, nên ta cập nhật $\textit{idx}$ thành chỉ số của tòa tháp hiện tại.
- Nếu hệ số chất lượng $q_i$ của tòa tháp hiện tại lớn hơn hệ số chất lượng $q_{\textit{idx}}$ của tòa tháp tốt nhất, ta cập nhật $\textit{idx}$ thành chỉ số của tòa tháp hiện tại.
- Nếu hệ số chất lượng $q_i$ của tòa tháp hiện tại bằng hệ số chất lượng $q_{\textit{idx}}$ của tòa tháp tốt nhất, ta so sánh tọa độ của hai tòa tháp và chọn tòa tháp có thứ tự từ điển nhỏ hơn.

Sau khi duyệt xong, nếu $\textit{idx} = -1$, nghĩa là không có tòa tháp nào có thể tiếp cận, nên ta trả về $[-1, -1]$. Nếu không, ta trả về tọa độ của tòa tháp tốt nhất.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số lượng tòa tháp. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def bestTower(
        self, towers: List[List[int]], center: List[int], radius: int
    ) -> List[int]:
        cx, cy = center
        idx = -1
        for i, (x, y, q) in enumerate(towers):
            dist = abs(x - cx) + abs(y - cy)
            if dist > radius:
                continue
            if (
                idx == -1
                or towers[idx][2] < q
                or (towers[idx][2] == q and towers[i][:2] < towers[idx][:2])
            ):
                idx = i
        return [-1, -1] if idx == -1 else towers[idx][:2]
```

#### Java

```java
class Solution {
    public int[] bestTower(int[][] towers, int[] center, int radius) {
        int cx = center[0], cy = center[1];
        int idx = -1;
        for (int i = 0; i < towers.length; i++) {
            int x = towers[i][0], y = towers[i][1], q = towers[i][2];
            int dist = Math.abs(x - cx) + Math.abs(y - cy);
            if (dist > radius) {
                continue;
            }
            if (idx == -1 || towers[idx][2] < q
                || (towers[idx][2] == q
                    && (x < towers[idx][0] || (x == towers[idx][0] && y < towers[idx][1])))) {
                idx = i;
            }
        }
        return idx == -1 ? new int[] {-1, -1} : new int[] {towers[idx][0], towers[idx][1]};
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> bestTower(vector<vector<int>>& towers, vector<int>& center, int radius) {
        int cx = center[0], cy = center[1];
        int idx = -1;
        for (int i = 0; i < towers.size(); ++i) {
            int x = towers[i][0], y = towers[i][1], q = towers[i][2];
            int dist = abs(x - cx) + abs(y - cy);
            if (dist > radius) {
                continue;
            }
            if (
                idx == -1
                || towers[idx][2] < q
                || (towers[idx][2] == q && (x < towers[idx][0] || (x == towers[idx][0] && y < towers[idx][1])))) {
                idx = i;
            }
        }
        if (idx == -1) {
            return {-1, -1};
        }
        return {towers[idx][0], towers[idx][1]};
    }
};
```

#### Go

```go
func bestTower(towers [][]int, center []int, radius int) []int {
	cx, cy := center[0], center[1]
	idx := -1
	for i, a := range towers {
		x, y, q := a[0], a[1], a[2]
		dist := abs(x-cx) + abs(y-cy)
		if dist > radius {
			continue
		}
		if idx == -1 ||
			towers[idx][2] < q ||
			(towers[idx][2] == q &&
				(x < towers[idx][0] ||
					(x == towers[idx][0] && y < towers[idx][1]))) {
			idx = i
		}
	}
	if idx == -1 {
		return []int{-1, -1}
	}
	return []int{towers[idx][0], towers[idx][1]}
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function bestTower(towers: number[][], center: number[], radius: number): number[] {
    const [cx, cy] = center;
    let idx = -1;
    for (let i = 0; i < towers.length; i++) {
        const [x, y, q] = towers[i];
        const dist = Math.abs(x - cx) + Math.abs(y - cy);
        if (dist > radius) {
            continue;
        }
        if (
            idx === -1 ||
            towers[idx][2] < q ||
            (towers[idx][2] === q &&
                (x < towers[idx][0] || (x === towers[idx][0] && y < towers[idx][1])))
        ) {
            idx = i;
        }
    }
    return idx === -1 ? [-1, -1] : [towers[idx][0], towers[idx][1]];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
