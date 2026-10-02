---
comments: true
difficulty: Medium
tags:
    - Array
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1057. Campus Bikes 🔒](https://leetcode.com/problems/campus-bikes)

[中文文档](/solution/1000-1099/1057.Campus%20Bikes/README.md)

## Mô tả

<!-- description:start -->

<p>Trong khuôn viên trường được biểu diễn trên mặt phẳng X-Y, có <code>n</code> nhân viên và <code>m</code> chiếc xe đạp, với <code>n &lt;= m</code>.</p>

<p>Bạn được cho mảng <code>workers</code> có độ dài <code>n</code>, trong đó <code>workers[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> là vị trí của nhân viên thứ <code>i<sup>th</sup></code>. Bạn cũng được cho mảng <code>bikes</code> có độ dài <code>m</code>, trong đó <code>bikes[j] = [x<sub>j</sub>, y<sub>j</sub>]</code> là vị trí của chiếc xe đạp thứ <code>j<sup>th</sup></code>. Tất cả vị trí đã cho đều <strong>khác nhau</strong>.</p>

<p>Hãy phân xe đạp cho từng nhân viên. Trong số các nhân viên và xe đạp còn chưa được ghép, ta chọn cặp <code>(worker<sub>i</sub>, bike<sub>j</sub>)</code> có <strong>khoảng cách Manhattan</strong> nhỏ nhất giữa hai bên rồi phân chiếc xe đó cho nhân viên tương ứng.</p>

<p>Nếu có nhiều cặp <code>(worker<sub>i</sub>, bike<sub>j</sub>)</code> cùng có <strong>khoảng cách Manhattan</strong> nhỏ nhất, ta chọn cặp có <strong>chỉ số nhân viên nhỏ nhất</strong>. Nếu vẫn có nhiều lựa chọn, ta chọn cặp có <strong>chỉ số xe đạp nhỏ nhất</strong>. Lặp lại quy trình này cho đến khi không còn nhân viên nào chưa được ghép.</p>

<p>Trả về <em>mảng </em><code>answer</code><em> có độ dài </em><code>n</code><em>, trong đó </em><code>answer[i]</code><em> là chỉ số (đánh số từ <strong>0</strong>) của chiếc xe được phân cho nhân viên thứ </em><code>i<sup>th</sup></code>.</p>

<p><strong>Khoảng cách Manhattan</strong> giữa hai điểm <code>p1</code> và <code>p2</code> là <code>Manhattan(p1, p2) = |p1.x - p2.x| + |p1.y - p2.y|</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1000-1099/1057.Campus%20Bikes/images/1261_example_1_v2.png" style="width: 376px; height: 366px;" />
<pre>
<strong>Input:</strong> workers = [[0,0],[2,1]], bikes = [[1,2],[3,3]]
<strong>Output:</strong> [1,0]
<strong>Giải thích:</strong> Nhân viên 1 nhận xe đạp 0 vì đây là cặp gần nhất (không có trường hợp hòa), còn nhân viên 0 được phân xe đạp 1. Vì vậy, kết quả là [1, 0].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1000-1099/1057.Campus%20Bikes/images/1261_example_2_v2.png" style="width: 376px; height: 366px;" />
<pre>
<strong>Input:</strong> workers = [[0,0],[1,1],[2,0]], bikes = [[1,0],[2,2],[2,1]]
<strong>Output:</strong> [0,2,1]
<strong>Giải thích:</strong> Đầu tiên, nhân viên 0 nhận xe đạp 0. Nhân viên 1 và nhân viên 2 có cùng khoảng cách đến xe đạp 2, nên nhân viên 1 được phân xe đạp 2, còn nhân viên 2 nhận xe đạp 1. Vì vậy, kết quả là [0,2,1].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == workers.length</code></li>
	<li><code>m == bikes.length</code></li>
	<li><code>1 &lt;= n &lt;= m &lt;= 1000</code></li>
	<li><code>workers[i].length == bikes[j].length == 2</code></li>
	<li><code>0 &lt;= x<sub>i</sub>, y<sub>i</sub> &lt; 1000</code></li>
	<li><code>0 &lt;= x<sub>j</sub>, y<sub>j</sub> &lt; 1000</code></li>
	<li>Vị trí của tất cả nhân viên và xe đạp đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi nhân viên nhận chiếc xe chưa được dùng gần nhất; nếu hòa thì ưu tiên chỉ số nhân viên nhỏ hơn, rồi đến chỉ số xe đạp nhỏ hơn. Vì $n,m\le 1000$, có thể sắp xếp toàn bộ $nm$ cặp rồi ghép theo thứ tự đó.
>
> Sắp xếp các bộ ba $(\textit{dist},i,j)$; mỗi khi gặp một cặp mà cả nhân viên lẫn xe đạp đều chưa được ghép, ta ghép chúng với nhau.
>
> Hai mảng đánh dấu đảm bảo mỗi nhân viên và mỗi xe đạp chỉ được dùng một lần.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def assignBikes(
        self, workers: List[List[int]], bikes: List[List[int]]
    ) -> List[int]:
        n, m = len(workers), len(bikes)
        arr = []
        for i, j in product(range(n), range(m)):
            dist = abs(workers[i][0] - bikes[j][0]) + abs(workers[i][1] - bikes[j][1])
            arr.append((dist, i, j))
        arr.sort()
        vis1 = [False] * n
        vis2 = [False] * m
        ans = [0] * n
        for _, i, j in arr:
            if not vis1[i] and not vis2[j]:
                vis1[i] = vis2[j] = True
                ans[i] = j
        return ans
```

#### Java

```java
class Solution {
    public int[] assignBikes(int[][] workers, int[][] bikes) {
        int n = workers.length, m = bikes.length;
        int[][] arr = new int[m * n][3];
        for (int i = 0, k = 0; i < n; ++i) {
            for (int j = 0; j < m; ++j) {
                int dist
                    = Math.abs(workers[i][0] - bikes[j][0]) + Math.abs(workers[i][1] - bikes[j][1]);
                arr[k++] = new int[] {dist, i, j};
            }
        }
        Arrays.sort(arr, (a, b) -> {
            if (a[0] != b[0]) {
                return a[0] - b[0];
            }
            if (a[1] != b[1]) {
                return a[1] - b[1];
            }
            return a[2] - b[2];
        });
        boolean[] vis1 = new boolean[n];
        boolean[] vis2 = new boolean[m];
        int[] ans = new int[n];
        for (var e : arr) {
            int i = e[1], j = e[2];
            if (!vis1[i] && !vis2[j]) {
                vis1[i] = true;
                vis2[j] = true;
                ans[i] = j;
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
    vector<int> assignBikes(vector<vector<int>>& workers, vector<vector<int>>& bikes) {
        int n = workers.size(), m = bikes.size();
        vector<tuple<int, int, int>> arr(n * m);
        for (int i = 0, k = 0; i < n; ++i) {
            for (int j = 0; j < m; ++j) {
                int dist = abs(workers[i][0] - bikes[j][0]) + abs(workers[i][1] - bikes[j][1]);
                arr[k++] = {dist, i, j};
            }
        }
        sort(arr.begin(), arr.end());
        vector<bool> vis1(n), vis2(m);
        vector<int> ans(n);
        for (auto& [_, i, j] : arr) {
            if (!vis1[i] && !vis2[j]) {
                vis1[i] = true;
                vis2[j] = true;
                ans[i] = j;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func assignBikes(workers [][]int, bikes [][]int) []int {
	n, m := len(workers), len(bikes)
	type tuple struct{ d, i, j int }
	arr := make([]tuple, n*m)
	for i, k := 0, 0; i < n; i++ {
		for j := 0; j < m; j++ {
			d := abs(workers[i][0]-bikes[j][0]) + abs(workers[i][1]-bikes[j][1])
			arr[k] = tuple{d, i, j}
			k++
		}
	}
	sort.Slice(arr, func(i, j int) bool {
		if arr[i].d != arr[j].d {
			return arr[i].d < arr[j].d
		}
		if arr[i].i != arr[j].i {
			return arr[i].i < arr[j].i
		}
		return arr[i].j < arr[j].j
	})
	vis1, vis2 := make([]bool, n), make([]bool, m)
	ans := make([]int, n)
	for _, e := range arr {
		i, j := e.i, e.j
		if !vis1[i] && !vis2[j] {
			vis1[i], vis2[j] = true, true
			ans[i] = j
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

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
