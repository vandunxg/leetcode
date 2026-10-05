---
comments: true
difficulty: Medium
rating: 1529
source: Biweekly Contest 184 Q2
tags:
    - Array
    - Sorting
---

<!-- problem:start -->

# [3951. Minimum Energy to Maintain Brightness](https://leetcode.com/problems/minimum-energy-to-maintain-brightness)

[Tài liệu tiếng Trung](/solution/3900-3999/3951.Minimum%20Energy%20to%20Maintain%20Brightness/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code>, biểu thị <code>n</code> bóng đèn được xếp thành một hàng và đánh số từ 0 đến <code>n - 1</code>.</p>

<p>Cho thêm một số nguyên <code>brightness</code> và một mảng số nguyên 2 chiều <code>intervals</code>, trong đó <code>intervals[i] = [start<sub>i</sub>, end<sub>i</sub>]</code> biểu thị một khoảng thời gian <strong>bao gồm cả hai đầu mút</strong> mà trong đó yêu cầu chiếu sáng <strong>bắt buộc</strong> phải được đáp ứng.</p>

<p>Ở mỗi đơn vị thời gian, mỗi bóng đèn có thể độc lập bật hoặc tắt. Một bóng đèn đang bật <strong>chiếu sáng</strong> vị trí của nó và các vị trí <strong>kề</strong> với nó, nếu có.</p>

<p><strong>Tổng độ sáng</strong> tại một đơn vị thời gian là số vị trí được <strong>chiếu sáng</strong>. Mỗi vị trí được tính <strong>nhiều nhất một lần</strong>.</p>

<p>Với mọi đơn vị thời gian nguyên nằm trong <strong>ít nhất</strong> một khoảng của <code>intervals</code>, <strong>tổng độ sáng</strong> phải <strong>ít nhất</strong> là <code>brightness</code>. Tại những đơn vị thời gian không nằm trong khoảng nào, tất cả bóng đèn có thể tắt. Mỗi bóng đèn đang bật tiêu thụ 1 đơn vị năng lượng trong đơn vị thời gian đó.</p>

<p>Trả về một số nguyên biểu thị <strong>tổng năng lượng nhỏ nhất</strong> cần thiết.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, brightness = 5, intervals = [[6,12]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">14</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Bật các bóng đèn ở vị trí 1 và 4.</li>
	<li>Trạng thái hiện tại của hàng: <code>0 1 0 0 1</code>.</li>
	<li>Cả 5 vị trí đều được chiếu sáng, nên đạt yêu cầu về độ sáng.</li>
	<li>Khoảng hoạt động có độ dài <code>12 - 6 + 1 = 7</code>, nên tổng năng lượng là <code>2 * 7 = 14</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2, brightness = 1, intervals = [[0,0],[2,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Bật một bóng đèn trong mỗi khoảng hoạt động.</li>
	<li>Mỗi khoảng có độ dài 1, nên tổng thời gian hoạt động là <code>1 + 1 = 2</code>.</li>
	<li>Tổng năng lượng là <code>1 * 2 = 2</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, brightness = 2, intervals = [[1,3],[2,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Bật một bóng đèn. Nó có thể chiếu sáng ít nhất 2 vị trí.</li>
	<li>Các khoảng hoạt động giao nhau, nên tổng thời gian hoạt động là độ dài của <code>[1,4]</code>, tức là 4.</li>
	<li>Tổng năng lượng là <code>1 * 4 = 4</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>6</sup></code></li>
	<li><code>1 &lt;= brightness &lt;= n</code></li>
	<li><code>1 &lt;= intervals.length &lt;= 10<sup>5</sup></code></li>
	<li><code>intervals[i] == [start<sub>i</sub>, end<sub>i</sub>]</code></li>
	<li><code>0 &lt;= start<sub>i</sub> &lt;= end<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Gộp khoảng

<!-- thinking:start -->

> **Tư duy**
>
> Tọa độ có thể lên tới $10^9$, nên ta không thể mở rộng qua từng điểm. Mỗi điểm được phủ cần $\lceil\textit{brightness}/3\rceil$ năng lượng, và các khoảng giao nhau có thể dùng chung một điểm, vì vậy trước tiên cần gộp các khoảng.
>
> Sắp xếp rồi gộp các đoạn giao nhau hoặc chạm nhau thành những đoạn rời nhau, sau đó nhân độ dài mỗi đoạn với năng lượng cần cho một điểm. Độ phức tạp phụ thuộc vào số khoảng, không phụ thuộc vào độ dài số học của con đường.

<!-- thinking:end -->

Một bóng đèn có thể chiếu sáng nhiều nhất 3 vị trí. Để đảm bảo tổng độ sáng ít nhất là $\textit{brightness}$, số bóng đèn cần bật là $\lceil \frac{\textit{brightness}}{3} \rceil$. Trong lập trình, biểu thức này thường được viết dưới dạng phép chia nguyên là `(brightness + 2) / 3`.

Bài toán có thể được giải theo các bước sau:

1. **Gộp các khoảng giao nhau**: Gộp tất cả các khoảng giao nhau để thu được một tập các khoảng liên tục, rời nhau từng đôi một.
2. **Tính đóng góp theo độ dài**: Với mỗi khoảng đã gộp $[start, end]$, số điểm nguyên (tức là số vị trí) mà nó bao phủ là $m = end - start + 1$. Vì mọi vị trí trong khoảng đều phải đáp ứng độ sáng tối thiểu, tổng năng lượng cần cho khoảng này là:
   $$\text{Energy} = \lceil \frac{\textit{brightness}}{3} \rceil \times m$$
3. **Cộng dồn**: Cộng năng lượng của tất cả các khoảng rời nhau để thu được đáp án cuối cùng $\textit{ans}$.

Độ phức tạp thời gian là $O(n \log n)$, độ phức tạp không gian là $O(n)$, trong đó $n$ là số khoảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minEnergy(self, n: int, brightness: int, intervals: list[list[int]]) -> int:
        intervals.sort()
        merged = [intervals[0]]
        for x in intervals[1:]:
            if merged[-1][1] < x[0]:
                merged.append(x)
            else:
                merged[-1][1] = max(merged[-1][1], x[1])
        ans = 0
        for start, end in merged:
            m = end - start + 1
            ans += (brightness + 2) // 3 * m
        return ans
```

#### Java

```java
class Solution {
    public long minEnergy(int n, int brightness, int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
        List<int[]> merged = new ArrayList<>();
        merged.add(intervals[0]);
        for (int i = 1; i < intervals.length; i++) {
            int[] x = intervals[i];
            int[] last = merged.get(merged.size() - 1);
            if (last[1] < x[0]) {
                merged.add(x);
            } else {
                last[1] = Math.max(last[1], x[1]);
            }
        }
        long ans = 0;
        for (int[] interval : merged) {
            int start = interval[0];
            int end = interval[1];
            int m = end - start + 1;
            ans += (brightness + 2L) / 3 * m;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minEnergy(int n, int brightness, vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end());
        vector<vector<int>> merged = {intervals[0]};
        for (int i = 1; i < intervals.size(); ++i) {
            auto& x = intervals[i];
            if (merged.back()[1] < x[0]) {
                merged.push_back(x);
            } else {
                merged.back()[1] = max(merged.back()[1], x[1]);
            }
        }
        long long ans = 0;
        for (const auto& interval : merged) {
            int start = interval[0];
            int end = interval[1];
            int m = end - start + 1;
            ans += (brightness + 2LL) / 3 * m;
        }
        return ans;
    }
};
```

#### Go

```go
func minEnergy(n int, brightness int, intervals [][]int) int64 {
	sort.Slice(intervals, func(i, j int) bool {
		return intervals[i][0] < intervals[j][0]
	})
	merged := [][]int{intervals[0]}
	for _, x := range intervals[1:] {
		if merged[len(merged)-1][1] < x[0] {
			merged = append(merged, x)
		} else {
			if x[1] > merged[len(merged)-1][1] {
				merged[len(merged)-1][1] = x[1]
			}
		}
	}
	ans := 0
	for _, interval := range merged {
		start := interval[0]
		end := interval[1]
		m := end - start + 1
		ans += (brightness + 2) / 3 * m
	}
	return int64(ans)
}
```

#### TypeScript

```ts
function minEnergy(n: number, brightness: number, intervals: number[][]): number {
    intervals.sort((a, b) => a[0] - b[0]);
    const merged: number[][] = [intervals[0]];
    for (let i = 1; i < intervals.length; i++) {
        const x = intervals[i];
        if (merged[merged.length - 1][1] < x[0]) {
            merged.push(x);
        } else {
            merged[merged.length - 1][1] = Math.max(merged[merged.length - 1][1], x[1]);
        }
    }
    let ans = 0;
    for (const [start, end] of merged) {
        const m = end - start + 1;
        ans += Math.ceil(brightness / 3) * m;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
