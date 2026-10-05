---
comments: true
difficulty: Easy
rating: 1184
source: Weekly Contest 515 Q1
tags:
    - Array
    - Enumeration
---

<!-- problem:start -->

# [4024. Nearest Available Drone](https://leetcode.com/problems/nearest-available-drone)

[中文文档](/solution/4000-4099/4024.Nearest%20Available%20Drone/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên 2 chiều <code>drones</code>, trong đó <code>drones[i] = [x<sub>i</sub>, y<sub>i</sub>, range<sub>i</sub>]</code> biểu thị tọa độ x, tọa độ y và phạm vi di chuyển của drone thứ <code>i<sup>th</sup></code>.</p>

<p>Bạn cũng được cho một mảng số nguyên <code>target = [tx, ty]</code>, biểu thị tọa độ của mục tiêu.</p>

<p>Một drone <code>drones[i]</code> có thể đến mục tiêu nếu khoảng cách <span data-keyword="manhattan-distance">Manhattan</span> giữa tọa độ của nó và tọa độ mục tiêu <strong>nhỏ hơn hoặc bằng</strong> <code>range<sub>i</sub></code> của nó.</p>

<p>Trả về <strong>chỉ số</strong> của drone có thể đến mục tiêu và có <strong>khoảng cách Manhattan nhỏ nhất</strong> đến mục tiêu. Nếu có nhiều drone cùng khoảng cách, trả về <strong>chỉ số nhỏ nhất</strong>. Nếu không có drone nào có thể đến mục tiêu, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">drones = [[0,0,8],[2,2,9]], target = [3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Khoảng cách giữa <code>drones[0]</code> và <code>target</code> là <code>|0 - 3| + |0 - 4| = 7</code>, nằm trong phạm vi 8 của nó.</li>
	<li>Khoảng cách giữa <code>drones[1]</code> và <code>target</code> là <code>|2 - 3| + |2 - 4| = 3</code>, nằm trong phạm vi 9 của nó.</li>
	<li>Vì <code>drones[1]</code> là drone gần nhất nên đáp án là 1.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">drones = [[2,1,5],[4,4,5],[6,6,8]], target = [5,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Khoảng cách giữa <code>drones[0]</code> và <code>target</code> là <code>|2 - 5| + |1 - 5| = 7</code>, lớn hơn phạm vi 5 của nó.</li>
	<li>Khoảng cách giữa <code>drones[1]</code> và <code>target</code> là <code>|4 - 5| + |4 - 5| = 2</code>, nằm trong phạm vi 5 của nó.</li>
	<li>Khoảng cách giữa <code>drones[2]</code> và <code>target</code> là <code>|6 - 5| + |6 - 5| = 2</code>, nằm trong phạm vi 8 của nó.</li>
	<li>Cả <code>drones[1]</code> và <code>drones[2]</code> đều là các drone gần nhất. Vì cần trả về chỉ số nhỏ nhất nên đáp án là 1.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">drones = [[4,4,5]], target = [8,6]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Khoảng cách giữa <code>drones[0]</code> và <code>target</code> là <code>|4 - 8| + |4 - 6| = 6</code>, lớn hơn phạm vi 5 của nó.</li>
	<li>Không có drone nào có thể đến mục tiêu nên đáp án là -1.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= drones.length &lt;= 100</code></li>
	<li><code>drones[i] = [x<sub>i</sub>, y<sub>i</sub>, range<sub>i</sub>]</code></li>
	<li><code>target = [tx, ty]</code></li>
	<li><code>-25 &lt;= x<sub>i</sub>, y<sub>i</sub>, tx, ty &lt;= 25</code></li>
	<li><code>1 &lt;= range<sub>i</sub> &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Một drone có thể đến mục tiêu khi và chỉ khi khoảng cách Manhattan của nó không vượt quá $\textit{range}$ của chính nó. Không có ràng buộc hình học nào khác.
>
> Duyệt tuyến tính cho phép duy trì khoảng cách nhỏ nhất hiện tại và chỉ số tương ứng, chỉ cập nhật khi gặp khoảng cách nhỏ hơn để khi hòa thì giữ lại chỉ số nhỏ hơn.
>
> Nếu không có drone nào có thể đến mục tiêu, đáp án vẫn là $-1$.

<!-- thinking:end -->

Chúng ta duyệt qua từng drone và tính khoảng cách Manhattan $d = |x_i - t_x| + |y_i - t_y|$ đến mục tiêu. Nếu $d \le \textit{range}_i$, drone đó có thể đến mục tiêu. Trong số tất cả các drone có thể đến mục tiêu, chúng ta chọn drone có khoảng cách nhỏ nhất. Nếu có nhiều drone cùng khoảng cách, chúng ta giữ lại chỉ số nhỏ hơn vì duyệt từ trái sang phải và chỉ cập nhật khi khoảng cách nhỏ hơn nghiêm ngặt. Nếu không có drone nào có thể đến mục tiêu, trả về $-1$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là số lượng drone.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def nearestDrone(self, drones: list[list[int]], target: list[int]) -> int:
        ans = -1
        mn = inf
        tx, ty = target
        for i, (x, y, r) in enumerate(drones):
            d = abs(x - tx) + abs(y - ty)
            if d <= r and mn > d:
                ans = i
                mn = d
        return ans
```

#### Java

```java
class Solution {
    public int nearestDrone(int[][] drones, int[] target) {
        int ans = -1;
        int mn = Integer.MAX_VALUE;
        int tx = target[0], ty = target[1];

        for (int i = 0; i < drones.length; i++) {
            int x = drones[i][0];
            int y = drones[i][1];
            int r = drones[i][2];

            int d = Math.abs(x - tx) + Math.abs(y - ty);

            if (d <= r && mn > d) {
                ans = i;
                mn = d;
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
    int nearestDrone(vector<vector<int>>& drones, vector<int>& target) {
        int ans = -1;
        int mn = INT_MAX;
        int tx = target[0], ty = target[1];

        for (int i = 0; i < drones.size(); i++) {
            int x = drones[i][0];
            int y = drones[i][1];
            int r = drones[i][2];

            int d = abs(x - tx) + abs(y - ty);

            if (d <= r && mn > d) {
                ans = i;
                mn = d;
            }
        }

        return ans;
    }
};
```

#### Go

```go
func nearestDrone(drones [][]int, target []int) int {
	ans := -1
	mn := math.MaxInt32
	tx, ty := target[0], target[1]

	for i, drone := range drones {
		x, y, r := drone[0], drone[1], drone[2]

		d := abs(x-tx) + abs(y-ty)

		if d <= r && mn > d {
			ans = i
			mn = d
		}
	}

	return ans
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
function nearestDrone(drones: number[][], target: number[]): number {
    let ans = -1;
    let mn = Infinity;
    const [tx, ty] = target;

    for (let i = 0; i < drones.length; i++) {
        const [x, y, r] = drones[i];

        const d = Math.abs(x - tx) + Math.abs(y - ty);

        if (d <= r && mn > d) {
            ans = i;
            mn = d;
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
