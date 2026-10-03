---
comments: true
difficulty: Hard
rating: 2449
source: Weekly Contest 274 Q4
tags:
    - Depth-First Search
    - Graph
    - Topological Sort
    - Array
    - Dynamic Programming
    - Kosaraju
    - Tarjan
---

<!-- problem:start -->

# [2127. Maximum Employees to Be Invited to a Meeting](https://leetcode.com/problems/maximum-employees-to-be-invited-to-a-meeting)

[中文文档](/solution/2100-2199/2127.Maximum%20Employees%20to%20Be%20Invited%20to%20a%20Meeting/README.md)

## Mô tả

<!-- description:start -->

<p>Một công ty đang tổ chức một cuộc họp và có danh sách gồm <code>n</code> nhân viên đang chờ được mời. Công ty đã chuẩn bị một chiếc bàn <strong>tròn</strong> lớn, có thể sắp xếp chỗ cho <strong>bất kỳ số lượng</strong> nhân viên nào.</p>

<p>Các nhân viên được đánh số từ <code>0</code> đến <code>n - 1</code>. Mỗi nhân viên có một người <strong>yêu thích</strong> và họ sẽ tham dự cuộc họp <strong>chỉ khi</strong> có thể ngồi cạnh người mình yêu thích tại bàn. Người mà một nhân viên yêu thích <strong>không phải</strong> chính họ.</p>

<p>Cho một mảng số nguyên <strong>0-indexed</strong> <code>favorite</code>, trong đó <code>favorite[i]</code> biểu thị người mà nhân viên <code>i<sup>th</sup></code> yêu thích, hãy trả về <em><strong>số nhân viên tối đa</strong> có thể được mời đến cuộc họp</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2127.Maximum%20Employees%20to%20Be%20Invited%20to%20a%20Meeting/images/ex1.png" style="width: 236px; height: 195px;" />
<pre>
<strong>Đầu vào:</strong> favorite = [2,2,1,2]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Hình trên cho thấy công ty có thể mời các nhân viên 0, 1 và 2, rồi sắp xếp họ quanh bàn tròn.
Không thể mời tất cả nhân viên vì nhân viên 2 không thể đồng thời ngồi cạnh các nhân viên 0, 1 và 3.
Lưu ý rằng công ty cũng có thể mời các nhân viên 1, 2 và 3, đồng thời sắp xếp cho họ những chỗ ngồi mong muốn.
Số nhân viên tối đa có thể được mời đến cuộc họp là 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> favorite = [1,2,0]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Mỗi nhân viên đều là người được ít nhất một nhân viên khác yêu thích, và cách duy nhất để công ty mời họ là mời tất cả nhân viên.
Cách sắp xếp chỗ ngồi giống như trong hình của ví dụ 1:
- Nhân viên 0 ngồi giữa nhân viên 2 và 1.
- Nhân viên 1 ngồi giữa nhân viên 0 và 2.
- Nhân viên 2 ngồi giữa nhân viên 1 và 0.
Số nhân viên tối đa có thể được mời đến cuộc họp là 3.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2127.Maximum%20Employees%20to%20Be%20Invited%20to%20a%20Meeting/images/ex2.png" style="width: 219px; height: 220px;" />
<pre>
<strong>Đầu vào:</strong> favorite = [3,0,1,4,1]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong>
Hình trên cho thấy công ty sẽ mời các nhân viên 0, 1, 3 và 4, rồi sắp xếp họ quanh bàn tròn.
Không thể mời nhân viên 2 vì hai chỗ cạnh người mà nhân viên này yêu thích, nhân viên 1, đã được sử dụng.
Vì vậy, công ty không mời nhân viên này dự cuộc họp.
Số nhân viên tối đa có thể được mời đến cuộc họp là 4.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == favorite.length</code></li>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= favorite[i] &lt;=&nbsp;n - 1</code></li>
	<li><code>favorite[i] != i</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Chu trình lớn nhất trong đồ thị + Chuỗi dài nhất

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi nhân viên thích đúng một người khác, nên đồ thị là hợp rời nhau của các cây hướng vào một chu trình gốc. Bàn tròn yêu cầu các cặp nhân viên ngồi cạnh nhau phải thích lẫn nhau: hoặc là một chu trình duy nhất, hoặc là các cặp tương hỗ có độ dài $2$ với các chuỗi đi vào gắn thêm. Không thể trộn các chu trình khác nhau.
>
> Một chu trình có độ dài ít nhất $3$ chỉ có thể đóng góp chính nó, nên một ứng viên là chu trình dài nhất. Tất cả các chu trình độ dài $2$ có thể được đặt cùng nhau, mỗi chu trình mở rộng thêm bằng chuỗi đi vào dài nhất, và quá trình loại bỏ theo thứ tự topo sẽ tính được các khoảng cách.
>
> Vì vậy, ta lấy giá trị lớn hơn giữa chu trình dài nhất và tổng khoảng cách trên tất cả các cặp tương hỗ.

