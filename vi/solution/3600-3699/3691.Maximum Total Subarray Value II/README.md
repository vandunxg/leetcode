---
comments: true
difficulty: Hard
rating: 2469
source: Weekly Contest 468 Q4
tags:
    - Greedy
    - Segment Tree
    - Array
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3691. Maximum Total Subarray Value II](https://leetcode.com/problems/maximum-total-subarray-value-ii)

[中文文档](/solution/3600-3699/3691.Maximum%20Total%20Subarray%20Value%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code> và một số nguyên <code>k</code>.</p>

<p>Bạn phải chọn <strong>chính xác</strong> <code>k</code> <strong>mảng con</strong> <span data-keyword="subarray-nonempty">khác nhau</span> <code>nums[l..r]</code> của <code>nums</code>. Các mảng con có thể chồng lấn lên nhau, nhưng không thể chọn cùng một mảng con (cùng <code>l</code> và <code>r</code>) nhiều hơn một lần.</p>

<p><strong>Giá trị</strong> của một mảng con <code>nums[l..r]</code> được định nghĩa là: <code>max(nums[l..r]) - min(nums[l..r])</code>.</p>

<p><strong>Tổng giá trị</strong> là tổng <strong>giá trị</strong> của tất cả các mảng con đã chọn.</p>

<p>Trả về <strong>tổng giá trị</strong> <strong>lớn nhất</strong> có thể đạt được.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,3,2], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một cách chọn tối ưu là:</p>

<ul>
	<li>Chọn <code>nums[0..1] = [1, 3]</code>. Giá trị lớn nhất là 3 và giá trị nhỏ nhất là 1, nên giá trị của mảng con là <code>3 - 1 = 2</code>.</li>
	<li>Chọn <code>nums[0..2] = [1, 3, 2]</code>. Giá trị lớn nhất vẫn là 3 và giá trị nhỏ nhất vẫn là 1, nên giá trị cũng là <code>3 - 1 = 2</code>.</li>
</ul>

<p>Cộng lại ta được <code>2 + 2 = 4</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,2,5,1], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một cách chọn tối ưu là:</p>

<ul>
	<li>Chọn <code>nums[0..3] = [4, 2, 5, 1]</code>. Giá trị lớn nhất là 5 và giá trị nhỏ nhất là 1, nên giá trị của mảng con là <code>5 - 1 = 4</code>.</li>
	<li>Chọn <code>nums[1..3] = [2, 5, 1]</code>. Giá trị lớn nhất là 5 và giá trị nhỏ nhất là 1, nên giá trị cũng là <code>4</code>.</li>
	<li>Chọn <code>nums[2..3] = [5, 1]</code>. Giá trị lớn nhất là 5 và giá trị nhỏ nhất là 1, nên giá trị một lần nữa là <code>4</code>.</li>
</ul>

