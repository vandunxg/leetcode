---
comments: true
difficulty: Hard
---

<!-- problem:start -->

# [4068. Maximize Meeting Earnings with Idle Gaps](https://leetcode.com/problems/maximize-meeting-earnings-with-idle-gaps)

[中文文档](/solution/4000-4099/4068.Maximize%20Meeting%20Earnings%20with%20Idle%20Gaps/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một mảng số nguyên 2 chiều <code>meetings</code>, trong đó <code>meetings[i] = [start<sub>i</sub>, end<sub>i</sub>, revenue<sub>i</sub>]</code> biểu diễn một cuộc họp bắt đầu tại thời điểm <code>start<sub>i</sub></code>, kết thúc tại thời điểm <code>end<sub>i</sub></code> và có doanh thu <code>revenue<sub>i</sub></code>.</p>

<p>Tất cả các cuộc họp đều sử dụng <strong>khoảng nửa kín</strong> <code>[start, end)</code>, vì vậy các cuộc họp chỉ chạm nhau tại điểm cuối thì <strong>không</strong> chồng lấn.</p>

<p>Bạn có thể chọn bất kỳ <strong>tập con không rỗng</strong> nào của các cuộc họp sao cho không có hai cuộc họp được chọn nào chồng lấn. Bạn nhận được doanh thu của mỗi cuộc họp được chọn.</p>

<p>Sắp xếp các cuộc họp được chọn theo <strong>thứ tự tăng dần của thời gian bắt đầu</strong>. Với mỗi cặp cuộc họp liền kề theo thứ tự này, bạn cũng nhận được 1 đơn vị doanh thu cho mỗi đơn vị thời gian rảnh giữa chúng. Thời gian rảnh này bằng thời điểm bắt đầu của cuộc họp đến sau trừ đi thời điểm kết thúc của cuộc họp trước đó.</p>

<p>Không có doanh thu từ thời gian rảnh trước khi cuộc họp được chọn sớm nhất bắt đầu hoặc sau khi cuộc họp được chọn muộn nhất kết thúc. Nếu chỉ chọn một cuộc họp thì không có doanh thu từ thời gian rảnh.</p>

<p>Trả về <strong>tổng thu nhập lớn nhất</strong> có thể đạt được.</p>

<p><strong>Tập con</strong> của một mảng là một lựa chọn các phần tử của mảng đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">meetings = [[2,5,4],[6,8,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn cả hai cuộc họp. Chúng không chồng lấn và mang lại <code>4 + 3 = 7</code> đơn vị doanh thu từ cuộc họp.</li>
	<li>Cuộc họp đầu tiên kết thúc tại thời điểm 5, còn cuộc họp thứ hai bắt đầu tại thời điểm 6. Khoảng thời gian rảnh này mang lại thêm <code>6 - 5 = 1</code> đơn vị.</li>
	<li>Tổng thu nhập lớn nhất là <code>7 + 1 = 8</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">meetings = [[3,5,4],[4,7,8],[8,10,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn các cuộc họp tại chỉ số 1 và 2. Chúng không chồng lấn và mang lại <code>8 + 3 = 11</code> đơn vị doanh thu từ cuộc họp.</li>
	<li>Theo thứ tự thời gian, các cuộc họp này diễn ra từ thời điểm 4 đến 7 và từ thời điểm 8 đến 10. Khoảng thời gian rảnh mang lại thêm <code>8 - 7 = 1</code> đơn vị.</li>
	<li>Tổng thu nhập lớn nhất là <code>11 + 1 = 12</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">meetings = [[1,2,2],[4,5,2],[7,9,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">11</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn cả ba cuộc họp. Chúng không chồng lấn và mang lại <code>2 + 2 + 3 = 7</code> đơn vị doanh thu từ cuộc họp.</li>
	<li>Khoảng thời gian rảnh từ thời điểm 2 đến 4 mang lại thêm <code>4 - 2 = 2</code> đơn vị.</li>
	<li>Khoảng thời gian rảnh từ thời điểm 5 đến 7 mang lại thêm <code>7 - 5 = 2</code> đơn vị.</li>
	<li>Tổng thu nhập lớn nhất là <code>7 + 2 + 2 = 11</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<h3><strong>Ràng buộc</strong></h3>

<ul>
	<li><code>1 &lt;= meetings.length &lt;= 10<sup>5</sup></code></li>
	<li><code>meetings[i] = [start<sub>i</sub>, end<sub>i</sub>, revenue<sub>i</sub>]</code></li>
	<li><code>0 &lt;= start<sub>i</sub> &lt; end<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= revenue<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp, tìm kiếm nhị phân và DP

<!-- thinking:start -->

> **Tư duy**
>
> Có thể có tới $10^5$ cuộc họp, nên không thể duyệt qua tất cả các tập con. Thu nhập gồm hai phần: doanh thu của mỗi cuộc họp được chọn và khoảng thời gian rảnh giữa các cuộc họp liền kề sau khi sắp xếp theo thời gian bắt đầu. Một cuộc họp đứng riêng không mang lại doanh thu từ thời gian rảnh.
>
> Cố định một cuộc họp làm cuộc họp cuối cùng của một lịch. Mọi cuộc họp được xếp trước nó phải kết thúc không muộn hơn thời điểm bắt đầu của nó, và khoảng thời gian rảnh mới bằng $\textit{start}$ trừ đi thời điểm kết thúc trước đó. Vì vậy, đại lượng cần tối ưu là thu nhập tốt nhất của một lịch kết thúc tại một cuộc họp, trừ đi thời điểm kết thúc của cuộc họp đó.
>
> Sau khi sắp xếp theo thời gian kết thúc, các cuộc họp có thể đứng trước cuộc họp hiện tại tạo thành một tiền tố, và có thể tìm giá trị lớn nhất của tiền tố bằng tìm kiếm nhị phân. Doanh thu và khoảng thời gian rảnh được cộng dồn bằng các số nguyên $64$-bit.

<!-- thinking:end -->

Sắp xếp các cuộc họp theo thời gian kết thúc tăng dần. Gọi $f[i]$ là thu nhập lớn nhất của một lịch mà cuộc họp cuối cùng là cuộc họp $i$. Mọi lịch không rỗng đều có một cuộc họp kết thúc sau cùng, nên đáp án là giá trị lớn nhất trong tất cả $f[i]$.

Chọn riêng cuộc họp $i$ cho ta $f[i]=\textit{revenue}_i$. Nếu một cuộc họp $j$ với $\textit{end}_j\le\textit{start}_i$ được đặt trước nó, thì

$$
f[i]=f[j]+\textit{revenue}_i+(\textit{start}_i-\textit{end}_j),
$$

biểu thức này có thể biến đổi thành

$$
f[i]=\textit{revenue}_i+\textit{start}_i+\max_j(f[j]-\textit{end}_j).
$$

Sau khi sắp xếp, các chỉ số $j$ đó tạo thành một tiền tố. Gọi

$$
\textit{preMax}[k]=\max_{j<k}(f[j]-\textit{end}_j),
$$

dùng một giá trị sentinel khi chưa có cuộc họp nào được xử lý. Khi xử lý cuộc họp $i$, tìm nhị phân chỉ số đầu tiên $p$ trong $[0,i)$ có thời gian kết thúc lớn hơn $\textit{start}_i$. Khi đó, $\textit{preMax}[p]$ là giá trị lớn nhất cần tìm ở trên. Nếu $\textit{start}_i$ nhỏ hơn thời gian kết thúc sớm nhất, tiền tố là rỗng và cuộc họp $i$ phải được chọn riêng.

Sau đó, đặt $\textit{preMax}[i+1]=\max(\textit{preMax}[i], f[i]-\textit{end}_i)$. Hai cuộc họp có cùng thời gian kết thúc sẽ chồng lấn, nên tìm kiếm nhị phân không nối chúng thành một chuỗi.

Độ phức tạp thời gian là $O(n\log n)$ và độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxEarnings(self, meetings: list[list[int]]) -> int:
        n = len(meetings)
        meetings.sort(key=lambda x: x[1])

        pre_max = [-inf] * (n + 1)
        ans = 0

        for i, (start, end, revenue) in enumerate(meetings):
            val = revenue
            if start >= meetings[0][1]:
                j = bisect_right(meetings, start, hi=i, key=lambda x: x[1])
                val += pre_max[j] + start

            ans = max(ans, val)
            pre_max[i + 1] = max(pre_max[i], val - end)

        return ans
```

#### Java

```java
class Solution {
    public long maxEarnings(int[][] meetings) {
        int n = meetings.length;
        Arrays.sort(meetings, (a, b) -> a[1] - b[1]);

        long[] preMax = new long[n + 1];
        Arrays.fill(preMax, Long.MIN_VALUE / 2);

        long ans = 0;

        for (int i = 0; i < n; i++) {
            int start = meetings[i][0];
            int end = meetings[i][1];
            int revenue = meetings[i][2];

            long val = revenue;
            if (start >= meetings[0][1]) {
                int j = upperBound(meetings, start, i);
                val += preMax[j] + start;
            }

            ans = Math.max(ans, val);
            preMax[i + 1] = Math.max(preMax[i], val - end);
        }

        return ans;
    }

    private int upperBound(int[][] meetings, int target, int hi) {
        int l = 0, r = hi;
        while (l < r) {
            int m = (l + r) >>> 1;
            if (meetings[m][1] <= target) {
                l = m + 1;
            } else {
                r = m;
            }
        }
        return l;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxEarnings(vector<vector<int>>& meetings) {
        int n = meetings.size();
        sort(meetings.begin(), meetings.end(), [](auto& a, auto& b) {
            return a[1] < b[1];
        });

        vector<long long> preMax(n + 1, LLONG_MIN / 2);
        long long ans = 0;

        for (int i = 0; i < n; i++) {
            int start = meetings[i][0];
            int end = meetings[i][1];
            int revenue = meetings[i][2];

            long long val = revenue;
            if (start >= meetings[0][1]) {
                int j = upper_bound(meetings.begin(), meetings.begin() + i, start,
                            [](int x, const vector<int>& y) {
                                return x < y[1];
                            })
                    - meetings.begin();
                val += preMax[j] + start;
            }

            ans = max(ans, val);
            preMax[i + 1] = max(preMax[i], val - end);
        }

        return ans;
    }
};
```

#### Go

```go
func maxEarnings(meetings [][]int) int64 {
	n := len(meetings)
	sort.Slice(meetings, func(i, j int) bool {
		return meetings[i][1] < meetings[j][1]
	})

	preMax := make([]int64, n+1)
	for i := range preMax {
		preMax[i] = -1 << 60
	}

	var ans int64

	for i, meeting := range meetings {
		start, end, revenue := meeting[0], meeting[1], meeting[2]

		val := int64(revenue)
		if start >= meetings[0][1] {
			j := sort.Search(i, func(j int) bool {
				return meetings[j][1] > start
			})
			val += preMax[j] + int64(start)
		}

		ans = max(ans, val)
		preMax[i+1] = max(preMax[i], val-int64(end))
	}

	return ans
}
```

#### TypeScript

```ts
function maxEarnings(meetings: number[][]): number {
    const n = meetings.length;
    meetings.sort((a, b) => a[1] - b[1]);

    const preMax = new Array<number>(n + 1).fill(-Infinity);
    let ans = 0;

    for (let i = 0; i < n; i++) {
        const start = meetings[i][0];
        const end = meetings[i][1];
        const revenue = meetings[i][2];

        let val = revenue;
        if (start >= meetings[0][1]) {
            let l = 0;
            let r = i;
            while (l < r) {
                const m = (l + r) >> 1;
                if (meetings[m][1] <= start) {
                    l = m + 1;
                } else {
                    r = m;
                }
            }
            val += preMax[l] + start;
        }

        ans = Math.max(ans, val);
        preMax[i + 1] = Math.max(preMax[i], val - end);
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
