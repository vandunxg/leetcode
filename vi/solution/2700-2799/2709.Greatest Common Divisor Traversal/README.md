---
comments: true
difficulty: Hard
rating: 2171
source: Biweekly Contest 105 Q4
tags:
    - Union Find
    - Array
    - Math
    - Greatest Common Divisor
    - Number Theory
    - Prime Factorization
    - Euclidean Algorithm
---

<!-- problem:start -->

# [2709. Greatest Common Divisor Traversal](https://leetcode.com/problems/greatest-common-divisor-traversal)

[中文文档](/solution/2700-2799/2709.Greatest%20Common%20Divisor%20Traversal/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums</code>, và được phép <strong>di chuyển</strong> giữa các chỉ số của mảng. Bạn có thể di chuyển giữa chỉ số <code>i</code> và chỉ số <code>j</code>, <code>i != j</code>, khi và chỉ khi <code>gcd(nums[i], nums[j]) &gt; 1</code>, trong đó <code>gcd</code> là <strong>ước chung lớn nhất</strong>.</p>

<p>Nhiệm vụ của bạn là xác định xem với <strong>mọi cặp</strong> chỉ số <code>i</code> và <code>j</code> trong nums, với <code>i &lt; j</code>, có tồn tại <strong>một chuỗi các lần di chuyển</strong> đưa ta từ <code>i</code> đến <code>j</code> hay không.</p>

<p>Trả về <code>true</code><em> nếu có thể di chuyển giữa mọi cặp chỉ số như vậy,</em><em> hoặc </em><code>false</code><em> nếu ngược lại.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,3,6]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Trong ví dụ này, có 3 cặp chỉ số có thể có: (0, 1), (0, 2) và (1, 2).
Để đi từ chỉ số 0 đến chỉ số 1, ta có thể sử dụng chuỗi di chuyển 0 -&gt; 2 -&gt; 1. Ta di chuyển từ chỉ số 0 đến chỉ số 2 vì gcd(nums[0], nums[2]) = gcd(2, 6) = 2 &gt; 1, sau đó di chuyển từ chỉ số 2 đến chỉ số 1 vì gcd(nums[2], nums[1]) = gcd(6, 3) = 3 &gt; 1.
Để đi từ chỉ số 0 đến chỉ số 2, ta có thể đi trực tiếp vì gcd(nums[0], nums[2]) = gcd(2, 6) = 2 &gt; 1. Tương tự, để đi từ chỉ số 1 đến chỉ số 2, ta có thể đi trực tiếp vì gcd(nums[1], nums[2]) = gcd(3, 6) = 3 &gt; 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,9,5]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Trong ví dụ này, không có chuỗi di chuyển nào có thể đưa ta từ chỉ số 0 đến chỉ số 2. Vì vậy, ta trả về false.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,3,12,8]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Có 6 cặp chỉ số có thể di chuyển giữa: (0, 1), (0, 2), (0, 3), (1, 2), (1, 3) và (2, 3). Với mỗi cặp đều tồn tại một chuỗi di chuyển hợp lệ, nên ta trả về true.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Hai chỉ số $i$ và $j$ kề nhau khi và chỉ khi $\gcd(nums[i], nums[j])>1$, và ta cần xác định xem toàn bộ đồ thị có liên thông hay không. Vì cả $n$ và giới hạn giá trị đều là $10^5$, việc tính gcd cho từng cặp là không thể thực hiện được.
>
> Các chỉ số có chung một thừa số nguyên tố nằm trong cùng một thành phần. Sau khi liệt kê các thừa số nguyên tố, ta hợp nhất chỉ số $i$ với một node ảo tương ứng với mỗi thừa số $p$. Nếu mọi chỉ số đều có chung một root, mọi cặp chỉ số đều có thể đi đến nhau.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class UnionFind:
    def __init__(self, n):
        self.p = list(range(n))
        self.size = [1] * n

    def find(self, x):
        if self.p[x] != x:
            self.p[x] = self.find(self.p[x])
        return self.p[x]

    def union(self, a, b):
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


mx = 100010
p = defaultdict(list)
for x in range(1, mx + 1):
    v = x
    i = 2
    while i <= v // i:
        if v % i == 0:
            p[x].append(i)
            while v % i == 0:
                v //= i
        i += 1
    if v > 1:
        p[x].append(v)


class Solution:
    def canTraverseAllPairs(self, nums: List[int]) -> bool:
        n = len(nums)
        m = max(nums)
        uf = UnionFind(n + m + 1)
        for i, x in enumerate(nums):
            for j in p[x]:
                uf.union(i, j + n)
        return len(set(uf.find(i) for i in range(n))) == 1
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
    private static final int MX = 100010;
    private static final List<Integer>[] P = new List[MX];

    static {
        Arrays.setAll(P, k -> new ArrayList<>());
        for (int x = 1; x < MX; ++x) {
            int v = x;
            int i = 2;
            while (i <= v / i) {
                if (v % i == 0) {
                    P[x].add(i);
                    while (v % i == 0) {
                        v /= i;
                    }
                }
                ++i;
            }
            if (v > 1) {
                P[x].add(v);
            }
        }
    }

    public boolean canTraverseAllPairs(int[] nums) {
        int m = Arrays.stream(nums).max().getAsInt();
        int n = nums.length;
        UnionFind uf = new UnionFind(n + m + 1);
        for (int i = 0; i < n; ++i) {
            for (int j : P[nums[i]]) {
                uf.union(i, j + n);
            }
        }
        Set<Integer> s = new HashSet<>();
        for (int i = 0; i < n; ++i) {
            s.add(uf.find(i));
        }
        return s.size() == 1;
    }
}
```

#### C++

```cpp
int MX = 100010;
vector<int> P[100010];

int init = []() {
    for (int x = 1; x < MX; ++x) {
        int v = x;
        int i = 2;
        while (i <= v / i) {
            if (v % i == 0) {
                P[x].push_back(i);
                while (v % i == 0) {
                    v /= i;
                }
            }
            ++i;
        }
        if (v > 1) {
            P[x].push_back(v);
        }
    }
    return 0;
}();

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
    bool canTraverseAllPairs(vector<int>& nums) {
        int m = *max_element(nums.begin(), nums.end());
        int n = nums.size();
        UnionFind uf(m + n + 1);
        for (int i = 0; i < n; ++i) {
            for (int j : P[nums[i]]) {
                uf.unite(i, j + n);
            }
        }
        unordered_set<int> s;
        for (int i = 0; i < n; ++i) {
            s.insert(uf.find(i));
        }
        return s.size() == 1;
    }
};
```

#### Go

```go
const mx = 100010

var p = make([][]int, mx)

func init() {
	for x := 1; x < mx; x++ {
		v := x
		i := 2
		for i <= v/i {
			if v%i == 0 {
				p[x] = append(p[x], i)
				for v%i == 0 {
					v /= i
				}
			}
			i++
		}
		if v > 1 {
			p[x] = append(p[x], v)
		}
	}
}

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

func canTraverseAllPairs(nums []int) bool {
	m := slices.Max(nums)
	n := len(nums)
	uf := newUnionFind(m + n + 1)
	for i, x := range nums {
		for _, j := range p[x] {
			uf.union(i, j+n)
		}
	}
	s := map[int]bool{}
	for i := 0; i < n; i++ {
		s[uf.find(i)] = true
	}
	return len(s) == 1
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
