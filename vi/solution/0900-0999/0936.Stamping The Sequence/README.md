---
comments: true
difficulty: Hard
tags:
    - Stack
    - Greedy
    - Queue
    - String
---

<!-- problem:start -->

# [936. Stamping The Sequence](https://leetcode.com/problems/stamping-the-sequence)

[中文文档](/solution/0900-0999/0936.Stamping%20The%20Sequence/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>stamp</code> và <code>target</code>. Ban đầu, có chuỗi <code>s</code> dài <code>target.length</code> và mọi vị trí đều thỏa <code>s[i] == &#39;?&#39;</code>.</p>

<p>Trong một lượt, bạn có thể đặt <code>stamp</code> lên <code>s</code> và thay mỗi ký tự của <code>s</code> bằng ký tự tương ứng trong <code>stamp</code>.</p>

<ul>
	<li>Ví dụ, nếu <code>stamp = &quot;abc&quot;</code> và <code>target = &quot;abcba&quot;</code>, ban đầu <code>s</code> là <code>&quot;?????&quot;</code>. Trong một lượt, bạn có thể:

    <ul>
    	<li>đặt <code>stamp</code> tại chỉ số <code>0</code> của <code>s</code> để thu được <code>&quot;abc??&quot;</code>,</li>
    	<li>đặt <code>stamp</code> tại chỉ số <code>1</code> của <code>s</code> để thu được <code>&quot;?abc?&quot;</code>, hoặc</li>
    	<li>đặt <code>stamp</code> tại chỉ số <code>2</code> của <code>s</code> để thu được <code>&quot;??abc&quot;</code>.</li>
    </ul>
    Lưu ý, toàn bộ <code>stamp</code> phải nằm trong phạm vi của <code>s</code> thì mới có thể đóng dấu (tức là bạn không thể đặt <code>stamp</code> tại chỉ số <code>3</code> của <code>s</code>).</li>

</ul>

<p>Ta muốn biến <code>s</code> thành <code>target</code> trong <strong>không quá</strong> <code>10 * target.length</code> lượt.</p>

<p>Trả về <em>mảng chứa chỉ số của ký tự ngoài cùng bên trái được đóng dấu ở mỗi lượt</em>. Nếu không thể biến <code>s</code> thành <code>target</code> trong <code>10 * target.length</code> lượt, trả về mảng rỗng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> stamp = &quot;abc&quot;, target = &quot;ababc&quot;
<strong>Đầu ra:</strong> [0,2]
<strong>Giải thích:</strong> Ban đầu, s = &quot;?????&quot;.
- Đặt stamp tại chỉ số 0 để thu được &quot;abc??&quot;.
- Đặt stamp tại chỉ số 2 để thu được &quot;ababc&quot;.
[1,0,2] và một số đáp án khác cũng được chấp nhận.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> stamp = &quot;abca&quot;, target = &quot;aabcaca&quot;
<strong>Đầu ra:</strong> [3,0,1]
<strong>Giải thích:</strong> Ban đầu, s = &quot;???????&quot;.
- Đặt stamp tại chỉ số 3 để thu được &quot;???abca&quot;.
- Đặt stamp tại chỉ số 0 để thu được &quot;abcabca&quot;.
- Đặt stamp tại chỉ số 1 để thu được &quot;aabcaca&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= stamp.length &lt;= target.length &lt;= 1000</code></li>
	<li><code>stamp</code> và <code>target</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tư duy ngược + Topological Sorting

<!-- thinking:start -->

> **Tư duy**
>
> Đóng dấu theo chiều xuôi sẽ ghi đè các dấu trước đó, khiến việc xác định khi nào có thể đóng dấu trở nên khó. Ta đi ngược từ $target$ về các dấu hỏi: có thể “gỡ dấu” một cửa sổ khi mọi ký tự còn nhìn thấy đều khớp với stamp.
>
> Bậc vào của một cửa sổ là số ký tự không khớp; mỗi vị trí liên kết tới các cửa sổ vẫn cần xét vị trí đó. Đưa các cửa sổ có bậc $0$ vào queue; khi gỡ dấu ở chúng, ta cập nhật các cửa sổ lân cận. Nếu mọi vị trí đều được xử lý, đảo thứ tự để có chuỗi thao tác xuôi.

<!-- thinking:end -->

Nếu xử lý chuỗi theo chiều xuôi, các thao tác sau sẽ ghi đè thao tác trước nên khá phức tạp. Vì vậy, ta xét theo chiều ngược: bắt đầu từ chuỗi đích $target$ và tìm cách biến nó thành $?????$.

Gọi độ dài của stamp là $m$ và độ dài chuỗi đích là $n$. Có $n-m+1$ vị trí bắt đầu có thể đặt stamp lên chuỗi đích. Ta duyệt các vị trí này và xử lý ngược bằng cách tương tự topological sort.

Trước tiên, mỗi vị trí bắt đầu tương ứng với một cửa sổ độ dài $m$.

Tiếp theo, ta định nghĩa các cấu trúc dữ liệu và biến sau:

- Mảng bậc vào $indeg$, trong đó $indeg[i]$ là số ký tự trong cửa sổ thứ $i$ khác với ký tự tương ứng trong stamp. Ban đầu, $indeg[i]=m$. Nếu $indeg[i]=0$, mọi ký tự trong cửa sổ thứ $i$ đều khớp với stamp và ta có thể đặt stamp tại đó.
- Danh sách kề $g$, trong đó $g[i]$ chứa các cửa sổ có ký tự tại vị trí thứ $i$ của chuỗi đích $target$ khác với ký tự tương ứng trong stamp.
- Queue $q$, dùng để lưu chỉ số các cửa sổ có bậc vào bằng $0$.
- Mảng $vis$, dùng để đánh dấu các vị trí của chuỗi đích $target$ đã được xử lý.
- Mảng $ans$, dùng để lưu đáp án.

Sau đó, ta thực hiện topological sort. Ở mỗi bước, lấy chỉ số cửa sổ $i$ ở đầu queue và thêm $i$ vào mảng đáp án $ans$. Tiếp theo, duyệt từng vị trí $j$ trong stamp. Nếu vị trí thứ $j$ của cửa sổ thứ $i$ chưa được xử lý, đánh dấu vị trí đó rồi giảm $1$ bậc vào của mọi cửa sổ trong mảng $indeg$ có ký tự chưa khớp tại vị trí tương ứng. Nếu bậc vào của cửa sổ nào trở thành $0$, đưa cửa sổ đó vào queue $q$ để xử lý sau.

Sau khi topological sort kết thúc, nếu mọi vị trí của chuỗi đích $target$ đều đã được xử lý thì mảng $ans$ chứa đáp án. Nếu không, không thể tạo được chuỗi $target$ nên ta trả về mảng rỗng.

Độ phức tạp thời gian là $O(n \times (n - m + 1))$ và độ phức tạp không gian là $O(n \times (n - m + 1))$, trong đó $n$ và $m$ lần lượt là độ dài của chuỗi đích $target$ và stamp.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def movesToStamp(self, stamp: str, target: str) -> List[int]:
        m, n = len(stamp), len(target)
        indeg = [m] * (n - m + 1)
        q = deque()
        g = [[] for _ in range(n)]
        for i in range(n - m + 1):
            for j, c in enumerate(stamp):
                if target[i + j] == c:
                    indeg[i] -= 1
                    if indeg[i] == 0:
                        q.append(i)
                else:
                    g[i + j].append(i)
        ans = []
        vis = [False] * n
        while q:
            i = q.popleft()
            ans.append(i)
            for j in range(m):
                if not vis[i + j]:
                    vis[i + j] = True
                    for k in g[i + j]:
                        indeg[k] -= 1
                        if indeg[k] == 0:
                            q.append(k)
        return ans[::-1] if all(vis) else []
```

#### Java

```java
class Solution {
    public int[] movesToStamp(String stamp, String target) {
        int m = stamp.length(), n = target.length();
        int[] indeg = new int[n - m + 1];
        Arrays.fill(indeg, m);
        List<Integer>[] g = new List[n];
        Arrays.setAll(g, i -> new ArrayList<>());
        Deque<Integer> q = new ArrayDeque<>();
        for (int i = 0; i < n - m + 1; ++i) {
            for (int j = 0; j < m; ++j) {
                if (target.charAt(i + j) == stamp.charAt(j)) {
                    if (--indeg[i] == 0) {
                        q.offer(i);
                    }
                } else {
                    g[i + j].add(i);
                }
            }
        }
        List<Integer> ans = new ArrayList<>();
        boolean[] vis = new boolean[n];
        while (!q.isEmpty()) {
            int i = q.poll();
            ans.add(i);
            for (int j = 0; j < m; ++j) {
                if (!vis[i + j]) {
                    vis[i + j] = true;
                    for (int k : g[i + j]) {
                        if (--indeg[k] == 0) {
                            q.offer(k);
                        }
                    }
                }
            }
        }
        for (int i = 0; i < n; ++i) {
            if (!vis[i]) {
                return new int[0];
            }
        }
        Collections.reverse(ans);
        return ans.stream().mapToInt(Integer::intValue).toArray();
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> movesToStamp(string stamp, string target) {
        int m = stamp.size(), n = target.size();
        vector<int> indeg(n - m + 1, m);
        vector<int> g[n];
        queue<int> q;
        for (int i = 0; i < n - m + 1; ++i) {
            for (int j = 0; j < m; ++j) {
                if (target[i + j] == stamp[j]) {
                    if (--indeg[i] == 0) {
                        q.push(i);
                    }
                } else {
                    g[i + j].push_back(i);
                }
            }
        }
        vector<int> ans;
        vector<bool> vis(n);
        while (q.size()) {
            int i = q.front();
            q.pop();
            ans.push_back(i);
            for (int j = 0; j < m; ++j) {
                if (!vis[i + j]) {
                    vis[i + j] = true;
                    for (int k : g[i + j]) {
                        if (--indeg[k] == 0) {
                            q.push(k);
                        }
                    }
                }
            }
        }
        for (int i = 0; i < n; ++i) {
            if (!vis[i]) {
                return {};
            }
        }
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```

#### Go

```go
func movesToStamp(stamp string, target string) (ans []int) {
	m, n := len(stamp), len(target)
	indeg := make([]int, n-m+1)
	for i := range indeg {
		indeg[i] = m
	}
	g := make([][]int, n)
	q := []int{}
	for i := 0; i < n-m+1; i++ {
		for j := range stamp {
			if target[i+j] == stamp[j] {
				indeg[i]--
				if indeg[i] == 0 {
					q = append(q, i)
				}
			} else {
				g[i+j] = append(g[i+j], i)
			}
		}
	}
	vis := make([]bool, n)
	for len(q) > 0 {
		i := q[0]
		q = q[1:]
		ans = append(ans, i)
		for j := range stamp {
			if !vis[i+j] {
				vis[i+j] = true
				for _, k := range g[i+j] {
					indeg[k]--
					if indeg[k] == 0 {
						q = append(q, k)
					}
				}
			}
		}
	}
	for _, v := range vis {
		if !v {
			return []int{}
		}
	}
	for i, j := 0, len(ans)-1; i < j; i, j = i+1, j-1 {
		ans[i], ans[j] = ans[j], ans[i]
	}
	return
}
```

#### TypeScript

```ts
function movesToStamp(stamp: string, target: string): number[] {
    const m: number = stamp.length;
    const n: number = target.length;
    const indeg: number[] = Array(n - m + 1).fill(m);
    const g: number[][] = Array.from({ length: n }, () => []);
    const q: number[] = [];
    for (let i = 0; i < n - m + 1; ++i) {
        for (let j = 0; j < m; ++j) {
            if (target[i + j] === stamp[j]) {
                if (--indeg[i] === 0) {
                    q.push(i);
                }
            } else {
                g[i + j].push(i);
            }
        }
    }

    const ans: number[] = [];
    const vis: boolean[] = Array(n).fill(false);
    while (q.length) {
        const i: number = q.shift()!;
        ans.push(i);
        for (let j = 0; j < m; ++j) {
            if (!vis[i + j]) {
                vis[i + j] = true;
                for (const k of g[i + j]) {
                    if (--indeg[k] === 0) {
                        q.push(k);
                    }
                }
            }
        }
    }
    if (!vis.every(v => v)) {
        return [];
    }
    ans.reverse();
    return ans;
}
```

#### Rust

```rust
use std::collections::VecDeque;

impl Solution {
    pub fn moves_to_stamp(stamp: String, target: String) -> Vec<i32> {
        let m = stamp.len();
        let n = target.len();

        let mut indeg: Vec<usize> = vec![m; n - m + 1];
        let mut g: Vec<Vec<usize>> = vec![Vec::new(); n];
        let mut q: VecDeque<usize> = VecDeque::new();

        for i in 0..n - m + 1 {
            for j in 0..m {
                if target.chars().nth(i + j).unwrap() == stamp.chars().nth(j).unwrap() {
                    indeg[i] -= 1;
                    if indeg[i] == 0 {
                        q.push_back(i);
                    }
                } else {
                    g[i + j].push(i);
                }
            }
        }

        let mut ans: Vec<i32> = Vec::new();
        let mut vis: Vec<bool> = vec![false; n];

        while let Some(i) = q.pop_front() {
            ans.push(i as i32);

            for j in 0..m {
                if !vis[i + j] {
                    vis[i + j] = true;

                    for &k in g[i + j].iter() {
                        indeg[k] -= 1;
                        if indeg[k] == 0 {
                            q.push_back(k);
                        }
                    }
                }
            }
        }

        if vis.iter().all(|&v| v) {
            ans.reverse();
            ans
        } else {
            Vec::new()
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