<!-- thinking:end -->

Ta nhận thấy quan hệ yêu thích của các nhân viên trong bài toán có thể được biểu diễn bằng một đồ thị có hướng, được chia thành nhiều "cây hướng vào chu trình gốc". Mỗi cấu trúc chứa một chu trình, và mỗi node trên chu trình được nối với một cây.

"Cây hướng vào chu trình gốc" là gì? Trước hết, cây chu trình gốc là một đồ thị có $n$ node và $n$ cạnh, còn cây hướng vào nghĩa là trong đồ thị này, mỗi node có đúng một cạnh đi ra. Trong bài toán này, mỗi nhân viên có đúng một nhân viên yêu thích, nên đồ thị có hướng được tạo ra có thể gồm nhiều "cây hướng vào chu trình gốc".

<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2127.Maximum%20Employees%20to%20Be%20Invited%20to%20a%20Meeting/images/05Dxh9.png"></p>

Với bài toán này, ta có thể tìm độ dài chu trình lớn nhất trong đồ thị. Ở đây, ta chỉ cần tìm độ dài chu trình lớn nhất, vì nếu có nhiều chu trình thì chúng không nối với nhau, không thỏa mãn yêu cầu của bài toán.

Ngoài ra, với chu trình có kích thước bằng $2$, tức là có hai nhân viên thích lẫn nhau, ta có thể xếp hai nhân viên này cạnh nhau. Nếu hai nhân viên này được các nhân viên khác yêu thích, ta chỉ cần xếp những nhân viên yêu thích họ ngồi cạnh họ. Nếu có nhiều trường hợp như vậy, ta có thể xếp tất cả chúng cùng nhau.

Do đó, bài toán thực chất tương đương với việc tìm độ dài chu trình lớn nhất trong đồ thị, cùng với tất cả các chu trình có độ dài $2$ và chuỗi dài nhất đi vào mỗi chu trình. Ta có thể tìm đáp án bằng giá trị lớn hơn giữa hai kết quả này. Để tìm chuỗi dài nhất đi vào chu trình có độ dài $2$, ta có thể sử dụng sắp xếp topo.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng `favorite`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumInvitations(self, favorite: List[int]) -> int:
        def max_cycle(fa: List[int]) -> int:
            n = len(fa)
            vis = [False] * n
            ans = 0
            for i in range(n):
                if vis[i]:
                    continue
                cycle = []
                j = i
                while not vis[j]:
                    cycle.append(j)
                    vis[j] = True
                    j = fa[j]
                for k, v in enumerate(cycle):
                    if v == j:
                        ans = max(ans, len(cycle) - k)
                        break
            return ans

        def topological_sort(fa: List[int]) -> int:
            n = len(fa)
            indeg = [0] * n
            dist = [1] * n
            for v in fa:
                indeg[v] += 1
            q = deque(i for i, v in enumerate(indeg) if v == 0)
            while q:
                i = q.popleft()
                dist[fa[i]] = max(dist[fa[i]], dist[i] + 1)
                indeg[fa[i]] -= 1
                if indeg[fa[i]] == 0:
                    q.append(fa[i])
            return sum(dist[i] for i, v in enumerate(fa) if i == fa[fa[i]])

        return max(max_cycle(favorite), topological_sort(favorite))
```

#### Java

```java
class Solution {
    public int maximumInvitations(int[] favorite) {
        return Math.max(maxCycle(favorite), topologicalSort(favorite));
    }

