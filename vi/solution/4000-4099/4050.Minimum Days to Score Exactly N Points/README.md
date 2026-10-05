---
comments: true
difficulty: Medium
rating: 1696
source: Biweekly Contest 191 Q3
---

<!-- problem:start -->

# [4050. Minimum Days to Score Exactly N Points](https://leetcode.com/problems/minimum-days-to-score-exactly-n-points)

[中文文档](/solution/4000-4099/4050.Minimum%20Days%20to%20Score%20Exactly%20N%20Points/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code> biểu thị điểm số mục tiêu.</p>

<p>Điểm số bắt đầu từ 0, và mỗi ngày bạn có thể <strong>cộng</strong> điểm hoặc <strong>bỏ qua</strong>.</p>

<p>Điểm được cộng trong một chuỗi ngày liên tiếp. Vào ngày đầu tiên của một chuỗi, bạn nhận được 1 điểm, ngày thứ hai nhận được 2 điểm, ngày thứ ba nhận được 3 điểm, và cứ tiếp tục như vậy. <strong>Bỏ qua</strong> một ngày sẽ không nhận được <strong>điểm nào</strong> và <strong>đặt lại</strong> chuỗi, vì vậy lần tiếp theo cộng điểm, bạn sẽ bắt đầu lại từ 1.</p>

<p>Trả về số ngày <strong>ít nhất</strong>, bao gồm cả những ngày bỏ qua, cần thiết để đạt được điểm số <code>n</code> <strong>chính xác</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<ul>
	<li>Ngày 1: cộng 1 điểm. Điểm số là 1.</li>
	<li>Ngày 2: bỏ qua, thao tác này đặt lại chuỗi. Nếu cộng điểm vào ngày này, bạn sẽ thêm 2 điểm và vượt quá <code>n = 2</code>.</li>
	<li>Ngày 3: chuỗi đã được đặt lại, nên cộng điểm sẽ nhận được 1 điểm. Sau 3 ngày, điểm số chính xác là <code>n = 2</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 9</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<ul>
	<li>Ngày 1 đến ngày 3: cộng lần lượt 1, 2 và 3 điểm. Điểm số là <code>1 + 2 + 3 = 6</code>.</li>
	<li>Ngày 4: bỏ qua, thao tác này đặt lại chuỗi.</li>
	<li>Ngày 5 và ngày 6: cộng lần lượt 1 và 2 điểm. Sau 6 ngày, điểm số chính xác là <code>6 + 1 + 2 = 9</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 12</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<ul>
	<li>Ngày 1 đến ngày 3: cộng lần lượt 1, 2 và 3 điểm. Điểm số là <code>1 + 2 + 3 = 6</code>.</li>
	<li>Ngày 4: bỏ qua, thao tác này đặt lại chuỗi.</li>
	<li>Ngày 5 đến ngày 7: cộng lần lượt 1, 2 và 3 điểm. Sau 7 ngày, điểm số chính xác là <code>6 + 1 + 2 + 3 = 12</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Điểm số là tổng của các chuỗi: một chuỗi có độ dài $j$ đóng góp số tam giác $j(j+1)/2$. Các chuỗi liên tiếp phải được ngăn cách bởi một ngày bỏ qua để đặt lại chuỗi; chuỗi cuối cùng không cần ngày bỏ qua ở phía sau.
>
> Với $n = 10^5$, mô phỏng từng ngày và tìm kiếm trên các cách phân hoạch đều không phù hợp. Hãy xem “chính xác $i$ điểm” là một bài toán knapsack không giới hạn, trong đó mỗi item là một chuỗi và cost là số ngày.
>
> Đặt $f[0] = -1$ và luôn cộng $j + 1$ (thêm một ngày bỏ qua) trong bước chuyển. Ngày bỏ qua thêm vào chuỗi cuối cùng được triệt tiêu bởi $f[0] = -1$, nên đáp án là $f[n]$.

<!-- thinking:end -->

Một chuỗi dài $j$ ngày đạt số tam giác $s = \frac{j(j+1)}{2}$. Hai chuỗi liên tiếp phải được ngăn cách bởi đúng một ngày bỏ qua để đặt lại chuỗi, trong khi chuỗi cuối cùng không cần thêm ngày bỏ qua.

Gọi $f[i]$ là số ngày ít nhất cần thiết để đạt chính xác $i$ điểm. Đặt $f[0] = -1$ và khởi tạo các phần tử còn lại bằng $+\infty$. Xét độ dài $j$ của chuỗi cuối cùng (có số điểm là $s$):

$$
f[i] = \min\bigl(f[i],\, f[i - s] + j + 1\bigr)
$$

$j + 1$ là $j$ ngày cộng điểm cộng với một ngày bỏ qua. $f[0] = -1$ triệt tiêu ngày bỏ qua thêm vào chuỗi cuối cùng: nếu một chuỗi duy nhất có độ dài $j$ đã đạt $n$ điểm, thì $f[n] = f[0] + j + 1 = j$.

Vì $n \le 10^5$, ta tính trước đến giới hạn và trả lời mỗi truy vấn trong $O(1)$. Giá trị $j$ lớn nhất cần xét xấp xỉ $\sqrt{2n}$.

Độ phức tạp thời gian tiền xử lý là $O(n \times \sqrt{n})$, độ phức tạp không gian là $O(n)$. Mỗi truy vấn có độ phức tạp $O(1)$.

<!-- tabs:start -->

#### Python3

```python
mx = 10**5 + 1
f = [inf] * mx
f[0] = -1
for i in range(1, mx):
    j = 1
    while (s := (1 + j) * j // 2) <= i:
        f[i] = min(f[i], f[i - s] + j + 1)
        j += 1


class Solution:
    def minDays(self, n: int) -> int:
        return f[n]
```

#### Java

```java
class Solution {
    private static final int MX = 100001;
    private static final int[] f = new int[MX];

    static {
        Arrays.fill(f, Integer.MAX_VALUE);
        f[0] = -1;

        for (int i = 1; i < MX; i++) {
            for (int j = 1; j * (j + 1) / 2 <= i; j++) {
                int s = j * (j + 1) / 2;
                f[i] = Math.min(f[i], f[i - s] + j + 1);
            }
        }
    }

    public int minDays(int n) {
        return f[n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minDays(int n) {
        static const auto f = [] {
            constexpr int mx = 100001;
            vector<int> f(mx, INT_MAX);

            f[0] = -1;

            for (int i = 1; i < mx; i++) {
                for (int j = 1; j * (j + 1) / 2 <= i; j++) {
                    int s = j * (j + 1) / 2;
                    f[i] = std::min(f[i], f[i - s] + j + 1);
                }
            }

            return f;
        }();

        return f[n];
    }
};
```

#### Go

```go
const mx = 100001

var f = func() []int {
	f := make([]int, mx)

	for i := range f {
		f[i] = int(^uint(0) >> 1)
	}

	f[0] = -1

	for i := 1; i < mx; i++ {
		for j := 1; j*(j+1)/2 <= i; j++ {
			s := j * (j + 1) / 2
			f[i] = min(f[i], f[i-s]+j+1)
		}
	}

	return f
}()

func minDays(n int) int {
	return f[n]
}
```

#### TypeScript

```ts
const MX = 100001;

const f = new Array<number>(MX).fill(Infinity);

f[0] = -1;

for (let i = 1; i < MX; i++) {
    for (let j = 1; (j * (j + 1)) / 2 <= i; j++) {
        const s = (j * (j + 1)) / 2;
        f[i] = Math.min(f[i], f[i - s] + j + 1);
    }
}

function minDays(n: number): number {
    return f[n];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
