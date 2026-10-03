---
comments: true
difficulty: Hard
rating: 2561
source: Weekly Contest 288 Q4
tags:
    - Greedy
    - Array
    - Two Pointers
    - Binary Search
    - Enumeration
    - Prefix Sum
    - Sorting
---

<!-- problem:start -->

# [2234. Maximum Total Beauty of the Gardens](https://leetcode.com/problems/maximum-total-beauty-of-the-gardens)

[中文文档](/solution/2200-2299/2234.Maximum%20Total%20Beauty%20of%20the%20Gardens/README.md)

## Mô tả

<!-- description:start -->

<p>Alice chăm sóc <code>n</code> khu vườn và muốn trồng hoa để tối đa hóa tổng vẻ đẹp của tất cả khu vườn.</p>

<p>Cho một mảng số nguyên <code>flowers</code> <strong>đánh chỉ số từ 0</strong> có kích thước <code>n</code>, trong đó <code>flowers[i]</code> là số lượng hoa đã được trồng trong khu vườn thứ <code>i<sup>th</sup></code>. Những bông hoa đã trồng <strong>không thể bị nhổ bỏ</strong>. Ngoài ra, cho số nguyên <code>newFlowers</code> là số lượng hoa <strong>tối đa</strong> mà Alice có thể trồng thêm. Bạn cũng được cho các số nguyên <code>target</code>, <code>full</code> và <code>partial</code>.</p>

<p>Một khu vườn được xem là <strong>hoàn thiện</strong> nếu có <strong>ít nhất</strong> <code>target</code> bông hoa. Khi đó, <strong>tổng vẻ đẹp</strong> của các khu vườn được xác định bằng <strong>tổng</strong> của:</p>

<ul>
	<li>Số khu vườn <strong>hoàn thiện</strong> nhân với <code>full</code>.</li>
	<li>Số lượng hoa <strong>ít nhất</strong> trong các khu vườn <strong>chưa hoàn thiện</strong> nhân với <code>partial</code>. Nếu không có khu vườn chưa hoàn thiện thì giá trị này bằng <code>0</code>.</li>
</ul>

<p>Hãy trả về <em><strong>tổng vẻ đẹp lớn nhất</strong> mà Alice có thể đạt được sau khi trồng nhiều nhất </em><code>newFlowers</code><em> bông hoa.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> flowers = [1,3,1,1], newFlowers = 7, target = 6, full = 12, partial = 1
<strong>Đầu ra:</strong> 14
<strong>Giải thích:</strong> Alice có thể trồng
- 2 bông hoa trong khu vườn thứ 0<sup>th</sup>
- 3 bông hoa trong khu vườn thứ 1<sup>st</sup>
- 1 bông hoa trong khu vườn thứ 2<sup>nd</sup>
- 1 bông hoa trong khu vườn thứ 3<sup>rd</sup>
Khi đó, các khu vườn sẽ là [3,6,2,2]. Tổng số hoa Alice đã trồng là 2 + 3 + 1 + 1 = 7.
Có 1 khu vườn hoàn thiện.
Số lượng hoa ít nhất trong các khu vườn chưa hoàn thiện là 2.
Vì vậy, tổng vẻ đẹp là 1 * 12 + 2 * 1 = 12 + 2 = 14.
Không có cách trồng hoa nào khác đạt được tổng vẻ đẹp lớn hơn 14.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> flowers = [2,4,5,3], newFlowers = 10, target = 5, full = 2, partial = 6
<strong>Đầu ra:</strong> 30
<strong>Giải thích:</strong> Alice có thể trồng
- 3 bông hoa trong khu vườn thứ 0<sup>th</sup>
- 0 bông hoa trong khu vườn thứ 1<sup>st</sup>
- 0 bông hoa trong khu vườn thứ 2<sup>nd</sup>
- 2 bông hoa trong khu vườn thứ 3<sup>rd</sup>
Khi đó, các khu vườn sẽ là [5,4,5,5]. Tổng số hoa Alice đã trồng là 3 + 0 + 0 + 2 = 5.
Có 3 khu vườn hoàn thiện.
Số lượng hoa ít nhất trong các khu vườn chưa hoàn thiện là 4.
Vì vậy, tổng vẻ đẹp là 3 * 2 + 4 * 6 = 6 + 24 = 30.
Không có cách trồng hoa nào khác đạt được tổng vẻ đẹp lớn hơn 30.
Lưu ý rằng Alice có thể làm cho tất cả khu vườn hoàn thiện, nhưng trong trường hợp đó, tổng vẻ đẹp sẽ thấp hơn.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= flowers.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= flowers[i], target &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= newFlowers &lt;= 10<sup>10</sup></code></li>
	<li><code>1 &lt;= full, partial &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Một khu vườn hoặc được trồng đủ đến $\textit{target}$ (hoàn thiện), hoặc chưa hoàn thiện; trong trường hợp sau, ta muốn số lượng hoa ít nhất của nó lớn nhất có thể. Vì $n \le 10^5$ và $newFlowers$ có thể lên tới $10^{10}$, sau khi cố định số khu vườn hoàn thiện, số hoa còn lại nên được dùng để tăng một prefix của các khu vườn chưa hoàn thiện.
