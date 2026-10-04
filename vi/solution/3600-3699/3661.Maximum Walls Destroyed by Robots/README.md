---
comments: true
difficulty: Hard
rating: 2525
source: Weekly Contest 464 Q4
tags:
    - Array
    - Binary Search
    - Dynamic Programming
    - Sorting
---

<!-- problem:start -->

# [3661. Maximum Walls Destroyed by Robots](https://leetcode.com/problems/maximum-walls-destroyed-by-robots)

[中文文档](/solution/3600-3699/3661.Maximum%20Walls%20Destroyed%20by%20Robots/README.md)

## Mô tả

<!-- description:start -->

<div data-docx-has-block-data="false" data-lark-html-role="root" data-page-id="Rax8d6clvoFeVtx7bzXcvkVynwf">
<div class="old-record-id-Y5dGdSKIMoNTttxGhHLccrpEnaf">Có một đường thẳng vô hạn, trên đó có một số robot và bức tường. Cho các mảng số nguyên <code>robots</code>, <code>distance</code> và <code>walls</code>:</div>
</div>

<ul>
	<li><code>robots[i]</code> là vị trí của robot thứ <code>i<sup>th</sup></code>.</li>
	<li><code>distance[i]</code> là khoảng cách <strong>tối đa</strong> mà viên đạn của robot thứ <code>i<sup>th</sup></code> có thể bay.</li>
	<li><code>walls[j]</code> là vị trí của bức tường thứ <code>j<sup>th</sup></code>.</li>
</ul>

<p>Mỗi robot có <strong>một</strong> viên đạn, có thể bắn sang trái hoặc sang phải trong phạm vi <strong>tối đa</strong> <code>distance[i]</code> mét.</p>

<p>Viên đạn phá hủy mọi bức tường trên đường đi nằm trong phạm vi của nó. Robot là vật cản cố định: nếu viên đạn chạm một robot khác trước khi đến bức tường, nó sẽ <strong>lập tức dừng lại</strong> ở robot đó và không thể đi tiếp.</p>

<p>Trả về số lượng <strong>lớn nhất</strong> các bức tường <strong>khác nhau</strong> có thể bị robot phá hủy.</p>

<p>Lưu ý:</p>

<ul>
	<li>Một bức tường và một robot có thể ở cùng vị trí; bức tường vẫn có thể bị robot ở vị trí đó phá hủy.</li>
	<li>Robot không bị phá hủy bởi đạn.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">robots = [4], distance = [3], walls = [1,10]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><code>robots[0] = 4</code> bắn sang <strong>trái</strong> với <code>distance[0] = 3</code>, bao phủ đoạn <code>[1, 4]</code> và phá hủy <code>walls[0] = 1</code>.</li>
	<li>Do đó, đáp án là 1.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">robots = [10,2], distance = [5,1], walls = [5,2,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><code>robots[0] = 10</code> bắn sang <strong>trái</strong> với <code>distance[0] = 5</code>, bao phủ đoạn <code>[5, 10]</code> và phá hủy <code>walls[0] = 5</code> cùng <code>walls[2] = 7</code>.</li>
	<li><code>robots[1] = 2</code> bắn sang <strong>trái</strong> với <code>distance[1] = 1</code>, bao phủ đoạn <code>[1, 2]</code> và phá hủy <code>walls[1] = 2</code>.</li>
	<li>Do đó, đáp án là 3.</li>
</ul>
</div>
<strong class="example">Ví dụ 3:</strong>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">robots = [1,2], distance = [100,1], walls = [10]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Trong ví dụ này, chỉ <code>robots[0]</code> có thể chạm đến bức tường, nhưng phát bắn sang <strong>phải</strong> của nó bị <code>robots[1]</code> chặn lại; do đó đáp án là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= robots.length == distance.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= walls.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= robots[i], walls[j] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= distance[i] &lt;= 10<sup>5</sup></code></li>
	<li>Mọi giá trị trong <code>robots</code> đều <strong>khác nhau</strong></li>
	<li>Mọi giá trị trong <code>walls</code> đều <strong>khác nhau</strong></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi robot bắn sang trái hoặc phải, với phạm vi bị giới hạn bởi $\textit{distance}$ và các robot lân cận. Có $2^n$ cách gán hướng, không thể duyệt hết.
>
> Sau khi sắp xếp theo vị trí, lựa chọn của robot $i$ chỉ phụ thuộc vào hướng của robot $i+1$. $\textit{dfs}(i,j)$ là số tường phá hủy lớn nhất sau khi quyết định cho $i$, với hướng tiếp theo là $j$.
>
> Phát bắn sang trái bị robot trước chặn lại; phát bắn sang phải bị robot sau chặn lại, và nếu robot đó cũng bắn sang trái thì còn bị giới hạn bởi phạm vi của nó. Tìm kiếm nhị phân đếm số tường trong đoạn còn lại; ghi nhớ giúp loại bỏ các trạng thái trùng lặp.

