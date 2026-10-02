---
comments: true
difficulty: Medium
rating: 1919
source: Weekly Contest 202 Q3
tags:
    - Array
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [1552. Magnetic Force Between Two Balls](https://leetcode.com/problems/magnetic-force-between-two-balls)

[中文文档](/solution/1500-1599/1552.Magnetic%20Force%20Between%20Two%20Balls/README.md)

## Mô tả

<!-- description:start -->

<p>Trong vũ trụ Earth C-137, Rick phát hiện một dạng lực từ đặc biệt giữa hai quả bóng khi chúng được đặt vào chiếc giỏ mới phát minh. Rick có <code>n</code> chiếc giỏ trống, chiếc giỏ thứ <code>i<sup>th</sup></code> ở vị trí <code>position[i]</code>, còn Morty có <code>m</code> quả bóng và cần phân bổ chúng vào các giỏ sao cho <strong>lực từ nhỏ nhất</strong> giữa bất kỳ hai quả bóng nào là <strong>lớn nhất</strong>.</p>

<p>Rick cho biết lực từ giữa hai quả bóng khác nhau ở các vị trí <code>x</code> và <code>y</code> là <code>|x - y|</code>.</p>

<p>Cho mảng số nguyên <code>position</code> và số nguyên <code>m</code>. Hãy trả về <em>lực từ cần tìm</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1552.Magnetic%20Force%20Between%20Two%20Balls/images/q3v1.jpg" style="width: 562px; height: 195px;" />
<pre>
<strong>Đầu vào:</strong> position = [1,2,3,4,7], m = 3
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Phân bổ 3 quả bóng vào các giỏ 1, 4 và 7 tạo ra lực từ giữa các cặp bóng là [3, 3, 6]. Lực từ nhỏ nhất là 3. Không thể đạt lực từ nhỏ nhất lớn hơn 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> position = [5,4,3,2,1,1000000000], m = 2
<strong>Đầu ra:</strong> 999999999
<strong>Giải thích:</strong> Ta có thể dùng các giỏ 1 và 1000000000.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == position.length</code></li>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= position[i] &lt;= 10<sup>9</sup></code></li>
	<li>Mọi số nguyên trong <code>position</code> đều <strong>khác nhau</strong>.</li>
	<li><code>2 &lt;= m &lt;= position.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Đặt $m$ quả bóng để tối đa hóa khoảng cách nhỏ nhất. Vị trí có thể lên tới $10^9$, nên không thể liệt kê các cách sắp xếp theo từng khoảng cách; nhưng với $n\le 10^5$, ta vẫn có thể kiểm tra tính khả thi nhanh.
>
> Khoảng cách càng lớn thì số bóng đặt được càng ít, tạo thành tính đơn điệu. Sau khi sắp xếp, ta tìm kiếm nhị phân khoảng cách $f$ và duyệt từ trái sang phải, chỉ đặt bóng khi nó cách quả trước đó ít nhất $f$. Dùng điều kiện “không thể đặt $m$ quả bóng” làm mốc tìm kiếm; giá trị ngay trước khoảng cách thất bại đầu tiên $f$ là khoảng cách khả thi lớn nhất.

<!-- thinking:end -->

Ta nhận thấy lực từ nhỏ nhất giữa các cặp bóng càng lớn thì càng đặt được ít bóng, thể hiện tính đơn điệu. Ta có thể dùng tìm kiếm nhị phân để tìm lực từ nhỏ nhất lớn nhất sao cho đặt được ít nhất $m$ quả bóng.

Trước tiên, ta sắp xếp vị trí các giỏ, sau đó dùng tìm kiếm nhị phân với biên trái $l = 1$ và biên phải $r = \textit{position}[n - 1]$, trong đó $n$ là số giỏ. Ở mỗi vòng lặp, ta tính trung điểm $m = (l + r + 1) / 2$, rồi xác định xem có thể đặt bóng sao cho số bóng được đặt không nhỏ hơn $m$ hay không.

Bài toán được chuyển thành việc xác định liệu lực từ nhỏ nhất cho trước $f$ có cho phép đặt $m$ quả bóng hay không. Ta duyệt vị trí các giỏ từ trái sang phải; nếu khoảng cách giữa vị trí quả bóng cuối cùng và giỏ hiện tại lớn hơn hoặc bằng $f$, ta có thể đặt một quả bóng vào giỏ đó. Cuối cùng, kiểm tra xem số bóng đã đặt có ít nhất là $m$ hay không.

Độ phức tạp thời gian là $O(n \times \log n + n \times \log M)$, còn độ phức tạp không gian là $O(\log n)$. Ở đây, $n$ và $M$ lần lượt là số giỏ và giá trị lớn nhất của các vị trí giỏ.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxDistance(self, position: List[int], m: int) -> int:
        def check(f: int) -> bool:
            prev = -inf
            cnt = 0
            for curr in position:
                if curr - prev >= f:
                    prev = curr
                    cnt += 1
            return cnt < m

        position.sort()
        l, r = 1, position[-1]
        return bisect_left(range(l, r + 1), True, key=check)
```

#### Java

```java
class Solution {
    private int[] position;

    public int maxDistance(int[] position, int m) {
        Arrays.sort(position);
        this.position = position;
        int l = 1, r = position[position.length - 1];
        while (l < r) {
            int mid = (l + r + 1) >> 1;
            if (count(mid) >= m) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return l;
    }

    private int count(int f) {
        int prev = position[0];
        int cnt = 1;
        for (int curr : position) {
            if (curr - prev >= f) {
                ++cnt;
                prev = curr;
            }
        }
        return cnt;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxDistance(vector<int>& position, int m) {
        ranges::sort(position);
        int l = 1, r = position.back();
        auto count = [&](int f) {
            int prev = position[0];
            int cnt = 1;
            for (int& curr : position) {
                if (curr - prev >= f) {
                    prev = curr;
                    cnt++;
                }
            }
            return cnt;
        };
        while (l < r) {
            int mid = (l + r + 1) >> 1;
            if (count(mid) >= m) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return l;
    }
};
```

#### Go

```go
func maxDistance(position []int, m int) int {
	sort.Ints(position)
	return sort.Search(position[len(position)-1], func(f int) bool {
		prev := position[0]
		cnt := 1
		for _, curr := range position {
			if curr-prev >= f {
				cnt++
				prev = curr
			}
		}
		return cnt < m
	}) - 1
}
```

#### TypeScript

```ts
function maxDistance(position: number[], m: number): number {
    position.sort((a, b) => a - b);
    let [l, r] = [1, position.at(-1)!];
    const count = (f: number): number => {
        let cnt = 1;
        let prev = position[0];
        for (const curr of position) {
            if (curr - prev >= f) {
                cnt++;
                prev = curr;
            }
        }
        return cnt;
    };
    while (l < r) {
        const mid = (l + r + 1) >> 1;
        if (count(mid) >= m) {
            l = mid;
        } else {
            r = mid - 1;
        }
    }
    return l;
}
```

#### JavaScript

```js
/**
 * @param {number[]} position
 * @param {number} m
 * @return {number}
 */
var maxDistance = function (position, m) {
    position.sort((a, b) => a - b);
    let [l, r] = [1, position.at(-1)];
    const count = f => {
        let cnt = 1;
        let prev = position[0];
        for (const curr of position) {
            if (curr - prev >= f) {
                cnt++;
                prev = curr;
            }
        }
        return cnt;
    };
    while (l < r) {
        const mid = (l + r + 1) >> 1;
        if (count(mid) >= m) {
            l = mid;
        } else {
            r = mid - 1;
        }
    }
    return l;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