<p>Cộng lại ta được <code>4 + 4 + 4 = 12</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 5 * 10<sup>​​​​​​​4</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= min(10<sup>5</sup>, n * (n + 1) / 2)</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Phương pháp 1: Sparse Table (ST) + Priority Queue (Max-Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Khác với I, $k$ mảng con phải khác nhau, vì vậy không thể sử dụng lại cùng một đoạn. Với một đầu trái cố định $l$, giá trị sẽ đơn điệu theo đầu phải.
>
> Khi đó ta có $n$ dãy đơn điệu và cần tính tổng của $k$ phần tử lớn nhất trên toàn bộ các dãy. Đưa phần tử cuối của mỗi dãy $[l,n-1]$ vào max-heap; sau khi lấy một phần tử ra, nếu đầu phải có thể thu hẹp thì đưa phần tử kế tiếp vào heap.
>
> Sparse table trả lời truy vấn max/min trên đoạn trong $O(1)$. $k$ lần lấy phần tử ra có chi phí $O(\log n)$ mỗi lần.

<!-- thinking:end -->

Xét việc duyệt đầu trái $l$ của mảng con. Khi đầu phải $r$ di chuyển sang phải, giá trị của mảng con $\textit{nums}[l..r]$ tăng đơn điệu. Điều này là do giá trị lớn nhất trong đoạn chỉ có thể tăng (hoặc không đổi), còn giá trị nhỏ nhất chỉ có thể giảm (hoặc không đổi). Vì vậy, hiệu của chúng, $\max(\textit{nums}[l..r]) - \min(\textit{nums}[l..r])$, có tính chất không giảm đơn điệu.

Điều này có nghĩa là với mỗi đầu trái cố định $l$, ta có một dãy tăng đơn điệu có độ dài $n - l$, trong đó phần tử thứ $i$ biểu diễn giá trị của $\textit{nums}[l..l+i]$. Khi đó bài toán trở thành: **Cho $n$ dãy tăng đơn điệu, hãy tìm tổng của $k$ phần tử lớn nhất trong tất cả các dãy.**

Vì phần tử cuối của mỗi dãy (tức là khi $r = n - 1$) là giá trị lớn nhất của dãy đó, ta có thể dùng **max-heap (priority queue)** để lọc chúng một cách hiệu quả:

1. **Khởi tạo**: Đưa phần tử cuối của mỗi dãy (với $r = n - 1$), cùng với tọa độ của nó được biểu diễn bởi $(val, l, n - 1)$, vào max-heap.
2. **Lựa chọn tham lam lặp lại**: Lặp lại thao tác này $k$ lần. Ở mỗi lần lặp, lấy phần tử đầu $(val, l, r)$ khỏi heap và cộng $val$ vào đáp án. Nếu $r > l$, điều đó cho biết dãy này vẫn còn các giá trị nhỏ hơn, lớn tiếp theo. Khi đó, ta tính giá trị của phần tử trước đó trong cùng dãy, $(l, r - 1)$, rồi đưa nó trở lại heap.
3. **Tối ưu hóa truy vấn max/min trên đoạn**: Để truy vấn đồng thời giá trị lớn nhất và nhỏ nhất của bất kỳ mảng con nào $[l, r]$ trong thời gian $\mathcal{O}(1)$, ta có thể tiền xử lý một **Sparse Table (ST)**.

Độ phức tạp thời gian là $\mathcal{O}(n \log n + k \log n)$, và độ phức tạp không gian là $\mathcal{O}(n \log n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class SparseTableRMQ:
    def __init__(self, data: List[int]):
        self.n = len(data)
        self.max_log = self.n.bit_length() + 1
        self.f_max = [[0] * self.max_log for _ in range(self.n)]
        self.f_min = [[0] * self.max_log for _ in range(self.n)]

        self.lg = [0] * (self.n + 1)
        for i in range(2, self.n + 1):
            self.lg[i] = self.lg[i >> 1] + 1

        for i in range(self.n):
            self.f_max[i][0] = data[i]
            self.f_min[i][0] = data[i]

        for j in range(1, self.max_log):
            for i in range(self.n - (1 << j) + 1):
                self.f_max[i][j] = max(
                    self.f_max[i][j - 1], self.f_max[i + (1 << (j - 1))][j - 1]
                )
                self.f_min[i][j] = min(
                    self.f_min[i][j - 1], self.f_min[i + (1 << (j - 1))][j - 1]
                )

    def query_max(self, l: int, r: int) -> int:
        k = self.lg[r - l + 1]
        return max(self.f_max[l][k], self.f_max[r - (1 << k) + 1][k])

    def query_min(self, l: int, r: int) -> int:
        k = self.lg[r - l + 1]
        return min(self.f_min[l][k], self.f_min[r - (1 << k) + 1][k])


class Solution:
    def maxTotalValue(self, nums: List[int], k: int) -> int:
        n = len(nums)
        st = SparseTableRMQ(nums)
        pq = []
        for l in range(n):
            val = st.query_max(l, n - 1) - st.query_min(l, n - 1)
            heappush(pq, (-val, l, n - 1))

        ans = 0
        for _ in range(k):
            val, l, r = heappop(pq)
            ans += -val
            if r > l:
                val = st.query_max(l, r - 1) - st.query_min(l, r - 1)
                heappush(pq, (-val, l, r - 1))
        return ans
```

#### Java

```java
class SparseTableRMQ {
    int n;
    int maxLog;
    int[][] fMax;
    int[][] fMin;
    int[] lg;

    public SparseTableRMQ(int[] data) {
        this.n = data.length;
        this.maxLog = 32 - Integer.numberOfLeadingZeros(n) + 1;
        this.fMax = new int[n][maxLog];
        this.fMin = new int[n][maxLog];
        this.lg = new int[n + 1];

        for (int i = 2; i <= n; i++) {
            this.lg[i] = this.lg[i >> 1] + 1;
        }

        for (int i = 0; i < n; i++) {
            this.fMax[i][0] = data[i];
            this.fMin[i][0] = data[i];
        }

        for (int j = 1; j < maxLog; j++) {
            for (int i = 0; i <= n - (1 << j); i++) {
                this.fMax[i][j]
                    = Math.max(this.fMax[i][j - 1], this.fMax[i + (1 << (j - 1))][j - 1]);
                this.fMin[i][j]
                    = Math.min(this.fMin[i][j - 1], this.fMin[i + (1 << (j - 1))][j - 1]);
            }
        }
    }

    public int queryMax(int l, int r) {
        int k = lg[r - l + 1];
        return Math.max(fMax[l][k], fMax[r - (1 << k) + 1][k]);
    }

    public int queryMin(int l, int r) {
        int k = lg[r - l + 1];
        return Math.min(fMin[l][k], fMin[r - (1 << k) + 1][k]);
    }
}

class Solution {
    public long maxTotalValue(int[] nums, int k) {
        int n = nums.length;
        SparseTableRMQ st = new SparseTableRMQ(nums);
        PriorityQueue<long[]> pq = new PriorityQueue<>((a, b) -> Long.compare(b[0], a[0]));

        for (int l = 0; l < n; l++) {
            long val = st.queryMax(l, n - 1) - st.queryMin(l, n - 1);
            pq.offer(new long[] {val, l, n - 1});
        }

        long ans = 0;
        for (int i = 0; i < k; i++) {
            long[] curr = pq.poll();
            long val = curr[0];
            int l = (int) curr[1];
            int r = (int) curr[2];
            ans += val;
            if (r > l) {
                long nextVal = st.queryMax(l, r - 1) - st.queryMin(l, r - 1);
                pq.offer(new long[] {nextVal, l, r - 1});
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class SparseTableRMQ {
public:
    int n;
    int maxLog;
    vector<vector<int>> fMax;
    vector<vector<int>> fMin;
    vector<int> lg;

    SparseTableRMQ(const vector<int>& data) {
        n = data.size();
        maxLog = 32 - __builtin_clz(n) + 1;
        fMax.assign(n, vector<int>(maxLog, 0));
        fMin.assign(n, vector<int>(maxLog, 0));
        lg.assign(n + 1, 0);

        for (int i = 2; i <= n; i++) {
            lg[i] = lg[i >> 1] + 1;
        }

        for (int i = 0; i < n; i++) {
            fMax[i][0] = data[i];
            fMin[i][0] = data[i];
        }

        for (int j = 1; j < maxLog; j++) {
            for (int i = 0; i <= n - (1 << j); i++) {
                fMax[i][j] = max(fMax[i][j - 1], fMax[i + (1 << (j - 1))][j - 1]);
                fMin[i][j] = min(fMin[i][j - 1], fMin[i + (1 << (j - 1))][j - 1]);
            }
        }
    }

    int queryMax(int l, int r) {
        int k = lg[r - l + 1];
        return max(fMax[l][k], fMax[r - (1 << k) + 1][k]);
    }

    int queryMin(int l, int r) {
        int k = lg[r - l + 1];
        return min(fMin[l][k], fMin[r - (1 << k) + 1][k]);
    }
};

class Solution {
public:
    long long maxTotalValue(vector<int>& nums, int k) {
        int n = nums.size();
        SparseTableRMQ st(nums);
        auto cmp = [](const tuple<long long, int, int>& a, const tuple<long long, int, int>& b) {
            return get<0>(a) < get<0>(b);
        };
        priority_queue<tuple<long long, int, int>, vector<tuple<long long, int, int>>, decltype(cmp)> pq(cmp);

        for (int l = 0; l < n; l++) {
            long long val = st.queryMax(l, n - 1) - st.queryMin(l, n - 1);
            pq.push({val, l, n - 1});
        }

        long long ans = 0;
        for (int i = 0; i < k; i++) {
            auto curr = pq.top();
            pq.pop();
            long long val = get<0>(curr);
            int l = get<1>(curr);
            int r = get<2>(curr);
            ans += val;
            if (r > l) {
                long long nextVal = st.queryMax(l, r - 1) - st.queryMin(l, r - 1);
                pq.push({nextVal, l, r - 1});
            }
        }
        return ans;
    }
};
```

#### Go

```go
type SparseTableRMQ struct {
	n      int
	maxLog int
	fMax   [][]int
	fMin   [][]int
	lg     []int
}

func NewSparseTableRMQ(data []int) *SparseTableRMQ {
	n := len(data)
	maxLog := bits.Len(uint(n)) + 1
	fMax := make([][]int, n)
	fMin := make([][]int, n)
	for i := range fMax {
		fMax[i] = make([]int, maxLog)
		fMin[i] = make([]int, maxLog)
	}
	lg := make([]int, n+1)

	for i := 2; i <= n; i++ {
		lg[i] = lg[i>>1] + 1
	}

	for i := 0; i < n; i++ {
		fMax[i][0] = data[i]
		fMin[i][0] = data[i]
	}

	for j := 1; j < maxLog; j++ {
		for i := 0; i <= n-(1<<j); i++ {
			fMax[i][j] = max(fMax[i][j-1], fMax[i+(1<<(j-1))][j-1])
			fMin[i][j] = min(fMin[i][j-1], fMin[i+(1<<(j-1))][j-1])
		}
	}

	return &SparseTableRMQ{n: n, maxLog: maxLog, fMax: fMax, fMin: fMin, lg: lg}
}

func (st *SparseTableRMQ) queryMax(l, r int) int {
	k := st.lg[r-l+1]
	return max(st.fMax[l][k], st.fMax[r-(1<<k)+1][k])
}

func (st *SparseTableRMQ) queryMin(l, r int) int {
	k := st.lg[r-l+1]
	return min(st.fMin[l][k], st.fMin[r-(1<<k)+1][k])
}

type Item struct {
	val  int64
	l, r int
}
type PriorityQueue []*Item

func (pq PriorityQueue) Len() int           { return len(pq) }
func (pq PriorityQueue) Less(i, j int) bool { return pq[i].val > pq[j].val }
func (pq PriorityQueue) Swap(i, j int)      { pq[i], pq[j] = pq[j], pq[i] }
func (pq *PriorityQueue) Push(x any)        { *pq = append(*pq, x.(*Item)) }
func (pq *PriorityQueue) Pop() any {
	old := *pq
	n := len(old)
	item := old[n-1]
	*pq = old[0 : n-1]
	return item
}

func maxTotalValue(nums []int, k int) int64 {
	n := len(nums)
	st := NewSparseTableRMQ(nums)
	pq := &PriorityQueue{}
	heap.Init(pq)

	for l := 0; l < n; l++ {
		val := int64(st.queryMax(l, n-1) - st.queryMin(l, n-1))
		heap.Push(pq, &Item{val: val, l: l, r: n - 1})
	}

	var ans int64 = 0
	for i := 0; i < k; i++ {
		curr := heap.Pop(pq).(*Item)
		ans += curr.val
		if curr.r > curr.l {
			nextVal := int64(st.queryMax(curr.l, curr.r-1) - st.queryMin(curr.l, curr.r-1))
			heap.Push(pq, &Item{val: nextVal, l: curr.l, r: curr.r - 1})
		}
	}
	return ans
}
```

#### TypeScript

```ts
class SparseTableRMQ {
    n: number;
    maxLog: number;
    fMax: number[][];
    fMin: number[][];
    lg: number[];

    constructor(data: number[]) {
        this.n = data.length;
        this.maxLog = Math.floor(Math.log2(this.n)) + 2;
        this.fMax = Array.from({ length: this.n }, () => Array(this.maxLog).fill(0));
        this.fMin = Array.from({ length: this.n }, () => Array(this.maxLog).fill(0));
        this.lg = Array(this.n + 1).fill(0);

        for (let i = 2; i <= this.n; i++) {
            this.lg[i] = this.lg[i >> 1] + 1;
        }

        for (let i = 0; i < this.n; i++) {
            this.fMax[i][0] = data[i];
            this.fMin[i][0] = data[i];
        }

        for (let j = 1; j < this.maxLog; j++) {
            for (let i = 0; i <= this.n - (1 << j); i++) {
                this.fMax[i][j] = Math.max(
                    this.fMax[i][j - 1],
                    this.fMax[i + (1 << (j - 1))][j - 1],
                );
                this.fMin[i][j] = Math.min(
                    this.fMin[i][j - 1],
                    this.fMin[i + (1 << (j - 1))][j - 1],
                );
            }
        }
    }

    queryMax(l: number, r: number): number {
        const k = this.lg[r - l + 1];
        return Math.max(this.fMax[l][k], this.fMax[r - (1 << k) + 1][k]);
    }

    queryMin(l: number, r: number): number {
        const k = this.lg[r - l + 1];
        return Math.min(this.fMin[l][k], this.fMin[r - (1 << k) + 1][k]);
    }
}

function maxTotalValue(nums: number[], k: number): number {
    const n = nums.length;
    const st = new SparseTableRMQ(nums);
    const pq = new PriorityQueue<[number, number, number]>((a, b) => b[0] - a[0]);

    for (let l = 0; l < n; l++) {
        const val = st.queryMax(l, n - 1) - st.queryMin(l, n - 1);
        pq.enqueue([val, l, n - 1]);
    }

    let ans = 0;
    for (let i = 0; i < k; i++) {
        if (pq.isEmpty()) break;
        const curr = pq.dequeue()!;
        const [val, l, r] = curr;
        ans += val;
        if (r > l) {
            const nextVal = st.queryMax(l, r - 1) - st.queryMin(l, r - 1);
            pq.enqueue([nextVal, l, r - 1]);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
