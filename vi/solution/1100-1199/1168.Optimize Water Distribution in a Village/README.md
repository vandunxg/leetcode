---
comments: true
difficulty: Hard
rating: 2069
source: Biweekly Contest 7 Q4
tags:
    - Union Find
    - Graph
    - Kruskal
    - Minimum Spanning Tree
    - Prim
    - Heap (Priority Queue)
    - Borůvka
---

<!-- problem:start -->

# [1168. Optimize Water Distribution in a Village 🔒](https://leetcode.com/problems/optimize-water-distribution-in-a-village)

[中文文档](/solution/1100-1199/1168.Optimize%20Water%20Distribution%20in%20a%20Village/README.md)

## Mô tả

<!-- description:start -->

<p>Một ngôi làng có <code>n</code> ngôi nhà. Ta muốn cấp nước cho tất cả các nhà bằng cách xây giếng và lắp đường ống.</p>

<p>Với mỗi ngôi nhà <code>i</code>, ta có thể xây giếng ngay trong nhà với chi phí <code>wells[i - 1]</code> (lưu ý chỉ số <code>-1</code> do mảng dùng <strong>0-indexing</strong>), hoặc dẫn nước từ giếng khác đến nhà đó. Chi phí lắp ống giữa các nhà được cho trong mảng <code>pipes</code>, trong đó mỗi <code>pipes[j] = [house1<sub>j</sub>, house2<sub>j</sub>, cost<sub>j</sub>]</code> biểu thị chi phí nối <code>house1<sub>j</sub></code> và <code>house2<sub>j</sub></code> bằng đường ống. Các kết nối là hai chiều và có thể có nhiều kết nối giữa cùng một cặp nhà với chi phí khác nhau.</p>

