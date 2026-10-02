---
comments: true
difficulty: Hard
rating: 2250
source: Weekly Contest 145 Q4
tags:
    - Bit Manipulation
    - Array
    - Dynamic Programming
    - Bitmask
---

<!-- problem:start -->

# [1125. Smallest Sufficient Team](https://leetcode.com/problems/smallest-sufficient-team)

[中文文档](/solution/1100-1199/1125.Smallest%20Sufficient%20Team/README.md)

## Mô tả

<!-- description:start -->

<p>Trong một dự án, bạn có danh sách kỹ năng cần thiết <code>req_skills</code> và danh sách mọi người. Người thứ <code>i<sup>th</sup></code> <code>people[i]</code> có một danh sách các kỹ năng mà người đó sở hữu.</p>

<p>Một team đủ năng lực là một tập hợp người sao cho với mỗi kỹ năng cần thiết trong <code>req_skills</code>, có ít nhất một người trong team sở hữu kỹ năng đó. Ta có thể biểu diễn team bằng chỉ số của từng người.</p>

<ul>
	<li>Ví dụ, <code>team = [0, 1, 3]</code> biểu diễn những người có kỹ năng trong <code>people[0]</code>, <code>people[1]</code> và <code>people[3]</code>.</li>
</ul>

<p>Hãy trả về <em>một team đủ năng lực có số người ít nhất có thể, được biểu diễn bằng chỉ số của từng người</em>. Bạn có thể trả về đáp án theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Đảm bảo luôn tồn tại đáp án.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> req_skills = ["java","nodejs","reactjs"], people = [["java"],["nodejs"],["nodejs","reactjs"]]
<strong>Đầu ra:</strong> [0,2]
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> req_skills = ["algorithms","math","java","reactjs","csharp","aws"], people = [["algorithms","math","java"],["algorithms","math","reactjs"],["java","csharp","aws"],["reactjs","csharp"],["csharp","math"],["aws","java"]]
<strong>Đầu ra:</strong> [1,2]
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= req_skills.length &lt;= 16</code></li>
	<li><code>1 &lt;= req_skills[i].length &lt;= 16</code></li>
	<li><code>req_skills[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li>Tất cả chuỗi trong <code>req_skills</code> đều <strong>khác nhau</strong>.</li>
	<li><code>1 &lt;= people.length &lt;= 60</code></li>
	<li><code>0 &lt;= people[i].length &lt;= 16</code></li>
	<li><code>1 &lt;= people[i][j].length &lt;= 16</code></li>
	<li><code>people[i][j]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li>Tất cả chuỗi trong <code>people[i]</code> đều <strong>khác nhau</strong>.</li>
	<li>Mọi kỹ năng trong <code>people[i]</code> đều nằm trong <code>req_skills</code>.</li>
	<li>Đảm bảo tồn tại một team đủ năng lực.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động nén trạng thái

<!-- thinking:start -->

> **Tư duy**
>
> Có $m\le 16$ kỹ năng, nên tập kỹ năng có thể được biểu diễn bằng $2^m$ trạng thái, trong khi chọn người trực tiếp sẽ có $2^n$ khả năng. Mã hóa mỗi người thành một mask $p[j]$ và đặt $f[i]$ là team nhỏ nhất bao phủ tập kỹ năng $i$.
>
> Từ trạng thái đã đạt được $i$, thêm người $j$ rồi chuyển sang $i\mid p[j]$. Các mảng $g$ và $h$ lưu người được thêm gần nhất và trạng thái trước đó để có thể truy vết danh sách chỉ số từ mask đầy đủ.

<!-- thinking:end -->

Ta nhận thấy độ dài của `req_skills` không vượt quá $16$, vì vậy có thể dùng một số nhị phân dài tối đa $16$ bit để biểu diễn mỗi kỹ năng đã được đáp ứng hay chưa. Gọi độ dài của `req_skills` là $m$ và độ dài của `people` là $n$.

Trước tiên, ta ánh xạ mỗi kỹ năng trong `req_skills` thành một số, tức $d[s]$ biểu diễn số tương ứng với kỹ năng $s$. Sau đó, duyệt từng người trong `people` và biểu diễn các kỹ năng họ có bằng một số nhị phân, tức $p[i]$ biểu diễn các kỹ năng của người có chỉ số $i$.

Tiếp theo, ta định nghĩa ba mảng sau:

- Mảng $f[i]$ biểu diễn số người ít nhất cần có để đáp ứng tập kỹ năng $i$, trong đó mỗi bit bằng $1$ trong biểu diễn nhị phân của $i$ cho biết kỹ năng tương ứng đã được đáp ứng. Ban đầu, $f[0] = 0$, các vị trí còn lại là vô cùng.
- Mảng $g[i]$ biểu diễn chỉ số của người cuối cùng trong team nhỏ nhất đáp ứng tập kỹ năng $i$.
- Mảng $h[i]$ biểu diễn trạng thái tập kỹ năng trước đó khi team nhỏ nhất đáp ứng tập kỹ năng $i$.

Ta lần lượt xét từng tập kỹ năng trong khoảng $[0,..2^m-1]$. Với mỗi tập kỹ năng $i$:

Ta duyệt từng người $j$ trong `people`. Nếu $f[i] + 1 \lt f[i | p[j]]$, điều đó có nghĩa là có thể chuyển sang $f[i | p[j]]$ từ $f[i]$. Khi đó, cập nhật $f[i | p[j]]$ thành $f[i] + 1$, cập nhật $g[i | p[j]]$ thành $j$ và cập nhật $h[i | p[j]]$ thành $i$. Nghĩa là, khi trạng thái tập kỹ năng hiện tại là $i | p[j]$, chỉ số người cuối cùng là $j$, còn trạng thái tập kỹ năng trước đó là $i$. Ở đây, ký hiệu $|$ là phép OR theo bit.

Cuối cùng, bắt đầu từ trạng thái tập kỹ năng $i=2^m-1$, lấy chỉ số người cuối cùng $g[i]$ và thêm vào đáp án, sau đó cập nhật $i$ thành $h[i]$. Tiếp tục truy vết ngược cho đến khi $i=0$ để thu được chỉ số những người trong team nhỏ nhất cần thiết.

Độ phức tạp thời gian là $O(2^m \times n)$ và độ phức tạp không gian là $O(2^m)$, trong đó $m$ và $n$ lần lượt là độ dài của `req_skills` và `people`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestSufficientTeam(
        self, req_skills: List[str], people: List[List[str]]
    ) -> List[int]:
        d = {s: i for i, s in enumerate(req_skills)}
        m, n = len(req_skills), len(people)
        p = [0] * n
        for i, ss in enumerate(people):
            for s in ss:
                p[i] |= 1 << d[s]
        f = [inf] * (1 << m)
        g = [0] * (1 << m)
        h = [0] * (1 << m)
        f[0] = 0
        for i in range(1 << m):
            if f[i] == inf:
                continue
            for j in range(n):
                if f[i] + 1 < f[i | p[j]]:
                    f[i | p[j]] = f[i] + 1
                    g[i | p[j]] = j
                    h[i | p[j]] = i
        i = (1 << m) - 1
        ans = []
        while i:
            ans.append(g[i])
            i = h[i]
        return ans
```

#### Java

```java
class Solution {
    public int[] smallestSufficientTeam(String[] req_skills, List<List<String>> people) {
        Map<String, Integer> d = new HashMap<>();
        int m = req_skills.length;
        int n = people.size();
        for (int i = 0; i < m; ++i) {
            d.put(req_skills[i], i);
        }
        int[] p = new int[n];
        for (int i = 0; i < n; ++i) {
            for (var s : people.get(i)) {
                p[i] |= 1 << d.get(s);
            }
        }
        int[] f = new int[1 << m];
        int[] g = new int[1 << m];
        int[] h = new int[1 << m];
        final int inf = 1 << 30;
        Arrays.fill(f, inf);
        f[0] = 0;
        for (int i = 0; i < 1 << m; ++i) {
            if (f[i] == inf) {
                continue;
            }
            for (int j = 0; j < n; ++j) {
                if (f[i] + 1 < f[i | p[j]]) {
                    f[i | p[j]] = f[i] + 1;
                    g[i | p[j]] = j;
                    h[i | p[j]] = i;
                }
            }
        }
        List<Integer> ans = new ArrayList<>();
        for (int i = (1 << m) - 1; i != 0; i = h[i]) {
            ans.add(g[i]);
        }
        return ans.stream().mapToInt(Integer::intValue).toArray();
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> smallestSufficientTeam(vector<string>& req_skills, vector<vector<string>>& people) {
        unordered_map<string, int> d;
        int m = req_skills.size(), n = people.size();
        for (int i = 0; i < m; ++i) {
            d[req_skills[i]] = i;
        }
        int p[n];
        memset(p, 0, sizeof(p));
        for (int i = 0; i < n; ++i) {
            for (auto& s : people[i]) {
                p[i] |= 1 << d[s];
            }
        }
        int f[1 << m];
        int g[1 << m];
        int h[1 << m];
        memset(f, 63, sizeof(f));
        f[0] = 0;
        for (int i = 0; i < 1 << m; ++i) {
            if (f[i] == 0x3f3f3f3f) {
                continue;
            }
            for (int j = 0; j < n; ++j) {
                if (f[i] + 1 < f[i | p[j]]) {
                    f[i | p[j]] = f[i] + 1;
                    g[i | p[j]] = j;
                    h[i | p[j]] = i;
                }
            }
        }
        vector<int> ans;
        for (int i = (1 << m) - 1; i; i = h[i]) {
            ans.push_back(g[i]);
        }
        return ans;
    }
};
```

#### Go

```go
func smallestSufficientTeam(req_skills []string, people [][]string) (ans []int) {
	d := map[string]int{}
	for i, s := range req_skills {
		d[s] = i
	}
	m, n := len(req_skills), len(people)
	p := make([]int, n)
	for i, ss := range people {
		for _, s := range ss {
			p[i] |= 1 << d[s]
		}
	}
	const inf = 1 << 30
	f := make([]int, 1<<m)
	g := make([]int, 1<<m)
	h := make([]int, 1<<m)
	for i := range f {
		f[i] = inf
	}
	f[0] = 0
	for i := range f {
		if f[i] == inf {
			continue
		}
		for j := 0; j < n; j++ {
			if f[i]+1 < f[i|p[j]] {
				f[i|p[j]] = f[i] + 1
				g[i|p[j]] = j
				h[i|p[j]] = i
			}
		}
	}
	for i := 1<<m - 1; i != 0; i = h[i] {
		ans = append(ans, g[i])
	}
	return
}
```

#### TypeScript

```ts
function smallestSufficientTeam(req_skills: string[], people: string[][]): number[] {
    const d: Map<string, number> = new Map();
    const m = req_skills.length;
    const n = people.length;
    for (let i = 0; i < m; ++i) {
        d.set(req_skills[i], i);
    }
    const p: number[] = new Array(n).fill(0);
    for (let i = 0; i < n; ++i) {
        for (const s of people[i]) {
            p[i] |= 1 << d.get(s)!;
        }
    }
    const inf = 1 << 30;
    const f: number[] = new Array(1 << m).fill(inf);
    const g: number[] = new Array(1 << m).fill(0);
    const h: number[] = new Array(1 << m).fill(0);
    f[0] = 0;
    for (let i = 0; i < 1 << m; ++i) {
        if (f[i] === inf) {
            continue;
        }
        for (let j = 0; j < n; ++j) {
            if (f[i] + 1 < f[i | p[j]]) {
                f[i | p[j]] = f[i] + 1;
                g[i | p[j]] = j;
                h[i | p[j]] = i;
            }
        }
    }
    const ans: number[] = [];
    for (let i = (1 << m) - 1; i; i = h[i]) {
        ans.push(g[i]);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
