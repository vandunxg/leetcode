---
comments: true
difficulty: Medium
rating: 1851
source: Weekly Contest 444 Q2
tags:
    - Design
    - Queue
    - Array
    - Hash Table
    - Binary Search
    - Ordered Set
---

<!-- problem:start -->

# [3508. Implement Router](https://leetcode.com/problems/implement-router)

[中文文档](/solution/3500-3599/3508.Implement%20Router/README.md)

## Mô tả

<!-- description:start -->

<p>Thiết kế một cấu trúc dữ liệu có thể quản lý hiệu quả các gói dữ liệu trong một router mạng. Mỗi gói dữ liệu gồm các thuộc tính sau:</p>

<ul>
    <li><code>source</code>: Mã định danh duy nhất của máy tạo ra gói dữ liệu.</li>
    <li><code>destination</code>: Mã định danh duy nhất của máy đích.</li>
    <li><code>timestamp</code>: Thời điểm gói dữ liệu đến router.</li>
</ul>

<p>Triển khai class <code>Router</code>:</p>

<p><code>Router(int memoryLimit)</code>: Khởi tạo đối tượng Router với giới hạn bộ nhớ cố định.</p>

<ul>
    <li><code>memoryLimit</code> là số lượng <strong>tối đa</strong> các gói dữ liệu mà router có thể lưu trữ tại một thời điểm bất kỳ.</li>
    <li>Nếu việc thêm một gói dữ liệu mới vượt quá giới hạn này, phải xóa gói dữ liệu <strong>cũ nhất</strong> để giải phóng chỗ trống.</li>
</ul>

<p><code>bool addPacket(int source, int destination, int timestamp)</code>: Thêm một gói dữ liệu với các thuộc tính đã cho vào router.</p>

<ul>
    <li>Một gói dữ liệu được xem là trùng lặp nếu đã có một gói khác trong router có cùng <code>source</code>, <code>destination</code> và <code>timestamp</code>.</li>
    <li>Trả về <code>true</code> nếu gói dữ liệu được thêm thành công (tức là không bị trùng lặp); ngược lại trả về <code>false</code>.</li>
</ul>

<p><code>int[] forwardPacket()</code>: Chuyển tiếp gói dữ liệu tiếp theo theo thứ tự FIFO (First In First Out).</p>

<ul>
    <li>Xóa gói dữ liệu khỏi bộ lưu trữ.</li>
    <li>Trả về gói dữ liệu dưới dạng mảng <code>[source, destination, timestamp]</code>.</li>
    <li>Nếu không có gói dữ liệu nào để chuyển tiếp, trả về một mảng rỗng.</li>
</ul>

<p><code>int getCount(int destination, int startTime, int endTime)</code>:</p>

<ul>
    <li>Trả về số lượng gói dữ liệu hiện đang được lưu trong router (tức là chưa được chuyển tiếp), có đích đến được chỉ định và có timestamp nằm trong đoạn đóng <code>[startTime, endTime]</code>.</li>
</ul>

<p><strong>Lưu ý</strong> rằng các truy vấn đến <code>addPacket</code> sẽ được thực hiện theo thứ tự <code>timestamp</code> không giảm.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong><br />
<span class="example-io">[&quot;Router&quot;, &quot;addPacket&quot;, &quot;addPacket&quot;, &quot;addPacket&quot;, &quot;addPacket&quot;, &quot;addPacket&quot;, &quot;forwardPacket&quot;, &quot;addPacket&quot;, &quot;getCount&quot;]<br />
[[3], [1, 4, 90], [2, 5, 90], [1, 4, 90], [3, 5, 95], [4, 5, 105], [], [5, 2, 110], [5, 100, 110]]</span></p>

<p><strong>Đầu ra:</strong><br />
<span class="example-io">[null, true, true, false, true, true, [2, 5, 90], true, 1] </span></p>

