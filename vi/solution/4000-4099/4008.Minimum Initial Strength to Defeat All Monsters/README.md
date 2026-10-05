---
comments: true
difficulty: Medium
rating: 1776
source: Biweekly Contest 188 Q3
tags:
    - Greedy
    - Array
    - Binary Search
    - Prefix Sum
---

<!-- problem:start -->

# [4008. Minimum Initial Strength to Defeat All Monsters](https://leetcode.com/problems/minimum-initial-strength-to-defeat-all-monsters)

[中文文档](/solution/4000-4099/4008.Minimum%20Initial%20Strength%20to%20Defeat%20All%20Monsters/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>monsters</code>, trong đó <code>monsters[i]</code> biểu thị sức mạnh của quái vật thứ <code>i<sup>th</sup></code>.</p>

<p>Đồng thời cho một mảng số nguyên 2D <code>boosts</code>, trong đó <code>boosts[i] = [l<sub>i</sub>, r<sub>i</sub>, v<sub>i</sub>]</code> cho biết <code>v<sub>i</sub></code> được cộng vào <strong>phần thưởng tạm thời</strong> khi chiến đấu với bất kỳ quái vật nào có chỉ số nằm trong <code>[l<sub>i</sub>, r<sub>i</sub>]</code>. Các khoảng boost có thể chồng lấn, và các giá trị của mọi boost áp dụng được sẽ được cộng lại.</p>

<p>Bạn bắt đầu với sức mạnh ban đầu <strong>không âm</strong> và chiến đấu với các quái vật từ trái sang phải.</p>

<p>Với mỗi quái vật ở chỉ số <code>i</code>:</p>

<ul>
	<li>Gọi <code>bonus</code> là <strong>tổng</strong> giá trị của tất cả boost áp dụng cho quái vật <code>i</code>.</li>
	<li>Bạn chỉ có thể đánh bại quái vật nếu sức mạnh hiện tại cộng với <code>bonus</code> <strong>ít nhất</strong> bằng <code>monsters[i]</code>.</li>
	<li>Sau khi đánh bại quái vật, chỉ sức mạnh hiện tại bị giảm đi <code>monsters[i]</code>. Nếu trở thành <strong>âm</strong>, nó được đặt về 0.</li>
</ul>

<p>Trả về sức mạnh ban đầu <strong>nhỏ nhất</strong> cần thiết để đánh bại tất cả quái vật.</p>

<p>Lưu ý: Phần thưởng tạm thời chỉ được dùng để xác định xem có thể đánh bại quái vật hiện tại hay không. Nó không làm thay đổi sức mạnh hiện tại theo cách nào khác.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">monsters = [5,10,15], boosts = [[1,1,10]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">30</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta bắt đầu với sức mạnh ban đầu là 30.</p>

<ul>
	<li><code>monsters[0] = 5</code>: Tại chỉ số 0, bonus là 0. Vì <code>30 + 0 &gt;= 5</code>, ta có thể đánh bại quái vật này. Sức mạnh còn lại là <code>30 - 5 = 25</code>.</li>
	<li><code>monsters[1] = 10</code>: Tại chỉ số 1, bonus là 10. Vì <code>25 + 10 &gt;= 10</code>, ta có thể đánh bại quái vật này. Sức mạnh còn lại là <code>25 - 10 = 15</code>.</li>
	<li><code>monsters[2] = 15</code>: Tại chỉ số 2, bonus là 0. Vì <code>15 + 0 &gt;= 15</code>, ta có thể đánh bại quái vật này. Sức mạnh còn lại là <code>15 - 15 = 0</code>.</li>
</ul>

<p>Vậy sức mạnh ban đầu nhỏ nhất cần thiết là 30.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">monsters = [5,10,15], boosts = [[1,2,10],[1,2,5]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta bắt đầu với sức mạnh ban đầu là 5.</p>

<ul>
	<li><code>monsters[0] = 5</code>: Bonus là 0. Vì <code>5 + 0 &gt;= 5</code>, ta có thể đánh bại quái vật này. Sức mạnh còn lại là <code>5 - 5 = 0</code>.</li>
	<li><code>monsters[1] = 10</code>: Hai boost chồng lấn cung cấp <code>bonus = 10 + 5 = 15</code>. Vì <code>0 + 15 &gt;= 10</code>, ta có thể đánh bại quái vật này. Sức mạnh vẫn là 0.</li>
	<li><code>monsters[2] = 15</code>: Hai boost chồng lấn một lần nữa cung cấp <code>bonus = 15</code>. Vì <code>0 + 15 &gt;= 15</code>, ta có thể đánh bại quái vật này. Sức mạnh vẫn là 0.</li>
</ul>

<p>Vậy sức mạnh ban đầu nhỏ nhất cần thiết là 5.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= monsters.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= monsters[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= boosts.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>boosts[i] == [l<sub>i</sub>, r<sub>i</sub>, v<sub>i</sub>]</code></li>
	<li><code>0 &lt;= l<sub>i</sub> &lt;= r<sub>i</sub> &lt; monsters.length</code></li>
	<li><code>1 &lt;= v<sub>i</sub> &lt;= 10<sup>9</sup></code>​​​​​​​</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng hiệu + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Sức mạnh ban đầu càng lớn thì việc đánh bại mọi quái vật càng dễ, nên mệnh đề khả thi có tính đơn điệu và cho phép tìm kiếm nhị phân.
>
> Nếu áp dụng từng boost như phép cộng trên khoảng trong mỗi lần kiểm tra, chi phí sẽ nhân với số lượng boost. Mảng hiệu biến mỗi boost thành hai cập nhật ở đầu mút; sau đó một lượt duyệt cùng các quái vật có thể kiểm tra một ứng viên trong $O(n)$.
>
> Cận trên $10^{15}$ đã đủ bao phủ tổng sức mạnh của mọi quái vật, nên tìm kiếm nhị phân sẽ cho giá trị khởi đầu nhỏ nhất khả thi.

<!-- thinking:end -->

Mỗi boost cộng một giá trị vào toàn bộ khoảng chỉ số $[l, r]$, nên trước hết ta áp dụng tất cả boost bằng một mảng hiệu $d$. Khi đó, $\textit{bonus}$ lúc chiến đấu với quái vật thứ $i$ là tổng tiền tố $\sum_{j=0}^{i} d[j]$.

Tiếp theo, ta tìm kiếm nhị phân sức mạnh ban đầu $v$. Với một $v$ cho trước, ta mô phỏng các trận chiến từ trái sang phải: duy trì $\textit{bonus}$ hiện tại (tổng tiền tố của mảng hiệu); nếu $v + \textit{bonus} < \textit{monsters}[i]$, quái vật không thể bị đánh bại và $v$ là không khả thi; ngược lại, ta đánh bại nó, giảm $v$ đi $\textit{monsters}[i]$, rồi đặt $v$ về $0$ nếu nó trở thành số âm. Nếu đánh bại được tất cả quái vật thì $v$ là khả thi.

Sức mạnh ban đầu lớn hơn không bao giờ khiến việc đánh bại tất cả quái vật khó hơn, nên tính khả thi đơn điệu theo $v$ và ta có thể tìm kiếm nhị phân sức mạnh ban đầu nhỏ nhất. Cận trên của phép tìm kiếm được đặt là $10^{15}$ (tổng sức mạnh của tất cả quái vật nhiều nhất là $5 \times 10^4 \times 10^9 = 5 \times 10^{13}$).

Độ phức tạp thời gian là $O((n + m) \times \log M)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là số quái vật, $m$ là số boost, và $M = 10^{15}$ là cận trên của tìm kiếm nhị phân.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minInitialStrength(self, monsters: list[int], boosts: list[list[int]]) -> int:
        def check(v: int) -> bool:
            bonus = 0
            for a, b in zip(monsters, d):
                bonus += b
                if v + bonus < a:
                    return False
                v -= a
                v = max(v, 0)
            return True

        n = len(monsters)
        d = [0] * (n + 1)
        for l, r, v in boosts:
            d[l] += v
            d[r + 1] -= v

        l, r = 0, 10**15
        while l < r:
            mid = (l + r) >> 1
            if check(mid):
                r = mid
            else:
                l = mid + 1
        return l
```

#### Java

```java
class Solution {
    private int[] monsters;
    private long[] d;

    public long minInitialStrength(int[] monsters, int[][] boosts) {
        this.monsters = monsters;
        int n = monsters.length;
        d = new long[n + 1];
        for (int[] b : boosts) {
            d[b[0]] += b[2];
            d[b[1] + 1] -= b[2];
        }

        long left = 0, right = (long) 1e15;
        while (left < right) {
            long mid = (left + right) >>> 1;
            if (check(mid)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }

    private boolean check(long v) {
        long bonus = 0;
        for (int i = 0; i < monsters.length; i++) {
            bonus += d[i];
            if (v + bonus < monsters[i]) {
                return false;
            }
            v -= monsters[i];
            if (v < 0) {
                v = 0;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minInitialStrength(vector<int>& monsters, vector<vector<int>>& boosts) {
        int n = monsters.size();
        vector<long long> d(n + 1);
        for (auto& b : boosts) {
            d[b[0]] += b[2];
            d[b[1] + 1] -= b[2];
        }

        auto check = [&](long long v) -> bool {
            long long bonus = 0;
            for (int i = 0; i < n; i++) {
                bonus += d[i];
                if (v + bonus < monsters[i]) {
                    return false;
                }
                v -= monsters[i];
                if (v < 0) {
                    v = 0;
                }
            }
            return true;
        };

        long long left = 0, right = 1000000000000000LL;
        while (left < right) {
            long long mid = (left + right) / 2;
            if (check(mid)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
};
```

#### Go

```go
func minInitialStrength(monsters []int, boosts [][]int) int64 {
    n := len(monsters)
    d := make([]int64, n+1)
    for _, b := range boosts {
        d[b[0]] += int64(b[2])
        d[b[1]+1] -= int64(b[2])
    }

    check := func(v int64) bool {
        var bonus int64
        for i, a := range monsters {
            bonus += d[i]
            if v+bonus < int64(a) {
                return false
            }
            v -= int64(a)
            if v < 0 {
                v = 0
            }
        }
        return true
    }

    var left, right int64 = 0, 1000000000000000
    for left < right {
        mid := (left + right) / 2
        if check(mid) {
            right = mid
        } else {
            left = mid + 1
        }
    }
    return left
}
```

#### TypeScript

```ts
function minInitialStrength(monsters: number[], boosts: number[][]): number {
    const n = monsters.length;
    const d = new Array<number>(n + 1).fill(0);

    for (const [l, r, v] of boosts) {
        d[l] += v;
        d[r + 1] -= v;
    }

    const check = (v: number): boolean => {
        let bonus = 0;
        for (let i = 0; i < n; i++) {
            bonus += d[i];
            if (v + bonus < monsters[i]) {
                return false;
            }
            v -= monsters[i];
            if (v < 0) {
                v = 0;
            }
        }
        return true;
    };

    let left = 0;
    let right = 1e15;
    while (left < right) {
        const mid = Math.floor((left + right) / 2);
        if (check(mid)) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    return left;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
