---
comments: true
difficulty: Medium
rating: 1525
source: Weekly Contest 284 Q2
tags:
    - Array
    - Hash Table
    - Simulation
---

<!-- problem:start -->

# [2201. Count Artifacts That Can Be Extracted](https://leetcode.com/problems/count-artifacts-that-can-be-extracted)

[中文文档](/solution/2200-2299/2201.Count%20Artifacts%20That%20Can%20Be%20Extracted/README.md)

## Mô tả

<!-- description:start -->

<p>Có một lưới <code>n x n</code> <strong>được đánh chỉ số từ 0</strong>, trong đó có một số hiện vật bị chôn vùi. Cho số nguyên <code>n</code> và mảng số nguyên 2 chiều <strong>được đánh chỉ số từ 0</strong> <code>artifacts</code> mô tả vị trí của các hiện vật hình chữ nhật, trong đó <code>artifacts[i] = [r1<sub>i</sub>, c1<sub>i</sub>, r2<sub>i</sub>, c2<sub>i</sub>]</code> cho biết hiện vật thứ <code>i<sup>th</sup></code> bị chôn trong vùng con, với:</p>

<ul>
	<li><code>(r1<sub>i</sub>, c1<sub>i</sub>)</code> là tọa độ của ô <strong>trên cùng bên trái</strong> của hiện vật thứ <code>i<sup>th</sup></code> và</li>
	<li><code>(r2<sub>i</sub>, c2<sub>i</sub>)</code> là tọa độ của ô <strong>dưới cùng bên phải</strong> của hiện vật thứ <code>i<sup>th</sup></code>.</li>
</ul>

<p>Bạn sẽ đào một số ô trong lưới và loại bỏ toàn bộ đất khỏi chúng. Nếu bên dưới ô có một phần của hiện vật, phần đó sẽ được phát hiện. Nếu tất cả các phần của một hiện vật đều được phát hiện, bạn có thể lấy hiện vật đó lên.</p>

<p>Cho mảng số nguyên 2 chiều <strong>được đánh chỉ số từ 0</strong> <code>dig</code>, trong đó <code>dig[i] = [r<sub>i</sub>, c<sub>i</sub>]</code> cho biết bạn sẽ đào ô <code>(r<sub>i</sub>, c<sub>i</sub>)</code>, hãy trả về <em>số hiện vật có thể lấy lên</em>.</p>

<p>Các test được tạo sao cho:</p>

<ul>
	<li>Không có hai hiện vật nào chồng lấn lên nhau.</li>
	<li>Mỗi hiện vật chỉ bao phủ nhiều nhất <code>4</code> ô.</li>
	<li>Các phần tử của <code>dig</code> là duy nhất.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2201.Count%20Artifacts%20That%20Can%20Be%20Extracted/images/untitled-diagram.jpg" style="width: 216px; height: 216px;" />
