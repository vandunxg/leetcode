---
comments: true
difficulty: Medium
rating: 1533
source: Weekly Contest 508 Q2
tags:
    - Array
    - Sorting
---

<!-- problem:start -->

# [3975. Filter Occupied Intervals](https://leetcode.com/problems/filter-occupied-intervals)

[中文文档](/solution/3900-3999/3975.Filter%20Occupied%20Intervals/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên hai chiều <code>occupiedIntervals</code>, trong đó <code>occupiedIntervals[i] = [start<sub>i</sub>, end<sub>i</sub>]</code> biểu thị một khoảng thời gian bạn bận. Mỗi khoảng bắt đầu tại <code>start<sub>i</sub></code> và kết thúc tại <code>end<sub>i</sub></code>, <strong>bao gồm</strong> cả hai đầu mút. Các khoảng này có thể <strong>chồng lấn</strong>.</p>

<p>Bạn cũng được cho hai số nguyên <code>freeStart</code> và <code>freeEnd</code>, xác định một khoảng thời gian rảnh từ <code>freeStart</code> đến <code>freeEnd</code>, bao gồm cả hai đầu mút.</p>

<p>Nhiệm vụ của bạn là gộp <strong>tất cả</strong> các khoảng bận chồng lấn hoặc tiếp giáp, sau đó xóa <strong>tất cả</strong> các điểm nguyên trong khoảng rảnh khỏi những khoảng bận đã gộp.</p>

<p>Hai khoảng được gọi là tiếp giáp nếu khoảng thứ hai bắt đầu <strong>ngay sau</strong> khi khoảng thứ nhất kết thúc. Ví dụ, <code>[1, 1]</code> và <code>[2, 2]</code> tiếp giáp nhau và phải được gộp thành <code>[1, 2]</code>.</p>

<p>Trả về các khoảng bận <strong>còn lại</strong> theo thứ tự <strong>tăng dần</strong>. Các khoảng trả về phải <strong>không chồng lấn</strong> và chứa số lượng khoảng <strong>ít nhất</strong> có thể. Nếu không còn điểm bận nào, trả về một danh sách rỗng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">occupiedIntervals = [[2,6],[4,8],[10,10],[10,12],[14,16]], freeStart = 7, freeEnd = 11</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[2,6],[12,12],[14,16]]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Sau khi gộp, các khoảng bận là <code>[2, 8]</code>, <code>[10, 12]</code> và <code>[14, 16]</code>.</li>
	<li>Loại bỏ khoảng rảnh <code>[7, 11]</code> cho kết quả <code>[2, 6]</code>, <code>[12, 12]</code> và <code>[14, 16]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">occupiedIntervals = [[1,5],[2,3]], freeStart = 3, freeEnd = 8</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[1,2]]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Sau khi gộp, khoảng bận là <code>[1, 5]</code>.</li>
	<li>Loại bỏ khoảng rảnh <code>[3, 8]</code> cho kết quả <code>[1, 2]</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= occupiedIntervals.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>occupiedIntervals[i].length == 2</code></li>
	<li><code>1 &lt;= start<sub>i</sub> &lt;= end<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= freeStart &lt;= freeEnd &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Gộp khoảng

<!-- thinking:start -->

> **Tư duy**
>
> Sắp xếp các khoảng bận theo đầu trái rồi gộp những khoảng chồng lấn thành các đoạn bận rời nhau. Lấy giao của các đoạn này với $[\textit{freeStart},\textit{freeEnd}]$: loại bỏ các đoạn nằm ngoài cửa sổ và cắt các đoạn đi qua cửa sổ.
>
> Việc gộp giúp lượt cắt sau đó chạy trong thời gian tuyến tính.

<!-- thinking:end -->

Trước hết, ta sắp xếp tất cả các khoảng bận theo đầu trái, sau đó duyệt qua các khoảng. Nếu đầu trái của khoảng hiện tại lớn hơn đầu phải của khoảng cuối cùng cộng $1$, ta thêm khoảng hiện tại vào kết quả. Ngược lại, ta gộp khoảng hiện tại với khoảng cuối cùng và cập nhật đầu phải của khoảng cuối cùng bằng giá trị lớn hơn giữa hai đầu phải.

Tiếp theo, ta duyệt qua các khoảng bận. Nếu đầu phải của khoảng hiện tại nhỏ hơn đầu trái của khoảng rảnh hoặc đầu trái của khoảng hiện tại lớn hơn đầu phải của khoảng rảnh, ta thêm khoảng hiện tại vào kết quả. Nếu không, ta kiểm tra xem đầu trái của khoảng hiện tại có nhỏ hơn đầu trái của khoảng rảnh hay không. Nếu có, ta cập nhật đầu trái của khoảng hiện tại thành đầu trái của khoảng rảnh trừ $1$, rồi thêm nó vào kết quả. Sau đó, ta kiểm tra xem đầu phải của khoảng hiện tại có lớn hơn đầu phải của khoảng rảnh hay không. Nếu có, ta cập nhật đầu phải của khoảng hiện tại thành đầu phải của khoảng rảnh cộng $1$, rồi thêm nó vào kết quả.

Cuối cùng, ta trả về kết quả.

Độ phức tạp thời gian là $O(n \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{occupiedIntervals}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def filterOccupiedIntervals(
        self, occupiedIntervals: List[List[int]], freeStart: int, freeEnd: int
    ) -> List[List[int]]:
        occupiedIntervals.sort(key=lambda x: x[0])
        busy = [occupiedIntervals[0]]
        for interval in occupiedIntervals[1:]:
            if busy[-1][1] + 1 < interval[0]:
                busy.append(interval)
            else:
                busy[-1][1] = max(busy[-1][1], interval[1])
        ans = []
        for interval in busy:
            if interval[1] < freeStart or freeEnd < interval[0]:
                ans.append(interval)
            else:
                if interval[0] < freeStart:
                    ans.append([interval[0], freeStart - 1])
                if interval[1] > freeEnd:
                    ans.append([freeEnd + 1, interval[1]])
        return ans
```

#### Java

```java
class Solution {
    public List<List<Integer>> filterOccupiedIntervals(
        int[][] occupiedIntervals, int freeStart, int freeEnd) {
        Arrays.sort(occupiedIntervals, (a, b) -> a[0] - b[0]);

        List<int[]> busy = new ArrayList<>();
        busy.add(occupiedIntervals[0]);

        for (int i = 1; i < occupiedIntervals.length; i++) {
            int[] cur = occupiedIntervals[i];
            int[] last = busy.get(busy.size() - 1);

            if (last[1] + 1 < cur[0]) {
                busy.add(cur);
            } else {
                last[1] = Math.max(last[1], cur[1]);
            }
        }

        List<List<Integer>> ans = new ArrayList<>();

        for (int[] interval : busy) {
            int s = interval[0], e = interval[1];

            if (e < freeStart || s > freeEnd) {
                ans.add(Arrays.asList(s, e));
            } else {
                if (s < freeStart) {
                    ans.add(Arrays.asList(s, freeStart - 1));
                }
                if (e > freeEnd) {
                    ans.add(Arrays.asList(freeEnd + 1, e));
                }
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
    vector<vector<int>> filterOccupiedIntervals(vector<vector<int>>& occupiedIntervals, int freeStart, int freeEnd) {
        sort(occupiedIntervals.begin(), occupiedIntervals.end());

        vector<vector<int>> busy;
        busy.push_back(occupiedIntervals[0]);

        for (int i = 1; i < occupiedIntervals.size(); i++) {
            auto& cur = occupiedIntervals[i];
            auto& last = busy.back();

            if (last[1] + 1 < cur[0]) {
                busy.push_back(cur);
            } else {
                last[1] = max(last[1], cur[1]);
            }
        }

        vector<vector<int>> ans;

        for (auto& it : busy) {
            int s = it[0], e = it[1];

            if (e < freeStart || s > freeEnd) {
                ans.push_back({s, e});
            } else {
                if (s < freeStart) {
                    ans.push_back({s, freeStart - 1});
                }
                if (e > freeEnd) {
                    ans.push_back({freeEnd + 1, e});
                }
            }
        }

        return ans;
    }
};
```

#### Go

```go
func filterOccupiedIntervals(occupiedIntervals [][]int, freeStart int, freeEnd int) [][]int {
    sort.Slice(occupiedIntervals, func(i, j int) bool {
        return occupiedIntervals[i][0] < occupiedIntervals[j][0]
    })

    busy := [][]int{occupiedIntervals[0]}

    for i := 1; i < len(occupiedIntervals); i++ {
        cur := occupiedIntervals[i]
        last := &busy[len(busy)-1]

        if (*last)[1]+1 < cur[0] {
            busy = append(busy, cur)
        } else {
            if cur[1] > (*last)[1] {
                (*last)[1] = cur[1]
            }
        }
    }

    ans := [][]int{}

    for _, it := range busy {
        s, e := it[0], it[1]

        if e < freeStart || s > freeEnd {
            ans = append(ans, []int{s, e})
        } else {
            if s < freeStart {
                ans = append(ans, []int{s, freeStart - 1})
            }
            if e > freeEnd {
                ans = append(ans, []int{freeEnd + 1, e})
            }
        }
    }

    return ans
}
```

#### TypeScript

```ts
function filterOccupiedIntervals(
    occupiedIntervals: number[][],
    freeStart: number,
    freeEnd: number,
): number[][] {
    occupiedIntervals.sort((a, b) => a[0] - b[0]);

    const busy: number[][] = [occupiedIntervals[0]];

    for (let i = 1; i < occupiedIntervals.length; i++) {
        const cur = occupiedIntervals[i];
        const last = busy[busy.length - 1];

        if (last[1] + 1 < cur[0]) {
            busy.push(cur);
        } else {
            last[1] = Math.max(last[1], cur[1]);
        }
    }

    const ans: number[][] = [];

    for (const [s, e] of busy) {
        if (e < freeStart || s > freeEnd) {
            ans.push([s, e]);
        } else {
            if (s < freeStart) {
                ans.push([s, freeStart - 1]);
            }
            if (e > freeEnd) {
                ans.push([freeEnd + 1, e]);
            }
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