<p><strong>Giải thích</strong></p>
Router router = new Router(3); // Initialize Router with memoryLimit of 3.<br />
router.addPacket(1, 4, 90); // Packet is added. Return True.<br />
router.addPacket(2, 5, 90); // Packet is added. Return True.<br />
router.addPacket(1, 4, 90); // This is a duplicate packet. Return False.<br />
router.addPacket(3, 5, 95); // Packet is added. Return True<br />
router.addPacket(4, 5, 105); // Packet is added, <code>[1, 4, 90]</code> is removed as number of packets exceeds memoryLimit. Return True.<br />
router.forwardPacket(); // Return <code>[2, 5, 90]</code> and remove it from router.<br />
router.addPacket(5, 2, 110); // Packet is added. Return True.<br />
router.getCount(5, 100, 110); // The only packet with destination 5 and timestamp in the inclusive range <code>[100, 110]</code> is <code>[4, 5, 105]</code>. Return 1.</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong><br />
<span class="example-io">[&quot;Router&quot;, &quot;addPacket&quot;, &quot;forwardPacket&quot;, &quot;forwardPacket&quot;]<br />
[[2], [7, 4, 90], [], []]</span></p>

<p><strong>Đầu ra:</strong><br />
<span class="example-io">[null, true, [7, 4, 90], []] </span></p>

<p><strong>Giải thích</strong></p>
Router router = new Router(2); // Initialize <code>Router</code> with <code>memoryLimit</code> of 2.<br />
router.addPacket(7, 4, 90); // Return True.<br />
router.forwardPacket(); // Return <code>[7, 4, 90]</code>.<br />
router.forwardPacket(); // There are no packets left, return <code>[]</code>.</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= memoryLimit &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= source, destination &lt;= 2 * 10<sup>5</sup></code></li>
    <li><code>1 &lt;= timestamp &lt;= 10<sup>9</sup></code></li>
    <li><code>1 &lt;= startTime &lt;= endTime &lt;= 10<sup>9</sup></code></li>
    <li>Tổng số lần gọi đến các phương thức <code>addPacket</code>, <code>forwardPacket</code> và <code>getCount</code> không vượt quá <code>10<sup>5</sup></code>.</li>
    <li>Các truy vấn đến <code>addPacket</code> sẽ được thực hiện theo thứ tự <code>timestamp</code> không giảm.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Map + Queue + Binary Search

<!-- thinking:start -->

> **Tư duy**
>
> Router phải từ chối các gói dữ liệu trùng lặp, loại bỏ gói cũ nhất theo thứ tự đến và đếm các gói theo destination cùng khoảng thời gian. Ta mã hóa một bộ ba thành một số nguyên để kiểm tra sự tồn tại trong $O(1)$; lưu cache trong một queue và lấy phần tử đầu ra khi queue đầy.
>
> Các timestamp của cùng một destination được thêm theo thứ tự, còn việc chuyển tiếp chỉ tăng left pointer, nên $\textit{getCount}$ có thể dùng binary search trên phần hậu tố chưa được chuyển tiếp.

<!-- thinking:end -->

Ta dùng một hash map $\textit{vis}$ để lưu các giá trị hash của những gói dữ liệu đã được thêm, một queue $\textit{q}$ để lưu các gói dữ liệu hiện có trong router, một hash map $\textit{idx}$ để ghi nhận số gói dữ liệu đã được chuyển tiếp của mỗi destination, và một hash map $\textit{d}$ để lưu danh sách timestamp của từng destination.

Với phương thức $\textit{addPacket}$, ta tính giá trị hash của gói dữ liệu. Nếu giá trị này đã tồn tại trong $\textit{vis}$, ta trả về $\text{false}$; nếu không, ta thêm nó vào $\textit{vis}$, kiểm tra xem kích thước queue có vượt quá giới hạn bộ nhớ hay không, và nếu có thì gọi phương thức $\textit{forwardPacket}$ để xóa gói dữ liệu cũ nhất. Sau đó, ta thêm gói dữ liệu mới vào queue và nối timestamp của nó vào danh sách timestamp tương ứng với destination, cuối cùng trả về $\text{true}$. Độ phức tạp thời gian là $O(1)$.

