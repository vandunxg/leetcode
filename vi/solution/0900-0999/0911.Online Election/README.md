---
comments: true
difficulty: Medium
tags:
    - Design
    - Array
    - Hash Table
    - Binary Search
---

<!-- problem:start -->

# [911. Online Election](https://leetcode.com/problems/online-election)

[中文文档](/solution/0900-0999/0911.Online%20Election/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>persons</code> và <code>times</code>. Trong một cuộc bầu cử, lá phiếu thứ <code>i<sup>th</sup></code> được bỏ cho <code>persons[i]</code> tại thời điểm <code>times[i]</code>.</p>

<p>Với mỗi query tại thời điểm <code>t</code>, hãy tìm ứng viên đang dẫn đầu cuộc bầu cử vào thời điểm đó. Các lá phiếu được bỏ tại thời điểm <code>t</code> cũng được tính. Nếu hòa, ứng viên nhận lá phiếu gần nhất trong số các ứng viên hòa sẽ thắng.</p>

<p>Hãy triển khai class <code>TopVotedCandidate</code>:</p>

<ul>
	<li><code>TopVotedCandidate(int[] persons, int[] times)</code> khởi tạo đối tượng bằng hai mảng <code>persons</code> và <code>times</code>.</li>
	<li><code>int q(int t)</code> trả về mã số của ứng viên dẫn đầu cuộc bầu cử tại thời điểm <code>t</code> theo các quy tắc trên.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input</strong>
[&quot;TopVotedCandidate&quot;, &quot;q&quot;, &quot;q&quot;, &quot;q&quot;, &quot;q&quot;, &quot;q&quot;, &quot;q&quot;]
[[[0, 1, 1, 0, 0, 1, 0], [0, 5, 10, 15, 20, 25, 30]], [3], [12], [25], [15], [24], [8]]
<strong>Output</strong>
[null, 0, 1, 1, 0, 0, 1]

<strong>Giải thích</strong>
TopVotedCandidate topVotedCandidate = new TopVotedCandidate([0, 1, 1, 0, 0, 1, 0], [0, 5, 10, 15, 20, 25, 30]);
topVotedCandidate.q(3); // return 0, At time 3, the votes are [0], and 0 is leading.
topVotedCandidate.q(12); // return 1, At time 12, the votes are [0,1,1], and 1 is leading.
topVotedCandidate.q(25); // return 1, At time 25, the votes are [0,1,1,0,0,1], and 1 is leading (as ties go to the most recent vote.)
topVotedCandidate.q(15); // return 0
topVotedCandidate.q(24); // return 0
topVotedCandidate.q(8); // return 1

</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= persons.length &lt;= 5000</code></li>
	<li><code>times.length == persons.length</code></li>
	<li><code>0 &lt;= persons[i] &lt; persons.length</code></li>
	<li><code>0 &lt;= times[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>times</code> được sắp xếp theo thứ tự tăng nghiêm ngặt.</li>
	<li><code>times[0] &lt;= t &lt;= 10<sup>9</sup></code></li>
	<li>Sẽ có nhiều nhất <code>10<sup>4</sup></code> lần gọi <code>q</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi query hỏi ai đang dẫn đầu tại thời điểm $t$. Duyệt các lá phiếu đến thời điểm $t$ sẽ mất thời gian tuyến tính và quá chậm khi có nhiều query. Vì $times$ tăng nghiêm ngặt, ta có thể tính trước người thắng sau mỗi lá phiếu, xử lý trường hợp hòa bằng cách ưu tiên người vừa nhận phiếu gần nhất.
>
> Với mỗi query, tìm kiếm nhị phân lá phiếu cuối cùng có thời điểm $\le t$ rồi trả về người thắng đã lưu tương ứng.

<!-- thinking:end -->

Trong lúc khởi tạo, ta có thể lưu người thắng sau mỗi thời điểm. Khi query, dùng tìm kiếm nhị phân để tìm thời điểm lớn nhất không vượt quá $t$, rồi trả về người thắng tại thời điểm đó.

Khi khởi tạo, ta dùng bộ đếm $cnt$ để lưu số phiếu của từng ứng viên và biến $cur$ để lưu ứng viên đang dẫn đầu. Sau đó, ta duyệt từng thời điểm, cập nhật $cnt$ và $cur$, rồi lưu người thắng tại thời điểm đó.

Khi query, ta dùng tìm kiếm nhị phân để tìm thời điểm lớn nhất không vượt quá $t$, rồi trả về người thắng tại thời điểm đó.

Độ phức tạp thời gian khi khởi tạo là $O(n)$, còn mỗi query cần $O(\log n)$. Độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class TopVotedCandidate:

    def __init__(self, persons: List[int], times: List[int]):
        cnt = Counter()
        self.times = times
        self.wins = []
        cur = 0
        for p in persons:
            cnt[p] += 1
            if cnt[cur] <= cnt[p]:
                cur = p
            self.wins.append(cur)

    def q(self, t: int) -> int:
        i = bisect_right(self.times, t) - 1
        return self.wins[i]


# Your TopVotedCandidate object will be instantiated and called as such:
# obj = TopVotedCandidate(persons, times)
# param_1 = obj.q(t)
```

#### Java

```java
class TopVotedCandidate {
    private int[] times;
    private int[] wins;

    public TopVotedCandidate(int[] persons, int[] times) {
        int n = persons.length;
        wins = new int[n];
        this.times = times;
        int[] cnt = new int[n];
        int cur = 0;
        for (int i = 0; i < n; ++i) {
            int p = persons[i];
            ++cnt[p];
            if (cnt[cur] <= cnt[p]) {
                cur = p;
            }
            wins[i] = cur;
        }
    }

    public int q(int t) {
        int i = Arrays.binarySearch(times, t + 1);
        i = i < 0 ? -i - 2 : i - 1;
        return wins[i];
    }
}

/**
 * Your TopVotedCandidate object will be instantiated and called as such:
 * TopVotedCandidate obj = new TopVotedCandidate(persons, times);
 * int param_1 = obj.q(t);
 */
```

#### C++

```cpp
class TopVotedCandidate {
public:
    TopVotedCandidate(vector<int>& persons, vector<int>& times) {
        int n = persons.size();
        this->times = times;
        wins.resize(n);
        vector<int> cnt(n);
        int cur = 0;
        for (int i = 0; i < n; ++i) {
            int p = persons[i];
            ++cnt[p];
            if (cnt[cur] <= cnt[p]) {
                cur = p;
            }
            wins[i] = cur;
        }
    }

    int q(int t) {
        int i = upper_bound(times.begin(), times.end(), t) - times.begin() - 1;
        return wins[i];
    }

private:
    vector<int> times;
    vector<int> wins;
};

/**
 * Your TopVotedCandidate object will be instantiated and called as such:
 * TopVotedCandidate* obj = new TopVotedCandidate(persons, times);
 * int param_1 = obj->q(t);
 */
```

#### Go

```go
type TopVotedCandidate struct {
	times []int
	wins  []int
}

func Constructor(persons []int, times []int) TopVotedCandidate {
	n := len(persons)
	wins := make([]int, n)
	cnt := make([]int, n)
	cur := 0
	for i, p := range persons {
		cnt[p]++
		if cnt[cur] <= cnt[p] {
			cur = p
		}
		wins[i] = cur
	}
	return TopVotedCandidate{times, wins}
}

func (this *TopVotedCandidate) Q(t int) int {
	i := sort.SearchInts(this.times, t+1) - 1
	return this.wins[i]
}

/**
 * Your TopVotedCandidate object will be instantiated and called as such:
 * obj := Constructor(persons, times);
 * param_1 := obj.Q(t);
 */
```

#### TypeScript

```ts
class TopVotedCandidate {
    private times: number[];
    private wins: number[];

    constructor(persons: number[], times: number[]) {
        const n = persons.length;
        this.times = times;
        this.wins = new Array<number>(n).fill(0);
        const cnt: Array<number> = new Array<number>(n).fill(0);
        let cur = 0;
        for (let i = 0; i < n; ++i) {
            const p = persons[i];
            cnt[p]++;
            if (cnt[cur] <= cnt[p]) {
                cur = p;
            }
            this.wins[i] = cur;
        }
    }

    q(t: number): number {
        const search = (t: number): number => {
            let l = 0,
                r = this.times.length;
            while (l < r) {
                const mid = (l + r) >> 1;
                if (this.times[mid] > t) {
                    r = mid;
                } else {
                    l = mid + 1;
                }
            }
            return l;
        };
        const i = search(t) - 1;
        return this.wins[i];
    }
}

/**
 * Your TopVotedCandidate object will be instantiated and called as such:
 * var obj = new TopVotedCandidate(persons, times)
 * var param_1 = obj.q(t)
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
