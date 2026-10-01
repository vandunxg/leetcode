---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Two Pointers
    - Prefix Sum
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [253. Meeting Rooms II 🔒](https://leetcode.com/problems/meeting-rooms-ii)

[中文文档](/solution/0200-0299/0253.Meeting%20Rooms%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng các khoảng thời gian họp <code>intervals</code>, trong đó <code>intervals[i] = [start<sub>i</sub>, end<sub>i</sub>]</code>, hãy trả về <em>số phòng họp tối thiểu cần dùng</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> intervals = [[0,30],[5,10],[15,20]]
<strong>Đầu ra:</strong> 2
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> intervals = [[7,10],[2,4]]
<strong>Đầu ra:</strong> 1
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;=&nbsp;intervals.length &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= start<sub>i</sub> &lt; end<sub>i</sub> &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Difference Array

<!-- thinking:start -->

> **Tư duy**
>
> Số phòng cần dùng bằng số cuộc họp đang diễn ra nhiều nhất tại cùng một thời điểm. Mỗi thời điểm bắt đầu cộng $1$, mỗi thời điểm kết thúc trừ $1$; prefix sum cho biết số cuộc họp đang diễn ra.
>
> Tạo difference array đến thời điểm kết thúc muộn nhất rồi duyệt một lượt để tìm giá trị lớn nhất đó.

<!-- thinking:end -->

Ta có thể giải bài này bằng difference array.

Trước tiên, ta tìm thời điểm kết thúc lớn nhất trong tất cả cuộc họp, ký hiệu là $m$. Sau đó, tạo difference array $d$ có độ dài $m + 1$. Với mỗi cuộc họp, ta cập nhật các vị trí tương ứng trong mảng: cộng $1$ tại thời điểm bắt đầu $d[l] = d[l] + 1$ và trừ $1$ tại thời điểm kết thúc $d[r] = d[r] - 1$.

Tiếp theo, ta tính prefix sum của difference array và tìm giá trị lớn nhất. Giá trị này chính là số phòng họp tối thiểu cần dùng.

Độ phức tạp thời gian là $O(n + m)$ và độ phức tạp không gian là $O(m)$, trong đó $n$ là số cuộc họp và $m$ là thời điểm kết thúc lớn nhất.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minMeetingRooms(self, intervals: List[List[int]]) -> int:
        m = max(e[1] for e in intervals)
        d = [0] * (m + 1)
        for l, r in intervals:
            d[l] += 1
            d[r] -= 1
        ans = s = 0
        for v in d:
            s += v
            ans = max(ans, s)
        return ans
```

#### Java

```java
class Solution {
    public int minMeetingRooms(int[][] intervals) {
        int m = 0;
        for (var e : intervals) {
            m = Math.max(m, e[1]);
        }
        int[] d = new int[m + 1];
        for (var e : intervals) {
            ++d[e[0]];
            --d[e[1]];
        }
        int ans = 0, s = 0;
        for (int v : d) {
            s += v;
            ans = Math.max(ans, s);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minMeetingRooms(vector<vector<int>>& intervals) {
        int m = 0;
        for (const auto& e : intervals) {
            m = max(m, e[1]);
        }
        vector<int> d(m + 1);
        for (const auto& e : intervals) {
            d[e[0]]++;
            d[e[1]]--;
        }
        int ans = 0, s = 0;
        for (int v : d) {
            s += v;
            ans = max(ans, s);
        }
        return ans;
    }
};
```

#### Go

```go
func minMeetingRooms(intervals [][]int) (ans int) {
	m := 0
	for _, e := range intervals {
		m = max(m, e[1])
	}

	d := make([]int, m+1)
	for _, e := range intervals {
		d[e[0]]++
		d[e[1]]--
	}

	s := 0
	for _, v := range d {
		s += v
		ans = max(ans, s)
	}
	return
}
```

#### TypeScript

```ts
function minMeetingRooms(intervals: number[][]): number {
    const m = Math.max(...intervals.map(([_, r]) => r));
    const d: number[] = Array(m + 1).fill(0);
    for (const [l, r] of intervals) {
        d[l]++;
        d[r]--;
    }
    let [ans, s] = [0, 0];
    for (const v of d) {
        s += v;
        ans = Math.max(ans, s);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_meeting_rooms(intervals: Vec<Vec<i32>>) -> i32 {
        let mut m = 0;
        for e in &intervals {
            m = m.max(e[1]);
        }

        let mut d = vec![0; (m + 1) as usize];
        for e in intervals {
            d[e[0] as usize] += 1;
            d[e[1] as usize] -= 1;
        }

        let mut ans = 0;
        let mut s = 0;
        for v in d {
            s += v;
            ans = ans.max(s);
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Difference (Hash Map)

<!-- thinking:start -->

> **Tư duy**
>
> Nếu các cuộc họp trải dài trên một khoảng thời gian lớn, mảng kích thước $O(m)$ sẽ tốn bộ nhớ không cần thiết. Hash map chỉ lưu các cập nhật tại thời điểm bắt đầu và kết thúc; sắp xếp các key rồi tìm đỉnh của prefix sum sẽ cho kết quả tương đương.

<!-- thinking:end -->

Nếu thời gian các cuộc họp trải dài trên một khoảng lớn, ta có thể dùng hash map thay cho difference array.

Trước tiên, ta tạo hash map $d$ và cập nhật các key tương ứng với thời điểm bắt đầu và kết thúc của mỗi cuộc họp: cộng $1$ tại thời điểm bắt đầu $d[l] = d[l] + 1$, và trừ $1$ tại thời điểm kết thúc $d[r] = d[r] - 1$.

Sau đó, ta sắp xếp các key của hash map, tính prefix sum và tìm giá trị lớn nhất. Giá trị này chính là số phòng họp tối thiểu cần dùng.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số cuộc họp.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minMeetingRooms(self, intervals: List[List[int]]) -> int:
        d = defaultdict(int)
        for l, r in intervals:
            d[l] += 1
            d[r] -= 1
        ans = s = 0
        for _, v in sorted(d.items()):
            s += v
            ans = max(ans, s)
        return ans
```

#### Java

```java
class Solution {
    public int minMeetingRooms(int[][] intervals) {
        Map<Integer, Integer> d = new TreeMap<>();
        for (var e : intervals) {
            d.merge(e[0], 1, Integer::sum);
            d.merge(e[1], -1, Integer::sum);
        }
        int ans = 0, s = 0;
        for (var e : d.values()) {
            s += e;
            ans = Math.max(ans, s);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minMeetingRooms(vector<vector<int>>& intervals) {
        map<int, int> d;
        for (const auto& e : intervals) {
            d[e[0]]++;
            d[e[1]]--;
        }
        int ans = 0, s = 0;
        for (auto& [_, v] : d) {
            s += v;
            ans = max(ans, s);
        }
        return ans;
    }
};
```

#### Go

```go
func minMeetingRooms(intervals [][]int) (ans int) {
	d := make(map[int]int)
	for _, e := range intervals {
		d[e[0]]++
		d[e[1]]--
	}

	keys := make([]int, 0, len(d))
	for k := range d {
		keys = append(keys, k)
	}
	sort.Ints(keys)

	s := 0
	for _, k := range keys {
		s += d[k]
		ans = max(ans, s)
	}
	return
}
```

#### TypeScript

```ts
function minMeetingRooms(intervals: number[][]): number {
    const d: { [key: number]: number } = {};
    for (const [l, r] of intervals) {
        d[l] = (d[l] || 0) + 1;
        d[r] = (d[r] || 0) - 1;
    }

    let [ans, s] = [0, 0];
    const keys = Object.keys(d)
        .map(Number)
        .sort((a, b) => a - b);
    for (const k of keys) {
        s += d[k];
        ans = Math.max(ans, s);
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn min_meeting_rooms(intervals: Vec<Vec<i32>>) -> i32 {
        let mut d: HashMap<i32, i32> = HashMap::new();
        for interval in intervals {
            let (l, r) = (interval[0], interval[1]);
            *d.entry(l).or_insert(0) += 1;
            *d.entry(r).or_insert(0) -= 1;
        }

        let mut times: Vec<i32> = d.keys().cloned().collect();
        times.sort();

        let mut ans = 0;
        let mut s = 0;
        for time in times {
            s += d[&time];
            ans = ans.max(s);
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
