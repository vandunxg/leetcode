---
comments: true
difficulty: Medium
rating: 2027
source: Weekly Contest 211 Q3
tags:
    - Array
    - Dynamic Programming
    - Sorting
    - Longest Increasing Subsequence
---

<!-- problem:start -->

# [1626. Best Team With No Conflicts](https://leetcode.com/problems/best-team-with-no-conflicts)

[中文文档](/solution/1600-1699/1626.Best%20Team%20With%20No%20Conflicts/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn là quản lý của một đội bóng rổ. Trong giải đấu sắp tới, bạn muốn chọn đội có tổng điểm cao nhất. Điểm của đội là <strong>tổng</strong> điểm của mọi cầu thủ trong đội.</p>

<p>Tuy nhiên, đội bóng không được có <strong>xung đột</strong>. <strong>Xung đột</strong> xảy ra nếu một cầu thủ trẻ hơn có điểm <strong>lớn hơn nghiêm ngặt</strong> một cầu thủ lớn tuổi hơn. Không có xung đột giữa các cầu thủ cùng tuổi.</p>

<p>Cho hai danh sách <code>scores</code> và <code>ages</code>, trong đó <code>scores[i]</code> và <code>ages[i]</code> lần lượt là điểm và tuổi của cầu thủ thứ <code>i<sup>th</sup></code>, hãy trả về <em><strong>tổng điểm cao nhất</strong> trong mọi đội bóng rổ có thể chọn</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> scores = [1,3,5,10,15], ages = [1,2,3,4,5]
<strong>Output:</strong> 34
<strong>Explanation:</strong>&nbsp;Bạn có thể chọn tất cả cầu thủ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> scores = [4,5,6,5], ages = [2,1,2,1]
<strong>Output:</strong> 16
<strong>Explanation:</strong>&nbsp;Tốt nhất là chọn 3 cầu thủ cuối. Lưu ý rằng được phép chọn nhiều người cùng tuổi.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> scores = [1,2,3,5], ages = [8,9,10,1]
<strong>Output:</strong> 6
<strong>Explanation:</strong>&nbsp;Tốt nhất là chọn 3 cầu thủ đầu tiên. 
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= scores.length, ages.length &lt;= 1000</code></li>
	<li><code>scores.length == ages.length</code></li>
	<li><code>1 &lt;= scores[i] &lt;= 10<sup>6</sup></code></li>
	<li><code>1 &lt;= ages[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Xung đột xảy ra khi cầu thủ lớn tuổi hơn có điểm nhỏ hơn nghiêm ngặt. Sắp xếp theo điểm rồi theo tuổi biến ràng buộc thành việc chọn một dãy con tuổi không giảm.
>
> Với $n \le 1000$, tổng điểm tốt nhất kết thúc tại một cầu thủ có thể tính bằng quy hoạch động kiểu LIS $O(n^2)$: lấy $f[j]$ lớn nhất trong các cầu thủ trước đó không lớn tuổi hơn, rồi cộng điểm hiện tại.
>
> Đáp án là giá trị lớn nhất của $f[i]$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def bestTeamScore(self, scores: List[int], ages: List[int]) -> int:
        arr = sorted(zip(scores, ages))
        n = len(arr)
        f = [0] * n
        for i, (score, age) in enumerate(arr):
            for j in range(i):
                if age >= arr[j][1]:
                    f[i] = max(f[i], f[j])
            f[i] += score
        return max(f)
```

#### Java

```java
class Solution {
    public int bestTeamScore(int[] scores, int[] ages) {
        int n = ages.length;
        int[][] arr = new int[n][2];
        for (int i = 0; i < n; ++i) {
            arr[i] = new int[] {scores[i], ages[i]};
        }
        Arrays.sort(arr, (a, b) -> a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);
        int[] f = new int[n];
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                if (arr[i][1] >= arr[j][1]) {
                    f[i] = Math.max(f[i], f[j]);
                }
            }
            f[i] += arr[i][0];
            ans = Math.max(ans, f[i]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int bestTeamScore(vector<int>& scores, vector<int>& ages) {
        int n = ages.size();
        vector<pair<int, int>> arr(n);
        for (int i = 0; i < n; ++i) {
            arr[i] = {scores[i], ages[i]};
        }
        sort(arr.begin(), arr.end());
        vector<int> f(n);
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                if (arr[i].second >= arr[j].second) {
                    f[i] = max(f[i], f[j]);
                }
            }
            f[i] += arr[i].first;
        }
        return *max_element(f.begin(), f.end());
    }
};
```

#### Go

```go
func bestTeamScore(scores []int, ages []int) int {
	n := len(ages)
	arr := make([][2]int, n)
	for i := range ages {
		arr[i] = [2]int{scores[i], ages[i]}
	}
	sort.Slice(arr, func(i, j int) bool {
		a, b := arr[i], arr[j]
		return a[0] < b[0] || a[0] == b[0] && a[1] < b[1]
	})
	f := make([]int, n)
	for i := range arr {
		for j := 0; j < i; j++ {
			if arr[i][1] >= arr[j][1] {
				f[i] = max(f[i], f[j])
			}
		}
		f[i] += arr[i][0]
	}
	return slices.Max(f)
}
```

#### TypeScript

```ts
function bestTeamScore(scores: number[], ages: number[]): number {
    const arr = ages.map((age, i) => [age, scores[i]]);
    arr.sort((a, b) => (a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]));
    const n = arr.length;
    const f = new Array(n).fill(0);
    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < i; ++j) {
            if (arr[i][1] >= arr[j][1]) {
                f[i] = Math.max(f[i], f[j]);
            }
        }
        f[i] += arr[i][1];
    }
    return Math.max(...f);
}
```

#### JavaScript

```js
/**
 * @param {number[]} scores
 * @param {number[]} ages
 * @return {number}
 */
