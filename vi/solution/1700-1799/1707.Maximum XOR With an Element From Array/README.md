---
comments: true
difficulty: Hard
rating: 2358
source: Weekly Contest 221 Q4
tags:
    - Bit Manipulation
    - Trie
    - Array
---

<!-- problem:start -->

# [1707. Maximum XOR With an Element From Array](https://leetcode.com/problems/maximum-xor-with-an-element-from-array)

[中文文档](/solution/1700-1799/1707.Maximum%20XOR%20With%20an%20Element%20From%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>nums</code> gồm các số nguyên không âm. Đồng thời cho mảng truy vấn <code>queries</code>, trong đó <code>queries[i] = [x<sub>i</sub>, m<sub>i</sub>]</code>.</p>

<p>Đáp án của truy vấn thứ <code>i<sup>th</sup></code> là giá trị <code>XOR</code> bit lớn nhất giữa <code>x<sub>i</sub></code> và một phần tử bất kỳ của <code>nums</code> không vượt quá <code>m<sub>i</sub></code>. Nói cách khác, đáp án là <code>max(nums[j] XOR x<sub>i</sub>)</code> với mọi <code>j</code> sao cho <code>nums[j] &lt;= m<sub>i</sub></code>. Nếu mọi phần tử trong <code>nums</code> đều lớn hơn <code>m<sub>i</sub></code>, đáp án là <code>-1</code>.</p>

<p>Trả về <em>mảng số nguyên </em><code>answer</code><em> sao cho </em><code>answer.length == queries.length</code><em> và </em><code>answer[i]</code><em> là đáp án của truy vấn thứ </em><code>i<sup>th</sup></code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,1,2,3,4], queries = [[3,1],[1,3],[5,6]]
<strong>Đầu ra:</strong> [3,3,7]
<strong>Giải thích:</strong>
1) 0 và 1 là hai số nguyên duy nhất không lớn hơn 1. 0 XOR 3 = 3 và 1 XOR 3 = 2. Giá trị lớn hơn là 3.
2) 1 XOR 2 = 3.
3) 5 XOR 2 = 7.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,2,4,6,6,3], queries = [[12,4],[8,1],[6,3]]
<strong>Đầu ra:</strong> [15,-1,5]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length, queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code>queries[i].length == 2</code></li>
	<li><code>0 &lt;= nums[j], x<sub>i</sub>, m<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Truy vấn offline + Binary Trie

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi truy vấn yêu cầu giá trị lớn nhất của $x_i\oplus nums[j]$ trong các giá trị $\le m_i$. Duyệt mảng cho từng truy vấn có độ phức tạp $O(nq)$ và không đáp ứng được khi $n,q\le 10^5$.
>
> Các truy vấn độc lập với nhau và không phụ thuộc thứ tự của $nums$. Sắp xếp theo $m_i$ cho phép ta lần lượt chèn các số đủ điều kiện vào một cấu trúc.
>
> Sắp xếp $nums$ rồi dùng một con trỏ để chèn các giá trị $\le m_i$ vào binary trie. Khi duyệt trie, ưu tiên bit đối lập sẽ cho XOR lớn nhất; trie rỗng cho đáp án $-1$.

<!-- thinking:end -->

Từ đề bài, ta biết mỗi truy vấn độc lập và kết quả không phụ thuộc thứ tự các phần tử trong $nums$. Vì vậy, ta sắp xếp các truy vấn tăng dần theo $m_i$, đồng thời sắp xếp $nums$ tăng dần.

Tiếp theo, ta dùng binary trie để lưu các phần tử của $nums$. Con trỏ $j$ ghi nhận các phần tử hiện đã được đưa vào trie, ban đầu $j=0$. Với mỗi truy vấn $[x_i, m_i]$, ta liên tục chèn các phần tử của $nums$ vào trie cho đến khi $nums[j] > m_i$. Khi đó, trie chứa mọi phần tử không vượt quá $m_i$, và ta lấy giá trị XOR lớn nhất với $x_i$ làm đáp án.

Độ phức tạp thời gian là $O(m \times \log m + n \times (\log n + \log M))$, còn độ phức tạp không gian là $O(n \times \log M)$. Trong đó, $m$ và $n$ lần lượt là độ dài của $nums$ và $queries$, còn $M$ là giá trị lớn nhất trong mảng $nums$. Trong bài này, $M \le 10^9$.

<!-- tabs:start -->

#### Python3

```python
class Trie:
    __slots__ = ["children"]

    def __init__(self):
        self.children = [None] * 2

    def insert(self, x: int):
        node = self
        for i in range(30, -1, -1):
            v = x >> i & 1
            if node.children[v] is None:
                node.children[v] = Trie()
            node = node.children[v]

    def search(self, x: int) -> int:
        node = self
        ans = 0
        for i in range(30, -1, -1):
            v = x >> i & 1
            if node.children[v ^ 1]:
                ans |= 1 << i
                node = node.children[v ^ 1]
            elif node.children[v]:
                node = node.children[v]
            else:
                return -1
        return ans


class Solution:
    def maximizeXor(self, nums: List[int], queries: List[List[int]]) -> List[int]:
        trie = Trie()
        nums.sort()
        j, n = 0, len(queries)
        ans = [-1] * n
        for i, (x, m) in sorted(zip(range(n), queries), key=lambda x: x[1][1]):
            while j < len(nums) and nums[j] <= m:
                trie.insert(nums[j])
                j += 1
            ans[i] = trie.search(x)
        return ans
```

#### Java

