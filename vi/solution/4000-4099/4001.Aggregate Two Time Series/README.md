---
comments: true
difficulty: Medium
rating: 1506
source: Weekly Contest 512 Q2
tags:
    - Array
    - Two Pointers
---

<!-- problem:start -->

# [4001. Aggregate Two Time Series](https://leetcode.com/problems/aggregate-two-time-series)

[中文文档](/solution/4000-4099/4001.Aggregate%20Two%20Time%20Series/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên 2D <code>series1</code> và <code>series2</code>.</p>

<p>Mỗi phần tử trong hai chuỗi có dạng <code>[timestamp, value]</code>, trong đó:</p>

<ul>
	<li><code>timestamp</code> là số nguyên biểu diễn thời gian.</li>
	<li><code>value</code> là số nguyên biểu diễn giá trị tại thời điểm đó.</li>
</ul>

<p>Mỗi mảng được sắp xếp theo thứ tự <span data-keyword="strictly-increasing-array">tăng nghiêm ngặt</span> của <code>timestamp</code>.</p>

<p>Với một timestamp <strong>không xuất hiện</strong> trong một chuỗi, giá trị của nó được lấy từ <strong>timestamp khả dụng kế tiếp</strong> trong cùng chuỗi nếu có. Nếu không, giá trị được xem là 0.</p>

<p><strong>Chuỗi tổng hợp</strong> được tạo bằng cách cộng các giá trị tương ứng của hai chuỗi tại mọi timestamp xuất hiện trong ít nhất một chuỗi.</p>

<p>Trả về <strong>chuỗi tổng hợp</strong> dưới dạng mảng số nguyên 2D gồm các cặp <code>[timestamp, summedValue]</code>, được sắp xếp theo thứ tự <strong>tăng nghiêm ngặt</strong> của timestamp.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">series1 = [[1,3],[4,1]], series2 = [[2,2],[5,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[1,5],[2,3],[4,3],[5,2]]</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;">Timestamp</th>
			<th style="border: 1px solid black;"><code>series1</code></th>
			<th style="border: 1px solid black;"><code>series2</code></th>
			<th style="border: 1px solid black;"><code>summedValue</code></th>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">5</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">3</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">4</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">3</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">5</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">2</td>
		</tr>
	</tbody>
</table>

<p>Do đó, chuỗi tổng hợp là <code>[[1, 5], [2, 3], [4, 3], [5, 2]]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">series1 = [[1,5],[3,1]], series2 = [[2,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[1,7],[2,3],[3,1]]</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;">Timestamp</th>
			<th style="border: 1px solid black;"><code>series1</code></th>
			<th style="border: 1px solid black;"><code>series2</code></th>
			<th style="border: 1px solid black;"><code>summedValue</code></th>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">5</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">7</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">3</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
	</tbody>
</table>

<p>Do đó, chuỗi tổng hợp là <code>[[1, 7], [2, 3], [3, 1]]</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">series1 = [[1,5]], series2 = [[1000000000,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[1,7],[1000000000,2]]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tại timestamp 1, giá trị khả dụng kế tiếp trong <code>series2</code> là 2 tại timestamp 1000000000. Tại timestamp 1000000000, <code>series1</code> không có timestamp nào lớn hơn nên giá trị của nó là 0. Chỉ các timestamp xuất hiện trong ít nhất một trong hai chuỗi mới được đưa vào kết quả.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= series1.length, series2.length &lt;= 10<sup>5</sup></code></li>
	<li><code>series1[i].length == series2[i].length == 2</code></li>
	<li><code>1 &lt;= series1[i][0], series2[i][0] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= series1[i][1], series2[i][1] &lt;= 10<sup>9</sup></code></li>
	<li>Mỗi chuỗi được sắp xếp theo thứ tự tăng nghiêm ngặt của <code>timestamp</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Cả hai chuỗi đều tăng nghiêm ngặt theo thời gian và có thể dài tới $10^5$. Việc đưa timestamp vào hash map rồi điền bù các điểm bị thiếu sẽ tạo thêm các trường hợp biên và các lần truy cập ngẫu nhiên không cần thiết.
>
> Việc căn chỉnh thực chất là merge có thứ tự. Hai con trỏ luôn đưa ra timestamp nhỏ hơn và cộng giá trị hiện tại của chuỗi còn lại; nếu hai timestamp bằng nhau thì cả hai con trỏ cùng tiến.
>
> Sau khi một chuỗi kết thúc, các điểm còn lại không còn bản cập nhật tương ứng từ chuỗi kia nên được thêm trực tiếp.

<!-- thinking:end -->

Cả hai chuỗi đều tăng nghiêm ngặt theo timestamp, nên có thể merge bằng hai con trỏ. Việc lấy giá trị của timestamp lớn hơn kế tiếp cho timestamp bị thiếu tương đương với việc dùng trực tiếp giá trị tại con trỏ hiện tại cho các timestamp bị thiếu trước đó trong chuỗi ấy.

Gọi các con trỏ $i$ và $j$ lần lượt trỏ vào hai chuỗi. Khi cả hai chưa duyệt hết:

- Nếu $t_1 = t_2$, đưa ra $[t_1, v_1 + v_2]$ rồi tăng cả hai con trỏ;
- Nếu $t_1 < t_2$, đưa ra $[t_1, v_1 + v_2]$ (series2 dùng $v_2$ hiện tại ở timestamp lớn hơn) rồi chỉ tăng $i$;
- Nếu $t_2 < t_1$, xử lý đối xứng.

Sau khi một chuỗi kết thúc, thêm trực tiếp các điểm còn lại của chuỗi kia (phía đối diện không còn timestamp lớn hơn nên giá trị của nó là $0$).

Độ phức tạp thời gian là $O(m + n)$ và độ phức tạp không gian là $O(m + n)$, trong đó $m$ và $n$ là độ dài của hai chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def aggregateTimeSeries(
        self, series1: list[list[int]], series2: list[list[int]]
    ) -> list[list[int]]:
        m, n = len(series1), len(series2)
        i = j = 0
        ans = []
        while i < m and j < n:
            t1, v1 = series1[i]
            t2, v2 = series2[j]
            if t1 == t2:
                ans.append([t1, v1 + v2])
                i += 1
                j += 1
            elif t1 < t2:
                ans.append([t1, v1 + v2])
                i += 1
            else:
                ans.append([t2, v1 + v2])
                j += 1
        while i < m:
            ans.append(series1[i])
            i += 1
        while j < n:
            ans.append(series2[j])
            j += 1
        return ans
```

#### Java

```java
class Solution {
    public List<List<Integer>> aggregateTimeSeries(int[][] series1, int[][] series2) {
        int m = series1.length, n = series2.length;
        int i = 0, j = 0;
        List<List<Integer>> ans = new ArrayList<>();

        while (i < m && j < n) {
            int t1 = series1[i][0], v1 = series1[i][1];
            int t2 = series2[j][0], v2 = series2[j][1];

            if (t1 == t2) {
                ans.add(List.of(t1, v1 + v2));
                i++;
                j++;
            } else if (t1 < t2) {
                ans.add(List.of(t1, v1 + v2));
                i++;
            } else {
                ans.add(List.of(t2, v1 + v2));
                j++;
            }
        }

        while (i < m) {
            ans.add(List.of(series1[i][0], series1[i][1]));
            i++;
        }

        while (j < n) {
            ans.add(List.of(series2[j][0], series2[j][1]));
            j++;
        }

        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> aggregateTimeSeries(vector<vector<int>>& series1, vector<vector<int>>& series2) {
        int m = series1.size(), n = series2.size();
        int i = 0, j = 0;
        vector<vector<int>> ans;

        while (i < m && j < n) {
            int t1 = series1[i][0], v1 = series1[i][1];
            int t2 = series2[j][0], v2 = series2[j][1];

            if (t1 == t2) {
                ans.push_back({t1, v1 + v2});
                i++;
                j++;
            } else if (t1 < t2) {
                ans.push_back({t1, v1 + v2});
                i++;
            } else {
                ans.push_back({t2, v1 + v2});
                j++;
            }
        }

        while (i < m) {
            ans.push_back(series1[i]);
            i++;
        }

        while (j < n) {
            ans.push_back(series2[j]);
            j++;
        }

        return ans;
    }
};
```

#### Go

```go
func aggregateTimeSeries(series1 [][]int, series2 [][]int) [][]int {
	m, n := len(series1), len(series2)
	i, j := 0, 0
	ans := make([][]int, 0)

	for i < m && j < n {
		t1, v1 := series1[i][0], series1[i][1]
		t2, v2 := series2[j][0], series2[j][1]

		if t1 == t2 {
			ans = append(ans, []int{t1, v1 + v2})
			i++
			j++
		} else if t1 < t2 {
			ans = append(ans, []int{t1, v1 + v2})
			i++
		} else {
			ans = append(ans, []int{t2, v1 + v2})
			j++
		}
	}

	for i < m {
		ans = append(ans, series1[i])
		i++
	}

	for j < n {
		ans = append(ans, series2[j])
		j++
	}

	return ans
}
```

#### TypeScript

```ts
function aggregateTimeSeries(series1: number[][], series2: number[][]): number[][] {
    const m = series1.length;
    const n = series2.length;
    let i = 0;
    let j = 0;
    const ans: number[][] = [];

    while (i < m && j < n) {
        const [t1, v1] = series1[i];
        const [t2, v2] = series2[j];

        if (t1 === t2) {
            ans.push([t1, v1 + v2]);
            i++;
            j++;
        } else if (t1 < t2) {
            ans.push([t1, v1 + v2]);
            i++;
        } else {
            ans.push([t2, v1 + v2]);
            j++;
        }
    }

    while (i < m) {
        ans.push(series1[i]);
        i++;
    }

    while (j < n) {
        ans.push(series2[j]);
        j++;
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
