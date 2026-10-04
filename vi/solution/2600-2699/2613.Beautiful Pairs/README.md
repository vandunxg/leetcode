---
comments: true
difficulty: Hard
tags:
    - Geometry
    - Array
    - Math
    - Divide and Conquer
    - Ordered Set
    - Sorting
---

<!-- problem:start -->

# [2613. Beautiful Pairs 🔒](https://leetcode.com/problems/beautiful-pairs)

[中文文档](/solution/2600-2699/2613.Beautiful%20Pairs/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên được đánh chỉ số từ <strong>0</strong> là <code>nums1</code> và <code>nums2</code>, có cùng độ dài. Một cặp chỉ số <code>(i,j)</code> được gọi là <strong>đẹp</strong> nếu <code>|nums1[i] - nums1[j]| + |nums2[i] - nums2[j]|</code> là giá trị nhỏ nhất trong tất cả các cặp chỉ số có thể có với <code>i &lt; j</code>.</p>

<p>Trả về <em>cặp đẹp. Nếu có nhiều cặp đẹp, hãy trả về cặp nhỏ nhất theo thứ tự từ điển.</em></p>

<p>Lưu ý rằng</p>

<ul>
	<li><code>|x|</code> biểu thị giá trị tuyệt đối của <code>x</code>.</li>
	<li>Một cặp chỉ số <code>(i<sub>1</sub>, j<sub>1</sub>)</code> nhỏ hơn theo thứ tự từ điển so với <code>(i<sub>2</sub>, j<sub>2</sub>)</code> nếu <code>i<sub>1</sub> &lt; i<sub>2</sub></code> hoặc <code>i<sub>1</sub> == i<sub>2</sub></code> và <code>j<sub>1</sub> &lt; j<sub>2</sub></code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [1,2,3,2,4], nums2 = [2,3,1,2,3]
<strong>Đầu ra:</strong> [0,3]
<strong>Giải thích:</strong> Xét chỉ số 0 và chỉ số 3. Giá trị của |nums1[i]-nums1[j]| + |nums2[i]-nums2[j]| là 1, đây là giá trị nhỏ nhất có thể đạt được.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [1,2,4,3,2,5], nums2 = [1,4,2,3,5,1]
<strong>Đầu ra:</strong> [1,4]
<strong>Giải thích:</strong> Xét chỉ số 1 và chỉ số 4. Giá trị của |nums1[i]-nums1[j]| + |nums2[i]-nums2[j]| là 1, đây là giá trị nhỏ nhất có thể đạt được.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums1.length, nums2.length &lt;= 10<sup>5</sup></code></li>
	<li><code>nums1.length == nums2.length</code></li>
	<li><code>0 &lt;= nums1<sub>i</sub><sub>&nbsp;</sub>&lt;= nums1.length</code></li>
	<li><code>0 &lt;= nums2<sub>i</sub>&nbsp;&lt;= nums2.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Chia để trị

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm cặp điểm gần nhau nhất theo khoảng cách Manhattan, với các trường hợp bằng nhau được phân biệt theo thứ tự từ điển của chỉ số. Duyệt từng cặp sẽ không đáp ứng được với $n \le 10^5$. Các điểm trùng nhau có khoảng cách bằng $0$, nên ngay lập tức cho ta cặp chỉ số nhỏ nhất.
>
> Nếu không có điểm trùng nhau, ta áp dụng cách chia để trị kinh điển cho bài toán tìm cặp gần nhau nhất: sắp xếp theo $x$, đệ quy trên hai nửa, sau đó duyệt dải có độ rộng $d$ theo $y$ để xét các cặp ở hai nửa khác nhau, dùng chỉ số để phá hòa.

<!-- thinking:end -->

Bài toán này tương đương với việc tìm hai điểm trên mặt phẳng sao cho khoảng cách Manhattan giữa chúng là nhỏ nhất. Nếu có nhiều điểm thỏa mãn, hãy trả về cặp có chỉ số nhỏ nhất.

Trước tiên, ta xử lý trường hợp có các điểm trùng nhau. Với mỗi điểm, ta lưu các chỉ số tương ứng vào một danh sách. Nếu độ dài danh sách chỉ số lớn hơn $1$, hai chỉ số đầu tiên trong danh sách có thể dùng làm ứng viên, sau đó ta tìm cặp chỉ số nhỏ nhất.

Nếu không có điểm trùng nhau, ta sắp xếp tất cả các điểm theo tọa độ $x$, rồi dùng chia để trị để giải bài toán.

Với mỗi đoạn $[l, r]$, trước tiên ta tính trung vị của các tọa độ $x$ là $m$, sau đó đệ quy giải các đoạn bên trái và bên phải, lần lượt nhận được $d_1, (pi_1, pj_1)$ và $d_2, (pi_2, pj_2)$, trong đó $d_1$ và $d_2$ là khoảng cách Manhattan nhỏ nhất của đoạn bên trái và bên phải, còn $(pi_1, pj_1)$ và $(pi_2, pj_2)$ là cặp chỉ số của hai điểm có khoảng cách Manhattan nhỏ nhất tương ứng trong mỗi đoạn. Ta chọn giá trị nhỏ hơn giữa $d_1$ và $d_2$ làm khoảng cách Manhattan nhỏ nhất của đoạn hiện tại; nếu $d_1 = d_2$, ta chọn cặp có chỉ số nhỏ hơn làm đáp án. Hai điểm tương ứng với cặp chỉ số đó được chọn làm đáp án.

Phần trên xét trường hợp hai điểm nằm cùng một phía. Nếu hai điểm nằm ở hai phía khác nhau, ta lấy điểm giữa, tức điểm có chỉ số $m = \lfloor (l + r) / 2 \rfloor$, làm mốc và chia một vùng mới. Phạm vi của vùng này được mở rộng từ điểm giữa sang hai phía trái và phải một khoảng $d_1$. Sau đó, ta sắp xếp các điểm trong vùng theo tọa độ $y$ rồi duyệt từng cặp điểm theo thứ tự đã sắp xếp. Nếu hiệu tọa độ $y$ của hai điểm lớn hơn khoảng cách Manhattan nhỏ nhất hiện tại, các cặp điểm tiếp theo không cần được xét nữa, vì hiệu tọa độ $y$ của chúng lớn hơn, nên khoảng cách Manhattan cũng lớn hơn và không thể nhỏ hơn khoảng cách Manhattan nhỏ nhất hiện tại. Ngược lại, ta cập nhật khoảng cách Manhattan nhỏ nhất và đáp án.

Cuối cùng, ta trả về đáp án.

Độ phức tạp thời gian: $O(n \times \log n)$, trong đó $n$ là độ dài của mảng.

Độ phức tạp không gian: $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def beautifulPair(self, nums1: List[int], nums2: List[int]) -> List[int]:
        def dist(x1: int, y1: int, x2: int, y2: int) -> int:
            return abs(x1 - x2) + abs(y1 - y2)

        def dfs(l: int, r: int):
            if l >= r:
                return inf, -1, -1
            m = (l + r) >> 1
            x = points[m][0]
            d1, pi1, pj1 = dfs(l, m)
            d2, pi2, pj2 = dfs(m + 1, r)
            if d1 > d2 or (d1 == d2 and (pi1 > pi2 or (pi1 == pi2 and pj1 > pj2))):
                d1, pi1, pj1 = d2, pi2, pj2
            t = [p for p in points[l : r + 1] if abs(p[0] - x) <= d1]
            t.sort(key=lambda x: x[1])
            for i in range(len(t)):
                for j in range(i + 1, len(t)):
                    if t[j][1] - t[i][1] > d1:
                        break
                    pi, pj = sorted([t[i][2], t[j][2]])
                    d = dist(t[i][0], t[i][1], t[j][0], t[j][1])
                    if d < d1 or (d == d1 and (pi < pi1 or (pi == pi1 and pj < pj1))):
                        d1, pi1, pj1 = d, pi, pj
            return d1, pi1, pj1

        pl = defaultdict(list)
        for i, (x, y) in enumerate(zip(nums1, nums2)):
            pl[(x, y)].append(i)
        points = []
        for i, (x, y) in enumerate(zip(nums1, nums2)):
            if len(pl[(x, y)]) > 1:
                return [i, pl[(x, y)][1]]
            points.append((x, y, i))
        points.sort()
        _, pi, pj = dfs(0, len(points) - 1)
        return [pi, pj]
```

#### Java

```java
class Solution {
    private List<int[]> points = new ArrayList<>();

    public int[] beautifulPair(int[] nums1, int[] nums2) {
        int n = nums1.length;
        Map<Long, List<Integer>> pl = new HashMap<>();
        for (int i = 0; i < n; ++i) {
            long z = f(nums1[i], nums2[i]);
            pl.computeIfAbsent(z, k -> new ArrayList<>()).add(i);
        }
        for (int i = 0; i < n; ++i) {
            long z = f(nums1[i], nums2[i]);
            if (pl.get(z).size() > 1) {
                return new int[] {i, pl.get(z).get(1)};
            }
            points.add(new int[] {nums1[i], nums2[i], i});
        }
        points.sort((a, b) -> a[0] - b[0]);
        int[] ans = dfs(0, points.size() - 1);
        return new int[] {ans[1], ans[2]};
    }

    private long f(int x, int y) {
        return x * 100000L + y;
    }

    private int dist(int x1, int y1, int x2, int y2) {
        return Math.abs(x1 - x2) + Math.abs(y1 - y2);
    }

    private int[] dfs(int l, int r) {
        if (l >= r) {
            return new int[] {1 << 30, -1, -1};
        }
        int m = (l + r) >> 1;
        int x = points.get(m)[0];
        int[] t1 = dfs(l, m);
        int[] t2 = dfs(m + 1, r);
        if (t1[0] > t2[0]
            || (t1[0] == t2[0] && (t1[1] > t2[1] || (t1[1] == t2[1] && t1[2] > t2[2])))) {
            t1 = t2;
        }
        List<int[]> t = new ArrayList<>();
        for (int i = l; i <= r; ++i) {
            if (Math.abs(points.get(i)[0] - x) <= t1[0]) {
                t.add(points.get(i));
            }
        }
        t.sort((a, b) -> a[1] - b[1]);
        for (int i = 0; i < t.size(); ++i) {
            for (int j = i + 1; j < t.size(); ++j) {
                if (t.get(j)[1] - t.get(i)[1] > t1[0]) {
                    break;
                }
                int pi = Math.min(t.get(i)[2], t.get(j)[2]);
                int pj = Math.max(t.get(i)[2], t.get(j)[2]);
                int d = dist(t.get(i)[0], t.get(i)[1], t.get(j)[0], t.get(j)[1]);
                if (d < t1[0] || (d == t1[0] && (pi < t1[1] || (pi == t1[1] && pj < t1[2])))) {
                    t1 = new int[] {d, pi, pj};
                }
            }
        }
        return t1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> beautifulPair(vector<int>& nums1, vector<int>& nums2) {
        int n = nums1.size();
        unordered_map<long long, vector<int>> pl;
        for (int i = 0; i < n; ++i) {
            pl[f(nums1[i], nums2[i])].push_back(i);
        }
        vector<tuple<int, int, int>> points;
        for (int i = 0; i < n; ++i) {
            long long z = f(nums1[i], nums2[i]);
            if (pl[z].size() > 1) {
                return {i, pl[z][1]};
            }
            points.emplace_back(nums1[i], nums2[i], i);
        }

        function<tuple<int, int, int>(int, int)> dfs = [&](int l, int r) -> tuple<int, int, int> {
            if (l >= r) {
                return {1 << 30, -1, -1};
            }
            int m = (l + r) >> 1;
            int x = get<0>(points[m]);
            auto t1 = dfs(l, m);
            auto t2 = dfs(m + 1, r);
            if (get<0>(t1) > get<0>(t2) || (get<0>(t1) == get<0>(t2) && (get<1>(t1) > get<1>(t2) || (get<1>(t1) == get<1>(t2) && get<2>(t1) > get<2>(t2))))) {
                swap(t1, t2);
            }
            vector<tuple<int, int, int>> t;
            for (int i = l; i <= r; ++i) {
                if (abs(get<0>(points[i]) - x) <= get<0>(t1)) {
                    t.emplace_back(points[i]);
                }
            }
            sort(t.begin(), t.end(), [](const tuple<int, int, int>& a, const tuple<int, int, int>& b) {
                return get<1>(a) < get<1>(b);
            });
            for (int i = 0; i < t.size(); ++i) {
                for (int j = i + 1; j < t.size(); ++j) {
                    if (get<1>(t[j]) - get<1>(t[i]) > get<0>(t1)) {
                        break;
                    }
                    int pi = min(get<2>(t[i]), get<2>(t[j]));
                    int pj = max(get<2>(t[i]), get<2>(t[j]));
                    int d = dist(get<0>(t[i]), get<1>(t[i]), get<0>(t[j]), get<1>(t[j]));
                    if (d < get<0>(t1) || (d == get<0>(t1) && (pi < get<1>(t1) || (pi == get<1>(t1) && pj < get<2>(t1))))) {
                        t1 = {d, pi, pj};
                    }
                }
            }
            return t1;
        };

        sort(points.begin(), points.end());
        auto [_, pi, pj] = dfs(0, points.size() - 1);
        return {pi, pj};
    }

    long long f(int x, int y) {
        return x * 100000LL + y;
    }

    int dist(int x1, int y1, int x2, int y2) {
        return abs(x1 - x2) + abs(y1 - y2);
    }
};
```

#### Go

```go
func beautifulPair(nums1 []int, nums2 []int) []int {
	n := len(nums1)
	pl := map[[2]int][]int{}
	for i := 0; i < n; i++ {
		k := [2]int{nums1[i], nums2[i]}
		pl[k] = append(pl[k], i)
	}
	points := [][3]int{}
	for i := 0; i < n; i++ {
		k := [2]int{nums2[i], nums1[i]}
		if len(pl[k]) > 1 {
			return []int{pl[k][0], pl[k][1]}
		}
		points = append(points, [3]int{nums1[i], nums2[i], i})
	}
	sort.Slice(points, func(i, j int) bool { return points[i][0] < points[j][0] })

	var dfs func(l, r int) [3]int
	dfs = func(l, r int) [3]int {
		if l >= r {
			return [3]int{1 << 30, -1, -1}
		}
		m := (l + r) >> 1
		x := points[m][0]
		t1 := dfs(l, m)
		t2 := dfs(m+1, r)
		if t1[0] > t2[0] || (t1[0] == t2[0] && (t1[1] > t2[1] || (t1[1] == t2[1] && t1[2] > t2[2]))) {
			t1 = t2
		}
		t := [][3]int{}
		for i := l; i <= r; i++ {
			if abs(points[i][0]-x) <= t1[0] {
				t = append(t, points[i])
			}
		}
		sort.Slice(t, func(i, j int) bool { return t[i][1] < t[j][1] })
		for i := 0; i < len(t); i++ {
			for j := i + 1; j < len(t); j++ {
				if t[j][1]-t[i][1] > t1[0] {
					break
				}
				pi := min(t[i][2], t[j][2])
				pj := max(t[i][2], t[j][2])
				d := dist(t[i][0], t[i][1], t[j][0], t[j][1])
				if d < t1[0] || (d == t1[0] && (pi < t1[1] || (pi == t1[1] && pj < t1[2]))) {
					t1 = [3]int{d, pi, pj}
				}
			}
		}
		return t1
	}
	ans := dfs(0, n-1)
	return []int{ans[1], ans[2]}
}

func dist(x1, y1, x2, y2 int) int {
	return abs(x1-x2) + abs(y1-y2)
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
function beautifulPair(nums1: number[], nums2: number[]): number[] {
    const pl: Map<number, number[]> = new Map();
    const n = nums1.length;
    for (let i = 0; i < n; ++i) {
        const z = f(nums1[i], nums2[i]);
        if (!pl.has(z)) {
            pl.set(z, []);
        }
        pl.get(z)!.push(i);
    }
    const points: number[][] = [];
    for (let i = 0; i < n; ++i) {
        const z = f(nums1[i], nums2[i]);
        if (pl.get(z)!.length > 1) {
            return [i, pl.get(z)![1]];
        }
        points.push([nums1[i], nums2[i], i]);
    }
    points.sort((a, b) => a[0] - b[0]);

    const dfs = (l: number, r: number): number[] => {
        if (l >= r) {
            return [1 << 30, -1, -1];
        }
        const m = (l + r) >> 1;
        const x = points[m][0];
        let t1 = dfs(l, m);
        let t2 = dfs(m + 1, r);
        if (
            t1[0] > t2[0] ||
            (t1[0] == t2[0] && (t1[1] > t2[1] || (t1[1] == t2[1] && t1[2] > t2[2])))
        ) {
            t1 = t2;
        }
        const t: number[][] = [];
        for (let i = l; i <= r; ++i) {
            if (Math.abs(points[i][0] - x) <= t1[0]) {
                t.push(points[i]);
            }
        }
        t.sort((a, b) => a[1] - b[1]);
        for (let i = 0; i < t.length; ++i) {
            for (let j = i + 1; j < t.length; ++j) {
                if (t[j][1] - t[i][1] > t1[0]) {
                    break;
                }
                const pi = Math.min(t[i][2], t[j][2]);
                const pj = Math.max(t[i][2], t[j][2]);
                const d = dist(t[i][0], t[i][1], t[j][0], t[j][1]);
                if (d < t1[0] || (d == t1[0] && (pi < t1[1] || (pi == t1[1] && pj < t1[2])))) {
                    t1 = [d, pi, pj];
                }
            }
        }
        return t1;
    };
    return dfs(0, n - 1).slice(1);
}

function dist(x1: number, y1: number, x2: number, y2: number): number {
    return Math.abs(x1 - x2) + Math.abs(y1 - y2);
}

function f(x: number, y: number): number {
    return x * 100000 + y;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
