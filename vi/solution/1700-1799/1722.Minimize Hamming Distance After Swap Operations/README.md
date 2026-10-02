---
comments: true
difficulty: Medium
rating: 1892
source: Weekly Contest 223 Q3
tags:
    - Depth-First Search
    - Union Find
    - Array
---

<!-- problem:start -->

# [1722. Minimize Hamming Distance After Swap Operations](https://leetcode.com/problems/minimize-hamming-distance-after-swap-operations)

[中文文档](/solution/1700-1799/1722.Minimize%20Hamming%20Distance%20After%20Swap%20Operations/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>source</code> và <code>target</code>, đều có độ dài <code>n</code>. Ngoài ra, cho mảng <code>allowedSwaps</code>, trong đó mỗi <code>allowedSwaps[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> cho biết ta được phép hoán đổi các phần tử tại chỉ số <code>a<sub>i</sub></code> và <code>b<sub>i</sub></code> <strong>(đánh chỉ số từ 0)</strong> của mảng <code>source</code>. Lưu ý rằng có thể hoán đổi các phần tử tại một cặp chỉ số cụ thể <strong>nhiều</strong> lần và theo <strong>bất kỳ</strong> thứ tự nào.</p>

<p><strong>Khoảng cách Hamming</strong> của hai mảng cùng độ dài <code>source</code> và <code>target</code> là số vị trí có các phần tử khác nhau. Cụ thể, đó là số chỉ số <code>i</code> thỏa mãn <code>0 &lt;= i &lt;= n-1</code> và <code>source[i] != target[i]</code> <strong>(đánh chỉ số từ 0)</strong>.</p>

<p>Trả về <em><strong>khoảng cách Hamming nhỏ nhất</strong> giữa </em><code>source</code><em> và </em><code>target</code><em> sau khi thực hiện <strong>bất kỳ</strong> số phép hoán đổi nào trên mảng </em><code>source</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> source = [1,2,3,4], target = [2,1,4,5], allowedSwaps = [[0,1],[2,3]]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> source có thể được biến đổi như sau:
- Swap indices 0 and 1: source = [<u>2</u>,<u>1</u>,3,4]
- Swap indices 2 and 3: source = [2,1,<u>4</u>,<u>3</u>]
Khoảng cách Hamming của source và target là 1 vì chúng khác nhau tại 1 vị trí: chỉ số 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> source = [1,2,3,4], target = [1,3,2,4], allowedSwaps = []
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Không có phép hoán đổi nào được phép.
Khoảng cách Hamming của source và target là 2 vì chúng khác nhau tại 2 vị trí: chỉ số 1 và chỉ số 2.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> source = [5,1,2,4,3], target = [1,5,4,2,3], allowedSwaps = [[0,4],[4,2],[1,3],[1,4]]
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == source.length == target.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= source[i], target[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= allowedSwaps.length &lt;= 10<sup>5</sup></code></li>
	<li><code>allowedSwaps[i].length == 2</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Union-Find + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Các phép hoán đổi được phép có thể kết hợp, nên các giá trị có thể được sắp xếp tự do trong một thành phần liên thông. Với $n\le 10^5$, ta phải xử lý các thành phần thay vì các hoán vị.
>
> DSU nối các chỉ số có thể hoán đổi. Trong một thành phần, đa tập của $\textit{source}$ nên khớp với $\textit{target}$ tại các vị trí đó nhiều nhất có thể.
>
> Đếm các giá trị của source theo root. Khi duyệt $\textit{target}$, tăng khoảng cách nếu thành phần không còn bản sao nào của giá trị đó.

<!-- thinking:end -->

Ta có thể xem mỗi chỉ số là một node và phần tử tương ứng với mỗi chỉ số là giá trị của node đó. Khi ấy, mỗi phần tử `[a_i, b_i]` trong `allowedSwaps` biểu diễn một cạnh giữa chỉ số `a_i` và `b_i`. Vì vậy, ta có thể dùng union-find để duy trì các thành phần liên thông này.

Sau khi xác định các thành phần liên thông, ta dùng hash table hai chiều $cnt$ để đếm số lần xuất hiện của mỗi phần tử trong từng thành phần. Cuối cùng, với mỗi phần tử trong mảng `target`, nếu số lần xuất hiện của nó trong thành phần tương ứng lớn hơn 0 thì giảm bộ đếm đi 1; ngược lại, tăng đáp án lên 1.

Độ phức tạp thời gian là $O(n \times \log n)$ hoặc $O(n \times \alpha(n))$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài mảng và $\alpha$ là hàm Ackermann ngược.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumHammingDistance(
        self, source: List[int], target: List[int], allowedSwaps: List[List[int]]
    ) -> int:
        def find(x: int) -> int:
            if p[x] != x:
                p[x] = find(p[x])
            return p[x]

        n = len(source)
        p = list(range(n))
        for a, b in allowedSwaps:
            p[find(a)] = find(b)
        cnt = defaultdict(Counter)
        for i, x in enumerate(source):
            j = find(i)
            cnt[j][x] += 1
        ans = 0
        for i, x in enumerate(target):
            j = find(i)
            cnt[j][x] -= 1
            ans += cnt[j][x] < 0
        return ans
```

#### Java

```java
class Solution {
    private int[] p;

    public int minimumHammingDistance(int[] source, int[] target, int[][] allowedSwaps) {
        int n = source.length;
        p = new int[n];
        for (int i = 0; i < n; i++) {
            p[i] = i;
        }
        for (int[] a : allowedSwaps) {
            p[find(a[0])] = find(a[1]);
        }
        Map<Integer, Map<Integer, Integer>> cnt = new HashMap<>();
        for (int i = 0; i < n; ++i) {
            int j = find(i);
            cnt.computeIfAbsent(j, k -> new HashMap<>()).merge(source[i], 1, Integer::sum);
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int j = find(i);
            Map<Integer, Integer> t = cnt.get(j);
            if (t.merge(target[i], -1, Integer::sum) < 0) {
                ++ans;
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
    int minimumHammingDistance(vector<int>& source, vector<int>& target, vector<vector<int>>& allowedSwaps) {
        int n = source.size();
        vector<int> p(n);
        iota(p.begin(), p.end(), 0);
        function<int(int)> find = [&](int x) {
            return x == p[x] ? x : p[x] = find(p[x]);
        };
        for (auto& a : allowedSwaps) {
            p[find(a[0])] = find(a[1]);
        }
        unordered_map<int, unordered_map<int, int>> cnt;
        for (int i = 0; i < n; ++i) {
            ++cnt[find(i)][source[i]];
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            if (--cnt[find(i)][target[i]] < 0) {
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minimumHammingDistance(source []int, target []int, allowedSwaps [][]int) (ans int) {
	n := len(source)
	p := make([]int, n)
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
	for _, a := range allowedSwaps {
		p[find(a[0])] = find(a[1])
	}
	cnt := map[int]map[int]int{}
	for i, x := range source {
		j := find(i)
		if cnt[j] == nil {
			cnt[j] = map[int]int{}
		}
		cnt[j][x]++
	}
	for i, x := range target {
		j := find(i)
		cnt[j][x]--
		if cnt[j][x] < 0 {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function minimumHammingDistance(
    source: number[],
    target: number[],
    allowedSwaps: number[][],
): number {
    const n = source.length;
    const p: number[] = Array.from({ length: n }, (_, i) => i);
    const find = (x: number): number => {
        if (p[x] !== x) {
            p[x] = find(p[x]);
        }
        return p[x];
    };
    for (const [a, b] of allowedSwaps) {
        p[find(a)] = find(b);
    }
    const cnt: Map<number, Map<number, number>> = new Map();
    for (let i = 0; i < n; ++i) {
        const j = find(i);
        if (!cnt.has(j)) {
            cnt.set(j, new Map());
        }
        const m = cnt.get(j)!;
        m.set(source[i], (m.get(source[i]) ?? 0) + 1);
    }
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        const j = find(i);
        const m = cnt.get(j)!;
        m.set(target[i], (m.get(target[i]) ?? 0) - 1);
        if (m.get(target[i])! < 0) {
            ++ans;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
