---
comments: true
difficulty: Medium
rating: 1849
source: Weekly Contest 265 Q3
tags:
    - Breadth-First Search
    - Array
---

<!-- problem:start -->

# [2059. Minimum Operations to Convert Number](https://leetcode.com/problems/minimum-operations-to-convert-number)

[中文文档](/solution/2000-2099/2059.Minimum%20Operations%20to%20Convert%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> là <code>nums</code>, trong đó các số <strong>khác nhau</strong>, một số nguyên <code>start</code> và một số nguyên <code>goal</code>. Có một số nguyên <code>x</code> ban đầu bằng <code>start</code>, và bạn muốn thực hiện các thao tác trên <code>x</code> để biến nó thành <code>goal</code>. Bạn có thể lặp lại thao tác sau trên số <code>x</code>:</p>

<p>Nếu <code>0 &lt;= x &lt;= 1000</code>, thì với mọi chỉ số <code>i</code> trong mảng (<code>0 &lt;= i &lt; nums.length</code>), bạn có thể gán <code>x</code> bằng một trong các giá trị sau:</p>

<ul>
	<li><code>x + nums[i]</code></li>
	<li><code>x - nums[i]</code></li>
	<li><code>x ^ nums[i]</code> (XOR theo bit)</li>
</ul>

<p>Lưu ý rằng bạn có thể sử dụng mỗi <code>nums[i]</code> bao nhiêu lần cũng được và theo bất kỳ thứ tự nào. Các thao tác khiến <code>x</code> nằm ngoài phạm vi <code>0 &lt;= x &lt;= 1000</code> vẫn hợp lệ, nhưng sau đó không thể thực hiện thêm thao tác nào.</p>

<p>Trả về <em>số thao tác <strong>ít nhất</strong> cần thực hiện để biến </em><code>x = start</code><em> thành </em><code>goal</code><em>, và trả về </em><code>-1</code><em> nếu không thể thực hiện</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,4,12], start = 2, goal = 12
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta có thể đi từ 2 &rarr; 14 &rarr; 12 với 2 thao tác sau.
- 2 + 12 = 14
- 14 - 2 = 12
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,5,7], start = 0, goal = -4
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta có thể đi từ 0 &rarr; 3 &rarr; -4 với 2 thao tác sau.
- 0 + 3 = 3
- 3 - 7 = -4
Lưu ý rằng thao tác cuối cùng đưa x ra ngoài phạm vi 0 &lt;= x &lt;= 1000, điều này vẫn hợp lệ.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,8,16], start = 0, goal = 1
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không có cách nào biến 0 thành 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>-10<sup>9</sup> &lt;= nums[i], goal &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= start &lt;= 1000</code></li>
	<li><code>start != goal</code></li>
	<li>Tất cả các số nguyên trong <code>nums</code> đều khác nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi bước cộng, trừ hoặc XOR một giá trị trong $nums$. Các giá trị trung gian được tiếp tục mở rộng đều nằm trong $[0,1000]$, nên có nhiều nhất $1001$ state; BFS là lựa chọn phù hợp cho bài toán tìm đường đi ngắn nhất không trọng số.
>
> Lấy $x$ ra khỏi queue, áp dụng ba phép toán với từng số; trả về ngay khi gặp $goal$, nếu không thì đưa các giá trị chưa được duyệt và nằm trong phạm vi vào queue.

<!-- thinking:end -->

Thực hiện BFS từ $start$, áp dụng phép cộng, phép trừ và phép XOR với từng số cho đến khi đạt được $goal$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumOperations(self, nums: List[int], start: int, goal: int) -> int:
        op1 = lambda x, y: x + y
        op2 = lambda x, y: x - y
        op3 = lambda x, y: x ^ y
        ops = [op1, op2, op3]
        vis = [False] * 1001
        q = deque([(start, 0)])
        while q:
            x, step = q.popleft()
            for num in nums:
                for op in ops:
                    nx = op(x, num)
                    if nx == goal:
                        return step + 1
                    if 0 <= nx <= 1000 and not vis[nx]:
                        q.append((nx, step + 1))
                        vis[nx] = True
        return -1