Với phương thức $\textit{forwardPacket}$, nếu queue rỗng, ta trả về một mảng rỗng; nếu không, ta xóa gói dữ liệu ở đầu queue, xóa giá trị hash của nó khỏi $\textit{vis}$, cập nhật số gói dữ liệu đã chuyển tiếp của destination tương ứng và trả về gói dữ liệu đó. Độ phức tạp thời gian là $O(1)$.

Với phương thức $\textit{getCount}$, ta lấy danh sách timestamp và số gói dữ liệu đã chuyển tiếp của destination được chỉ định, sau đó dùng binary search để tìm số timestamp nằm trong khoảng đã cho và trả về số lượng đó. Độ phức tạp thời gian là $O(\log n)$, trong đó $n$ là độ dài của danh sách timestamp.

Độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class Router:

    def __init__(self, memoryLimit: int):
        self.lim = memoryLimit
        self.vis = set()
        self.q = deque()
        self.idx = defaultdict(int)
        self.d = defaultdict(list)

    def addPacket(self, source: int, destination: int, timestamp: int) -> bool:
        x = self.f(source, destination, timestamp)
        if x in self.vis:
            return False
        self.vis.add(x)
        if len(self.q) >= self.lim:
            self.forwardPacket()
        self.q.append((source, destination, timestamp))
        self.d[destination].append(timestamp)
        return True

    def forwardPacket(self) -> List[int]:
        if not self.q:
            return []
        s, d, t = self.q.popleft()
        self.vis.remove(self.f(s, d, t))
        self.idx[d] += 1
        return [s, d, t]

    def f(self, a: int, b: int, c: int) -> int:
        return a << 46 | b << 29 | c

    def getCount(self, destination: int, startTime: int, endTime: int) -> int:
        ls = self.d[destination]
        k = self.idx[destination]
        i = bisect_left(ls, startTime, k)
        j = bisect_left(ls, endTime + 1, k)
        return j - i


# Your Router object will be instantiated and called as such:
# obj = Router(memoryLimit)
# param_1 = obj.addPacket(source,destination,timestamp)
# param_2 = obj.forwardPacket()
# param_3 = obj.getCount(destination,startTime,endTime)
```

#### Java

```java
class Router {
    private int lim;
    private Set<Long> vis = new HashSet<>();
    private Deque<int[]> q = new ArrayDeque<>();
    private Map<Integer, Integer> idx = new HashMap<>();
    private Map<Integer, List<Integer>> d = new HashMap<>();

    public Router(int memoryLimit) {
        this.lim = memoryLimit;
    }

    public boolean addPacket(int source, int destination, int timestamp) {
        long x = f(source, destination, timestamp);
        if (vis.contains(x)) {
            return false;
        }
        vis.add(x);
        if (q.size() >= lim) {
            forwardPacket();
        }
        q.offer(new int[] {source, destination, timestamp});
        d.computeIfAbsent(destination, k -> new ArrayList<>()).add(timestamp);
        return true;
    }

    public int[] forwardPacket() {
        if (q.isEmpty()) {
            return new int[] {};
        }
        int[] packet = q.poll();
        int s = packet[0], d_ = packet[1], t = packet[2];
        vis.remove(f(s, d_, t));
        idx.merge(d_, 1, Integer::sum);
        return new int[] {s, d_, t};
    }

    private long f(int a, int b, int c) {
        return ((long) a << 46) | ((long) b << 29) | (long) c;
    }

    public int getCount(int destination, int startTime, int endTime) {
        List<Integer> ls = d.getOrDefault(destination, List.of());
        int k = idx.getOrDefault(destination, 0);
        int i = lowerBound(ls, startTime, k);
        int j = lowerBound(ls, endTime + 1, k);
        return j - i;
    }

    private int lowerBound(List<Integer> list, int target, int fromIndex) {
        int l = fromIndex, r = list.size();
        while (l < r) {
            int m = (l + r) >>> 1;
            if (list.get(m) < target) {
                l = m + 1;
            } else {
                r = m;
            }
        }
        return l;
    }
}

