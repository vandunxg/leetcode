---
comments: true
difficulty: Medium
rating: 1789
source: Biweekly Contest 168 Q3
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [3724. Minimum Operations to Transform Array](https://leetcode.com/problems/minimum-operations-to-transform-array)

[Tài liệu tiếng Trung](/solution/3700-3799/3724.Minimum%20Operations%20to%20Transform%20Array/README.md)

## Mô tả

<!-- description:start -->

<p data-end="180" data-start="93">Bạn được cho hai mảng số nguyên <code>nums1</code> có độ dài <code>n</code> và <code>nums2</code> có độ dài <code>n + 1</code>.</p>

<p>Bạn muốn biến đổi <code>nums1</code> thành <code>nums2</code> bằng số lượng thao tác <strong>ít nhất</strong>.</p>

<p>Bạn có thể thực hiện các thao tác sau <strong>bất kỳ</strong> số lần nào, mỗi lần chọn một chỉ số <code>i</code>:</p>

<ul>
	<li><strong>Tăng</strong> <code>nums1[i]</code> lên 1.</li>
	<li><strong>Giảm</strong> <code>nums1[i]</code> đi 1.</li>
	<li><strong>Nối thêm</strong> <code>nums1[i]</code> vào <strong>cuối</strong> mảng.</li>
</ul>

<p>Trả về số lượng thao tác <strong>ít nhất</strong> cần thiết để biến đổi <code>nums1</code> thành <code>nums2</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [2,8], nums2 = [1,7,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th align="center" style="border: 1px solid black;">Bước</th>
			<th align="center" style="border: 1px solid black;"><code>i</code></th>
			<th align="center" style="border: 1px solid black;">Thao tác</th>
			<th align="center" style="border: 1px solid black;"><code>nums1[i]</code></th>
			<th align="center" style="border: 1px solid black;"><code>nums1</code> sau khi cập nhật</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td align="center" style="border: 1px solid black;">1</td>
			<td align="center" style="border: 1px solid black;">0</td>
			<td align="center" style="border: 1px solid black;">Nối thêm</td>
			<td align="center" style="border: 1px solid black;">-</td>
			<td align="center" style="border: 1px solid black;">[2, 8, 2]</td>
		</tr>
		<tr>
			<td align="center" style="border: 1px solid black;">2</td>
			<td align="center" style="border: 1px solid black;">0</td>
			<td align="center" style="border: 1px solid black;">Giảm</td>
			<td align="center" style="border: 1px solid black;">Giảm xuống còn 1</td>
			<td align="center" style="border: 1px solid black;">[1, 8, 2]</td>
		</tr>
		<tr>
			<td align="center" style="border: 1px solid black;">3</td>
			<td align="center" style="border: 1px solid black;">1</td>
			<td align="center" style="border: 1px solid black;">Giảm</td>
			<td align="center" style="border: 1px solid black;">Giảm xuống còn 7</td>
			<td align="center" style="border: 1px solid black;">[1, 7, 2]</td>
		</tr>
		<tr>
			<td align="center" style="border: 1px solid black;">4</td>
			<td align="center" style="border: 1px solid black;">2</td>
			<td align="center" style="border: 1px solid black;">Tăng</td>
			<td align="center" style="border: 1px solid black;">Tăng lên 3</td>
			<td align="center" style="border: 1px solid black;">[1, 7, 3]</td>
		</tr>
	</tbody>
</table>

<p>Vì vậy, sau 4 thao tác, <code>nums1</code> được biến đổi thành <code>nums2</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [1,3,6], nums2 = [2,4,5,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th align="center" style="border: 1px solid black;">Bước</th>
			<th align="center" style="border: 1px solid black;"><code>i</code></th>
			<th align="center" style="border: 1px solid black;">Thao tác</th>
			<th align="center" style="border: 1px solid black;"><code>nums1[i]</code></th>
			<th align="center" style="border: 1px solid black;"><code>nums1</code> sau khi cập nhật</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td align="center" style="border: 1px solid black;">1</td>
			<td align="center" style="border: 1px solid black;">1</td>
			<td align="center" style="border: 1px solid black;">Nối thêm</td>
			<td align="center" style="border: 1px solid black;">-</td>
			<td align="center" style="border: 1px solid black;">[1, 3, 6, 3]</td>
		</tr>
		<tr>
			<td align="center" style="border: 1px solid black;">2</td>
			<td align="center" style="border: 1px solid black;">0</td>
			<td align="center" style="border: 1px solid black;">Tăng</td>
			<td align="center" style="border: 1px solid black;">Tăng lên 2</td>
			<td align="center" style="border: 1px solid black;">[2, 3, 6, 3]</td>
		</tr>
		<tr>
			<td align="center" style="border: 1px solid black;">3</td>
			<td align="center" style="border: 1px solid black;">1</td>
			<td align="center" style="border: 1px solid black;">Tăng</td>
			<td align="center" style="border: 1px solid black;">Tăng lên 4</td>
			<td align="center" style="border: 1px solid black;">[2, 4, 6, 3]</td>
		</tr>
		<tr>
			<td align="center" style="border: 1px solid black;">4</td>
			<td align="center" style="border: 1px solid black;">2</td>
			<td align="center" style="border: 1px solid black;">Giảm</td>
			<td align="center" style="border: 1px solid black;">Giảm xuống còn 5</td>
			<td align="center" style="border: 1px solid black;">[2, 4, 5, 3]</td>
		</tr>
	</tbody>
</table>

<p>Vì vậy, sau 4 thao tác, <code>nums1</code> được biến đổi thành <code>nums2</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [2], nums2 = [3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th align="center" style="border: 1px solid black;">Bước</th>
			<th align="center" style="border: 1px solid black;"><code>i</code></th>
			<th align="center" style="border: 1px solid black;">Thao tác</th>
			<th align="center" style="border: 1px solid black;"><code>nums1[i]</code></th>
			<th align="center" style="border: 1px solid black;"><code>nums1</code> sau khi cập nhật</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td align="center" style="border: 1px solid black;">1</td>
			<td align="center" style="border: 1px solid black;">0</td>
			<td align="center" style="border: 1px solid black;">Tăng</td>
			<td align="center" style="border: 1px solid black;">Tăng lên 3</td>
			<td align="center" style="border: 1px solid black;">[3]</td>
		</tr>
		<tr>
			<td align="center" style="border: 1px solid black;">2</td>
			<td align="center" style="border: 1px solid black;">0</td>
			<td align="center" style="border: 1px solid black;">Nối thêm</td>
			<td align="center" style="border: 1px solid black;">-</td>
			<td align="center" style="border: 1px solid black;">[3, 3]</td>
		</tr>
		<tr>
			<td align="center" style="border: 1px solid black;">3</td>
			<td align="center" style="border: 1px solid black;">1</td>
			<td align="center" style="border: 1px solid black;">Tăng</td>
			<td align="center" style="border: 1px solid black;">Tăng lên 4</td>
			<td align="center" style="border: 1px solid black;">[3, 4]</td>
		</tr>
	</tbody>
</table>

<p>Vì vậy, sau 3 thao tác, <code>nums1</code> được biến đổi thành <code>nums2</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums1.length &lt;= 10<sup>5</sup></code></li>
	<li><code>nums2.length == n + 1</code></li>
	<li><code>1 &lt;= nums1[i], nums2[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> $n$ vị trí đầu tiên phải trở thành các giá trị tương ứng trong $\textit{nums2}$ với chi phí bằng chênh lệch tuyệt đối; giá trị cuối cùng chỉ có thể được tạo bằng thao tác nối thêm, có chi phí ít nhất là $1$. Nếu một cặp $(\textit{nums1}[i],\textit{nums2}[i])$ nào đó đã bao phủ $\textit{nums2}[n]$, thao tác nối thêm không cần thay đổi bổ sung; nếu không, ta cũng đưa đầu mút gần hơn về giá trị cuối cùng đó.

<!-- thinking:end -->

Ta định nghĩa một biến đáp án $\text{ans}$ để ghi lại số lượng thao tác ít nhất, với giá trị ban đầu là $1$, biểu thị thao tác cần thiết để nối phần tử cuối vào cuối mảng.

Sau đó, ta duyệt qua $n$ phần tử đầu tiên của mảng. Với mỗi cặp phần tử tương ứng $(\text{nums1}[i], \text{nums2}[i])$, ta tính độ chênh lệch của chúng và cộng vào $\text{ans}$.

Trong quá trình duyệt, ta cũng cần kiểm tra điều kiện $\min(\text{nums1}[i], \text{nums2}[i]) \leq \text{nums2}[n] \leq \max(\text{nums1}[i], \text{nums2}[i])$ có đúng hay không. Nếu đúng, điều đó có nghĩa là ta có thể trực tiếp điều chỉnh $\text{nums1}[i]$ để đạt đến $\text{nums2}[n]$. Nếu không, ta cần ghi nhận một độ chênh lệch nhỏ nhất $d$, biểu thị số lượng thao tác ít nhất cần thiết để điều chỉnh một phần tử nào đó thành $\text{nums2}[n]$.

Cuối cùng, nếu sau khi duyệt không tìm thấy phần tử nào thỏa mãn điều kiện, ta cần cộng $d$ vào $\text{ans}$, cho biết ta cần thêm thao tác để điều chỉnh một phần tử thành $\text{nums2}[n]$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums1: List[int], nums2: List[int]) -> int:
        ans = 1
        ok = False
        d = inf
        for x, y in zip(nums1, nums2):
            if x < y:
                x, y = y, x
            ans += x - y
            d = min(d, abs(x - nums2[-1]), abs(y - nums2[-1]))
            ok = ok or y <= nums2[-1] <= x
        if not ok:
            ans += d
        return ans
```

#### Java

```java
class Solution {
    public long minOperations(int[] nums1, int[] nums2) {
        long ans = 1;
        int n = nums1.length;
        boolean ok = false;
        int d = 1 << 30;
        for (int i = 0; i < n; ++i) {
            int x = Math.max(nums1[i], nums2[i]);
            int y = Math.min(nums1[i], nums2[i]);
            ans += x - y;
            d = Math.min(d, Math.min(Math.abs(x - nums2[n]), Math.abs(y - nums2[n])));
            ok = ok || (nums2[n] >= y && nums2[n] <= x);
        }
        if (!ok) {
            ans += d;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minOperations(vector<int>& nums1, vector<int>& nums2) {
        long long ans = 1;
        int n = nums1.size();
        bool ok = false;
        int d = 1 << 30;
        for (int i = 0; i < n; ++i) {
            int x = max(nums1[i], nums2[i]);
            int y = min(nums1[i], nums2[i]);
            ans += x - y;
            d = min(d, min(abs(x - nums2[n]), abs(y - nums2[n])));
            ok = ok || (nums2[n] >= y && nums2[n] <= x);
        }
        if (!ok) {
            ans += d;
        }
        return ans;
    }
};
```

#### Go

```go
func minOperations(nums1 []int, nums2 []int) int64 {
	var ans int64 = 1
	n := len(nums1)
	ok := false
	d := 1 << 30
	for i := 0; i < n; i++ {
		x := max(nums1[i], nums2[i])
		y := min(nums1[i], nums2[i])
		ans += int64(x - y)
		d = min(d, min(abs(x-nums2[n]), abs(y-nums2[n])))
		if nums2[n] >= y && nums2[n] <= x {
			ok = true
		}
	}
	if !ok {
		ans += int64(d)
	}
	return ans
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function minOperations(nums1: number[], nums2: number[]): number {
    let ans = 1;
    const n = nums1.length;
    let ok = false;
    let d = 1 << 30;
    for (let i = 0; i < n; ++i) {
        const x = Math.max(nums1[i], nums2[i]);
        const y = Math.min(nums1[i], nums2[i]);
        ans += x - y;
        d = Math.min(d, Math.abs(x - nums2[n]), Math.abs(y - nums2[n]));
        if (nums2[n] >= y && nums2[n] <= x) {
            ok = true;
        }
    }
    if (!ok) {
        ans += d;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
