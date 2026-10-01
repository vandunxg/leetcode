---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [17.08. Circus Tower](https://leetcode.cn/problems/circus-tower-lcci)

[中文文档](/lcci/17.08.Circus%20Tower/README.md)

## Mô tả

<!-- description:start -->

<p>Một đoàn xiếc đang thiết kế một tiết mục xếp tháp, trong đó mọi người đứng trên vai nhau. Vì lý do thực tế và thẩm mỹ, mỗi người phải vừa thấp hơn vừa nhẹ hơn người ở bên dưới. Cho biết chiều cao và cân nặng của mỗi người trong đoàn xiếc, hãy viết một method để tính số người lớn nhất có thể xếp thành một tòa tháp như vậy.</p>
<p><strong>Ví dụ: </strong></p>
<pre>

<strong>Đầu vào: </strong>height = [65,70,56,75,60,68] weight = [100,150,90,190,95,110]

<strong>Đầu ra: </strong>6

<strong>Giải thích: </strong>Tháp dài nhất có 6 người và gồm những người từ trên xuống dưới: (56,90), (60,95), (65,100), (68,110), (70,150), (75,190)</pre>

<p>Lưu ý:</p>
<ul>
	<li><code>height.length == weight.length &lt;= 10000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Rời rạc hóa + Binary Indexed Tree

<!-- thinking:start -->

> **Tư duy**
>
> Tháp xiếc yêu cầu chiều cao và cân nặng tăng nghiêm ngặt. Việc thử mọi hoán vị là không khả thi; LIS $O(n^2)$ chậm khi $n$ lớn.
>
> Sắp xếp theo chiều cao, và theo cân nặng giảm dần khi chiều cao bằng nhau, sau đó chỉ tìm LIS trên cân nặng. Thứ tự giảm dần này ngăn những người có cùng chiều cao được xếp chồng lên nhau.
>
> Sau khi rời rạc hóa cân nặng, Fenwick tree lưu độ dài chuỗi tốt nhất trong các cân nặng nhỏ hơn. Query $[1,id(w)-1]$, sau đó update $id(w)$.

<!-- thinking:end -->

Trước tiên, chúng ta sắp xếp tất cả mọi người theo chiều cao tăng dần. Nếu chiều cao bằng nhau, chúng ta sắp xếp theo cân nặng giảm dần. Nhờ đó, chúng ta có thể chuyển bài toán thành tìm dãy con tăng dài nhất của mảng cân nặng.

Bài toán dãy con tăng dài nhất có thể được giải bằng quy hoạch động với độ phức tạp thời gian $O(n^2)$. Tuy nhiên, chúng ta có thể tối ưu quá trình giải bằng Binary Indexed Tree, giúp giảm độ phức tạp thời gian xuống còn $O(n \log n)$.

Độ phức tạp không gian là $O(n)$, trong đó $n$ là số người.

<!-- tabs:start -->

#### Python3

```python
class BinaryIndexedTree:
    def __init__(self, n):
        self.n = n
        self.c = [0] * (n + 1)

    def update(self, x, delta):
        while x <= self.n:
            self.c[x] = max(self.c[x], delta)
            x += x & -x

    def query(self, x):
        s = 0
        while x:
            s = max(s, self.c[x])
            x -= x & -x
        return s


class Solution:
    def bestSeqAtIndex(self, height: List[int], weight: List[int]) -> int:
        arr = list(zip(height, weight))
        arr.sort(key=lambda x: (x[0], -x[1]))
        alls = sorted({w for _, w in arr})
        m = {w: i for i, w in enumerate(alls, 1)}
        tree = BinaryIndexedTree(len(m))
        ans = 1
        for _, w in arr:
            x = m[w]
            t = tree.query(x - 1) + 1
            ans = max(ans, t)
            tree.update(x, t)
        return ans
```

#### Java

```java
class BinaryIndexedTree {
    private int n;
    private int[] c;

    public BinaryIndexedTree(int n) {
        this.n = n;
        c = new int[n + 1];
    }

    public void update(int x, int val) {
        while (x <= n) {
            this.c[x] = Math.max(this.c[x], val);
            x += x & -x;
        }
    }

    public int query(int x) {
        int s = 0;
        while (x > 0) {
            s = Math.max(s, this.c[x]);
            x -= x & -x;
        }
        return s;
    }
}

class Solution {
    public int bestSeqAtIndex(int[] height, int[] weight) {
        int n = height.length;
        int[][] arr = new int[n][2];
        for (int i = 0; i < n; ++i) {
            arr[i] = new int[] {height[i], weight[i]};
        }
        Arrays.sort(arr, (a, b) -> a[0] == b[0] ? b[1] - a[1] : a[0] - b[0]);
        Set<Integer> s = new HashSet<>();
        for (int[] e : arr) {
            s.add(e[1]);
        }
        List<Integer> alls = new ArrayList<>(s);
        Collections.sort(alls);
        Map<Integer, Integer> m = new HashMap<>(alls.size());
        for (int i = 0; i < alls.size(); ++i) {
            m.put(alls.get(i), i + 1);
        }
        BinaryIndexedTree tree = new BinaryIndexedTree(alls.size());
        int ans = 1;
        for (int[] e : arr) {
            int x = m.get(e[1]);
            int t = tree.query(x - 1) + 1;
            ans = Math.max(ans, t);
            tree.update(x, t);
        }
        return ans;
    }
}
```

#### C++

```cpp
class BinaryIndexedTree {
public:
    BinaryIndexedTree(int _n)
        : n(_n)
        , c(_n + 1) {}

    void update(int x, int val) {
        while (x <= n) {
            c[x] = max(c[x], val);
            x += x & -x;
        }
    }

    int query(int x) {
        int s = 0;
        while (x > 0) {
            s = max(s, c[x]);
            x -= x & -x;
        }
        return s;
    }

private:
    int n;
    vector<int> c;
};

class Solution {
public:
    int bestSeqAtIndex(vector<int>& height, vector<int>& weight) {
        int n = height.size();
        vector<pair<int, int>> people;
        for (int i = 0; i < n; ++i) {
            people.emplace_back(height[i], weight[i]);
        }
        sort(people.begin(), people.end(), [](const pair<int, int>& a, const pair<int, int>& b) {
            if (a.first == b.first) {
                return a.second > b.second;
            }
            return a.first < b.first;
        });
        vector<int> alls = weight;
        sort(alls.begin(), alls.end());
        alls.erase(unique(alls.begin(), alls.end()), alls.end());
        BinaryIndexedTree tree(alls.size());
        int ans = 1;
        for (auto& [_, w] : people) {
            int x = lower_bound(alls.begin(), alls.end(), w) - alls.begin() + 1;
            int t = tree.query(x - 1) + 1;
            ans = max(ans, t);
            tree.update(x, t);
        }
        return ans;
    }
};
```

#### Go

```go
type BinaryIndexedTree struct {
	n int
	c []int
}

func newBinaryIndexedTree(n int) *BinaryIndexedTree {
	c := make([]int, n+1)
	return &BinaryIndexedTree{n, c}
}

func (this *BinaryIndexedTree) update(x, val int) {
	for x <= this.n {
		if this.c[x] < val {
			this.c[x] = val
		}
		x += x & -x
	}
}

func (this *BinaryIndexedTree) query(x int) int {
	s := 0
	for x > 0 {
		if s < this.c[x] {
			s = this.c[x]
		}
		x -= x & -x
	}
	return s
}

func bestSeqAtIndex(height []int, weight []int) int {
	n := len(height)
	people := make([][2]int, n)
	s := map[int]bool{}
	for i := range people {
		people[i] = [2]int{height[i], weight[i]}
		s[weight[i]] = true
	}
	sort.Slice(people, func(i, j int) bool {
		a, b := people[i], people[j]
		return a[0] < b[0] || a[0] == b[0] && a[1] > b[1]
	})
	alls := make([]int, 0, len(s))
	for k := range s {
		alls = append(alls, k)
	}
	sort.Ints(alls)
	tree := newBinaryIndexedTree(len(alls))
	ans := 1
	for _, p := range people {
		x := sort.SearchInts(alls, p[1]) + 1
		t := tree.query(x-1) + 1
		ans = max(ans, t)
		tree.update(x, t)
	}
	return ans
}
```

#### Swift

```swift
class BinaryIndexedTree {
    private var n: Int
    private var c: [Int]

    init(_ n: Int) {
        self.n = n
        self.c = [Int](repeating: 0, count: n + 1)
    }

    func update(_ x: Int, _ val: Int) {
        var x = x
        while x <= n {
            c[x] = max(c[x], val)
            x += x & -x
        }
    }

    func query(_ x: Int) -> Int {
        var x = x
        var s = 0
        while x > 0 {
            s = max(s, c[x])
            x -= x & -x
        }
        return s
    }
}

class Solution {
    func bestSeqAtIndex(_ height: [Int], _ weight: [Int]) -> Int {
        let n = height.count
        var arr: [(Int, Int)] = []
        for i in 0..<n {
            arr.append((height[i], weight[i]))
        }
        arr.sort {
            if $0.0 == $1.0 {
                return $1.1 < $0.1
            }
            return $0.0 < $1.0
        }

        let weights = Set(arr.map { $1 })
        let sortedWeights = Array(weights).sorted()
        let m = sortedWeights.enumerated().reduce(into: [Int: Int]()) {
            $0[$1.element] = $1.offset + 1
        }

        let tree = BinaryIndexedTree(sortedWeights.count)
        var ans = 1
        for (_, w) in arr {
            let x = m[w]!
            let t = tree.query(x - 1) + 1
            ans = max(ans, t)
            tree.update(x, t)
        }
        return ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