    private int maxCycle(int[] fa) {
        int n = fa.length;
        boolean[] vis = new boolean[n];
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            if (vis[i]) {
                continue;
            }
            List<Integer> cycle = new ArrayList<>();
            int j = i;
            while (!vis[j]) {
                cycle.add(j);
                vis[j] = true;
                j = fa[j];
            }
            for (int k = 0; k < cycle.size(); ++k) {
                if (cycle.get(k) == j) {
                    ans = Math.max(ans, cycle.size() - k);
                }
            }
        }
        return ans;
    }

    private int topologicalSort(int[] fa) {
        int n = fa.length;
        int[] indeg = new int[n];
        int[] dist = new int[n];
        Arrays.fill(dist, 1);
        for (int v : fa) {
            indeg[v]++;
        }
        Deque<Integer> q = new ArrayDeque<>();
        for (int i = 0; i < n; ++i) {
            if (indeg[i] == 0) {
                q.offer(i);
            }
        }
        int ans = 0;
        while (!q.isEmpty()) {
            int i = q.pollFirst();
            dist[fa[i]] = Math.max(dist[fa[i]], dist[i] + 1);
            if (--indeg[fa[i]] == 0) {
                q.offer(fa[i]);
            }
        }
        for (int i = 0; i < n; ++i) {
            if (i == fa[fa[i]]) {
                ans += dist[i];
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumInvitations(vector<int>& favorite) {
        return max(maxCycle(favorite), topologicalSort(favorite));
    }

    int maxCycle(vector<int>& fa) {
        int n = fa.size();
        vector<bool> vis(n);
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            if (vis[i]) continue;
            vector<int> cycle;
            int j = i;
            while (!vis[j]) {
                cycle.push_back(j);
                vis[j] = true;
                j = fa[j];
            }
            for (int k = 0; k < cycle.size(); ++k) {
                if (cycle[k] == j) {
                    ans = max(ans, (int) cycle.size() - k);
                    break;
                }
            }
        }
        return ans;
    }

    int topologicalSort(vector<int>& fa) {
        int n = fa.size();
        vector<int> indeg(n);
        vector<int> dist(n, 1);
        for (int v : fa) ++indeg[v];
        queue<int> q;
        for (int i = 0; i < n; ++i)
            if (indeg[i] == 0) q.push(i);
        while (!q.empty()) {
            int i = q.front();
            q.pop();
            dist[fa[i]] = max(dist[fa[i]], dist[i] + 1);
            if (--indeg[fa[i]] == 0) q.push(fa[i]);
        }
        int ans = 0;
        for (int i = 0; i < n; ++i)
            if (i == fa[fa[i]]) ans += dist[i];
        return ans;
    }
};
```

#### Go

```go
func maximumInvitations(favorite []int) int {
	a, b := maxCycle(favorite), topologicalSort(favorite)
	return max(a, b)
}

func maxCycle(fa []int) int {
	n := len(fa)
	vis := make([]bool, n)
	ans := 0
	for i := range fa {
		if vis[i] {
			continue
		}
		j := i
		cycle := []int{}
		for !vis[j] {
			cycle = append(cycle, j)
			vis[j] = true
			j = fa[j]
		}
		for k, v := range cycle {
			if v == j {
				ans = max(ans, len(cycle)-k)
				break
			}
		}
	}
	return ans
}

func topologicalSort(fa []int) int {
	n := len(fa)
	indeg := make([]int, n)
	dist := make([]int, n)
	for i := range fa {
		dist[i] = 1
	}
	for _, v := range fa {
		indeg[v]++
	}
	q := []int{}
	for i, v := range indeg {
		if v == 0 {
			q = append(q, i)
		}
	}
	for len(q) > 0 {
		i := q[0]
		q = q[1:]
		dist[fa[i]] = max(dist[fa[i]], dist[i]+1)
		indeg[fa[i]]--
		if indeg[fa[i]] == 0 {
			q = append(q, fa[i])
		}
	}
	ans := 0
	for i := range fa {
		if i == fa[fa[i]] {
			ans += dist[i]
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maximumInvitations(favorite: number[]): number {
    return Math.max(maxCycle(favorite), topologicalSort(favorite));
}

function maxCycle(fa: number[]): number {
    const n = fa.length;
    const vis: boolean[] = Array(n).fill(false);
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        if (vis[i]) {
            continue;
        }
        const cycle: number[] = [];
        let j = i;
        for (; !vis[j]; j = fa[j]) {
            cycle.push(j);
            vis[j] = true;
        }
        for (let k = 0; k < cycle.length; ++k) {
            if (cycle[k] === j) {
                ans = Math.max(ans, cycle.length - k);
            }
        }
    }
    return ans;
}

function topologicalSort(fa: number[]): number {
    const n = fa.length;
    const indeg: number[] = Array(n).fill(0);
    const dist: number[] = Array(n).fill(1);
    for (const v of fa) {
        ++indeg[v];
    }
    const q: number[] = [];
    for (let i = 0; i < n; ++i) {
        if (indeg[i] === 0) {
            q.push(i);
        }
    }
    let ans = 0;
    while (q.length) {
        const i = q.pop()!;
        dist[fa[i]] = Math.max(dist[fa[i]], dist[i] + 1);
        if (--indeg[fa[i]] === 0) {
            q.push(fa[i]);
        }
    }
    for (let i = 0; i < n; ++i) {
        if (i === fa[fa[i]]) {
            ans += dist[i];
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