>
> Hãy sắp xếp và tính tổng tiền tố. Liệt kê số khu vườn hoàn thiện cuối cùng là $x$, trả chi phí để hoàn thiện những khu vườn tốn nhiều hoa nhất, sau đó dùng tìm kiếm nhị phân để xác định prefix của các khu vườn chưa hoàn thiện có thể được tăng lên bao nhiêu với số hoa còn lại. Các bông hoa thêm vào được chia đều nhưng giá trị vẫn nhỏ hơn $\textit{target}$. Tối đa hóa $x\cdot\textit{full}+y\cdot\textit{partial}$.

<!-- thinking:end -->

Ta nhận thấy nếu số lượng hoa trong một khu vườn đã lớn hơn hoặc bằng $\textit{target}$ thì khu vườn đó đã hoàn thiện và không thể thay đổi. Với các khu vườn chưa hoàn thiện, ta có thể trồng thêm hoa để làm chúng hoàn thiện.

Tiếp theo, ta liệt kê số khu vườn cuối cùng sẽ hoàn thiện. Giả sử ban đầu có x khu vườn hoàn thiện, khi đó ta có thể liệt kê $x$ trong đoạn $[x, n]$. Nên chọn những khu vườn nào để làm chúng hoàn thiện? Thực tế, ta nên chọn các khu vườn có nhiều hoa hơn để số hoa còn lại có thể được dùng để tăng số lượng hoa ít nhất trong các khu vườn chưa hoàn thiện. Vì vậy, ta sắp xếp mảng $\textit{flowers}$.

Sau đó, ta liệt kê số khu vườn hoàn thiện $x$. Khu vườn hiện tại cần được làm hoàn thiện là $\textit{target}[n-x]$, và số hoa cần thêm là $\max(0, \textit{target} - \textit{flowers}[n - x])$.

Ta cập nhật số hoa còn lại $\textit{newFlowers}$. Nếu giá trị này nhỏ hơn $0$, nghĩa là ta không thể làm thêm khu vườn nào hoàn thiện, nên dừng việc liệt kê.

