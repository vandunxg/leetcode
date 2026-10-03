---
comments: true
difficulty: Hard
rating: 2022
source: Weekly Contest 290 Q4
tags:
    - Array
    - Hash Table
    - Binary Search
    - Ordered Set
    - Prefix Sum
    - Sorting
---

<!-- problem:start -->

# [2251. Number of Flowers in Full Bloom](https://leetcode.com/problems/number-of-flowers-in-full-bloom)

[中文文档](/solution/2200-2299/2251.Number%20of%20Flowers%20in%20Full%20Bloom/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một mảng số nguyên 2 chiều <strong>đánh chỉ số từ 0</strong> <code>flowers</code>, trong đó <code>flowers[i] = [start<sub>i</sub>, end<sub>i</sub>]</code> nghĩa là bông hoa thứ <code>i<sup>th</sup></code> sẽ <strong>nở rộ</strong> từ <code>start<sub>i</sub></code> đến <code>end<sub>i</sub></code> (<strong>bao gồm cả hai đầu mút</strong>). Bạn cũng được cung cấp một mảng số nguyên <strong>đánh chỉ số từ 0</strong> <code>people</code> có kích thước <code>n</code>, trong đó <code>people[i]</code> là thời điểm người thứ <code>i<sup>th</sup></code> đến ngắm hoa.</p>

<p>Trả về <em>một mảng số nguyên </em><code>answer</code><em> có kích thước </em><code>n</code><em>, trong đó </em><code>answer[i]</code><em> là <strong>số lượng</strong> hoa đang nở rộ khi người thứ </em><code>i<sup>th</sup></code><em> đến.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2251.Number%20of%20Flowers%20in%20Full%20Bloom/images/ex1new.jpg" style="width: 550px; height: 216px;" />
<pre>
<strong>Đầu vào:</strong> flowers = [[1,6],[3,7],[9,12],[4,13]], people = [2,3,7,11]
<strong>Đầu ra:</strong> [1,2,2,2]
<strong>Giải thích: </strong>Hình trên minh họa các thời điểm hoa đang nở rộ và thời điểm mọi người đến.
Với mỗi người, ta trả về số lượng hoa đang nở rộ khi họ đến.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2251.Number%20of%20Flowers%20in%20Full%20Bloom/images/ex2new.jpg" style="width: 450px; height: 195px;" />
<pre>
<strong>Đầu vào:</strong> flowers = [[1,10],[3,3]], people = [3,3,2]
<strong>Đầu ra:</strong> [2,2,1]
<strong>Giải thích:</strong> Hình trên minh họa các thời điểm hoa đang nở rộ và thời điểm mọi người đến.
Với mỗi người, ta trả về số lượng hoa đang nở rộ khi họ đến.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= flowers.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>flowers[i].length == 2</code></li>
	<li><code>1 &lt;= start<sub>i</sub> &lt;= end<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= people.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= people[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi thời điểm đến, ta đếm số hoa vẫn đang nở. Cả hai mảng có thể chứa $5\times 10^4$ phần tử và thời điểm có thể lên tới $10^9$. Một bông hoa đang nở tại $t$ khi và chỉ khi $\textit{start} \le t \le \textit{end}$, tức là số hoa đã bắt đầu nở trừ số hoa đã tàn.
>
> Ta sắp xếp tất cả thời điểm bắt đầu và kết thúc. Với thời điểm đến $p$, dùng $\textit{bisect\_right}$ trên các thời điểm bắt đầu trừ $\textit{bisect\_left}$ trên các thời điểm kết thúc để tính số lượng.

<!-- thinking:end -->

Ta sắp xếp các bông hoa theo thời điểm bắt đầu và kết thúc. Sau đó, với mỗi người, ta có thể dùng tìm kiếm nhị phân để tìm số hoa đang nở khi họ đến. Cụ thể, ta lấy số hoa đã bắt đầu nở trước hoặc đúng thời điểm mỗi người đến, rồi trừ đi số hoa đã tàn trước thời điểm đó để thu được đáp án.

Độ phức tạp thời gian là $O((m + n) \times \log n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ và $m$ lần lượt là độ dài của các mảng $\textit{flowers}$ và $\textit{people}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def fullBloomFlowers(
        self, flowers: List[List[int]], people: List[int]
    ) -> List[int]:
        start, end = sorted(a for a, _ in flowers), sorted(b for _, b in flowers)
        return [bisect_right(start, p) - bisect_left(end, p) for p in people]
```

#### Java

```java
class Solution {
    public int[] fullBloomFlowers(int[][] flowers, int[] people) {
        int n = flowers.length;
        int[] start = new int[n];
        int[] end = new int[n];
        for (int i = 0; i < n; ++i) {
            start[i] = flowers[i][0];
            end[i] = flowers[i][1];
        }
        Arrays.sort(start);
        Arrays.sort(end);
        int m = people.length;
        int[] ans = new int[m];
        for (int i = 0; i < m; ++i) {
            ans[i] = search(start, people[i] + 1) - search(end, people[i]);
        }
        return ans;
    }

    private int search(int[] nums, int x) {
        int l = 0, r = nums.length;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (nums[mid] >= x) {
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
    vector<int> fullBloomFlowers(vector<vector<int>>& flowers, vector<int>& people) {
        int n = flowers.size();
        vector<int> start;
        vector<int> end;
        for (auto& f : flowers) {
            start.push_back(f[0]);
            end.push_back(f[1]);
        }
        sort(start.begin(), start.end());
        sort(end.begin(), end.end());
        vector<int> ans;
        for (auto& p : people) {
            auto r = upper_bound(start.begin(), start.end(), p) - start.begin();
            auto l = lower_bound(end.begin(), end.end(), p) - end.begin();
            ans.push_back(r - l);
        }
        return ans;
    }
};
```

#### Go

```go
func fullBloomFlowers(flowers [][]int, people []int) (ans []int) {
	n := len(flowers)
	start := make([]int, n)
	end := make([]int, n)
	for i, f := range flowers {
		start[i] = f[0]
		end[i] = f[1]
	}
	sort.Ints(start)
	sort.Ints(end)
	for _, p := range people {
		r := sort.SearchInts(start, p+1)
		l := sort.SearchInts(end, p)
		ans = append(ans, r-l)
	}
	return
}
```

#### TypeScript

```ts
function fullBloomFlowers(flowers: number[][], people: number[]): number[] {
    const n = flowers.length;
    const start = new Array(n).fill(0);
    const end = new Array(n).fill(0);
    for (let i = 0; i < n; ++i) {
        start[i] = flowers[i][0];
        end[i] = flowers[i][1];
    }
    start.sort((a, b) => a - b);
    end.sort((a, b) => a - b);
    const ans: number[] = [];
    for (const p of people) {
        const r = search(start, p + 1);
        const l = search(end, p);
        ans.push(r - l);
    }
    return ans;
}

function search(nums: number[], x: number): number {
    let l = 0;
    let r = nums.length;
    while (l < r) {
        const mid = (l + r) >> 1;
        if (nums[mid] >= x) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return l;
}
```

#### Rust

```rust
use std::collections::BTreeMap;

impl Solution {
    #[allow(dead_code)]
    pub fn full_bloom_flowers(flowers: Vec<Vec<i32>>, people: Vec<i32>) -> Vec<i32> {
        let n = people.len();

        // First sort the people vector based on the first item
        let mut people: Vec<(usize, i32)> = people.into_iter().enumerate().map(|x| x).collect();

        people.sort_by(|lhs, rhs| lhs.1.cmp(&rhs.1));

        // Initialize the difference vector
        let mut diff = BTreeMap::new();
        let mut ret = vec![0; n];

        for f in flowers {
            let (left, right) = (f[0], f[1]);
            diff.entry(left)
                .and_modify(|x| {
                    *x += 1;
                })
                .or_insert(1);

            diff.entry(right + 1)
                .and_modify(|x| {
                    *x -= 1;
                })
                .or_insert(-1);
        }

        let mut sum = 0;
        let mut i = 0;
        for (k, v) in diff {
            while i < n && people[i].1 < k {
                ret[people[i].0] += sum;
                i += 1;
            }
            sum += v;
        }

        ret
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Mảng hiệu + Sắp xếp + Truy vấn offline

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 thực hiện tìm kiếm nhị phân riêng cho từng truy vấn. Các kết quả đếm này có thể được biểu diễn bằng mảng hiệu: cộng một tại $\textit{start}$ và trừ một tại $\textit{end}+1$. Ta quét các thời điểm sự kiện cùng với các thời điểm đến đã được sắp xếp.
>
> Một con trỏ tích lũy mọi sự kiện có thời điểm $\le t$; đó chính là số hoa đang nở đối với người đến tại thời điểm đó. Độ phức tạp tiệm cận tương đương, nhưng không cần thực hiện hai lần tìm kiếm nhị phân cho mỗi truy vấn.

<!-- thinking:end -->

Ta có thể dùng mảng hiệu để duy trì số lượng hoa tại mỗi thời điểm. Tiếp theo, ta sắp xếp $people$ theo thời điểm đến tăng dần. Khi mỗi người đến, ta thực hiện phép tính tổng tiền tố trên mảng hiệu để thu được đáp án.

Độ phức tạp thời gian là $O(m \times \log m + n \times \log n)$ và độ phức tạp không gian là $O(n + m)$. Ở đây, $n$ và $m$ lần lượt là độ dài của các mảng $\textit{flowers}$ và $\textit{people}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def fullBloomFlowers(
        self, flowers: List[List[int]], people: List[int]
    ) -> List[int]:
        d = defaultdict(int)
        for st, ed in flowers:
            d[st] += 1
            d[ed + 1] -= 1
        ts = sorted(d)
        s = i = 0
        m = len(people)
        ans = [0] * m
        for t, j in sorted(zip(people, range(m))):
            while i < len(ts) and ts[i] <= t:
                s += d[ts[i]]
                i += 1
            ans[j] = s
        return ans
```

#### Java

```java
class Solution {
    public int[] fullBloomFlowers(int[][] flowers, int[] people) {
        TreeMap<Integer, Integer> d = new TreeMap<>();
        for (int[] f : flowers) {
            d.merge(f[0], 1, Integer::sum);
            d.merge(f[1] + 1, -1, Integer::sum);
        }
        int s = 0;
        int m = people.length;
        Integer[] idx = new Integer[m];
        for (int i = 0; i < m; i++) {
            idx[i] = i;
        }
        Arrays.sort(idx, Comparator.comparingInt(i -> people[i]));
        int[] ans = new int[m];
        for (int i : idx) {
            int t = people[i];
            while (!d.isEmpty() && d.firstKey() <= t) {
                s += d.pollFirstEntry().getValue();
            }
            ans[i] = s;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> fullBloomFlowers(vector<vector<int>>& flowers, vector<int>& people) {
        map<int, int> d;
        for (auto& f : flowers) {
            d[f[0]]++;
            d[f[1] + 1]--;
        }
        int m = people.size();
        vector<int> idx(m);
        iota(idx.begin(), idx.end(), 0);
        sort(idx.begin(), idx.end(), [&](int i, int j) {
            return people[i] < people[j];
        });
        vector<int> ans(m);
        int s = 0;
        for (int i : idx) {
            int t = people[i];
            while (!d.empty() && d.begin()->first <= t) {
                s += d.begin()->second;
                d.erase(d.begin());
            }
            ans[i] = s;
        }
        return ans;
    }
};
```

#### Go

```go
func fullBloomFlowers(flowers [][]int, people []int) []int {
	d := map[int]int{}
	for _, f := range flowers {
		d[f[0]]++
		d[f[1]+1]--
	}
	ts := []int{}
	for t := range d {
		ts = append(ts, t)
	}
	sort.Ints(ts)
	m := len(people)
	idx := make([]int, m)
	for i := range idx {
		idx[i] = i
	}
	sort.Slice(idx, func(i, j int) bool { return people[idx[i]] < people[idx[j]] })
	ans := make([]int, m)
	s, i := 0, 0
	for _, j := range idx {
		t := people[j]
		for i < len(ts) && ts[i] <= t {
			s += d[ts[i]]
			i++
		}
		ans[j] = s
	}
	return ans
}
```

#### TypeScript

```ts
function fullBloomFlowers(flowers: number[][], people: number[]): number[] {
    const d: Map<number, number> = new Map();
    for (const [st, ed] of flowers) {
        d.set(st, (d.get(st) || 0) + 1);
        d.set(ed + 1, (d.get(ed + 1) || 0) - 1);
    }
    const ts = [...d.keys()].sort((a, b) => a - b);
    let s = 0;
    let i = 0;
    const m = people.length;
    const idx: number[] = [...Array(m)].map((_, i) => i).sort((a, b) => people[a] - people[b]);
    const ans = Array(m).fill(0);
    for (const j of idx) {
        const t = people[j];
        while (i < ts.length && ts[i] <= t) {
            s += d.get(ts[i])!;
            ++i;
        }
        ans[j] = s;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