<pre>
<strong>Đầu vào:</strong> n = 2, artifacts = [[0,0,0,0],[0,1,1,1]], dig = [[0,0],[0,1]]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
Các màu khác nhau biểu thị các hiện vật khác nhau. Các ô đã đào được đánh dấu bằng &#39;D&#39; trên lưới.
Có 1 hiện vật có thể lấy lên, đó là hiện vật màu đỏ.
Hiện vật màu xanh có một phần nằm trong ô (1,1) vẫn chưa được phát hiện, nên ta không thể lấy nó lên.
Vì vậy, ta trả về 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2201.Count%20Artifacts%20That%20Can%20Be%20Extracted/images/untitled-diagram-1.jpg" style="width: 216px; height: 216px;" />
<pre>
<strong>Đầu vào:</strong> n = 2, artifacts = [[0,0,0,0],[0,1,1,1]], dig = [[0,0],[0,1],[1,1]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Cả hiện vật màu đỏ và màu xanh đều có tất cả các phần được phát hiện (được đánh dấu bằng &#39;D&#39;), nên có thể lấy lên. Vì vậy, ta trả về 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
	<li><code>1 &lt;= artifacts.length, dig.length &lt;= min(n<sup>2</sup>, 10<sup>5</sup>)</code></li>
	<li><code>artifacts[i].length == 4</code></li>
	<li><code>dig[i].length == 2</code></li>
	<li><code>0 &lt;= r1<sub>i</sub>, c1<sub>i</sub>, r2<sub>i</sub>, c2<sub>i</sub>, r<sub>i</sub>, c<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>r1<sub>i</sub> &lt;= r2<sub>i</sub></code></li>
	<li><code>c1<sub>i</sub> &lt;= c2<sub>i</sub></code></li>
	<li>Không có hai hiện vật nào chồng lấn lên nhau.</li>
	<li>Số ô mà một hiện vật bao phủ <strong>nhiều nhất</strong> là <code>4</code>.</li>
	<li>Các phần tử của <code>dig</code> là duy nhất.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Một hiện vật chỉ có thể được lấy lên khi mọi ô thuộc hình chữ nhật của nó đều đã được đào. Việc ghi mỗi ô đã đào vào một lưới $n \times n$ rồi quét từng hiện vật vẫn phù hợp với $n \le 10^3$, nhưng mỗi hiện vật chỉ chiếm nhiều nhất bốn ô, nên không cần dùng cả lưới.
>
> Đưa các ô đã đào phân biệt vào hash set $s$. Với mỗi hiện vật, duyệt $[r_1, r_2] \times [c_1, c_2]$ và đếm hiện vật đó nếu mọi ô đều nằm trong $s$. Tổng số lần kiểm tra ô có bậc bằng số hiện vật cộng số ô đã đào.

<!-- thinking:end -->

Ta có thể dùng một hash table $s$ để lưu tất cả các ô đã đào, sau đó duyệt qua toàn bộ hiện vật và kiểm tra xem tất cả các phần của hiện vật có nằm trong hash table hay không. Nếu có, ta có thể lấy hiện vật lên và tăng đáp án lên một đơn vị.

Độ phức tạp thời gian là $O(m + k)$, còn độ phức tạp không gian là $O(k)$. Trong đó, $m$ là số hiện vật và $k$ là số ô đã đào.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def digArtifacts(
        self, n: int, artifacts: List[List[int]], dig: List[List[int]]
    ) -> int:
        def check(a: List[int]) -> bool:
            x1, y1, x2, y2 = a
            return all(
                (x, y) in s for x in range(x1, x2 + 1) for y in range(y1, y2 + 1)
            )

        s = {(i, j) for i, j in dig}
        return sum(check(a) for a in artifacts)
```

#### Java

```java
class Solution {
    private Set<Integer> s = new HashSet<>();
    private int n;

    public int digArtifacts(int n, int[][] artifacts, int[][] dig) {
        this.n = n;
        for (var p : dig) {
            s.add(p[0] * n + p[1]);
        }
        int ans = 0;
        for (var a : artifacts) {
            ans += check(a);
        }
        return ans;
    }

    private int check(int[] a) {
        int x1 = a[0], y1 = a[1], x2 = a[2], y2 = a[3];
        for (int x = x1; x <= x2; ++x) {
            for (int y = y1; y <= y2; ++y) {
                if (!s.contains(x * n + y)) {
                    return 0;
                }
            }
        }
        return 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int digArtifacts(int n, vector<vector<int>>& artifacts, vector<vector<int>>& dig) {
        unordered_set<int> s;
        for (auto& p : dig) {
            s.insert(p[0] * n + p[1]);
        }
        auto check = [&](vector<int>& a) {
            int x1 = a[0], y1 = a[1], x2 = a[2], y2 = a[3];
            for (int x = x1; x <= x2; ++x) {
                for (int y = y1; y <= y2; ++y) {
                    if (!s.count(x * n + y)) {
                        return 0;
                    }
                }
            }
            return 1;
        };
        int ans = 0;
        for (auto& a : artifacts) {
            ans += check(a);
        }
        return ans;
    }
};
```

#### Go

```go
func digArtifacts(n int, artifacts [][]int, dig [][]int) (ans int) {
	s := map[int]bool{}
	for _, p := range dig {
		s[p[0]*n+p[1]] = true
	}
	check := func(a []int) int {
		x1, y1, x2, y2 := a[0], a[1], a[2], a[3]
		for x := x1; x <= x2; x++ {
			for y := y1; y <= y2; y++ {
				if !s[x*n+y] {
					return 0
				}
			}
		}
		return 1
	}
	for _, a := range artifacts {
		ans += check(a)
	}
	return
}
```

#### TypeScript

```ts
function digArtifacts(n: number, artifacts: number[][], dig: number[][]): number {
    const s: Set<number> = new Set();
    for (const [x, y] of dig) {
        s.add(x * n + y);
    }
    let ans = 0;
    const check = (a: number[]): number => {
        const [x1, y1, x2, y2] = a;
        for (let x = x1; x <= x2; ++x) {
            for (let y = y1; y <= y2; ++y) {
                if (!s.has(x * n + y)) {
                    return 0;
                }
            }
        }
        return 1;
    };
    for (const a of artifacts) {
        ans += check(a);
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashSet;

impl Solution {
    pub fn dig_artifacts(n: i32, artifacts: Vec<Vec<i32>>, dig: Vec<Vec<i32>>) -> i32 {
        let mut s: HashSet<i32> = HashSet::new();
        for p in dig {
            s.insert(p[0] * n + p[1]);
        }
        let check = |a: &[i32]| -> i32 {
            let x1 = a[0];
            let y1 = a[1];
            let x2 = a[2];
            let y2 = a[3];
            for x in x1..=x2 {
                for y in y1..=y2 {
                    if !s.contains(&(x * n + y)) {
                        return 0;
                    }
                }
            }
            1
        };
        let mut ans = 0;
        for a in artifacts {
            ans += check(&a);
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