<!-- thinking:end -->

Trước tiên, ta lưu mỗi robot cùng với phạm vi bắn của nó vào một mảng rồi sắp xếp theo vị trí robot. Ta cũng sắp xếp các vị trí của tường. Tiếp theo, ta dùng tìm kiếm theo chiều sâu (DFS) để tính số tường mỗi robot có thể phá hủy, đồng thời dùng tìm kiếm có ghi nhớ để tránh tính toán trùng lặp.

Ta thiết kế hàm $\text{dfs}(i, j)$, trong đó $i$ biểu thị chỉ số của robot hiện tại đang được xét, còn $j$ biểu thị hướng bắn của robot tiếp theo (0 là trái, 1 là phải), và hàm trả về số tường có thể phá hủy. Đáp án là $\text{dfs}(n - 1, 1)$, trong đó ở trạng thái biên $j$ có thể là 0 hoặc 1.

Logic thực thi của hàm $\text{dfs}(i, j)$ như sau:

Nếu $i \lt 0$, nghĩa là đã xét xong tất cả robot, ta trả về 0.

Ngược lại, với robot hiện tại, ta có hai hướng bắn để lựa chọn.

Nếu chọn bắn **sang trái**, ta cần tính đoạn bên trái $[\text{left}, \text{robot}[i][0]]$, rồi dùng tìm kiếm nhị phân để tính số tường có thể phá hủy trong đoạn này. Khi đó, tổng số tường có thể phá hủy là $\text{dfs}(i - 1, 0) + \text{count}$, trong đó $\text{count}$ là số tường bị phá hủy khi robot hiện tại bắn sang trái.

Nếu chọn bắn **sang phải**, ta cần tính đoạn bên phải $[\text{robot}[i][0], \text{right}]$, rồi dùng tìm kiếm nhị phân để tính số tường có thể phá hủy trong đoạn này. Khi đó, tổng số tường có thể phá hủy là $\text{dfs}(i - 1, 1) + \text{count}$, trong đó $\text{count}$ là số tường bị phá hủy khi robot hiện tại bắn sang phải.

Giá trị trả về của hàm là số tường lớn nhất có thể phá hủy bằng hai hướng bắn.

Độ phức tạp thời gian là $O(n \times \log n + m \times \log m)$, độ phức tạp không gian là $O(n)$. Trong đó $n$ và $m$ lần lượt là số robot và số tường.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxWalls(self, robots: List[int], distance: List[int], walls: List[int]) -> int:
        n = len(robots)
        arr = sorted(zip(robots, distance), key=lambda x: x[0])
        walls.sort()

        @cache
        def dfs(i: int, j: int) -> int:
            if i < 0:
                return 0
            left = arr[i][0] - arr[i][1]
            if i > 0:
                left = max(left, arr[i - 1][0] + 1)
            l = bisect_left(walls, left)
            r = bisect_left(walls, arr[i][0] + 1)
            ans = dfs(i - 1, 0) + r - l
            right = arr[i][0] + arr[i][1]
            if i + 1 < n:
                if j == 0:
                    right = min(right, arr[i + 1][0] - arr[i + 1][1] - 1)
                else:
                    right = min(right, arr[i + 1][0] - 1)
            l = bisect_left(walls, arr[i][0])
            r = bisect_left(walls, right + 1)
            ans = max(ans, dfs(i - 1, 1) + r - l)
            return ans

        ans = dfs(n - 1, 1)
        dfs.cache_clear()
        return ans
```

#### Java

```java
class Solution {
    private Integer[][] f;
    private int[][] arr;
    private int[] walls;
    private int n;

    public int maxWalls(int[] robots, int[] distance, int[] walls) {
        n = robots.length;
        arr = new int[n][2];
        for (int i = 0; i < n; i++) {
            arr[i][0] = robots[i];
            arr[i][1] = distance[i];
        }
        Arrays.sort(arr, Comparator.comparingInt(a -> a[0]));
        Arrays.sort(walls);
        this.walls = walls;
        f = new Integer[n][2];
        return dfs(n - 1, 1);
    }

    private int dfs(int i, int j) {
        if (i < 0) {
            return 0;
        }
        if (f[i][j] != null) {
            return f[i][j];
        }

        int left = arr[i][0] - arr[i][1];
        if (i > 0) {
            left = Math.max(left, arr[i - 1][0] + 1);
        }
        int l = lowerBound(walls, left);
        int r = lowerBound(walls, arr[i][0] + 1);
        int ans = dfs(i - 1, 0) + (r - l);

        int right = arr[i][0] + arr[i][1];
        if (i + 1 < n) {
            if (j == 0) {
                right = Math.min(right, arr[i + 1][0] - arr[i + 1][1] - 1);
            } else {
                right = Math.min(right, arr[i + 1][0] - 1);
            }
        }
        l = lowerBound(walls, arr[i][0]);
        r = lowerBound(walls, right + 1);
        ans = Math.max(ans, dfs(i - 1, 1) + (r - l));
        return f[i][j] = ans;
    }

