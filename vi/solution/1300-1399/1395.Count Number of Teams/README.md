---
comments: true
difficulty: Medium
rating: 1343
source: Weekly Contest 182 Q2
tags:
    - Binary Indexed Tree
    - Segment Tree
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [1395. Count Number of Teams](https://leetcode.com/problems/count-number-of-teams)

[中文文档](/solution/1300-1399/1395.Count%20Number%20of%20Teams/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> người lính đứng thành một hàng. Mỗi người được gán một giá trị <code>rating</code> <strong>duy nhất</strong>.</p>

<p>Bạn cần chọn một đội gồm 3 người lính theo các quy tắc sau:</p>

<ul>
	<li>Chọn 3 người lính có chỉ số (<code>i</code>, <code>j</code>, <code>k</code>) và rating tương ứng là (<code>rating[i]</code>, <code>rating[j]</code>, <code>rating[k]</code>).</li>
	<li>Một đội hợp lệ nếu (<code>rating[i] &lt; rating[j] &lt; rating[k]</code>) hoặc (<code>rating[i] &gt; rating[j] &gt; rating[k]</code>), với (<code>0 &lt;= i &lt; j &lt; k &lt; n</code>).</li>
</ul>

<p>Trả về số đội có thể lập theo các điều kiện trên. (Một người lính có thể thuộc nhiều đội.)</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> rating = [2,5,3,4,1]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Có thể lập ba đội thỏa mãn các điều kiện: (2,3,4), (5,4,1), (5,3,1). 
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> rating = [2,1,3]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không thể lập đội nào thỏa mãn các điều kiện.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> rating = [1,2,3,4]
<strong>Đầu ra:</strong> 4
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == rating.length</code></li>
	<li><code>3 &lt;= n &lt;= 1000</code></li>
	<li><code>1 &lt;= rating[i] &lt;= 10<sup>5</sup></code></li>
	<li>Tất cả số nguyên trong <code>rating</code> đều <strong>duy nhất</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt phần tử ở giữa

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các bộ ba tăng hoặc giảm nghiêm ngặt. Vì $n \le 1000$, dùng ba vòng lặp sẽ có độ phức tạp bậc ba. Cố định rating $b$ ở giữa: tích của $l$ giá trị nhỏ hơn ở bên trái và $r$ giá trị lớn hơn ở bên phải là số đội tăng; các số đếm bổ sung tương ứng cho số đội giảm. Với mỗi phần tử ở giữa, chỉ cần duyệt tuyến tính hai phía.

<!-- thinking:end -->

Ta lần lượt chọn mỗi phần tử $rating[i]$ trong mảng $rating$ làm phần tử ở giữa, rồi đếm số phần tử $l$ nhỏ hơn nó ở bên trái và số phần tử $r$ lớn hơn nó ở bên phải. Số đội có phần tử này ở giữa là $l \times r + (i - l) \times (n - i - 1 - r)$. Cộng số này vào đáp án.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là độ dài của mảng $rating$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numTeams(self, rating: List[int]) -> int:
        ans, n = 0, len(rating)
        for i, b in enumerate(rating):
            l = sum(a < b for a in rating[:i])
            r = sum(c > b for c in rating[i + 1 :])
            ans += l * r
            ans += (i - l) * (n - i - 1 - r)
        return ans
```

#### Java

```java
class Solution {
    public int numTeams(int[] rating) {
        int n = rating.length;
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int l = 0, r = 0;
            for (int j = 0; j < i; ++j) {
                if (rating[j] < rating[i]) {
                    ++l;
                }
            }
            for (int j = i + 1; j < n; ++j) {
                if (rating[j] > rating[i]) {
                    ++r;
                }
            }
            ans += l * r;
            ans += (i - l) * (n - i - 1 - r);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numTeams(vector<int>& rating) {
        int n = rating.size();
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int l = 0, r = 0;
            for (int j = 0; j < i; ++j) {
                if (rating[j] < rating[i]) {
                    ++l;
                }
            }
            for (int j = i + 1; j < n; ++j) {
                if (rating[j] > rating[i]) {
                    ++r;
                }
            }
            ans += l * r;
            ans += (i - l) * (n - i - 1 - r);
        }
        return ans;
    }
};
```

#### Go

```go
func numTeams(rating []int) (ans int) {
	n := len(rating)
	for i, b := range rating {
		l, r := 0, 0
		for _, a := range rating[:i] {
			if a < b {
				l++
			}
		}
		for _, c := range rating[i+1:] {
			if c < b {
				r++
			}
		}
		ans += l * r
		ans += (i - l) * (n - i - 1 - r)
	}
	return
}
```

#### TypeScript

```ts
function numTeams(rating: number[]): number {
    let ans = 0;
    const n = rating.length;
    for (let i = 0; i < n; ++i) {
        let l = 0;
        let r = 0;
        for (let j = 0; j < i; ++j) {
            if (rating[j] < rating[i]) {
                ++l;
            }
        }
        for (let j = i + 1; j < n; ++j) {
            if (rating[j] > rating[i]) {
                ++r;
            }
        }
        ans += l * r;
        ans += (i - l) * (n - i - 1 - r);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Binary Indexed Tree

<!-- thinking:start -->

> **Tư duy**
>
> Duyệt hai phía cho mỗi phần tử ở giữa tốn $O(n^2)$. Sau khi rời rạc hóa giá trị, hai Fenwick tree có thể truy vấn “có bao nhiêu giá trị nhỏ hơn ở bên trái / lớn hơn ở bên phải” trong $O(\log n)$: ban đầu mọi giá trị nằm trong tree bên phải, rồi lần lượt được chuyển sang tree bên trái khi $i$ tăng.

<!-- thinking:end -->

Ta có thể dùng hai binary indexed tree để duy trì số phần tử $l$ nhỏ hơn phần tử hiện tại ở bên trái trong mảng $rating$, và số phần tử $r$ lớn hơn nó ở bên phải. Số đội có phần tử hiện tại ở giữa là $l \times r + (i - l) \times (n - i - 1 - r)$; cộng số này vào đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $rating$.

<!-- tabs:start -->

#### Python3

```python
class BinaryIndexedTree:
    def __init__(self, n: int):
        self.n = n
        self.c = [0] * (n + 1)

    def update(self, x: int, v: int):
        while x <= self.n:
            self.c[x] += v
            x += x & -x

    def query(self, x: int) -> int:
        s = 0
        while x:
            s += self.c[x]
            x -= x & -x
        return s


class Solution:
    def numTeams(self, rating: List[int]) -> int:
        nums = sorted(set(rating))
        m = len(nums)
        tree1 = BinaryIndexedTree(m)
        tree2 = BinaryIndexedTree(m)
        for v in rating:
            x = bisect_left(nums, v) + 1
            tree2.update(x, 1)
        n = len(rating)
        ans = 0
        for i, v in enumerate(rating):
            x = bisect_left(nums, v) + 1
            tree1.update(x, 1)
            tree2.update(x, -1)
            l = tree1.query(x - 1)
            r = n - i - 1 - tree2.query(x)
            ans += l * r
            ans += (i - l) * (n - i - 1 - r)
        return ans
```

#### Java

```java
class BinaryIndexedTree {
    private int n;
    private int[] c;

    public BinaryIndexedTree(int n) {
        this.n = n;
        this.c = new int[n + 1];
    }

    public void update(int x, int v) {
        while (x <= n) {
            c[x] += v;
            x += x & -x;
        }
    }

    public int query(int x) {
        int s = 0;
        while (x > 0) {
            s += c[x];
            x -= x & -x;
        }
        return s;
    }
}

class Solution {
    public int numTeams(int[] rating) {
        int n = rating.length;
        int[] nums = rating.clone();
        Arrays.sort(nums);
        int m = 0;
        for (int i = 0; i < n; ++i) {
            if (i == 0 || nums[i] != nums[i - 1]) {
                nums[m++] = nums[i];
            }
        }
        BinaryIndexedTree tree1 = new BinaryIndexedTree(m);
        BinaryIndexedTree tree2 = new BinaryIndexedTree(m);
        for (int v : rating) {
            int x = search(nums, v);
            tree2.update(x, 1);
        }

        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int x = search(nums, rating[i]);
            tree1.update(x, 1);
            tree2.update(x, -1);
            int l = tree1.query(x - 1);
            int r = n - i - 1 - tree2.query(x);
            ans += l * r;
            ans += (i - l) * (n - i - 1 - r);
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
        return l + 1;
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

    void update(int x, int delta) {
        while (x <= n) {
            c[x] += delta;
            x += x & -x;
        }
    }

    int query(int x) {
        int s = 0;
        while (x) {
            s += c[x];
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
    int numTeams(vector<int>& rating) {
        vector<int> nums = rating;
        sort(nums.begin(), nums.end());
        nums.erase(unique(nums.begin(), nums.end()), nums.end());
        int m = nums.size();
        BinaryIndexedTree tree1(m);
        BinaryIndexedTree tree2(m);
        for (int& v : rating) {
            int x = lower_bound(nums.begin(), nums.end(), v) - nums.begin() + 1;
            tree2.update(x, 1);
        }
        int ans = 0;
        int n = rating.size();
        for (int i = 0; i < n; ++i) {
            int x = lower_bound(nums.begin(), nums.end(), rating[i]) - nums.begin() + 1;
            tree1.update(x, 1);
            tree2.update(x, -1);
            int l = tree1.query(x - 1);
            int r = n - i - 1 - tree2.query(x);
            ans += l * r;
            ans += (i - l) * (n - i - 1 - r);
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

func (this *BinaryIndexedTree) update(x, delta int) {
	for x <= this.n {
		this.c[x] += delta
		x += x & -x
	}
}

func (this *BinaryIndexedTree) query(x int) int {
	s := 0
	for x > 0 {
		s += this.c[x]
		x -= x & -x
	}
	return s
}

func numTeams(rating []int) (ans int) {
	nums := make([]int, len(rating))
	copy(nums, rating)
	sort.Ints(nums)
	m := 0
	for i, x := range nums {
		if i == 0 || x != nums[i-1] {
			nums[m] = x
			m++
		}
	}
	nums = nums[:m]
	tree1 := newBinaryIndexedTree(m)
	tree2 := newBinaryIndexedTree(m)
	for _, x := range rating {
		tree2.update(sort.SearchInts(nums, x)+1, 1)
	}
	n := len(rating)
	for i, v := range rating {
		x := sort.SearchInts(nums, v) + 1
		tree1.update(x, 1)
		tree2.update(x, -1)
		l := tree1.query(x - 1)
		r := n - i - 1 - tree2.query(x)
		ans += l * r
		ans += (i - l) * (n - i - 1 - r)
	}
	return
}
```

#### TypeScript

```ts
class BinaryIndexedTree {
    private n: number;
    private c: number[];

    constructor(n: number) {
        this.n = n;
        this.c = new Array(n + 1).fill(0);
    }

    public update(x: number, v: number): void {
        while (x <= this.n) {
            this.c[x] += v;
            x += x & -x;
        }
    }

    public query(x: number): number {
        let s = 0;
        while (x > 0) {
            s += this.c[x];
            x -= x & -x;
        }
        return s;
    }
}

function numTeams(rating: number[]): number {
    let nums = [...rating];
    nums.sort((a, b) => a - b);
    const n = rating.length;
    let m = 0;
    for (let i = 0; i < n; ++i) {
        if (i === 0 || nums[i] !== nums[i - 1]) {
            nums[m++] = nums[i];
        }
    }
    nums = nums.slice(0, m);
    const search = (x: number): number => {
        let l = 0;
        let r = m;
        while (l < r) {
            const mid = (l + r) >> 1;
            if (nums[mid] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l + 1;
    };
    let ans = 0;
    const tree1 = new BinaryIndexedTree(m);
    const tree2 = new BinaryIndexedTree(m);
    for (const x of rating) {
        tree2.update(search(x), 1);
    }
    for (let i = 0; i < n; ++i) {
        const x = search(rating[i]);
        tree1.update(x, 1);
        tree2.update(x, -1);
        const l = tree1.query(x - 1);
        const r = n - i - 1 - tree2.query(x);
        ans += l * r;
        ans += (i - l) * (n - i - 1 - r);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3: Đệ quy + Memoization

<!-- thinking:start -->

> **Tư duy**
>
> Fenwick tree cần rời rạc hóa giá trị. Ta memoize theo chỉ số, số phần tử còn cần chọn và chiều tăng/giảm để đếm số cách hoàn tất. Từ $i$, thử mọi $j$ đứng sau thỏa mãn thứ tự. Có $O(n)$ trạng thái và $O(n)$ chuyển tiếp, nên độ phức tạp tương đương phương pháp đầu tiên nhưng được biểu diễn bằng đệ quy.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function numTeams(rating: number[]): number {
    const n = rating.length;
    const f: Record<Type, number[][]> = {
        asc: Array.from({ length: n }, () => Array(3).fill(-1)),
        desc: Array.from({ length: n }, () => Array(3).fill(-1)),
    };

    const fn = (i: number, available: number, type: Type) => {
        if (!available) {
            return 1;
        }
        if (f[type][i][available] !== -1) {
            return f[type][i][available];
        }

        let ans = 0;
        for (let j = i + 1; j < n; j++) {
            if (rating[j] > rating[i]) {
                if (type === 'asc') {
                    ans += fn(j, available - 1, 'asc');
                }
            } else {
                if (type === 'desc') {
                    ans += fn(j, available - 1, 'desc');
                }
            }
        }
        f[type][i][available] = ans;

        return ans;
    };

    let ans = 0;
    for (let i = 0; i < n; i++) {
        ans += fn(i, 2, 'asc') + fn(i, 2, 'desc');
    }

    return ans;
}

type Type = 'asc' | 'desc';
```

#### JavaScript

```js
/**
 * @param {number[]} rating
 * @return {number}
 */
var numTeams = function (rating) {
    const n = rating.length;
    const f = {
        asc: Array.from({ length: n }, () => Array(3).fill(-1)),
        desc: Array.from({ length: n }, () => Array(3).fill(-1)),
    };

    const fn = (i, available, type) => {
        if (!available) {
            return 1;
        }
        if (f[type][i][available] !== -1) {
            return f[type][i][available];
        }

        let ans = 0;
        for (let j = i + 1; j < n; j++) {
            if (rating[j] > rating[i]) {
                if (type === 'asc') {
                    ans += fn(j, available - 1, 'asc');
                }
            } else {
                if (type === 'desc') {
                    ans += fn(j, available - 1, 'desc');
                }
            }
        }
        f[type][i][available] = ans;

        return ans;
    };

    let ans = 0;
    for (let i = 0; i < n; i++) {
        ans += fn(i, 2, 'asc') + fn(i, 2, 'desc');
    }

    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
