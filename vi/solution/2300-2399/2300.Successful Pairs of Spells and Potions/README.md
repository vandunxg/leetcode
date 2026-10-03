---
comments: true
difficulty: Medium
rating: 1476
source: Biweekly Contest 80 Q2
tags:
    - Array
    - Two Pointers
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [2300. Successful Pairs of Spells and Potions](https://leetcode.com/problems/successful-pairs-of-spells-and-potions)

[中文文档](/solution/2300-2399/2300.Successful%20Pairs%20of%20Spells%20and%20Potions/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng số nguyên dương <code>spells</code> và <code>potions</code>, lần lượt có độ dài <code>n</code> và <code>m</code>, trong đó <code>spells[i]</code> biểu thị sức mạnh của phép thuật thứ <code>i<sup>th</sup></code> và <code>potions[j]</code> biểu thị độ mạnh của lọ thuốc thứ <code>j<sup>th</sup></code>.</p>

<p>Bạn cũng được cho một số nguyên <code>success</code>. Một cặp phép thuật và lọ thuốc được xem là <strong>thành công</strong> nếu <strong>tích</strong> sức mạnh của chúng <strong>lớn hơn hoặc bằng</strong> <code>success</code>.</p>

<p>Trả về <em>một mảng số nguyên </em><code>pairs</code><em> có độ dài </em><code>n</code><em>, trong đó </em><code>pairs[i]</code><em> là số lượng <strong>lọ thuốc</strong> tạo thành một cặp thành công với phép thuật thứ </em><code>i<sup>th</sup></code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> spells = [5,1,3], potions = [1,2,3,4,5], success = 7
<strong>Đầu ra:</strong> [4,0,3]
<strong>Giải thích:</strong>
- Phép thuật thứ 0<sup>th</sup>: 5 * [1,2,3,4,5] = [5,<u><strong>10</strong></u>,<u><strong>15</strong></u>,<u><strong>20</strong></u>,<u><strong>25</strong></u>]. Có 4 cặp thành công.
- Phép thuật thứ 1<sup>st</sup>: 1 * [1,2,3,4,5] = [1,2,3,4,5]. Có 0 cặp thành công.
- Phép thuật thứ 2<sup>nd</sup>: 3 * [1,2,3,4,5] = [3,6,<u><strong>9</strong></u>,<u><strong>12</strong></u>,<u><strong>15</strong></u>]. Có 3 cặp thành công.
Vì vậy, trả về [4,0,3].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> spells = [3,1,2], potions = [8,5,8], success = 16
<strong>Đầu ra:</strong> [2,0,2]
<strong>Giải thích:</strong>
- Phép thuật thứ 0<sup>th</sup>: 3 * [8,5,8] = [<u><strong>24</strong></u>,15,<u><strong>24</strong></u>]. Có 2 cặp thành công.
- Phép thuật thứ 1<sup>st</sup>: 1 * [8,5,8] = [8,5,8]. Có 0 cặp thành công.
- Phép thuật thứ 2<sup>nd</sup>: 2 * [8,5,8] = [<strong><u>16</u></strong>,10,<u><strong>16</strong></u>]. Có 2 cặp thành công.
Vì vậy, trả về [2,0,2].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == spells.length</code></li>
	<li><code>m == potions.length</code></li>
	<li><code>1 &lt;= n, m &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= spells[i], potions[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= success &lt;= 10<sup>10</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Duyệt qua mọi lọ thuốc với từng phép thuật có độ phức tạp $O(nm)$. Với $n, m \le 10^5$, cách này không đáp ứng được.
>
> Với một phép thuật cố định $v$, điều kiện thành công là $potions[j] \ge \frac{success}{v}$, tạo thành một ngưỡng đơn điệu trên độ mạnh của lọ thuốc. Sắp xếp các lọ thuốc, rồi tìm kiếm nhị phân lọ đầu tiên hợp lệ; toàn bộ phần đuôi sau đó sẽ tạo thành cặp với $v$. Mỗi phép thuật chỉ cần một truy vấn có độ phức tạp logarit.

<!-- thinking:end -->

Ta có thể sắp xếp mảng lọ thuốc, sau đó duyệt qua mảng phép thuật. Với mỗi phép thuật $v$, ta dùng tìm kiếm nhị phân để tìm lọ thuốc đầu tiên lớn hơn hoặc bằng $\frac{success}{v}$. Gọi chỉ số của lọ này là $i$. Độ dài của mảng lọ thuốc trừ đi $i$ là số lượng lọ thuốc có thể kết hợp thành công với phép thuật này.

Độ phức tạp thời gian là $O((m + n) \times \log m)$, độ phức tạp không gian là $O(\log n)$. Trong đó, $m$ và $n$ lần lượt là độ dài của mảng lọ thuốc và mảng phép thuật.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def successfulPairs(
        self, spells: List[int], potions: List[int], success: int
    ) -> List[int]:
        potions.sort()
        m = len(potions)
        return [m - bisect_left(potions, success / v) for v in spells]
```

#### Java

```java
class Solution {
    public int[] successfulPairs(int[] spells, int[] potions, long success) {
        Arrays.sort(potions);
        int n = spells.length, m = potions.length;
        int[] ans = new int[n];
        for (int i = 0; i < n; ++i) {
            int left = 0, right = m;
            while (left < right) {
                int mid = (left + right) >> 1;
                if ((long) spells[i] * potions[mid] >= success) {
                    right = mid;
                } else {
                    left = mid + 1;
                }
            }
            ans[i] = m - left;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> successfulPairs(vector<int>& spells, vector<int>& potions, long long success) {
        ranges::sort(potions);
        const int m = potions.size();
        vector<int> ans;
        ans.reserve(spells.size());

        for (int v : spells) {
            auto it = ranges::lower_bound(potions, static_cast<double>(success) / v);
            ans.push_back(m - static_cast<int>(it - potions.begin()));
        }
        return ans;
    }
};
```

#### Go

```go
func successfulPairs(spells []int, potions []int, success int64) (ans []int) {
	sort.Ints(potions)
	m := len(potions)
	for _, v := range spells {
		i := sort.Search(m, func(i int) bool { return int64(potions[i]*v) >= success })
		ans = append(ans, m-i)
	}
	return ans
}
```

#### TypeScript

```ts
function successfulPairs(spells: number[], potions: number[], success: number): number[] {
    potions.sort((a, b) => a - b);
    const m = potions.length;
    const ans: number[] = [];

    for (const v of spells) {
        const targetPotion = success / v;
        const idx = _.sortedIndexBy(potions, targetPotion, p => p);
        ans.push(m - idx);
    }

    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn successful_pairs(spells: Vec<i32>, mut potions: Vec<i32>, success: i64) -> Vec<i32> {
        potions.sort();
        let m = potions.len();
        let mut ans = Vec::with_capacity(spells.len());

        for &v in &spells {
            let target = (success + v as i64 - 1) / v as i64;
            let idx = potions.partition_point(|&p| (p as i64) < target);
            ans.push((m - idx) as i32);
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
