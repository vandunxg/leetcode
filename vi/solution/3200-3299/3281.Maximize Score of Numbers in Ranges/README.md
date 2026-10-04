---
comments: true
difficulty: Medium
rating: 1768
source: Weekly Contest 414 Q2
tags:
    - Greedy
    - Array
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [3281. Maximize Score of Numbers in Ranges](https://leetcode.com/problems/maximize-score-of-numbers-in-ranges)

[中文文档](/solution/3200-3299/3281.Maximize%20Score%20of%20Numbers%20in%20Ranges/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>start</code> và một số nguyên <code>d</code>, biểu diễn <code>n</code> khoảng <code>[start[i], start[i] + d]</code>.</p>

<p>Hãy chọn <code>n</code> số nguyên sao cho số nguyên <code>i<sup>th</sup></code> phải thuộc khoảng <code>i<sup>th</sup></code>. <strong>Điểm số</strong> của các số nguyên được chọn được định nghĩa là <strong>giá trị nhỏ nhất</strong> của hiệu tuyệt đối giữa bất kỳ hai số nguyên nào được chọn.</p>

<p>Trả về <strong>điểm số</strong> <em>lớn nhất có thể</em> của các số nguyên được chọn.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">start = [6,0,3], d = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có thể đạt được điểm số lớn nhất bằng cách chọn các số nguyên: 8, 0 và 4. Điểm số của các số nguyên được chọn là <code>min(|8 - 0|, |8 - 4|, |0 - 4|)</code>, bằng 4.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">start = [2,6,13,13], d = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có thể đạt được điểm số lớn nhất bằng cách chọn các số nguyên: 2, 7, 13 và 18. Điểm số của các số nguyên được chọn là <code>min(|2 - 7|, |2 - 13|, |2 - 18|, |7 - 13|, |7 - 18|, |13 - 18|)</code>, bằng 5.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= start.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= start[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= d &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Chọn một số nguyên trong mỗi $[start_i,start_i+d]$ để tối đa hóa khoảng cách nhỏ nhất giữa các số được chọn liên tiếp. Với $n\le 10^5$, không thể duyệt hết các giá trị. Sau khi sắp xếp, khoảng cách có tính đơn điệu: nếu $x$ thỏa mãn, thì mọi khoảng cách nhỏ hơn đều thỏa mãn.
>
> Tìm kiếm nhị phân $x$ và duyệt từ trái sang phải, chọn điểm sớm nhất không nhỏ hơn $last+x$ trong mỗi khoảng. Giá trị $x$ lớn nhất khả thi là đáp án.

<!-- thinking:end -->

Trước tiên, ta có thể sắp xếp mảng $\textit{start}$. Sau đó, ta xét việc chọn các số nguyên từ trái sang phải, trong đó điểm số bằng hiệu nhỏ nhất giữa hai số nguyên được chọn liền kề.

Nếu một hiệu $x$ thỏa mãn điều kiện, thì mọi $x' < x$ cũng sẽ thỏa mãn điều kiện. Vì vậy, ta có thể dùng tìm kiếm nhị phân để tìm hiệu lớn nhất thỏa mãn điều kiện.

Ta đặt biên trái của tìm kiếm nhị phân là $l = 0$ và biên phải là $r = \textit{start}[-1] + d - \textit{start}[0]$. Mỗi lần, ta lấy giá trị giữa $mid = \left\lfloor \frac{l + r + 1}{2} \right\rfloor$ và kiểm tra xem nó có thỏa mãn điều kiện hay không.

Ta định nghĩa hàm $\text{check}(mi)$ để xác định điều kiện có được thỏa mãn hay không, với cách triển khai như sau:

- Ta định nghĩa biến $\textit{last} = -\infty$, biểu diễn số nguyên được chọn gần nhất.
- Ta duyệt mảng $\textit{start}$. Nếu $\textit{last} + \textit{mi} > \textit{st} + d$, nghĩa là ta không thể chọn số nguyên $\textit{st}$, nên trả về $\text{false}$. Ngược lại, ta cập nhật $\textit{last} = \max(\textit{st}, \textit{last} + \textit{mi})$.
- Nếu duyệt hết mảng $\textit{start}$ và mọi điều kiện đều được thỏa mãn, ta trả về $\text{true}$.

Độ phức tạp thời gian là $O(n \times \log M)$, trong đó $n$ và $M$ lần lượt là độ dài và giá trị lớn nhất của mảng $\textit{start}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxPossibleScore(self, start: List[int], d: int) -> int:
        def check(mi: int) -> bool:
            last = -inf
            for st in start:
                if last + mi > st + d:
                    return False
                last = max(st, last + mi)
            return True

        start.sort()
        l, r = 0, start[-1] + d - start[0]
        while l < r:
            mid = (l + r + 1) >> 1
            if check(mid):
                l = mid
            else:
                r = mid - 1
        return l
```

#### Java

```java
class Solution {
    private int[] start;
    private int d;

    public int maxPossibleScore(int[] start, int d) {
        Arrays.sort(start);
        this.start = start;
        this.d = d;
        int n = start.length;
        int l = 0, r = start[n - 1] + d - start[0];
        while (l < r) {
            int mid = (l + r + 1) >>> 1;
            if (check(mid)) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return l;
    }

    private boolean check(int mi) {
        long last = Long.MIN_VALUE;
        for (int st : start) {
            if (last + mi > st + d) {
                return false;
            }
            last = Math.max(st, last + mi);
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxPossibleScore(vector<int>& start, int d) {
        ranges::sort(start);
        auto check = [&](int mi) -> bool {
            long long last = LLONG_MIN;
            for (int st : start) {
                if (last + mi > st + d) {
                    return false;
                }
                last = max((long long) st, last + mi);
            }
            return true;
        };
        int l = 0, r = start.back() + d - start[0];
        while (l < r) {
            int mid = l + (r - l + 1) / 2;
            if (check(mid)) {
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
func maxPossibleScore(start []int, d int) int {
	check := func(mi int) bool {
		last := math.MinInt64
		for _, st := range start {
			if last+mi > st+d {
				return false
			}
			last = max(st, last+mi)
		}
		return true
	}
	sort.Ints(start)
	l, r := 0, start[len(start)-1]+d-start[0]
	for l < r {
		mid := (l + r + 1) >> 1
		if check(mid) {
			l = mid
		} else {
			r = mid - 1
		}
	}
	return l
}
```

#### TypeScript

```ts
function maxPossibleScore(start: number[], d: number): number {
    start.sort((a, b) => a - b);
    let [l, r] = [0, start.at(-1)! + d - start[0]];
    const check = (mi: number): boolean => {
        let last = -Infinity;
        for (const st of start) {
            if (last + mi > st + d) {
                return false;
            }
            last = Math.max(st, last + mi);
        }
        return true;
    };
    while (l < r) {
        const mid = l + ((r - l + 1) >> 1);
        if (check(mid)) {
            l = mid;
        } else {
            r = mid - 1;
        }
    }
    return l;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
