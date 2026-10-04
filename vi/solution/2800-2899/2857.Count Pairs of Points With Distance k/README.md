---
comments: true
difficulty: Medium
rating: 2081
source: Biweekly Contest 113 Q3
tags:
    - Bit Manipulation
    - Array
    - Hash Table
---

<!-- problem:start -->

# [2857. Count Pairs of Points With Distance k](https://leetcode.com/problems/count-pairs-of-points-with-distance-k)

[中文文档](/solution/2800-2899/2857.Count%20Pairs%20of%20Points%20With%20Distance%20k/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>2D</strong> <code>coordinates</code> và một số nguyên <code>k</code>, trong đó <code>coordinates[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> là tọa độ của điểm thứ <code>i<sup>th</sup></code> trên mặt phẳng 2D.</p>

<p>Ta định nghĩa <strong>khoảng cách</strong> giữa hai điểm <code>(x<sub>1</sub>, y<sub>1</sub>)</code> và <code>(x<sub>2</sub>, y<sub>2</sub>)</code> là <code>(x1 XOR x2) + (y1 XOR y2)</code>, trong đó <code>XOR</code> là phép toán <code>XOR</code> theo bit.</p>

<p>Trả về <em>số lượng cặp </em><code>(i, j)</code><em> sao cho </em><code>i &lt; j</code><em> và khoảng cách giữa điểm </em><code>i</code><em> và điểm </em><code>j</code><em> bằng </em><code>k</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> coordinates = [[1,2],[4,2],[1,3],[5,2]], k = 5
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta có thể chọn các cặp sau:
- (0,1): Vì (1 XOR 4) + (2 XOR 2) = 5.
- (2,3): Vì (1 XOR 5) + (3 XOR 2) = 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> coordinates = [[1,3],[1,3],[1,3],[1,3],[1,3]], k = 0
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> Mọi cặp được chọn đều có khoảng cách bằng 0. Có 10 cách chọn hai điểm.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= coordinates.length &lt;= 50000</code></li>
	<li><code>0 &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= 10<sup>6</sup></code></li>
	<li><code>0 &lt;= k &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash table + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Khoảng cách là $(x_1\oplus x_2)+(y_1\oplus y_2)=k$. Với $k\le 100$, ta liệt kê XOR của $x$ là $a$ và đặt XOR của $y$ là $k-a$; điểm tương ứng được khôi phục bằng phép XOR. Một hash map lưu các điểm trước đó giúp đếm các cặp mà không bị đếm trùng.

<!-- thinking:end -->

Ta có thể sử dụng một hash table $cnt$ để đếm số lần xuất hiện của từng điểm trong mảng $coordinates$.

Tiếp theo, ta duyệt từng điểm $(x_2, y_2)$ trong mảng $coordinates$. Vì miền giá trị của $k$ là $[0, 100]$ và kết quả của $x_1 \oplus x_2$ hoặc $y_1 \oplus y_2$ luôn lớn hơn hoặc bằng $0$, ta có thể liệt kê kết quả $a$ của $x_1 \oplus x_2$ trong đoạn $[0,..k]$. Khi đó, kết quả của $y_1 \oplus y_2$ là $b = k - a$. Nhờ vậy, ta tính được các giá trị của $x_1$ và $y_1$, rồi cộng số lần xuất hiện của $(x_1, y_1)$ vào đáp án.

Độ phức tạp thời gian là $O(n \times k)$, độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $coordinates$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countPairs(self, coordinates: List[List[int]], k: int) -> int:
        cnt = Counter()
        ans = 0
        for x2, y2 in coordinates:
            for a in range(k + 1):
                b = k - a
                x1, y1 = a ^ x2, b ^ y2
                ans += cnt[(x1, y1)]
            cnt[(x2, y2)] += 1
        return ans
```

#### Java

```java
class Solution {
    public int countPairs(List<List<Integer>> coordinates, int k) {
        Map<List<Integer>, Integer> cnt = new HashMap<>();
        int ans = 0;
        for (var c : coordinates) {
            int x2 = c.get(0), y2 = c.get(1);
            for (int a = 0; a <= k; ++a) {
                int b = k - a;
                int x1 = a ^ x2, y1 = b ^ y2;
                ans += cnt.getOrDefault(List.of(x1, y1), 0);
            }
            cnt.merge(c, 1, Integer::sum);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countPairs(vector<vector<int>>& coordinates, int k) {
        map<pair<int, int>, int> cnt;
        int ans = 0;
        for (auto& c : coordinates) {
            int x2 = c[0], y2 = c[1];
            for (int a = 0; a <= k; ++a) {
                int b = k - a;
                int x1 = a ^ x2, y1 = b ^ y2;
                ans += cnt[{x1, y1}];
            }
            ++cnt[{x2, y2}];
        }
        return ans;
    }
};
```

#### Go

```go
func countPairs(coordinates [][]int, k int) (ans int) {
	cnt := map[[2]int]int{}
	for _, c := range coordinates {
		x2, y2 := c[0], c[1]
		for a := 0; a <= k; a++ {
			b := k - a
			x1, y1 := a^x2, b^y2
			ans += cnt[[2]int{x1, y1}]
		}
		cnt[[2]int{x2, y2}]++
	}
	return
}
```

#### TypeScript

```ts
function countPairs(coordinates: number[][], k: number): number {
    const cnt: Map<number, number> = new Map();
    const f = (x: number, y: number): number => x * 1000000 + y;
    let ans = 0;
    for (const [x2, y2] of coordinates) {
        for (let a = 0; a <= k; ++a) {
            const b = k - a;
            const [x1, y1] = [a ^ x2, b ^ y2];
            ans += cnt.get(f(x1, y1)) ?? 0;
        }
        cnt.set(f(x2, y2), (cnt.get(f(x2, y2)) ?? 0) + 1);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