/**
 * Your Router object will be instantiated and called as such:
 * Router obj = new Router(memoryLimit);
 * boolean param_1 = obj.addPacket(source,destination,timestamp);
 * int[] param_2 = obj.forwardPacket();
 * int param_3 = obj.getCount(destination,startTime,endTime);
 */
```

#### C++

```cpp
class Router {
private:
    int lim;
    unordered_set<long long> vis;
    deque<array<int, 3>> q;
    unordered_map<int, int> idx;
    unordered_map<int, vector<int>> d;

    long long f(int a, int b, int c) {
        return ((long long) a << 46) | ((long long) b << 29) | (long long) c;
    }

public:
    Router(int memoryLimit) {
        lim = memoryLimit;
    }

    bool addPacket(int source, int destination, int timestamp) {
        long long x = f(source, destination, timestamp);
        if (vis.count(x)) {
            return false;
        }
        vis.insert(x);
        if ((int) q.size() >= lim) {
            forwardPacket();
        }
        q.push_back({source, destination, timestamp});
        d[destination].push_back(timestamp);
        return true;
    }

    vector<int> forwardPacket() {
        if (q.empty()) {
            return {};
        }
        auto packet = q.front();
        q.pop_front();
        int s = packet[0], d_ = packet[1], t = packet[2];
        vis.erase(f(s, d_, t));
        idx[d_]++;
        return {s, d_, t};
    }

    int getCount(int destination, int startTime, int endTime) {
        auto& ls = d[destination];
        int k = idx[destination];
        auto i = lower_bound(ls.begin() + k, ls.end(), startTime);
        auto j = lower_bound(ls.begin() + k, ls.end(), endTime + 1);
        return j - i;
    }
};

/**
 * Your Router object will be instantiated and called as such:
 * Router* obj = new Router(memoryLimit);
 * bool param_1 = obj->addPacket(source,destination,timestamp);
 * vector<int> param_2 = obj->forwardPacket();
 * int param_3 = obj->getCount(destination,startTime,endTime);
 */
```

#### Go

```go
type Router struct {
    lim int
    vis map[int64]struct{}
    q   [][3]int
    idx map[int]int
    d   map[int][]int
}

func Constructor(memoryLimit int) Router {
    return Router{
        lim: memoryLimit,
        vis: make(map[int64]struct{}),
        q:   make([][3]int, 0),
        idx: make(map[int]int),
        d:   make(map[int][]int),
    }
}

func (this *Router) f(a, b, c int) int64 {
    return int64(a)<<46 | int64(b)<<29 | int64(c)
}

func (this *Router) AddPacket(source int, destination int, timestamp int) bool {
    x := this.f(source, destination, timestamp)
    if _, ok := this.vis[x]; ok {
        return false
    }
    this.vis[x] = struct{}{}
    if len(this.q) >= this.lim {
        this.ForwardPacket()
    }
    this.q = append(this.q, [3]int{source, destination, timestamp})
    this.d[destination] = append(this.d[destination], timestamp)
    return true
}

func (this *Router) ForwardPacket() []int {
    if len(this.q) == 0 {
        return []int{}
    }
    packet := this.q[0]
    this.q = this.q[1:]
    s, d, t := packet[0], packet[1], packet[2]
    delete(this.vis, this.f(s, d, t))
    this.idx[d]++
    return []int{s, d, t}
}

func (this *Router) GetCount(destination int, startTime int, endTime int) int {
    ls := this.d[destination]
    k := this.idx[destination]
    i := sort.Search(len(ls)-k, func(i int) bool { return ls[i+k] >= startTime }) + k
    j := sort.Search(len(ls)-k, func(i int) bool { return ls[i+k] >= endTime+1 }) + k
    return j - i
}

/**
 * Your Router object will be instantiated and called as such:
 * obj := Constructor(memoryLimit)
 * param_1 := obj.AddPacket(source,destination,timestamp)
 * param_2 := obj.ForwardPacket()
 * param_3 := obj.GetCount(destination,startTime,endTime)
 */
```

#### TypeScript

```ts
class Router {
    private lim: number;
    private vis: Set<number>;
    private q: [number, number, number][];
    private idx: Map<number, number>;
    private d: Map<number, number[]>;

