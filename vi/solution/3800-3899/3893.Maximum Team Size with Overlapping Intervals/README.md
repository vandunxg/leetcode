---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [3893. Maximum Team Size with Overlapping Intervals 🔒](https://leetcode.com/problems/maximum-team-size-with-overlapping-intervals)

[中文文档](/solution/3800-3899/3893.Maximum%20Team%20Size%20with%20Overlapping%20Intervals/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng số nguyên <code>startTime</code> và <code>endTime</code> có cùng độ dài <code>n</code>.</p>

<ul>
	<li><code>startTime[i]</code> biểu thị thời điểm bắt đầu của nhân viên thứ <code>i<sup>th</sup></code>.</li>
	<li><code>endTime[i]</code> biểu thị thời điểm kết thúc của nhân viên thứ <code>i<sup>th</sup></code>.</li>
</ul>

<p>Hai nhân viên <code>i</code> và <code>j</code> có thể tương tác nếu các khoảng thời gian của họ <strong>giao nhau</strong>. Hai khoảng được xem là giao nhau nếu chúng có chung <strong>ít nhất một</strong> thời điểm.</p>

<p>Một team là <strong>hợp lệ</strong> nếu tồn tại <strong>ít nhất một</strong> nhân viên trong team có thể tương tác với mọi thành viên khác.</p>

<p>Hãy trả về một số nguyên biểu thị kích thước <strong>lớn nhất</strong> có thể của một team như vậy.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">startTime = [1,2,3], endTime = [4,5,6]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Với <code>i = 0</code>, khoảng thời gian là <code>[1, 4]</code>.</li>
	<li>Nó giao với <code>i = 1</code> có khoảng thời gian <code>[2, 5]</code> và <code>i = 2</code> có khoảng thời gian <code>[3, 6]</code>.</li>
	<li>Do đó, chỉ số 0 có thể tương tác với mọi chỉ số khác, nên kích thước team là 3.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">startTime = [2,5,8], endTime = [3,7,9]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Với <code>i = 0</code>, khoảng thời gian <code>[2, 3]</code> không giao với <code>[5, 7]</code> hoặc <code>[8, 9]</code>.</li>
	<li>Với <code>i = 1</code>, khoảng thời gian <code>[5, 7]</code> không giao với <code>[2, 3]</code> hoặc <code>[8, 9]</code>.</li>
	<li>Với <code>i = 2</code>, khoảng thời gian <code>[8, 9]</code> không giao với <code>[2, 3]</code> hoặc <code>[5, 7]</code>.</li>
	<li>Do đó, không có chỉ số nào có thể tương tác với các chỉ số khác, nên kích thước team lớn nhất là 1.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">startTime = [3,4,6], endTime = [8,5,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Với <code>i = 0</code>, khoảng thời gian là <code>[3, 8]</code>.</li>
	<li>Nó giao với <code>i = 1</code> có khoảng thời gian <code>[4, 5]</code> và <code>i = 2</code> có khoảng thời gian <code>[6, 7]</code>.</li>
	<li>Do đó, chỉ số 0 có thể tương tác với mọi chỉ số khác, nên kích thước team là 3.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == startTime.length == endTime.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= startTime[i] &lt;= endTime[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Một team hợp lệ có một thành viên giao với mọi thành viên khác. Ta muốn tìm team lớn nhất như vậy. $n \le 10^5$.
>
> Những người giao với $i$ tạo thành một team có tâm tại $i$, với kích thước bằng số khoảng thời gian giao với $i$ (bao gồm cả $i$).
>
> Sắp xếp tất cả điểm đầu trái và phải. Với $[l,r]$, dùng tìm kiếm nhị phân để đếm có bao nhiêu khoảng kết thúc trước $l$ và bao nhiêu khoảng bắt đầu sau $r$; phần chênh lệch là số khoảng giao nhau.
>
> Lấy giá trị lớn nhất trên tất cả nhân viên. Team tối ưu luôn có một tâm như vậy.

<!-- thinking:end -->

Trước tiên, ta gộp thời điểm bắt đầu và kết thúc của mỗi nhân viên thành một mảng khoảng, $\textit{intervals}$, đồng thời sắp xếp riêng tất cả thời điểm bắt đầu và thời điểm kết thúc.

Với mỗi nhân viên $i$, ta dùng tìm kiếm nhị phân để tính số nhân viên có thời điểm kết thúc trước thời điểm bắt đầu của nhân viên $i$, và số nhân viên có thời điểm bắt đầu không muộn hơn thời điểm kết thúc của nhân viên $i$. Hiệu giữa hai số đếm này là số nhân viên có khoảng thời gian giao với nhân viên $i$. Ta duyệt qua tất cả nhân viên, tính số khoảng giao nhau cho từng người và lấy giá trị lớn nhất làm đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là số nhân viên.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumTeamSize(self, startTime: list[int], endTime: list[int]) -> int:
        intervals = list(zip(startTime, endTime))
        startTime.sort()
        endTime.sort()
        ans = 0
        for l, r in intervals:
            i = bisect_right(endTime, l - 1)
            j = bisect_right(startTime, r)
            ans = max(ans, j - i)
        return ans
```

#### Java

```java
class Solution {
    public int maximumTeamSize(int[] startTime, int[] endTime) {
        int n = startTime.length;
        int[][] intervals = new int[n][2];
        for (int i = 0; i < n; i++) {
            intervals[i][0] = startTime[i];
            intervals[i][1] = endTime[i];
        }
        Arrays.sort(startTime);
        Arrays.sort(endTime);

        int ans = 0;
        for (int[] it : intervals) {
            int l = it[0], r = it[1];

            int i = search(endTime, l - 1);
            int j = search(startTime, r);

            ans = Math.max(ans, j - i);
        }

        return ans;
    }

    private int search(int[] arr, int x) {
        int l = 0, r = arr.length;
        while (l < r) {
            int mid = (l + r) >>> 1;
            if (arr[mid] > x) {
                r = mid;
            } else {
                l = mid + 1;
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
    int maximumTeamSize(vector<int>& startTime, vector<int>& endTime) {
        int n = startTime.size();
        vector<pair<int, int>> intervals(n);
        for (int i = 0; i < n; i++) {
            intervals[i] = {startTime[i], endTime[i]};
        }

        sort(startTime.begin(), startTime.end());
        sort(endTime.begin(), endTime.end());

        int ans = 0;
        for (const auto& [l, r] : intervals) {
            int i = upper_bound(endTime.begin(), endTime.end(), l - 1) - endTime.begin();
            int j = upper_bound(startTime.begin(), startTime.end(), r) - startTime.begin();
            ans = max(ans, j - i);
        }

        return ans;
    }
};

```

#### Go

```go
func maximumTeamSize(startTime []int, endTime []int) int {
	n := len(startTime)
	intervals := make([][2]int, n)
	for i := 0; i < n; i++ {
		intervals[i] = [2]int{startTime[i], endTime[i]}
	}

	sort.Ints(startTime)
	sort.Ints(endTime)

	ans := 0
	for _, it := range intervals {
		l, r := it[0], it[1]

		i := sort.Search(len(endTime), func(k int) bool { return endTime[k] > l-1 })
		j := sort.Search(len(startTime), func(k int) bool { return startTime[k] > r })

		ans = max(ans, j-i)
	}

	return ans
}
```

#### TypeScript

```ts
function maximumTeamSize(startTime: number[], endTime: number[]): number {
    const n = startTime.length;
    const intervals: [number, number][] = Array.from({ length: n }, (_, i) => [
        startTime[i],
        endTime[i],
    ]);

    startTime.sort((a, b) => a - b);
    endTime.sort((a, b) => a - b);

    let ans = 0;
    for (const [l, r] of intervals) {
        const i = search(endTime, l - 1);
        const j = search(startTime, r);

        ans = Math.max(ans, j - i);
    }

    return ans;
}

function search(arr: number[], x: number): number {
    let l = 0;
    let r = arr.length;
    while (l < r) {
        const mid = (l + r) >> 1;
        if (arr[mid] > x) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return l;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
