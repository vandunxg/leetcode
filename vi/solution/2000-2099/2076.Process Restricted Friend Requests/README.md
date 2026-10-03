---
comments: true
difficulty: Hard
rating: 2130
source: Weekly Contest 267 Q4
tags:
    - Union Find
    - Graph
---

<!-- problem:start -->

# [2076. Process Restricted Friend Requests](https://leetcode.com/problems/process-restricted-friend-requests)

[中文文档](/solution/2000-2099/2076.Process%20Restricted%20Friend%20Requests/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code> biểu thị số người trong một mạng lưới. Mỗi người được gán nhãn từ <code>0</code> đến <code>n - 1</code>.</p>

<p>Bạn cũng được cho một mảng số nguyên 2 chiều <strong>được đánh chỉ số từ 0</strong> <code>restrictions</code>, trong đó <code>restrictions[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> nghĩa là người <code>x<sub>i</sub></code> và người <code>y<sub>i</sub></code> <strong>không thể </strong>trở thành <strong>bạn bè</strong>,<strong> </strong>dù là <strong>trực tiếp</strong> hay <strong>gián tiếp</strong> thông qua những người khác.</p>

<p>Ban đầu, không ai là bạn của ai. Bạn được cho một danh sách yêu cầu kết bạn dưới dạng mảng số nguyên 2 chiều <strong>được đánh chỉ số từ 0</strong> <code>requests</code>, trong đó <code>requests[j] = [u<sub>j</sub>, v<sub>j</sub>]</code> là yêu cầu kết bạn giữa người <code>u<sub>j</sub></code> và người <code>v<sub>j</sub></code>.</p>

<p>Một yêu cầu kết bạn <strong>thành công</strong> nếu <code>u<sub>j</sub></code> và <code>v<sub>j</sub></code> có thể trở thành <strong>bạn bè</strong>. Mỗi yêu cầu kết bạn được xử lý theo thứ tự đã cho (tức là <code>requests[j]</code> xuất hiện trước <code>requests[j + 1]</code>), và nếu yêu cầu thành công, <code>u<sub>j</sub></code> và <code>v<sub>j</sub></code> sẽ <strong>trở thành bạn trực tiếp</strong> đối với mọi yêu cầu kết bạn về sau.</p>

<p>Trả về <em>một <strong>mảng boolean</strong> </em><code>result</code>,<em> trong đó </em><code>result[j]</code><em> là </em><code>true</code><em> nếu yêu cầu kết bạn thứ </em><code>j<sup>th</sup></code><em> <strong>thành công</strong>, hoặc </em><code>false</code><em> nếu không thành công</em>.</p>

<p><strong>Lưu ý:</strong> Nếu <code>u<sub>j</sub></code> và <code>v<sub>j</sub></code> đã là bạn trực tiếp, yêu cầu vẫn được xem là <strong>thành công</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, restrictions = [[0,1]], requests = [[0,2],[2,1]]
<strong>Đầu ra:</strong> [true,false]
<strong>Giải thích:
</strong>Yêu cầu 0: Người 0 và người 2 có thể trở thành bạn, nên họ trở thành bạn trực tiếp.
Yêu cầu 1: Người 2 và người 1 không thể trở thành bạn vì người 0 và người 1 sẽ trở thành bạn gián tiếp (1--2--0).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, restrictions = [[0,1]], requests = [[1,2],[0,2]]
<strong>Đầu ra:</strong> [true,false]
<strong>Giải thích:
</strong>Yêu cầu 0: Người 1 và người 2 có thể trở thành bạn, nên họ trở thành bạn trực tiếp.
Yêu cầu 1: Người 0 và người 2 không thể trở thành bạn vì người 0 và người 1 sẽ trở thành bạn gián tiếp (0--2--1).
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5, restrictions = [[0,1],[1,2],[2,3]], requests = [[0,4],[1,2],[3,1],[3,4]]
<strong>Đầu ra:</strong> [true,false,true,false]
<strong>Giải thích:
</strong>Yêu cầu 0: Người 0 và người 4 có thể trở thành bạn, nên họ trở thành bạn trực tiếp.
Yêu cầu 1: Người 1 và người 2 không thể trở thành bạn vì họ bị hạn chế trực tiếp.
Yêu cầu 2: Người 3 và người 1 có thể trở thành bạn, nên họ trở thành bạn trực tiếp.
Yêu cầu 3: Người 3 và người 4 không thể trở thành bạn vì người 0 và người 1 sẽ trở thành bạn gián tiếp (0--4--3--1).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 1000</code></li>
	<li><code>0 &lt;= restrictions.length &lt;= 1000</code></li>
	<li><code>restrictions[i].length == 2</code></li>
	<li><code>0 &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>x<sub>i</sub> != y<sub>i</sub></code></li>
	<li><code>1 &lt;= requests.length &lt;= 1000</code></li>
	<li><code>requests[j].length == 2</code></li>
	<li><code>0 &lt;= u<sub>j</sub>, v<sub>j</sub> &lt;= n - 1</code></li>
	<li><code>u<sub>j</sub> != v<sub>j</sub></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Union-Find

<!-- thinking:start -->

> **Tư duy**
>
> Các request được xử lý tuần tự và một restriction cấm hai người cùng thuộc một component. Union-find lưu các mối quan hệ bạn bè. Với $q,m,n \le 1000$, mỗi request có thể duyệt toàn bộ restrictions.
>
> Những cặp đã hợp nhất được chấp nhận; nếu không, ta từ chối khi có restriction mà hai đầu mút nằm trong hai component tương ứng, và chỉ hợp nhất khi request thành công. Path compression giúp các thao tác find nhanh hơn.

<!-- thinking:end -->

Ta có thể sử dụng một tập hợp union-find để duy trì các mối quan hệ bạn bè, sau đó với mỗi yêu cầu, xác định xem yêu cầu đó có vi phạm điều kiện hạn chế hay không.

Với hai người $(u, v)$ trong yêu cầu hiện tại, nếu họ đã là bạn thì có thể chấp nhận yêu cầu ngay; nếu không, ta duyệt qua các điều kiện hạn chế. Nếu tồn tại một điều kiện $(x, y)$ sao cho $u$ và $x$ là bạn, đồng thời $v$ và $y$ là bạn, hoặc $u$ và $y$ là bạn, đồng thời $v$ và $x$ là bạn, thì không thể chấp nhận yêu cầu.

Độ phức tạp thời gian là $O(q \times m \times \log(n))$, còn độ phức tạp không gian là $O(n)$. Trong đó, $q$ và $m$ lần lượt là số yêu cầu và số điều kiện hạn chế.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def friendRequests(
        self, n: int, restrictions: List[List[int]], requests: List[List[int]]
    ) -> List[bool]:
        def find(x: int) -> int:
            if p[x] != x:
                p[x] = find(p[x])
            return p[x]

        p = list(range(n))
        ans = []
        for u, v in requests:
            pu, pv = find(u), find(v)
            if pu == pv:
                ans.append(True)
            else:
                ok = True
                for x, y in restrictions:
                    px, py = find(x), find(y)
                    if (pu == px and pv == py) or (pu == py and pv == px):
                        ok = False
                        break
                ans.append(ok)
                if ok:
                    p[pu] = pv
        return ans
```

#### Java

```java
class Solution {
    private int[] p;

    public boolean[] friendRequests(int n, int[][] restrictions, int[][] requests) {
        p = new int[n];
        for (int i = 0; i < n; ++i) {
            p[i] = i;
        }
        int m = requests.length;
        boolean[] ans = new boolean[m];
        for (int i = 0; i < m; ++i) {
            int u = requests[i][0], v = requests[i][1];
            int pu = find(u), pv = find(v);
            if (pu == pv) {
                ans[i] = true;
            } else {
                boolean ok = true;
                for (var r : restrictions) {
                    int px = find(r[0]), py = find(r[1]);
                    if ((pu == px && pv == py) || (pu == py && pv == px)) {
                        ok = false;
                        break;
                    }
                }
                if (ok) {
                    ans[i] = true;
                    p[pu] = pv;
                }
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
    vector<bool> friendRequests(int n, vector<vector<int>>& restrictions, vector<vector<int>>& requests) {
        vector<int> p(n);
        iota(p.begin(), p.end(), 0);
        function<int(int)> find = [&](int x) {
            if (p[x] != x) {
                p[x] = find(p[x]);
            }
            return p[x];
        };
        vector<bool> ans;
        for (auto& req : requests) {
            int u = req[0], v = req[1];
            int pu = find(u), pv = find(v);
            if (pu == pv) {
                ans.push_back(true);
            } else {
                bool ok = true;
                for (auto& r : restrictions) {
                    int px = find(r[0]), py = find(r[1]);
                    if ((pu == px && pv == py) || (pu == py && pv == px)) {
                        ok = false;
                        break;
                    }
                }
                ans.push_back(ok);
                if (ok) {
                    p[pu] = pv;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func friendRequests(n int, restrictions [][]int, requests [][]int) (ans []bool) {
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
	for _, req := range requests {
		pu, pv := find(req[0]), find(req[1])
		if pu == pv {
			ans = append(ans, true)
		} else {
			ok := true
			for _, r := range restrictions {
				px, py := find(r[0]), find(r[1])
				if px == pu && py == pv || px == pv && py == pu {
					ok = false
					break
				}
			}
			ans = append(ans, ok)
			if ok {
				p[pv] = pu
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function friendRequests(n: number, restrictions: number[][], requests: number[][]): boolean[] {
    const p: number[] = Array.from({ length: n }, (_, i) => i);
    const find = (x: number): number => {
        if (p[x] !== x) {
            p[x] = find(p[x]);
        }
        return p[x];
    };
    const ans: boolean[] = [];
    for (const [u, v] of requests) {
        const pu = find(u);
        const pv = find(v);
        if (pu === pv) {
            ans.push(true);
        } else {
            let ok = true;
            for (const [x, y] of restrictions) {
                const px = find(x);
                const py = find(y);
                if ((px === pu && py === pv) || (px === pv && py === pu)) {
                    ok = false;
                    break;
                }
            }
            ans.push(ok);
            if (ok) {
                p[pu] = pv;
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