    constructor(memoryLimit: number) {
        this.lim = memoryLimit;
        this.vis = new Set();
        this.q = [];
        this.idx = new Map();
        this.d = new Map();
    }

    private f(a: number, b: number, c: number): number {
        return ((BigInt(a) << 46n) | (BigInt(b) << 29n) | BigInt(c)) as unknown as number;
    }

    addPacket(source: number, destination: number, timestamp: number): boolean {
        const x = this.f(source, destination, timestamp);
        if (this.vis.has(x)) {
            return false;
        }
        this.vis.add(x);
        if (this.q.length >= this.lim) {
            this.forwardPacket();
        }
        this.q.push([source, destination, timestamp]);
        if (!this.d.has(destination)) {
            this.d.set(destination, []);
        }
        this.d.get(destination)!.push(timestamp);
        return true;
    }

    forwardPacket(): number[] {
        if (this.q.length === 0) {
            return [];
        }
        const [s, d, t] = this.q.shift()!;
        this.vis.delete(this.f(s, d, t));
        this.idx.set(d, (this.idx.get(d) ?? 0) + 1);
        return [s, d, t];
    }

    getCount(destination: number, startTime: number, endTime: number): number {
        const ls = this.d.get(destination) ?? [];
        const k = this.idx.get(destination) ?? 0;
        const i = this.lowerBound(ls, startTime, k);
        const j = this.lowerBound(ls, endTime + 1, k);
        return j - i;
    }

    private lowerBound(arr: number[], target: number, from: number): number {
        let l = from,
            r = arr.length;
        while (l < r) {
            const m = Math.floor((l + r) / 2);
            if (arr[m] < target) {
                l = m + 1;
            } else {
                r = m;
            }
        }
        return l;
    }
}

/**
 * Your Router object will be instantiated and called as such:
 * var obj = new Router(memoryLimit)
 * var param_1 = obj.addPacket(source,destination,timestamp)
 * var param_2 = obj.forwardPacket()
 * var param_3 = obj.getCount(destination,startTime,endTime)
 */
```

#### Rust

```rust
use std::collections::{HashSet, HashMap, VecDeque};

struct Router {
    lim: usize,
    vis: HashSet<i64>,
    q: VecDeque<(i32, i32, i32)>,
    idx: HashMap<i32, usize>,
    d: HashMap<i32, Vec<i32>>,
}

impl Router {
    fn new(memory_limit: i32) -> Self {
        Router {
            lim: memory_limit as usize,
            vis: HashSet::new(),
            q: VecDeque::new(),
            idx: HashMap::new(),
            d: HashMap::new(),
        }
    }

    fn f(a: i32, b: i32, c: i32) -> i64 {
        ((a as i64) << 46) | ((b as i64) << 29) | (c as i64)
    }

    fn add_packet(&mut self, source: i32, destination: i32, timestamp: i32) -> bool {
        let x = Self::f(source, destination, timestamp);
        if self.vis.contains(&x) {
            return false;
        }
        self.vis.insert(x);
        if self.q.len() >= self.lim {
            self.forward_packet();
        }
        self.q.push_back((source, destination, timestamp));
        self.d.entry(destination).or_default().push(timestamp);
        true
    }

    fn forward_packet(&mut self) -> Vec<i32> {
        if let Some((s, d, t)) = self.q.pop_front() {
            self.vis.remove(&Self::f(s, d, t));
            *self.idx.entry(d).or_insert(0) += 1;
            vec![s, d, t]
        } else {
            vec![]
        }
    }

    fn get_count(&self, destination: i32, start_time: i32, end_time: i32) -> i32 {
        let ls = self.d.get(&destination);
        if ls.is_none() {
            return 0;
        }
        let ls = ls.unwrap();
        let k = *self.idx.get(&destination).unwrap_or(&0);
        let i = k + ls[k..].partition_point(|&x| x < start_time);
        let j = k + ls[k..].partition_point(|&x| x < end_time + 1);
        (j - i) as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
