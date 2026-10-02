---
comments: true
difficulty: Medium
rating: 1348
source: Weekly Contest 133 Q1
tags:
    - Greedy
    - Array
    - Hungarian Algorithm
    - Sorting
    - SSP
---

<!-- problem:start -->

# [1029. Two City Scheduling](https://leetcode.com/problems/two-city-scheduling)

[中文文档](/solution/1000-1099/1029.Two%20City%20Scheduling/README.md)

## Mô tả

<!-- description:start -->

<p>Một công ty dự định phỏng vấn <code>2n</code> người. Cho mảng <code>costs</code>, trong đó <code>costs[i] = [aCost<sub>i</sub>, bCost<sub>i</sub>]</code>; chi phí để đưa người thứ <code>i<sup>th</sup></code> đến thành phố <code>a</code> là <code>aCost<sub>i</sub></code>, còn chi phí để đưa người đó đến thành phố <code>b</code> là <code>bCost<sub>i</sub></code>.</p>

<p>Hãy trả về <em>chi phí nhỏ nhất để đưa tất cả mọi người đến một thành phố</em>, sao cho mỗi thành phố có đúng <code>n</code> người.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> costs = [[10,20],[30,200],[400,50],[30,20]]
<strong>Output:</strong> 110
<strong>Giải thích: </strong>
Người thứ nhất đến thành phố A với chi phí 10.
Người thứ hai đến thành phố A với chi phí 30.
Người thứ ba đến thành phố B với chi phí 50.
Người thứ tư đến thành phố B với chi phí 20.

Tổng chi phí nhỏ nhất là 10 + 30 + 50 + 20 = 110 để mỗi thành phố có một nửa số người được phỏng vấn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> costs = [[259,770],[448,54],[926,667],[184,139],[840,118],[577,469]]
<strong>Output:</strong> 1859
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> costs = [[515,563],[451,713],[537,709],[343,819],[855,779],[457,60],[650,359],[631,42]]
<strong>Output:</strong> 3086
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 * n == costs.length</code></li>
	<li><code>2 &lt;= costs.length &lt;= 100</code></li>
	<li><code>costs.length</code> là số chẵn.</li>
	<li><code>1 &lt;= aCost<sub>i</sub>, bCost<sub>i</sub> &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Nếu chọn trực tiếp $n$ trong số $2n$ người bay đến $A$ bằng cách xét các tập con thì số trường hợp tăng theo hàm mũ. Thay vào đó, hãy đưa tất cả đến $B$, rồi chuyển $n$ người sang $A$; mức thay đổi chi phí của mỗi người là $aCost-bCost$.
>
> Những người có hiệu số nhỏ nhất (âm nhất) sẽ tiết kiệm nhiều chi phí nhất khi được chuyển sang $A$. Sắp xếp theo $aCost-bCost$, đưa nửa đầu đến $A$ và những người còn lại đến $B$ sẽ cho phương án tối ưu.
>
> Đáp án là tổng chi phí tương ứng của hai nửa.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def twoCitySchedCost(self, costs: List[List[int]]) -> int:
        costs.sort(key=lambda x: x[0] - x[1])
        n = len(costs) >> 1
        return sum(costs[i][0] + costs[i + n][1] for i in range(n))
```

#### Java

```java
class Solution {
    public int twoCitySchedCost(int[][] costs) {
        Arrays.sort(costs, (a, b) -> { return a[0] - a[1] - (b[0] - b[1]); });
        int ans = 0;
        int n = costs.length >> 1;
        for (int i = 0; i < n; ++i) {
            ans += costs[i][0] + costs[i + n][1];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int twoCitySchedCost(vector<vector<int>>& costs) {
        sort(costs.begin(), costs.end(), [](const vector<int>& a, const vector<int>& b) {
            return a[0] - a[1] < b[0] - b[1];
        });
        int n = costs.size() / 2;
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            ans += costs[i][0] + costs[i + n][1];
        }
        return ans;
    }
};
```

#### Go

```go
func twoCitySchedCost(costs [][]int) (ans int) {
	sort.Slice(costs, func(i, j int) bool {
		return costs[i][0]-costs[i][1] < costs[j][0]-costs[j][1]
	})
	n := len(costs) >> 1
	for i, a := range costs[:n] {
		ans += a[0] + costs[i+n][1]
	}
	return
}
```

#### TypeScript

```ts
function twoCitySchedCost(costs: number[][]): number {
    costs.sort((a, b) => a[0] - a[1] - (b[0] - b[1]));
    const n = costs.length >> 1;
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        ans += costs[i][0] + costs[i + n][1];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