Nếu không, ta thực hiện tìm kiếm nhị phân trong đoạn $[0,..n-x-1]$ để tìm chỉ số lớn nhất của các khu vườn chưa hoàn thiện có thể được tăng lên. Gọi chỉ số đó là $l$, khi đó số hoa cần dùng là $\textit{cost} = \textit{flowers}[l] \times (l + 1) - s[l + 1]$, trong đó $s[i]$ là tổng của $i$ phần tử đầu tiên trong mảng $\textit{flowers}$. Nếu vẫn có thể tăng số lượng hoa ít nhất, ta tính mức tăng $\frac{\textit{newFlowers} - \textit{cost}}{l + 1}$ và đảm bảo giá trị nhỏ nhất cuối cùng không vượt quá $\textit{target}-1$. Khi đó, giá trị nhỏ nhất $y = \min(\textit{flowers}[l] + \frac{\textit{newFlowers} - \textit{cost}}{l + 1}, \textit{target} - 1)$. Tổng vẻ đẹp của các khu vườn là $x \times \textit{full} + y \times \textit{partial}$. Đáp án là giá trị lớn nhất trong tất cả các tổng vẻ đẹp.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $\textit{flowers}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumBeauty(
        self, flowers: List[int], newFlowers: int, target: int, full: int, partial: int
    ) -> int:
        flowers.sort()
        n = len(flowers)
        s = list(accumulate(flowers, initial=0))
        ans, i = 0, n - bisect_left(flowers, target)
        for x in range(i, n + 1):
            newFlowers -= 0 if x == 0 else max(target - flowers[n - x], 0)
            if newFlowers < 0:
                break
            l, r = 0, n - x - 1
            while l < r:
                mid = (l + r + 1) >> 1
                if flowers[mid] * (mid + 1) - s[mid + 1] <= newFlowers:
                    l = mid
                else:
                    r = mid - 1
            y = 0
            if r != -1:
                cost = flowers[l] * (l + 1) - s[l + 1]
                y = min(flowers[l] + (newFlowers - cost) // (l + 1), target - 1)
            ans = max(ans, x * full + y * partial)
        return ans
```

#### Java

```java
class Solution {
    public long maximumBeauty(int[] flowers, long newFlowers, int target, int full, int partial) {
        Arrays.sort(flowers);
        int n = flowers.length;
        long[] s = new long[n + 1];
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + flowers[i];
        }
        long ans = 0;
        int x = 0;
        for (int v : flowers) {
            if (v >= target) {
                ++x;
            }
        }
        for (; x <= n; ++x) {
            newFlowers -= (x == 0 ? 0 : Math.max(target - flowers[n - x], 0));
            if (newFlowers < 0) {
                break;
            }
            int l = 0, r = n - x - 1;
            while (l < r) {
                int mid = (l + r + 1) >> 1;
                if ((long) flowers[mid] * (mid + 1) - s[mid + 1] <= newFlowers) {
                    l = mid;
                } else {
                    r = mid - 1;
                }
            }
            long y = 0;
            if (r != -1) {
                long cost = (long) flowers[l] * (l + 1) - s[l + 1];
                y = Math.min(flowers[l] + (newFlowers - cost) / (l + 1), target - 1);
            }
            ans = Math.max(ans, (long) x * full + y * partial);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maximumBeauty(vector<int>& flowers, long long newFlowers, int target, int full, int partial) {
        sort(flowers.begin(), flowers.end());
        int n = flowers.size();
        long long s[n + 1];
        s[0] = 0;
        for (int i = 1; i <= n; ++i) {
            s[i] = s[i - 1] + flowers[i - 1];
        }
        long long ans = 0;
        int i = flowers.end() - lower_bound(flowers.begin(), flowers.end(), target);
        for (int x = i; x <= n; ++x) {
            newFlowers -= (x == 0 ? 0 : max(target - flowers[n - x], 0));
            if (newFlowers < 0) {
                break;
            }
            int l = 0, r = n - x - 1;
            while (l < r) {
                int mid = (l + r + 1) >> 1;
                if (1LL * flowers[mid] * (mid + 1) - s[mid + 1] <= newFlowers) {
                    l = mid;
                } else {
                    r = mid - 1;
                }
            }
            int y = 0;
            if (r != -1) {
                long long cost = 1LL * flowers[l] * (l + 1) - s[l + 1];
                long long mx = flowers[l] + (newFlowers - cost) / (l + 1);
                long long threshold = target - 1;
                y = min(mx, threshold);
            }
            ans = max(ans, 1LL * x * full + 1LL * y * partial);
        }
        return ans;
    }
};
```

#### Go

```go
func maximumBeauty(flowers []int, newFlowers int64, target int, full int, partial int) int64 {
	sort.Ints(flowers)
	n := len(flowers)
	s := make([]int, n+1)
	for i, x := range flowers {
		s[i+1] = s[i] + x
	}
	ans := 0
	i := n - sort.SearchInts(flowers, target)
	for x := i; x <= n; x++ {
		if x > 0 {
			newFlowers -= int64(max(target-flowers[n-x], 0))
		}
		if newFlowers < 0 {
			break
		}
		l, r := 0, n-x-1
		for l < r {
			mid := (l + r + 1) >> 1
			if int64(flowers[mid]*(mid+1)-s[mid+1]) <= newFlowers {
				l = mid
			} else {
				r = mid - 1
			}
		}
		y := 0
		if r != -1 {
			cost := flowers[l]*(l+1) - s[l+1]
			y = min(flowers[l]+int((newFlowers-int64(cost))/int64(l+1)), target-1)
		}
		ans = max(ans, x*full+y*partial)
	}
	return int64(ans)
}
```

#### TypeScript

```ts
function maximumBeauty(
    flowers: number[],
    newFlowers: number,
    target: number,
    full: number,
    partial: number,
): number {
    flowers.sort((a, b) => a - b);
    const n = flowers.length;
    const s: number[] = Array(n + 1).fill(0);
    for (let i = 1; i <= n; i++) {
        s[i] = s[i - 1] + flowers[i - 1];
    }
    let x = flowers.filter(f => f >= target).length;
    let ans = 0;
    for (; x <= n; ++x) {
        newFlowers -= x === 0 ? 0 : Math.max(target - flowers[n - x], 0);
        if (newFlowers < 0) {
            break;
        }
        let l = 0;
        let r = n - x - 1;
        while (l < r) {
            const mid = (l + r + 1) >> 1;
            if (flowers[mid] * (mid + 1) - s[mid + 1] <= newFlowers) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        let y = 0;
        if (r !== -1) {
            const cost = flowers[l] * (l + 1) - s[l + 1];
            y = Math.min(flowers[l] + Math.floor((newFlowers - cost) / (l + 1)), target - 1);
        }
        ans = Math.max(ans, x * full + y * partial);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
