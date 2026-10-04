---
comments: true
difficulty: Medium
rating: 2405
source: Biweekly Contest 107 Q4
tags:
    - Array
    - Hash Table
    - Sorting
    - Sliding Window
---

<!-- problem:start -->

# [2747. Count Zero Request Servers](https://leetcode.com/problems/count-zero-request-servers)

[中文文档](/solution/2700-2799/2747.Count%20Zero%20Request%20Servers/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code> biểu thị tổng số server và một mảng số nguyên <strong>2D</strong> <strong>đánh chỉ số từ 0 </strong><code>logs</code>, trong đó <code>logs[i] = [server_id, time]</code> biểu thị server có id <code>server_id</code> đã nhận một request tại thời điểm <code>time</code>.</p>

<p>Bạn cũng được cho một số nguyên <code>x</code> và một mảng số nguyên <strong>đánh chỉ số từ 0</strong> <code>queries</code>.</p>

<p>Trả về <em>một <strong>mảng số nguyên đánh chỉ số từ 0</strong></em> <code>arr</code> <em>có độ dài bằng</em> <code>queries.length</code> <em>trong đó</em> <code>arr[i]</code> <em>biểu thị số lượng server <strong>không nhận</strong> bất kỳ request nào trong khoảng thời gian</em> <code>[queries[i] - x, queries[i]]</code>.</p>

<p>Lưu ý rằng các khoảng thời gian bao gồm cả hai đầu mút.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, logs = [[1,3],[2,6],[1,5]], x = 5, queries = [10,11]
<strong>Đầu ra:</strong> [1,2]
<strong>Giải thích:</strong>
Với queries[0]: Các server có id 1 và 2 nhận request trong khoảng thời gian [5, 10]. Do đó, chỉ server 3 không nhận request nào.
Với queries[1]: Chỉ server có id 2 nhận request trong khoảng thời gian [6,11]. Do đó, các server có id 1 và 3 là những server duy nhất không nhận request nào trong khoảng thời gian đó.

</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, logs = [[2,4],[2,1],[1,2],[3,1]], x = 2, queries = [3,4]
<strong>Đầu ra:</strong> [0,1]
<strong>Giải thích:</strong>
Với queries[0]: Tất cả server đều nhận ít nhất một request trong khoảng thời gian [1, 3].
Với queries[1]: Chỉ server có id 3 không nhận request nào trong khoảng thời gian [2,4].

</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= logs.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code><font face="monospace">logs[i].length == 2</font></code></li>
	<li><code>1 &lt;= logs[i][0] &lt;= n</code></li>
	<li><code>1 &lt;= logs[i][1] &lt;= 10<sup>6</sup></code></li>
	<li><code>1 &lt;= x &lt;= 10<sup>5</sup></code></li>
	<li><code>x &lt;&nbsp;queries[i]&nbsp;&lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Offline Queries + Sorting + Two Pointers

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi thời điểm $q$, ta đếm số server không có log nào trong $[q-x,q]$. Nếu duyệt các log cho từng query, độ phức tạp sẽ là $O(qm)$.
>
> Sắp xếp các query theo điểm kết thúc và các log theo thời gian. Hai con trỏ thêm các log đi vào window và loại bỏ các log rời khỏi window; một hash map lưu các server khác nhau trong window. Đáp án là $n$ trừ đi số lượng server đó.

<!-- thinking:end -->

Ta có thể sắp xếp tất cả query theo thời gian từ nhỏ đến lớn, sau đó xử lý từng query theo thứ tự thời gian.

Với mỗi query $q = (r, i)$, biên trái của window là $l = r - x$, và ta cần đếm số server đã nhận request trong window $[l, r]$. Ta dùng hai con trỏ $j$ và $k$ để duy trì biên trái và phải của window, ban đầu $j = k = 0$. Mỗi lần, nếu thời gian của log mà $k$ đang trỏ tới nhỏ hơn hoặc bằng $r$, ta thêm log đó vào window, rồi dịch $k$ sang phải một vị trí. Nếu thời gian của log mà $j$ đang trỏ tới nhỏ hơn $l$, ta xóa log đó khỏi window, rồi dịch $j$ sang phải một vị trí. Trong quá trình dịch chuyển, ta cần đếm số server khác nhau trong window, việc này có thể được thực hiện bằng hash table. Sau khi dịch chuyển xong, số server không nhận request nào trong khoảng thời gian hiện tại bằng $n$ trừ đi số server khác nhau trong hash table.

Độ phức tạp thời gian là $O(l \times \log l + m \times \log m + n)$, và độ phức tạp không gian là $O(l + m)$. Ở đây, $l$ và $n$ lần lượt là độ dài của mảng $\textit{logs}$ và số lượng server, còn $m$ là độ dài của mảng $\textit{queries}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countServers(
        self, n: int, logs: List[List[int]], x: int, queries: List[int]
    ) -> List[int]:
        cnt = Counter()
        logs.sort(key=lambda x: x[1])
        ans = [0] * len(queries)
        j = k = 0
        for r, i in sorted(zip(queries, count())):
            l = r - x
            while k < len(logs) and logs[k][1] <= r:
                cnt[logs[k][0]] += 1
                k += 1
            while j < len(logs) and logs[j][1] < l:
                cnt[logs[j][0]] -= 1
                if cnt[logs[j][0]] == 0:
                    cnt.pop(logs[j][0])
                j += 1
            ans[i] = n - len(cnt)
        return ans
```

#### Java

```java
class Solution {
    public int[] countServers(int n, int[][] logs, int x, int[] queries) {
        Arrays.sort(logs, (a, b) -> a[1] - b[1]);
        int m = queries.length;
        int[][] qs = new int[m][0];
        for (int i = 0; i < m; ++i) {
            qs[i] = new int[] {queries[i], i};
        }
        Arrays.sort(qs, (a, b) -> a[0] - b[0]);
        Map<Integer, Integer> cnt = new HashMap<>();
        int[] ans = new int[m];
        int j = 0, k = 0;
        for (var q : qs) {
            int r = q[0], i = q[1];
            int l = r - x;
            while (k < logs.length && logs[k][1] <= r) {
                cnt.merge(logs[k++][0], 1, Integer::sum);
            }
            while (j < logs.length && logs[j][1] < l) {
                if (cnt.merge(logs[j][0], -1, Integer::sum) == 0) {
                    cnt.remove(logs[j][0]);
                }
                j++;
            }
            ans[i] = n - cnt.size();
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> countServers(int n, vector<vector<int>>& logs, int x, vector<int>& queries) {
        sort(logs.begin(), logs.end(), [](const auto& a, const auto& b) {
            return a[1] < b[1];
        });
        int m = queries.size();
        vector<pair<int, int>> qs(m);
        for (int i = 0; i < m; ++i) {
            qs[i] = {queries[i], i};
        }
        sort(qs.begin(), qs.end());
        unordered_map<int, int> cnt;
        vector<int> ans(m);
        int j = 0, k = 0;
        for (auto& [r, i] : qs) {
            int l = r - x;
            while (k < logs.size() && logs[k][1] <= r) {
                ++cnt[logs[k++][0]];
            }
            while (j < logs.size() && logs[j][1] < l) {
                if (--cnt[logs[j][0]] == 0) {
                    cnt.erase(logs[j][0]);
                }
                ++j;
            }
            ans[i] = n - cnt.size();
        }
        return ans;
    }
};
```

#### Go

```go
func countServers(n int, logs [][]int, x int, queries []int) []int {
	sort.Slice(logs, func(i, j int) bool { return logs[i][1] < logs[j][1] })
	m := len(queries)
	qs := make([][2]int, m)
	for i, q := range queries {
		qs[i] = [2]int{q, i}
	}
	sort.Slice(qs, func(i, j int) bool { return qs[i][0] < qs[j][0] })
	cnt := map[int]int{}
	ans := make([]int, m)
	j, k := 0, 0
	for _, q := range qs {
		r, i := q[0], q[1]
		l := r - x
		for k < len(logs) && logs[k][1] <= r {
			cnt[logs[k][0]]++
			k++
		}
		for j < len(logs) && logs[j][1] < l {
			cnt[logs[j][0]]--
			if cnt[logs[j][0]] == 0 {
				delete(cnt, logs[j][0])
			}
			j++
		}
		ans[i] = n - len(cnt)
	}
	return ans
}
```

#### TypeScript

```ts
function countServers(n: number, logs: number[][], x: number, queries: number[]): number[] {
    logs.sort((a, b) => a[1] - b[1]);
    const m = queries.length;
    const qs: number[][] = [];
    for (let i = 0; i < m; ++i) {
        qs.push([queries[i], i]);
    }
    qs.sort((a, b) => a[0] - b[0]);
    const cnt: Map<number, number> = new Map();
    const ans: number[] = new Array(m);
    let j = 0;
    let k = 0;
    for (const [r, i] of qs) {
        const l = r - x;
        while (k < logs.length && logs[k][1] <= r) {
            cnt.set(logs[k][0], (cnt.get(logs[k][0]) || 0) + 1);
            ++k;
        }
        while (j < logs.length && logs[j][1] < l) {
            cnt.set(logs[j][0], (cnt.get(logs[j][0]) || 0) - 1);
            if (cnt.get(logs[j][0]) === 0) {
                cnt.delete(logs[j][0]);
            }
            ++j;
        }
        ans[i] = n - cnt.size;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
