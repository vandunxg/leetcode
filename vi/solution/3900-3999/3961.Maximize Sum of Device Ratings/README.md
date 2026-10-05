---
comments: true
difficulty: Medium
rating: 1879
source: Weekly Contest 506 Q3
---

<!-- problem:start -->

# [3961. Maximize Sum of Device Ratings](https://leetcode.com/problems/maximize-sum-of-device-ratings)

[中文文档](/solution/3900-3999/3961.Maximize%20Sum%20of%20Device%20Ratings/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên 2D <code>units</code> có kích thước <code>m &times; n</code>, trong đó <code>units[i][j]</code> biểu thị dung lượng của unit thứ <code>j<sup>th</sup></code> trong thiết bị thứ <code>i<sup>th</sup></code>. Mỗi thiết bị chứa <strong>chính xác</strong> <code>n</code> unit.</p>

<p><strong>Rating</strong> của một thiết bị là dung lượng <strong>nhỏ nhất</strong> trong tất cả unit của thiết bị đó.</p>

<p>Bạn có thể thực hiện thao tác sau một số lần tùy ý, kể cả không lần nào:</p>

<ul>
	<li>Chọn một thiết bị <code>i</code> <strong>chưa từng</strong> được dùng làm nguồn trước đó.</li>
	<li>Xóa <strong>chính xác</strong> một unit khỏi thiết bị <code>i</code> và thêm nó vào một thiết bị <strong>khác</strong> bất kỳ.</li>
	<li>Sau đó đánh dấu thiết bị <code>i</code> đã được sử dụng, nên không thể chọn lại làm nguồn.</li>
</ul>

<p>Trả về <strong>tổng rating lớn nhất</strong> có thể đạt được của tất cả thiết bị sau một số lần thực hiện các thao tác trên.</p>

<p><strong>Lưu ý:</strong></p>

<ul>
	<li>Một thiết bị có thể nhận unit từ nhiều thiết bị, bất kể thiết bị đó đã được chọn hay chưa.</li>
	<li>Rating của một thiết bị rỗng là 0.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">units = [[1,3],[2,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn thiết bị <code>i = 0</code> và chuyển <code>units[0][0] = 1</code> sang thiết bị <code>i = 1</code>.</li>
	<li>Sau khi chuyển, các rating là:
	<ul>
		<li>Thiết bị <code>0 = [3]</code>: <code>rating[0] = 3</code></li>
		<li>Thiết bị <code>1 = [2, 2, <u>1</u>]</code>: <code>rating[1] = 1</code></li>
	</ul>
	</li>
	<li>Do đó, tổng rating là <code>3 + 1 = 4</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">units = [[1,2,3],[4,5,6]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn thiết bị <code>i = 1</code> và chuyển <code>units[1][0] = 4</code> sang thiết bị <code>i = 0</code>.</li>
	<li>Sau khi chuyển, các rating là:
	<ul>
		<li>Thiết bị <code>0 = [1, 2, 3, <u>4</u>]</code>: <code>rating[0] = 1</code></li>
		<li>Thiết bị <code>1 = [5, 6]</code>: <code>rating[1] = 5</code></li>
	</ul>
	</li>
	<li>Do đó, tổng rating là <code>1 + 5 = 6</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">units = [[5,5,5],[1,1,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Không có thao tác chuyển nào làm tăng tổng rating. Vì vậy, tổng rating là <code>5 + 1 = 6</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= m == units.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= n == units[i].length &lt;= 10<sup>5</sup></code></li>
	<li><code>m * n &lt;= 2 * 10<sup>5</sup></code></li>
	<li><code>1 &lt;= units[i][j] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Rating của một thiết bị là unit nhỏ nhất mà nó giữ lại. Chuyển giá trị nhỏ nhất của một thiết bị sang thiết bị khác chỉ có thể thay thế giá trị nhỏ thứ hai của thiết bị nhận. Khi $n=1$, không thể chuyển gì, nên đáp án là tổng các giá trị nhỏ nhất.
>
> Với $n\ge 2$, mỗi thiết bị giữ lại ít nhất hai unit: sắp xếp từng hàng và bắt đầu từ tổng các giá trị nhỏ thứ hai. Điều chỉnh hữu ích duy nhất là gộp giá trị nhỏ nhất toàn cục vào thiết bị có giá trị nhỏ thứ hai nhỏ nhất, thay thế giá trị nhỏ thứ hai đó.
>
> Công thức đóng là $\sum x[1]-(mn_2-mn)$.

<!-- thinking:end -->

Việc thêm một unit vào thiết bị chỉ có thể làm giảm hoặc giữ nguyên rating của thiết bị đó. Vì vậy, nếu $n = 1$, ta có thể trả về trực tiếp tổng rating của tất cả thiết bị.

Ngược lại, ta sắp xếp các unit của từng thiết bị theo thứ tự tăng dần, lấy unit nhỏ nhất của mỗi thiết bị và tập trung chúng vào một thiết bị có rating $\textit{mn}$. Nếu tập trung chúng vào thiết bị $i$, rating của thiết bị $i$ thay đổi từ giá trị nhỏ thứ hai $\textit{mn2}$ thành $\textit{mn}$, nên tổng rating giảm $\textit{mn2} - \textit{mn}$. Để tối đa hóa tổng rating, ta nên chọn thiết bị có mức giảm nhỏ nhất, tức là thiết bị có $\textit{mn2}$ nhỏ nhất.

Độ phức tạp thời gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số thiết bị và số unit trong mỗi thiết bị. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxRatings(self, units: List[List[int]]) -> int:
        n = len(units[0])
        if n == 1:
            return sum(x[0] for x in units)

        ans = 0
        mn = mn2 = inf
        for x in units:
            x.sort()
            ans += x[1]
            mn2 = min(mn2, x[1])
            mn = min(mn, x[0])
        ans -= mn2 - mn
        return ans
```

#### Java

```java
class Solution {
    public long maxRatings(int[][] units) {
        int n = units[0].length;
        if (n == 1) {
            long ans = 0;
            for (int[] x : units) {
                ans += x[0];
            }
            return ans;
        }

        long ans = 0;
        int mn = Integer.MAX_VALUE;
        int mn2 = Integer.MAX_VALUE;

        for (int[] x : units) {
            Arrays.sort(x);
            ans += x[1];
            mn2 = Math.min(mn2, x[1]);
            mn = Math.min(mn, x[0]);
        }

        ans -= (mn2 - mn);

        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxRatings(vector<vector<int>>& units) {
        int n = units[0].size();
        if (n == 1) {
            long long ans = 0;
            for (auto& x : units) {
                ans += x[0];
            }
            return ans;
        }

        long long ans = 0;
        int mn = INT_MAX;
        int mn2 = INT_MAX;

        for (auto& x : units) {
            sort(x.begin(), x.end());
            ans += x[1];
            mn2 = min(mn2, x[1]);
            mn = min(mn, x[0]);
        }

        return ans - (mn2 - mn);
    }
};
```

#### Go

```go
func maxRatings(units [][]int) int64 {
	n := len(units[0])
	if n == 1 {
		var ans int64
		for _, x := range units {
			ans += int64(x[0])
		}
		return ans
	}

	var ans int64
	mn, mn2 := int(^uint(0)>>1), int(^uint(0)>>1)

	for _, x := range units {
		sort.Ints(x)
		ans += int64(x[1])
		if x[1] < mn2 {
			mn2 = x[1]
		}
		if x[0] < mn {
			mn = x[0]
		}
	}

	return ans - int64(mn2-mn)
}
```

#### TypeScript

```ts
function maxRatings(units: number[][]): number {
    const n = units[0].length;

    if (n === 1) {
        let ans = 0;
        for (const x of units) {
            ans += x[0];
        }
        return ans;
    }

    let ans = 0;
    let mn = Infinity;
    let mn2 = Infinity;

    for (const x of units) {
        x.sort((a, b) => a - b);
        ans += x[1];
        mn2 = Math.min(mn2, x[1]);
        mn = Math.min(mn, x[0]);
    }

    return ans - (mn2 - mn);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
