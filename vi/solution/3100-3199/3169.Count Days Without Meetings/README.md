---
comments: true
difficulty: Medium
rating: 1483
source: Weekly Contest 400 Q2
tags:
    - Array
    - Sorting
---

<!-- problem:start -->

# [3169. Count Days Without Meetings](https://leetcode.com/problems/count-days-without-meetings)

[中文文档](/solution/3100-3199/3169.Count%20Days%20Without%20Meetings/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên dương <code>days</code>, biểu thị tổng số ngày một nhân viên có thể làm việc (bắt đầu từ ngày 1). Bạn cũng được cho một mảng 2 chiều <code>meetings</code> kích thước <code>n</code>, trong đó <code>meetings[i] = [start_i, end_i]</code> biểu thị ngày bắt đầu và ngày kết thúc của cuộc họp thứ <code>i</code> (bao gồm cả hai đầu).</p>

<p>Hãy trả về số ngày nhân viên có thể làm việc nhưng không có cuộc họp nào được lên lịch.</p>

<p><strong>Lưu ý: </strong>Các cuộc họp có thể chồng lấp lên nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">days = 10, meetings = [[5,7],[1,3],[9,10]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có cuộc họp nào được lên lịch vào ngày thứ 4<sup>th</sup> và ngày thứ 8<sup>th</sup>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">days = 5, meetings = [[2,4],[1,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có cuộc họp nào được lên lịch vào ngày thứ 5<sup>th </sup>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">days = 6, meetings = [[1,6]]</span></p>

<p><strong>Đầu ra:</strong> 0</p>

<p><strong>Giải thích:</strong></p>

<p>Các cuộc họp được lên lịch trong tất cả các ngày làm việc.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= days &lt;= 10<sup>9</sup></code></li>
    <li><code>1 &lt;= meetings.length &lt;= 10<sup>5</sup></code></li>
    <li><code>meetings[i].length == 2</code></li>
    <li><code><font face="monospace">1 &lt;= meetings[i][0] &lt;= meetings[i][1] &lt;= days</font></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các ngày trong $[1,days]$ không nằm trong bất kỳ cuộc họp nào. Đánh dấu từng ngày sẽ không phù hợp khi $days$ rất lớn và các cuộc họp chồng lấp.
>
> Sắp xếp theo thời điểm bắt đầu rồi gộp các khoảng, để khoảng trống giữa điểm kết thúc hiện tại $last$ và thời điểm bắt đầu tiếp theo là các ngày rảnh.
>
> Khi $last<st$, cộng $st-last-1$, sau đó đặt $last=\max(last,ed)$. Sau cuộc họp cuối cùng, cộng $days-last$.

<!-- thinking:end -->

Ta có thể sắp xếp tất cả các cuộc họp theo thời điểm bắt đầu và dùng biến `last` để ghi nhận thời điểm kết thúc muộn nhất của các cuộc họp trước đó.

Tiếp theo, ta duyệt qua tất cả các cuộc họp. Với mỗi cuộc họp $(st, ed)$, nếu `last < st`, điều đó có nghĩa là khoảng thời gian từ `last` đến `st` là khoảng thời gian nhân viên có thể làm việc mà không có cuộc họp nào được lên lịch. Ta cộng khoảng thời gian này vào đáp án. Sau đó cập nhật `last = max(last, ed)`.

Cuối cùng, nếu `last < days`, điều đó có nghĩa là sau khi cuộc họp cuối cùng kết thúc vẫn còn một khoảng thời gian nhân viên có thể làm việc mà không có cuộc họp nào được lên lịch. Ta cộng khoảng thời gian này vào đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(\log n)$. Trong đó $n$ là số cuộc họp.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countDays(self, days: int, meetings: List[List[int]]) -> int:
        meetings.sort()
        ans = last = 0
        for st, ed in meetings:
            if last < st:
                ans += st - last - 1
            last = max(last, ed)
        ans += days - last
        return ans
```

#### Java

```java
class Solution {
    public int countDays(int days, int[][] meetings) {
        Arrays.sort(meetings, (a, b) -> a[0] - b[0]);
        int ans = 0, last = 0;
        for (var e : meetings) {
            int st = e[0], ed = e[1];
            if (last < st) {
                ans += st - last - 1;
            }
            last = Math.max(last, ed);
        }
        ans += days - last;
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countDays(int days, vector<vector<int>>& meetings) {
        sort(meetings.begin(), meetings.end());
        int ans = 0, last = 0;
        for (auto& e : meetings) {
            int st = e[0], ed = e[1];
            if (last < st) {
                ans += st - last - 1;
            }
            last = max(last, ed);
        }
        ans += days - last;
        return ans;
    }
};
```

#### Go

```go
func countDays(days int, meetings [][]int) (ans int) {
    sort.Slice(meetings, func(i, j int) bool { return meetings[i][0] < meetings[j][0] })
    last := 0
    for _, e := range meetings {
        st, ed := e[0], e[1]
        if last < st {
            ans += st - last - 1
        }
        last = max(last, ed)
    }
    ans += days - last
    return
}
```

#### TypeScript

```ts
function countDays(days: number, meetings: number[][]): number {
    meetings.sort((a, b) => a[0] - b[0]);
    let [ans, last] = [0, 0];
    for (const [st, ed] of meetings) {
        if (last < st) {
            ans += st - last - 1;
        }
        last = Math.max(last, ed);
    }
    ans += days - last;
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_days(days: i32, mut meetings: Vec<Vec<i32>>) -> i32 {
        meetings.sort_by_key(|m| m[0]);
        let mut ans = 0;
        let mut last = 0;

        for e in meetings {
            let st = e[0];
            let ed = e[1];
            if last < st {
                ans += st - last - 1;
            }
            last = last.max(ed);
        }

        ans + (days - last)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
