---
comments: true
difficulty: Hard
rating: 2723
source: Weekly Contest 431 Q4
tags:
    - Array
    - Binary Search
    - Dynamic Programming
    - Sorting
---

<!-- problem:start -->

# [3414. Maximum Score of Non-overlapping Intervals](https://leetcode.com/problems/maximum-score-of-non-overlapping-intervals)

[中文文档](/solution/3400-3499/3414.Maximum%20Score%20of%20Non-overlapping%20Intervals/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên 2 chiều <code>intervals</code>, trong đó <code>intervals[i] = [l<sub>i</sub>, r<sub>i</sub>, weight<sub>i</sub>]</code>. Khoảng <code>i</code> bắt đầu tại vị trí <code>l<sub>i</sub></code>, kết thúc tại <code>r<sub>i</sub></code> và có trọng số <code>weight<sub>i</sub></code>. Bạn có thể chọn <em>tối đa</em> 4 khoảng <strong>không chồng lấn</strong>. <strong>Điểm số</strong> của các khoảng được chọn là tổng trọng số của chúng.</p>

<p>Hãy trả về mảng gồm nhiều nhất 4 chỉ số <span data-keyword="lexicographically-smaller-array">nhỏ nhất theo thứ tự từ điển</span> trong <code>intervals</code> có <strong>điểm số lớn nhất</strong>, biểu diễn lựa chọn các khoảng không chồng lấn của bạn.</p>

<p>Hai khoảng được gọi là <strong>không chồng lấn</strong> nếu chúng không có điểm chung. Đặc biệt, các khoảng có chung biên trái hoặc biên phải được xem là chồng lấn.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">intervals = [[1,3,2],[4,5,2],[1,5,5],[6,9,3],[6,7,1],[8,9,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,3]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Bạn có thể chọn các khoảng có chỉ số 2 và 3, với trọng số tương ứng là 5 và 3.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">intervals = [[5,8,1],[6,7,7],[4,7,3],[9,10,6],[7,8,2],[11,14,3],[3,5,5]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,3,5,6]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Bạn có thể chọn các khoảng có chỉ số 1, 3, 5 và 6, với trọng số tương ứng là 7, 6, 3 và 5.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= intevals.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>intervals[i].length == 3</code></li>
	<li><code>intervals[i] = [l<sub>i</sub>, r<sub>i</sub>, weight<sub>i</sub>]</code></li>
	<li><code>1 &lt;= l<sub>i</sub> &lt;= r<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= weight<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Tìm kiếm nhị phân + Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Ta chọn tối đa bốn khoảng không chồng lấn có trọng số để tối đa hóa tổng trọng số, nếu hòa thì chọn bộ chỉ số nhỏ nhất theo thứ tự từ điển. $n\le 5\times 10^4$ nên không thể tìm kiếm trên các tập con.
>
> Đây là bài toán lập lịch khoảng có trọng số với giới hạn bốn khoảng. Sau khi sắp xếp theo đầu mút trái, khoảng tiếp theo không chồng lấn được tìm bằng tìm kiếm nhị phân.
>
> Trạng thái $(i,k)$ bắt đầu từ khoảng $i$ với $k$ lượt chọn còn lại. Ta có thể bỏ qua $i$ hoặc chọn nó rồi nhảy đến $\textit{nxt}[i]$, so sánh cả trọng số và danh sách chỉ số để giữ lại nghiệm tối ưu có thứ tự từ điển nhỏ nhất.

<!-- thinking:end -->

Sao chép các khoảng và ghi lại chỉ số ban đầu của từng khoảng, sau đó sắp xếp theo đầu mút trái. Với mỗi khoảng $i$, dùng tìm kiếm nhị phân để tìm vị trí đầu tiên $\textit{nxt}[i]$ có đầu mút trái lớn hơn nghiêm ngặt đầu mút phải của khoảng $i$ (các đầu mút chung được xem là chồng lấn).

Gọi $f[i][k]$ là trọng số lớn nhất có thể đạt được từ khoảng $i$ trở đi khi được chọn nhiều nhất $k$ khoảng, và $g[i][k]$ lưu danh sách chỉ số nhỏ nhất theo thứ tự từ điển tương ứng. Chuyển trạng thái từ cuối về đầu: bỏ qua $i$ kế thừa $f[i+1][k]$; chọn $i$ thì chèn chỉ số ban đầu của nó vào $g[\textit{nxt}[i]][k-1]$ và cộng trọng số hiện tại. Chọn phương án có trọng số lớn hơn, hoặc danh sách chỉ số nhỏ hơn theo thứ tự từ điển nếu hai trọng số bằng nhau. Đáp án là $g[0][4]$.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$. Vì chọn nhiều nhất $4$ khoảng, việc chèn và so sánh các danh sách chỉ số đều có thời gian hằng số.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumWeight(self, intervals: List[List[int]]) -> List[int]:
        n = len(intervals)
        arr = [[e[0], e[1], e[2], i] for i, e in enumerate(intervals)]
        arr.sort()
        nxt = [0] * n
        for i in range(n):
            l, r = i + 1, n
            while l < r:
                mid = (l + r) >> 1
                if arr[mid][0] > arr[i][1]:
                    r = mid
                else:
                    l = mid + 1
            nxt[i] = l
        f = [[0] * 5 for _ in range(n + 1)]
        g = [[[] for _ in range(5)] for _ in range(n + 1)]
        for i in range(n - 1, -1, -1):
            for k in range(1, 5):
                s1, a1 = f[i + 1][k], g[i + 1][k]
                a2 = g[nxt[i]][k - 1][:]
                x = arr[i][3]
                j = 0
                while j < len(a2) and a2[j] < x:
                    j += 1
                a2.insert(j, x)
                s2 = f[nxt[i]][k - 1] + arr[i][2]
                if s2 > s1 or (s2 == s1 and a2 < a1):
                    f[i][k] = s2
                    g[i][k] = a2
                else:
                    f[i][k] = s1
                    g[i][k] = a1
        return g[0][4]
```

#### Java

```java
class Solution {
    public int[] maximumWeight(List<List<Integer>> intervals) {
        int n = intervals.size();
        int[][] arr = new int[n][4];
        for (int i = 0; i < n; ++i) {
            List<Integer> e = intervals.get(i);
            arr[i] = new int[] {e.get(0), e.get(1), e.get(2), i};
        }
        Arrays.sort(arr,
            (a, b) -> a[0] != b[0] ? Integer.compare(a[0], b[0]) : Integer.compare(a[1], b[1]));
        int[] nxt = new int[n];
        for (int i = 0; i < n; ++i) {
            nxt[i] = search(arr, arr[i][1], i + 1);
        }
        long[][] f = new long[n + 1][5];
        int[][][] g = new int[n + 1][5][];
        for (int k = 0; k < 5; ++k) {
            g[n][k] = new int[0];
        }
        for (int i = n - 1; i >= 0; --i) {
            g[i][0] = new int[0];
            for (int k = 1; k < 5; ++k) {
                long s1 = f[i + 1][k];
                int[] a1 = g[i + 1][k];
                long s2 = f[nxt[i]][k - 1] + arr[i][2];
                int[] a2 = insert(g[nxt[i]][k - 1], arr[i][3]);
                if (s2 > s1 || (s2 == s1 && less(a2, a1))) {
                    f[i][k] = s2;
                    g[i][k] = a2;
                } else {
                    f[i][k] = s1;
                    g[i][k] = a1;
                }
            }
        }
        return g[0][4];
    }

    private int search(int[][] arr, int x, int l) {
        int r = arr.length;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (arr[mid][0] > x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }

    private int[] insert(int[] a, int x) {
        int n = a.length;
        int[] b = new int[n + 1];
        int i = 0;
        while (i < n && a[i] < x) {
            b[i] = a[i];
            ++i;
        }
        b[i] = x;
        while (i < n) {
            b[i + 1] = a[i];
            ++i;
        }
        return b;
    }

    private boolean less(int[] a, int[] b) {
        int m = Math.min(a.length, b.length);
        for (int i = 0; i < m; ++i) {
            if (a[i] != b[i]) {
                return a[i] < b[i];
            }
        }
        return a.length < b.length;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> maximumWeight(vector<vector<int>>& intervals) {
        int n = intervals.size();
        vector<array<int, 4>> arr(n);
        for (int i = 0; i < n; ++i) {
            arr[i] = {intervals[i][0], intervals[i][1], intervals[i][2], i};
        }
        ranges::sort(arr);
        vector<int> nxt(n);
        for (int i = 0; i < n; ++i) {
            int l = i + 1, r = n;
            while (l < r) {
                int mid = (l + r) >> 1;
                if (arr[mid][0] > arr[i][1]) {
                    r = mid;
                } else {
                    l = mid + 1;
                }
            }
            nxt[i] = l;
        }
        vector<vector<long long>> f(n + 1, vector<long long>(5));
        vector<vector<vector<int>>> g(n + 1, vector<vector<int>>(5));
        for (int i = n - 1; i >= 0; --i) {
            for (int k = 1; k < 5; ++k) {
                long long s1 = f[i + 1][k];
                vector<int> a1 = g[i + 1][k];
                long long s2 = f[nxt[i]][k - 1] + arr[i][2];
                vector<int> a2 = g[nxt[i]][k - 1];
                a2.insert(ranges::lower_bound(a2, arr[i][3]), arr[i][3]);
                if (s2 > s1 || (s2 == s1 && a2 < a1)) {
                    f[i][k] = s2;
                    g[i][k] = move(a2);
                } else {
                    f[i][k] = s1;
                    g[i][k] = move(a1);
                }
            }
        }
        return g[0][4];
    }
};
```

#### Go

```go
func maximumWeight(intervals [][]int) []int {
	n := len(intervals)
	arr := make([][4]int, n)
	for i, e := range intervals {
		arr[i] = [4]int{e[0], e[1], e[2], i}
	}
	sort.Slice(arr, func(i, j int) bool {
		if arr[i][0] != arr[j][0] {
			return arr[i][0] < arr[j][0]
		}
		return arr[i][1] < arr[j][1]
	})
	nxt := make([]int, n)
	for i := 0; i < n; i++ {
		l, r := i+1, n
		for l < r {
			mid := (l + r) >> 1
			if arr[mid][0] > arr[i][1] {
				r = mid
			} else {
				l = mid + 1
			}
		}
		nxt[i] = l
	}
	f := make([][5]int64, n+1)
	g := make([][5][]int, n+1)
	for i := n - 1; i >= 0; i-- {
		for k := 1; k < 5; k++ {
			s1, a1 := f[i+1][k], g[i+1][k]
			a2 := append([]int(nil), g[nxt[i]][k-1]...)
			x := arr[i][3]
			j := sort.SearchInts(a2, x)
			a2 = append(a2, 0)
			copy(a2[j+1:], a2[j:])
			a2[j] = x
			s2 := f[nxt[i]][k-1] + int64(arr[i][2])
			if s2 > s1 || (s2 == s1 && lessInts(a2, a1)) {
				f[i][k] = s2
				g[i][k] = a2
			} else {
				f[i][k] = s1
				g[i][k] = a1
			}
		}
	}
	return g[0][4]
}

func lessInts(a, b []int) bool {
	m := min(len(a), len(b))
	for i := 0; i < m; i++ {
		if a[i] != b[i] {
			return a[i] < b[i]
		}
	}
	return len(a) < len(b)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
