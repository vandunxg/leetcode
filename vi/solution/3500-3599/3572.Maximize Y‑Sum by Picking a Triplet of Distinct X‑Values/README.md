---
comments: true
difficulty: Medium
rating: 1319
source: Biweekly Contest 158 Q1
tags:
    - Greedy
    - Array
    - Hash Table
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3572. Maximize Y‑Sum by Picking a Triplet of Distinct X‑Values](https://leetcode.com/problems/maximize-ysum-by-picking-a-triplet-of-distinct-xvalues)

[中文文档](/solution/3500-3599/3572.Maximize%20Y%E2%80%91Sum%20by%20Picking%20a%20Triplet%20of%20Distinct%20X%E2%80%91Values/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>x</code> và <code>y</code>, mỗi mảng có độ dài <code>n</code>. Bạn phải chọn ba chỉ số <strong>khác nhau</strong> <code>i</code>, <code>j</code> và <code>k</code> sao cho:</p>

<ul>
	<li><code>x[i] != x[j]</code></li>
	<li><code>x[j] != x[k]</code></li>
	<li><code>x[k] != x[i]</code></li>
</ul>

<p>Mục tiêu của bạn là <strong>tối đa hóa</strong> giá trị <code>y[i] + y[j] + y[k]</code> với các điều kiện trên. Hãy trả về tổng <strong>lớn nhất</strong> có thể đạt được khi chọn một bộ ba chỉ số như vậy.</p>

<p>Nếu không tồn tại bộ ba như vậy, trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">x = [1,2,1,3,2], y = [5,3,4,6,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">14</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn <code>i = 0</code> (<code>x[i] = 1</code>, <code>y[i] = 5</code>), <code>j = 1</code> (<code>x[j] = 2</code>, <code>y[j] = 3</code>), <code>k = 3</code> (<code>x[k] = 3</code>, <code>y[k] = 6</code>).</li>
	<li>Cả ba giá trị được chọn từ <code>x</code> đều khác nhau. <code>5 + 3 + 6 = 14</code> là giá trị lớn nhất có thể đạt được. Do đó, đầu ra là 14.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">x = [1,2,1,2], y = [4,5,6,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chỉ có hai giá trị khác nhau trong <code>x</code>. Do đó, đầu ra là -1.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == x.length == y.length</code></li>
	<li><code>3 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= x[i], y[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Tham lam + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Ba giá trị $x$ phải khác nhau và mục tiêu là tổng các giá trị $y$ tương ứng, vì vậy mỗi $x$ nên đóng góp giá trị $y$ lớn nhất của nó. Sắp xếp các cặp theo $y$ giảm dần, lưu các $x$ đã dùng vào một set, rồi cộng ba giá trị $x$ mới đầu tiên.
>
> Nếu có ít hơn ba giá trị $x$ khác nhau, trả về $-1$. Chỉ cần một lần sắp xếp và một lần duyệt.

<!-- thinking:end -->

Ta ghép các phần tử của hai mảng $x$ và $y$ thành một mảng hai chiều $\textit{arr}$, sau đó sắp xếp $\textit{arr}$ theo giá trị $y$ giảm dần. Tiếp theo, ta dùng một hash table để lưu các giá trị $x$ đã được chọn, rồi duyệt qua $\textit{arr}$ và mỗi lần chọn một giá trị $x$ cùng giá trị $y$ tương ứng chưa được chọn, cho đến khi chọn đủ ba giá trị $x$ khác nhau.

Nếu chọn được ba giá trị $x$ khác nhau trong quá trình duyệt, ta trả về tổng của ba giá trị $y$ tương ứng; nếu duyệt hết mà vẫn chưa chọn đủ ba giá trị $x$ khác nhau, ta trả về -1.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của hai mảng $\textit{x}$ và $\textit{y}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSumDistinctTriplet(self, x: List[int], y: List[int]) -> int:
        arr = [(a, b) for a, b in zip(x, y)]
        arr.sort(key=lambda x: -x[1])
        vis = set()
        ans = 0
        for a, b in arr:
            if a in vis:
                continue
            vis.add(a)
            ans += b
            if len(vis) == 3:
                return ans
        return -1
```

#### Java

```java
class Solution {
    public int maxSumDistinctTriplet(int[] x, int[] y) {
        int n = x.length;
        int[][] arr = new int[n][0];
        for (int i = 0; i < n; i++) {
            arr[i] = new int[] {x[i], y[i]};
        }
        Arrays.sort(arr, (a, b) -> b[1] - a[1]);
        int ans = 0;
        Set<Integer> vis = new HashSet<>();
        for (int i = 0; i < n; ++i) {
            int a = arr[i][0], b = arr[i][1];
            if (vis.add(a)) {
                ans += b;
                if (vis.size() == 3) {
                    return ans;
                }
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxSumDistinctTriplet(vector<int>& x, vector<int>& y) {
        int n = x.size();
        vector<array<int, 2>> arr(n);
        for (int i = 0; i < n; ++i) {
            arr[i] = {x[i], y[i]};
        }
        ranges::sort(arr, [](auto& a, auto& b) {
            return b[1] < a[1];
        });
        int ans = 0;
        unordered_set<int> vis;
        for (int i = 0; i < n; ++i) {
            int a = arr[i][0], b = arr[i][1];
            if (vis.insert(a).second) {
                ans += b;
                if (vis.size() == 3) {
                    return ans;
                }
            }
        }
        return -1;
    }
};
```

#### Go

```go
func maxSumDistinctTriplet(x []int, y []int) int {
	n := len(x)
	arr := make([][2]int, n)
	for i := 0; i < n; i++ {
		arr[i] = [2]int{x[i], y[i]}
	}
	sort.Slice(arr, func(i, j int) bool {
		return arr[i][1] > arr[j][1]
	})
	ans := 0
	vis := make(map[int]bool)
	for i := 0; i < n; i++ {
		a, b := arr[i][0], arr[i][1]
		if !vis[a] {
			vis[a] = true
			ans += b
			if len(vis) == 3 {
				return ans
			}
		}
	}
	return -1
}
```

#### TypeScript

```ts
function maxSumDistinctTriplet(x: number[], y: number[]): number {
    const n = x.length;
    const arr: [number, number][] = [];
    for (let i = 0; i < n; i++) {
        arr.push([x[i], y[i]]);
    }
    arr.sort((a, b) => b[1] - a[1]);
    const vis = new Set<number>();
    let ans = 0;
    for (let i = 0; i < n; i++) {
        const [a, b] = arr[i];
        if (!vis.has(a)) {
            vis.add(a);
            ans += b;
            if (vis.size === 3) {
                return ans;
            }
        }
    }
    return -1;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_sum_distinct_triplet(x: Vec<i32>, y: Vec<i32>) -> i32 {
        let n = x.len();
        let mut arr: Vec<(i32, i32)> = (0..n).map(|i| (x[i], y[i])).collect();
        arr.sort_by(|a, b| b.1.cmp(&a.1));
        let mut vis = std::collections::HashSet::new();
        let mut ans = 0;
        for (a, b) in arr {
            if vis.insert(a) {
                ans += b;
                if vis.len() == 3 {
                    return ans;
                }
            }
        }
        -1
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