```

#### Java

```java
class Solution {
    public int minimumOperations(int[] nums, int start, int goal) {
        IntBinaryOperator op1 = (x, y) -> x + y;
        IntBinaryOperator op2 = (x, y) -> x - y;
        IntBinaryOperator op3 = (x, y) -> x ^ y;
        IntBinaryOperator[] ops = {op1, op2, op3};
        boolean[] vis = new boolean[1001];
        Queue<int[]> queue = new ArrayDeque<>();
        queue.offer(new int[] {start, 0});
        while (!queue.isEmpty()) {
            int[] p = queue.poll();
            int x = p[0], step = p[1];
            for (int num : nums) {
                for (IntBinaryOperator op : ops) {
                    int nx = op.applyAsInt(x, num);
                    if (nx == goal) {
                        return step + 1;
                    }
                    if (nx >= 0 && nx <= 1000 && !vis[nx]) {
                        queue.offer(new int[] {nx, step + 1});
                        vis[nx] = true;
                    }
                }
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumOperations(vector<int>& nums, int start, int goal) {
        using pii = pair<int, int>;
        vector<function<int(int, int)>> ops{
            [](int x, int y) { return x + y; },
            [](int x, int y) { return x - y; },
            [](int x, int y) { return x ^ y; },
        };
        vector<bool> vis(1001, false);
        queue<pii> q;
        q.push({start, 0});
        while (!q.empty()) {
            auto [x, step] = q.front();
            q.pop();
            for (int num : nums) {
                for (auto op : ops) {
                    int nx = op(x, num);
                    if (nx == goal) {
                        return step + 1;
                    }
                    if (nx >= 0 && nx <= 1000 && !vis[nx]) {
                        q.push({nx, step + 1});
                        vis[nx] = true;
                    }
                }
            }
        }
        return -1;
    }
};
```

#### Go

```go
func minimumOperations(nums []int, start int, goal int) int {
	type pair struct {
		x    int
		step int
	}

	ops := []func(int, int) int{
		func(x, y int) int { return x + y },
		func(x, y int) int { return x - y },
		func(x, y int) int { return x ^ y },
	}
	vis := make([]bool, 1001)
	q := []pair{{start, 0}}

	for len(q) > 0 {
		x, step := q[0].x, q[0].step
		q = q[1:]
		for _, num := range nums {
			for _, op := range ops {
				nx := op(x, num)
				if nx == goal {
					return step + 1
				}
				if nx >= 0 && nx <= 1000 && !vis[nx] {
					q = append(q, pair{nx, step + 1})
					vis[nx] = true
				}
			}
		}
	}
	return -1
}
```

#### TypeScript

```ts
function minimumOperations(nums: number[], start: number, goal: number): number {
    const n = nums.length;
    const op1 = function (x: number, y: number): number {
        return x + y;
    };
    const op2 = function (x: number, y: number): number {
        return x - y;
    };
    const op3 = function (x: number, y: number): number {
        return x ^ y;
    };
    const ops = [op1, op2, op3];
    let vis = new Array(1001).fill(false);
    let quenue: Array<Array<number>> = [[start, 0]];
    vis[start] = true;
    while (quenue.length) {
        let [x, step] = quenue.shift();
        for (let i = 0; i < n; i++) {
            for (let j = 0; j < ops.length; j++) {
                const nx = ops[j](x, nums[i]);
                if (nx == goal) {
                    return step + 1;
                }
                if (nx >= 0 && nx <= 1000 && !vis[nx]) {
                    vis[nx] = true;
                    quenue.push([nx, step + 1]);
                }
            }
        }
    }
    return -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: BFS (Duyệt theo từng tầng)

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 lưu số bước trong mỗi phần tử của queue. BFS theo từng tầng chỉ cần tăng $ans$ một lần cho mỗi tầng và giúp các phần tử trong queue gọn hơn.
>
> `next` liệt kê các trạng thái kế tiếp; nếu queue rỗng thì trả về $-1$.

<!-- thinking:end -->

BFS tương tự, nhưng mở rộng từng tầng một.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumOperations(self, nums: List[int], start: int, goal: int) -> int:
        def next(x):
            res = []
            for num in nums:
                res.append(x + num)
                res.append(x - num)
                res.append(x ^ num)
            return res

        q = deque([start])
        vis = {start}
        ans = 0
        while q:
            ans += 1
            for _ in range(len(q)):
                x = q.popleft()
                for y in next(x):
                    if y == goal:
                        return ans
                    if 0 <= y <= 1000 and y not in vis:
                        vis.add(y)
                        q.append(y)
        return -1
```

#### Java

```java
class Solution {
    public int minimumOperations(int[] nums, int start, int goal) {
        Deque<Integer> q = new ArrayDeque<>();
        q.offer(start);
        boolean[] vis = new boolean[1001];
        int ans = 0;
        while (!q.isEmpty()) {
            ++ans;
            for (int n = q.size(); n > 0; --n) {
                int x = q.poll();
                for (int y : next(nums, x)) {
                    if (y == goal) {
                        return ans;
                    }
                    if (y >= 0 && y <= 1000 && !vis[y]) {
                        vis[y] = true;
                        q.offer(y);
                    }
                }
            }
        }
        return -1;
    }

    private List<Integer> next(int[] nums, int x) {
        List<Integer> res = new ArrayList<>();
        for (int num : nums) {
            res.add(x + num);
            res.add(x - num);
            res.add(x ^ num);
        }
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumOperations(vector<int>& nums, int start, int goal) {
        queue<int> q{{start}};
        vector<bool> vis(1001);
        int ans = 0;
        while (!q.empty()) {
            ++ans;
            for (int n = q.size(); n > 0; --n) {
                int x = q.front();
                q.pop();
                for (int y : next(nums, x)) {
                    if (y == goal) return ans;
                    if (y >= 0 && y <= 1000 && !vis[y]) {
                        vis[y] = true;
                        q.push(y);
                    }
                }
            }
        }
        return -1;
    }

    vector<int> next(vector<int>& nums, int x) {
        vector<int> res;
        for (int num : nums) {
            res.push_back(x + num);
            res.push_back(x - num);
            res.push_back(x ^ num);
        }
        return res;
    }
};
```

#### Go

```go
func minimumOperations(nums []int, start int, goal int) int {
	next := func(x int) []int {
		var res []int
		for _, num := range nums {
			res = append(res, []int{x + num, x - num, x ^ num}...)
		}
		return res
	}
	q := []int{start}
	vis := make([]bool, 1001)
	ans := 0
	for len(q) > 0 {
		ans++
		for n := len(q); n > 0; n-- {
			x := q[0]
			q = q[1:]
			for _, y := range next(x) {
				if y == goal {
					return ans
				}
				if y >= 0 && y <= 1000 && !vis[y] {
					vis[y] = true
					q = append(q, y)
				}
			}
		}
	}
	return -1
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3: BFS hai chiều

<!-- thinking:start -->

> **Tư duy**
>
> BFS một chiều phát triển nhanh. Tìm kiếm từ cả $start$ và $goal$ giúp hai phía gặp nhau ở giữa; luôn mở rộng frontier nhỏ hơn và cộng hai khoảng cách.
>
> Các giá trị trung gian nằm trong $[0,1000]$; $goal$ có thể nằm ngoài phạm vi này, nên lần mở rộng đầu tiên từ phía đó vẫn có thể chạm tới đích.

<!-- thinking:end -->

Tìm kiếm từ cả $start$ và $goal$ cho đến khi hai phía gặp nhau.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumOperations(self, nums: List[int], start: int, goal: int) -> int:
        def next(x):
            res = []
            for num in nums:
                res.append(x + num)
                res.append(x - num)
                res.append(x ^ num)
            return res

        def extend(m1, m2, q):
            for _ in range(len(q)):
                x = q.popleft()
                step = m1[x]
                for y in next(x):
                    if y in m1:
                        continue
                    if y in m2:
                        return step + 1 + m2[y]
                    if 0 <= y <= 1000:
                        m1[y] = step + 1
                        q.append(y)
            return -1

        m1, m2 = {start: 0}, {goal: 0}
        q1, q2 = deque([start]), deque([goal])
        while q1 and q2:
            t = extend(m1, m2, q1) if len(q1) <= len(q2) else extend(m2, m1, q2)
            if t != -1:
                return t
        return -1
```

#### Java

```java
class Solution {
    private int[] nums;

    public int minimumOperations(int[] nums, int start, int goal) {
        this.nums = nums;
        Map<Integer, Integer> m1 = new HashMap<>();
        Map<Integer, Integer> m2 = new HashMap<>();
        Deque<Integer> q1 = new ArrayDeque<>();
        Deque<Integer> q2 = new ArrayDeque<>();
        m1.put(start, 0);
        m2.put(goal, 0);
        q1.offer(start);
        q2.offer(goal);
        while (!q1.isEmpty() && !q2.isEmpty()) {
            int t = q1.size() <= q2.size() ? extend(m1, m2, q1) : extend(m2, m1, q2);
            if (t != -1) {
                return t;
            }
        }
        return -1;
    }

    private int extend(Map<Integer, Integer> m1, Map<Integer, Integer> m2, Deque<Integer> q) {
        for (int i = q.size(); i > 0; --i) {
            int x = q.poll();
            int step = m1.get(x);
            for (int y : next(x)) {
                if (m1.containsKey(y)) {
                    continue;
                }
                if (m2.containsKey(y)) {
                    return step + 1 + m2.get(y);
                }
                if (y >= 0 && y <= 1000) {
                    m1.put(y, step + 1);
                    q.offer(y);
                }
            }
        }
        return -1;
    }

    private List<Integer> next(int x) {
        List<Integer> res = new ArrayList<>();
        for (int num : nums) {
            res.add(x + num);
            res.add(x - num);
            res.add(x ^ num);
        }
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumOperations(vector<int>& nums, int start, int goal) {
        unordered_map<int, int> m1;
        unordered_map<int, int> m2;
        m1[start] = 0;
        m2[goal] = 0;
        queue<int> q1{{start}};
        queue<int> q2{{goal}};
        while (!q1.empty() && !q2.empty()) {
            int t = q1.size() <= q2.size() ? extend(m1, m2, q1, nums) : extend(m2, m1, q2, nums);
            if (t != -1) return t;
        }
        return -1;
    }

    int extend(unordered_map<int, int>& m1, unordered_map<int, int>& m2, queue<int>& q, vector<int>& nums) {
        for (int i = q.size(); i > 0; --i) {
            int x = q.front();
            int step = m1[x];
            q.pop();
            for (int y : next(nums, x)) {
                if (m1.count(y)) continue;
                if (m2.count(y)) return step + 1 + m2[y];
                if (y >= 0 && y <= 1000) {
                    m1[y] = step + 1;
                    q.push(y);
                }
            }
        }
        return -1;
    }

    vector<int> next(vector<int>& nums, int x) {
        vector<int> res;
        for (int num : nums) {
            res.push_back(x + num);
            res.push_back(x - num);
            res.push_back(x ^ num);
        }
        return res;
    }
};
```

#### Go

```go
func minimumOperations(nums []int, start int, goal int) int {
	next := func(x int) []int {
		var res []int
		for _, num := range nums {
			res = append(res, []int{x + num, x - num, x ^ num}...)
		}
		return res
	}
	m1, m2 := map[int]int{start: 0}, map[int]int{goal: 0}
	q1, q2 := []int{start}, []int{goal}
	extend := func() int {
		for i := len(q1); i > 0; i-- {
			x := q1[0]
			q1 = q1[1:]
			step, _ := m1[x]
			for _, y := range next(x) {
				if _, ok := m1[y]; ok {
					continue
				}
				if v, ok := m2[y]; ok {
					return step + 1 + v
				}
				if y >= 0 && y <= 1000 {
					m1[y] = step + 1
					q1 = append(q1, y)
				}
			}
		}
		return -1
	}
	for len(q1) > 0 && len(q2) > 0 {
		if len(q1) > len(q2) {
			m1, m2 = m2, m1
			q1, q2 = q2, q1
		}
		t := extend()
		if t != -1 {
			return t
		}
	}
	return -1
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
