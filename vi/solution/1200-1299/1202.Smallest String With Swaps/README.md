---
comments: true
difficulty: Medium
rating: 1855
source: Weekly Contest 155 Q3
tags:
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Array
    - Hash Table
    - String
    - Sorting
---

<!-- problem:start -->

# [1202. Smallest String With Swaps](https://leetcode.com/problems/smallest-string-with-swaps)

[中文文档](/solution/1200-1299/1202.Smallest%20String%20With%20Swaps/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> và mảng các cặp chỉ số <code>pairs</code>, trong đó <code>pairs[i] =&nbsp;[a, b]</code> biểu thị hai chỉ số (đánh số từ 0) trong chuỗi.</p>

<p>Bạn có thể hoán đổi ký tự tại bất kỳ cặp chỉ số nào trong <code>pairs</code> <strong>bao nhiêu lần tùy ý</strong>.</p>

<p>Trả về chuỗi nhỏ nhất theo thứ tự từ điển mà <code>s</code> có thể biến đổi thành sau khi thực hiện các phép hoán đổi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;dcab&quot;, pairs = [[0,3],[1,2]]
<strong>Đầu ra:</strong> &quot;bacd&quot;
<strong>Giải thích:</strong> 
Hoán đổi s[0] và s[3], khi đó s = &quot;bcad&quot;
Hoán đổi s[1] và s[2], khi đó s = &quot;bacd&quot;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;dcab&quot;, pairs = [[0,3],[1,2],[0,2]]
<strong>Đầu ra:</strong> &quot;abcd&quot;
<strong>Giải thích: </strong>
Hoán đổi s[0] và s[3], khi đó s = &quot;bcad&quot;
Hoán đổi s[0] và s[2], khi đó s = &quot;acbd&quot;
Hoán đổi s[1] và s[2], khi đó s = &quot;abcd&quot;</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;cba&quot;, pairs = [[0,1],[1,2]]
<strong>Đầu ra:</strong> &quot;abc&quot;
<strong>Giải thích: </strong>
Hoán đổi s[0] và s[1], khi đó s = &quot;bca&quot;
Hoán đổi s[1] và s[2], khi đó s = &quot;bac&quot;
Hoán đổi s[0] và s[1], khi đó s = &quot;abc&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10^5</code></li>
	<li><code>0 &lt;= pairs.length &lt;= 10^5</code></li>
	<li><code>0 &lt;= pairs[i][0], pairs[i][1] &lt;&nbsp;s.length</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Union-Find

<!-- thinking:start -->

> **Tư duy**
>
> Thử mọi thứ tự hoán đổi là không khả thi khi cả $n$ và $|pairs|$ có thể lên đến $10^5$.
>
> Quan hệ hoán đổi có tính bắc cầu: các chỉ số tạo thành những connected component, và có thể tùy ý sắp xếp lại ký tự bên trong mỗi component. Để chuỗi nhỏ nhất theo thứ tự từ điển, ta gán các ký tự theo thứ tự tăng dần vào các chỉ số của từng component.
>
> Union-find tạo các component; ta gom ký tự theo root, sắp xếp giảm dần rồi lấy ký tự nhỏ nhất để gán lại cho từng chỉ số. Union-find xác định nhóm, còn việc sắp xếp giúp chọn thứ tự gán tối ưu.

<!-- thinking:end -->

Ta nhận thấy các cặp chỉ số có tính bắc cầu: nếu có thể hoán đổi $a$ với $b$, và $b$ với $c$, thì cũng có thể hoán đổi $a$ với $c$. Vì vậy, ta dùng cấu trúc union-find để quản lý tính liên thông giữa các cặp chỉ số, rồi sắp xếp các ký tự thuộc cùng một connected component theo thứ tự từ điển.

Cuối cùng, ta duyệt chuỗi. Tại mỗi vị trí, thay ký tự hiện tại bằng ký tự nhỏ nhất trong connected component tương ứng, rồi loại ký tự đó khỏi component và tiếp tục duyệt.

Độ phức tạp thời gian là $O(n \times \log n + m \times \alpha(m))$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài chuỗi, $m$ là số cặp chỉ số, còn $\alpha$ là hàm Ackermann ngược.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestStringWithSwaps(self, s: str, pairs: List[List[int]]) -> str:
        def find(x: int) -> int:
            if p[x] != x:
                p[x] = find(p[x])
            return p[x]

        n = len(s)
        p = list(range(n))
        for a, b in pairs:
            p[find(a)] = find(b)
        d = defaultdict(list)
        for i, c in enumerate(s):
            d[find(i)].append(c)
        for i in d.keys():
            d[i].sort(reverse=True)
        return "".join(d[find(i)].pop() for i in range(n))
```

#### Java

```java
class Solution {
    private int[] p;

    public String smallestStringWithSwaps(String s, List<List<Integer>> pairs) {
        int n = s.length();
        p = new int[n];
        List<Character>[] d = new List[n];
        for (int i = 0; i < n; ++i) {
            p[i] = i;
            d[i] = new ArrayList<>();
        }
        for (var pair : pairs) {
            int a = pair.get(0), b = pair.get(1);
            p[find(a)] = find(b);
        }
        char[] cs = s.toCharArray();
        for (int i = 0; i < n; ++i) {
            d[find(i)].add(cs[i]);
        }
        for (var e : d) {
            e.sort((a, b) -> b - a);
        }
        for (int i = 0; i < n; ++i) {
            var e = d[find(i)];
            cs[i] = e.remove(e.size() - 1);
        }
        return String.valueOf(cs);
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
    string smallestStringWithSwaps(string s, vector<vector<int>>& pairs) {
        int n = s.size();
        int p[n];
        iota(p, p + n, 0);
        vector<char> d[n];
        function<int(int)> find = [&](int x) -> int {
            if (p[x] != x) {
                p[x] = find(p[x]);
            }
            return p[x];
        };
        for (auto e : pairs) {
            int a = e[0], b = e[1];
            p[find(a)] = find(b);
        }
        for (int i = 0; i < n; ++i) {
            d[find(i)].push_back(s[i]);
        }
        for (auto& e : d) {
            sort(e.rbegin(), e.rend());
        }
        for (int i = 0; i < n; ++i) {
            auto& e = d[find(i)];
            s[i] = e.back();
            e.pop_back();
        }
        return s;
    }
};
```

#### Go

```go
func smallestStringWithSwaps(s string, pairs [][]int) string {
	n := len(s)
	p := make([]int, n)
	d := make([][]byte, n)
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
	for _, pair := range pairs {
		a, b := pair[0], pair[1]
		p[find(a)] = find(b)
	}
	cs := []byte(s)
	for i, c := range cs {
		j := find(i)
		d[j] = append(d[j], c)
	}
	for i := range d {
		sort.Slice(d[i], func(a, b int) bool { return d[i][a] > d[i][b] })
	}
	for i := range cs {
		j := find(i)
		cs[i] = d[j][len(d[j])-1]
		d[j] = d[j][:len(d[j])-1]
	}
	return string(cs)
}
```

#### TypeScript

```ts
function smallestStringWithSwaps(s: string, pairs: number[][]): string {
    const n = s.length;
    const p = new Array(n).fill(0).map((_, i) => i);
    const find = (x: number): number => {
        if (p[x] !== x) {
            p[x] = find(p[x]);
        }
        return p[x];
    };
    const d: string[][] = new Array(n).fill(0).map(() => []);
    for (const [a, b] of pairs) {
        p[find(a)] = find(b);
    }
    for (let i = 0; i < n; ++i) {
        d[find(i)].push(s[i]);
    }
    for (const e of d) {
        e.sort((a, b) => b.charCodeAt(0) - a.charCodeAt(0));
    }
    const ans: string[] = [];
    for (let i = 0; i < n; ++i) {
        ans.push(d[find(i)].pop()!);
    }
    return ans.join('');
}
```

#### Rust

```rust
impl Solution {
    #[allow(dead_code)]
    pub fn smallest_string_with_swaps(s: String, pairs: Vec<Vec<i32>>) -> String {
        let n = s.as_bytes().len();
        let s = s.as_bytes();
        let mut disjoint_set: Vec<usize> = vec![0; n];
        let mut str_vec: Vec<Vec<u8>> = vec![Vec::new(); n];
        let mut ret_str = String::new();

        // Initialize the disjoint set
        for i in 0..n {
            disjoint_set[i] = i;
        }

        // Union the pairs
        for pair in pairs {
            Self::union(pair[0] as usize, pair[1] as usize, &mut disjoint_set);
        }

        // Initialize the return vector
        for (i, c) in s.iter().enumerate() {
            let p_c = Self::find(i, &mut disjoint_set);
            str_vec[p_c].push(*c);
        }

        // Sort the return vector in reverse order
        for cur_vec in &mut str_vec {
            cur_vec.sort();
            cur_vec.reverse();
        }

        // Construct the return string
        for i in 0..n {
            let index = Self::find(i, &mut disjoint_set);
            ret_str.push(str_vec[index].last().unwrap().clone() as char);
            str_vec[index].pop();
        }

        ret_str
    }

    #[allow(dead_code)]
    fn find(x: usize, d_set: &mut Vec<usize>) -> usize {
        if d_set[x] != x {
            d_set[x] = Self::find(d_set[x], d_set);
        }
        d_set[x]
    }

    #[allow(dead_code)]
    fn union(x: usize, y: usize, d_set: &mut Vec<usize>) {
        let p_x = Self::find(x, d_set);
        let p_y = Self::find(y, d_set);
        d_set[p_x] = p_y;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
