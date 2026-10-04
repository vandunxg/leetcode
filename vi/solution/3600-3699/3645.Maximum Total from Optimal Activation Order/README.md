---
comments: true
difficulty: Medium
rating: 2018
source: Weekly Contest 462 Q3
tags:
    - Greedy
    - Array
    - Two Pointers
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3645. Maximum Total from Optimal Activation Order](https://leetcode.com/problems/maximum-total-from-optimal-activation-order)

[中文文档](/solution/3600-3699/3645.Maximum%20Total%20from%20Optimal%20Activation%20Order/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>value</code> và <code>limit</code>, cả hai đều có độ dài <code>n</code>.</p>

<p>Ban đầu, tất cả phần tử đều <strong>không hoạt động</strong>. Bạn có thể kích hoạt chúng theo bất kỳ thứ tự nào.</p>

<ul>
	<li>Để kích hoạt một phần tử <strong>không hoạt động</strong> tại chỉ số <code>i</code>, số phần tử <strong>đang hoạt động</strong> phải <strong>nhỏ hơn nghiêm ngặt</strong> <code>limit[i]</code>.</li>
	<li>Khi kích hoạt phần tử tại chỉ số <code>i</code>, phần tử đó cộng <code>value[i]</code> vào <strong>tổng</strong> giá trị kích hoạt (tức là tổng <code>value[i]</code> của tất cả phần tử đã trải qua thao tác kích hoạt).</li>
	<li>Sau mỗi lần kích hoạt, nếu số phần tử <strong>đang hoạt động</strong> trở thành <code>x</code>, thì <strong>tất cả</strong> phần tử <code>j</code> với <code>limit[j] &lt;= x</code> sẽ trở thành <strong>không hoạt động vĩnh viễn</strong>, kể cả khi chúng đã được kích hoạt.</li>
</ul>

<p>Hãy trả về <strong>tổng</strong> <strong>lớn nhất</strong> có thể đạt được bằng cách chọn thứ tự kích hoạt tối ưu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">value = [3,5,8], limit = [2,1,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">16</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một thứ tự kích hoạt tối ưu là:</p>

<table>
	<thead>
		<tr>
			<th align="center" style="border: 1px solid black;">Bước</th>
			<th align="center" style="border: 1px solid black;">Đã kích hoạt <code>i</code></th>
			<th align="center" style="border: 1px solid black;"><code>value[i]</code></th>
			<th align="center" style="border: 1px solid black;">Số phần tử hoạt động trước <code>i</code></th>
			<th align="center" style="border: 1px solid black;">Số phần tử hoạt động sau <code>i</code></th>
			<th align="center" style="border: 1px solid black;">Trở nên không hoạt động <code>j</code></th>
			<th align="center" style="border: 1px solid black;">Các phần tử không hoạt động</th>
			<th align="center" style="border: 1px solid black;">Tổng</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td align="center" style="border: 1px solid black;">1</td>
			<td align="center" style="border: 1px solid black;">1</td>
			<td align="center" style="border: 1px solid black;">5</td>
			<td align="center" style="border: 1px solid black;">0</td>
			<td align="center" style="border: 1px solid black;">1</td>
			<td align="center" style="border: 1px solid black;"><code>j = 1</code> vì <code>limit[1] = 1</code></td>
			<td align="center" style="border: 1px solid black;">[1]</td>
			<td align="center" style="border: 1px solid black;">5</td>
		</tr>
		<tr>
			<td align="center" style="border: 1px solid black;">2</td>
			<td align="center" style="border: 1px solid black;">0</td>
			<td align="center" style="border: 1px solid black;">3</td>
			<td align="center" style="border: 1px solid black;">0</td>
			<td align="center" style="border: 1px solid black;">1</td>
			<td align="center" style="border: 1px solid black;">-</td>
			<td align="center" style="border: 1px solid black;">[1]</td>
			<td align="center" style="border: 1px solid black;">8</td>
		</tr>
		<tr>
			<td align="center" style="border: 1px solid black;">3</td>
			<td align="center" style="border: 1px solid black;">2</td>
			<td align="center" style="border: 1px solid black;">8</td>
			<td align="center" style="border: 1px solid black;">1</td>
			<td align="center" style="border: 1px solid black;">2</td>
			<td align="center" style="border: 1px solid black;"><code>j = 0</code> vì <code>limit[0] = 2</code></td>
			<td align="center" style="border: 1px solid black;">[0, 1]</td>
			<td align="center" style="border: 1px solid black;">16</td>
		</tr>
	</tbody>
</table>

<p>Vì vậy, tổng lớn nhất có thể đạt được là 16.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">value = [4,2,6], limit = [1,1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một thứ tự kích hoạt tối ưu là:</p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th align="center" style="border: 1px solid black;">Bước</th>
			<th align="center" style="border: 1px solid black;">Đã kích hoạt <code>i</code></th>
			<th align="center" style="border: 1px solid black;"><code>value[i]</code></th>
			<th align="center" style="border: 1px solid black;">Số phần tử hoạt động trước <code>i</code></th>
			<th align="center" style="border: 1px solid black;">Số phần tử hoạt động sau <code>i</code></th>
			<th align="center" style="border: 1px solid black;">Trở nên không hoạt động <code>j</code></th>
			<th align="center" style="border: 1px solid black;">Các phần tử không hoạt động</th>
			<th align="center" style="border: 1px solid black;">Tổng</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td align="center" style="border: 1px solid black;">1</td>
			<td align="center" style="border: 1px solid black;">2</td>
			<td align="center" style="border: 1px solid black;">6</td>
			<td align="center" style="border: 1px solid black;">0</td>
			<td align="center" style="border: 1px solid black;">1</td>
			<td align="center" style="border: 1px solid black;"><code>j = 0, 1, 2</code> vì <code>limit[j] = 1</code></td>
			<td align="center" style="border: 1px solid black;">[0, 1, 2]</td>
			<td align="center" style="border: 1px solid black;">6</td>
		</tr>
	</tbody>
</table>

<p>Vì vậy, tổng lớn nhất có thể đạt được là 6.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">value = [4,1,5,2], limit = [3,3,2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một thứ tự kích hoạt tối ưu là:</p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th align="center" style="border: 1px solid black;">Bước</th>
			<th align="center" style="border: 1px solid black;">Đã kích hoạt <code>i</code></th>
			<th align="center" style="border: 1px solid black;"><code>value[i]</code></th>
			<th align="center" style="border: 1px solid black;">Số phần tử hoạt động trước <code>i</code></th>
			<th align="center" style="border: 1px solid black;">Số phần tử hoạt động sau <code>i</code></th>
			<th align="center" style="border: 1px solid black;">Trở nên không hoạt động <code>j</code></th>
			<th align="center" style="border: 1px solid black;">Các phần tử không hoạt động</th>
			<th align="center" style="border: 1px solid black;">Tổng</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td align="center" style="border: 1px solid black;">1</td>
			<td align="center" style="border: 1px solid black;">2</td>
			<td align="center" style="border: 1px solid black;">5</td>
			<td align="center" style="border: 1px solid black;">0</td>
			<td align="center" style="border: 1px solid black;">1</td>
			<td align="center" style="border: 1px solid black;">-</td>
			<td align="center" style="border: 1px solid black;">[ ]</td>
			<td align="center" style="border: 1px solid black;">5</td>
		</tr>
		<tr>
			<td align="center" style="border: 1px solid black;">2</td>
			<td align="center" style="border: 1px solid black;">0</td>
			<td align="center" style="border: 1px solid black;">4</td>
			<td align="center" style="border: 1px solid black;">1</td>
			<td align="center" style="border: 1px solid black;">2</td>
			<td align="center" style="border: 1px solid black;"><code>j = 2</code> vì <code>limit[2] = 2</code></td>
			<td align="center" style="border: 1px solid black;">[2]</td>
			<td align="center" style="border: 1px solid black;">9</td>
		</tr>
		<tr>
			<td align="center" style="border: 1px solid black;">3</td>
			<td align="center" style="border: 1px solid black;">1</td>
			<td align="center" style="border: 1px solid black;">1</td>
			<td align="center" style="border: 1px solid black;">1</td>
			<td align="center" style="border: 1px solid black;">2</td>
			<td align="center" style="border: 1px solid black;">-</td>
			<td align="center" style="border: 1px solid black;">[2]</td>
			<td align="center" style="border: 1px solid black;">10</td>
		</tr>
		<tr>
			<td align="center" style="border: 1px solid black;">4</td>
			<td align="center" style="border: 1px solid black;">3</td>
			<td align="center" style="border: 1px solid black;">2</td>
			<td align="center" style="border: 1px solid black;">2</td>
			<td align="center" style="border: 1px solid black;">3</td>
			<td align="center" style="border: 1px solid black;"><code>j = 0, 1, 3</code> vì <code>limit[j] = 3</code></td>
			<td align="center" style="border: 1px solid black;">[0, 1, 2, 3]</td>
			<td align="center" style="border: 1px solid black;">12</td>
		</tr>
	</tbody>
</table>

<p>Vì vậy, tổng lớn nhất có thể đạt được là 12.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == value.length == limit.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= value[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= limit[i] &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Việc kích hoạt một giá trị chiếm một slot bị giới hạn bởi $\textit{limit}$, và các phần tử có cùng limit phải cạnh tranh các slot đó.
>
> Nhóm theo $\textit{limit}$. Trong mỗi nhóm, nhiều nhất $\textit{limit}$ giá trị được giữ lại, vì vậy hãy giữ lại các giá trị lớn nhất.
>
> Các nhóm độc lập: sắp xếp từng danh sách rồi tính tổng $\textit{lim}$ phần tử cuối. Các limit khác nhau không ràng buộc lẫn nhau.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxTotal(self, value: List[int], limit: List[int]) -> int:
        g = defaultdict(list)
        for v, lim in zip(value, limit):
            g[lim].append(v)
        ans = 0
        for lim, vs in g.items():
            vs.sort()
            ans += sum(vs[-lim:])
        return ans
```

#### Java

```java
class Solution {
    public long maxTotal(int[] value, int[] limit) {
        Map<Integer, List<Integer>> g = new HashMap<>();
        for (int i = 0; i < value.length; ++i) {
            g.computeIfAbsent(limit[i], k -> new ArrayList<>()).add(value[i]);
        }
        long ans = 0;
        for (var e : g.entrySet()) {
            int lim = e.getKey();
            var vs = e.getValue();
            vs.sort((a, b) -> b - a);
            for (int i = 0; i < Math.min(lim, vs.size()); ++i) {
                ans += vs.get(i);
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxTotal(vector<int>& value, vector<int>& limit) {
        unordered_map<int, vector<int>> g;
        int n = value.size();
        for (int i = 0; i < n; ++i) {
            g[limit[i]].push_back(value[i]);
        }
        long long ans = 0;
        for (auto& [lim, vs] : g) {
            sort(vs.begin(), vs.end(), greater<int>());
            for (int i = 0; i < min(lim, (int) vs.size()); ++i) {
                ans += vs[i];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxTotal(value []int, limit []int) (ans int64) {
	g := make(map[int][]int)
	for i := range value {
		g[limit[i]] = append(g[limit[i]], value[i])
	}
	for lim, vs := range g {
		slices.SortFunc(vs, func(a, b int) int { return b - a })
		for i := 0; i < min(lim, len(vs)); i++ {
			ans += int64(vs[i])
		}
	}
	return
}
```

#### TypeScript

```ts
function maxTotal(value: number[], limit: number[]): number {
    const g = new Map<number, number[]>();
    for (let i = 0; i < value.length; i++) {
        if (!g.has(limit[i])) {
            g.set(limit[i], []);
        }
        g.get(limit[i])!.push(value[i]);
    }
    let ans = 0;
    for (const [lim, vs] of g) {
        vs.sort((a, b) => b - a);
        ans += vs.slice(0, lim).reduce((acc, v) => acc + v, 0);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
