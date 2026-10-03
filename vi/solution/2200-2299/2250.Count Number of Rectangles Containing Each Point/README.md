---
comments: true
difficulty: Medium
rating: 1997
source: Weekly Contest 290 Q3
tags:
    - Binary Indexed Tree
    - Array
    - Hash Table
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [2250. Count Number of Rectangles Containing Each Point](https://leetcode.com/problems/count-number-of-rectangles-containing-each-point)

[中文文档](/solution/2200-2299/2250.Count%20Number%20of%20Rectangles%20Containing%20Each%20Point/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên 2 chiều <code>rectangles</code>, trong đó <code>rectangles[i] = [l<sub>i</sub>, h<sub>i</sub>]</code> cho biết hình chữ nhật thứ <code>i<sup>th</sup></code> có chiều dài là <code>l<sub>i</sub></code> và chiều cao là <code>h<sub>i</sub></code>. Bạn cũng được cho một mảng số nguyên 2 chiều <code>points</code>, trong đó <code>points[j] = [x<sub>j</sub>, y<sub>j</sub>]</code> là một điểm có tọa độ <code>(x<sub>j</sub>, y<sub>j</sub>)</code>.</p>

<p>Hình chữ nhật thứ <code>i<sup>th</sup></code> có góc <strong>dưới bên trái</strong> tại tọa độ <code>(0, 0)</code> và góc <strong>trên bên phải</strong> tại tọa độ <code>(l<sub>i</sub>, h<sub>i</sub>)</code>.</p>

<p>Trả về<em> một mảng số nguyên </em><code>count</code><em> có độ dài </em><code>points.length</code><em>, trong đó </em><code>count[j]</code><em> là số hình chữ nhật <strong>chứa</strong> điểm thứ </em><code>j<sup>th</sup></code><em>.</em></p>

<p>Hình chữ nhật thứ <code>i<sup>th</sup></code> <strong>chứa</strong> điểm thứ <code>j<sup>th</sup></code> nếu <code>0 &lt;= x<sub>j</sub> &lt;= l<sub>i</sub></code> và <code>0 &lt;= y<sub>j</sub> &lt;= h<sub>i</sub></code>. Lưu ý rằng các điểm nằm trên <strong>cạnh</strong> của hình chữ nhật cũng được xem là nằm trong hình chữ nhật đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2250.Count%20Number%20of%20Rectangles%20Containing%20Each%20Point/images/example1.png" style="width: 300px; height: 509px;" />
<pre>
<strong>Đầu vào:</strong> rectangles = [[1,2],[2,3],[2,5]], points = [[2,1],[1,4]]
<strong>Đầu ra:</strong> [2,1]
<strong>Giải thích:</strong>
Hình chữ nhật thứ nhất không chứa điểm nào.
Hình chữ nhật thứ hai chỉ chứa điểm (2, 1).
Hình chữ nhật thứ ba chứa các điểm (2, 1) và (1, 4).
Số hình chữ nhật chứa điểm (2, 1) là 2.
Số hình chữ nhật chứa điểm (1, 4) là 1.
Do đó, ta trả về [2, 1].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2250.Count%20Number%20of%20Rectangles%20Containing%20Each%20Point/images/example2.png" style="width: 300px; height: 312px;" />
<pre>
<strong>Đầu vào:</strong> rectangles = [[1,1],[2,2],[3,3]], points = [[1,3],[1,1]]
<strong>Đầu ra:</strong> [1,3]
<strong>Giải thích:
</strong>Hình chữ nhật thứ nhất chỉ chứa điểm (1, 1).
Hình chữ nhật thứ hai chỉ chứa điểm (1, 1).
Hình chữ nhật thứ ba chứa các điểm (1, 3) và (1, 1).
Số hình chữ nhật chứa điểm (1, 3) là 1.
Số hình chữ nhật chứa điểm (1, 1) là 3.
Do đó, ta trả về [1, 3].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= rectangles.length, points.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>rectangles[i].length == points[j].length == 2</code></li>
	<li><code>1 &lt;= l<sub>i</sub>, x<sub>j</sub> &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= h<sub>i</sub>, y<sub>j</sub> &lt;= 100</code></li>
	<li>Tất cả <code>rectangles</code> đều <strong>khác nhau</strong>.</li>
	<li>Tất cả <code>points</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các hình chữ nhật song song với các trục tọa độ đều bắt đầu từ gốc tọa độ; ta cần đếm xem mỗi điểm truy vấn nằm trong bao nhiêu hình chữ nhật. Cả hai danh sách có thể chứa tới $5\times 10^4$ phần tử, chiều cao tối đa là $100$, còn chiều rộng có thể lên tới $10^9$. Duyệt qua mọi hình chữ nhật cho từng điểm sẽ quá chậm. Miền giá trị chiều cao nhỏ cho phép ta phân nhóm theo chiều cao.
>
> Ta lưu chiều rộng của các hình chữ nhật có cùng chiều cao vào một danh sách đã sắp xếp. Một điểm $(x,y)$ được bao phủ bởi các hình chữ nhật có chiều cao $h \ge y$ và chiều rộng $\ge x$, vì vậy ta tìm cận dưới trong từng danh sách với các giá trị từ $h=y$ đến $100$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countRectangles(
        self, rectangles: List[List[int]], points: List[List[int]]
    ) -> List[int]:
        d = defaultdict(list)
        for x, y in rectangles:
            d[y].append(x)
        for y in d.keys():
            d[y].sort()
        ans = []
        for x, y in points:
            cnt = 0
            for h in range(y, 101):
                xs = d[h]
                cnt += len(xs) - bisect_left(xs, x)
            ans.append(cnt)
        return ans
```

#### Java

```java
class Solution {
    public int[] countRectangles(int[][] rectangles, int[][] points) {
        int n = 101;
        List<Integer>[] d = new List[n];
        Arrays.setAll(d, k -> new ArrayList<>());
        for (int[] r : rectangles) {
            d[r[1]].add(r[0]);
        }
        for (List<Integer> v : d) {
            Collections.sort(v);
        }
        int m = points.length;
        int[] ans = new int[m];
        for (int i = 0; i < m; ++i) {
            int x = points[i][0], y = points[i][1];
            int cnt = 0;
            for (int h = y; h < n; ++h) {
                List<Integer> xs = d[h];
                int left = 0, right = xs.size();
                while (left < right) {
                    int mid = (left + right) >> 1;
                    if (xs.get(mid) >= x) {
                        right = mid;
                    } else {
                        left = mid + 1;
                    }
                }
                cnt += xs.size() - left;
            }
            ans[i] = cnt;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> countRectangles(vector<vector<int>>& rectangles, vector<vector<int>>& points) {
        int n = 101;
        vector<vector<int>> d(n);
        for (auto& r : rectangles) d[r[1]].push_back(r[0]);
        for (auto& v : d) sort(v.begin(), v.end());
        vector<int> ans;
        for (auto& p : points) {
            int x = p[0], y = p[1];
            int cnt = 0;
            for (int h = y; h < n; ++h) {
                auto& xs = d[h];
                cnt += xs.size() - (lower_bound(xs.begin(), xs.end(), x) - xs.begin());
            }
            ans.push_back(cnt);
        }
        return ans;
    }
};
```

#### Go

```go
func countRectangles(rectangles [][]int, points [][]int) []int {
	n := 101
	d := make([][]int, 101)
	for _, r := range rectangles {
		d[r[1]] = append(d[r[1]], r[0])
	}
	for _, v := range d {
		sort.Ints(v)
	}
	var ans []int
	for _, p := range points {
		x, y := p[0], p[1]
		cnt := 0
		for h := y; h < n; h++ {
			xs := d[h]
			left, right := 0, len(xs)
			for left < right {
				mid := (left + right) >> 1
				if xs[mid] >= x {
					right = mid
				} else {
					left = mid + 1
				}
			}
			cnt += len(xs) - left
		}
		ans = append(ans, cnt)
	}
	return ans
}
```

#### TypeScript

```ts
function countRectangles(rectangles: number[][], points: number[][]): number[] {
    const n = 101;
    let ymap = Array.from({ length: n }, v => []);
    for (let [x, y] of rectangles) {
        ymap[y].push(x);
    }
    for (let nums of ymap) {
        nums.sort((a, b) => a - b);
    }
    let ans = [];
    for (let [x, y] of points) {
        let count = 0;
        for (let h = y; h < n; h++) {
            const nums = ymap[h];
            let left = 0,
                right = nums.length;
            while (left < right) {
                let mid = (left + right) >> 1;
                if (x > nums[mid]) {
                    left = mid + 1;
                } else {
                    right = mid;
                }
            }
            count += nums.length - right;
        }
        ans.push(count);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