```java
class Trie {
    private Trie[] children = new Trie[2];

    public void insert(int x) {
        Trie node = this;
        for (int i = 30; i >= 0; --i) {
            int v = x >> i & 1;
            if (node.children[v] == null) {
                node.children[v] = new Trie();
            }
            node = node.children[v];
        }
    }

    public int search(int x) {
        Trie node = this;
        int ans = 0;
        for (int i = 30; i >= 0; --i) {
            int v = x >> i & 1;
            if (node.children[v ^ 1] != null) {
                ans |= 1 << i;
                node = node.children[v ^ 1];
            } else if (node.children[v] != null) {
                node = node.children[v];
            } else {
                return -1;
            }
        }
        return ans;
    }
}

class Solution {
    public int[] maximizeXor(int[] nums, int[][] queries) {
        Arrays.sort(nums);
        int n = queries.length;
        Integer[] idx = new Integer[n];
        Arrays.setAll(idx, i -> i);
        Arrays.sort(idx, (i, j) -> queries[i][1] - queries[j][1]);
        int[] ans = new int[n];
        Trie trie = new Trie();
        int j = 0;
        for (int i : idx) {
            int x = queries[i][0], m = queries[i][1];
            while (j < nums.length && nums[j] <= m) {
                trie.insert(nums[j++]);
            }
            ans[i] = trie.search(x);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Trie {
private:
    Trie* children[2];

public:
    Trie()
        : children{nullptr, nullptr} {}

    void insert(int x) {
        Trie* node = this;
        for (int i = 30; ~i; --i) {
            int v = (x >> i) & 1;
            if (!node->children[v]) {
                node->children[v] = new Trie();
            }
            node = node->children[v];
        }
    }

    int search(int x) {
        Trie* node = this;
        int ans = 0;
        for (int i = 30; ~i; --i) {
            int v = (x >> i) & 1;
            if (node->children[v ^ 1]) {
                ans |= 1 << i;
                node = node->children[v ^ 1];
            } else if (node->children[v]) {
                node = node->children[v];
            } else {
                return -1;
            }
        }
        return ans;
    }
};

class Solution {
public:
    vector<int> maximizeXor(vector<int>& nums, vector<vector<int>>& queries) {
        sort(nums.begin(), nums.end());
        int n = queries.size();
        vector<int> idx(n);
        iota(idx.begin(), idx.end(), 0);
        sort(idx.begin(), idx.end(), [&](int i, int j) { return queries[i][1] < queries[j][1]; });
        vector<int> ans(n);
        Trie trie;
        int j = 0;
        for (int i : idx) {
            int x = queries[i][0], m = queries[i][1];
            while (j < nums.size() && nums[j] <= m) {
                trie.insert(nums[j++]);
            }
            ans[i] = trie.search(x);
        }
        return ans;
    }
};
```

#### Go

```go
type Trie struct {
	children [2]*Trie
}

func NewTrie() *Trie {
	return &Trie{}
}

func (t *Trie) insert(x int) {
	node := t
	for i := 30; i >= 0; i-- {
		v := x >> i & 1
		if node.children[v] == nil {
			node.children[v] = NewTrie()
		}
		node = node.children[v]
	}
}

func (t *Trie) search(x int) int {
	node := t
	ans := 0
	for i := 30; i >= 0; i-- {
		v := x >> i & 1
		if node.children[v^1] != nil {
			ans |= 1 << i
			node = node.children[v^1]
		} else if node.children[v] != nil {
			node = node.children[v]
		} else {
			return -1
		}
	}
	return ans
}

func maximizeXor(nums []int, queries [][]int) []int {
	sort.Ints(nums)
	n := len(queries)
	idx := make([]int, n)
	for i := 0; i < n; i++ {
		idx[i] = i
	}
	sort.Slice(idx, func(i, j int) bool {
		return queries[idx[i]][1] < queries[idx[j]][1]
	})
	ans := make([]int, n)
	trie := NewTrie()
	j := 0
	for _, i := range idx {
		x, m := queries[i][0], queries[i][1]
		for j < len(nums) && nums[j] <= m {
			trie.insert(nums[j])
			j++
		}
		ans[i] = trie.search(x)
	}
	return ans
}
```

#### TypeScript

```ts
class Trie {
    children: (Trie | null)[];

    constructor() {
        this.children = [null, null];
    }

    insert(x: number): void {
        let node: Trie | null = this;
        for (let i = 30; ~i; i--) {
            const v = (x >> i) & 1;
            if (node.children[v] === null) {
                node.children[v] = new Trie();
            }
            node = node.children[v] as Trie;
        }
    }

    search(x: number): number {
        let node: Trie | null = this;
        let ans = 0;
        for (let i = 30; ~i; i--) {
            const v = (x >> i) & 1;
            if (node.children[v ^ 1] !== null) {
                ans |= 1 << i;
                node = node.children[v ^ 1] as Trie;
            } else if (node.children[v] !== null) {
                node = node.children[v] as Trie;
            } else {
                return -1;
            }
        }
        return ans;
    }
}

function maximizeXor(nums: number[], queries: number[][]): number[] {
    nums.sort((a, b) => a - b);
    const n = queries.length;
    const idx = Array.from({ length: n }, (_, i) => i);
    idx.sort((i, j) => queries[i][1] - queries[j][1]);
    const ans: number[] = [];
    const trie = new Trie();
    let j = 0;
    for (const i of idx) {
        const x = queries[i][0];
        const m = queries[i][1];
        while (j < nums.length && nums[j] <= m) {
            trie.insert(nums[j++]);
        }
        ans[i] = trie.search(x);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
