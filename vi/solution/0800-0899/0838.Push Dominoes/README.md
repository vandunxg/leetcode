---
comments: true
difficulty: Medium
tags:
    - Two Pointers
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [838. Push Dominoes](https://leetcode.com/problems/push-dominoes)

[中文文档](/solution/0800-0899/0838.Push%20Dominoes/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> quân domino xếp thành một hàng, ban đầu mỗi quân đều đứng thẳng. Lúc đầu, ta đồng thời đẩy một số quân sang trái hoặc sang phải.</p>

<p>Sau mỗi giây, mỗi quân đang đổ sang trái sẽ đẩy quân liền kề bên trái. Tương tự, quân đang đổ sang phải sẽ đẩy quân liền kề bên phải.</p>

<p>Nếu một quân đang đứng thẳng bị các quân hai bên đẩy từ cả hai phía, quân đó sẽ đứng yên do hai lực cân bằng.</p>

<p>Trong bài này, một quân đang đổ không tạo thêm lực lên quân đang đổ hoặc đã đổ.</p>

<p>Cho chuỗi <code>dominoes</code> biểu diễn trạng thái ban đầu, trong đó:</p>

<ul>
	<li><code>dominoes[i] = &#39;L&#39;</code> nếu quân domino thứ <code>i</code> bị đẩy sang trái,</li>
	<li><code>dominoes[i] = &#39;R&#39;</code> nếu quân domino thứ <code>i</code> bị đẩy sang phải, và</li>
	<li><code>dominoes[i] = &#39;.&#39;</code> nếu quân domino thứ <code>i</code> chưa bị đẩy.</li>
</ul>

<p>Trả về <em>chuỗi biểu diễn trạng thái cuối cùng</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> dominoes = &quot;RR.L&quot;
<strong>Output:</strong> &quot;RR.L&quot;
<strong>Giải thích:</strong> Quân domino đầu tiên không tạo thêm lực lên quân thứ hai.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0838.Push%20Dominoes/images/domino.png" style="height: 196px; width: 512px;" />
<pre>
<strong>Input:</strong> dominoes = &quot;.L.R...LR..L..&quot;
<strong>Output:</strong> &quot;LL.RR.LLRRLL..&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == dominoes.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>dominoes[i]</code> là một trong các giá trị <code>&#39;L&#39;</code>, <code>&#39;R&#39;</code> hoặc <code>&#39;.&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS đa nguồn

<!-- thinking:start -->

> **Tư duy**
>
> Các quân domino đổ đồng thời; lực từ hai hướng đối nhau sẽ triệt tiêu. Vì $n\le 10^5$, mô phỏng toàn bộ hàng sau từng nhịp sẽ chậm. Lực lan ra từ các quân đã đổ, nên có thể dùng BFS đa nguồn.
>
> Đưa mọi quân ban đầu có hướng $L$ hoặc $R$ vào queue. Một vị trí chỉ đổ khi tại thời điểm đó nó nhận lực từ một hướng; nếu nhận lực từ cả hai hướng trong cùng giây thì vẫn đứng thẳng. Duyệt theo từng layer thời gian sẽ thu được chuỗi cuối cùng.

<!-- thinking:end -->

Xem mọi quân domino được đẩy ban đầu (`L` hoặc `R`) là **source**, đồng thời lan truyền lực ra xung quanh. Dùng queue để chạy BFS theo từng layer (0, 1, 2, ...):

Ta định nghĩa $\text{time[i]}$ để ghi lại thời điểm đầu tiên quân domino thứ _i_ chịu tác động của lực; `-1` nghĩa là quân đó chưa bị tác động. Ta cũng định nghĩa $\text{force[i]}$ là danh sách có độ dài thay đổi, lưu các hướng (`'L'`, `'R'`) của lực tác động lên quân tại cùng một thời điểm. Ban đầu, đưa chỉ số của tất cả quân `L/R` vào queue và đặt `time` của chúng bằng 0.

Khi lấy chỉ số _i_ khỏi queue, nếu $\text{force[i]}$ chỉ chứa một hướng thì quân domino sẽ đổ theo hướng đó, ký hiệu là $f$. Gọi chỉ số của quân kế tiếp là:

$$
j =
\begin{cases}
i - 1, & f = L,\\
i + 1, & f = R.
\end{cases}
$$

Nếu $0 \leq j < n$:

- Nếu $\text{time[j]} = -1$, nghĩa là _j_ chưa bị tác động. Ghi $\text{time[j]} = \text{time[i]} + 1$, đưa nó vào queue và thêm $f$ vào $\text{force[j]}$.
- Nếu $\text{time[j]} = \text{time[i]} + 1$, nghĩa là _j_ đã nhận một lực khác tại cùng thời điểm kế tiếp. Khi đó, thêm $f$ vào $\text{force[j]}$, tạo thế cân bằng. Vì $\text{len(force[j])} = 2$, quân này sẽ đứng thẳng.

Sau khi queue rỗng, mọi vị trí có $\text{force[i]}$ dài 1 sẽ đổ theo hướng tương ứng, còn vị trí có độ dài 2 vẫn là `.`. Cuối cùng, nối mảng ký tự để tạo đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, với $n$ là số quân domino.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def pushDominoes(self, dominoes: str) -> str:
        n = len(dominoes)
        q = deque()
        time = [-1] * n
        force = defaultdict(list)
        for i, f in enumerate(dominoes):
            if f != '.':
                q.append(i)
                time[i] = 0
                force[i].append(f)
        ans = ['.'] * n
        while q:
            i = q.popleft()
            if len(force[i]) == 1:
                ans[i] = f = force[i][0]
                j = i - 1 if f == 'L' else i + 1
                if 0 <= j < n:
                    t = time[i]
                    if time[j] == -1:
                        q.append(j)
                        time[j] = t + 1
                        force[j].append(f)
                    elif time[j] == t + 1:
                        force[j].append(f)
        return ''.join(ans)
```

#### Java

```java
class Solution {
    public String pushDominoes(String dominoes) {
        int n = dominoes.length();
        Deque<Integer> q = new ArrayDeque<>();
        int[] time = new int[n];
        Arrays.fill(time, -1);
        List<Character>[] force = new List[n];
        for (int i = 0; i < n; ++i) {
            force[i] = new ArrayList<>();
        }
        for (int i = 0; i < n; ++i) {
            char f = dominoes.charAt(i);
            if (f != '.') {
                q.offer(i);
                time[i] = 0;
                force[i].add(f);
            }
        }
        char[] ans = new char[n];
        Arrays.fill(ans, '.');
        while (!q.isEmpty()) {
            int i = q.poll();
            if (force[i].size() == 1) {
                ans[i] = force[i].get(0);
                char f = ans[i];
                int j = f == 'L' ? i - 1 : i + 1;
                if (j >= 0 && j < n) {
                    int t = time[i];
                    if (time[j] == -1) {
                        q.offer(j);
                        time[j] = t + 1;
                        force[j].add(f);
                    } else if (time[j] == t + 1) {
                        force[j].add(f);
                    }
                }
            }
        }
        return new String(ans);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string pushDominoes(string dominoes) {
        int n = dominoes.size();
        queue<int> q;
        vector<int> time(n, -1);
        vector<string> force(n);
        for (int i = 0; i < n; i++) {
            if (dominoes[i] == '.') continue;
            q.emplace(i);
            time[i] = 0;
            force[i].push_back(dominoes[i]);
        }

        string ans(n, '.');
        while (!q.empty()) {
            int i = q.front();
            q.pop();
            if (force[i].size() == 1) {
                char f = force[i][0];
                ans[i] = f;
                int j = (f == 'L') ? (i - 1) : (i + 1);
                if (j >= 0 && j < n) {
                    int t = time[i];
                    if (time[j] == -1) {
                        q.emplace(j);
                        time[j] = t + 1;
                        force[j].push_back(f);
                    } else if (time[j] == t + 1)
                        force[j].push_back(f);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func pushDominoes(dominoes string) string {
	n := len(dominoes)
	q := []int{}
	time := make([]int, n)
	for i := range time {
		time[i] = -1
	}
	force := make([][]byte, n)
	for i, c := range dominoes {
		if c != '.' {
			q = append(q, i)
			time[i] = 0
			force[i] = append(force[i], byte(c))
		}
	}

	ans := bytes.Repeat([]byte{'.'}, n)
	for len(q) > 0 {
		i := q[0]
		q = q[1:]
		if len(force[i]) > 1 {
			continue
		}
		f := force[i][0]
		ans[i] = f
		j := i - 1
		if f == 'R' {
			j = i + 1
		}
		if 0 <= j && j < n {
			t := time[i]
			if time[j] == -1 {
				q = append(q, j)
				time[j] = t + 1
				force[j] = append(force[j], f)
			} else if time[j] == t+1 {
				force[j] = append(force[j], f)
			}
		}
	}
	return string(ans)
}
```

#### TypeScript

```ts
function pushDominoes(dominoes: string): string {
    const n = dominoes.length;
    const q: number[] = [];
    const time: number[] = Array(n).fill(-1);
    const force: string[][] = Array.from({ length: n }, () => []);

    for (let i = 0; i < n; i++) {
        const f = dominoes[i];
        if (f !== '.') {
            q.push(i);
            time[i] = 0;
            force[i].push(f);
        }
    }

    const ans: string[] = Array(n).fill('.');
    let head = 0;
    while (head < q.length) {
        const i = q[head++];
        if (force[i].length === 1) {
            const f = force[i][0];
            ans[i] = f;
            const j = f === 'L' ? i - 1 : i + 1;
            if (j >= 0 && j < n) {
                const t = time[i];
                if (time[j] === -1) {
                    q.push(j);
                    time[j] = t + 1;
                    force[j].push(f);
                } else if (time[j] === t + 1) {
                    force[j].push(f);
                }
            }
        }
    }
    return ans.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
