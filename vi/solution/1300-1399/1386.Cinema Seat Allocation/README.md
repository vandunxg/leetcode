---
comments: true
difficulty: Medium
rating: 1636
source: Biweekly Contest 22 Q2
tags:
    - Greedy
    - Bit Manipulation
    - Array
    - Hash Table
---

<!-- problem:start -->

# [1386. Cinema Seat Allocation](https://leetcode.com/problems/cinema-seat-allocation)

[中文文档](/solution/1300-1399/1386.Cinema%20Seat%20Allocation/README.md)

## Mô tả

<!-- description:start -->

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1386.Cinema%20Seat%20Allocation/images/cinema_seats_1.png" style="width: 400px; height: 149px;" /></p>

<p>Một rạp chiếu phim có <code>n</code> hàng ghế, đánh số từ 1 đến <code>n</code>. Mỗi hàng có 10 ghế, đánh số từ 1 đến 10.</p>

<p>Cho mảng số nguyên 2D <code data-end="170" data-start="155">reservedSeats</code>, trong đó <code data-end="212" data-start="178">reservedSeats[i] = [row<sub>i</sub>, seat<sub>i</sub>]</code> nghĩa là ghế <code data-end="236" data-start="229">seat<sub>i</sub></code> ở hàng <code data-end="250" data-start="244">row<sub>i</sub></code> đã được đặt trước.</p>

<p>Một nhóm bốn người cần được xếp vào bốn ghế trong <strong>cùng một</strong> hàng. Nhóm có thể ngồi ở một trong các dãy ghế sau:</p>

<ul>
	<li>các ghế <code data-end="423" data-start="411">2, 3, 4, 5</code></li>
	<li>các ghế <code data-end="444" data-start="432">4, 5, 6, 7</code></li>
	<li>các ghế <code data-end="465" data-start="453">6, 7, 8, 9</code></li>
</ul>

<p>Chỉ có thể sử dụng một dãy ghế nếu <strong>không ghế nào</strong> trong dãy đã được đặt trước. Mỗi ghế được xếp cho <strong>nhiều nhất </strong>một nhóm.</p>

<p>Trả về số nguyên biểu thị số nhóm bốn người tối đa có thể xếp chỗ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1386.Cinema%20Seat%20Allocation/images/cinema_seats_3.png" style="width: 400px; height: 96px;" /></p>

<pre>
<strong>Đầu vào:</strong> n = 3, reservedSeats = [[1,2],[1,3],[1,8],[2,6],[3,1],[3,10]]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Hình trên minh họa cách xếp tối ưu cho bốn nhóm. Các ghế màu xanh đã được đặt trước, còn mỗi nhóm bốn ghế liền nhau màu cam được xếp cho một nhóm.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2, reservedSeats = [[2,1],[1,8],[2,6]]
<strong>Đầu ra:</strong> 2
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4, reservedSeats = [[4,3],[1,4],[4,6],[1,7]]
<strong>Đầu ra:</strong> 4
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= reservedSeats.length &lt;= min(10 * n, 10<sup>4</sup>)</code></li>
	<li><code>reservedSeats[i] == [row<sub>i</sub>, seat<sub>i</sub>]</code></li>
	<li><code>1 &lt;= row<sub>i</sub> &lt;= n</code></li>
	<li><code>1 &lt;= seat<sub>i</sub> &lt;= 10</code></li>
	<li>Mọi <code>reservedSeats[i]</code> đều khác nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash table + thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Một nhóm chiếm bốn ghế liên tiếp ($2$– $5$, $4$– $7$ hoặc $6$– $9$). Vì $n$ có thể lên đến $10^9$, không thể duyệt các hàng không có ghế đặt trước. Mỗi hàng chưa có ghế đặt trước xếp được hai nhóm, đóng góp $2(n-|d|)$. Với hàng có ghế đặt trước, ta biểu diễn trạng thái bằng mask 10 bit; lần lượt xét ba dãy ghế, và đánh dấu mask khi xếp được để hai nhóm không dùng chung ghế.

<!-- thinking:end -->

Ta dùng hash table $d$ để lưu các ghế đã đặt trước: key là số hàng, còn value là trạng thái ghế đã đặt trong hàng đó, được biểu diễn bằng số nhị phân. Bit thứ $j$ bằng $1$ nghĩa là ghế thứ $j$ đã được đặt trước; bằng $0$ nghĩa là ghế chưa được đặt.

Ta duyệt $reservedSeats$. Với mỗi ghế $(i, j)$, ta thêm trạng thái của ghế thứ $j$ (tương ứng với bit thứ $10-j$ trong các bit thấp) vào $d[i]$.

