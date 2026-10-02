---
comments: true
difficulty: Medium
tags:
    - Array
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [436. Find Right Interval](https://leetcode.com/problems/find-right-interval)

[中文文档](/solution/0400-0499/0436.Find%20Right%20Interval/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>intervals</code>, trong đó <code>intervals[i] = [start<sub>i</sub>, end<sub>i</sub>]</code> và mỗi <code>start<sub>i</sub></code> đều <strong>khác nhau</strong>.</p>

<p><strong>Right interval</strong> của interval <code>i</code> là interval <code>j</code> thỏa mãn <code>start<sub>j</sub> &gt;= end<sub>i</sub></code> và có <code>start<sub>j</sub></code> <strong>nhỏ nhất</strong>. Lưu ý rằng <code>i</code> có thể bằng <code>j</code>.</p>

<p>Trả về <em>mảng chứa chỉ số của <strong>right interval</strong> tương ứng với mỗi interval <code>i</code></em>. Nếu interval <code>i</code> không có <strong>right interval</strong>, hãy đặt <code>-1</code> tại chỉ số <code>i</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> intervals = [[1,2]]
<strong>Đầu ra:</strong> [-1]
<strong>Giải thích:</strong> Tập chỉ có một interval nên kết quả là -1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> intervals = [[3,4],[2,3],[1,2]]
<strong>Đầu ra:</strong> [-1,0,1]
<strong>Giải thích:</strong> [3,4] không có right interval.
Right interval của [2,3] là [3,4], vì start<sub>0</sub> = 3 là điểm bắt đầu nhỏ nhất thỏa mãn start<sub>0</sub> &gt;= end<sub>1</sub> = 3.
Right interval của [1,2] là [2,3], vì start<sub>1</sub> = 2 là điểm bắt đầu nhỏ nhất thỏa mãn start<sub>1</sub> &gt;= end<sub>2</sub> = 2.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> intervals = [[1,4],[2,3],[3,4]]
<strong>Đầu ra:</strong> [-1,2,-1]
<strong>Giải thích:</strong> [1,4] và [3,4] không có right interval.
Right interval của [2,3] là [3,4], vì start<sub>2</sub> = 3 là điểm bắt đầu nhỏ nhất thỏa mãn start<sub>2</sub> &gt;= end<sub>1</sub> = 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= intervals.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>intervals[i].length == 2</code></li>
	<li><code>-10<sup>6</sup> &lt;= start<sub>i</sub> &lt;= end<sub>i</sub> &lt;= 10<sup>6</sup></code></li>
	<li>Điểm bắt đầu của mỗi interval đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi interval, cần tìm điểm bắt đầu nhỏ nhất nhưng vẫn lớn hơn hoặc bằng điểm kết thúc của nó. Duyệt tuyến tính cho từng điểm kết thúc sẽ tốn $O(n^2)$.
>
> Sắp xếp các cặp $(\textit{start},\textit{index})$ theo điểm bắt đầu; với mỗi điểm kết thúc, dùng tìm kiếm nhị phân để tìm điểm bắt đầu đầu tiên $\ge$ điểm kết thúc đó.
>
> Các điểm bắt đầu sau khi sắp xếp có tính đơn điệu, nên điểm đầu tiên thỏa mãn chính là điểm gần nhất. Lưu chỉ số ban đầu để vẫn xác định được interval sau khi sắp xếp.

<!-- thinking:end -->

Ta có thể lưu điểm bắt đầu và chỉ số của mỗi interval vào mảng `arr`, rồi sắp xếp mảng theo điểm bắt đầu. Sau đó, duyệt mảng interval; với mỗi interval `[_, ed]`, dùng tìm kiếm nhị phân để tìm interval đầu tiên có điểm bắt đầu lớn hơn hoặc bằng `ed`. Đây là right interval cần tìm. Nếu tìm thấy, lưu chỉ số của nó vào mảng kết quả; nếu không, lưu `-1`.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng intervals.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findRightInterval(self, intervals: List[List[int]]) -> List[int]:
        n = len(intervals)
        ans = [-1] * n
        arr = sorted((st, i) for i, (st, _) in enumerate(intervals))
        for i, (_, ed) in enumerate(intervals):
            j = bisect_left(arr, (ed, -inf))
            if j < n:
                ans[i] = arr[j][1]
        return ans
```

#### Java

```java
class Solution {
    public int[] findRightInterval(int[][] intervals) {
        int n = intervals.length;
        int[][] arr = new int[n][0];
        for (int i = 0; i < n; ++i) {
            arr[i] = new int[] {intervals[i][0], i};
        }
        Arrays.sort(arr, (a, b) -> a[0] - b[0]);
        int[] ans = new int[n];
        for (int i = 0; i < n; ++i) {
            int j = search(arr, intervals[i][1]);
            ans[i] = j < n ? arr[j][1] : -1;
        }
        return ans;
    }

    private int search(int[][] arr, int x) {
        int l = 0, r = arr.length;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (arr[mid][0] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> findRightInterval(vector<vector<int>>& intervals) {
        int n = intervals.size();
        vector<pair<int, int>> arr;
        for (int i = 0; i < n; ++i) {
            arr.emplace_back(intervals[i][0], i);
        }
        sort(arr.begin(), arr.end());
        vector<int> ans;
        for (auto& e : intervals) {
            int j = lower_bound(arr.begin(), arr.end(), make_pair(e[1], -1)) - arr.begin();
            ans.push_back(j == n ? -1 : arr[j].second);
        }
        return ans;
    }
};
```

#### Go

```go
func findRightInterval(intervals [][]int) (ans []int) {
	arr := make([][2]int, len(intervals))
	for i, v := range intervals {
		arr[i] = [2]int{v[0], i}
	}
	sort.Slice(arr, func(i, j int) bool { return arr[i][0] < arr[j][0] })
	for _, e := range intervals {
		j := sort.Search(len(arr), func(i int) bool { return arr[i][0] >= e[1] })
		if j < len(arr) {
			ans = append(ans, arr[j][1])
		} else {
			ans = append(ans, -1)
		}
	}
	return
}
```

#### TypeScript

```ts
function findRightInterval(intervals: number[][]): number[] {
    const n = intervals.length;
    const arr: number[][] = Array.from({ length: n }, (_, i) => [intervals[i][0], i]);
    arr.sort((a, b) => a[0] - b[0]);
    return intervals.map(([_, ed]) => {
        let [l, r] = [0, n];
        while (l < r) {
            const mid = (l + r) >> 1;
            if (arr[mid][0] >= ed) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l < n ? arr[l][1] : -1;
    });
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
