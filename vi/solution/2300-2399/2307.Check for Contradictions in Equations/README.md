---
comments: true
difficulty: Hard
tags:
    - Depth-First Search
    - Union Find
    - Graph
    - Array
    - String
---

<!-- problem:start -->

# [2307. Check for Contradictions in Equations 🔒](https://leetcode.com/problems/check-for-contradictions-in-equations)

[中文文档](/solution/2300-2399/2307.Check%20for%20Contradictions%20in%20Equations/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng 2D gồm các chuỗi <code>equations</code> và một mảng số thực <code>values</code>, trong đó <code>equations[i] = [A<sub>i</sub>, B<sub>i</sub>]</code> và <code>values[i]</code> biểu thị rằng <code>A<sub>i</sub> / B<sub>i</sub> = values[i]</code>.</p>

<p>Hãy xác định xem các phương trình có mâu thuẫn hay không. Trả về <code>true</code><em> nếu có mâu thuẫn, hoặc </em><code>false</code><em> nếu không có mâu thuẫn</em>.</p>

<p><strong>Ghi chú</strong>:</p>

<ul>
	<li>Khi kiểm tra xem hai số có bằng nhau hay không, hãy kiểm tra xem <strong>độ lệch tuyệt đối</strong> của chúng có nhỏ hơn <code>10<sup>-5</sup></code> hay không.</li>
	<li>Các test case được tạo sao cho không có trường hợp nào tập trung vào độ chính xác, tức là dùng <code>double</code> là đủ để giải bài toán.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> equations = [[&quot;a&quot;,&quot;b&quot;],[&quot;b&quot;,&quot;c&quot;],[&quot;a&quot;,&quot;c&quot;]], values = [3,0.5,1.5]
<strong>Đầu ra:</strong> false
<strong>Giải thích:
</strong>Các phương trình đã cho là: a / b = 3, b / c = 0.5, a / c = 1.5
Các phương trình không có mâu thuẫn. Một cách gán thỏa mãn tất cả các phương trình là:
a = 3, b = 1 và c = 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> equations = [[&quot;le&quot;,&quot;et&quot;],[&quot;le&quot;,&quot;code&quot;],[&quot;code&quot;,&quot;et&quot;]], values = [2,5,0.5]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong>
Các phương trình đã cho là: le / et = 2, le / code = 5, code / et = 0.5
Dựa trên hai phương trình đầu tiên, ta suy ra code / et = 0.4.
Vì phương trình thứ ba là code / et = 0.5 nên xảy ra mâu thuẫn.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= equations.length &lt;= 100</code></li>
	<li><code>equations[i].length == 2</code></li>
	<li><code>1 &lt;= A<sub>i</sub>.length, B<sub>i</sub>.length &lt;= 5</code></li>
	<li><code>A<sub>i</sub></code>, <code>B<sub>i</sub></code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>equations.length == values.length</code></li>
	<li><code>0.0 &lt; values[i] &lt;= 10.0</code></li>
	<li><code>values[i]</code> có tối đa 2 chữ số sau dấu thập phân.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Union-Find có trọng số

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi phương trình cho biết tỉ số của hai biến. Các đường đi khác nhau giữa cùng một cặp biến có thể cho kết quả không giống nhau. Số lượng biến chỉ vài trăm và có $100$ phương trình, vì vậy ta có thể dùng cấu trúc union-find để quản lý các thành phần.
>
> Ánh xạ tên biến sang số nguyên và đặt $w[x]$ là tỉ số từ $x$ đến root của nó. Khi union, cập nhật trọng số bằng phép nhân. Nếu hai biến đã có cùng root, so sánh $v \cdot w[a]$ với $w[b]$ trong sai số cho phép. Nếu hai giá trị khác nhau thì đó là mâu thuẫn.

<!-- thinking:end -->

Trước tiên, ta chuyển các chuỗi thành các số nguyên bắt đầu từ $0$. Sau đó, ta duyệt qua tất cả các phương trình, ánh xạ hai chuỗi trong mỗi phương trình thành hai số nguyên tương ứng $a$ và $b$. Nếu hai số nguyên này không thuộc cùng một tập, ta gộp chúng vào cùng một tập và ghi lại trọng số của hai số nguyên, tức là tỉ số của $a$ so với $b$. Nếu hai số nguyên đã thuộc cùng một tập, ta kiểm tra xem trọng số của chúng có thỏa mãn phương trình hay không. Nếu không, ta trả về `true`.

Độ phức tạp thời gian là $O(n \times \log n)$ hoặc $O(n \times \alpha(n))$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là số lượng phương trình.

Bài tương tự:

- [399. Evaluate Division](https://github.com/doocs/leetcode/blob/main/solution/0300-0399/0399.Evaluate%20Division/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkContradictions(
        self, equations: List[List[str]], values: List[float]
    ) -> bool:
        def find(x: int) -> int:
            if p[x] != x:
                root = find(p[x])
                w[x] *= w[p[x]]
                p[x] = root
            return p[x]

        d = defaultdict(int)
        n = 0
        for e in equations:
            for s in e:
                if s not in d:
                    d[s] = n
                    n += 1
        p = list(range(n))
        w = [1.0] * n
        eps = 1e-5
        for (a, b), v in zip(equations, values):
            a, b = d[a], d[b]
            pa, pb = find(a), find(b)
            if pa != pb:
                p[pb] = pa
                w[pb] = v * w[a] / w[b]
            elif abs(v * w[a] - w[b]) >= eps:
                return True
        return False
```

#### Java

```java
class Solution {
    private int[] p;
    private double[] w;

    public boolean checkContradictions(List<List<String>> equations, double[] values) {
        Map<String, Integer> d = new HashMap<>();
        int n = 0;
        for (var e : equations) {
            for (var s : e) {
                if (!d.containsKey(s)) {
                    d.put(s, n++);
                }
            }
        }
        p = new int[n];
        w = new double[n];
        for (int i = 0; i < n; ++i) {
            p[i] = i;
            w[i] = 1.0;
        }
        final double eps = 1e-5;
        for (int i = 0; i < equations.size(); ++i) {
            int a = d.get(equations.get(i).get(0)), b = d.get(equations.get(i).get(1));
            int pa = find(a), pb = find(b);
            double v = values[i];
            if (pa != pb) {
                p[pb] = pa;
                w[pb] = v * w[a] / w[b];
            } else if (Math.abs(v * w[a] - w[b]) >= eps) {
                return true;
            }
        }
        return false;
    }

    private int find(int x) {
        if (p[x] != x) {
            int root = find(p[x]);
            w[x] *= w[p[x]];
            p[x] = root;
        }
        return p[x];
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool checkContradictions(vector<vector<string>>& equations, vector<double>& values) {
        unordered_map<string, int> d;
        int n = 0;
        for (auto& e : equations) {
            for (auto& s : e) {
                if (!d.count(s)) {
                    d[s] = n++;
                }
            }
        }
        vector<int> p(n);
        iota(p.begin(), p.end(), 0);
        vector<double> w(n, 1.0);
        function<int(int)> find = [&](int x) -> int {
            if (p[x] != x) {
                int root = find(p[x]);
                w[x] *= w[p[x]];
                p[x] = root;
            }
            return p[x];
        };
        for (int i = 0; i < equations.size(); ++i) {
            int a = d[equations[i][0]], b = d[equations[i][1]];
            double v = values[i];
            int pa = find(a), pb = find(b);
            if (pa != pb) {
                p[pb] = pa;
                w[pb] = v * w[a] / w[b];
            } else if (fabs(v * w[a] - w[b]) >= 1e-5) {
                return true;
            }
        }
        return false;
    }
};
```

#### Go

```go
func checkContradictions(equations [][]string, values []float64) bool {
	d := make(map[string]int)
	n := 0

	for _, e := range equations {
		for _, s := range e {
			if _, ok := d[s]; !ok {
				d[s] = n
				n++
			}
		}
	}

	p := make([]int, n)
	for i := range p {
		p[i] = i
	}

	w := make([]float64, n)
	for i := range w {
		w[i] = 1.0
	}

	var find func(int) int
	find = func(x int) int {
		if p[x] != x {
			root := find(p[x])
			w[x] *= w[p[x]]
			p[x] = root
		}
		return p[x]
	}
	for i, e := range equations {
		a, b := d[e[0]], d[e[1]]
		v := values[i]

		pa, pb := find(a), find(b)
		if pa != pb {
			p[pb] = pa
			w[pb] = v * w[a] / w[b]
		} else if v*w[a]-w[b] >= 1e-5 || w[b]-v*w[a] >= 1e-5 {
			return true
		}
	}

	return false
}
```

#### TypeScript

```ts
function checkContradictions(equations: string[][], values: number[]): boolean {
    const d: { [key: string]: number } = {};
    let n = 0;

    for (const e of equations) {
        for (const s of e) {
            if (!(s in d)) {
                d[s] = n;
                n++;
            }
        }
    }

    const p: number[] = Array.from({ length: n }, (_, i) => i);
    const w: number[] = Array.from({ length: n }, () => 1.0);

    const find = (x: number): number => {
        if (p[x] !== x) {
            const root = find(p[x]);
            w[x] *= w[p[x]];
            p[x] = root;
        }
        return p[x];
    };

    for (let i = 0; i < equations.length; i++) {
        const a = d[equations[i][0]];
        const b = d[equations[i][1]];
        const v = values[i];

        const pa = find(a);
        const pb = find(b);

        if (pa !== pb) {
            p[pb] = pa;
            w[pb] = (v * w[a]) / w[b];
        } else if (Math.abs(v * w[a] - w[b]) >= 1e-5) {
            return true;
        }
    }

    return false;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
