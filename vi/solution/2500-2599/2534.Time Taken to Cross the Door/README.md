---
comments: true
difficulty: Hard
tags:
    - Queue
    - Array
    - Simulation
---

<!-- problem:start -->

# [2534. Time Taken to Cross the Door 🔒](https://leetcode.com/problems/time-taken-to-cross-the-door)

[中文文档](/solution/2500-2599/2534.Time%20Taken%20to%20Cross%20the%20Door/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> người được đánh số từ <code>0</code> đến <code>n - 1</code> và một cánh cửa. Mỗi người có thể đi vào hoặc đi ra qua cửa một lần, mất một giây.</p>

<p>Bạn được cho một mảng số nguyên <strong>không giảm</strong> <code>arrival</code> có kích thước <code>n</code>, trong đó <code>arrival[i]</code> là thời điểm người thứ <code>i<sup>th</sup></code> đến cửa. Bạn cũng được cho một mảng <code>state</code> có kích thước <code>n</code>, trong đó <code>state[i]</code> là <code>0</code> nếu người <code>i</code> muốn đi vào qua cửa hoặc <code>1</code> nếu họ muốn đi ra qua cửa.</p>

<p>Nếu có từ hai người trở lên muốn sử dụng cửa tại <strong>cùng một thời điểm</strong>, họ tuân theo các quy tắc sau:</p>

<ul>
	<li>Nếu cửa <strong>không được sử dụng ở giây trước đó</strong>, người muốn <strong>đi ra</strong> được ưu tiên.</li>
	<li>Nếu cửa được sử dụng ở giây trước đó để <strong>đi vào</strong>, người muốn đi vào được ưu tiên.</li>
	<li>Nếu cửa được sử dụng ở giây trước đó để <strong>đi ra</strong>, người muốn <strong>đi ra</strong> được ưu tiên.</li>
	<li>Nếu có nhiều người muốn đi cùng một hướng, người có <strong>chỉ số</strong> nhỏ nhất được ưu tiên.</li>
</ul>

<p>Trả về <em>một mảng </em><code>answer</code><em> có kích thước </em><code>n</code><em>, trong đó </em><code>answer[i]</code><em> là giây mà người thứ </em><code>i<sup>th</sup></code><em> đi qua cửa</em>.</p>

<p><strong>Lưu ý</strong> rằng:</p>

<ul>
	<li>Chỉ một người có thể đi qua cửa trong mỗi giây.</li>
	<li>Một người có thể đến cửa và chờ mà chưa đi vào hoặc đi ra để tuân theo các quy tắc đã nêu.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> arrival = [0,1,1,2,4], state = [0,1,0,0,1]
<strong>Output:</strong> [0,3,1,2,4]
<strong>Giải thích:</strong> Ở mỗi giây, ta có:
- Tại t = 0: Người 0 là người duy nhất muốn đi vào, nên họ đi vào qua cửa.
- Tại t = 1: Người 1 muốn đi ra, còn người 2 muốn đi vào. Vì cửa đã được sử dụng ở giây trước đó để đi vào, người 2 đi vào.
- Tại t = 2: Người 1 vẫn muốn đi ra, còn người 3 muốn đi vào. Vì cửa đã được sử dụng ở giây trước đó để đi vào, người 3 đi vào.
- Tại t = 3: Người 1 là người duy nhất muốn đi ra, nên họ đi ra qua cửa.
- Tại t = 4: Người 4 là người duy nhất muốn đi ra, nên họ đi ra qua cửa.
</pre>

<p><strong>Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> arrival = [0,0,0], state = [1,0,1]
<strong>Output:</strong> [0,2,1]
<strong>Giải thích:</strong> Ở mỗi giây, ta có:
- Tại t = 0: Người 1 muốn đi vào, trong khi người 0 và người 2 muốn đi ra. Vì cửa chưa được sử dụng ở giây trước đó, những người muốn đi ra được ưu tiên. Vì người 0 có chỉ số nhỏ hơn, họ đi ra trước.
- Tại t = 1: Người 1 muốn đi vào, còn người 2 muốn đi ra. Vì cửa đã được sử dụng ở giây trước đó để đi ra, người 2 đi ra.
- Tại t = 2: Người 1 là người duy nhất muốn đi vào, nên họ đi vào qua cửa.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == arrival.length == state.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= arrival[i] &lt;= n</code></li>
	<li><code>arrival</code> được sắp xếp theo thứ tự <strong>không giảm</strong>.</li>
	<li><code>state[i]</code> chỉ có thể là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Queue + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Cửa cho một người đi qua mỗi giây. Khi có người chờ ở cả hai phía, cửa tiếp tục ưu tiên hướng của giây trước; sau một khoảng thời gian không có người sử dụng, cửa ưu tiên chiều đi ra. Vì thời điểm đến được cho trong $O(n)$, việc duyệt từng giây vẫn có độ phức tạp tuyến tính.
>
> Ta dùng hai queue để lưu các yêu cầu đi vào và đi ra. Ở thời điểm $t$, thêm vào queue tất cả những người đã đến. Nếu cả hai queue đều không rỗng, lấy phần tử ở phía $st$; nếu chỉ một queue không rỗng, cập nhật $st$ thành phía đó; nếu cả hai đều rỗng, đặt lại $st$ thành chiều đi ra. Sau đó ghi nhận thời điểm đi qua cửa của từng người.

<!-- thinking:end -->

Ta định nghĩa hai queue, trong đó $q[0]$ lưu chỉ số của những người muốn đi vào, còn $q[1]$ lưu chỉ số của những người muốn đi ra.

Ta duy trì biến $t$ biểu diễn thời điểm hiện tại và biến $st$ biểu diễn trạng thái hiện tại của cửa. Khi $st = 1$, điều đó có nghĩa là cửa không được sử dụng hoặc có người đã đi ra ở giây trước đó. Khi $st = 0$, điều đó có nghĩa là có người đã đi vào ở giây trước đó. Ban đầu, $t = 0$ và $st = 1$.

Ta duyệt mảng $\textit{arrival}$. Với mỗi người, nếu thời điểm hiện tại $t$ lớn hơn hoặc bằng thời điểm người đó đến cửa $\textit{arrival}[i]$, ta thêm chỉ số của người đó vào queue tương ứng $q[\text{state}[i]]$.

Sau đó, ta kiểm tra xem cả hai queue $q[0]$ và $q[1]$ có khác rỗng hay không. Nếu cả hai đều không rỗng, ta lấy phần tử đầu của queue $q[st]$ và gán thời điểm hiện tại $t$ làm thời điểm người đó đi qua cửa. Nếu chỉ một queue không rỗng, ta cập nhật giá trị của $st$ dựa trên queue không rỗng, sau đó lấy phần tử đầu của queue đó và gán thời điểm hiện tại $t$ làm thời điểm người đó đi qua cửa. Nếu cả hai queue đều rỗng, ta cập nhật $st$ thành $1$, biểu thị cửa không được sử dụng.

Tiếp theo, ta tăng thời điểm $t$ lên $1$ và tiếp tục duyệt mảng $\textit{arrival}$ cho đến khi tất cả mọi người đã đi qua cửa.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $\textit{arrival}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def timeTaken(self, arrival: List[int], state: List[int]) -> List[int]:
        q = [deque(), deque()]
        n = len(arrival)
        t = i = 0
        st = 1
        ans = [0] * n
        while i < n or q[0] or q[1]:
            while i < n and arrival[i] <= t:
                q[state[i]].append(i)
                i += 1
            if q[0] and q[1]:
                ans[q[st].popleft()] = t
            elif q[0] or q[1]:
                st = 0 if q[0] else 1
                ans[q[st].popleft()] = t
            else:
                st = 1
            t += 1
        return ans
```

#### Java

```java
class Solution {
    public int[] timeTaken(int[] arrival, int[] state) {
        Deque<Integer>[] q = new Deque[2];
        Arrays.setAll(q, i -> new ArrayDeque<>());
        int n = arrival.length;
        int t = 0, i = 0, st = 1;
        int[] ans = new int[n];
        while (i < n || !q[0].isEmpty() || !q[1].isEmpty()) {
            while (i < n && arrival[i] <= t) {
                q[state[i]].add(i++);
            }
            if (!q[0].isEmpty() && !q[1].isEmpty()) {
                ans[q[st].poll()] = t;
            } else if (!q[0].isEmpty() || !q[1].isEmpty()) {
                st = q[0].isEmpty() ? 1 : 0;
                ans[q[st].poll()] = t;
            } else {
                st = 1;
            }
            ++t;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> timeTaken(vector<int>& arrival, vector<int>& state) {
        int n = arrival.size();
        queue<int> q[2];
        int t = 0, i = 0, st = 1;
        vector<int> ans(n);

        while (i < n || !q[0].empty() || !q[1].empty()) {
            while (i < n && arrival[i] <= t) {
                q[state[i]].push(i++);
            }

            if (!q[0].empty() && !q[1].empty()) {
                ans[q[st].front()] = t;
                q[st].pop();
            } else if (!q[0].empty() || !q[1].empty()) {
                st = q[0].empty() ? 1 : 0;
                ans[q[st].front()] = t;
                q[st].pop();
            } else {
                st = 1;
            }

            ++t;
        }

        return ans;
    }
};
```

#### Go

```go
func timeTaken(arrival []int, state []int) []int {
	n := len(arrival)
	q := [2][]int{}
	t, i, st := 0, 0, 1
	ans := make([]int, n)

	for i < n || len(q[0]) > 0 || len(q[1]) > 0 {
		for i < n && arrival[i] <= t {
			q[state[i]] = append(q[state[i]], i)
			i++
		}

		if len(q[0]) > 0 && len(q[1]) > 0 {
			ans[q[st][0]] = t
			q[st] = q[st][1:]
		} else if len(q[0]) > 0 || len(q[1]) > 0 {
			if len(q[0]) == 0 {
				st = 1
			} else {
				st = 0
			}
			ans[q[st][0]] = t
			q[st] = q[st][1:]
		} else {
			st = 1
		}

		t++
	}

	return ans
}
```

#### TypeScript

```ts
function timeTaken(arrival: number[], state: number[]): number[] {
    const n = arrival.length;
    const q: number[][] = [[], []];
    let [t, i, st] = [0, 0, 1];
    const ans: number[] = Array(n).fill(0);

    while (i < n || q[0].length || q[1].length) {
        while (i < n && arrival[i] <= t) {
            q[state[i]].push(i++);
        }

        if (q[0].length && q[1].length) {
            ans[q[st][0]] = t;
            q[st].shift();
        } else if (q[0].length || q[1].length) {
            st = q[0].length ? 0 : 1;
            ans[q[st][0]] = t;
            q[st].shift();
        } else {
            st = 1;
        }

        t++;
    }

    return ans;
}
```

#### Rust

```rust
use std::collections::VecDeque;

impl Solution {
    pub fn time_taken(arrival: Vec<i32>, state: Vec<i32>) -> Vec<i32> {
        let n = arrival.len();
        let mut q = vec![VecDeque::new(), VecDeque::new()];
        let mut t = 0;
        let mut i = 0;
        let mut st = 1;
        let mut ans = vec![-1; n];

        while i < n || !q[0].is_empty() || !q[1].is_empty() {
            while i < n && arrival[i] <= t {
                q[state[i] as usize].push_back(i);
                i += 1;
            }

            if !q[0].is_empty() && !q[1].is_empty() {
                ans[*q[st].front().unwrap()] = t;
                q[st].pop_front();
            } else if !q[0].is_empty() || !q[1].is_empty() {
                st = if q[0].is_empty() { 1 } else { 0 };
                ans[*q[st].front().unwrap()] = t;
                q[st].pop_front();
            } else {
                st = 1;
            }

            t += 1;
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
