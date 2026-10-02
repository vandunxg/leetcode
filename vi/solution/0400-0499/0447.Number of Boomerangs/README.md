---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - Math
---

<!-- problem:start -->

# [447. Number of Boomerangs](https://leetcode.com/problems/number-of-boomerangs)

[中文文档](/solution/0400-0499/0447.Number%20of%20Boomerangs/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>n</code> điểm <code>points</code> trên mặt phẳng, đôi một <strong>khác nhau</strong>, trong đó <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code>. Một <strong>boomerang</strong> là bộ ba điểm <code>(i, j, k)</code> sao cho khoảng cách giữa <code>i</code> và <code>j</code> bằng khoảng cách giữa <code>i</code> và <code>k</code> <strong>(thứ tự trong bộ ba có ý nghĩa)</strong>.</p>

<p>Trả về <em>số lượng boomerang</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> points = [[0,0],[1,0],[2,0]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Hai boomerang là [[1,0],[0,0],[2,0]] và [[1,0],[2,0],[0,0]].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> points = [[1,1],[2,2],[3,3]]
<strong>Đầu ra:</strong> 2
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> points = [[1,1]]
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == points.length</code></li>
	<li><code>1 &lt;= n &lt;= 500</code></li>
	<li><code>points[i].length == 2</code></li>
	<li><code>-10<sup>4</sup> &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= 10<sup>4</sup></code></li>
	<li>Tất cả các điểm đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê + Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Boomerang là bộ ba có thứ tự; liệt kê cả ba điểm sẽ tốn $O(n^3)$. Khi cố định $i$, các điểm cách $i$ cùng một khoảng cách có thể ghép thành $(j,k)$ theo bất kỳ thứ tự nào.
>
> Với mỗi tâm $p_1$, đếm khoảng cách đến các điểm còn lại. Khi gặp một khoảng cách đã được ghi nhận $x$ lần, ta tạo thêm $x$ cặp có thứ tự; nhân đôi để tính cả hai hướng.
>
> Cộng dồn ngay khi thêm phần tử để không phải duyệt lại hash map.

<!-- thinking:end -->

Ta lần lượt chọn mỗi điểm trong `points` làm điểm $i$ của boomerang, rồi dùng hash table $cnt$ để ghi nhận số lần mỗi khoảng cách từ các điểm khác đến $i$ xuất hiện.

Nếu có $x$ điểm cách đều $i$, ta có thể chọn tùy ý hai điểm trong số đó làm $j$ và $k$ của boomerang. Số cách chọn có thứ tự là $A_x^2 = x \times (x - 1)$. Vì vậy, với mỗi giá trị $x$ trong hash table, ta tính và cộng dồn $A_x^2$ để có tổng số boomerang thỏa mãn yêu cầu.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng `points`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfBoomerangs(self, points: List[List[int]]) -> int:
        ans = 0
        for p1 in points:
            cnt = Counter()
            for p2 in points:
                d = dist(p1, p2)
                ans += cnt[d]
                cnt[d] += 1
        return ans << 1
```

#### Java

```java
class Solution {
    public int numberOfBoomerangs(int[][] points) {
        int ans = 0;
        for (int[] p1 : points) {
            Map<Integer, Integer> cnt = new HashMap<>();
            for (int[] p2 : points) {
                int d = (p1[0] - p2[0]) * (p1[0] - p2[0]) + (p1[1] - p2[1]) * (p1[1] - p2[1]);
                ans += cnt.getOrDefault(d, 0);
                cnt.merge(d, 1, Integer::sum);
            }
        }
        return ans << 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfBoomerangs(vector<vector<int>>& points) {
        int ans = 0;
        for (auto& p1 : points) {
            unordered_map<int, int> cnt;
            for (auto& p2 : points) {
                int d = (p1[0] - p2[0]) * (p1[0] - p2[0]) + (p1[1] - p2[1]) * (p1[1] - p2[1]);
                ans += cnt[d];
                cnt[d]++;
            }
        }
        return ans << 1;
    }
};
```

#### Go

```go
func numberOfBoomerangs(points [][]int) (ans int) {
	for _, p1 := range points {
		cnt := map[int]int{}
		for _, p2 := range points {
			d := (p1[0]-p2[0])*(p1[0]-p2[0]) + (p1[1]-p2[1])*(p1[1]-p2[1])
			ans += cnt[d]
			cnt[d]++
		}
	}
	ans <<= 1
	return
}
```

#### TypeScript

```ts
function numberOfBoomerangs(points: number[][]): number {
    let ans = 0;
    for (const [x1, y1] of points) {
        const cnt: Map<number, number> = new Map();
        for (const [x2, y2] of points) {
            const d = (x1 - x2) ** 2 + (y1 - y2) ** 2;
            ans += cnt.get(d) || 0;
            cnt.set(d, (cnt.get(d) || 0) + 1);
        }
    }
    return ans << 1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 cộng dồn ngay trong quá trình đếm. Cách này đếm xong trước rồi cộng $x(x-1)$ theo từng tần suất chính là viết trực tiếp $A_x^2$. Độ phức tạp tiệm cận không đổi, nhưng cách trình bày trực tiếp hơn một chút.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfBoomerangs(self, points: List[List[int]]) -> int:
        ans = 0
        for p1 in points:
            cnt = Counter()
            for p2 in points:
                d = dist(p1, p2)
                cnt[d] += 1
            ans += sum(x * (x - 1) for x in cnt.values())
        return ans
```

#### Java

```java
class Solution {
    public int numberOfBoomerangs(int[][] points) {
        int ans = 0;
        for (int[] p1 : points) {
            Map<Integer, Integer> cnt = new HashMap<>();
            for (int[] p2 : points) {
                int d = (p1[0] - p2[0]) * (p1[0] - p2[0]) + (p1[1] - p2[1]) * (p1[1] - p2[1]);
                cnt.merge(d, 1, Integer::sum);
            }
            for (int x : cnt.values()) {
                ans += x * (x - 1);
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
    int numberOfBoomerangs(vector<vector<int>>& points) {
        int ans = 0;
        for (auto& p1 : points) {
            unordered_map<int, int> cnt;
            for (auto& p2 : points) {
                int d = (p1[0] - p2[0]) * (p1[0] - p2[0]) + (p1[1] - p2[1]) * (p1[1] - p2[1]);
                cnt[d]++;
            }
            for (auto& [_, x] : cnt) {
                ans += x * (x - 1);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func numberOfBoomerangs(points [][]int) (ans int) {
	for _, p1 := range points {
		cnt := map[int]int{}
		for _, p2 := range points {
			d := (p1[0]-p2[0])*(p1[0]-p2[0]) + (p1[1]-p2[1])*(p1[1]-p2[1])
			cnt[d]++
		}
		for _, x := range cnt {
			ans += x * (x - 1)
		}
	}
	return
}
```

#### TypeScript

```ts
function numberOfBoomerangs(points: number[][]): number {
    let ans = 0;
    for (const [x1, y1] of points) {
        const cnt: Map<number, number> = new Map();
        for (const [x2, y2] of points) {
            const d = (x1 - x2) ** 2 + (y1 - y2) ** 2;
            cnt.set(d, (cnt.get(d) || 0) + 1);
        }
        for (const [_, x] of cnt) {
            ans += x * (x - 1);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
