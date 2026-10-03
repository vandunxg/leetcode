---
comments: true
difficulty: Medium
tags:
    - Stack
    - Array
    - Sorting
    - Monotonic Stack
---

<!-- problem:start -->

# [2345. Finding the Number of Visible Mountains 🔒](https://leetcode.com/problems/finding-the-number-of-visible-mountains)

[中文文档](/solution/2300-2399/2345.Finding%20the%20Number%20of%20Visible%20Mountains/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên 2 chiều <strong>đánh chỉ số từ 0</strong> <code>peaks</code>, trong đó <code>peaks[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> cho biết ngọn núi <code>i</code> có đỉnh tại tọa độ <code>(x<sub>i</sub>, y<sub>i</sub>)</code>. Một ngọn núi có thể được mô tả là một tam giác vuông cân, với đáy nằm trên trục <code>x</code> và góc vuông tại đỉnh. Cụ thể hơn, <strong>độ dốc</strong> khi đi lên và đi xuống núi lần lượt là <code>1</code> và <code>-1</code>.</p>

<p>Một ngọn núi được xem là <strong>nhìn thấy được</strong> nếu đỉnh của nó không nằm bên trong một ngọn núi khác (kể cả trên biên của ngọn núi khác).</p>

<p>Trả về <em>số lượng ngọn núi nhìn thấy được</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2345.Finding%20the%20Number%20of%20Visible%20Mountains/images/ex1.png" style="width: 402px; height: 210px;" />
<pre>
<strong>Đầu vào:</strong> peaks = [[2,2],[6,3],[5,4]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Hình trên minh họa các ngọn núi.
- Núi 0 nhìn thấy được vì đỉnh của nó không nằm bên trong ngọn núi khác hoặc trên sườn của ngọn núi khác.
- Núi 1 không nhìn thấy được vì đỉnh của nó nằm trên sườn của núi 2.
- Núi 2 nhìn thấy được vì đỉnh của nó không nằm bên trong ngọn núi khác hoặc trên sườn của ngọn núi khác.
Có 2 ngọn núi nhìn thấy được.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2300-2399/2345.Finding%20the%20Number%20of%20Visible%20Mountains/images/ex2new1.png" style="width: 300px; height: 180px;" />
<pre>
<strong>Đầu vào:</strong> peaks = [[1,3],[1,3]]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Hình trên minh họa các ngọn núi (chúng hoàn toàn chồng lên nhau).
Cả hai ngọn núi đều không nhìn thấy được vì đỉnh của chúng nằm bên trong ngọn núi còn lại.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= peaks.length &lt;= 10<sup>5</sup></code></li>
	<li><code>peaks[i].length == 2</code></li>
	<li><code>1 &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp khoảng + duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Một đỉnh nhìn thấy được khi và chỉ khi không có đỉnh nào khác bao phủ nó. Đỉnh $(x,y)$ bao phủ khoảng $(x-y,x+y)$. Các khoảng trùng nhau che khuất lẫn nhau.
>
> Sắp xếp theo đầu trái tăng dần và đầu phải giảm dần, sau đó duyệt: nếu đầu phải không vượt qua giá trị lớn nhất hiện tại thì khoảng đó đã bị chứa. Chỉ đếm một khoảng khi nó là duy nhất và mở rộng giá trị lớn nhất đó.

<!-- thinking:end -->

Trước hết, ta chuyển mỗi ngọn núi $(x, y)$ thành một khoảng ngang $(x - y, x + y)$, sau đó sắp xếp các khoảng theo đầu trái tăng dần và đầu phải giảm dần.

Tiếp theo, ta khởi tạo đầu phải của khoảng hiện tại bằng $-\infty$. Ta duyệt qua từng ngọn núi. Nếu đầu phải của ngọn núi hiện tại nhỏ hơn hoặc bằng đầu phải của khoảng hiện tại, ta bỏ qua ngọn núi này. Ngược lại, ta cập nhật đầu phải của khoảng hiện tại thành đầu phải của ngọn núi hiện tại. Nếu khoảng của ngọn núi hiện tại chỉ xuất hiện một lần, ta tăng đáp án lên một.

Cuối cùng, ta trả về đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số lượng ngọn núi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def visibleMountains(self, peaks: List[List[int]]) -> int:
        arr = [(x - y, x + y) for x, y in peaks]
        cnt = Counter(arr)
        arr.sort(key=lambda x: (x[0], -x[1]))
        ans, cur = 0, -inf
        for l, r in arr:
            if r <= cur:
                continue
            cur = r
            if cnt[(l, r)] == 1:
                ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int visibleMountains(int[][] peaks) {
        int n = peaks.length;
        int[][] arr = new int[n][2];
        for (int i = 0; i < n; ++i) {
            int x = peaks[i][0], y = peaks[i][1];
            arr[i] = new int[] {x - y, x + y};
        }
        Arrays.sort(arr, (a, b) -> a[0] == b[0] ? b[1] - a[1] : a[0] - b[0]);
        int ans = 0;
        int cur = Integer.MIN_VALUE;
        for (int i = 0; i < n; ++i) {
            int l = arr[i][0], r = arr[i][1];
            if (r <= cur) {
                continue;
            }
            cur = r;
            if (!(i < n - 1 && arr[i][0] == arr[i + 1][0] && arr[i][1] == arr[i + 1][1])) {
                ++ans;
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
    int visibleMountains(vector<vector<int>>& peaks) {
        vector<pair<int, int>> arr;
        for (auto& e : peaks) {
            int x = e[0], y = e[1];
            arr.emplace_back(x - y, -(x + y));
        }
        sort(arr.begin(), arr.end());
        int n = arr.size();
        int ans = 0, cur = INT_MIN;
        for (int i = 0; i < n; ++i) {
            int l = arr[i].first, r = -arr[i].second;
            if (r <= cur) {
                continue;
            }
            cur = r;
            ans += i == n - 1 || (i < n - 1 && arr[i] != arr[i + 1]);
        }
        return ans;
    }
};
```

#### Go

```go
func visibleMountains(peaks [][]int) (ans int) {
	n := len(peaks)
	type pair struct{ l, r int }
	arr := make([]pair, n)
	for _, p := range peaks {
		x, y := p[0], p[1]
		arr = append(arr, pair{x - y, x + y})
	}
	sort.Slice(arr, func(i, j int) bool { return arr[i].l < arr[j].l || (arr[i].l == arr[j].l && arr[i].r > arr[j].r) })
	cur := math.MinInt32
	for i, e := range arr {
		l, r := e.l, e.r
		if r <= cur {
			continue
		}
		cur = r
		if !(i < n-1 && l == arr[i+1].l && r == arr[i+1].r) {
			ans++
		}
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