<p>Hãy trả về <em>tổng chi phí nhỏ nhất để cấp nước cho tất cả các ngôi nhà</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1100-1199/1168.Optimize%20Water%20Distribution%20in%20a%20Village/images/1359_ex1.png" style="width: 189px; height: 196px;" />
<pre>
<strong>Đầu vào:</strong> n = 3, wells = [1,2,2], pipes = [[1,2,1],[2,3,1]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Hình minh họa chi phí nối các ngôi nhà bằng đường ống.
Cách tối ưu là xây giếng ở nhà thứ nhất với chi phí 1 rồi nối các nhà còn lại với chi phí 2, tổng chi phí là 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2, wells = [1,1], pipes = [[1,2,1],[1,2,2]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta có thể cấp nước với chi phí 2 bằng một trong ba cách sau:
Cách 1:
  - Xây giếng trong nhà 1 với chi phí 1.
  - Xây giếng trong nhà 2 với chi phí 1.
Tổng chi phí là 2.
Cách 2:
  - Xây giếng trong nhà 1 với chi phí 1.
  - Nối nhà 2 với nhà 1 bằng đường ống với chi phí 1.
Tổng chi phí là 2.
Cách 3:
  - Xây giếng trong nhà 2 với chi phí 1.
  - Nối nhà 1 với nhà 2 bằng đường ống với chi phí 1.
Tổng chi phí là 2.
Lưu ý rằng ta có thể nối nhà 1 và 2 với chi phí 1 hoặc 2, nhưng luôn chọn <strong>phương án rẻ nhất</strong>. 
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>wells.length == n</code></li>
	<li><code>0 &lt;= wells[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= pipes.length &lt;= 10<sup>4</sup></code></li>
	<li><code>pipes[j].length == 3</code></li>
	<li><code>1 &lt;= house1<sub>j</sub>, house2<sub>j</sub> &lt;= n</code></li>
	<li><code>0 &lt;= cost<sub>j</sub> &lt;= 10<sup>5</sup></code></li>
	<li><code>house1<sub>j</sub> != house2<sub>j</sub></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Kruskal's Algorithm (Minimum Spanning Tree)

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi nhà có thể đào giếng riêng hoặc nhận nước qua đường ống. Thêm node ảo $0$, nối node này với mỗi nhà bằng cạnh có trọng số là chi phí xây giếng; khi đó bài toán trở thành tìm MST trên $n+1$ đỉnh. Sắp xếp các cạnh theo chi phí rồi union các thành phần cho đến khi đã nối đủ các nhà với node $0$; tổng chi phí lúc đó là nhỏ nhất.

<!-- thinking:end -->

Ta giả sử có một giếng mang số hiệu $0$. Khi đó, xem kết nối giữa mỗi ngôi nhà và giếng $0$ là một cạnh có trọng số bằng chi phí xây giếng tại nhà đó. Tương tự, kết nối giữa hai ngôi nhà cũng là một cạnh có trọng số bằng chi phí lắp đường ống. Như vậy, bài toán được chuyển thành tìm cây khung nhỏ nhất của đồ thị vô hướng.

Ta có thể dùng thuật toán Kruskal để tìm cây khung nhỏ nhất của đồ thị vô hướng. Trước tiên, thêm cạnh nối giếng $0$ với từng nhà vào mảng $pipes$, rồi sắp xếp $pipes$ theo trọng số cạnh tăng dần. Sau đó, duyệt từng cạnh. Nếu cạnh nối hai thành phần liên thông khác nhau, chọn cạnh đó và gộp hai thành phần. Khi chỉ còn đúng một thành phần liên thông, ta đã tìm được cây khung nhỏ nhất. Đáp án là tổng trọng số các cạnh đã chọn.

Độ phức tạp thời gian là $O((m + n) \times \log (m + n))$ và độ phức tạp không gian là $O(m + n)$. Trong đó, $m$ và $n$ lần lượt là độ dài của mảng $pipes$ và mảng $wells$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCostToSupplyWater(
        self, n: int, wells: List[int], pipes: List[List[int]]
    ) -> int:
        def find(x: int) -> int:
            if p[x] != x:
                p[x] = find(p[x])
            return p[x]

        for i, w in enumerate(wells, 1):
            pipes.append([0, i, w])
        pipes.sort(key=lambda x: x[2])
        p = list(range(n + 1))
        ans = 0
        for a, b, c in pipes:
            pa, pb = find(a), find(b)
            if pa != pb:
                p[pa] = pb
                n -= 1
                ans += c
                if n == 0:
                    return ans
```

#### Java

```java
class Solution {
    private int[] p;

    public int minCostToSupplyWater(int n, int[] wells, int[][] pipes) {
        int[][] nums = Arrays.copyOf(pipes, pipes.length + n);
        for (int i = 0; i < n; i++) {
            nums[pipes.length + i] = new int[] {0, i + 1, wells[i]};
        }
        Arrays.sort(nums, (a, b) -> a[2] - b[2]);
        p = new int[n + 1];
        for (int i = 0; i <= n; i++) {
            p[i] = i;
        }
        int ans = 0;
        for (var x : nums) {
            int a = x[0], b = x[1], c = x[2];
            int pa = find(a), pb = find(b);
            if (pa != pb) {
                p[pa] = pb;
                ans += c;
                if (--n == 0) {
                    return ans;
                }
            }
        }
        return ans;
    }

    private int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minCostToSupplyWater(int n, vector<int>& wells, vector<vector<int>>& pipes) {
        for (int i = 0; i < n; ++i) {
            pipes.push_back({0, i + 1, wells[i]});
        }
        sort(pipes.begin(), pipes.end(), [](const vector<int>& a, const vector<int>& b) {
            return a[2] < b[2];
        });
        int p[n + 1];
        iota(p, p + n + 1, 0);
        function<int(int)> find = [&](int x) {
            if (p[x] != x) {
                p[x] = find(p[x]);
            }
            return p[x];
        };
        int ans = 0;
        for (const auto& x : pipes) {
            int pa = find(x[0]), pb = find(x[1]);
            if (pa == pb) {
                continue;
            }
            p[pa] = pb;
            ans += x[2];
            if (--n == 0) {
                break;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minCostToSupplyWater(n int, wells []int, pipes [][]int) (ans int) {
	for i, w := range wells {
		pipes = append(pipes, []int{0, i + 1, w})
	}
	sort.Slice(pipes, func(i, j int) bool { return pipes[i][2] < pipes[j][2] })
	p := make([]int, n+1)
	for i := range p {
		p[i] = i
	}
	var find func(int) int
	find = func(x int) int {
		if p[x] != x {
			p[x] = find(p[x])
		}
		return p[x]
	}

	for _, x := range pipes {
		pa, pb := find(x[0]), find(x[1])
		if pa == pb {
			continue
		}
		p[pa] = pb
		ans += x[2]
		n--
		if n == 0 {
			break
		}
	}
	return
}
```

#### TypeScript

```ts
function minCostToSupplyWater(n: number, wells: number[], pipes: number[][]): number {
    for (let i = 0; i < n; ++i) {
        pipes.push([0, i + 1, wells[i]]);
    }
    pipes.sort((a, b) => a[2] - b[2]);
    const p = Array(n + 1)
        .fill(0)
        .map((_, i) => i);
    const find = (x: number): number => {
        if (p[x] !== x) {
            p[x] = find(p[x]);
        }
        return p[x];
    };
    let ans = 0;
    for (const [a, b, c] of pipes) {
        const pa = find(a);
        const pb = find(b);
        if (pa === pb) {
            continue;
        }
        p[pa] = pb;
        ans += c;
        if (--n === 0) {
            break;
        }
    }
    return ans;
}
```

#### Rust

```rust
struct UnionFind {
    p: Vec<usize>,
    size: Vec<usize>,
}

impl UnionFind {
    fn new(n: usize) -> Self {
        let p: Vec<usize> = (0..n).collect();
        let size = vec![1; n];
        UnionFind { p, size }
    }

    fn find(&mut self, x: usize) -> usize {
        if self.p[x] != x {
            self.p[x] = self.find(self.p[x]);
        }
        self.p[x]
    }

    fn union(&mut self, a: usize, b: usize) -> bool {
        let pa = self.find(a);
        let pb = self.find(b);
        if pa == pb {
            false
        } else if self.size[pa] > self.size[pb] {
            self.p[pb] = pa;
            self.size[pa] += self.size[pb];
            true
        } else {
            self.p[pa] = pb;
            self.size[pb] += self.size[pa];
            true
        }
    }
}

impl Solution {
    pub fn min_cost_to_supply_water(n: i32, wells: Vec<i32>, pipes: Vec<Vec<i32>>) -> i32 {
        let n = n as usize;
        let mut pipes = pipes;
        for i in 0..n {
            pipes.push(vec![0, (i + 1) as i32, wells[i]]);
        }
        pipes.sort_by(|a, b| a[2].cmp(&b[2]));
        let mut uf = UnionFind::new(n + 1);
        let mut ans = 0;
        for pipe in pipes {
            let a = pipe[0] as usize;
            let b = pipe[1] as usize;
            let c = pipe[2];
            if uf.union(a, b) {
                ans += c;
                if n == 0 {
                    break;
                }
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Cách 1 cài đặt union-find trực tiếp. Cách 2 đóng gói union-by-size; `union` cho biết có thực sự gộp hai thành phần hay không, còn vòng lặp chỉ cộng chi phí và đếm số lần gộp. Đồ thị và thứ tự chạy Kruskal vẫn giữ nguyên.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class UnionFind:
    __slots__ = ("p", "size")

    def __init__(self, n):
        self.p = list(range(n))
        self.size = [1] * n

    def find(self, x: int) -> int:
        if self.p[x] != x:
            self.p[x] = self.find(self.p[x])
        return self.p[x]

    def union(self, a: int, b: int) -> bool:
        pa, pb = self.find(a), self.find(b)
        if pa == pb:
            return False
        if self.size[pa] > self.size[pb]:
            self.p[pb] = pa
            self.size[pa] += self.size[pb]
        else:
            self.p[pa] = pb
            self.size[pb] += self.size[pa]
        return True


class Solution:
    def minCostToSupplyWater(
        self, n: int, wells: List[int], pipes: List[List[int]]
    ) -> int:
        for i, w in enumerate(wells, 1):
            pipes.append([0, i, w])
        pipes.sort(key=lambda x: x[2])
        uf = UnionFind(n + 1)
        ans = 0
        for a, b, c in pipes:
            if uf.union(a, b):
                ans += c
                n -= 1
                if n == 0:
                    return ans
```

#### Java

```java
class UnionFind {
    private int[] p;
    private int[] size;

    public UnionFind(int n) {
        p = new int[n];
        size = new int[n];
        for (int i = 0; i < n; ++i) {
            p[i] = i;
            size[i] = 1;
        }
    }

    public int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }

    public boolean union(int a, int b) {
        int pa = find(a), pb = find(b);
        if (pa == pb) {
            return false;
        }
        if (size[pa] > size[pb]) {
            p[pb] = pa;
            size[pa] += size[pb];
        } else {
            p[pa] = pb;
            size[pb] += size[pa];
        }
        return true;
    }
}

class Solution {
    public int minCostToSupplyWater(int n, int[] wells, int[][] pipes) {
        int[][] nums = Arrays.copyOf(pipes, pipes.length + n);
        for (int i = 0; i < n; i++) {
            nums[pipes.length + i] = new int[] {0, i + 1, wells[i]};
        }
        Arrays.sort(nums, (a, b) -> a[2] - b[2]);
        UnionFind uf = new UnionFind(n + 1);
        int ans = 0;
        for (var x : nums) {
            int a = x[0], b = x[1], c = x[2];
            if (uf.union(a, b)) {
                ans += c;
                if (--n == 0) {
                    break;
                }
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class UnionFind {
public:
    UnionFind(int n) {
        p = vector<int>(n);
        size = vector<int>(n, 1);
        iota(p.begin(), p.end(), 0);
    }

    bool unite(int a, int b) {
        int pa = find(a), pb = find(b);
        if (pa == pb) {
            return false;
        }
        if (size[pa] > size[pb]) {
            p[pb] = pa;
            size[pa] += size[pb];
        } else {
            p[pa] = pb;
            size[pb] += size[pa];
        }
        return true;
    }

    int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }

private:
    vector<int> p, size;
};

class Solution {
public:
    int minCostToSupplyWater(int n, vector<int>& wells, vector<vector<int>>& pipes) {
        for (int i = 0; i < n; ++i) {
            pipes.push_back({0, i + 1, wells[i]});
        }
        sort(pipes.begin(), pipes.end(), [](const vector<int>& a, const vector<int>& b) {
            return a[2] < b[2];
        });
        UnionFind uf(n + 1);
        int ans = 0;
        for (const auto& x : pipes) {
            if (uf.unite(x[0], x[1])) {
                ans += x[2];
                if (--n == 0) {
                    break;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
type unionFind struct {
	p, size []int
}

func newUnionFind(n int) *unionFind {
	p := make([]int, n)
	size := make([]int, n)
	for i := range p {
		p[i] = i
		size[i] = 1
	}
	return &unionFind{p, size}
}

func (uf *unionFind) find(x int) int {
	if uf.p[x] != x {
		uf.p[x] = uf.find(uf.p[x])
	}
	return uf.p[x]
}

func (uf *unionFind) union(a, b int) bool {
	pa, pb := uf.find(a), uf.find(b)
	if pa == pb {
		return false
	}
	if uf.size[pa] > uf.size[pb] {
		uf.p[pb] = pa
		uf.size[pa] += uf.size[pb]
	} else {
		uf.p[pa] = pb
		uf.size[pb] += uf.size[pa]
	}
	return true
}

func minCostToSupplyWater(n int, wells []int, pipes [][]int) (ans int) {
	for i, w := range wells {
		pipes = append(pipes, []int{0, i + 1, w})
	}
	sort.Slice(pipes, func(i, j int) bool { return pipes[i][2] < pipes[j][2] })
	uf := newUnionFind(n + 1)
	for _, x := range pipes {
		if uf.union(x[0], x[1]) {
			ans += x[2]
			n--
			if n == 0 {
				break
			}
		}
	}
	return
}
```

#### TypeScript

```ts
class UnionFind {
    private p: number[];
    private size: number[];

    constructor(n: number) {
        this.p = Array(n)
            .fill(0)
            .map((_, i) => i);
        this.size = Array(n).fill(1);
    }

    find(x: number): number {
        if (this.p[x] !== x) {
            this.p[x] = this.find(this.p[x]);
        }
        return this.p[x];
    }

    union(a: number, b: number): boolean {
        const pa = this.find(a);
        const pb = this.find(b);
        if (pa === pb) {
            return false;
        }
        if (this.size[pa] > this.size[pb]) {
            this.p[pb] = pa;
            this.size[pa] += this.size[pb];
        } else {
            this.p[pa] = pb;
            this.size[pb] += this.size[pa];
        }
        return true;
    }
}

function minCostToSupplyWater(n: number, wells: number[], pipes: number[][]): number {
    for (let i = 0; i < n; ++i) {
        pipes.push([0, i + 1, wells[i]]);
    }
    pipes.sort((a, b) => a[2] - b[2]);
    const uf = new UnionFind(n + 1);
    let ans = 0;
    for (const [a, b, c] of pipes) {
        if (uf.union(a, b)) {
            ans += c;
            if (--n === 0) {
                break;
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
