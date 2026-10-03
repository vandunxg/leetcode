---
comments: true
difficulty: Hard
rating: 2648
source: Biweekly Contest 65 Q4
tags:
    - Greedy
    - Queue
    - Array
    - Two Pointers
    - Binary Search
    - Sorting
    - Monotonic Queue
---

<!-- problem:start -->

# [2071. Maximum Number of Tasks You Can Assign](https://leetcode.com/problems/maximum-number-of-tasks-you-can-assign)

[中文文档](/solution/2000-2099/2071.Maximum%20Number%20of%20Tasks%20You%20Can%20Assign/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có <code>n</code> nhiệm vụ và <code>m</code> worker. Yêu cầu sức mạnh của mỗi nhiệm vụ được lưu trong một mảng số nguyên <code>tasks</code> <strong>được đánh chỉ số từ 0</strong>, trong đó nhiệm vụ thứ <code>i<sup>th</sup></code> cần sức mạnh <code>tasks[i]</code> để hoàn thành. Sức mạnh của mỗi worker được lưu trong một mảng số nguyên <code>workers</code> <strong>được đánh chỉ số từ 0</strong>, trong đó worker thứ <code>j<sup>th</sup></code> có sức mạnh <code>workers[j]</code>. Mỗi worker chỉ có thể được giao <strong>một</strong> nhiệm vụ và phải có sức mạnh <strong>lớn hơn hoặc bằng</strong> yêu cầu sức mạnh của nhiệm vụ đó (tức là <code>workers[j] &gt;= tasks[i]</code>).</p>

<p>Ngoài ra, bạn có <code>pills</code> viên thuốc thần kỳ giúp <strong>tăng sức mạnh của một worker</strong> thêm <code>strength</code>. Bạn có thể quyết định worker nào nhận thuốc, nhưng mỗi worker chỉ được nhận <strong>nhiều nhất một</strong> viên thuốc thần kỳ.</p>

<p>Cho các mảng số nguyên <code>tasks</code> và <code>workers</code> <strong>được đánh chỉ số từ 0</strong> cùng các số nguyên <code>pills</code> và <code>strength</code>, hãy trả về <em><strong>số nhiệm vụ tối đa có thể hoàn thành</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> tasks = [<u><strong>3</strong></u>,<u><strong>2</strong></u>,<u><strong>1</strong></u>], workers = [<u><strong>0</strong></u>,<u><strong>3</strong></u>,<u><strong>3</strong></u>], pills = 1, strength = 1
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Ta có thể phân công thuốc và nhiệm vụ như sau:
- Cho worker 0 dùng thuốc thần kỳ.
- Giao worker 0 cho nhiệm vụ 2 (0 + 1 &gt;= 1)
- Giao worker 1 cho nhiệm vụ 1 (3 &gt;= 2)
- Giao worker 2 cho nhiệm vụ 0 (3 &gt;= 3)
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> tasks = [<u><strong>5</strong></u>,4], workers = [<u><strong>0</strong></u>,0,0], pills = 1, strength = 5
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
Ta có thể phân công thuốc và nhiệm vụ như sau:
- Cho worker 0 dùng thuốc thần kỳ.
- Giao worker 0 cho nhiệm vụ 0 (0 + 5 &gt;= 5)
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> tasks = [<u><strong>10</strong></u>,<u><strong>15</strong></u>,30], workers = [<u><strong>0</strong></u>,<u><strong>10</strong></u>,10,10,10], pills = 3, strength = 10
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Ta có thể phân công thuốc và nhiệm vụ như sau:
- Cho worker 0 và worker 1 dùng thuốc thần kỳ.
- Giao worker 0 cho nhiệm vụ 0 (0 + 10 &gt;= 10)
- Giao worker 1 cho nhiệm vụ 1 (10 + 10 &gt;= 15)
Không dùng viên thuốc cuối cùng vì nó không giúp worker nào đủ mạnh để làm nhiệm vụ cuối cùng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == tasks.length</code></li>
	<li><code>m == workers.length</code></li>
	<li><code>1 &lt;= n, m &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= pills &lt;= m</code></li>
	<li><code>0 &lt;= tasks[i], workers[j], strength &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Số nhiệm vụ $x$ có tính đơn điệu. Việc kiểm tra phải gần tuyến tính khi $n,m \le 5 \times 10^4$. Ta chọn $x$ nhiệm vụ khó nhất và $x$ worker mạnh nhất.
>
> Xét $x$ worker này từ yếu đến mạnh: nếu có thể, chọn nhiệm vụ dễ nhất còn lại mà không dùng thuốc; nếu không thì dùng thuốc cho nhiệm vụ khó nhất. Một deque lưu các nhiệm vụ hiện có thể hoàn thành.
>
> Dùng tìm kiếm nhị phân để tìm $x$ lớn nhất khả thi.

<!-- thinking:end -->

Sắp xếp các nhiệm vụ theo thứ tự tăng dần của yêu cầu sức mạnh để hoàn thành, đồng thời sắp xếp các worker theo thứ tự tăng dần của sức mạnh.

Giả sử số nhiệm vụ cần phân công là $x$. Ta có thể tham lam giao $x$ nhiệm vụ đầu tiên cho $x$ worker có sức mạnh cao nhất. Nếu có thể hoàn thành $x$ nhiệm vụ, thì cũng có thể hoàn thành $x-1$, $x-2$, $x-3$, ..., $1$, $0$ nhiệm vụ. Vì vậy, ta có thể dùng tìm kiếm nhị phân để tìm $x$ lớn nhất sao cho có thể hoàn thành $x$ nhiệm vụ.

Ta định nghĩa hàm $check(x)$ để xác định liệu có thể hoàn thành $x$ nhiệm vụ hay không.

Cách triển khai $check(x)$ như sau:

Duyệt qua $x$ worker có sức mạnh cao nhất theo thứ tự tăng dần. Gọi worker hiện tại đang được xử lý là $j$. Các nhiệm vụ đang khả dụng phải thỏa mãn $tasks[i] \leq workers[j] + strength$.

Nếu nhiệm vụ có yêu cầu sức mạnh nhỏ nhất $task[i]$ trong số các nhiệm vụ đang khả dụng nhỏ hơn hoặc bằng $workers[j]$, thì worker $j$ có thể hoàn thành nhiệm vụ $task[i]$ mà không cần dùng thuốc. Nếu không, worker hiện tại phải dùng thuốc. Nếu vẫn còn thuốc, dùng một viên và hoàn thành nhiệm vụ có yêu cầu sức mạnh lớn nhất trong số các nhiệm vụ đang khả dụng. Nếu không còn thuốc, trả về `false`.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(n)$, trong đó $n$ là số nhiệm vụ.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxTaskAssign(
        self, tasks: List[int], workers: List[int], pills: int, strength: int
    ) -> int:
        def check(x):
            i = 0
            q = deque()
            p = pills
            for j in range(m - x, m):
                while i < x and tasks[i] <= workers[j] + strength:
                    q.append(tasks[i])
                    i += 1
                if not q:
                    return False
                if q[0] <= workers[j]:
                    q.popleft()
                elif p == 0:
                    return False
                else:
                    p -= 1
                    q.pop()
            return True

        n, m = len(tasks), len(workers)
        tasks.sort()
        workers.sort()
        left, right = 0, min(n, m)
        while left < right:
            mid = (left + right + 1) >> 1
            if check(mid):
                left = mid
            else:
                right = mid - 1
        return left
```

#### Java

```java
class Solution {
    private int[] tasks;
    private int[] workers;
    private int strength;
    private int pills;
    private int m;
    private int n;

    public int maxTaskAssign(int[] tasks, int[] workers, int pills, int strength) {
        Arrays.sort(tasks);
        Arrays.sort(workers);
        this.tasks = tasks;
        this.workers = workers;
        this.strength = strength;
        this.pills = pills;
        n = tasks.length;
        m = workers.length;
        int left = 0, right = Math.min(m, n);
        while (left < right) {
            int mid = (left + right + 1) >> 1;
            if (check(mid)) {
                left = mid;
            } else {
                right = mid - 1;
            }
        }
        return left;
    }

    private boolean check(int x) {
        int i = 0;
        Deque<Integer> q = new ArrayDeque<>();
        int p = pills;
        for (int j = m - x; j < m; ++j) {
            while (i < x && tasks[i] <= workers[j] + strength) {
                q.offer(tasks[i++]);
            }
            if (q.isEmpty()) {
                return false;
            }
            if (q.peekFirst() <= workers[j]) {
                q.pollFirst();
            } else if (p == 0) {
                return false;
            } else {
                --p;
                q.pollLast();
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxTaskAssign(vector<int>& tasks, vector<int>& workers, int pills, int strength) {
        sort(tasks.begin(), tasks.end());
        sort(workers.begin(), workers.end());
        int n = tasks.size(), m = workers.size();
        int left = 0, right = min(m, n);
        auto check = [&](int x) {
            int p = pills;
            deque<int> q;
            int i = 0;
            for (int j = m - x; j < m; ++j) {
                while (i < x && tasks[i] <= workers[j] + strength) {
                    q.push_back(tasks[i++]);
                }
                if (q.empty()) {
                    return false;
                }
                if (q.front() <= workers[j]) {
                    q.pop_front();
                } else if (p == 0) {
                    return false;
                } else {
                    --p;
                    q.pop_back();
                }
            }
            return true;
        };
        while (left < right) {
            int mid = (left + right + 1) >> 1;
            if (check(mid)) {
                left = mid;
            } else {
                right = mid - 1;
            }
        }
        return left;
    }
};
```

#### Go

```go
func maxTaskAssign(tasks []int, workers []int, pills int, strength int) int {
	sort.Ints(tasks)
	sort.Ints(workers)
	n, m := len(tasks), len(workers)
	left, right := 0, min(m, n)
	check := func(x int) bool {
		p := pills
		q := []int{}
		i := 0
		for j := m - x; j < m; j++ {
			for i < x && tasks[i] <= workers[j]+strength {
				q = append(q, tasks[i])
				i++
			}
			if len(q) == 0 {
				return false
			}
			if q[0] <= workers[j] {
				q = q[1:]
			} else if p == 0 {
				return false
			} else {
				p--
				q = q[:len(q)-1]
			}
		}
		return true
	}
	for left < right {
		mid := (left + right + 1) >> 1
		if check(mid) {
			left = mid
		} else {
			right = mid - 1
		}
	}
	return left
}
```

#### TypeScript

```ts
function maxTaskAssign(
    tasks: number[],
    workers: number[],
    pills: number,
    strength: number,
): number {
    tasks.sort((a, b) => a - b);
    workers.sort((a, b) => a - b);

    const n = tasks.length;
    const m = workers.length;

    const check = (x: number): boolean => {
        const dq = new Array<number>(x);
        let head = 0;
        let tail = 0;
        const empty = () => head === tail;
        const pushBack = (val: number) => {
            dq[tail++] = val;
        };
        const popFront = () => {
            head++;
        };
        const popBack = () => {
            tail--;
        };
        const front = () => dq[head];

        let i = 0;
        let p = pills;

        for (let j = m - x; j < m; j++) {
            while (i < x && tasks[i] <= workers[j] + strength) {
                pushBack(tasks[i]);
                i++;
            }

            if (empty()) return false;

            if (front() <= workers[j]) {
                popFront();
            } else {
                if (p === 0) return false;
                p--;
                popBack();
            }
        }
        return true;
    };

    let [left, right] = [0, Math.min(n, m)];
    while (left < right) {
        const mid = (left + right + 1) >> 1;
        if (check(mid)) left = mid;
        else right = mid - 1;
    }
    return left;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