var bestTeamScore = function (scores, ages) {
    const arr = ages.map((age, i) => [age, scores[i]]);
    arr.sort((a, b) => (a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]));
    const n = arr.length;
    const f = new Array(n).fill(0);
    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < i; ++j) {
            if (arr[i][1] >= arr[j][1]) {
                f[i] = Math.max(f[i], f[j]);
            }
        }
        f[i] += arr[i][1];
    }
    return Math.max(...f);
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 duyệt mọi phần tử trước đó trong $O(n^2)$. Sau cùng cách sắp xếp, ta chỉ cần tổng điểm tốt nhất trong các tuổi không vượt quá tuổi hiện tại, tức là một giá trị lớn nhất trên prefix.
>
> Fenwick tree lưu các giá trị lớn nhất đó: truy vấn $[1,\textit{age}]$, cộng $\textit{score}$ rồi cập nhật tuổi đó, đạt thời gian $O(n\log m)$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class BinaryIndexedTree:
    def __init__(self, n):
        self.n = n
        self.c = [0] * (n + 1)

    def update(self, x, val):
        while x <= self.n:
            self.c[x] = max(self.c[x], val)
            x += x & -x

    def query(self, x):
        s = 0
        while x:
            s = max(s, self.c[x])
            x -= x & -x
        return s


class Solution:
    def bestTeamScore(self, scores: List[int], ages: List[int]) -> int:
        m = max(ages)
        tree = BinaryIndexedTree(m)
        for score, age in sorted(zip(scores, ages)):
            tree.update(age, score + tree.query(age))
        return tree.query(m)
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
            c[x] = Math.max(c[x], val);
            x += x & -x;
        }
    }

    public int query(int x) {
        int s = 0;
        while (x > 0) {
            s = Math.max(s, c[x]);
            x -= x & -x;
        }
        return s;
    }
}

class Solution {
    public int bestTeamScore(int[] scores, int[] ages) {
        int n = ages.length;
        int[][] arr = new int[n][2];
        for (int i = 0; i < n; ++i) {
            arr[i] = new int[] {scores[i], ages[i]};
        }
        Arrays.sort(arr, (a, b) -> a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);
        int m = 0;
        for (int age : ages) {
            m = Math.max(m, age);
        }
        BinaryIndexedTree tree = new BinaryIndexedTree(m);
        for (int[] x : arr) {
            tree.update(x[1], x[0] + tree.query(x[1]));
        }
        return tree.query(m);
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
        while (x) {
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
    int bestTeamScore(vector<int>& scores, vector<int>& ages) {
        int n = ages.size();
        vector<pair<int, int>> arr(n);
        for (int i = 0; i < n; ++i) {
            arr[i] = {scores[i], ages[i]};
        }
        sort(arr.begin(), arr.end());
        int m = *max_element(ages.begin(), ages.end());
        BinaryIndexedTree tree(m);
        for (auto& [score, age] : arr) {
            tree.update(age, score + tree.query(age));
        }
        return tree.query(m);
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
		this.c[x] = max(this.c[x], val)
		x += x & -x
	}
}

func (this *BinaryIndexedTree) query(x int) int {
	s := 0
	for x > 0 {
		s = max(s, this.c[x])
		x -= x & -x
	}
	return s
}

func bestTeamScore(scores []int, ages []int) int {
	n := len(ages)
	arr := make([][2]int, n)
	m := 0
	for i, age := range ages {
		m = max(m, age)
		arr[i] = [2]int{scores[i], age}
	}
	sort.Slice(arr, func(i, j int) bool {
		a, b := arr[i], arr[j]
		return a[0] < b[0] || a[0] == b[0] && a[1] < b[1]
	})
	tree := newBinaryIndexedTree(m)
	for _, x := range arr {
		tree.update(x[1], x[0]+tree.query(x[1]))
	}
	return tree.query(m)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