Với các hàng không xuất hiện trong hash table $d$, ta có thể tùy ý xếp $2$ nhóm, nên khởi tạo đáp án là $(n - len(d)) \times 2$.

Tiếp theo, ta duyệt trạng thái của từng hàng trong hash table. Với mỗi hàng, lần lượt thử xếp các nhóm ghế $1234, 5678, 3456$. Nếu có thể xếp một nhóm, ta cộng $1$ vào đáp án.

Sau khi duyệt xong, ta thu được đáp án cuối cùng.

Độ phức tạp thời gian là $O(m)$ và độ phức tạp không gian là $O(m)$, với $m$ là độ dài của $reservedSeats$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxNumberOfFamilies(self, n: int, reservedSeats: List[List[int]]) -> int:
        d = defaultdict(int)
        for i, j in reservedSeats:
            d[i] |= 1 << (10 - j)
        masks = (0b0111100000, 0b0000011110, 0b0001111000)
        ans = (n - len(d)) * 2
        for x in d.values():
            for mask in masks:
                if (x & mask) == 0:
                    x |= mask
                    ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int maxNumberOfFamilies(int n, int[][] reservedSeats) {
        Map<Integer, Integer> d = new HashMap<>();
        for (var e : reservedSeats) {
            int i = e[0], j = e[1];
            d.merge(i, 1 << (10 - j), (x, y) -> x | y);
        }
        int[] masks = {0b0111100000, 0b0000011110, 0b0001111000};
        int ans = (n - d.size()) * 2;
        for (int x : d.values()) {
            for (int mask : masks) {
                if ((x & mask) == 0) {
                    x |= mask;
                    ++ans;
                }
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
    int maxNumberOfFamilies(int n, vector<vector<int>>& reservedSeats) {
        unordered_map<int, int> d;
        for (auto& e : reservedSeats) {
            int i = e[0], j = e[1];
            d[i] |= 1 << (10 - j);
        }
        int masks[3] = {0b0111100000, 0b0000011110, 0b0001111000};
        int ans = (n - d.size()) * 2;
        for (auto& [_, x] : d) {
            for (int& mask : masks) {
                if ((x & mask) == 0) {
                    x |= mask;
                    ++ans;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxNumberOfFamilies(n int, reservedSeats [][]int) int {
	d := map[int]int{}
	for _, e := range reservedSeats {
		i, j := e[0], e[1]
		d[i] |= 1 << (10 - j)
	}
	ans := (n - len(d)) * 2
	masks := [3]int{0b0111100000, 0b0000011110, 0b0001111000}
	for _, x := range d {
		for _, mask := range masks {
			if x&mask == 0 {
				x |= mask
				ans++
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maxNumberOfFamilies(n: number, reservedSeats: number[][]): number {
    const d: Map<number, number> = new Map();
    for (const [i, j] of reservedSeats) {
        d.set(i, (d.get(i) ?? 0) | (1 << (10 - j)));
    }
    let ans = (n - d.size) << 1;
    const masks = [0b0111100000, 0b0000011110, 0b0001111000];
    for (let [_, x] of d) {
        for (const mask of masks) {
            if ((x & mask) === 0) {
                x |= mask;
                ++ans;
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn max_number_of_families(n: i32, reserved_seats: Vec<Vec<i32>>) -> i32 {
        let mut d: HashMap<i32, i32> = HashMap::new();

        for e in reserved_seats {
            let row = e[0];
            let col = e[1];
            let mask = 1 << (10 - col);

            d.entry(row)
                .and_modify(|x| *x |= mask)
                .or_insert(mask);
        }

        let masks = [
            0b0111100000,
            0b0000011110,
            0b0001111000,
        ];

        let mut ans = (n - d.len() as i32) * 2;

        for mut x in d.values().copied() {
            for &mask in &masks {
                if (x & mask) == 0 {
                    x |= mask;
                    ans += 1;
                }
            }
        }

        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int MaxNumberOfFamilies(int n, int[][] reservedSeats) {
        Dictionary<int, int> d = new Dictionary<int, int>();

        foreach (var e in reservedSeats) {
            int row = e[0];
            int col = e[1];
            int mask = 1 << (10 - col);

            if (d.ContainsKey(row)) {
                d[row] |= mask;
            } else {
                d[row] = mask;
            }
        }

        int[] masks = {
            0b0111100000,
            0b0000011110,
            0b0001111000
        };

        int ans = (n - d.Count) * 2;

        foreach (int value in d.Values) {
            int x = value;

            foreach (int mask in masks) {
                if ((x & mask) == 0) {
                    x |= mask;
                    ans++;
                }
            }
        }

        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
