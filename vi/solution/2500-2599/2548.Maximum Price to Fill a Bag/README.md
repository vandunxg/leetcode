---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [2548. Maximum Price to Fill a Bag 🔒](https://leetcode.com/problems/maximum-price-to-fill-a-bag)

[中文文档](/solution/2500-2599/2548.Maximum%20Price%20to%20Fill%20a%20Bag/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên 2 chiều <code>items</code>, trong đó <code>items[i] = [price<sub>i</sub>, weight<sub>i</sub>]</code> lần lượt biểu thị giá và khối lượng của vật phẩm thứ <code>i<sup>th</sup></code>.</p>

<p>Đồng thời, cho một số nguyên <strong>dương</strong> <code>capacity</code>.</p>

<p>Mỗi vật phẩm có thể được chia thành hai vật phẩm với các tỉ lệ <code>part1</code> và <code>part2</code>, trong đó <code>part1 + part2 == 1</code>.</p>

<ul>
	<li>Khối lượng của vật phẩm thứ nhất là <code>weight<sub>i</sub> * part1</code> và giá của vật phẩm thứ nhất là <code>price<sub>i</sub> * part1</code>.</li>
	<li>Tương tự, khối lượng của vật phẩm thứ hai là <code>weight<sub>i</sub> * part2</code> và giá của vật phẩm thứ hai là <code>price<sub>i</sub> * part2</code>.</li>
</ul>

<p>Hãy trả về <em><strong>tổng giá tối đa</strong> để lấp đầy một chiếc túi có sức chứa</em> <code>capacity</code> <em>bằng các vật phẩm đã cho</em>. Nếu không thể lấp đầy túi, hãy trả về <code>-1</code>. Các đáp án có sai số không quá <code>10<sup>-5</sup></code> so với <strong>đáp án thực tế</strong> được xem là đúng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> items = [[50,1],[10,8]], capacity = 5
<strong>Đầu ra:</strong> 55.00000
<strong>Giải thích:</strong>
Chia vật phẩm thứ 2<sup>nd</sup> thành hai phần với part1 = 0.5 và part2 = 0.5.
Giá và khối lượng của vật phẩm thứ 1<sup>st</sup> lần lượt là 5, 4. Tương tự, giá và khối lượng của vật phẩm thứ 2<sup>nd</sup> lần lượt là 5, 4.
Mảng items sau thao tác này trở thành [[50,1],[5,4],[5,4]].
Để lấp đầy túi có sức chứa 5, ta lấy phần tử thứ 1<sup>st</sup> với giá 50 và phần tử thứ 2<sup>nd</sup> với giá 5.
Có thể chứng minh rằng 55.0 là tổng giá tối đa có thể đạt được.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> items = [[100,30]], capacity = 50
<strong>Đầu ra:</strong> -1.00000
<strong>Giải thích:</strong> Không thể lấp đầy túi bằng vật phẩm đã cho.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= items.length &lt;= 10<sup>5</sup></code></li>
	<li><code>items[i].length == 2</code></li>
	<li><code>1 &lt;= price<sub>i</sub>, weight<sub>i</sub> &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= capacity &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Có thể lấy vật phẩm theo bất kỳ phần khối lượng nào. Ta cần tối đa hóa giá và trả về $-1$ nếu không thể lấp đầy sức chứa. Đây là bài toán ba lô phân số: ưu tiên đơn giá cao hơn.
>
> Sắp xếp theo $w/p$ tăng dần tương đương với sắp xếp đơn giá theo thứ tự giảm dần. Với mỗi vật phẩm, lấy $\min(w,\textit{capacity})$ của vật phẩm đó và cộng thêm giá tương ứng theo tỉ lệ. Nếu sức chứa còn dư thì tổng khối lượng các vật phẩm không đủ.

<!-- thinking:end -->

Ta sắp xếp các vật phẩm theo thứ tự giảm dần của đơn giá, sau đó lần lượt lấy từng vật phẩm cho đến khi ba lô đầy.

Nếu cuối cùng ba lô vẫn chưa đầy, trả về $-1$; ngược lại, trả về tổng giá.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(\log n)$, trong đó $n$ là số lượng vật phẩm.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxPrice(self, items: List[List[int]], capacity: int) -> float:
        ans = 0
        for p, w in sorted(items, key=lambda x: x[1] / x[0]):
            v = min(w, capacity)
            ans += v / w * p
            capacity -= v
        return -1 if capacity else ans
```

#### Java

```java
class Solution {
    public double maxPrice(int[][] items, int capacity) {
        Arrays.sort(items, (a, b) -> a[1] * b[0] - a[0] * b[1]);
        double ans = 0;
        for (var e : items) {
            int p = e[0], w = e[1];
            int v = Math.min(w, capacity);
            ans += v * 1.0 / w * p;
            capacity -= v;
        }
        return capacity > 0 ? -1 : ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    double maxPrice(vector<vector<int>>& items, int capacity) {
        sort(items.begin(), items.end(), [&](const auto& a, const auto& b) { return a[1] * b[0] < a[0] * b[1]; });
        double ans = 0;
        for (auto& e : items) {
            int p = e[0], w = e[1];
            int v = min(w, capacity);
            ans += v * 1.0 / w * p;
            capacity -= v;
        }
        return capacity > 0 ? -1 : ans;
    }
};
```

#### Go

```go
func maxPrice(items [][]int, capacity int) (ans float64) {
	sort.Slice(items, func(i, j int) bool { return items[i][1]*items[j][0] < items[i][0]*items[j][1] })
	for _, e := range items {
		p, w := e[0], e[1]
		v := min(w, capacity)
		ans += float64(v) / float64(w) * float64(p)
		capacity -= v
	}
	if capacity > 0 {
		return -1
	}
	return
}
```

#### TypeScript

```ts
function maxPrice(items: number[][], capacity: number): number {
    items.sort((a, b) => a[1] * b[0] - a[0] * b[1]);
    let ans = 0;
    for (const [p, w] of items) {
        const v = Math.min(w, capacity);
        ans += (v / w) * p;
        capacity -= v;
    }
    return capacity ? -1 : ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
