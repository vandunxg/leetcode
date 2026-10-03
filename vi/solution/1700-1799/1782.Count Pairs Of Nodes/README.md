---
comments: true
difficulty: Hard
rating: 2457
source: Biweekly Contest 47 Q4
tags:
    - Graph
    - Array
    - Hash Table
    - Two Pointers
    - Binary Search
    - Counting
    - Sorting
---

<!-- problem:start -->

# [1782. Count Pairs Of Nodes](https://leetcode.com/problems/count-pairs-of-nodes)

[中文文档](/solution/1700-1799/1782.Count%20Pairs%20Of%20Nodes/README.md)

## Mô tả

<!-- description:start -->

<p>Cho đồ thị vô hướng gồm số nguyên <code>n</code> là số nút và mảng số nguyên hai chiều <code>edges</code> là các cạnh, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> cho biết có một cạnh <strong>vô hướng</strong> giữa <code>u<sub>i</sub></code> và <code>v<sub>i</sub></code>. Đồng thời cho mảng số nguyên <code>queries</code>.</p>

<p>Định nghĩa <code>incident(a, b)</code> là <strong>số cạnh</strong> nối với <strong>một trong hai</strong> nút <code>a</code> hoặc <code>b</code>.</p>

<p>Đáp án của truy vấn thứ <code>j<sup>th</sup></code> là <strong>số cặp</strong> nút <code>(a, b)</code> thỏa mãn <strong>cả hai</strong> điều kiện:</p>

<ul>
	<li><code>a &lt; b</code></li>
	<li><code>incident(a, b) &gt; queries[j]</code></li>
</ul>

<p>Trả về <em>mảng </em><code>answers</code><em> sao cho </em><code>answers.length == queries.length</code><em> và </em><code>answers[j]</code><em> là đáp án của truy vấn thứ </em><code>j<sup>th</sup></code><em>.</em></p>

<p>Lưu ý rằng có thể có <strong>nhiều cạnh</strong> giữa cùng một cặp nút.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1700-1799/1782.Count%20Pairs%20Of%20Nodes/images/winword_2021-06-08_00-58-39.png" style="width: 529px; height: 305px;" />
<pre>
<strong>Đầu vào:</strong> n = 4, edges = [[1,2],[2,4],[1,3],[2,3],[2,1]], queries = [2,3]
<strong>Đầu ra:</strong> [6,5]
<strong>Giải thích:</strong> Các phép tính incident(a, b) được trình bày trong bảng ở trên.
Đáp án cho từng truy vấn như sau:
- answers[0] = 6. Tất cả các cặp đều có giá trị incident(a, b) lớn hơn 2.
- answers[1] = 5. Tất cả các cặp trừ (3, 4) đều có giá trị incident(a, b) lớn hơn 3.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5, edges = [[1,5],[1,5],[3,4],[2,5],[1,3],[5,1],[2,3],[2,5]], queries = [1,2,3,4,5]
<strong>Đầu ra:</strong> [10,10,9,8,6]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= edges.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt;= n</code></li>
	<li><code>u<sub>i </sub>!= v<sub>i</sub></code></li>
	<li><code>1 &lt;= queries.length &lt;= 20</code></li>
	<li><code>0 &lt;= queries[j] &lt; edges.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Sắp xếp + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Số cạnh liên thuộc của $(a,b)$ là $\deg(a)+\deg(b)$ trừ số cạnh lặp giữa chúng. Nhiều truy vấn và $n$ có kích thước vừa khiến việc liệt kê mọi cặp cho từng truy vấn không khả thi.
>
> Sắp xếp các bậc và với mỗi $a$, dùng tìm kiếm nhị phân để tìm số $b$ thỏa $\deg(a)+\deg(b)>q$. Sau đó trừ các cặp chỉ vượt $q$ trước khi loại các cạnh chung.

<!-- thinking:end -->

Từ đề bài, ta biết số cạnh nối với cặp điểm $(a, b)$ bằng "số cạnh nối với $a$" cộng "số cạnh nối với $b$", rồi trừ số cạnh nối với cả $a$ và $b$.

Do đó, trước tiên ta dùng mảng $cnt$ để đếm số cạnh nối với mỗi điểm, và dùng hash table $g$ để đếm số cạnh của từng cặp điểm.

Sau đó, với mỗi truy vấn $q$, ta liệt kê $a$. Với mỗi $a$, dùng tìm kiếm nhị phân để tìm $b$ đầu tiên thỏa $cnt[a] + cnt[b] > q$, cộng số lượng vào đáp án truy vấn hiện tại, rồi trừ các cạnh trùng.

