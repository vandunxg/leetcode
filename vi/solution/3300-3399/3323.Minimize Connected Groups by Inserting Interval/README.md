---
comments: true
difficulty: Medium
tags:
    - Array
    - Binary Search
    - Sorting
    - Sliding Window
---

<!-- problem:start -->

# [3323. Minimize Connected Groups by Inserting Interval 🔒](https://leetcode.com/problems/minimize-connected-groups-by-inserting-interval)

[中文文档](/solution/3300-3399/3323.Minimize%20Connected%20Groups%20by%20Inserting%20Interval/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng 2 chiều <code>intervals</code>, trong đó <code>intervals[i] = [start<sub>i</sub>, end<sub>i</sub>]</code> biểu diễn điểm bắt đầu và điểm kết thúc của interval <code>i</code>. Bạn cũng được cho một số nguyên <code>k</code>.</p>

<p>Bạn phải thêm <strong>chính xác một</strong> interval mới <code>[start<sub>new</sub>, end<sub>new</sub>]</code> vào mảng sao cho:</p>

<ul>
    <li>Độ dài của interval mới, <code>end<sub>new</sub> - start<sub>new</sub></code>, không vượt quá <code>k</code>.</li>
    <li>Sau khi thêm, số lượng <strong>nhóm liên thông</strong> trong <code>intervals</code> được <strong>tối thiểu hóa</strong>.</li>
</ul>

<p>Một <strong>nhóm liên thông</strong> gồm các interval là một tập hợp cực đại các interval mà khi xét cùng nhau, chúng phủ một đoạn liên tục từ điểm nhỏ nhất đến điểm lớn nhất mà không có khoảng trống. Dưới đây là một số ví dụ:</p>

<ul>
    <li>Một nhóm các interval <code>[[1, 2], [2, 5], [3, 3]]</code> là liên thông vì chúng cùng nhau phủ đoạn từ 1 đến 5 mà không có khoảng trống.</li>
    <li>Tuy nhiên, một nhóm các interval <code>[[1, 2], [3, 4]]</code> không liên thông vì đoạn <code>(2, 3)</code> không được phủ.</li>
</ul>

<p>Trả về <strong>số lượng nhỏ nhất</strong> các nhóm liên thông sau khi thêm <strong>chính xác một</strong> interval mới vào mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">intervals = [[1,3],[5,6],[8,10]], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Sau khi thêm interval <code>[3, 5]</code>, ta có hai nhóm liên thông: <code>[[1, 3], [3, 5], [5, 6]]</code> và <code>[[8, 10]]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">intervals = [[5,10],[1,1],[3,3]], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Sau khi thêm interval <code>[1, 1]</code>, ta có ba nhóm liên thông: <code>[[1, 1], [1, 1]]</code>, <code>[[3, 3]]</code> và <code>[[5, 10]]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= intervals.length &lt;= 10<sup>5</sup></code></li>
    <li><code>intervals[i] == [start<sub>i</sub>, end<sub>i</sub>]</code></li>
    <li><code>1 &lt;= start<sub>i</sub> &lt;= end<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
    <li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể chèn một interval có độ dài không vượt quá $k$ để tối thiểu hóa số lượng nhóm liên thông. Vì $n \le 10^5$, trước tiên ta gộp các interval giao nhau, sau đó tìm xem một đoạn nối có thể ghép bao nhiêu phần đã gộp.
>
> Sau khi gộp, các phần không giao nhau. Từ điểm cuối $e$ của phần $i$, mọi phần có điểm đầu nhỏ hơn $e+k+1$ đều có thể được đoạn nối đó phủ.
>
> Tìm kiếm nhị phân cho biết chỉ số đầu tiên $j$ nằm ngoài đoạn nối; số lượng mới là $|\textit{merged}|-(j-i-1)$, và ta lấy giá trị nhỏ nhất.

<!-- thinking:end -->

Trước hết, ta sắp xếp tập $\textit{intervals}$ đã cho theo điểm đầu, sau đó gộp tất cả các interval giao nhau để thu được một tập interval mới là $\textit{merged}$.

Tiếp theo, ta có thể đặt đáp án ban đầu bằng độ dài của $\textit{merged}$.

Sau đó, ta duyệt qua từng interval $[\_, e]$ trong $\textit{merged}$. Sử dụng tìm kiếm nhị phân, ta tìm interval đầu tiên trong $\textit{merged}$ có điểm đầu lớn hơn hoặc bằng $e + k + 1$, và gọi chỉ số của nó là $j$. Khi đó, ta có thể cập nhật đáp án theo công thức $\textit{ans} = \min(\textit{ans}, |\textit{merged}| - (j - i - 1))$.

Cuối cùng, ta trả về đáp án $\textit{ans}$.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là số lượng interval.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minConnectedGroups(self, intervals: List[List[int]], k: int) -> int:
        intervals.sort()
        merged = [intervals[0]]
        for s, e in intervals[1:]:
            if merged[-1][1] < s:
                merged.append([s, e])
            else:
                merged[-1][1] = max(merged[-1][1], e)
        ans = len(merged)
        for i, (_, e) in enumerate(merged):
            j = bisect_left(merged, [e + k + 1, 0])
            ans = min(ans, len(merged) - (j - i - 1))
        return ans
```

#### Java

```java
class Solution {
    public int minConnectedGroups(int[][] intervals, int k) {
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
        List<int[]> merged = new ArrayList<>();
        merged.add(intervals[0]);
        for (int i = 1; i < intervals.length; i++) {
            int[] interval = intervals[i];
            int[] last = merged.get(merged.size() - 1);
            if (last[1] < interval[0]) {
                merged.add(interval);
            } else {
                last[1] = Math.max(last[1], interval[1]);
            }
        }

        int ans = merged.size();
        for (int i = 0; i < merged.size(); i++) {
            int[] interval = merged.get(i);
            int j = binarySearch(merged, interval[1] + k + 1);
            ans = Math.min(ans, merged.size() - (j - i - 1));
        }

        return ans;
    }

    private int binarySearch(List<int[]> nums, int x) {
        int l = 0, r = nums.size();
        while (l < r) {
            int mid = (l + r) >> 1;
            if (nums.get(mid)[0] >= x) {
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
    int minConnectedGroups(vector<vector<int>>& intervals, int k) {
        sort(intervals.begin(), intervals.end());
        vector<vector<int>> merged;
        for (const auto& interval : intervals) {
            int s = interval[0], e = interval[1];
            if (merged.empty() || merged.back()[1] < s) {
                merged.emplace_back(interval);
            } else {
                merged.back()[1] = max(merged.back()[1], e);
            }
        }
        int ans = merged.size();
        for (int i = 0; i < merged.size(); ++i) {
            auto& interval = merged[i];
            int j = lower_bound(merged.begin(), merged.end(), vector<int>{interval[1] + k + 1, 0}) - merged.begin();
            ans = min(ans, (int) merged.size() - (j - i - 1));
        }
        return ans;
    }
};
```

#### Go

```go
func minConnectedGroups(intervals [][]int, k int) int {
    sort.Slice(intervals, func(i, j int) bool { return intervals[i][0] < intervals[j][0] })
    merged := [][]int{}
    for _, interval := range intervals {
        s, e := interval[0], interval[1]
        if len(merged) == 0 || merged[len(merged)-1][1] < s {
            merged = append(merged, interval)
        } else {
            merged[len(merged)-1][1] = max(merged[len(merged)-1][1], e)
        }
    }
    ans := len(merged)
    for i, interval := range merged {
        j := sort.Search(len(merged), func(j int) bool { return merged[j][0] >= interval[1]+k+1 })
        ans = min(ans, len(merged)-(j-i-1))
    }
    return ans
}
```

#### TypeScript

```ts
function minConnectedGroups(intervals: number[][], k: number): number {
    intervals.sort((a, b) => a[0] - b[0]);
    const merged: number[][] = [];
    for (const interval of intervals) {
        const [s, e] = interval;
        if (merged.length === 0 || merged.at(-1)![1] < s) {
            merged.push(interval);
        } else {
            merged.at(-1)![1] = Math.max(merged.at(-1)![1], e);
        }
    }
    const search = (x: number): number => {
        let [l, r] = [0, merged.length];
        while (l < r) {
            const mid = (l + r) >> 1;
            if (merged[mid][0] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    };
    let ans = merged.length;
    for (let i = 0; i < merged.length; ++i) {
        const j = search(merged[i][1] + k + 1);
        ans = Math.min(ans, merged.length - (j - i - 1));
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