    private int lowerBound(int[] arr, int target) {
        int idx = Arrays.binarySearch(arr, target);
        if (idx < 0) {
            return -idx - 1;
        }
        return idx;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxWalls(vector<int>& robots, vector<int>& distance, vector<int>& walls) {
        int n = robots.size();
        vector<pair<int, int>> arr(n);
        for (int i = 0; i < n; i++) {
            arr[i] = {robots[i], distance[i]};
        }
        ranges::sort(arr, {}, &pair<int, int>::first);
        ranges::sort(walls);

        vector f(n, vector<int>(2, -1));

        auto dfs = [&](this auto&& dfs, int i, int j) -> int {
            if (i < 0) {
                return 0;
            }
            if (f[i][j] != -1) {
                return f[i][j];
            }

            int left = arr[i].first - arr[i].second;
            if (i > 0) {
                left = max(left, arr[i - 1].first + 1);
            }
            int l = ranges::lower_bound(walls, left) - walls.begin();
            int r = ranges::lower_bound(walls, arr[i].first + 1) - walls.begin();
            int ans = dfs(i - 1, 0) + (r - l);

            int right = arr[i].first + arr[i].second;
            if (i + 1 < n) {
                if (j == 0) {
                    right = min(right, arr[i + 1].first - arr[i + 1].second - 1);
                } else {
                    right = min(right, arr[i + 1].first - 1);
                }
            }
            l = ranges::lower_bound(walls, arr[i].first) - walls.begin();
            r = ranges::lower_bound(walls, right + 1) - walls.begin();
            ans = max(ans, dfs(i - 1, 1) + (r - l));

            return f[i][j] = ans;
        };

        return dfs(n - 1, 1);
    }
};
```

#### Go

```go
func maxWalls(robots []int, distance []int, walls []int) int {
	type pair struct {
		x, d int
	}
	n := len(robots)
	arr := make([]pair, n)
	for i := 0; i < n; i++ {
		arr[i] = pair{robots[i], distance[i]}
	}
	sort.Slice(arr, func(i, j int) bool {
		return arr[i].x < arr[j].x
	})
	sort.Ints(walls)

	f := make(map[[2]int]int)

	var dfs func(int, int) int
	dfs = func(i, j int) int {
		if i < 0 {
			return 0
		}
		key := [2]int{i, j}
		if v, ok := f[key]; ok {
			return v
		}

		left := arr[i].x - arr[i].d
		if i > 0 {
			left = max(left, arr[i-1].x+1)
		}
		l := sort.SearchInts(walls, left)
		r := sort.SearchInts(walls, arr[i].x+1)
		ans := dfs(i-1, 0) + (r - l)

		right := arr[i].x + arr[i].d
		if i+1 < n {
			if j == 0 {
				right = min(right, arr[i+1].x-arr[i+1].d-1)
			} else {
				right = min(right, arr[i+1].x-1)
			}
		}
		l = sort.SearchInts(walls, arr[i].x)
		r = sort.SearchInts(walls, right+1)
		ans = max(ans, dfs(i-1, 1)+(r-l))

		f[key] = ans
		return ans
	}

	return dfs(n-1, 1)
}
```

#### TypeScript

```ts
function maxWalls(robots: number[], distance: number[], walls: number[]): number {
    type Pair = [number, number];
    const n = robots.length;
    const arr: Pair[] = robots.map((r, i) => [r, distance[i]]);

    _.sortBy(arr, p => p[0]).forEach((p, i) => (arr[i] = p));
    walls.sort((a, b) => a - b);
    const f: number[][] = Array.from({ length: n }, () => Array(2).fill(-1));

    function dfs(i: number, j: number): number {
        if (i < 0) {
            return 0;
        }
        if (f[i][j] !== -1) {
            return f[i][j];
        }

        let left = arr[i][0] - arr[i][1];
        if (i > 0) left = Math.max(left, arr[i - 1][0] + 1);
        let l = _.sortedIndex(walls, left);
        let r = _.sortedIndex(walls, arr[i][0] + 1);
        let ans = dfs(i - 1, 0) + (r - l);

        let right = arr[i][0] + arr[i][1];
        if (i + 1 < n) {
            if (j === 0) {
                right = Math.min(right, arr[i + 1][0] - arr[i + 1][1] - 1);
            } else {
                right = Math.min(right, arr[i + 1][0] - 1);
            }
        }
        l = _.sortedIndex(walls, arr[i][0]);
        r = _.sortedIndex(walls, right + 1);
        ans = Math.max(ans, dfs(i - 1, 1) + (r - l));

        f[i][j] = ans;
        return ans;
    }

    return dfs(n - 1, 1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
