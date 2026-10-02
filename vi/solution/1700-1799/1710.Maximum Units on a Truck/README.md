---
comments: true
difficulty: Easy
rating: 1309
source: Weekly Contest 222 Q1
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [1710. Maximum Units on a Truck](https://leetcode.com/problems/maximum-units-on-a-truck)

[中文文档](/solution/1700-1799/1710.Maximum%20Units%20on%20a%20Truck/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được giao nhiệm vụ xếp một số thùng hàng lên <strong>một xe tải</strong>. Cho mảng 2 chiều <code>boxTypes</code>, trong đó <code>boxTypes[i] = [numberOfBoxes<sub>i</sub>, numberOfUnitsPerBox<sub>i</sub>]</code>:</p>

<ul>
	<li><code>numberOfBoxes<sub>i</sub></code> là số thùng thuộc loại <code>i</code>.</li>
	<li><code>numberOfUnitsPerBox<sub>i</sub></code><sub> </sub>là số đơn vị hàng trong mỗi thùng loại <code>i</code>.</li>
</ul>

<p>Bạn cũng được cho số nguyên <code>truckSize</code>, là số <strong>thùng</strong> <strong>tối đa</strong> có thể xếp lên xe. Bạn có thể chọn bất kỳ thùng nào miễn là tổng số thùng không vượt quá <code>truckSize</code>.</p>

<p>Trả về <em>tổng số <strong>đơn vị hàng</strong> <strong>lớn nhất</strong> có thể xếp lên xe.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> boxTypes = [[1,3],[2,2],[3,1]], truckSize = 4
<strong>Output:</strong> 8
<strong>Explanation:</strong> Có:
- 1 thùng loại đầu tiên chứa 3 đơn vị hàng.
- 2 thùng loại thứ hai, mỗi thùng chứa 2 đơn vị hàng.
- 3 thùng loại thứ ba, mỗi thùng chứa 1 đơn vị hàng.
Bạn có thể lấy tất cả thùng loại đầu tiên và thứ hai, cùng một thùng loại thứ ba.
Tổng số đơn vị hàng là (1 * 3) + (2 * 2) + (1 * 1) = 8.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> boxTypes = [[5,10],[2,5],[4,7],[3,9]], truckSize = 10
<strong>Output:</strong> 91
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= boxTypes.length &lt;= 1000</code></li>
	<li><code>1 &lt;= numberOfBoxes<sub>i</sub>, numberOfUnitsPerBox<sub>i</sub> &lt;= 1000</code></li>
	<li><code>1 &lt;= truckSize &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Sorting

<!-- thinking:start -->

> **Tư duy**
>
> Sức chứa được tính theo số thùng, còn mục tiêu là số đơn vị hàng. Các thùng cùng loại có số đơn vị như nhau, nên cần lấy các loại có nhiều đơn vị mỗi thùng trước.
>
> Sắp xếp theo số đơn vị mỗi thùng giảm dần, rồi xếp $\min(\textit{truckSize},\textit{count})$ thùng của từng loại và giảm sức chứa còn lại cho đến khi hết.

<!-- thinking:end -->

Theo đề bài, ta cần chọn được nhiều đơn vị hàng nhất có thể. Vì vậy, trước hết ta sắp xếp `boxTypes` theo số đơn vị giảm dần.

Sau đó, ta duyệt `boxTypes` từ đầu đến cuối, chọn tối đa `truckSize` thùng và cộng dồn số đơn vị hàng.

Độ phức tạp thời gian là $O(n \times \log n)$, trong đó $n$ là độ dài của mảng hai chiều `boxTypes`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumUnits(self, boxTypes: List[List[int]], truckSize: int) -> int:
        ans = 0
        for a, b in sorted(boxTypes, key=lambda x: -x[1]):
            ans += b * min(truckSize, a)
            truckSize -= a
            if truckSize <= 0:
                break
        return ans
```

#### Java

```java
class Solution {
    public int maximumUnits(int[][] boxTypes, int truckSize) {
        Arrays.sort(boxTypes, (a, b) -> b[1] - a[1]);
        int ans = 0;
        for (var e : boxTypes) {
            int a = e[0], b = e[1];
            ans += b * Math.min(truckSize, a);
            truckSize -= a;
            if (truckSize <= 0) {
                break;
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
    int maximumUnits(vector<vector<int>>& boxTypes, int truckSize) {
        sort(boxTypes.begin(), boxTypes.end(), [](auto& a, auto& b) { return a[1] > b[1]; });
        int ans = 0;
        for (auto& e : boxTypes) {
            int a = e[0], b = e[1];
            ans += b * min(truckSize, a);
            truckSize -= a;
            if (truckSize <= 0) break;
        }
        return ans;
    }
};
```

#### Go

```go
func maximumUnits(boxTypes [][]int, truckSize int) (ans int) {
	sort.Slice(boxTypes, func(i, j int) bool { return boxTypes[i][1] > boxTypes[j][1] })
	for _, e := range boxTypes {
		a, b := e[0], e[1]
		ans += b * min(truckSize, a)
		truckSize -= a
		if truckSize <= 0 {
			break
		}
	}
	return
}
```

#### TypeScript

```ts
export function maximumUnits(boxTypes: number[][], truckSize: number): number {
    boxTypes.sort(([_, a], [__, b]) => b - a);
    let ans = 0;
    for (const [count, size] of boxTypes) {
        ans += Math.min(truckSize, count) * size;
        truckSize -= count;
        if (truckSize < 0) break;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn maximum_units(mut box_types: Vec<Vec<i32>>, truck_size: i32) -> i32 {
        box_types.sort_by(|a, b| b[1].cmp(&a[1]));
        let mut sum = 0;
        let mut ans = 0;
        for box_type in box_types.iter() {
            if sum + box_type[0] < truck_size {
                sum += box_type[0];
                ans += box_type[0] * box_type[1];
            } else {
                ans += (truck_size - sum) * box_type[1];
                break;
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Counting Sort

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 tốn $O(n\log n)$ cho việc sắp xếp. Số đơn vị mỗi thùng không vượt quá $1000$, nên ta có thể dùng mảng đếm để lưu số thùng theo giá trị này.
>
> Duyệt từ $1000$ xuống $1$ và xếp hàng theo cùng thứ tự greedy, nay với thời gian tuyến tính.

<!-- thinking:end -->

Ta cũng có thể dùng counting sort: tạo mảng $cnt$ có độ dài $1001$, trong đó $cnt[b]$ là số thùng có $b$ đơn vị hàng.

Sau đó, bắt đầu từ loại thùng có nhiều đơn vị hàng nhất, chọn tối đa `truckSize` thùng và cộng dồn số đơn vị hàng.

Độ phức tạp thời gian là $O(M)$, trong đó $M$ là số đơn vị hàng lớn nhất. Với bài này, $M=1000$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumUnits(self, boxTypes: List[List[int]], truckSize: int) -> int:
        cnt = [0] * 1001
        for a, b in boxTypes:
            cnt[b] += a
        ans = 0
        for b in range(1000, 0, -1):
            a = cnt[b]
            if a:
                ans += b * min(truckSize, a)
                truckSize -= a
                if truckSize <= 0:
                    break
        return ans
```

#### Java

```java
class Solution {
    public int maximumUnits(int[][] boxTypes, int truckSize) {
        int[] cnt = new int[1001];
        for (var e : boxTypes) {
            int a = e[0], b = e[1];
            cnt[b] += a;
        }
        int ans = 0;
        for (int b = 1000; b > 0 && truckSize > 0; --b) {
            int a = cnt[b];
            if (a > 0) {
                ans += b * Math.min(truckSize, a);
                truckSize -= a;
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
    int maximumUnits(vector<vector<int>>& boxTypes, int truckSize) {
        int cnt[1001] = {0};
        for (auto& e : boxTypes) {
            int a = e[0], b = e[1];
            cnt[b] += a;
        }
        int ans = 0;
        for (int b = 1000; b > 0 && truckSize > 0; --b) {
            int a = cnt[b];
            if (a) {
                ans += b * min(truckSize, a);
                truckSize -= a;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maximumUnits(boxTypes [][]int, truckSize int) (ans int) {
	cnt := [1001]int{}
	for _, e := range boxTypes {
		a, b := e[0], e[1]
		cnt[b] += a
	}
	for b := 1000; b > 0 && truckSize > 0; b-- {
		a := cnt[b]
		if a > 0 {
			ans += b * min(truckSize, a)
			truckSize -= a
		}
	}
	return
}
```

#### TypeScript

```ts
function maximumUnits(boxTypes: number[][], truckSize: number): number {
    const cnt = new Array(1001).fill(0);
    for (const [a, b] of boxTypes) {
        cnt[b] += a;
    }
    let ans = 0;
    for (let b = 1000; b > 0 && truckSize > 0; --b) {
        const a = cnt[b];
        if (a > 0) {
            ans += b * Math.min(truckSize, a);
            truckSize -= a;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