Độ phức tạp thời gian là $O(q \times (n \times \log n + m))$, còn độ phức tạp không gian là $O(n + m)$. Trong đó, $n$ và $m$ lần lượt là số điểm và số cạnh, còn $q$ là số truy vấn.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countPairs(
        self, n: int, edges: List[List[int]], queries: List[int]
    ) -> List[int]:
        cnt = [0] * n
        g = defaultdict(int)
        for a, b in edges:
            a, b = a - 1, b - 1
            a, b = min(a, b), max(a, b)
            cnt[a] += 1
            cnt[b] += 1
            g[(a, b)] += 1

        s = sorted(cnt)
        ans = [0] * len(queries)
        for i, t in enumerate(queries):
            for j, x in enumerate(s):
                k = bisect_right(s, t - x, lo=j + 1)
                ans[i] += n - k
            for (a, b), v in g.items():
                if cnt[a] + cnt[b] > t and cnt[a] + cnt[b] - v <= t:
                    ans[i] -= 1
        return ans
```

#### Java

```java
class Solution {
    public int[] countPairs(int n, int[][] edges, int[] queries) {
        int[] cnt = new int[n];
        Map<Integer, Integer> g = new HashMap<>();
        for (var e : edges) {
            int a = e[0] - 1, b = e[1] - 1;
            ++cnt[a];
            ++cnt[b];
            int k = Math.min(a, b) * n + Math.max(a, b);
            g.merge(k, 1, Integer::sum);
        }
        int[] s = cnt.clone();
        Arrays.sort(s);
        int[] ans = new int[queries.length];
        for (int i = 0; i < queries.length; ++i) {
            int t = queries[i];
            for (int j = 0; j < n; ++j) {
                int x = s[j];
                int k = search(s, t - x, j + 1);
                ans[i] += n - k;
            }
            for (var e : g.entrySet()) {
                int a = e.getKey() / n, b = e.getKey() % n;
                int v = e.getValue();
                if (cnt[a] + cnt[b] > t && cnt[a] + cnt[b] - v <= t) {
                    --ans[i];
                }
            }
        }
        return ans;
    }

    private int search(int[] arr, int x, int i) {
        int left = i, right = arr.length;
        while (left < right) {
            int mid = (left + right) >> 1;
            if (arr[mid] > x) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> countPairs(int n, vector<vector<int>>& edges, vector<int>& queries) {
        vector<int> cnt(n);
        unordered_map<int, int> g;
        for (auto& e : edges) {
            int a = e[0] - 1, b = e[1] - 1;
            ++cnt[a];
            ++cnt[b];
            int k = min(a, b) * n + max(a, b);
            ++g[k];
        }
        vector<int> s = cnt;
        sort(s.begin(), s.end());
        vector<int> ans(queries.size());
        for (int i = 0; i < queries.size(); ++i) {
            int t = queries[i];
            for (int j = 0; j < n; ++j) {
                int x = s[j];
                int k = upper_bound(s.begin() + j + 1, s.end(), t - x) - s.begin();
                ans[i] += n - k;
            }
            for (auto& [k, v] : g) {
                int a = k / n, b = k % n;
                if (cnt[a] + cnt[b] > t && cnt[a] + cnt[b] - v <= t) {
                    --ans[i];
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countPairs(n int, edges [][]int, queries []int) []int {
	cnt := make([]int, n)
	g := map[int]int{}
	for _, e := range edges {
		a, b := e[0]-1, e[1]-1
		cnt[a]++
		cnt[b]++
		if a > b {
			a, b = b, a
		}
		g[a*n+b]++
	}
	s := make([]int, n)
	copy(s, cnt)
	sort.Ints(s)
	ans := make([]int, len(queries))
	for i, t := range queries {
		for j, x := range s {
			k := sort.Search(n, func(h int) bool { return s[h] > t-x && h > j })
			ans[i] += n - k
		}
		for k, v := range g {
			a, b := k/n, k%n
			if cnt[a]+cnt[b] > t && cnt[a]+cnt[b]-v <= t {
				ans[i]--
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function countPairs(n: number, edges: number[][], queries: number[]): number[] {
    const cnt: number[] = new Array(n).fill(0);
    const g: Map<number, number> = new Map();
    for (const [a, b] of edges) {
        ++cnt[a - 1];
        ++cnt[b - 1];
        const k = Math.min(a - 1, b - 1) * n + Math.max(a - 1, b - 1);
        g.set(k, (g.get(k) || 0) + 1);
    }
    const s = cnt.slice().sort((a, b) => a - b);
    const search = (nums: number[], x: number, l: number): number => {
        let r = nums.length;
        while (l < r) {
            const mid = (l + r) >> 1;
            if (nums[mid] > x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    };
    const ans: number[] = [];
    for (const t of queries) {
        let res = 0;
        for (let j = 0; j < s.length; ++j) {
            const k = search(s, t - s[j], j + 1);
            res += n - k;
        }
        for (const [k, v] of g) {
            const a = Math.floor(k / n);
            const b = k % n;
            if (cnt[a] + cnt[b] > t && cnt[a] + cnt[b] - v <= t) {
                --res;
            }
        }
        ans.push(res);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
