---
comments: true
difficulty: Medium
tags:
    - Breadth-First Search
    - Array
    - Hash Table
    - Two Pointers
    - Hash Function
---

<!-- problem:start -->

# [3991. Sort Array Using Prefix Reversals 🔒](https://leetcode.com/problems/sort-array-using-prefix-reversals)

[中文文档](/solution/3900-3999/3991.Sort%20Array%20Using%20Prefix%20Reversals/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>, trong đó <code>nums</code> là một <span data-keyword="permutation-array">hoán vị</span> của các số nguyên trong đoạn <code>[0, n - 1]</code>.</p>

<p>Bạn cũng được cho một mảng số nguyên <code>pre</code>, trong đó mỗi <code>pre[i]</code> là một độ dài <span data-keyword="array-prefix">prefix</span> hợp lệ.</p>

<p>Trong một phép toán, bạn có thể chọn một độ dài <code>x</code> bất kỳ từ <code>pre</code> và đảo ngược <code>x</code> phần tử đầu tiên của <code>nums</code>.</p>

<p>Ví dụ, thực hiện phép đảo prefix có độ dài <code>3</code> trên <code>[4, 1, 2, 3]</code> sẽ thu được <code>[2, 1, 4, 3]</code>.</p>

<p>Trả về số phép toán nhỏ nhất cần thực hiện để sắp xếp <code>nums</code> theo thứ tự tăng dần. Nếu không thể sắp xếp <code>nums</code>, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,0,1], pre = [2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Đảo ngược <code>pre[1] = 3</code> phần tử để được <code>nums = [1, 0, 2]</code>.</li>
	<li>Sau đó đảo ngược <code>pre[0] = 2</code> phần tử để được <code>nums = [0, 1, 2]</code>.</li>
	<li>Vậy số phép đảo prefix nhỏ nhất cần thực hiện là 2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,0,2], pre = [1,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không thể sắp xếp mảng bằng các độ dài prefix đã cho, nên đáp án là -1.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [0,1], pre = [2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Vì <code>nums</code> đã được sắp xếp, không cần thực hiện phép đảo prefix nào. Do đó, đáp án là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 8</code></li>
	<li><code>0 &lt;= nums[i] &lt;= n - 1</code></li>
	<li><code>1 &lt;= pre.length &lt;= n</code></li>
	<li><code>1 &lt;= pre[i] &lt;= n</code></li>
	<li><code>​​​​​​​nums</code> là một hoán vị của các số nguyên từ 0 đến <code>n - 1</code>.</li>
	<li><code>pre</code> gồm các số nguyên <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS

<!-- thinking:start -->

> **Tư duy**
>
> $n\le 8$ nên có nhiều nhất $8!=40320$ hoán vị, còn số phép đảo prefix được phép cũng ít, vì vậy BFS trên các hoán vị sẽ cho số bước nhỏ nhất.
>
> Trạng thái đích là $(0,1,\ldots,n-1)$. Từ tuple ban đầu, đảo ngược từng prefix trong $\textit{pre}$ rồi đưa các trạng thái chưa thăm vào queue. Khi gặp trạng thái đích, ta có đáp án tối ưu; nếu queue rỗng thì không thể thực hiện.
>
> Có thể khử trùng lặp bằng tuple hoặc một số nguyên cơ số $8$.

<!-- thinking:end -->

Vì $n \le 8$, số hoán vị nhiều nhất là $8! = 40320$, nên ta có thể dùng BFS để tìm số phép toán nhỏ nhất.

Coi mảng hiện tại là một trạng thái, trạng thái đích là $[0, 1, \ldots, n - 1]$. Nếu trạng thái ban đầu đã là đích, trả về $0$. Nếu không, bắt đầu BFS từ trạng thái ban đầu: mỗi lần lấy một trạng thái khỏi queue, duyệt qua mọi độ dài prefix $x$ trong $\textit{pre}$ và đảo ngược $x$ phần tử đầu tiên để thu được trạng thái mới. Nếu trạng thái mới bằng trạng thái đích, trả về số bước hiện tại; nếu chưa được thăm, đưa nó vào queue. Nếu tìm kiếm kết thúc mà không đến được đích, trả về $-1$.

Để thuận tiện khử trùng lặp, mã hóa mỗi hoán vị thành một số nguyên ở cơ số $8$ (mọi phần tử đều nằm trong $[0, 7]$).

Độ phức tạp thời gian là $O(n! \cdot m \cdot n)$, và độ phức tạp không gian là $O(n! \cdot n)$. Trong đó, $n$ là độ dài mảng, còn $m$ là độ dài của $\textit{pre}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sortArray(self, nums: List[int], pre: List[int]) -> int:
        n = len(nums)
        target = tuple(range(n))
        start = tuple(nums)

        if start == target:
            return 0

        vis = {start}
        q = deque([(start, 0)])

        while q:
            state, dist = q.popleft()
            nd = dist + 1
            for x in pre:
                nxt = state[:x][::-1] + state[x:]
                if nxt == target:
                    return nd
                if nxt not in vis:
                    vis.add(nxt)
                    q.append((nxt, nd))
        return -1
```

#### Java

```java
class Solution {
    public int sortArray(int[] nums, int[] pre) {
        int n = nums.length;

        int target = 0;
        for (int i = 0; i < n; i++) {
            target = target * 8 + i;
        }

        int start = 0;
        for (int x : nums) {
            start = start * 8 + x;
        }

        if (start == target) {
            return 0;
        }

        Set<Integer> vis = new HashSet<>();
        vis.add(start);

        Deque<int[]> q = new ArrayDeque<>();
        Deque<Integer> dist = new ArrayDeque<>();
        q.offer(nums.clone());
        dist.offer(0);

        while (!q.isEmpty()) {
            int[] state = q.poll();
            int d = dist.poll();
            int nd = d + 1;

            for (int x : pre) {
                int[] nxt = state.clone();

                int l = 0, r = x - 1;
                while (l < r) {
                    int t = nxt[l];
                    nxt[l] = nxt[r];
                    nxt[r] = t;
                    l++;
                    r--;
                }

                int key = 0;
                for (int v : nxt) {
                    key = key * 8 + v;
                }

                if (key == target) {
                    return nd;
                }

                if (vis.add(key)) {
                    q.offer(nxt);
                    dist.offer(nd);
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
    int sortArray(vector<int>& nums, vector<int>& pre) {
        int n = nums.size();

        int target = 0;
        for (int i = 0; i < n; i++) {
            target = target * 8 + i;
        }

        int start = 0;
        for (int x : nums) {
            start = start * 8 + x;
        }

        if (start == target) {
            return 0;
        }

        unordered_set<int> vis;
        vis.insert(start);

        queue<pair<vector<int>, int>> q;
        q.emplace(nums, 0);

        while (!q.empty()) {
            auto [state, dist] = q.front();
            q.pop();

            int nd = dist + 1;

            for (int x : pre) {
                vector<int> nxt = state;
                reverse(nxt.begin(), nxt.begin() + x);

                int key = 0;
                for (int v : nxt) {
                    key = key * 8 + v;
                }

                if (key == target) {
                    return nd;
                }

                if (vis.insert(key).second) {
                    q.emplace(move(nxt), nd);
                }
            }
        }

        return -1;
    }
};
```

#### Go

```go
func sortArray(nums []int, pre []int) int {
	n := len(nums)

	target := 0
	for i := 0; i < n; i++ {
		target = target*8 + i
	}

	start := 0
	for _, x := range nums {
		start = start*8 + x
	}

	if start == target {
		return 0
	}

	vis := map[int]bool{start: true}

	type pair struct {
		state []int
		dist  int
	}
	q := []pair{{append([]int(nil), nums...), 0}}

	for len(q) > 0 {
		p := q[0]
		q = q[1:]

		state := p.state
		dist := p.dist
		nd := dist + 1

		for _, x := range pre {
			nxt := append([]int(nil), state...)

			for l, r := 0, x-1; l < r; l, r = l+1, r-1 {
				nxt[l], nxt[r] = nxt[r], nxt[l]
			}

			key := 0
			for _, v := range nxt {
				key = key*8 + v
			}

			if key == target {
				return nd
			}

			if !vis[key] {
				vis[key] = true
				q = append(q, pair{nxt, nd})
			}
		}
	}

	return -1
}
```

#### TypeScript

```ts
function sortArray(nums: number[], pre: number[]): number {
    const n = nums.length;

    let target = 0;
    for (let i = 0; i < n; i++) {
        target = target * 8 + i;
    }

    let start = 0;
    for (const x of nums) {
        start = start * 8 + x;
    }

    if (start === target) {
        return 0;
    }

    const vis = new Set<number>();
    vis.add(start);

    const q: [number[], number][] = [[nums.slice(), 0]];

    while (q.length) {
        const [state, dist] = q.shift()!;
        const nd = dist + 1;

        for (const x of pre) {
            const nxt = state.slice();

            for (let l = 0, r = x - 1; l < r; l++, r--) {
                [nxt[l], nxt[r]] = [nxt[r], nxt[l]];
            }

            let key = 0;
            for (const v of nxt) {
                key = key * 8 + v;
            }

            if (key === target) {
                return nd;
            }

            if (!vis.has(key)) {
                vis.add(key);
                q.push([nxt, nd]);
            }
        }
    }

    return -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
