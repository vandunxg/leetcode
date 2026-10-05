---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Hash Table
    - Sorting
    - Sliding Window
---

<!-- problem:start -->

# [3851. Maximum Requests Without Violating the Limit 🔒](https://leetcode.com/problems/maximum-requests-without-violating-the-limit)

[中文文档](/solution/3800-3899/3851.Maximum%20Requests%20Without%20Violating%20the%20Limit/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên 2D <code>requests</code>, trong đó <code>requests[i] = [user<sub>i</sub>, time<sub>i</sub>]</code> cho biết <code>user<sub>i</sub></code> đã thực hiện một request tại <code>time<sub>i</sub></code>.</p>

<p>Bạn cũng được cho hai số nguyên <code>k</code> và <code>window</code>.</p>

<p>Một user vi phạm giới hạn nếu tồn tại một số nguyên <code>t</code> sao cho user đó thực hiện nhiều hơn <code>k</code> request trong khoảng đóng <code>[t, t + window]</code>.</p>

<p>Bạn có thể loại bỏ tùy ý số lượng request.</p>

<p>Trả về một số nguyên biểu thị <strong>tối đa</strong> số request có thể <strong>còn lại</strong> sao cho không user nào vi phạm giới hạn.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">requests = [[1,1],[2,1],[1,7],[2,8]], k = 1, window = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<ul>
	<li>Với user 1, các thời điểm request là <code>[1, 7]</code>. Hiệu giữa chúng là 6, lớn hơn <code>window = 4</code>.</li>
	<li>Với user 2, các thời điểm request là <code>[1, 8]</code>. Hiệu giữa chúng là 7, cũng lớn hơn <code>window = 4</code>.</li>
	<li>Không user nào thực hiện nhiều hơn <code>k = 1</code> request trong bất kỳ khoảng đóng nào có độ dài <code>window</code>. Vì vậy, cả 4 request đều có thể được giữ lại.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">requests = [[1,2],[1,5],[1,2],[1,6]], k = 2, window = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<ul>
	<li>Với user 1, các thời điểm request là <code>[2, 2, 5, 6]</code>. Khoảng đóng <code>[2, 7]</code> có độ dài <code>window = 5</code> chứa cả 4 request.</li>
	<li>Vì 4 lớn hơn <code>k = 2</code>, cần loại bỏ ít nhất 2 request.</li>
	<li>Sau khi loại bỏ bất kỳ 2 request nào, mọi khoảng đóng có độ dài <code>window</code> đều chứa nhiều nhất <code>k = 2</code> request.</li>
	<li>Do đó, số request tối đa có thể còn lại là 2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">requests = [[1,1],[2,5],[1,2],[3,9]], k = 1, window = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Với user 1, các thời điểm request là <code>[1, 2]</code>. Hiệu giữa chúng là 1, bằng <code>window = 1</code>.</li>
	<li>Khoảng đóng <code>[1, 2]</code> chứa cả hai request, nên số lượng là 2, vượt quá <code>k = 1</code>. Cần loại bỏ một request.</li>
	<li>User 2 và user 3 mỗi user chỉ có một request nên không vi phạm giới hạn. Vì vậy, số request tối đa có thể còn lại là 3.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= requests.length &lt;= 10<sup>5</sup></code></li>
	<li><code>requests[i] = [user<sub>i</sub>, time<sub>i</sub>]</code></li>
	<li><code>1 &lt;= k &lt;= requests.length</code></li>
	<li><code>1 &lt;= user<sub>i</sub>, time<sub>i</sub>, window &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sliding Window + Deque

<!-- thinking:start -->

> **Tư duy**
>
> Không user nào được có nhiều hơn $k$ request trong bất kỳ khoảng đóng nào có độ dài $\textit{window}$; ta có thể loại bỏ request. Các user độc lập với nhau, và $n \le 10^5$.
>
> Sắp xếp các thời điểm của một user rồi tham lam giữ một request khi cửa sổ còn chỗ, loại bỏ thời điểm hiện tại khi cửa sổ đã đầy để việc loại bỏ xảy ra ở đầu phải.
>
> Một deque lưu các thời điểm được giữ; loại phần tử đầu khi nó cách $t$ hơn $\textit{window}$. Nếu deque đã có $k$ phần tử, loại $t$.
>
> Bắt đầu từ tổng số request và giảm một cho mỗi request bị loại.

<!-- thinking:end -->

Ta có thể nhóm các request theo user và lưu chúng trong một hash table $g$, trong đó $g[u]$ là danh sách thời điểm request của user $u$. Với mỗi user, ta cần loại bỏ một số request khỏi danh sách thời điểm sao cho trong mọi khoảng có độ dài $window$, số request còn lại không vượt quá $k$.

Ta khởi tạo đáp án $\textit{ans}$ bằng tổng số request.

Với danh sách thời điểm request $g[u]$ của user $u$, trước tiên ta sort danh sách. Sau đó, ta dùng một deque $kept$ để lưu các thời điểm request hiện đang được giữ lại. Ta duyệt từng thời điểm request $t$ trong danh sách. Với mỗi thời điểm, ta loại khỏi $kept$ tất cả thời điểm request có hiệu với $t$ lớn hơn $window$. Sau đó, nếu số request còn lại trong $kept$ nhỏ hơn $k$, ta thêm $t$ vào $kept$; ngược lại, ta loại $t$ và giảm đáp án đi 1.

Cuối cùng, trả về đáp án $\textit{ans}$.

Độ phức tạp thời gian là $O(n \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số request. Mỗi request được duyệt một lần, việc sort tốn $O(n \log n)$ thời gian, còn các thao tác trên hash table và deque tốn $O(n)$ thời gian.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxRequests(self, requests: list[list[int]], k: int, window: int) -> int:
        g = defaultdict(list)
        for u, t in requests:
            g[u].append(t)
        ans = len(requests)
        for ts in g.values():
            ts.sort()
            kept = deque()
            for t in ts:
                while kept and t - kept[0] > window:
                    kept.popleft()
                if len(kept) < k:
                    kept.append(t)
                else:
                    ans -= 1
        return ans
```

#### Java

```java
class Solution {
    public int maxRequests(int[][] requests, int k, int window) {
        Map<Integer, List<Integer>> g = new HashMap<>();
        for (int[] r : requests) {
            int u = r[0], t = r[1];
            g.computeIfAbsent(u, x -> new ArrayList<>()).add(t);
        }

        int ans = requests.length;
        ArrayDeque<Integer> kept = new ArrayDeque<>();

        for (List<Integer> ts : g.values()) {
            Collections.sort(ts);
            kept.clear();
            for (int t : ts) {
                while (!kept.isEmpty() && t - kept.peekFirst() > window) {
                    kept.pollFirst();
                }
                if (kept.size() < k) {
                    kept.addLast(t);
                } else {
                    --ans;
                }
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
    int maxRequests(vector<vector<int>>& requests, int k, int window) {
        unordered_map<int, vector<int>> g;
        for (auto& r : requests) {
            g[r[0]].push_back(r[1]);
        }

        int ans = requests.size();
        for (auto& [_, ts] : g) {
            sort(ts.begin(), ts.end());
            queue<int> kept;
            int deletions = 0;

            for (int t : ts) {
                while (!kept.empty() && t - kept.front() > window) {
                    kept.pop();
                }
                if (kept.size() < k) {
                    kept.push(t);
                } else {
                    ans--;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxRequests(requests [][]int, k int, window int) int {
	g := make(map[int][]int)
	for _, r := range requests {
		u, t := r[0], r[1]
		g[u] = append(g[u], t)
	}
	ans := len(requests)
	for _, ts := range g {
		sort.Ints(ts)
		kept := make([]int, 0)
		for _, t := range ts {
			for len(kept) > 0 && t-kept[0] > window {
				kept = kept[1:]
			}
			if len(kept) < k {
				kept = append(kept, t)
			} else {
				ans--
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maxRequests(requests: number[][], k: number, window: number): number {
    const g = new Map<number, number[]>();
    for (const [u, t] of requests) {
        if (!g.has(u)) g.set(u, []);
        g.get(u)!.push(t);
    }
    let ans = requests.length;
    for (const ts of g.values()) {
        ts.sort((a, b) => a - b);
        const kept: number[] = [];
        let head = 0;
        for (const t of ts) {
            while (head < kept.length && t - kept[head] > window) {
                head++;
            }
            if (kept.length - head < k) {
                kept.push(t);
            } else {
                --ans;
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::{HashMap, VecDeque};

impl Solution {
    pub fn max_requests(requests: Vec<Vec<i32>>, k: i32, window: i32) -> i32 {
        let mut g: HashMap<i32, Vec<i32>> = HashMap::new();
        for r in &requests {
            let u: i32 = r[0];
            let t: i32 = r[1];
            g.entry(u).or_insert_with(Vec::new).push(t);
        }

        let mut ans: i32 = requests.len() as i32;
        let mut kept: VecDeque<i32> = VecDeque::new();

        for ts in g.values_mut() {
            ts.sort();
            kept.clear();

            for &t in ts.iter() {
                while let Some(&front) = kept.front() {
                    if t - front > window {
                        kept.pop_front();
                    } else {
                        break;
                    }
                }

                if kept.len() < k as usize {
                    kept.push_back(t);
                } else {
                    ans -= 1;
                }
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
